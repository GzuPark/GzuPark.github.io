---
layout: post
title: Why Anaconda? How to control Anaconda?
excerpt: "왜 Anaconda를 쓸까? 어떻게 사용해야 될까?"
categories: blog
tags: [Anaconda, Python, TensorFlow, Data Science]
image:
  feature: bg-cloud-wave.jpg
  credit: Ritz Carlton, Half Moon Bay, CA
comments: true
share: true
---

#### 목차
  * [1. 왜 Anaconda를 사용할까?](#1-0)
  * [2. Anaconda란 무엇일까?](#2-0)
      * [2.1. 패키지 관리](#2-1)
      * [2.2. 가상환경](#2-2)
  * [3. Anaconda 설치](#3)
  * [4. 패키지 관리](#4-0)
  * [5. 가상환경](#5-0)
      * [5.1. 가상환경 만들기](#5-1)
      * [5.2. 가상환경 실행](#5-2)
      * [5.3. 가상환경 저장](#5-3)
      * [5.4. 가상환경 불러오기](#5-4)
      * [5.5. 가상환경 삭제](#5-5)
      * [5.6. 가상환경 공유](#5-6)
  * [6. 더 알고 싶다면](#6-0)
  * [Reference](#ref)

왜 [Anaconda](https://www.continuum.io/why-anaconda) 라는 도구를 쓰는 것일까? Python이나 R의 사용방법을 익히면서 이유없이 설치했거나 도대체 이것이 무엇인지 잘 모른다면 이 글을 읽고 도움이 되길 바란다.

<a name='1-0'></a>

### 1. 왜 Anaconda를 사용할까?
[Anaconda](https://anaconda.org/)는 데이터사이언스를 유연하게 하기 위한 도구이며, conda 라는 가상환경 관리도구를 사용한다. Anaconda는 설치하기 쉬운 프로그램이며 앞으로 사용하게 될 다양한 개발 환경에 유연하게 대처할 수 있다. 예컨데 구현해보고자 하는 Python 버전과 사용하는 패키지 버전이 내 환경과 다르다면 구현하고자 하는 환경과 동일하게 만들어주기 위해 사용하게 된다.

<a name='2-0'></a>

### 2. Anaconda란 무엇일까?
Anacona는 꽤 큰 용량을 (~500MB) 다운로드 해야된다. 왜냐하면 Python 내에서 150개가 넘는 대중적인 데이터 사이언스 패키지들이 같이 설치되기 때문이다. 만약 모든 패키지가 필요없거나 대역폭이나 저장공간을 절약할 필요가 있다면 오직 conda와 Python만 포함되는 Minicona라는 더 작은 배포판이 있다.

콘다는 대체로 커맨드라인을 통해 사용하게 될 것인데 만약 익숙하지 않다면 Windows의 경우 [command prompt tutorial for Windows](https://www.lynda.com/-tutorials/Windows-command-line-basics/497312/513424-4.html), OSX/Linux의 경우 Udacity의 코스인 [Linux Command Line Basics](https://www.udacity.com/course/linux-command-line-basics--ud595)을 보고 사용방법을 익히길 권한다.

아마 이미 설치하고 사용하는 Python이 있을텐데 왜 이 모든 것들이 필요할까 의문이 들 것이다. 첫번째로, Anaconda는 많은 데이터 사이언스 패키지들을 같이 설치해주기 때문에 필요한 거의 모든 것을 준비해줄 수 있다. 두번째로, 당신의 패키지와 가상환경을 관리하는 conda를 사용함으로써 당신이 사용할 다양한 라이브러리에서 발생하게 될 이슈들을 줄일 수 있을 것이다. 예컨데, Python 2 와 Python 3 사이에서 오는 `print` 명령어 [차이](https://docs.python.org/3.0/whatsnew/3.0.html)가 유명하다.

<a name='2-1'></a>

##### 2.1. 패키지 관리
Python을 다뤄봤다면 이미 [pip](https://pypi.python.org/pypi/pip)(Python 라이브러리 관리를 위한 기본 패키지)와 친숙할 것이다. Conda는 pip와 유사하게 사용가능하지만, conda는 pip처럼 Python에 특화되어있는 것이 아니라서 non-Python 패키지도 설치할 수도 있다. 재밌게도 모든 Python 라이브러리가 Anaconda와 conda에서 사용할 수 없다. 대신 pip를 통해 conda 환경에 Python 패키지를 설치할 수 있다. [예: TensorFlow](#TensorFlow-install)

Conda는 미리 컴파일된 패키지를 설치하게 된다. 예를 들어, 아나콘다 배포판은 [MKL library](https://docs.continuum.io/mkl-optimizations/)으로부터 컴파일된 Numpy, Scipy, Scikit-learn 같은 다양한 수학연산을 도와주는 패키지를 포함한다. 컴파일된 패키지들은 Anaconda 배포판의 제작자로부터 유지보수된되는데, 즉 그 이야기는 제작자들이 보통 새로운 패키지 버전보다 천천히 업데이트 한다는 것을 의미한다. 하지만 패키지가 안정화된 후에 포함하는 경향이 있기 때문에 사용자의 입장에서는 편리하다.

<a name='2-2'></a>

##### 2.2. 가상환경
Conda는 가상환경 관리도구이다. [virtulaenv](https://virtualenv.pypa.io/en/stable/)나 [pyenv](https://github.com/yyuu/pyenv)와 같이 다른 가상환경 도구들과 비슷하다.

가상환경은 당신이 다른 프로젝트를 위해 사용하는 패키지들로부터 분리 및 독립 시켜줄 수 있다. 아마도 당신은 어떤 라이브러리의 특정 버전에서 작동하는 코드를 다루게 될 것이다. 예를들어 당신 Numpy의 새로운 특징을 사용하는 코드를 작성할 수도 있고, 또는 이미 제거된 오래된 특징을 사용하는 코드를 작성할 수도 있다. 일반적으로는 동시에 두가지 버전의 Numpy를 사용하는 것은 불가능하다. 하지만 conda를 사용한다면 특정 버전의 Numpy를 설치 해놓은 가상환경을 사용할 수 있기 때문에 조건에 딱 맞는 프로젝트 환경을 만들 수 있다.

대표적인 이슈는 위에서 언급한 바와 같이 Python 2 와 Python 3 에서 많이 발생한다. 오래된 코드는 Python 3 에서 작동하지 않을 것이고, 새로운 코드는 Python 2 에서 작동하지 않을 것이다. 만약 두가지 버전을 같은 환경에 모두 설치했다면 많은 혼란과 버그를 경험하고 구글링을 하고 있는 자신을 발견하게 될 것이다. 따라서 무엇보다 분리된 환경을 구축하는 것이 훨씬 좋다.

Conda 에서는 필요에 따라 사용하는 패키지 버전을 포함한 가상환경을 파일에 저장할 수 있고 이 파일을 코드와 같이 포함하는 것이 일반적이다. 왜냐하면 다른 사람들이 코드에서 사용되는 모든 환경을 그대로 불러오기 쉽게 해주기 때문이다. 물론, pip 또한 비슷한 것이 있다 `pip freeze > requirements.txt`.

<a name='3-0'></a>

### 3. Anaconda 설치
Anaconda는 [Windows](https://www.continuum.io/downloads#windows), [Mac OS X](https://www.continuum.io/downloads#osx), [Linux](https://www.continuum.io/downloads#linux) 에서 가능하다. 만약 [Miniconda](https://conda.io/miniconda.html)를 사용하고 싶다면 [설치방법](https://conda.io/docs/install/quick.html)을 참고하길 바란다.

만약 Anaconda를 설치해도 이미 설치된 Python에는 아무런 영향도 없테을니 안심해도 된다. 대신 스크립트와 코드가 사용되는 default Python이 기존의 Python이 아니라 Anaconda의 Python에서 실행될테니 이게 싫다면 `PATH` 설정을 수정하면 된다.

다운로드 항목을 보면 크게 Python 3.6 버전과 Python 2 버전이 있다(*2017-3-30 기준*). 아무거나 설치해도 되지만 Python 3 을 추천한다. 왜냐하면 새로 쓰여진 코드들은 Python 3 버전에 맞춰 쓰여지고 있고, Python 2 의 환경을 구축하려면 Conda 환경을 만들면서 가능하기 때문이다. 무엇보다 [Python 2 는 은퇴할 예정](https://pythonclock.org/)이다. 그리고 현재 운영체제가 64-bit, 32-bit를 확인하고 선택하면 된다.

설치를 마친 후(*만약 터미널을 사용하여 설치하였다면 터미널을 종료하고 다시 실행*) `conda list` 명령어를 통해 default conda 환경에 설치되어 있는 모든 패키지들을 확인할 수 있다.

<center><figure><img src="{{ site.url }}/images/blog-2017-03-30/001-conda-list.png">
<figcaption><b>그림 1</b> Miniconda의 <code>conda list</code></figcaption></figure></center>

글쓴이는 OSX 환경을 사용하고 Miniconda를 설치하여 보여주기 때문에 만약 Windows 환경과 다른 점이 있다면 이야기 해주도록 하겠다.

> **잠깐!** Windows 에서 Anaconda를 설치하면 몇 가지 프로그램들이 같이 설치될 것이다.  
&emsp; Anaconda Navigator: 가상환경과 패키지를 관리하는 GUI  
&emsp; Anaconda Prompt: 커맨드 명령어를 통해 가상환경과 패키지를 관리하는 터미널  
&emsp; Spyder: 개발을 위한 IDE  

그리고 혹시 발생할지 모르는 에러를 방지하기 위해 기본 환경의 패키지들을 업데이트 해주는 것이 좋다. **Anaconda Prompt** 를 열고 아래의 두 명령어를 실행시킨다.

```
$ conda upgrade conda
$ conda upgrade --all
```

<center><figure><img src="{{ site.url }}/images/blog-2017-03-30/002-conda-upgrade-conda.png">
<figcaption><b>그림 2</b> <code>conda upgrade conda</code></figcaption></figure></center>

<center><figure><img src="{{ site.url }}/images/blog-2017-03-30/003-conda-upgrade-all.png">
<figcaption><b>그림 3</b> <code>conda upgrade --all</code></figcaption></figure></center>

패키지를 설치하고 싶다면 **그림 2** 와 **그림 3** 처럼 `yes` 를 입력하면 된다. 패키지의 초기 설치는 대체로 마이너 업데이트를 하지 않은 경우가 대부분이라 혹시 모를 실행 중 발생하게 될 에러를 방지하기 위해 업데이트를 해줘야된다.

> **잠깐!** `conda upgrade conda` 의 경우 `--all` 에 포함되기 때문에 따로 실행할 필요가 없다. 하지만 가끔 에러가 발생하는 경우에는 따로 실행해주는게 좋다.

<a name='4-0'></a>

### 4. 패키지 관리
Anacona를 설치하였다면 패키지 관리는 꽤나 직관적이다. 패키지를 설치하기 위해서는 터미널창에 `conda install package_name`을 입력하면 된다. 예를 들어 `Numpy`패키지를 설치한다면, **그림 4** 처럼 `conda install numpy`를 입력하면 된다.

<center><figure><img src="{{ site.url }}/images/blog-2017-03-30/004-conda-install-numpy.png">
<figcaption><b>그림 4</b> <code>conda install numpy</code></figcaption></figure></center>

만약 여러개의 패키지를 동시에 설치하려면 `conda install numpy scipy pandas`를 사용하면 된다. 그리고 특정 버전의 패키지를 설치하는 것도 가능하다. 가령, 데이터 구조를 다루기 위한 패키지인 `pandas`(R 의 `data.table`과 유사)의 `0.19.1` 버전(*최신 버전은* `0.19.2`, *2017-3-30 기준*)을 설치하고 싶다면, 아래 **그림 5** 처럼 `conda install pandas=0.19.1`을 입력하면 된다.

<center><figure><img src="{{ site.url }}/images/blog-2017-03-30/005-conda-install-pandas-0-19-1.png">
<figcaption><b>그림 5</b> <code>conda install pandas=0.19.1</code></figcaption></figure></center>

`pandas`의 경우 `numpy`, `scipy`와 같은 자동으로 설치되는 패키지가 있다. 왜냐하면 `pandas` 패키지는 `numpy`와 `scipy` 패키지를 사용하기 때문이다. 만약 필수 패키지가 이미 설치되어 있지 않다면 Conda는 자동적으로 필수 패키지도 설치할 것이다.

만약 패키지를 지우고 싶다면, `conda remove package_name`을 입력하면 되고, 업데이트를 하고 싶다면 `conda update package_name`을 입력하면 된다. 앞에서도 이야기한 바와 같이 모든 패키지를 업데이트하고 싶다면 `conda update --all`을 하면 된다.

그리고 패키지 이름이 정확하게 기억나지 않다면 `conda search search_term`을 입력해보자. 예를 들면, 크롤링할때 유용하게 쓰이는 [Beatiful Soup](https://www.crummy.com/software/BeautifulSoup/)의 스펠링도 잘 모르겠고, 정확한 이름도 기억나지 않는다면, **그림 6** 과 같이 `conda search beaut` 라고 입력해보자.

<center><figure><img src="{{ site.url }}/images/blog-2017-03-30/006-conda-search-beaut.png">
<figcaption><b>그림 6</b> <code>conda search beaut</code></figcaption></figure></center>

`beautiful-soup`, `beautifulsoup4` 의 패키지 버전과 설치 가능한 Python 버전도 같이 나온다.

> **잠깐!** 물론, 인터넷이 연결된 상태여야되며 패키지 버전을 보면 `beautiful-soup` 이라는 이름은 이후 버전부터 `beautifulsoup4` 로만 쓰인다.

<a name='TensorFlow-install'></a>

[TensorFlow](https://www.tensorflow.org) 라는 기계학습에 최적화된 오픈소스 패키지가 있다. TensorFlow 는 Anaconda 의 가상환경에서 사용이 가능하지만, `conda` 명령어를 통해 설치하는 것보다 `pip install tensorflow`를 사용하여 설치하기를 권장하고 있다. [링크](https://www.tensorflow.org/install/install_mac)

> **잠깐!** TensorFlow에 대해서 알고 싶다면 홍콩과학기술대학교 [김성훈 교수님](https://github.com/hunkim)의 자료를 참고하길 바란다. ([GitHub](https://github.com/hunkim/DeepLearningZeroToAll), [YouTube](https://www.youtube.com/watch?v=BS6O0zOGX4E&list=PLlMkM4tgfjnLSOjrEJN31gZATbcj_MpUm))

<a name='5-0'></a>

### 5. 가상환경

<a name='5-1'></a>

##### 5.1. 가상환경 만들기
Conda는 프로젝트마다 독립된 환경을 구축할 수 있다. 따라서 아래의 **그림 7** 과 같이 `conda create -n env_name list_of_packages` 을 입력하면 `env_name` 이라는 가상환경이 `list_of_packages` 에 명시한 패키지와 함께 만들어지게 된다.

<center><figure><img src="{{ site.url }}/images/blog-2017-03-30/007-conda-create-env.png">
<figcaption><b>그림 7</b> <code>conda create -n my_env numpy</code></figcaption></figure></center>

Default Python 버전이 3.6.0 이라서 가상환경이 Python 3.6.0 으로 만들어졌지만 필요에 따라 버전을 다르게 할 수도 있다. `conda create -n python35 python=3.5` 이렇게 Python 3 특정 버전의 환경을 만들 수도 있고, `conda create -n python2 python=2` 와 같이 Python 2 버전 환경도 만들 수 있다. (*새로운 글을 통해 예를 소개할 예정*)

<a name='5-2'></a>

##### 5.2. 가상환경 실행
원하는 가상환경을 만들었다면, Widows 의 경우 `activate my_env`, OSX 또는 Linux 의 경우 `source activate my_env` 을 입력한다.

<center><figure><img src="{{ site.url }}/images/blog-2017-03-30/008-source-activate-env.png">
<figcaption><b>그림 8</b> <code></code>source activate my_env</figcaption></figure></center>

**그림 8** 과 같이, 터미널 프롬프트 앞 부분에 `(my_env) ~ $` 이 보인다면 `my_env` 라는 가상환경이 실행 중인 것이다. 이 환경에서는 필수적인 패키지만 설치되어 있기 때문에 `conda list`로 확인하고 필요한 패키지들을 설치해주면 된다. 그리고 환경을 종료하려면 Widows 의 경우 `deactivate`, OSX 또는 Linux 의 경우 **그림 9** 처럼 `source deactivate` 을 입력하면 된다.

<center><figure><img src="{{ site.url }}/images/blog-2017-03-30/009-source-deactivate.png">
<figcaption><b>그림 9</b> <code>source deactivate</code></figcaption></figure></center>

<a name='5-3'></a>

##### 5.3. 가상환경 저장
내가 작성한 코드를 다른 사람들이 실행하려면 내가 개발한 환경을 다른 사람들과 공유할 수 있어야 된다. 이 부분이 Anaconda를 쓰는 또 다른 중요한 이유 중 하나다. [YAML](http://www.yaml.org/) 파일을 통해 Python 과 패키지의 버전을 저장할 수 있다. `conda env export`를 입력하면 현재 실행하고 있는 가상환경을 환경을 볼 수 있고, `conda env export > environment.yaml`을 입력하면 `environment.yaml` 이라는 파일 이름으로 저장할 수 있다.

<center><figure><img src="{{ site.url }}/images/blog-2017-03-30/010-conda-env-export.png">
<figcaption><b>그림 10</b> <code>conda env export</code></figcaption></figure></center>

> **잠깐!** **그림 10** 에서 보이는 [`DEPRECATION:`](https://github.com/conda/conda/issues/4596) 출력은 conda 때문이 아니라 [`pip freeze`](https://pip.pypa.io/en/stable/reference/pip_freeze/) 에서 발생하는 일종의 주의사항이라고 보면 된다.

<a name='5-4'></a>

##### 5.4. 가상환경 불러오기
`environment.yaml` 파일을 통해 가상환경을 만들 수 있는데 `conda env create -f environment.yaml` 이라고 입력하면 된다. 또한, **그림 11** 의 `conda env list` 결과에서 보이는 `root`는 default conda 가상환경이다.

<center><figure><img src="{{ site.url }}/images/blog-2017-03-30/011-conda-env-list.png">
<figcaption><b>그림 11</b> <code>conda env list</code></figcaption></figure></center>

> **잠깐!** `prefix` 부분은 신경쓰지 않아도 된다. 그리고 나의 디렉토리 구조가 알려지는 것이 꺼려진다면 prefix 줄 전체를 지워도 상관없다.

<a name='5-5'></a>

##### 5.5. 가상환경 삭제
만약 더 이상 특정 가상환경을 사용하지 않을 것이라면, `conda env remove -n env_name` 을 통해 가상환경을 삭제할 수 있다.

<a name='5-6'></a>

##### 5.6. 가상환경 공유
Conda를 사용하지 않는 사람들과 나의 개발환경을 공유하려면 위에서 언급한 `pip freeze`([링크](https://pip.pypa.io/en/stable/reference/pip_freeze/))를 통해 `requirements.txt` 패키지 버전을 저장하고 공유하면 된다.

<a name='6-0'></a>

### 6. 더 알고 싶다면
Conda에 대해 더 알고 싶거나 Python 생태계와의 관계에 대해 더 알고 싶다면, Jake Vanderplas의 [Conda myths and misconceptions](https://jakevdp.github.io/blog/2016/08/25/conda-myths-and-misconceptions/)을 읽어보길 권한다.

<a name='ref'></a>

### Reference
* [Anaconda](https://anaconda.org/)
* [Conda Documentation](http://conda.pydata.org/docs/using/index.html)
* [Deep Learning Nanodegree Foundation on Udacity](https://www.udacity.com/course/deep-learning-nanodegree-foundation--nd101)
* [Package: pandas](http://pandas.pydata.org/)
* [TensorFlow](https://www.tensorflow.org)
* [issue: `conda env export > envirionment.yaml`](https://github.com/conda/conda/issues/4596)
