# 스프링 MVC 구현

스프링이 제공하는 컨트롤러는 어노테이션 기반으로 동작해서, 매우 유연하고 실용적이다. 과거에는 자바 언어에 어노테이션이 없기도 했고, 스프링도 처음부터 이런 유연한 컨트롤러를 제공한 것은 아니다.

**@RequestMapping**

스프링은 어노테이션을 활용한 매우 유연하고, 실용적인 컨트롤러를 만들었는데, 이것이 바로 @RequestMapping 어노테이션을 사용하는 컨트롤러이다.

- RequestMappingHandlerMapping
- RequestMappingHandlerAdapter

앞서 보았듯이 가장 우선순위가 높은 핸들러 매핑과 핸들러 어댑터는 RequestMappingHandlerMapping, RequestMappingHandlerAdapter이다.

@RequestMapping 의 앞 글자를 따서 만든 이름인데, 이것이 바로 지금 스프링에서 주로 사용하는 어노테이션 기반의 컨트롤러를 지우너하는 핸들러 매핑 어댑터이다.

지금까지 만들었던 프레임워크에서 사용했던 컨트롤러를 @RequestMapping  기반의 스프링 MVC 컨트롤러로 변경해보겠다.

### **SpringMemberFormControllerV1 - 회원 등록 폼**

```java
package hello.servlet.web.springmvc.v1;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class SpringMemberFormControllerV1 {

    @RequestMapping("/springmvc/v1/members/new-form")
    public ModelAndView process() {
        return new ModelAndView("new-form");
    }
}
```

- `@Controller`
    - 스프링이 자동으로 스프링 빈으로 등록한다.
        - 내부에 @Component 어노테이션이 있어서 컴포넌트 스캔의 대상이 된다.
    - 스프링 MVC에서 어노테이션 기반 컨트롤러로 인식한다.
- `@RequestMapping`
    - 요청 정보를 매핑한다. 해당 URL이 호출되면 이 메서드가 호출된다. 어노테이션을 기반으로 동작하기 때문에 메서드의 이름은 임의로 지으면 된다.
- **ModelAndView**
    - 모델과 뷰 정보를 담아서 반환하면 된다.

RequestMappingHandlerMapping은 스프링 빈 중에서 @RequestMappin 또는 @Controller가 클래스 레벨에 붙어 있는 경우에 매핑 정보로 인식한다.

따라서 아래 코드도 동일하게 동작한다.

```java
@Component //컴포넌트 스캔을 통해 스프링 빈으로 등록 @RequestMapping
public class SpringMemberFormControllerV1 {
     @RequestMapping("/springmvc/v1/members/new-form")
     public ModelAndView process() {
         return new ModelAndView("new-form");
     }
}
```

물론 컴포넌트 스캔 없이 아래와 같이 스프링 빈으로 직접 등록해도 된다.

```java
@RequestMapping
 public class SpringMemberFormControllerV1 {
     @RequestMapping("/springmvc/v1/members/new-form")
     public ModelAndView process() {
         return new ModelAndView("new-form");
     }
}
```

참고로 스프링 부트 3.0 이상 부터는 클래스 레벨에 `@RequestMapping` 이 있어도 스프링 컨트롤러로 인식하지 않는다. 오직 `@Controller`가 있어야 스프링 컨트롤러로 인식한다. 참고로 `@RestController` 는 해당 어노테이션 내부에 `@Controller`를 포함하고 있으므로 인식이 된다.

따라서 `@Controller`가 없는 위의 두 코드는 스프링 컨트롤러로 인식되지 않는다. RequestMappingHandlerMapping에서 `@RequestMapping`는 이제 인식하지 않고, Controller 만 인식한다.

**ServletApplication**

```java
//스프링 빈 직접 등록
@Bean
SpringMemberFormControllerV1 springMemberFormControllerV1() {
     return new SpringMemberFormControllerV1();
 }
```

### **SpringMemberSaveControllerV1 - 회원 저장**

```java
package hello.servlet.web.springmvc.v1;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class SpringMemberSaveControllerV1 {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @RequestMapping("/springmvc/v1/members/save")
    public ModelAndView process(HttpServletRequest request, HttpServletResponse response) {
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        ModelAndView mv = new ModelAndView("save-result");
        mv.addObject("member", member);
        return mv;
    }
}
```

