# Filter와 Interceptor 차이 및 장단점

## 1. 공통 관심사를 분리하기 위해 사용된다.

공통 관심사는 비즈니스 로직으로부터 분리되어야한다. 인증 및 인가, 로깅, 인코딩 등 어플리케이션의 다양안 로직에 걸쳐 공통으로 적용되는 로직을 공통 관심사라고 한다. Spring AOP를 사용할 수도 있겠지만 WEB과 관련된 관심사를 분리하는데는 Filter나 Interceptor를 사용한는 것이 좋아보인다.

왜냐하면, 파라미터로 ServletRequest, ServletResponse를 제공해주기 때문에 URL 정보나 HTTP Header를 직접 조작할 수 있다는 장점이 있기 때문이다.

### Filter란?

Filter는 J2EE 표준 스펙으로 Servlet API 2.3 부터 등장하였고

Dispatcher Servlet에 요청이 전달되기 전,후에 부가작업을 처리하는 객체이다.

### 서블릿 필터가 제공하는 메서드

- init
- doFilter
  - 필수적으로 Override 해줘야 한다.
- destroy

### Spring Interceptor란?

인터셉터는 Spring이 제공하는 기술로,

디스패처 서블릿이 컨트롤러를 호울하기 전/후 요청에 대해 부가적인 작업을 처리하는 객체이다.

### Spring Interceptor 인터페이스에서 제공하는 메서드

- preHandle
  - Handler가 실행되기 전에 실행되는 메서드
  - 비즈니스 로직에서 또는 별개로 공동 관심사를 처리하는 로직
- postHandle
  - Handler가 실행된 이후에 실행되는 메서드
  - 파라미터로 ModelAndView를 받기 때문에 ModelAndView에 대한 추가작업
- afterCompletion
  - Handler가 실행된 이후에 실행되는 메서드
  - 파라미터로 Exception를 받기 때문에 예외를 처리할 수 있고, 리소스를 정리하는데 사용
