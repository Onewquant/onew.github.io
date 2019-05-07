---
layout: single
title: "Windows 10 환경에서 파이썬 패키지 배포 (Distribution of own python package in windows 10)"
author_profile: false
tag: 
- python
- windows
- programming-techniques
categories: 
- python
---

Curvelib 이라는 퀀트트레이딩 툴을 만들었는데, 사용하다보니 몇 가지 불편함이 느껴졌다. 

> (1) 워크스테이션 노트북에서 코드를 수정하여 github에 올리는데, 다른 컴퓨터에서 사용하려면 매번 github에서 다운 받아야 한다.    
> (2) 다른 사람이 사용하기에는 다운로드, 설치, 개발환경설정 등이 너무 복잡하다  
 
이 문제를 해결하려면 내가 만든 패키지를 PyPI에 오픈소스로 배포해야 했다.  
 
이번 기회를 통해서 파이썬 패키지 배포하는 법을 알게 되었다.  
혹시나 나중에 배포하게 될 일이 또 있을 것 같아 스스로 정리하는 차원에서 글을 써본다.  
 
참고로, 나의 개발환경은 Windows 10, Python 3.6.8 버전, Pycharm IDLE 이다. (아나콘다는 쓰지 않는다)  
배포하면서 다른 기술 블로그들을 참고하였는데, 대부분의 개발자 분들은 리눅스나 맥 환경에서의 구현을 보여주고 계셔서 약간의 어려움을 극복해야 했다.
(물론 그냥 내가 리눅스나 맥을 쓰면 해결될 문제이긴하다...)

 
## Step1. 패키지 만들기 ##    
 
Pycharm 환경에서 보통 개발할때 C:\Users\사용자명\PycharmProjects 에 프로젝트 폴더를 만들고 그 안에 가상환경을 구축하여 사용한다. 
가상 환경은 보통 프로젝트 폴더 안에 venv라는 폴더로 되어있다.  
 
그림으로 보면 아래와 같다. 설명을 위해 'TestPack'이라는 패키지를 만들었다고 가정하겠다.  
여기서 중요한 것은 패키지 폴더 내에 ```__init.py__``` 파일이 반드시 있어야 패키지로 인식된다는 점이다.  
따라서 만약 이 파일을 안 만들어 놓으면 패키지 폴더가 포함 안 된 채 빈 껍데기 패키지만 빌드업 된다.  
파일 속 내용은 비어있더라도 ```__init__.py``` 파일 자체는 꼭 만들자 !  
 
