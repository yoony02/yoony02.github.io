---
layout: post
title:  "gitblog 생성기"
date:   2025-02-08 16:36:00
categories: 사담
tags: 사담
image: /assets/article_images/2014-08-29-welcome-to-jekyll/desktop.JPG
---

깃 블로그를 만들었다가, 제대로 사용하지 못한 채 벌써 2년이 흘렀다. <br>
기록도, 리서치도 계속 하는데 한 군데에 정보가 모여 있지도 않고 검색이 되지 않는 게 어려워서 다시 시작한다.

## 1. 깃 레포 만들기

블로그 관련 코드를 모아둘 새로운 레포지토리 (Repository) 를 만든다.

✔️ 이때 반드시 Repository 이름을 다음의 형식으로 만들어 준다. 
```
{username}.github.io
```
✔️ public 에 체크 <br>
✔️ `Add a README file` 에 체크

## 2. 로컬 저장소와 연결

레포를 만들었다면, 로컬 폴더와 연결한다. 

✔️ git clone 을 진행한다.
```
cd git_blog
git clone {아까 만든 레포 주소}
```

✔️ 클론한 폴더에는 아마 `README.md` 만 있을 것이고, `index.html` 등을 추가해서 블로그가 잘 작동하는지 확인 가능하다. [참고 블로그 1](https://zeddios.tistory.com/1222)


## 3. 내 것으로 만들기

이제 테마를 변경하고, 실제 배포를 해서 내 블로그로 만들려면 ruby 와 jekyll 을 설치하여야 한다.

✔️ 모든 절차는 아까 연결한 로컬 저장소 폴더에서 이루어진다. 

### 3-1. Ruby

✔️ cmd / terminal 을 켜서 다음의 명령어들을 입력한다. 

#### ruby 버전 관리 툴 설치

```zsh
brew update
brew install rbenv
```

#### rbenv 를 통해 ruby 설치
✔️ 버전이 매우 헷갈리는데, [github pages의 ruby 버전](https://pages.github.com/versions/)에 맞게 설정했다. [참고 블로그 2](https://1221jyp.com/posts/github-blog_1/)
```zsh
rbenv install 3.3.4
rbenv rehash
rbenv global 3.3.4
```

#### ruby 환경 설정
```zsh
vi ~/.zshrc

# vim 편집기로 이동 후, INSERT 모드 진입하여 아래 내용 복붙
export PATH={$Home}/.rbenv/bin:$PATH && \
eval "$(rbenv init -)"

# ESC -> :wq 로 vim 편집기 빠져나가기
```

### 3-2. Jekyll
✔️ 루비는 끝. 이제 지킬만이 남았다.

#### jeykll 설치
```zsh
gem install jekyll bundler
```

#### ERROR 발생 
✔️ 에러가 발생했다. 해당 폴더에 접근 권한이 없다보다.

```zsh
(base) github.io [main●●] gem install jekyll        
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```

✔️ 권한이 없다는 것인데, 보안상 sudo 를 통한 root 권한 실행은 권장하지 않는다고 한다. 그래서 rbenv 를 사용해서 해결했다. [참고 블로그 3](https://ccomccomhan.tistory.com/282)

```zsh
brew install rbenv ruby-build  # 아까완 달리 ruby-build 라는 게 생겼다. 
```

✔️ ruby 버전을 변경하는 등 다른 절차도 있긴 한데, 모두 생략하고 환경설정만 건드려 주었다. 
```zsh 
vi ~/.zshrc

# vim 편집기로 이동 후, INSERT 모드 진입하여 아래 내용 복붙
[[ -d ~/.rbenv  ]] && \             # <- 요 내용이 추가되었다. 
export PATH={$Home}/.rbenv/bin:$PATH && \
eval "$(rbenv init -)"

# ESC -> :wq 로 vim 편집기 빠져나가기

source ~./zshrc
```

✔️ 이제 다시 jekyll 을 설치한다. bundler 도 같이 설치한다.

```zsh
gem install jekyll bundler

# 버전 확인 
jekyll -v
```


### 3-3. 테마 설정
✔️ 깔끔한 무료 테마를 [찾다가](https://jekyllthemes.io/free), [mediator](https://github.com/dirkfabisch/mediator) 를 적용했다.  

#### 선택한 테마 git 에 접속해서 파일 내려받기
우상단 `code` 버튼을 클릭해서 `Download ZIP` 을 하면 된다. 

#### 적용
✔️ 다운받은 ZIP 파일 해제
✔️ 폴더 내 파일을 아까 연동해두었던 로컬 폴더로 복사/붙여넣기 한다.
✔️ 겹치는 파일이 있을 수 있는데, 이는 모두 '대치' 처리 한다. 

#### 로컬 배포 
아래 명령어들을 입력하면, Server address 가 나온다. *아마도 http://127.0.0.1:4000 일것..*

```zsh
bundle install
bundle exec jekyll serve
```

주어진 주소로 접속해보면, 테마가 적용되어 있는 것을 확인해볼 수 있다.

최소한으로 `_config.yml` 파일 내 노출 정보들을 본인의 정보로 바꿔서 해본다면 테스트도 가능하다.

`ctrl+c` 로 **로컬 배포 상태를 종료**한다. 이 종료를 꼭 해줘야 내용 변경 후에도 꼬임 없이 잘 적용된다.

#### 실제 배포 
실제 배포를 위해서는 github 에 변경된 파일들을 push 해야 한다.

```zsh
# 용도에 맞게 추후에 변경해서 쓰면 된다. 

git add .
git commit -m "add: new files"
git push origin master
```
push 가 되면 자동적으로 배포가 될 수도 있고, 아니면 git 레포 웹에 접속하여 빌드 할 수도 있다.

1. 해당 git 레포 접속 
2. 레포의 Setting 클릭 
3. 왼쪽 카테고리 하단의 Pages 클릭 
4. Build and Deploy 에서 git action 실행

-------- 

<br>

오랜만에 깃 블로그를 다시 만드니 (귀찮고) 재밌었다. 


* [참고 블로그 1](https://zeddios.tistory.com/1222)
* [참고 블로그 2](https://1221jyp.com/posts/github-blog_1/)
* [참고 블로그 3](https://ccomccomhan.tistory.com/282)