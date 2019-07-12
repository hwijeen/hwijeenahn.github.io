---
layout: post
title: "Dataloading with torchtext"
description: "Torchtext 사용하기 - template과 설명"
comments: true
categories: [개발]
tags:
- torchtext
---



Torchtext는 '파일 읽어오기 - tokenization - dataset split - build vocabulary -  numericalization - pretrained word embedding - batch iterator' 일련의 과정을 편리하게 할 수 있도록 도와준다. Supervised learning할 때 torchtext를 사용하는 방법과 그 설명을 정리했다 [외국 블로그](http://anie.me/On-Torchtext/)글, torchtext 소스코드를 참고했다.



## torchtext.data.Field

`Field`는 data의 한 영역을 represent하는 데 쓰인다. Supervised learning을 위한 paired data의 경우 src문장과 trg문장이 하나의 파일에 저장되어 있는 경우가 많다. 이때,  tab등의 구분자로 나뉘는 각 열에 해당하는 게 `Field`이다. `Field`를 만들 때 인자로 넘기는 것들은 그 필드에 적용할 전/후처리 등에 대한 정보이다. 

```python
MAXLEN = 15
SRC = ReversibleField(tokenize='spacy', batch_first=True, include_lengths=True,
                          preprocessing=lambda x: x[:MAXLEN+1])
TGT = ReversibleField(tokenize='spacy', batch_first=True, include_lengths=True,
                          preprocessing=lambda x: x[:MAXLEN+1])
```

`Include_lengths`와 `batch_first`는 `Iterator`가 batch tensor을 내뱉을 때 영향을 미치지만, 여기서부터 설정해줘야한다. Pytorch의 `pack_padded_sequence`는 인자로 paddding된 batch와 batch내 example들의 길이 정보를 받기 때문에 `include_lengths=True`가 필요하다. `batch_first`는 선택사항이다. 텍스트 데이터를 다룰 때 최대 길이 설정도 사실상 필수인데, 여기서 lambda function을 통해 최대 길이를 넘는 부분은 잘라버린다. 이후 lambda function의 input으로 들어가는 x는 `Example`(=list of strings)이다.

- Spacy tokenizer 사용시 다른 기능은 load하지 않는 방법은 [여기](https://spacy.io/usage/processing-pipelines#disabling)



## torchtext.data.Dataset

`Dataset`는 `Example` 단위로 정리된 데이터들을 갖는 클래스이다. 많은 경우에 로컬에 특정 형태로 저장된 데이터를 사용할텐데, 이 경우 사용해야하는 class는 `TabularDataset`이다. Instance를 만든 뒤엔 인덱싱을 통한 `Example` 접근(\_\_getitem\_\_), `len` 메서드 사용(\_\_len\_\_), `Example`에 대한 iteration(\_\_iter\_\_), `Example`들의 특정 Field에 대한 iteration(\_\_getattr\_\_)을 지원한다.

```python
corpus = TabularDataset(path='./data/en-fr.toy.txt', format='tsv',
                            fields=[('src', SRC), ('tgt', TGT)])
train, val, test = corpus.split(split_ratio=[0.8, 0.1, 0.1]) 
```

`TabularDataset`이 `Example`을 만들려면 데이터가 어떻게 생겼는지에 대한 정보, 즉 `Field` 정보가 필요하기에 instance를 만들 때 `Field`를 dictionary혹은 list of tuples로 넘겨줘야한다. `TabularDataset` instance를 train, valid, test로 나누는 건 `.split()`라는 instance method를 이용한다. 만약 로컬에 있는 파일이 이미 split된 상태라면, `.splits()`라는 class method를 통해 각각 읽어올 수 있다.

- [두 Dataset을 병합하는 방법](https://github.com/pytorch/text/issues/375). 만약 데이터가 label 별로 다른 파일에 저장되어 있을 때 유용하다.



## torchtext.data.Field.build_vocab()

Torchtext에서 `Vocab`은 `Field`의 attribute이다. Torchtext는 하나의 `Field`가 하나의`Vocab`을 갖도록 만들어졌기 때문에(data가 갖는 게 아님!), 두 `Field`가 `Vocab`을 공유해야하는 경우엔 약간의 hack이 필요하다. 기계 번역의 경우엔 일반적으로 `Field`마다 `Vocab`이 필요하겠지만, src와 trg의 언어가 같은 대화 데이터 같은 경우엔 `Vocab`을 공유할 필요가 있다. 그럴 경우, [임의의 `Field`가 `Vocab`을 갖도록 하고,  만들 때 src와 trg의 단어를 모두 사용하도록 한다.](https://github.com/pytorch/text/issues/232)

```python
SRC.build_vocab(train, val) # Dataset
TGT.build_vocab(train.tgt, val.tgt) # 'Example의 특정 Field 내용을 반환하는 iterator'
# SRC.build_vocab(train.src, train.tgt, val.src, val.tgt) # possible
```

무슨 데이터를 사용해서 `Vocab`을 만들지를 알려주기 위해 `.build_vocab()`의 인자로 `Dataset` 혹은 '`Example`의 특정 `Field`에 대한 iterator'를 넣어준다. 만약 pretrained vector를 다운로드 해서 쓸 거라면 여기서 vectors 인자를 넘겨야한다. 안 쓰는 경우엔 train, valid 데이터만 넣어줘서 test data 대한 정보 없이 vocab을 구성하는 것이 맞다. 하지만 pretrained vector을 쓸 거라면 train, valid, test 데이터 모두 넣어줘야한다('vocabulary expansion'). Train, valid에 등장하지 않았더라도 pretrained vector에 있는 단어라면 vocab에 포함시켜야한다는 것이다. 그러한 단어들은 어차피 train 때 추가적으로 학습되지 않고 기존 pretrained 값을 유지한다.



## torchtext.data.Iterator

`Iterator`는 `Example`을 모아 만든 `Batch`를 반복적으로 내뱉는 역할을 담당한다.  텍스트 데이터를 다룰 때 padding을 최소화하기 위해서 bucketing을 하는데, `BucketIterator`가 이를 담당한다. `Dataset`에 들어있는 `Example`은 list of string이다. `Iterator`는 string를 index로 바꿔서(numericalize), `torch.LongTensor`로 내뱉는다. 

````python
train_iter, valid_iter, test_iter = BucketIterator.splits((train,val,test),
                                                     sort_key=lambda x:len(x.src),
                                                     sort_within_batch=True,
                                                     batch_size=32, 
                                                     device=torch.device('cuda'),
                                                     repeat=False)
# sort_key는 Dataset안의 Example에 대해서
# repeat=False 시 1 epoch 돌고 iter 끝남
# sort_within_batch=True for pack_padded_sequence
````

`.splits()`의 인자로 shuffle 값을 주지 않으면 train iterator는 `shuffle=True`가 된다. 이는 train할 때 batch들이 random하게 뽑히는 걸 보장한다. `batch_size_fn`인자를 주면 `Batch`구성 기준을 `Example`개수에서 다른 것(e.g. token 개수)으로 바꿀 수 있다. 만약 GPU가 한 번에 몇 token까지 처리할 수 있는지를 알고있다면 cumstom `batch_size_fn`을 사용하는 게 GPU를 최대한으로 이용할 수 있는 방법이다.



모델 훈련 부분의 코드는 이렇게 구성할 수 있다. Iterator에서 repeat=False를 해줬기에 다음과 같이 epoch에 대한 for문을 사용할 수 있다.

```python
EPOCH = 100
for epoch in range(EPOCH): # repeat=False 필요
    for train_batch in train_iter:
        src_batch, tgt_batch = train_batch.src, train_batch.tgt
        pass # train for one epoch
    for valid_batch in valid_iter:
        pass # record valid performance
```



## Template





## Questions

- is_target: Batch내에서 iterate할 때 target field에 해당하는 값이 나중에 나오도록
- ReversibleField 사용법?
- 이미 있는 pretrained vector파일 불러와서 적용하는 방법은?
- vocab 저장