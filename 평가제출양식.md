# 1 수행형 과제

## 1.1 목표 시스템의 환경 요구 사항에 대한 분석 능력 [p.5]
* 목표 시스템을 정하고, 그에 해당하는 환경을 분석해 보자.
  - 목표 시스템 : [내용 채우기]
  - 환경
    - 개발 언어 : [내용 채우기]
    - 개발 인원 및 기간 : [내용 채우기]
    - 개발 HW 사양 : [내용 채우기]

## 1.2 개발 도구들의 역할에 대한 파악도 [p.7]
* 목표 시스템 개발에 활용한 도구들을 나열하고 역할에 설명해 보자.
```
  - [내용 채우기]
  -
  -
```
## 1.3 형상관리 지침의 이해와 환경 구축 능력 [p.22]
* 형상관리(SCM)의 정의를 설명해 보자.
```
[내용 채우기]
```

* 각자 형상관리툴에 이름과 이메일을 등록해 보자.(등록된 내용은 출력해서 붙여 넣기)
```console
[본인 화면 복사]
[샘플]
hkit00@ubuntu:~/tinypetshop$ git config --list
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=https://github.com/luibelstudy/tinypetshop.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master
hkit00@ubuntu:~/tinypetshop$
```


# 2 실행형 과제

## 2.1 만들고자 하는 서비스의 서버프로그램 구현
[본인 화면 복사]

## 2.2 서버프로그램에 대한 unit test 수행

### unit test 라이브러리 설치
* 설치
```console
$ pip3 install Flask
```

* 설치 결과
```console
[본인 화면 복사]
```

### 테스트 파일 추가
* test_app.py
```python
import flask

app = flask.Flask(__name__)

with app.test_request_context('/'):
    assert flask.request.path == '/'
```

### unit test 실행

* 실행 방법
```console
$ pytest
```

* 실행 결과
```console
[본인 화면 복사]
```

## 2.3 수행형 과제에서 선정한 서비스에 대한 공통 모듈 구현
[본인 화면 복사]

## 2.4 공통 모듈에 대한 unit test 수행

### unit test 라이브러리 설치

### 테스트 파일 추가
* test_app.py
```python
import flask

app = flask.Flask(__name__)

with app.test_request_context('/'):
    assert flask.request.path == '/'

with app.test_request_context('/getPosts.html?no=1'):
    assert flask.request.path == '/getPosts.html'
    assert flask.request.args['no'] == '1'
```

### unit test 실행

* 실행 방법
```console
$ pytest
```

* 실행 결과
```console
[본인 화면 복사]
```


## 2.5 잠재적 보안 취약성 제거를 위한 secret_key or CSRF Protection 적용

### csrf 라이브러리 설치
* 설치
```console
$ pip3 install Flask-WTF
```
* 설치 결과 
```console
[본인 화면 복사]
```

### CSRF 적용
* app.py
```python
    :
    :
from flask_wtf.csrf import CSRFProtect

app = Flask(__name__)
app.secret_key = 'very secret'
csrf = CSRFProtect(app)
    :
    :
```
* getPosts.html
```html
{% extends "layout.html" %}

{% block section %}
<h2>게시물 리스트</h2>
<ul>
    {% for post in posts %}
    <li><a href="getPost.html?csrf_token={{ csrf_token() }}&no={{post.no}}">{{post.subject}}</a> - {{post.content}}</li>    {% endfor %}

</ul>
<a href="addPost.html">추가</a>
{% endblock %}
```

### 작동 확인
* 콘솔에서 확인
```console
[본인 화면 복사]
[샘플]
hkit00@ubuntu:~/tinypetshop$ python3 app.py
 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on http://0.0.0.0:5100/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 105-203-154
192.168.0.1 - - [14/Jun/2020 22:48:05] "GET / HTTP/1.1" 200 -
192.168.0.1 - - [14/Jun/2020 22:48:18] "GET /getPosts.html HTTP/1.1" 200 -
192.168.0.1 - - [14/Jun/2020 22:48:21] "GET /getPost.html?csrf_token=IjMyMGFkN2FiYTUyNjE3OWU1NzQ5NzFkMjVmNDNhZTk0YjdhOWI4YjIi.XuapMg.855qVHuJtFJspJFU09cmP-P0lEc&no=1 HTTP/1.1" 200 -
```
* 브라우저에서 확인
[본인 화면 복사]
[샘플]
![](https://github.com/luibelstudy/hkit_2020_gonggong/blob/master/3.%20%EC%84%9C%EB%B2%84%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8%20%EA%B5%AC%ED%98%84/%ED%8F%89%EA%B0%80/hkit01.PNG?raw=true)
