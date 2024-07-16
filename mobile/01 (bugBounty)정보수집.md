정보수집 - 대상 서비스를 이용해보는 것이 중요. 큰 기능뿐만 아니라 세부적인 모든 기능을 이용해보아야 함 

구글 해킹 - 구글을 통해 해킹을 하기 위한 필요한 정보들을 찾아주는 과정

<br>

## 검색 연산자 🔍

구글 검색 엔진은 검색할 때 특정 조건으로 필터링할 수 있도록 검색 연산자가 있음

- site : 검색 결과 중 특정 사이트에서의 자료만 보고싶을 때 → 하위도메인 발견 가능
- inurl : URL 중에 특정 문자가 포함되어 있는 자료만 검색
- intitle : 타이틀에 특정 글자가 포함된 페이지 (디렉토리 리스팅에서 쓰임)
- link : 특정 링크를 포함하는 페이지 (숨겨진 페이지를 찾을 수 있음)
- filetype : 특정 유형의 파일들만 검색
- 와일드카드 : 농*곰
- 큰따옴표 : “농담곰 인형”

<br>

### 응용

특정 도메인 사이트, 페이지들 검색 : `site:*.target.com`

타겟 도메인 중 특정 경로 : `site:*.target.com inurl:admin` 

특정 회사의 AmazoneS3 버킷 : `site:s3.amazonaws.com target`

text파일 중 password 단어 검색 : `site:*.target.com etx:txt password`

LFI : `inurl:/etc/passwd root:x:0:0:root:/root:/bin/bash`

디렉토리 리스팅 취약점 : `intitle:index of`

<br>

### 🌟 구글해킹활용 공유하는 곳  : [google-hacking-database](https://www.exploit-db.com/google-hacking-database)

<br>

## 정보 수집 툴 🕷️

Spidering : 사이트에서 확인할 수 있는 모든 페이지를 식별하는 것 

`ZAP` : 웹 스파이더링을 할 수 있는 툴. https://www.zaproxy.org/download/ 에서 다운받을 수 있다 

<br>

### S3 Bucket 🪣

`S3` : Simple Storage Service (아마존의 온라인 저장 공간 서비스)

개발자가 DB를 구축하지 않고도 S3를 이용하여 쉽게 데이터를 관리할 수 있다 

<br>

### Github Recon 🐈‍⬛

그 기업의 Github 레포지토리 조사 → 민감한 정보, 취약점을 발견할 수 있는 단서 정보  (타겟 회사 이름, 프로젝트 이름, 개발자 이름&닉네임 조사)

`Issue` , `Code Commits` 에서 다양한 버그들을 발견할 수 있음.

Code History , Blame 에서도 버그 확인 가능

---

발견될 수 있는 정보들 → API key , 암호화 key , DB비번

검색 키워드 → key , secret , password , cred

---

🌟 **Github Recon Tool → [Gitrob](https://github.com/michenriksen/gitrob) , [TruffleHog](https://github.com/trufflesecurity/trufflehog)**
