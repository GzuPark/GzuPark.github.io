---
layout: post
title: Comfortable R package loading
excerpt: "R package의 install, update 그리고 load를 한번에!"
categories: blog
tags: [R, package]
image:
  feature: bg-yongin.jpg
  credit: Yong-in, Korea
comments: true
share: true
modified: 2017-03-22
---


### 1. 서론

이 글을 읽는 사람들은 R의 package를 사용할때 생기는 사소한 불편함에 공감을 가질 것으로 생각된다.

* install 할 package개수가 많은데 설치여부가 확실하지 않아 시간을 낭비할 때
* 현재 불러온 package를 중복으로 로드하는 것을 피하려고 할때
* 오랜만에 사용하는 package의 새로운 version이 나왔는데 모르고 지나칠때

이런 상황들 이외에도 다양한 상황을 한번쯤은 느껴봤을 것이라고 생각한다.
심지어 R, RStudio의 update를 매일 확인하지 않는다면 새로운 기능을 모르고 지나치거나 bug fix가 되지 않은 상태로 작업을 하는 것은 아쉽지 않을까?
매일 아니 매번 이런 환경을 확인하는 분들이라면 모르겠지만 필자처럼 깜박하는 사람들을 위해 **[<font color="red">code</font>](https://gzupark.github.io/usefulcode/autoload.R)** 를 작성해보았다.


### 2. 흐름도

<center><figure><img src="{{ site.url }}/images/blog-2017-02-20/flowchart.jpg"><figcaption><b>그림 1</b></figcaption></figure></center>

흐름도의 내용을 목록화 한다면, <br>
1. 사용할 package를 입력 <br>
2. install 되어 있는가 확인 <br>
3. latest version 을 확인 <br>
4. 필요한 package를 install <br>
5. 현재 load 되어 있는가 확인 <br>
6. 필요한 package load <br>


이렇게 대략 6단계로 구성되어 있다는 것을 알 수 있을 것이다. 이렇게 흐름도를 그린다던가 아래와 같이 구조를 알 수 있는 간략한 명령어로된 식을 구성해 보는 것이 좋다.

<center><figure><img src="{{ site.url }}/images/blog-2017-02-20/codebyhand.jpg"><figcaption><b>그림 2</b> </figcaption></figure></center>

### 3. 본론

#### 설치 안된 package 목록
사용할 package 목록 중에 설치가 안된 package의 목록을 만든다.
```R
new.pkg <- pkg[!(pkg %in% installed.packages()[,"Package"])]
```

##### 3.1. 업데이트가 필요한 package 목록
최신 버전이 아닌 업데이트가 필요한 package의 목록을 `new.pkg`에 추가한다.
```R
old.pkg <- pkg[pkg %in% old.packages()]
if(length(old.pkg)) {
    new.pkg <- c(new.pkg, old.pkg)
}
```

##### 3.2. packge 설치
`new.pkg` 목록에 1개 이상의 package가 있다면 설치를 진행한다.
```R
if(length(new.pkg)) install.packages(new.pkg)
```

##### 3.3. packge 불러오기
현재 session에서 이미 불러온 package를 제외하고 아직 불러오지 않은 package를 차례로 불러온다.
```R
loaded.pkg <- search()[search() %in% paste0("package:", pkg)]
    loaded.pkg <- substring(loaded.pkg, 9)
    load.pkg <- pkg[!(pkg %in% loaded.pkg)]
    if(length(load.pkg)) {
        for(i in 1:length(load.pkg)) {
            library(load.pkg[i], character.only=TRUE)
        }
    }
```

### 4. 결론
`autoload()` 라는 함수 하나로 package의 install, update 확인, load까지 해결하는 편리한 기능이라고 생각한다. 그렇지만 `update.packages()` 함수를 활용한다면 `autoload()` 안의 update list 부분은 생략해도 될 것이다. 매번 세션을 시작하거나 파일을 새로 만들 때 필자의 github에 올려놓은 [`autoload.R`](https://raw.githubusercontent.com/GzuPark/usefulcode/master/autoload.R) 파일을 불러와서 활용할 수도 있고, 또는 기본 환경에서 실행되도록 설정해놓는 것도 방법일 것이다.

### Reference
* [R package](https://cran.r-project.org/web/packages/available_packages_by_name.html)의 종류는 매우 다양하며 현재 **10105** 개가 있다. (2017-02-20, http://cran.nexr.com/)
* [available.packages()](https://stat.ethz.ch/R-manual/R-devel/library/utils/html/available.packages.html)
* [library()](https://stat.ethz.ch/R-manual/R-devel/library/base/html/library.html)
* [old.packages()](https://stat.ethz.ch/R-manual/R-devel/library/utils/html/update.packages.html)
* [search()](https://stat.ethz.ch/R-manual/R-devel/library/base/html/search.html)
