# Swagger

## Swagger API

![스크린샷 2023-09-08 오후 10 40 57](https://github.com/Heo-y-y/development-blog/assets/112863029/87eaad88-ec6f-426a-8da4-b3b4cb5658e2)

## Swaggeer 구현

배포하기 전 프론트엔드 동료들과 좀 더 원활하게 소통하기 위해 Swagger로 API 문서를 만들어서 공유하기로 했다.

처음 적용해보는 거라 생소했지만, 생각보다 간단해서 개발 도구의 편리함을 느낄 수 있었다.

### dependencies 추가

```java
implementation 'io.springfox:springfox-swagger2:2.9.2'
implementation 'io.springfox:springfox-swagger-ui:2.9.2'
```

### application.yml 추가

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

### SwaggerConfig 클래스 구현(설정 담당)

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.BE.cocktail"))
                .paths(PathSelectors.any())
                .build()
                .apiInfo(apiInfo());
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("cocktail API")
                .description("API Documentation for cocktail")
                .version("1.0.0")
                .build();
    }
}
```

`@Configuration`을 사용하여 **빈을 주입하여 스프링 컨테이너에서 관리**하도록 한다.

`@EnableSwagger2` ****어노테이션은 **Swagger 2.0 버전을 활성화**하는 데 사용된다. 해당 어노테이션을 통해 **스프링 부트에서 Swagger를 사용**할 수 있게 된다.

`Docket`은 **Swagger API 문서를 생성하는데 사용되는 핵심 클래스**인데, `DocumentationType.SWAGGER_2`를 사용하여 **Swagger 2.0 스펙을 사용하도록 설정**했다.

그리고 `select()` 메서드로 **조건을 설정**하는데, 여기서 중요한게 `RequestHandlerSelectors.basePackage("com.BE.cocktail")` 이 부분이다. 해당 코드는 **특정 패키지에 있는 컨트롤러 클래스만 문서화하도록 지정**하는 것이다.

![스크린샷 2023-09-08 오후 10.54.19.png](https://github.com/Heo-y-y/development-blog/assets/112863029/4d51914a-2b19-4e9c-980d-105cde8876dd)

그림과 프로젝트에 적용한 코드를 보면 패키지 위치에 맞게 넣어준 것을 확인할 수 있다.

그리고 아래에 있는 `ApiInfo`는 **Swagger 문서에 표시되는 API에 대한 정보를 설정**하는데, 직관적인 코드 그대로 `title()`은 **문서 제목**이고 `description()`은 **문서에 대한 설명**이다.

### Swagger - Controller에 적용

코드가 중복이 많아서, 간단하게 정규 레시피 상세 조회 API 하나로 코드를 설명하겠다.

```java
@RestController
@RequiredArgsConstructor
@Api(tags = "regularRecipe", description = "정규 레시피 API")
@RequestMapping("/regular")
public class RegularRecipeController {
    private final RegularRecipeService regularRecipeService;

