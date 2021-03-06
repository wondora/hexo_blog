---
title: hexo + github + blog 연동하기
date: 2022-01-31 17:21:27
categories: [Web programming, Hexo]
tags: [hexo]
---

### hexo 시작하기

hexo 라는걸 시작하기 위해 몇가지 준비물이 있다.

* node 설치
* git 설치
* github에 블로그로 사용할 빈 repository 생성
* github에 hexo 설정을 저장할 빈 repository 생성


### hexo 설치

간단하다. hexo 페이지에도 나와있는것처럼 아래 명령어를 실행해주면 된다.

```bash
$ npm install -g hexo-cli
```
블로그로 운영할 폴더 hexo 초기화
폴더 구조로 구성이 되기 때문에 임의의 폴더를 하나 만들고 해당 폴더를 hexo 명령어로 초기화 시켜준다.

```bash
$ mkdir <디렉토리명>
$ hexo init <디렉토리명>
```

### 로컬서버 띄워보기

이제 로컬에서 서버를 띄워서 블로그가 어떻게 나오는지 확인을 해보면 된다.

```bash
$ hexo s (or server)
```

간혹 서버가 실행이 안되거나 오류가 발생, 수정한 부분이 반영이 안된다면 clean 명령어를 한번 해준 다음에 다시 서버를 실행해주면 되는 경우도 있다.

```bash
$ hexo clean
$ hexo s (or server)
```
http://localhost:4000 을 접속해서 정상적으로 페이지가 나오는지 확인을 해보자. 페이지가 정상적으로 나온다면 성공!

### 글 작성

아래 명령어를 실행하면 /source/_post/ 아래에 .md 파일이 생성이 된다.

```bash
$ hexo new <글 제목>
```

해당 파일을 사용하기 편한 에디터로 열어서 마크다운 문법에 맞추어 수정을 하면 끝!

왜 두개의 repository가 필요한가
아래에서 이야기 하겠지만, 하나는 실제 블로그 내용이 올라갈 저장소이고 다른 하나는 블로그를 운영하고 있는 hexo 자체를 저장할 저장소이다. hexo 정보를 가지고 있지 않을꺼라면(필자처럼 다양한 PC에서 업로드 환경을 구축하지 않을꺼라면) 하나의 레파지토리만 필요할수도 있다.

### github 셋팅

지금부터가 알짜배기다. 즉 이글을 포스팅 하는 의미. 다른 글들에서도 hexo 사용법을 친절하게 알려 주셨으나 github와의 연동, 그리고 어떤식으로 운영해야할지는 찾기 힘들었다. 필자는 감으로 그런가보다(?)하고서 터득한 바를 공유하려한다. (이게 정답은 아니지만, 나는 이렇게 사용하는게 맞겠다 싶어..)

일반적으로 github에서 블로그로 사용할 repository를 만들게 되면 http://(github아이디).github.io/(repository이름) 으로 블로그가 만들어 지는데 뭔가 조금 이상해서 간지가 안나서 찾고 찾아서 아래와 같은 방식으로 하게 되었다. 필자의 github 아이디는 taetaetae 이고 도메인은 아이디 그대로를 사용하여 http://taetaetae.github.io 으로 사용하고 싶었다. 따라서 github에 repository이름을 taetaetae.github.io로 만들어야 한다. 여기까지만 하면 일단 github에 배포할수 있는 준비가 되어있는 상태

### hexo로 배포하기

포스팅한 글이 정상적으로 등록이 된 것을 로컬서버에서 확인이 되었으면 이 상태를 조금전 만든 github repository으로 배포(정확히 말하면 git push)해줘야 한다. 그전에 최상위 폴더에 있는 _config.yml 파일을 열어서 github 정보를 입력해 줘야 한다. 하단 영역 Deployment 부분에 다음과 같이 작성하고 저장한다.

```bash
# Deployment
deploy:
  type: git
  repo: https://github.com/taetaetae/taetaetae.github.io
  branch: master
```
그 다음 hexo 에서 github로 배포할수있는 플러그인을 설치해준다.

```bash
$ npm install hexo-deployer-git --save
```

이제 설정한 github에 배포를 하면 끝!

```bash
$ hexo deploy
```

1분~3분 뒤에 도메인을 접속하여 정상적으로 페이지가 나오는지 확인하고, github에 파일들이 push가 잘 되었는지를 확인한다.

### 향후 관리 hexo 정보 저장

나중에 다른 PC에서도 블로그를 포스팅 할 경우가 있으니 hexo를 이용하여 포스팅 한 환경 자체를 저장 해야할 필요가 생겼다. 따라서 만들었던 폴더 또한 github에 업로드를 해놓는게 좋을것 같다.

### git 초기화

```bash
$ git init
```
### 변경사항 추가(전체)

```bash
$ git add .
```

### 커밋

```bash
$ git commit -m "커밋메시지"
```

### remote repository 등록

```bash
$ git remote add origin https://github.com/taetaetae/hexo.git
```

remote repository로 push

```bash
$ git push origin master
```

### 마치며

아직 git command 나 markdown, hexo 등 잘 모르는 부분들이 많다. 하나씩 시행착오를 겪어가면서 정리될수 있는 부분들은 이어 정리를 할 계획이다. 그리고 hexo 기본개념이나 설정파일 수정하는 부분들은 다른 분들이 많이 올려놓으셨기 때문데 중요하다고 생각되는 지극히 개인적으로 정리해야겠다고 느끼는 부분들 중심으로 정리를 해야겠다.

---

위의 글은 [천성의 블로그] 중 해당글을 발췌함

[천성의 블로그]: https://chinsun9.github.io "천성의 블로그로 이동합니다."