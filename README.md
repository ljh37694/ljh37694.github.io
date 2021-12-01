설치 환경: Windows

## Ruby 설치
- RubyInstaller 홈페이지에 들어가서 Ruby+Dekit을 다운로드
- 실행 후 ```ridk install```를 입력하고 1, 3을 설치
- ```gem install jekyll bundler```입력
- ```jekyll -v```를 통해 설치 확인


## Jekyll 테마 설정
- repository를 ```ljh37694.github.io```로 설정
- C 드라이브에 Blog폴더를 만들고 ```git init``` 후에 ```git clone https://github.com/ljh37694/ljh37694.github.io```
- minimal-mistakes를 다운로드 받고 ljh37694.github.io 폴더에 압축 해제 후 Ctrl C, Ctrl V
- 필요없는 파일 삭제


## Disqus를 이용한 댓글 기능 추가
- disqus 회원가입
- jekyll 사이트? 만들기
- \_config.yml에 아래 코드를 추가
```
comment:
  provider: "disqus"
  disqus:
    shortname: "hoon37694"
```
- posts.html에 disqus에서 복사한 코드 추가후 앞에 ```{% if page.comments %}``` 뒤에 ```{% endif %}``` 추가
- post를 작성할 때 ```comments: true```를 입력하면 해당 post에 댓글 기능 추가 
