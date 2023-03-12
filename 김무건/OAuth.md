# OAuth란

---

- **자신이 소유한 리소스에 소프트웨어 애플리케이션이 접근할 수 있도록 허용해 줌으로써 접근 권한을 위임해주는 개방형 표준 프로토콜**

보통 소셜 로그인에 많이 사용한다.

### 구성 요소

1. Resource Owner(리소스 소유자): 자원(서비스)에 대한 접근 권한을 가지고 있는 사용자
2. Client(클라이언트): 자원에 접근하고자 하는 애플리케이션 또는 서비스
3. Resource Server(리소스 서버): 자원(서비스)을 보유하고 있는 서버
4. Authorization Server(인증 서버): Resource Owner의 권한을 인증하고, Client에게 Access Token을 발급해주는 서버

![image](https://user-images.githubusercontent.com/103854287/224526724-b148ef13-b092-476c-b16b-b3a5df2f51bc.png)

### 동작 방식

1. Client가 Resource Owner에게 서비스 이용을 위한 권한을 요청합니다.
2. Resource Owner는 Authorization Server로부터 인증을 받고, Client에게 Access Token을 발급합니다.
3. Client는 Access Token을 이용하여 Resource Server에서 자원(서비스)에 대한 요청을 보냅니다.
4. Resource Server는 Access Token을 검증하고, 요청에 대한 응답을 보냅니다.

### 질문 리스트

1. **OAuth란 무엇인가요?**
- OAuth는 서비스 제공자(Service Provider)와 사용자 인증을 담당하는 인증 공급자(Authentication Provider) 사이의 권한 위임을 가능하게 해주는 프로토콜입니다.

1. **OAuth를 사용하는 이유는 무엇인가요?**
- OAuth를 사용하면, 사용자는 자신의 정보를 공유하거나 민감한 정보를 제공할 필요 없이, Client가 Resource Server에 접근할 수 있습니다. 또한, Resource Server는 자신의 데이터를 안전하게 유지하면서도, 다른 애플리케이션과 연동하여 서비스를 제공할 수 있습니다.

1. OAuth에서 Resource Owner, Client, Resource Server, Authorization Server가 각각 무엇을 의미하나요?
- Resource Owner(리소스 소유자): 자원(서비스)에 대한 접근 권한을 가지고 있는 사용자
- Client(클라이언트): 자원에 접근하고자 하는 애플리케이션 또는 서비스
- Resource Server(리소스 서버): 자원(서비스)을 보유하고 있는 서버
- Authorization Server(인증 서버): Resource Owner의 권한을 인증하고, Client에게 Access Token을 발급해주는 서버

1. **OAuth 인증 과정은 어떻게 이루어지나요?**
- Client가 Resource Owner에게 서비스 이용을 위한 권한을 요청합니다.
- Resource Owner는 Authorization Server로부터 인증을 받고, Client에게 Access Token을 발급합니다.
- Client는 Access Token을 이용하여 Resource Server에서 자원(서비스)에 대한 요청을 보냅니다.
- Resource Server는 Access Token을 검증하고, 요청에 대한 응답을 보냅니다.

1. **OAuth 1.0과 OAuth 2.0의 차이점은 무엇인가요?**
- OAuth 1.0은 서명된 요청(Signed Request) 방식을 사용하고, OAuth 2.0은 토큰 기반(Token-based) 방식을 사용합니다. 또한, OAuth 2.0은 더욱 간결하고 확장성이 높은 구조를 가지고 있습니다.

1. **OAuth 2.0에서 Access Token과 Refresh Token이 무엇인가요?**
- Access Token은 클라이언트가 Resource Server에서 자원(서비스)에 접근할 때 사용되는 인증 토큰입니다.
- Refresh Token은 Access Token의 유효 기간이 만료되었을 때, 새로운 Access Token을 발급받기 위해 사용되는 토큰입니다.
