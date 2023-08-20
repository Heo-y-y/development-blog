# Swagger

## Swagger란?

**OpenAPI Specification(OAS)**를 위한 **프레임워크**이다. OpenAPI에서 빼 놓을 수 없는 기능이 바로 Swagger이다. API 에디터, 코드젠, API 메뉴얼 자동 생성 및 테스트 사이트 생성으로 유명하다.

우선 **OAS**가 무엇인지 알아보자.

- OpenAPI = Specification
- Swagger = Tools for implementing the specification

**OAS(OpenAPI Specification)**는 **RESTful 웹 서비스를 약속된 규칙에 따라 약속된 규칙에 맞게 API 스펙을 json과 yaml 형식으로 표현**한다. 

즉, **Swagger**는 API들이 가지고 있는 **Specification(스펙/명세 등)를 관리할 수 있는 프로젝트**이다.

**Swagger**를 이용하게 되면 **API 문서**를 개발자가 따로 작성할 필요 없이 코드만 작성해 주면 알아서 **Web Page를 만들어 문서를 보여주는 도구**인 것이다.

### Swagger의 기능

![스크린샷 2023-08-20 오후 9 50 19](https://github.com/Heo-y-y/development-blog/assets/112863029/2bfca895-9fe7-4ed9-a75d-6321e9fededf)

- **API 디자인**: Swagger-editor를 통해 API를 문서화하고 빠르게 명세할 수 있다.
- **API Development**: Swagger-codegen을 통해 작성된 문설를 SDK를 생성하여 빌드 프로세스를 간소화 할 수 있다.
- **API Documentation**: Swagger-UI를 통해 작성된 API를 시각화해준다.
- **API Testing**: Swagger-Insepctor를 통해 API를 시각화하고 빠른 테스팅을 진행할 수 있다.
- **Standardize**: Swagger-hub를 통해 개인, 팀원들이 API 정보를 공유하는 Hub이다.

아래 그림은 **Swagger 어노테이션**이다.

![Untitled](https://github.com/Heo-y-y/development-blog/assets/112863029/7c624ebb-4b3b-47bc-9d72-8b2a5962e098)

### Swagger 사용법

build.gradle에 아래 코드를 추가한다.

```java
implementation 'io.springfox:springfox-swagger2:2.9.2'
implementation 'io.springfox:springfox-swagger-ui:2.9.2'
```

그리고 application.yml 파일에 아래 코드를 추가하고,

```java
spring:
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher

  springfox:
    documentation:
      swagger-ui:
        host: localhost:8080
```

SwaggerConfig 클래스를 만들어 주고 설정해준다.

```java
package com.wanted.backend.global.config.swagger;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.wanted.backend"))
                .paths(PathSelectors.any())
                .build()
                .apiInfo(apiInfo());
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Wanted API")
                .description("Wanted API 문서입니다.")
                .version("1.0.0")
                .build();
    }
}
```

위 코드에서 `basePackage()`에는 **어떤 패키지를 스캔해야 하는지를 지정**하는 것이다. 그러면 해당 패키지 내에 있는 `@RestController` 또는 `@Controller` 어노테이션을 가진 클래스들을 검색하고, 이 클래스들의 메서드가 **API 엔드포인트를 정의**한다.

컨트롤러 클래스에 `@Api()`와 `@ApiOperation()`를 넣어줘서 설정해준다.

```java
@RequiredArgsConstructor
@RestController
@Api(tags = "postController")
@RequestMapping("/api")
public class PostController {
    private final PostService postService;

    @ApiOperation(value = "게시글 등록")
    @PostMapping("/post")
    public ResponseEntity<ApiResponse<PostCreateDto>> createPost(@Valid @RequestBody PostCreateDto postCreateDto, @AuthenticationPrincipal UserDetails userDetails) {
        postService.createPost(postCreateDto, userDetails.getUsername());
        return ResponseEntity.status(HttpStatus.CREATED).body(ApiResponse.createPost(postCreateDto));
    }
```

`@Api` 어노테이션은 `tags` 매개변수를 사용하여 관련된 **엔드포인트를 특정 태그로 그룹화하고 분류**할 수 있다.

`@ApiOperation` 어노테이션은 특정 API 작업(엔드포인트)에 대한 자세한 정보를 제공하는데 사용된다. 즉 매개변수 `value`를 사용하여 이 앤드포인트가 “**게시글 등록**” 에 사용된다는 것을 나타내는 것이다.

그리고 애플리케이션을 실행 시킨 뒤 http://localhost:8080/swagger-ui.html 로 들어가면 아래 그림들 처럼 API 문서가 나오는 것을 확인할 수 있다.

![스크린샷 2023-08-20 오후 10 25 03](https://github.com/Heo-y-y/development-blog/assets/112863029/87ed2cea-e60c-4140-8937-bed58657492e)

![스크린샷 2023-08-20 오후 10 25 49](https://github.com/Heo-y-y/development-blog/assets/112863029/47550bd4-b7fe-4a37-a977-9ee52656715a)

![스크린샷 2023-08-20 오후 10 31 52](https://github.com/Heo-y-y/development-blog/assets/112863029/9271b0eb-da40-4392-b17a-a290d43d9366)

혹시나 **Security**를 사용 중이라면 **SecurityConfig 클래스에 들어가 권한 허용**을 해준다.

```java
.antMatchers(
    "/h2-console/**",
    "/favicon.ico",
    "/v2/api-docs",
    "/swagger-resources/**",
    "/swagger-ui.html",
    "/webjars/**",
    "/swagger/**"
).permitAll()
```

그리고 swagger-ui.html으로 접속해야 할 지 swagger-ui/index.html으로 접속해야 할 지 버전에 따라 다르기 때문에 swagger-ui 404 에러가 종종 발생한다.

만약 필자처럼 **2.9.2** 버전을 사용한다면 위에 링크로도 에러없이 Swagger 페이지를 띄울 수 있을 것이다.

**참고 자료**

- <https://junyharang.tistory.com/273>
- <https://etloveguitar.tistory.com/58>
- [https://velog.io/@stpn94/Swagger-란-무엇인고](https://velog.io/@stpn94/Swagger-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B3%A0)
