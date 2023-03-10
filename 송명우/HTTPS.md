# HTTPS 란?
HTTPS 는 평문 데이터를 주고 받는 HTTP 에서 SSL, TLS 를 통한 암호화 기능이 추가된 프로토콜.

# HTTPS 에서 사용하는 암호화 방식은? 
- 대칭키와 비대칭키 암호를 모두 사용한다.
  - 데이터의 암호화에는 대칭키 암호를 사용.
  - 대칭키 전달에는 비대칭키 암호를 사용.

# 대칭키를 사용하는 이유는?
- 비대칭키 암호화방식이 대칭키 암호화방식에 비해 느리기 때문에 더 빠른 통신을 위해 대칭키 암호화를 사용한다.


# HTTPS 에서 대칭키를 교환하는 절차

## 대칭키를 그냥 주고 받는다면?
- MITM 공격을 통해 대칭키가 유출될 수 있다.

## 서버의 비밀키로 대칭키를 암호화하여 전달한다면?
- MITM 공격을 통해 서버의 공개키로 대칭키를 복호화할 수 있다.

## 클라이언트가 서버의 공개키로 대칭키를 암호화하여 전달한다면?
- 서버만 해당 대칭키를 복호화할 수 있겠지만 정말로 서버의 공개키인지 보장할 수 없다.

## 따라서 제 3자가 서버의 공개키를 보장할 필요성이 있다.

1. CA(인증기관) 의 비밀키로 서버의 공개키를 암호화하여 브라우저에게 전달한다.
2. 브라우저는 CA 의 공개키로 서버의 공개키를 복호화해낸다.
3. 브라우저가 생성한 대칭키를 서버의 공개키로 암호화하여 전달한다.


