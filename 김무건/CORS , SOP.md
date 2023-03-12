# *****CORS(Cross-Origin Resource Sharing)*****

---

CORS란 교차 출러 리소스 공유를 의미한다. 

기본적으로 웹 브라우저에서 다른 출처의 리소스를 접글할 때 SOP의 제약을 우회하여 리소스를 공유를 가능하게 하는 메커니즘으로 CORS를 사용하면 애플리케이션에서 다른 도메인 API를 호출하거나 원격 서버에 저장된 리소스를 로드할 수 있다.

<aside>
💡 출처란

</aside>

![image](https://user-images.githubusercontent.com/103854287/224526751-f109da98-97d0-4918-9449-2d1cba42de6f.png)


- 출처란 위 사진에 보면 Protocol , Host , 포트 번호를 합친 것(80 ,:443)

→ www.naver.com:433까지를 출처라고 생각하면 된다.

→ 이때 http는 80 https는 443으로 지정되어 있어 생략이 가능하다.

# SOP ( Same Origin Policy )

---

- 웹 브라우저에서 보안을 유지하기 위한 정책으로, 다른 출처(origin)로부터 로드되는 리소스에 대한 접근을 제한합니다.
    
    **SOP는 현재 페이지의 출처와 다른 출처의 리소스에 대한 접근을 차단합니다.**
    

- 출처 : 프로토콜 , 호스트 , 포트번호

이를 통해, 악의적인 스크립트 등에 의한 정보 탈취, 변조, XSS(Cross-Site Scripting) 등의 보안 문제를 예방할 수 있습니다.

SOP를 우회하는 방법

1.  CORS(Cross-Origin Resource Sharing)
2.  JSONP(JSON with Padding)

# CORS 동작 과정

---

### cors 접근 제어 시나리오는 크게 세가지가 있다.

1. **단순 요청(Simple Request)** 
- GET, POST, HEAD 중 하나의 메서드를 사용하고, 추가적인 헤더를 포함하지 않는 요청입니다.
- 이 경우, 브라우저는 서버에 preflight 요청을 보내지 않고, 바로 리소스에 접근

![image](https://user-images.githubusercontent.com/103854287/224526757-65a00bbc-9db1-4325-93f1-d0552f58153b.png)


- 이 경우에는 3개의 조건을 만족을 해야된다.

1. 요청의 메소드는 GET, HEAD, POST 중 하나여야 한다.

2. Accept, Accept-Language,Content Language,ContentType, DPR, Downlink,
Save-Data, Viewport-Width,Width를 제외한 헤더를 사용하면 안 된다.

3. 만약 ContentType를 사용하는 경우에는 application/x-www-form urlencoded,
multipart/form data, text/plain만 허용된다.

### 2. **Preflight Request**

![image](https://user-images.githubusercontent.com/103854287/224526768-18e1ca44-b2ea-4991-82ef-d8398889ad1d.png)

1. 브라우저는 Preflight Request를 보내기 위해 OPTIONS 요청을 서버에 보냅니다.
    
    OPTIONS 요청은 요청 헤더에 Origin, Access-Control-Request-Method, Access-Control-Request-Headers를 포함시킵니다.
    

1. 서버는 OPTIONS 요청을 받아들이고, 허용 여부를 결정합니다.

2. 서버는 이 요청에 대한 응답으로 Access-Control-Allow-Origin, Access-Control-Allow-Methods, Access-Control-Allow-Headers 등의 헤더를 포함한 200 OK 응답을 보냅니다.

3. 브라우저는 Preflight Request 과정을 거친 후에, 실제 요청을 보냅니다.

4. 실제 요청은 요청 헤더에 Origin 헤더가 포함됩니다.

5. 서버는 Access-Control-Allow-Origin 응답 헤더를 포함하여 CORS 요청을 허용합니다.
- 즉. 클라이언트는 출처 정보를 포함한 요청을 보내면 서버에서 요청을 받아 출처 정보를 얻게 된다. 이후 출처 정보가 Access-Control-Allow-Origin 판단한다.

6. 만약 서버가 CORS 요청을 허용하지 않는다면, 브라우저는 에러를 반환하거나, 요청을 중단합니다.

### *****Spring에서 CORS 이슈를 해결하는 방법*****

1. 백엔드에서 해결하는 방식
- 어노테이션 방식으로 해결

```jsx
@RestController
@CrossOrigin(origins = "http://localhost:8080")
public class MyController {

    @GetMapping("/my-resource")
    public String getResource() {
        return "Hello World!";
    }
}
```

**`@CrossOrigin`**
을 사용하면 서버가 자신의 자원에 액세스할 수 있는 도메인을 지정할 수 있습니다. 클라이언트가 서버에 요청을 보내면, 서버는 **`@CrossOrigin`**
 어노테이션을 확인하여 허용된 도메인인 경우 요청된 자원을 반환합니다.

- Config방식

```jsx
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")  // 모든 경로에 대해 CORS를 허용
                .allowedOrigins("http://localhost:8080") //허용할 도메인
                .allowedMethods("GET", "POST", "PUT", "DELETE")  // 허용할 HTTP 메소드
                .allowedHeaders("*")  // 모든 헤더를 허용
                .allowCredentials(true)  // 쿠키를 허용
                .maxAge(3600);  // 캐시 시간 설정
    }
}
```

1. 프론트에서 해결하는 방식

```jsx
export default defineConfig({
  plugins: [vue(), vueJsx()],
  resolve: {
    alias: {
      "@": fileURLToPath(new URL("./src", import.meta.url)),
    },
  },
  server: {
    proxy: {
      "/api": {
        target: "http://localhost:8080",
        rewrite: (path) => path.replace(/^\/api/, ""),
      },
    },
  },
});
```

- vue에서 cors관련 오류를 방지하기 위해 작성한 코드

일반적으로 백엔드와 프론트는 서로 다른 도메인에서 실행을 한다. 이때 브라우저에서 api서버로 직접 요청을 하면 CORS문제가 발생한다.

이를 방지하기 위해 프록시 서버를 사용해 개발 서버에서 API서버로 요청을 전달하는 방식을 사용한다.

- 코드 설명

→ 프록시 서버에 대한 설정 정보를 등록하는데 이 코드에서 /API경로로 들어오는 요청을 `http://localhost:8080`로 전달하는 프록시 서버 등록하고 이후에 rewrite 함수를 사용해서 요청 경로에 /api를 제거하게 설정

vue.ts

```jsx
const write = function(){
  axios.post("/api/posts",{
    title:title.value,
    content:content.value
  })
      .then(()=>{
        router.replace({name:"home"});
      })
}
```

controller.java

```jsx
@PostMapping("/posts")
    @ResponseStatus(HttpStatus.CREATED)
    public void post(@RequestBody @Valid PostCreate PostCreate){
        System.out.println("PostCreate = " + PostCreate.getTitle());
        postService.write(PostCreate);
    }
```
