# 서블릿, JSP, MVC 패턴

## 회원 관리 웹 애플리케이션 요구사항

**회원 정보**

- 이름: username
- 나이: age

**기능 요구사항**

- 회원 저장
- 회원 목록 조회

## **서블릿으로 회원 관리 웹 애플리케이션 만들기**

**회원 등록 폼**

```java
package hello.servlet.web.servlet;

import hello.servlet.domain.member.MemberRepository;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(name = "memberFormServlet", urlPatterns = "/servlet/members/new-form")
public class MemberFormServlet extends HttpServlet {
    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");

        PrintWriter w = response.getWriter();
        w.write("<!DOCTYPE html>\n" +
                "<html>\n" +
                "<head>\n" +
                "    <meta charset=\"UTF-8\">\n" +
                "    <title>Title</title>\n" +
                "</head>\n" +
                "<body>\n" +
                "<form action=\"/servlet/members/save\" method=\"post\">\n" +
                "    username: <input type=\"text\" name=\"username\" />\n" +
                "    age:      <input type=\"text\" name=\"age\" />\n" +
                "    <button type=\"submit\">전송</button>\n" +
                "</form>\n" +
                "</body>\n" +
                "</html>\n");
    }
}
```

`MemberFormServlet` 같은 경우는 단순하게 회원 정보를 입력할 수 있는 HTML Form을 만들어서 응답한다.

자바 코드로 직접 HTML을 구현하려고 하니 상당히 힘들다….

그러면 위 회원 등록 폼에서 실제로 데이터를 입력하고 전송을 누를 경우 회원 데이터가 잘 저장되는지 회원 저장을 만들어서 확인해보자.

**회원 저장**

```java
package hello.servlet.web.servlet;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(name = "memberSaveServlet", urlPatterns = "/servlet/members/save")
public class MemberSaveServlet extends HttpServlet {
    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("MemberSaveServlet.service");
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");
        PrintWriter w = response.getWriter();
        w.write("<html>\n" +
                "<head>\n" +
                " <meta charset=\"UTF-8\">\n" + "</head>\n" +
                "<body>\n" +
                "성공\n" +
                "<ul>\n" +
                "    <li>id="+member.getId()+"</li>\n" +
                "    <li>username="+member.getUsername()+"</li>\n" +
                " <li>age="+member.getAge()+"</li>\n" + "</ul>\n" +
                "<a href=\"/index.html\">메인</a>\n" + "</body>\n" +
                "</html>");
    }
}
```

