## Spring Data JPA란?

- **공통 인터페이스(CRUD)를 제공**하고, Repository를 개발할 때 인터페이스만 작성하면 **실행 시점에 스프링 데이터 JPA가 구현 객체를 동적으로 생성해 주입**해준다.
- 일반적인 **CRUD 메서드는 JPA Repository 인터페이스가 공통으로 제공**하고, **특정 메서드들은 스프링 데이터 JPA가 메서드를 분석해 JPQL로 변환**한다.
- **스프링 데이터 JPA**는 **스프링 프레임워크에서 JPA를 편리하게 사용할 수 있게 지원하는 프로젝트**이다.

![스크린샷1 2023-11-07 오후 5 24 50](https://github.com/mo2-Study-Group/StudyGroup/assets/112863029/754cfd5f-91cb-4299-8702-bd04042685ba)

위 그림을 보면 Spring Data JPA는 Spring 데이터 프로젝트의 하위 프로젝트이다.

### 생성

우리는 JPA를 사용할 Repository에 JPA Repository를 상속해서 인터페이스로 정의하면 끝인데, 아래 그림처럼 인터페이스만 만들어 놓으면 **스프링 데이터 JPA가 자동으로 실행 시점에 구현 클래스를 만들어 주입**해준다.

![스크린샷2 2023-11-07 오후 5 26 52](https://github.com/mo2-Study-Group/StudyGroup/assets/112863029/b45424a5-9768-4d68-91a3-f89fb387cad9)

## 공통 인터페이스 기능

Spring Data JPA를 사용하는 제일 쉬운 방법은 JPA Repository를 상속 받아서 사용하는 것이다.

```java
public interface MemberRepository extends JpaRepository<Member,Long>{}
```

이런식으로 제네릭 타입은 `<엔티티, 식별자>` 으로 설정하고, 주요 메서드는 아래와 같다.

- `sava()`: 새로운 엔티티를 저장, 존재하는 엔티티는 병합
- `delete()`: 엔티티 하나 삭제, 내부에서 `EntityManager.remove()` 호출
- `findById()`: 엔티티 하나를 조회, 내부에서 `EntityManager.find()` 호출
- `getOne()`: 엔티티를 프록시로 조회, 내부에서 `EntityManager.getReference()` 호출
- `findAll()`: 모든 엔티티 조회, 정렬이나 페이징 조건 제공 가능

## 쿼리 메서드 기능

스프링 데이터 JPA가 제공하는 쿼리 메서드 기능은 크게 3가지가 있다.

- 메서드 이름으로 쿼리 생성
- 메서드 이름으로 JPA NamedQuery 호출
- `@Query` 어노테이션을 사용해 Repository 인터페이스에 쿼리 직접 적용

### 메서드 이름으로 쿼리 생성

스프링 데이터 JPA에서 정해진 규칙으로 메서드 이름을 지으면 알아서 JPQL로 변경해준다.

```java
//JPA 
public List<Member> findByUsernameAndAgeGreaterThan(String username, int age) {
	
	return em.createQuery("select m from Member m where m.username = :username and m.age >:age")
		.setParameter("username",username)
		.setParameter("age",age)
		.getResultList();
}
//스프링 데이터 JPA
public interface MemberRepository extends JpaRepository<Member,Long> {
	
	List<Member> findByUsernameAndAgeGreaterThan(String username, int age);	
	}
```

메서드가 여러가지가 있지만, 규칙을 잠깐 살펴보면,

- `findBy`, `readBy`, `queryBy`, `getBy` 등등
- `countBy`, `existsBy`, `deleteBy` 등등
- `findFirst3`, `findTop3` 등등
- 더 많은건 [스프링 공식 문서](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.query-methods)를 참고하자

### JPA NamedQuery 호출

`@NamedQuery`**로 미리 정의하는 방법**

```java
// NamedQuery 정의
@Entity
@NamedQuery(
    name = "Member.findByUsername",
    query = "select m from Member m where m.username = :username")
public class Member {
    ...
}
```

**JPA 또는 스프링 데이터 JPA로 호출해서 사용하는 방법**

```java
// NamedQuery 호출
public interface MemberRepository extends JpaRepository<Member, Long> {

    List<Member> findByUsername(@Param("username") String username);
}
```

스프링 데이터 JPA는 도메인 클래스 + 메서드 이름으로 Named 쿼리를 알아서 찾아서 실행한다.

하지만 `@Query`가 편하여 잘 안쓰인다고 한다.

### Query, Repository 메서드에 쿼리 정의

실행할 메서드 위에 `@Query`를 통해 정적 쿼리를 직접 작성한다.

```java
public interface MemberRepository extends JpaRepository<Member, Long> {

    @Query("select m from Member m where m.username = ?1")
    List<Member> findByUsername(String username);
}
```

애플리케이션 실행 시점에서 문법 오류를 발견할 수 있는 방법이며, 실무에서 가장 많이 쓰인다고 한다.

값, DTO 조회도 할 수 있다.

```java
//단순 값 조회
@Query("select m.username from Member m")
List<String> findUsernameList();

//DTO로 직접 조회
@Query("select new study.datajpa.repository.MemberDto(m.id,m.username,t.name" + "from Member m join m.team t")
List<MemberDto> findMemberDto();
```

DTO로 조회할 때 JPA의 new 명령어를 사용해야 한다. 그리고 생성자가 맞는 dto가 있어야 한다.

### 파라미터 바인딩

스프링 데이터 JPA는 이름 기반 파라미터 바인딩(`:username`)과 위치 기반 파라미터 바인딩(`?1`)을 모두 지원한다.

하지만 이름 기반이 더 가독성이 좋고 위치 기반은 순서를 맞춰야해서 쓰기가 까다롭다.

```java
public interface MemberRepository extends JpaRepository<Member, Long> {

    List<Member> findListByUsername(String username); //컬렉션

    Member findMemberByUsername(String username); //단건

    Optional<Member> findOptionalByUsername(String username); //옵셔널
}
```

컬렉션은 결과가 없으면 빈 컬렉션을 반환하고, 단 건 조회는 결과가 없으면 `Null`, 결과가 2개 이상이면 `NonUniqueResultException`을 반환합니다.

### 페이징, 정렬

**순수 JPA**

```java
public List<Member> findByPage(int age, int offset, int limit) {
        return em.createQuery("select m from Member m where m.age=:age order by m.username desc",Member.class)
                .setParameter("age",age)
                .setFirstResult(offset) //몇명부터
                .setMaxResults(limit) //최대 몇명
                .getResultList();
    }

    
public long totalCount(int age) {
        return em.createQuery("select count(m) from Member m where m.age= :age",Long.class)
                .setParameter("age",age)
                .getSingleResult();
    }
```

여기서 count 쿼리를 하냐 안하냐로 또 나눌 수 있다.

```java
// count 쿼리O
Page<Member> findByUsername(String name, Pageable pagealbe);

//count 쿼리X
Slice<Member> findByUsername(String name, Pageable pageable);

//count 쿼리 O
List<Member> findByUsername(String name, Pageable pageable);

//정렬
List<Member> findByUsername(String name,Sort sort);
```

여기서 `Slice`는 추가 **count 쿼리 없이 다음 페이지만 확인**할 수 있고, 내부적으로 `limit + 1`을 **조회**한다.

구현되는 그림은 애플리케이션에서 더 보기할 때 하는 느낌이다.

물론 count 쿼리를 분리할 수도 있다.(`join` 없이 `totalCount`만 가져올 경우)

```java
@Query(value = "select m from Member m",
		countQuery = "select count(m.usernmae) from Member m")
Page<Member> findMemberAllCountBy(Pageable pageable);
```

**페이지를 유지하면서 엔티티를 DTO로 변환하는 코드**

```java
Page<Member> page = memberRepository.findByAge(10,pageRequest);

Page<MemberDto> dtoPage = page.map(m -> new MemberDto(m.getId(), m.getUsername(), null));
```

### 벌크성 수정 쿼리

**순수 JPA**

```java
public int bulkAgePlus(int age) {

	int resultCount = em.createQuery(
		"update Member m set m.age = m.age+1" +
			"where m.age >= :age")	
		.setParameter("age",age)
		.executeUpdate(); //업데이트 쿼리(벌크 쿼리)

	return resultCount

	}
```

**스프링 데이터 JPA를 사용한 벌크 쿼리**

```java
@Modifying(clearAutomatically=true) // 이게 있어야 executeUpdate가 실행
@Query("update Member m set m.age = m.age+1 where m.age>= :age")
int bulkAgePlus(@Param("age") int age);
```

벌크 쿼리는 영속성 컨텍스트에 거치는게 아닌 DB에 바로 실행을 해서 후처리를 해줘야 나중에 조회할 때 올바른 값이 나온다.

따라서 `clearAutomatically = true`를 넣어줘서 정리를 해줘야 한다.

그래서 다음 로직이 있다면 주의해서 사용해야 한다.

### @EntityGraph

예를들어 `Member`가 팀을 참조하고 있고, 지연로딩 관계인데

```java
List<Member> members = memberRepository.findAll();
for(Member member : members) {
	
	System.out.println("member.getTeam().getName()")

}
```

`findAll` 했을 때 `Member`를 찾기 위해 쿼리가 한번 하고(1), 루프를 돌면서 각각의 `Member`에 해당하는 팀 프록시를 찾기 위해 쿼리가 한번 더 나간다.(N)

이럴 때 `N + 1` 문제가 발생한다.

그래서 연관된 엔티티를 한번에 조회하기 위해 `Fetch Join`을 사용해야 한다.

```java
@Query("select m from Member m left join fetch m.team")
List<Member> findMemberFetchJoin();
```

스프링 데이터 JPA는 이 `@EntitGraph` 기능을 지원해서 `Fetch Join`을 사용할 수 있다.

```java
@Override //공통 메서드 오버라이드
@EntityGraph(attributePaths = {"team"})
List<Member> findAll();

//Jpql + 엔티티그래프
@EntityGraph(attributePaths = {"team"})
@Query("select m from Member m")
List<Member> findMemberEntityGraph();

//메서드 쿼리
@EntityGraph(attributePaths = {"team"})
List<Member> findByUsername(@Param("username") String username)

@EntityGraph(attributePaths = {"addresses"}, type = EntityGraph.EntityGraphType.LOAD)
Optional<User> findWithAddressesById(Long userId);

@NamedEntityGraph(name = "Member.all", attributeNodes = @NamedAttributeNode("team"))
@Entity
public class Member {

}

@EntityGraph("Member.all")
@Query)("select m from Member m")
List<Member> findMemberByEntityGraph();
```

`attributePaths` 옵션 안에 해당 되는 엔티티를 넣으면 된다.

`@NamedEntityGraph`도 동일하게 사용하면 된다.

`@EntityGraph`의 옵션은 type이 있다.

- `Fetch`: 어노테이션에 명시한 attribute는 EAGER로 패치하고, 나머지 attribute는 Lazy로 패치한다.
- `LOAD`: 어노테이션에 명시한 attribute는 EAGER로 패치하고, 나머지는 엔티티에 명시한 fetch type나 디폴트 Fetch Type으로 패치한다.

→ `@OneToMany`는 LAZY, `@ManyToOne`은 EAGER가 디폴트이다.

### 쿼리 힌트

```java
@QueryHints(value = @QueryHint(name = "org.hibernate.readOnly",value = "true"))
Member findreadOnlyByUsername(String username);
```

영속성 컨텍스트에서 스냅샷을 안만들어서 변경이 안된다고 가정하고 다 무시한다.

```java
@QueryHints(value = {@QueryHint(name = "org.hibernate.readOnly", value = "true)},forCounting = true)

Page<Member> findByUsername(String name, Pagable pageable);
```

반환 타입이 page이면 count 쿼리도 쿼리 힌트가 적용된다.

참고로 이렇게 많은 쿼리에 대한 옵션이 있지만, 많이 쓴다고 성능 최적화가 되는게 아니고, 진짜 중요하고 트래픽이 많은 API 몇개만 사용하는 것이지 중요한건 복잡한 쿼리가 아니라고 한다.

### 명세

명세를 이해하기 위해서는 술어라는 단어를 알아야하는데, **술어**는 **데이터를 검색하기 위한 제약 조건 하나 하나**를 의미한다.

스프링 데이터 JPA는 `Specification` 클래스로 정의하였는데, 다양한 검색 조건을 조립하여 새로운 검색 조건을 쉽게 만들 수 있다.

```java
// 명세 정의
public class OrderSpec {

    public static Specification<Order> memberName(String memberName) {
        return new Specification<Order>() {
            public Predicate toPredicate(Root<Order> root,
                CriteriaQuery<?> query, CriteriaBuilder builder) {

                if (StringUtils.isEmpty(memberName)) return null;

                Join<Order, Member> m = root.join("member", JoinType.INNER);
                return builder.equal(m.get("name"), memberName);
            }
        }
    };
    // 비슷한 방식으로 isOrderStatus() 구현했다고 하면.
}

// 명세 사용
List<Order> result = orderRepository.findAll(
    where(memberName(name)).and(isOrderStatus())
);
```

JPA Criteria 방식으로 명세를 위와 같이 정의하고 사용할 수 있다.

### 사용자 정의 Repository 구현

특정 메서드를 직접 구현하기 위해 구현체를 만들어야 하는 경우가 있다.

`Repository`를 **직접 구현하면 공통 인터페이스가 제공하는 기능까지 모두 구현해야 한다는 문제**가 있다.

→ **스프링 데이터 JPA**는 **해당 문제를 해결하여 필요한 메서드만 구현해 주는 방식을 제공**한다.

```java
// 사용자 정의 인터페이스
public interface MemberRepositoryCustom {

    public List<Member> findMemberCustom();
}

// 사용자 정의 구현 클래스
public class MemberRepositoryImpl implements MemberRepositoryCustom {

    @Override
    public List<Member> findMemberCustom() {
        ...
    }
}

// 사용자 정의 인터페이스 상속
public interface MemberRepository extends JpaRepository<Member, Long>, 
    MemberRepositoryCustom {

}
```

`Repository` **구현 클래스 이름 끝에 무조건** `Impl`**을 붙여**야 스프링 데이터 JPA가 사용자 정의 `Repository`로 인식한다.

## Spring Data JPA와 QueryDSL 통합

### QuerydslPredicateExecutor(인터페이스 지원)

```java
public interface QuerydslPredicateExecutor<T> {
	Optional<T> findById(Predicate predicate);
    Iterable<T> findAll(Predicate predicate);
    long count(Predicate predicate);
    boolean exists(Predicate predicate);
    // _ more functionality omitted.
}
```

위와 같이 구성된 인터페이스인데, 적용 시켜보면

```java
import org.springframework.data.querydsl.QuerydslPredicateExecutor;

public interface MemberRepository extends JpaRepository<Member, Long> , MemberRepositoryCustom, QuerydslPredicateExecutor<Member> {
    List<Member> findByUsername(String username);
}
```

이렇게 `QuerydslPredicateExecutor` 를 `implements`를 하면 안에 구성된 메서드를 모두 사용할 수 있다.

특히 메서드 인자로 `Predicate` 타입을 직접 넘겨주는데, 이는 querydsl 사용시 `where` 조건문에 넣었던 타입이다.

**한계점**

- 조인이 X: 묵시적 조인은 가능하지만 `left join`이 불가능
- 클라이언트가 Querydsl에 의존해야 한다. 서비스 클래스가 Querydsl이라는 구현 기술에 의존하게 된다.
- 복잡한 실무 환경에서 사용하기에는 한계가 명확하다.

### QuerydslRepositorySupport(레파지토리 지원)

```java
@Repository
public abstract class QuerydslRepositorySupport {

   private final PathBuilder<?> builder;

   private @Nullable EntityManager entityManager;
   private @Nullable Querydsl querydsl;

   public QuerydslRepositorySupport(Class<?> domainClass) {
      Assert.notNull(domainClass, "Domain class must not be null!");
      this.builder = new PathBuilderFactory().create(domainClass);
   }

   @Autowired
   public void setEntityManager(EntityManager entityManager) {
      Assert.notNull(entityManager, "EntityManager must not be null!");
      this.querydsl = new Querydsl(entityManager, builder);
      this.entityManager = entityManager;
   }

   @PostConstruct
   public void validate() {
      Assert.notNull(entityManager, "EntityManager must not be null!");
      Assert.notNull(querydsl, "Querydsl must not be null!");
   }

   @Nullable
   protected EntityManager getEntityManager() {
      return entityManager;
   }

   protected JPQLQuery<Object> from(EntityPath<?>... paths) {
      return getRequiredQuerydsl().createQuery(paths);
   }

   protected <T> JPQLQuery<T> from(EntityPath<T> path) {
      return getRequiredQuerydsl().createQuery(path).select(path);
   }

   protected DeleteClause<JPADeleteClause> delete(EntityPath<?> path) {
      return new JPADeleteClause(getRequiredEntityManager(), path);
   }

   protected UpdateClause<JPAUpdateClause> update(EntityPath<?> path) {
      return new JPAUpdateClause(getRequiredEntityManager(), path);
   }

   @SuppressWarnings("unchecked")
   protected <T> PathBuilder<T> getBuilder() {
      return (PathBuilder<T>) builder;
   }

   @Nullable
   protected Querydsl getQuerydsl() {
      return this.querydsl;
   }

   private Querydsl getRequiredQuerydsl() {
      if (querydsl == null) 
         throw new IllegalStateException("Querydsl is null!");
      return querydsl;
   }

   private EntityManager getRequiredEntityManager() {
      if (entityManager == null) 
         throw new IllegalStateException("EntityManager is null!");
      return entityManager;
   }
}
```

이렇게 내부 코드가 구성되어 있다.

`extends` 해서 사용하면

```java
public class MemberRepositoryImpl extends QuerydslRepositorySupport implements MemberRepositoryCustom{

    public MemberRepositoryImpl() {
        super(Member.class); /* 엔티티타입을 super에 넘겨준다.*/
    }

    @Override
    public List<MemberTeamDto> search2(MemberSearchCondition condition) {
        List<MemberTeamDto> result = from(member)
                .leftJoin(member.team, team)
                .where(
                        usernameEq(condition.getUsername()),
                        teamNameEq(condition.getTeamName()),
                        ageGoe(condition.getAgeGoe()),
                        ageLoe(condition.getAgeLoe())
                )
                .select(new QMemberTeamDto(
                        member.id,
                        member.username,
                        member.age,
                        team.id,
                        team.name
                ))
                .fetch();
    }
}
```

이전에 사용한 `MemberRepositoryImpl`에서 `QuerydslRepository`를 `extends`했다. 그리고 생성자에 `super(엔티티 타입)`을 해주는 것을 볼 수 있다.

`Page` 메서드를 작성했을 때는 `offset`과 `limit`을 생략할 수 있다.

**장점**

- `getQuerydsl().applyPagination()`스프링 데이터가 제공하는 페이징을 Querydsl로 편리하게 변환이 가능하다.(하지만 `Sort`는 오류 발생)
- `from()`으로 시작 가능(최근에는 `QueryFactory`를 사용하여 `select()`로 시작하는 것이 더 명시적)
- `EntityManager` 제공

**한계점**

- Querydsl 3.x 버전을 대상으로 만들었다.
- Querydsl 4.x에 나온 `JPAQueryFactory`로 시작할 수 없다.
- `select`로 시작할 수 없다.(`from`으로 시작해야 한다) `QueryFactory`를 제공하지 않는다.
- 스프링 데이터 `Sort` 기능이 정상 동작하지 않는다.

**참고 자료**

- <https://wonin.tistory.com/496>
- <https://kingchan223.tistory.com/391>
- <https://steady-coding.tistory.com/592#google_vignette>
- [https://velog.io/@junho918/Spring-Data-Jpa-JPA..그래-알겠어..-%EA%B7%B8%EB%9E%98%EC%84%9C-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-JPA%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%93%B0%EB%8A%94%EB%8D%B0](https://velog.io/@junho918/Spring-Data-Jpa-JPA..%EA%B7%B8%EB%9E%98-%EC%95%8C%EA%B2%A0%EC%96%B4..-%EA%B7%B8%EB%9E%98%EC%84%9C-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-JPA%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%93%B0%EB%8A%94%EB%8D%B0)
