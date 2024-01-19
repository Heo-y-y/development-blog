# **스프링 MVC 전체 구조**

앞에서 만든 MVC 프레임워크와 스프링 MVC를 비교해보자.

### **직접 만든 MVC 프레임워크 구조**

![스크린샷 2024-01-19 오후 3.55.37.png](https://github.com/Heo-y-y/development-blog/assets/112863029/7e1b927b-8a9f-4763-9a22-4d4bf1b18492)

### Spring MVC 구조

![스크린샷 2024-01-19 오후 3.55.58.png](https://github.com/Heo-y-y/development-blog/assets/112863029/d13f3ed1-4502-4434-8d98-95dffc3096f8)

이름만 다를 뿐이고, 구조가 똑같은 걸 볼 수 있다.

### 동작 순서

1. **핸들러 조회**: 핸들러 매핑을 통해 요청 URL에 매핑된 핸들러(컨트롤러)를 조회한다.
2. **핸들러 어댑터 조회**: 핸들러를 실행할 수 있는 핸들러 어댑터를 조회한다.
3. **핸들러 어댑터 실행**: 핸들러 어댑터를 실행한다.
4. **핸들러 실행**: 핸들러 어댑터가 실제 핸들러를 실행한다.
5. **ModelAndView 반환**: 핸들러 어댑터는 핸들러가 반환하는 정보를 ModelAndView로 변환해서 반환한다.
6. **viewResolver 호출**: 뷰 리졸버를 찾고 실행한다.
    - JSP의 경우: InternalResourceViewResolver가 자동 등록되고 사용된다.
7. **View 반환**: 뷰 리졸버는 뷰의 논리 이름을 물리 이름으로 바꾸고, 렌더링 역할을 담당하는 뷰 객체를 반환한다.
    - JSP의 경우: InternalResourceView(JstlView)를 반환하는데, 내부에 forward() 로직이 있다.
8. **뷰 렌더링**: 뷰를 통해서 뷰를 렌더링 한다.

### DispatcherServlet 구조 살펴보기

`org.springframework.web.servlet.DispatcherServlet`

스프링 MVC도 프론트 컨트롤러 패턴으로 구현되어 있다.

스프링 MVC의 프론트 컨트롤러가 바로 DispatcherServlet이다. 그리고 이 DispatcherServlet이 스프링 MVC의 핵심이다.

**DispatcherServlet 서블릿 등록**

- DispatcherServlet도 부모 클래스에서 HttpServlet을 상속 받아서 사용하고, 서블릿으로 동작한다.
    - DispatcherServlet → FrameworkServlet → HttpServletBean → HttpServlet
- 스프링 부트는 DispatcherServlet을 서블릿으로 자동으로 등록하면서 **모든 경로**(`urlPatterns=”/”`)에 대해서 매핑한다.
    - 참고로 더 자세한 경로가 우선순위가 높다. 그래서 기존에 등록한 서블릿도 함께 동작한다.

**요청 흐름**

- 서블릿이 호출되면 HttpServlet이 제공하는 `service()`가 호출된다.
- 스프링 MVC는 DispatcherServlet의 부모인 FrameworkServlet에서 `service()`를 오버라이드 해수었다.
- `FrameworkServlet.service()`를 시작으로 여러 메서드가 호출되면서 `DispatcherServlet.doDispatch()`가 호출된다.

지금부터 DispatcherServlet의 핵심인 `doDispatch()` 코드를 분석해보자.

`DispatcherServlet.doDispatch()`

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse
 response) throws Exception {
     HttpServletRequest processedRequest = request;
     HandlerExecutionChain mappedHandler = null;
     ModelAndView mv = null;
// 1. 핸들러 조회
mappedHandler = getHandler(processedRequest); if (mappedHandler == null) {
         noHandlerFound(processedRequest, response);
return; }
//2.핸들러 어댑터 조회-핸들러를 처리할 수 있는 어댑터
HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
// 3. 핸들러 어댑터 실행 -> 4. 핸들러 어댑터를 통해 핸들러 실행 -> 5. ModelAndView 반환 mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
     processDispatchResult(processedRequest, response, mappedHandler, mv,
 dispatchException);
}
 private void processDispatchResult(HttpServletRequest request,
 HttpServletResponse response, HandlerExecutionChain mappedHandler, ModelAndView
 mv, Exception exception) throws Exception {
// 뷰 렌더링 호출
render(mv, request, response);
 }
 protected void render(ModelAndView mv, HttpServletRequest request,
 HttpServletResponse response) throws Exception {
     View view;
String viewName = mv.getViewName(); //6. 뷰 리졸버를 통해서 뷰 찾기,7.View 반환
     view = resolveViewName(viewName, mv.getModelInternal(), locale, request);
// 8. 뷰 렌더링
     view.render(mv.getModelInternal(), request, response);
 }
```

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)