![스크린샷1 2024-01-02 오후 10 00 23](https://github.com/Heo-y-y/development-blog/assets/112863029/9f589d76-5712-494a-a127-345bc352fe8f)

위 그림처럼 입력한 데이터 값이 아래 그림을 보면 정상적으로 저장된 것을 확인할 수 있다.

![스크린샷2 2024-01-02 오후 10 00 32](https://github.com/Heo-y-y/development-blog/assets/112863029/47c13a92-7dc0-4e29-81a5-9c3d8371973f)

`MemberSaveServlet`의 순서를 정리하면 아래와 같다.

1. 파라미터를 조회해서 `Member` 객체를 만든다.
2. `Member` 객체를 `MemberRepository`를 통해서 저장한다.
3. `Member` 객체를 사용해서 결과 화면용 HTML을 동적으로 만들어서 응답한다.

**회원 목록**

```java
package hello.servlet.web.servlet;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.List;

@WebServlet(name = "memberListServlet", urlPatterns = "/servlet/members")
public class MemberListServlet extends HttpServlet {
    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        List<Member> members = memberRepository.findAll();

        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");

        PrintWriter w = response.getWriter();
        w.write("<html>");
        w.write("<head>");
        w.write("    <meta charset=\"UTF-8\">");
        w.write("    <title>Title</title>");
        w.write("</head>");
        w.write("<body>");
        w.write("<a href=\"/index.html\">메인</a>"); w.write("<table>");
        w.write(" <thead>");
        w.write(" <th>id</th>");
        w.write(" <th>username</th>"); w.write(" <th>age</th>");
        w.write(" </thead>");
        w.write(" <tbody>");
        for (Member member : members) {
            w.write("    <tr>");
            w.write("        <td>"+member.getId()+"</td>");
            w.write("        <td>"+member.getUsername()+"</td>");
            w.write("        <td>"+member.getAge()+"</td>");
            w.write("    </tr>");

        }
        w.write("    </tbody>");
        w.write("</table>");
        w.write("</body>");
        w.write("</html>");

    }
}
```

회원 목록 리스트도 저장하고 해당 url을 타고 들어가면 아래 그림처럼 정상적으로 출력되는 것을 확인할 수 있다.

![스크린샷3 2024-01-02 오후 10 09 23](https://github.com/Heo-y-y/development-blog/assets/112863029/0148e388-8c87-4d4c-a58e-a69ebad4931c)

지금까지는 서블릿과 자바 코드만으로 HTML을 만들어봤다. 서블릿 덕분에 동적으로 원하는 HTML을 만들 수는 있지만, 상당히 불편하다는 것을 알 수 있다.

차라리 HTML 문서에 동적으로 변경해야 하는 부분만 자바 코드를 넣는 것이 편리해보인다. 이것이 바로 템플릿이 나온 이유인데, 템플릿 엔진을 사용하면 HTML 문서에서 필요한 곳만 코드로 적용해서 동적으로 변경할 수 있다.

템플릿 엔진에는 JSP, Thymeleaf, Freemarker, Velocity 등이 있다.

## JSP**로 회원 관리 웹 애플리케이션 만들기**

### JSP 라이브러리 추가

**스프링 부트 3.0 이상**

```java
//JSP 추가 시작
implementation 'org.apache.tomcat.embed:tomcat-embed-jasper'
implementation 'jakarta.servlet:jakarta.servlet-api'
implementation 'jakarta.servlet.jsp.jstl:jakarta.servlet.jsp.jstl-api'
implementation 'org.glassfish.web:jakarta.servlet.jsp.jstl'
```

**회원 등록 폼**

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
 <html>
 <head>
     <title>Title</title>
 </head>
<body>
<form action="/jsp/members/save.jsp" method="post">
    username: <input type="text" name="username" />
    age: <input type="text" name="age" />
    <button type="submit">전송</button>
 </form>
 </body>
 </html>
```

**회원 저장**

```java
<%@ page import="hello.servlet.domain.member.MemberRepository" %>
<%@ page import="hello.servlet.domain.member.Member" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    // request, response 사용 가능
    MemberRepository memberRepository = MemberRepository.getInstance();

    System.out.println("MemberSaveServlet.service");
    String username = request.getParameter("username");
    int age = Integer.parseInt(request.getParameter("age"));

    Member member = new Member(username, age);
    memberRepository.save(member);
%>
<html>
<head>
    <meta charset="UTF-8">
</head>
<body>
성공
<ul>
     <li>id=<%=member.getId()%></li>
     <li>username=<%=member.getUsername()%></li>
     <li>age=<%=member.getAge()%></li>
</ul>
<a href="/index.html">메인</a>
</body>
</html>
```

JSP는 자바 코드를 그대로 다 사용할 수 있다.

```java
<%@ page import="hello.servlet.domain.member.MemberRepository" %>
```

위 코드 같은 경우는 자바의 `import` 문과 같다.

```java
<% 자바코드 %>
```

그리고 위 코드처럼 해당 코드 안에는 자바 코드를 입력할 수 있다.

```java
<%= 자바코드 %>
```

그리고 `=` 이 들어간 코드에 자바 코드가 들어가면 출력할 수 있다.

여기서 서블리과의 차이점이 딱 보인다. JSP는 HTML을 중심으로 자바 코드를 부분부분 입력해주고, HTML 중간에 자바 코드를 출력한다.

**회원 목록**

```java
<%@ page import="java.util.List" %>
<%@ page import="hello.servlet.domain.member.MemberRepository" %>
<%@ page import="hello.servlet.domain.member.Member" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
     MemberRepository memberRepository = MemberRepository.getInstance();
     List<Member> members = memberRepository.findAll();
%>
 <html>
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
 <body>
 <a href="/index.html">메인</a> <table>
 <thead>
 <th>id</th>
     <th>username</th>
     <th>age</th>
     </thead>
 <tbody>
<%
     for (Member member : members) {
         out.write("    <tr>");
         out.write("        <td>" + member.getId() + "</td>");
         out.write("        <td>" + member.getUsername() + "</td>");
         out.write("        <td>" + member.getAge() + "</td>");
         out.write("    </tr>");
     }
%>
      </tbody>
  </table>
  </body>
  </html>
```

회원 레파지토리를 먼저 조회하고, 결과 `List`를 사용해서 중간에 `<tr><td>` HTML 태그를 반복해서 출력한다.

### 서블릿과 JSP의 한계

![스크린샷 2024-01-02 오후 10.57.15.png](https://github.com/Heo-y-y/development-blog/assets/112863029/7829334e-b338-4118-bba8-d53a5af03a72)

서블릿으로 개발할 때는 View 화면을 위한 HTML을 만드는 작업이 자바 코드에 섞여서 지저분하고 복잡하다. JSP를 사용한 덕분에 View를 생성하는 HTML 작업을 깔끔하게 가져가고, 중간중간 동적으로 변경이 필요한 부분만 자바 코드를 적용한다.

하지만 JSP 같은 경우 상위 절반은 비즈니스 로직이 들어가고, 나머지 하위 절반만 결과를 보여주는 HTML 즉 View의 영역이다. 지금은 간단한 회원 정보만 뽑는 로직이여서 편해보이지만 복잡한 로직과 규모가 커지면 유지보수도 힘들고 코드가 엄청 복잡하고 보기 힘들 것이다…

이 문제를 해결하기 위해 **MVC 패턴**이 등장했다.

비즈니스 로직은 서블릿 처럼 다른 곳에서 처리하고, JSP는 목적에 맞게 HTML로 화면(View)을 그리는 일에 집중하도록 하는 패턴이다.

## MVC 패턴

**너무 많은 역할**

하나의 서블릿이나 JSP만으로 비즈니스 로직과 뷰 렌더링까지 모두 처리하게 되면, 너무 많은 역할을 담당하게 되고, 결과적으로 유지보수가 어려워진다.

**변경의 라이프 사이클**

둘 사이의 라이프 사이클이 다르다. 예를 들어 UI를 일부 수정하는 일과 비즈니스 로직을 수정하는 일은 각각 다르게 발생하고, 서로에게 영향을 주지 않는다. 이렇게 다른 부분을 하나의 코드로 관리하는 것은 유지보수에 좋지 않다.

**기능 특화**

JPS 같은 뷰 템플릿은 화면을 렌더링 하는데 최적화 되어 있기 때문에 이 부분의 업무만 담당하는 것이 효과적이다.

**Model View Controller**

MVC 패턴은 서블릿이나 JSP로 처리하던 것을 Controller와 View라는 영역으로 서로 역할을 나눈 것을 말한다. 웹 애플리케이션은 보통 이 MVC를 사용한다.

![스크린샷 2024-01-02 오후 10.57.57.png](https://github.com/Heo-y-y/development-blog/assets/112863029/781295a7-a8f0-4a29-a2cd-ca8ad1b478da)

- **컨트롤러**
    - HTTP 요청을 받아서 파라미터를 검증하고, 비즈니스 로직을 실행한다. 그리고 뷰에 전달할 결과 데이터를 조회해서 모델에 담는다.
- **모델**
    - 뷰에 출력할 데이터를 담아둔다. 뷰가 필요한 데이터를 모두 모델에 담아서 전달해주는 덕분에 뷰는 비즈니스 로직이나 데이터 접근을 몰라도 되고, 화면을 렌더링 하는 일에 집중할 수 있다.
- **뷰**
    - 모델에 담겨있는 데이터를 사용해서 화면을 그리는 일에 집중한다. 여기서는 HTML을 생성하는 부분을 말한다.

## MVC 패턴 - 적용

그러면 서블릿을 컨트롤러로 사용하고, JSP를 뷰로 사용해서 MVC 패턴을 적용해보자.

모델은 `HttpServletRequest` 객체를 사용한다. `request`는 내부에 데이터 저장소를 가지고 있는데, `request.setAttribute()` , `request.getAttribute()` 를 사용하면 데이터를 보관하고, 조회할 수 있다.

### **회원 등록**

**회원 등록 폼 - 컨트롤러**

```java
package hello.servlet.web.servletmvc;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

@WebServlet(name = "mvcMemberFormServlet", urlPatterns = "/servlet-mvc/members/new-form")
public class MvcMemberFormServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String viewPath = "/WEB-INF/views/new-form.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }
}
```

`dispatcher.forward()`는 다른 서블릿이나 JSP로 이동할 수 있는 기능이다. 서버 내부에서 다시 호출이 발생한다.

 `/WEB-INF`

![스크린샷 2024-01-02 오후 11.14.13.png](https://github.com/Heo-y-y/development-blog/assets/112863029/35873dba-d4a3-46f4-b9ec-a764369c51f0)

이 경로안에 JSP가 있으면 외부에서 직접 JSP를 호출할 수 없다. 우리가 기대하는 것은 항상 컨트롤러를 통해서
JSP를 호출하는 것이다.

**redirect vs forward**

redirect는 실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect 경로로 다시 요청한다. 따라서 클라이언트가 인지할 수 있고, URL 경로도 실제로 변경된다.

반면에 forward는 서버 내부에서 일어나는 호출이기 때문에 클라이언트가 전혀 인지하지 못한다.

**회원 등록 폼 - 뷰**

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
 <html>
 <head>
     <meta charset="UTF-8">
     <title>Title</title>
 </head>
<body>
<!-- 상대경로 사용, [현재 URL이 속한 계층 경로 + /save] -->
<form action="save" method="post">
username: <input type="text" name="username" />
age: <input type="text" name="age" />
<button type="submit">전송</button>
 </form>
 </body>
</html>
```

여기서 `form`의 `action`을 보면 절대경로 `/` 로 시작이 아니라 상대경로인 것을 확인할 수 있다. 이렇게 상대경로를 사용하면 `form` 전송시 현재 URL이 속한 계층 경로 `+ save`가 호출된다.

### 회원 저장

**회원 저장 - 컨트롤러**

```java
package hello.servlet.web.servletmvc;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;
import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

@WebServlet(name = "mvcMemberSaveServlet", urlPatterns = "/servlet-mvc/members/save")
public class MvcMemberSaveServlet extends HttpServlet {

   private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        // 모델에 데이터를 보관
        request.setAttribute("member", member);

        String viewPath = "/WEB-INF/views/save-result.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }
}
```

`HttpServletRequest`를 모델로 사용한다. `request`가 제공하는 `setAttribute()`를 사용하면 `request` 객체에 데이터를 보관해서 뷰에 전달할 수 있다. 뷰는 `request.getAttribute()`를 사용해서 데이터를 꺼내면 된다.

**회원 저장 - 뷰**

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <meta charset="UTF-8">
</head>
<body>
성공
<ul>
     <li>id=${member.id}</li>
     <li>username=${member.username}</li>
     <li>age=${member.age}</li>
</ul>
<a href="/index.html">메인</a>
</body>
</html>
```

`<%= request.getAttribute("member")%>` 로 모델에 저장한 `member` 객체를 꺼낼 수 있지만, 너무 복잡해진다.

JSP에서는 `${}` 문법을 제공하는데, 이 문법을 사용하면 `request`의 `attribute`에 담긴 데이터를 편리하게 조회할 수 있다.

### **회원 목록 조회**

**회원 목록 조회 - 컨트롤러**

```java
package hello.servlet.web.servletmvc;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;
import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.util.List;

@WebServlet(name = "mvcMemberListServlet", urlPatterns = "/servlet-mvc/members")
public class MvcMemberListServlet extends HttpServlet {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        List<Member> members = memberRepository.findAll();

        request.setAttribute("members", members);

        String viewPath = "/WEB-INF/views/members.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }
}
```

**회원 목록 조회 - 뷰**

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
 <html>
 <head>
     <meta charset="UTF-8">
     <title>Title</title>
 </head>
<body>
<a href="/index.html">메인</a> <table>
     <thead>
     <th>id</th>
     <th>username</th>
     <th>age</th>
     </thead>
     <tbody>
     <c:forEach var="item" items="${members}">
         <tr>
             <td>${item.id}</td>
             <td>${item.username}</td>
             <td>${item.age}</td>
         </tr>
     </c:forEach>
     </tbody>
 </table>
 </body>
 </html>
```

모델에 담아둔 `members`를 JSP가 제공하는 `taglib`기능을 사용해서 반복하면서 출력했다.
`members` 리스트에서 `member` 를 순서대로 꺼내서 `item` 변수에 담고, 출력하는 과정을 반복한다.

`<c:forEach>` 이 기능을 사용하려면 다음과 같이 선언해야 한다.

```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

```

## MVC 패턴 - 한계

MVC 패턴을 적용해서 컨트롤러의 역할과 뷰를 렌더링하는 역할을 명확하게 구분해봤다.

특히 뷰는 화면을 그리는 역할에만 맡아서 코드가 깔끔하고 직관적이다.

하지만 계속해서 컨트롤러에서 반복적인 코드가 생기고 불필요한 코드들도 보인다.

### **MVC 컨트롤러의 단점**

**포워드 중복**

뷰로 이동하는 코드가 항상 중복 호출되어야 한다.

```java
RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
dispatcher.forward(request, response);
```

**ViewPath 중복**

```java
String viewPath = "/WEB-INF/views/new-form.jsp";
```

그리고 만약 JSP가 아닌 Thymeleaf 같은 다른 뷰로 변경한다면 전체 코드를 다 변경해야 한다.

**사용하지 않는 코드**

```java
HttpServletRequest request, HttpServletResponse response
```

위 코드를 사용할 때도 있고, 사용하지 않는 경우도 있다. 특히 `response`는 현재 사용하지 않는다.

그리고 이러한 `HttpServletRequest`, `HttpServletResponse`을 사용하는 코드는 테스트 케이스를 작성하기도 어렵다.

**공통 처리가 어렵다.**

기능이 복잡해지면 컨트롤러에서 공통으로 처리해야 하는 부분이 점점 더 늘어난다. 단순히 공통 기능을 메서드로 뽑으면 될 것 같지만, 결과적으로 해당 메서드를 항상 호출해야 하고, 실수로 누락한 경우 호출되지 않는 문제가 생길 것이다.

즉, 정리하면 공통 처리가 어렵다는 문제가 있다. 이 문제를 해결하려면 **컨트롤러 호출 전에 먼저 공통 기능을 처리**해야할 필요가 있어 보인다. **프론트 컨트롤러(Front Controller)패턴**을 도입하면 이러한 문제를 해결 할 수 있다고 한다.

**스프링 MVC의 핵심**도 바로 이 **프론트 컨트롤러**에 있다.

---

**참고 자료**

- <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard>
