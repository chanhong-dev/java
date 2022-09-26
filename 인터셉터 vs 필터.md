- 자바, 스프링 웹 개발을 하다보면, **공통적으로 처리해야할 로직**들이 존재한다.
- 로그인 관련 처리, 권한 체크, XSS 방어, 접속 OS처리, 로그 등..
- 공통된 부분은 따로 빼서 구현 및 관리 하는 것이 유리하다.

### 공통처리를 위해 활용 가능한 3가지

1. Filter
2. Interceptor
3. AOP

## Spring에서의 흐름

![](https://velog.velcdn.com/images/chanhong-dev/post/c7c0f9c2-6610-471d-9579-ca0801feb570/image.png)


- Interceptor와 Filter는 `Servlet 단위`에서 실행
- AOP는 메소드 앞에 `Proxy패턴의 형태`로 실행
- Filter → Interceptor → AOP → Interceptor → Filter 순

## Filter(필터)

- J2EE 표준 스펙 기능
- 디스패처 서블릿(Dispatcher Servlet)에 요청이 전달 되기 전/후에 패턴에 맞는 모든 요청에 대해 부가작업을 처리할 수 있는 기능을 제공
- 웹 컨테이너에서 동작

![](https://velog.velcdn.com/images/chanhong-dev/post/d2b04ace-815e-4849-b057-caf910eadd83/image.png)



### Filter의 메소드

- `init()` : 필터 인스턴스 초기화
- `doFilter()` : 전/후 처리
- `destroy()` : 필터 인스턴스 종료

```java
public interface Filter {

    public default void init(FilterConfig filterConfig) throws ServletException {}

    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException;

    public default void destroy() {}
}
```

## 필터의 용도

- 공통된 보안 및 인증/인가 관련작업
- 모든 요청에 대한 로깅 또는 감사
- 이미지/데이터 압축 및 문자열 인코딩
- Spring과 분리 되어야 하는 기능

---

## Interceptor(인터셉터)

- Spring에서 제공하는 기술
- 디스패처 서블릿(Dispatcher Servlet)이 컨트롤러를 호출하기 전과 후에 요청과 응답을 참조하거나 가공할 수 있는 기능
- 스프링 컨텍스트에서 동작
- 핸들러 매핑의 결과로 실행체인(HandlerExecutionChain)이 나옴.
→ 인터셉터가 등록되어 있다면 순차적으로 인터셉터들을 거쳐 컨트롤러가 실행되도록 함.

![](https://velog.velcdn.com/images/chanhong-dev/post/8c1fcd12-ef12-44b9-b308-fc7d4410e78a/image.png)


### 인터셉터의 메소드

- `preHandler()` : 컨트롤러 메소드가 실행되기 전
- `postHandler()` : 컨트롤러 메소드 실행 직 후 / view 페이지 렌더링 전
- `afterCompletion()` : view 페이지 렌더링 후

```java
public interface HandlerInterceptor {

    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
        throws Exception {
        
        return true;
    }

    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
        @Nullable ModelAndView modelAndView) throws Exception {
    }

    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
        @Nullable Exception ex) throws Exception {
    }
}
```

## 인터셉터의 용도

- 세부적인 보안 및 인증/인가 공통 작업
- API 호출에 대한 로깅 또는 감사
- Controller로 넘겨주는 정보 가공

---

## 필터 VS 인터셉터

![](https://velog.velcdn.com/images/chanhong-dev/post/8657e8ce-5287-485f-ae52-e76ad7a39689/image.png)


### Request/Response 조작 여부

- 필터는 가능하지만, 인터셉터는 불가능
- 조작이란 내부 상태를 변경하는 것이 아니라 다른 객체로 바꿔지기가 가능하다는 것

**필터**

```java
public MyFilter implements Filter {

    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
        // 개발자가 다른 request와 response를 넣어줄 수 있음
        chain.doFilter(request, response);       
    }
    
}
```

**인터셉터**

```java
public class MyInterceptor implements HandlerInterceptor {

    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        // Request/Response를 교체할 수 없고 boolean 값만 반환할 수 있다.
        return true;
    }

}
```

## Reference

[https://mangkyu.tistory.com/173](https://mangkyu.tistory.com/173)
