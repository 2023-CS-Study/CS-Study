# JWT(JSON Web Token)

---

### JWT 구조

1. **Header** : JWT의 종류와 암화화에 사용되는 알고리즘 지정한다.

- 구성 : alg(서명 알고리즘 , SHA256) , typ (JWT 타입을 지정 , 생략 가능)

```jsx
{
	"alg" : "HS256",
	"typ" : "JWT"
}
```

그런 다음 이 JSON은 **Base64Url로** 인코딩되어 JWT의 첫 번째 부분을 구성합니다.

1. **Payload** : 사용자 정보와 클레임 정보를 인코딩하여 저장 / 클레임은 보통 엔티티(사용자) 및 추가 데이터에 
    
    대한 설명으로 이름-값 쌍의 형태로 이루어진다.
    
    1.  **등록된 클레임(Registered**): 필수는 아니지만 유용하고 상호 가능한 클레임을 제공하기 위해 권장되는 미리 정의된 클레임 그들 중 일부는 
        
        'iss' (발급자), 'exp' (만료 시간), 'sub' (주제) , `aud' (대상) ..
        
    
    1. **공개 클레임(Public)**: JWT를 사용하는 애플리케이션에서 정의할 수 있는 클레임 이름 
        
        하지만 충돌을 방지하려면 [IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml) 정의하거나 충돌 방지 네임스페이스를 포함하는 URI를 정의 해야됨
        
    
    - [IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml)
        
        인코딩, 디코딩, 검증 및 사용에 대한 공식적인 규격
        
    
    1. **개인 클레임(Private)**: 사용에 동의한 당사자 간에 정보를 공유하기 위해 생성된 사용자 정의 클레임 
    
    ```jsx
    {
      "sub": "1234567890",
      "name": "John Doe",
      "admin": true
    }
    ```
    
    그런 다음 페이로드는 **Base64Url로** 인코딩되어 JSON 웹 토큰의 두 번째 부분을 형성합니다.
    
2. **Signature** : Header와 Payload를 조합하여 암호화된 문자열로 구성한다.

HMAC SHA256 알고리즘을 사용하려는 경우 서명은 다음과 같은 방식으로 생성됩니다.

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/29f66683-08e3-4cf4-a4d9-70c61bc462a8/Untitled.png)

## JWT 동작

---

JWT를 발급받은 클라이언트는 이를 HTTP 요청의 Authorization 헤더에 포함하여 서버에 전송합니다. 이후 서버는 받은 JWT를 디코딩하여 사용자 정보와 클레임 정보를 확인합니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aea1b840-4b1e-47a7-ace5-67a7892ecf0a/Untitled.png)


### JWT의 작동 과정

1. 사용자가 로그인을 합니다. 이때, 서버는 사용자 인증 정보를 검증하고, JWT를 발급합니다.
2. 서버는 발급된 JWT를 클라이언트에게 전송합니다.
3. 클라이언트는 JWT를 저장하고, 이후 서버에 요청을 보낼 때마다, HTTP 요청 헤더나 URL 매개변수에 JWT를 포함시켜 전송합니다.
4. 서버는 JWT를 검증하고, 해당 요청에 대한 권한을 확인합니다.
5. 만약 JWT가 만료되거나 유효하지 않다면, 서버는 요청을 거부합니다.