- `mv.addObject("member", member)`
    - 스프링이 제공하는 ModlAndView를 통해 Model 데이터를 추가할 때는 addObject() 를 사용하면 된다. 이 데이터는 이후 뷰를 렌더링 할 때 사용된다.

### **SpringMemberListControllerV1 - 회원 목록**

```java
package hello.servlet.web.springmvc.v1;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import java.util.List;

@Controller
public class SpringMemberListControllerV1 {

    private final MemberRepository memberRepository = MemberRepository.getInstance();

    @RequestMapping("/springmvc/v1/members")
    public ModelAndView process() {
        List<Member> members = memberRepository.findAll();
        ModelAndView mv = new ModelAndView("members");
        mv.addObject("members", members);
        return mv;

    }
}
```

### 스프링 MVC - 컨트롤러 통합

@RequestMapping을 잘 보면 클래스 단위가 아니라 메서드 단위에 적용된 것을 확인할 수 있다. 따라서 컨트롤러 클래스를 유연하게 하나로 통합이 가능하다.

### **SpringMemberControllerV2**

```java
package hello.servlet.web.springmvc.v2;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import java.util.List;

@Controller
@RequestMapping("/springmvc/v2/members")
public class SpringMemberControllerV2 {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @RequestMapping("/new-form")
    public ModelAndView newForm() {
        return new ModelAndView("new-form");
    }

    @RequestMapping("/save")
    public ModelAndView save(HttpServletRequest request, HttpServletResponse response) {
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        ModelAndView mv = new ModelAndView("save-result");
        mv.addObject("member", member);
        return mv;
    }
    
    @RequestMapping
    public ModelAndView members() {
        List<Member> members = memberRepository.findAll();
        ModelAndView mv = new ModelAndView("members");
        mv.addObject("members", members);
        return mv;

    }
}
```

Controller는 클래스를 통합하는 것을 넘어서 조합도 가능하다.

```java
@RequestMapping("/springmvc/v2/members")
```

해당 코드를 클래스 레벨에 적용하면, 아래 메서드 레벨에 있는 `@RequestMapping()` 의 URL은 그 뒤로 붙어서 URL을 완성시킨다.

## 스프링 MVC - 실용적인 방식

MVC 프레임워크의 v3 버전은 ModelView를 개발자가 직접 생성해서 반환하기 때문에 불편하다.

스프링 MVC는 개발자가 편리하게 개발할 수 있도록 수 많은 기능을 제공한다.

### **SpringMemberControllerV3**

```java
package hello.servlet.web.springmvc.v3;

import hello.servlet.domain.member.Member;
import hello.servlet.domain.member.MemberRepository;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

import java.util.List;

@Controller
@RequestMapping("/springmvc/v3/members")
public class SpringMemberControllerV3 {
    private MemberRepository memberRepository = MemberRepository.getInstance();

    @GetMapping("/new-form")
    public ModelAndView newForm() {
        return new ModelAndView("new-form");
    }

    @PostMapping("/save")
    public String save(
            @RequestParam("username") String username,
            @RequestParam("age") int age,
            Model model) {

        Member member = new Member(username, age);
        memberRepository.save(member);

        model.addAttribute("member", member);
        return "save-result";
    }

    @GetMapping
    public String members(Model model) {
        List<Member> members = memberRepository.findAll();
        model.addAttribute("members", members);
        return "members";
    }
}
```

**Model 파라미터**

`save()`, `members()`를 보면 Model을 파라미터로 받는다. 스프링 MVC는 이러한 편의 기능을 제공하낟.

**ViewName 직접 반환**

뷰의 논리 이름을 반환할 수 있다.

**@RequestParam 사용**

스프링은 HTTP 요청 파라미터를 @RequestParam으로 받을 수 있다.

`@RequestParam**(”username”)`은 `request.getParameter(”username”)`과 거의 같은 코드라고 생각하면 된다.**

물론 GET 쿼리 파라미터, POST Form 방식을 모두 지원한다.

**@RequestMapping → @GetMapping, @PostMapping**

RequestMapping은 URL만 매칭하는 것이 아니라 HTTP Method도 함께 구분할 수 있다.

```java
@RequestMapping(value = "/new-form", method = RequestMethod.GET)
```

이것을  ****@GetMapping, @PostMapping으로 더 편리하게 사용할 수 있다.

참고로 Get, Post, Put, Delete, Patch 모두 어노테이션이 있다.

---

**참고 자료**

- [https://www.inflearn.com/course/스프링-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)
