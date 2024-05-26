# 신입 or Jr 엔지니어 면접을 위한 리눅스 명령어 top 10

[유튜브 - 신입 or Jr 엔지니어 면접을 위한 리눅스 명령어 top 10](https://www.youtube.com/watch?v=u9RukvKZJZM&list=PLSJb8dsKrZ97QCyLe3HNwlpYxXJnpJqSF&index=7) 강의 정리


## 1. Server를 어떻게 접속하나요? 특별히 사용하는 도구나 방법이 있을까요?
> 신입 대상: SSH key 방식을 사용해 봤는지
> 
> 주니어 대상: 특정 도구와 대체 도구들에 대해
- SSH를 단순히 패스워드 뿐만 아니라 RSA Key를 이용한 인증을 해봤는지?

#### public key & private key의 관계
- Private key:
    - 개인 키
    - Public key로 암호화된 데이터를 복호화할 때 사용
- Public key:
    - 공개 키
    - 데이터를 암호화할 때 사용


- **SSH**: Secure Shell의 약자
- Well-known port:
    - 22: SSH
    - 80: HTTP
    - 443: HTTPS


<br>

## 2-1. IP를 확인하는 리눅스 명령어는 무엇인가요?

#### IP 조회하는 명령어
```bash
ifconfig
```

## 2-2. 자신의 Public Ip는 어떻게 확인하나요?

#### Public Ip 확인하는 명령어
```bash
curl ifconfig.co

curl ifconfig.me
```

127.0.0.1: 자신을 의미

<br>

## 3. 웹사이트가 잘 동작하는지 체크할 때 사용하는 명령어는?

> - curl을 사용해 본 적은 있는지?
> - curl을 얼마나 사용해봤는지?(옵션을 얼마나 다뤄봤는지)

**CURL**: Client URL

#### curl 옵션
- `-X`(--request): 요청 시 사용할 HTTP Method 설정
- `-H`: HTTP Header 설정
- `-d`(--date): HTTP Request body 설정
- `-v`(--verbose): 다양한 세부 정보 출력

```bash
curl google.com

curl -X POST https://www.domain.com \
     -H "X-USER-ID: user_id1" \
     -H "Content-Type: application/json" \
     -d '{"name": "apple"}'
```

<br>

## 4. 도메인의 IP를 조회하는 명령어는?




<br>

## 📚 References
- [유튜브 - 신입 or Jr 엔지니어 면접을 위한 리눅스 명령어 top 10](https://www.youtube.com/watch?v=u9RukvKZJZM&list=PLSJb8dsKrZ97QCyLe3HNwlpYxXJnpJqSF&index=7)