![image](https://user-images.githubusercontent.com/34860302/57270806-d40dfc80-70c7-11e9-86f5-86d4b123dc5a.png)  
 
##### (Figure 1) PycharmProjects 폴더 안에 패키지 폴더를 만듦 #####   
  
  
   
![image](https://user-images.githubusercontent.com/34860302/57276560-f6f6db80-70dc-11e9-9b9a-3265916b7405.png)  
 
  
   
##### (Figure 2) 패키지 폴더 안에 venv를 구축하여 사용함 #####   
 
 
이 상태에서 'TestPack'을 배포해 보겠다.
 
## Step2. 패키지 배포에 필요한 파일 작성 ##  
 
패키지 배포에 필요한 파일들을 작성해야한다.  
 
* __setup.py__  
* __setup.cfg__  
* __README.md__  
* __MANIFEST.in__  
 
작성은 패키지 폴더가 들어있는 루트 디렉토리인 PycharmProjects 폴더에 한다.  
 
![image](https://user-images.githubusercontent.com/34860302/57270860-facc3300-70c7-11e9-9f72-d400be634ac5.png)  
 
각 파일의 내용 작성법은 Reference에 참조한 다른 기술블로그들을 참조하는 것을 추천한다. (나는 각 항목들의 자세한 내막은 잘 모르기에...)  

위의 4개의 파일 중 반드시 필요한 것은 __setup.py__ 하나이다. 나머지는 필요하면 넣고 아니면 빼도 상관 없다.
 
#### (1) setup.py ####
 
TestPack 기준으로 작성한 __setup.py__ 내용이다

```
from setuptools import setup, find_packages

setup(
    name             = 'TestPack',
    version          = '1.0.0',
    description      = 'Test package for distribution',
    author           = 'Onew',
    author_email     = 'seunghwan0216@gmail.com',
    url              = 'https://github.com/onewquant/TestPack',
    download_url     = 'https://github.com/Onewquant/TestPack/archive/master.zip',
    install_requires = ['bs4','pandas<=0.22.0','urllib3'],
    packages         = find_packages(exclude = ['tests*']),
    keywords         = ['TestPack', 'testpack'],
    python_requires  = '>=3',
    package_data     =  {
        'TestPack' : [
            'testpack_configs.txt',
    ]},
    zip_safe=False,
    classifiers      = [
        'Programming Language :: Python :: 3.5',
        'Programming Language :: Python :: 3.6'
    ]
) 
```  
 
대부분의 항목들은 직관적으로 어떤 내용을 쓰면 될지 알 수 있다.  
헷갈리는 부분만 짚어보면,  
* __install_requires__ 는 TestPack 내에서 import해서 사용하는 다른 라이브러리 패키지를 써놓는 곳이다. 여기에 써 놓으면 나중에 pip를 통해 TestPack을 다른 컴퓨터에 설치할때, 써 놓은 패키지들은 자동으로 같이 설치가 된다.  
* __packages__ 는 만약 TestPack을 여러 개의 패키지 폴더 묶음으로 배포하고 싶다면 사용하는 옵션인데, 현재 여기서는 TestPack이 그런 상황은 아니므로 패스한다. 위에 써 놓은 내용은, 폴더 이름에 tests라는 문자가 포함된 폴더는 빼고 빌드 하겠다는 내용이다.  
* __package_data__ 는 패키지 안에 .py 파일이 아닌 다른 파일을 더 포함시키고 싶을때 쓰면 된다. TestPack 내에 일부러 testpack_configs.txt라는 파일을 포함시켜보도록 하겠다.
* __zip_safe__ 정확히 무슨 역할을 하는지는 잘 모르겠으나 package_data에 추가한 내용이 있으면 False로 설정해야한다고 한다...  
* __classifiers__ 파이썬 3.5와 3.6에서만 된다고 명시했다. (그냥 명시만 될 뿐 실제 에러가 따로 뜨거나 설치가 안되거나 하는 일은 없는 듯 하다.
 
#### (2) setup.cfg ####  
 
파이썬 공식 문서에 보면 setup.cfg의 역할에 대해 서술된 곳이 있다

> installers can override some of what you put in setup.py by editing setup.cfg
> you can provide non-standard defaults for options that are not easily set in setup.py
> installers can override anything in setup.cfg using the command-line options to setup.py

대략적인 내용은, setup.py에 뭔가 정형화되게 딱 설정해놓기 어려운 부분을 setup.cfg를 통해 사용자가 유도리 있게 쓸 수 있도록 도와주는 역할을 하는 파일이다. 음... 어떻게 잘 활용할 수 있을지는 차차 시간을 두고 알아보기로 한다... 일단은 참고한 기술블로그에서 나온 내용만 써 놓겠다. 패키지를 설명해주는 파일이 README.md 파일임을 명시해놓는 내용이다.

``` 
[metadata]
description-file = README.md
``` 

#### (3) README.md ####  
 
패키지에 대한 설명을 써놓는 곳이다. 맘껏 편집하면 될듯.  
 
#### (4) MANIFEST.in ####  
 
패키지 폴더 밖에 있는 다른 파일을 포함시키고 싶을때 쓴다.

여기서는 README.md 만 포함시켜보는 것으로 하겠다.
 
``` 
include README.md
``` 

여기까지가 배포를 위한 파일들을 작성하는 과정이다.
위에서도 이야기했지만, 가장 중요한 파일은 __setup.py__ 임을 기억하고 이걸 공들여 작성하면 된다. !
 
## Step3. wheel로 빌드업 하기 ##    
 
커맨드창 (cmd)을 실행시킨 후 루트디렉토리로 이동한다 (PycharmProjects 폴더)  
 
``` 
cd PycharmProjects  
```  
 
wheel이 이미 설치되어 있으면 바로 빌드업을 실행시키면 되지만, 아니면 wheel 설치 후 진행한다.  
 
설치는  
``` 
pip install wheel  
``` 
로 한다 (커맨드창 열린 상태에서 그냥 저 코드 치면 된다)  
 
설치가 완료되면 배포판으로 빌드업한다.  
``` 
python setup.py bdist_wheel
``` 
 
그러면 아래 그림들과 같이 루트디렉토리(PycharmProjects 폴더)에 dist 라는 폴더가 생성되고 거기에 배포판 파일이 생성된다.  
 
![image](https://user-images.githubusercontent.com/34860302/57277206-a2ecf680-70de-11e9-9481-41bc290952ee.png)  
 
![image](https://user-images.githubusercontent.com/34860302/57277278-ca43c380-70de-11e9-8afb-aaf7680b37f8.png)  
 
여기까지가 배포판으로 빌드업하는 과정이다.  
 
빌드업 완료된 파일 이름은 step4에서 배포시 사용되므로 클립보드에 복사해 두던지 하면 편하다.

내용을 정리하는 차원에서 커맨드창에서 이루어지는 빌드업 전체 과정을 사진으로 한 눈에 보면 다음과 같다.  
 
![image](https://user-images.githubusercontent.com/34860302/57275276-fceabd80-70d8-11e9-90ee-a805bdeee3fb.png)  
 
## Step4. twine으로 PyPI에 배포하기 ##    
 
빌드업이 완료된 후에는 이제 마지막 과정인 배포만 하면 된다.  
 
PyPI 홈페이지로 들어가서 <https://pypi.org/>  
 
PyPI에 미리 가입을 해놓는다.  
 
이후 커맨드창(cmd)에서 루트디렉토리로 이동한 상태에서 다음 코드를 하나씩 실행해 나간다.  
 
twine이 설치되어 있지 않다면 다음 코드로 설치먼저 한다.
 
``` 
pip install twine  
```  
 
이후 아까 빌드업한 파일 이름을 사용하여 twine으로 배포한다.  
 
``` 
twine upload dist/TestPack-1.0.0-py3-none-any.whl
``` 
 
본인의 PyPI 아이디와 비밀번호를 치면 배포과정이 시작된다.  
비밀번호를 치는데 화면에 입력이 안되는 것처럼 보인다면, 그냥 신경쓰지 않아도 된다.  
그냥 입력하고 엔터치면 잘 들어가 있다. (나의 경우는 이것 신경쓰다가 괜히 시간 허비했다..)
 
커맨드 창에서의 전체 입력 과정을 정리해 보면 다음 그림과 같다.  
 
![image](https://user-images.githubusercontent.com/34860302/57278334-2a3b6980-70e1-11e9-842a-2cf44302c1f2.png)  
 
![image](https://user-images.githubusercontent.com/34860302/57278436-67076080-70e1-11e9-9f46-54b13ca5cf09.png)  
 
모든 과정을 다 마쳤다면 PyPI에 패키지가 정상적으로 등록될 것이다.  
아래 그림은 내가 배포한 wecolib 패키지이다.
 
![image](https://user-images.githubusercontent.com/34860302/57278758-5dcac380-70e2-11e9-8e42-053904a1017f.png)  
 
배포 후에 패키지 설치는 바로 할 수 있으나 검색은 바로 안 될 수 있다.  
시간이 좀 흐른 뒤(나의 경우는 0.5~1일 정도) 검색도 완전히 되는 오픈 소스 패키지 제작자가 될 수 있었다.  
 
더 세밀한 컨트롤과 제작은 아래 Reference에 첨부한 기술블로그들을 참고하시길.  
 
## Reference ##    
(OS) Windows 10 Pro  
(Programming Language) Python 3.6.8   
(IDLE) Pycharm   
(Figure 1,2 및 기타 스크린샷) 자체 제작   
(1) <https://code.tutsplus.com/ko/tutorials/how-to-write-your-own-python-packages--cms-26076>  
(2) <https://rampart81.github.io/post/python_package_publish/>  
(3) <https://docs.python.org/3/distutils/configfile.html>  
  