    @ApiOperation(value = "정규 레시피 상세 조회", notes = "정규 레시피 상세 조회 API")
    @ApiImplicitParam(name = "recipe_id", value = "정규 레시피 아이디")
    @GetMapping("/find/{recipe_id}")
    public ApiResponse<RegularRecipeGetResponseDto> findDetailInfo(@PathVariable("recipe_id") Long id) {
        return ApiResponse.ok(regularRecipeService.find(id));
    }
}
```

먼저 클래스 레벨에 `@Api(tags = "regularRecipe", description = "정규 레시피 API")`라고 등록을 해줬는데, `@Api()`는 **Swagger 프레임워크에서 제공**되는데, **API 문서에 대한 메타 데이터를 제공**한다.

![스크린샷 2023-09-08 오후 11.10.02.png](https://github.com/Heo-y-y/development-blog/assets/112863029/3cfb80fa-f8a3-40e5-8416-f9effbee55d2)

그림을 보면서 설명하면, `tags` 같은 경우는 🟥 색에 위치하는 곳에 보여지고, `description` 은 🟦 색에 위치하게 된다.

그럼 메서드 레벨에 설정한 어노테이션을 설명하겠다.

`@ApiOperation(value = "정규 레시피 상세 조회", notes = "정규 레시피 상세 조회 API")`를 먼저 보자.

`@ApiOperation` 는 **대상 API의 설명을 작성**하기 위한 어노테이션이다.

정규 레시피 API를 열면 보이는 부분인데 그림으로 설명하겠다.

![스크린샷 2023-09-08 오후 11.21.36.png](https://github.com/Heo-y-y/development-blog/assets/112863029/4dceddec-ade8-43b8-b8ea-cb5ab96dc797)

먼저 `value`에 작성한 것은 🟥 색에 위치하고, `notes`는  🟦 색에 보여진다.

다음은 `@ApiImplicitParam(name = "recipe_id", value = "정규 레시피 아이디")`에 대해 설명하겠다.

`@ApiImplicitParam`는 **매개 변수에 대한 세부 정보를 설명**하기 위해 사용한다.

![스크린샷 2023-09-08 오후 11.21.36.png](https://github.com/Heo-y-y/development-blog/assets/112863029/ee6d4827-571f-4242-8620-99facad03cda)

`name`은 🟥 색에 위치하고, `value`는  🟦 색에 보여진다.

다음은 Dto 클래스를 살펴보겠다.

### Swagger - Dto 적용

```java
@Getter
@AllArgsConstructor(access = AccessLevel.PRIVATE)
public class RegularRecipeGetResponseDto {
    @ApiModelProperty(example = "정규 레시피 아이디")
    private Long id;
    @ApiModelProperty(example = "칵테일 이름")
    private String name;
    @ApiModelProperty(example = "칵테일 재료")
    private String ingredient;
    @ApiModelProperty(example = "칵테일 설명")
    private String description;
    @ApiModelProperty(example = "칵테일 레시피")
    private String recipe;
    @ApiModelProperty(example = "칵테일 이미지 URL")
    private String imageUrl;
    @ApiModelProperty(example = "칵테일 찜 여부")
    private boolean wishList;
}
```

`@ApiModelProperty` 어노테이션을 사용하여 Swagger 문서화를 위한 정보를 제공하는데, 해당 어노테이션은 각 필드에 대한 설명과 예시를 제공해준다. 즉, **API 사용자에게 API 응답 객체의 의미를 설명**하는 것이다.

![스크린샷 2023-09-08 오후 11.30.39.png](https://github.com/Heo-y-y/development-blog/assets/112863029/c7cf2045-2578-4c21-8d9d-ffaf913f6812)

그림을 보면 바로 알 수 있다. API 문서에서 코드에 적용한 `exaple`그대로 **응답 객체를 설명**해준다.

### 통신 테스트

Swagger는 API 명세 관리 뿐만 아니라 직접 **통신도 기능도 제공**하여 알딸딸 프로젝트 진행 중에 편리하게 이용했다.

`dml.sql` 파일에 만들어 놓은 데이터로 **정규 레시피 전체 조회 API**를 통해 통신테스트를 해보겠다.

![스크린샷 2023-09-08 오후 11.49.27.png](https://github.com/Heo-y-y/development-blog/assets/112863029/55bf76ee-da0a-4aee-bf3b-d1370ef474e1)

`Tryitout` 버튼을 통해 입력 값을 넣고, 테스트를 할 수 있다.

![스크린샷 2023-09-08 오후 11.37.10.png](https://github.com/Heo-y-y/development-blog/assets/112863029/7fcbdbe2-8c2f-4776-9e12-4a2eef33a1a8)

(1)번을 통해 입력 값을 넣으면, (2)번에 데이터 값이 나오는 것을 확인할 수 있다.
