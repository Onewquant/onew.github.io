---
layout: single
title: "Windows 10 환경에서 pyinstaller를 이용한 파이썬 .exe파일 배포 (Distribution of .exe file with pyinstaller library in windows 10)"
author_profile: false
tag: 
- python
- windows
- programming-techniques
categories: 
- python
---  
 
Python으로 다른 사람이 쓸 수 있는 자동화프로그램을 하나 만들었다.  
이 사람의 경우 파이썬을 전혀 사용하지 않기 때문에 .exe 파일로 사용할 수 있도록 만들어야 했다.  
 
이 역시 나중에 배포하게 될 일이 다시 있을 것 같아 스스로 정리하는 차원에서 글을 써본다.  
 
개발환경 : Windows 10, Python 3.6.8 버전, virtual env를 구성해서 사용하는 python 파일이라고 가정하겠다.  
 
배포하려는 파이썬 파일은 pyinstaller_test.py 이다.  
 
## Step1. 가상환경 활성화 ##    
 
가상 환경은 프로젝트 폴더 안에 venv라는 폴더로 되어있다.  
 
먼저 커맨드창(cmd)을 실행해 가상 환경을 activate 시킨다.  
 
![image](https://user-images.githubusercontent.com/34860302/58310693-31cd7300-7e42-11e9-8072-8b03e9c64a2f.png)  
 
##### (Figure 1) 가상 환경을 activate 시킴 #####   
  
  
## Step2. pyinstaller 설치 ##  
 
가상환경에 pyinstaller가 설치되어 있지 않다면 설치한다. (가상환경 activate 상태로 아래 명령어를 입력하면 된다.)   
 
```
pip install pyinstaller  
```  
 
이후 빌드하려는 파이썬 파일이 들어있는 폴더로 이동한다. (이 또한 가상환경 activate 상태로 !)  
 
![image](https://user-images.githubusercontent.com/34860302/58312038-6db60780-7e45-11e9-8e2b-10270a1bf379.png)  
 
##### (Figure 2) 빌드하려는 곳으로 이동 #####   
 
## Step3. pyinstaller로 파이썬 파일 빌드 ##  
 
```
pyinstaller -F pyinstaller_test.py  
```  
 
위의 명령어를 입력하면 그 안에 있는 모든 파일과 함께 .py 파일이 빌드된다.  
 
![image](https://user-images.githubusercontent.com/34860302/58312770-f2555580-7e46-11e9-96b3-027940cdd296.png)  
 
##### (Figure 3) 빌드를 위한 명령어 입력 시킴 #####   
 
빌드가 완료되면 아래 그림과 같이 dist라는 폴더가 생성되고, 그 안에 빌드 완료된 파일이 있게 된다.  
 
  
  
![image](https://user-images.githubusercontent.com/34860302/58312962-73145180-7e47-11e9-8f46-6e0d0df5af6a.png)  
 
##### (Figure 4) dist 폴더가 생성되었다 #####   
 
![image](https://user-images.githubusercontent.com/34860302/58313011-8cb59900-7e47-11e9-99a0-80377bf12af3.png)  
 
##### (Figure 5) dist 폴더 안에 pyinstaller_test.exe 파일이 생성되었다 #####   
 
이 파일을 더블클릭하면 아주 잘 실행된다.  
 
이 파이썬 파일을 돌리기 위한 라이브러리도 다 포함되어 있는 파일이며, 컴퓨터에 파이썬이 설치되어 있지 않아도 돌아간다 !  
 
일반 사용자를 위한 소프트웨어 배포에 아주 좋다.  
 
## pyinstaller로 빌드시 유의점 ##  
 
만약 pyinstaller_test.py 파일내의 코드에서 다른파일(e.g. Excell 파일 등)을 열거나 저장하려면 
빌드한 파일이 존재하는 위치로 지정해주던지, 아니면 아예 절대경로로 쓰면 된다.  
 
```
import pandas as pd  
 
## 절대 경로로 패스를 지정해주면 그 절대경로에 있는 파일이 열린다.  
 
df = pd.read_excel("C:/Users/Onew/PycharmProjects/Pyinstaller_Test/dataframe_test.xlsx")  
 
## 상대 경로로 패스를 지정하면 생성된 pyinstaller_test.exe 파일과 엑셀 파일을 한 폴더 안에 두고 실행해야 한다.  
 
df = pd.read_excel("./dataframe_test.xlsx")  
 
```  
 
pyinstaller 사용법을 더 자세히 알고싶다면 가상환경이 activate된 커맨드 창에서 다음을 입력하면 된다.  
 
```
pyinstaller -h  
```  
 
그럼 이렇게 보기 힘든 설명들이 나온다 ;;  
 
![image](https://user-images.githubusercontent.com/34860302/58313919-9b9d4b00-7e49-11e9-91c0-3c3d5f2b32f4.png)  
 
더 쉽게 배우시려면 다른 기술블로그들을 찾아보시길.  
 
## Reference ##    
(OS) Windows 10 Pro  
(Programming Language) Python 3.6.8   
(IDLE) Pycharm   
(Figure 1,2,3,4,5) 자체 제작   
  
