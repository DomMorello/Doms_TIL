# fix CRLF problem
윈도우(Win32)에서 작성한 텍스트 파일을 유닉스/리눅스 환경에서 읽으면 에러가 나거나 ^M 이라는 이상한 문자가 찍힌다.
MS윈도우는 텍스트 파일의 끝에서 CR-LF로 줄바꿈을 하고, 유닉스는 LF 문자로 줄바꿈을 하기 때문이다.

vim 으로 아무 파일을 열고 나서 `esc`를 누른 후 `:se ff=unix` 명령을 입력하고 wq로 저장을 하면 세팅이 저장된다.
`:se ff=dos` , `:se ff=mac` 으로 설정할 수도 있다.
##### [출처]
- (http://mwultong.blogspot.com/2007/05/vim-vi-dos-cr-lf-to-unix-newline.html)
