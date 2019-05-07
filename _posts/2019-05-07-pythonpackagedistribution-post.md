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
 
이 문제들을 해결하기 위해서는 내가 만든 패키지를 PyPi에 오픈소스로 배포하는 방법밖에는 없었다.  
 
이번 기회를 통해서 파이썬 패키지 배포하는 법을 공부하게 되었다.  
나중에 배포하게 될 일이 또 있을 것 같아 스스로 정리하는 차원에서 글을 써본다.  
 
참고로, 나의 개발환경은 Windows 10, Python 3.6.8 버전, Pycharm IDLE 이다. (아나콘다는 쓰지 않는다)  
배포하면서 다른 기술 블로그들을 참고하였는데, 대부분의 개발자 분들은 리눅스나 맥 환경에서의 구현을 보여주고 계셔서 약간의 어려움을 극복해야 했다.
(물론 그냥 내가 리눅스나 맥을 쓰면 해결될 문제이긴하다...)

 
## 패키지 만들기 ##    
 
Pycharm 환경에서 보통 개발할때 C:\Users\사용자명\PycharmProjects 에 프로젝트 폴더를 만들고 그 안에 가상환경을 구축하여 사용한다. 
가상 환경은 보통 프로젝트 폴더 안에 venv라는 폴더로 되어있다. 그림으로 보면 아래와 같다. 설명을 위해 'TestPack'이라는 패키지를 만들었다고 가정하겠다.

![image](https://user-images.githubusercontent.com/34860302/57270806-d40dfc80-70c7-11e9-86f5-86d4b123dc5a.png)  
 
##### (Figure 1) PycharmProjects 폴더 안에 패키지 폴더를 만듦 #####   
 
 
![image](https://user-images.githubusercontent.com/34860302/57270751-927d5180-70c7-11e9-8db2-65f8d0ad8e5d.png)  
 
##### (Figure 2) 패키지 폴더 안에 venv를 구축하여 사용함 #####   
 
 
이 상태에서 'TestPack'을 배포해 보겠다.
 
## 패키지 배포에 필요한 파일 작성 ##  
 
패키지 배포에 필요한 파일들을 작성해야한다.  
 
* setup.py  
* setup.cfg  
* README.md  
* MANIFEST.in  
 
작성은 패키지 폴더가 들어있는 루트 디렉토리인 PycharmProjects 폴더에 한다.  
 
![image](https://user-images.githubusercontent.com/34860302/57270860-facc3300-70c7-11e9-9f72-d400be634ac5.png)  
 
각 파일의 내용 작성법은 Reference에 참조한 다른 기술블로그들을 참조하는 것을 추천한다. (나는 각 항목들의 자세한 내막은 잘 모르기에...)  

위의 4개의 파일 중 반드시 필요한 것은 setup.py 하나이다. 나머지는 필요하면 넣고 아니면 빼도 상관 없다.
 
TestPack 기준으로 작성한 setup.py 내용이다

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
    packages         = find_packages(exclude = ['docs', 'tests*']),
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
 

## Reference ##    
(OS) Windows 10 Pro  
(Programming Language) Python 3.6.8   
(IDLE) Pycharm   
(Figure 1,2,3) 자체 제작   
(1) <https://code.tutsplus.com/ko/tutorials/how-to-write-your-own-python-packages--cms-26076>
(2) <https://rampart81.github.io/post/python_package_publish/>

  
