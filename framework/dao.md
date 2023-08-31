## DAO와 Repository의 차이점

이 부분에서는 논란이 많다고 한다. 

개인적인 견해가 다 달라서 내용은 그냥 “~아 그렇구나” 하고 넘어가자.

- **Repository == dao (비슷함)**
    
    이 둘은 거의 같다고 생각해도 무방하다고 한다. 좀 더 깊이있게 차이를 보자면, **Repository**는 **엔티티 객체를 보관하고 관리하는 저장소**이고, **dao**는 데이터에 접근하도록 **DB접근 관련 로직을 모아둔 객체**이다. 둘다 개념의 차이일 뿐 실제로 개발할 때는 비슷하게 사용된다.
    
1. **추상화**
    - DAO는 영속성 컨텍스트(data Persistence)의 추상화이다.
    - Repository는 오브젝트 하나의 컬렉션 추상화이다.
2. **도메인, 데이터베이스**
    - DAO는 데이터베이스와 관련이 많으며 Table 중심이다.
    - Repository는 도메인과 관련이 많으며 Arggregate Roots만을 다룬다.

Repository는 여러 DAO를 사용해 구현될 수 있지만, 그 반대는 안된다.

또한 Repository는 보통 한정된 인터페이스이다. 단순시 Get(id), Find(Specification), Add(Entity)와 같은 객체들의 collection이다.

Update 같은 메서드는 DAO에 적합하고, Repository를 사용할 땐 보통 분리된 비즈니스 로직 수준의 트랜잭션에 의해 엔티티들의 변화가 관리된다.

둘다 비슷한데, DAO가 Repository보다 좀 더 유연하다. 그래서 둘다 쓰고 싶으면 DAO에서 Repository를 사용한다.

### Repository

**객체들의 특정 타입의 저장소**이다. 특정 타입의 객체들을 찾을 수 있고 저장할 수 있다.

**보통 객체들의 한가지 타임**만 다룬다.

아래 코드를 보자.

```java
List<Apple> findAll(Creteria criteria);
Apple save(Apple apple);
```

**Repository**는 **DDD** 용어이다. 데이터베이스 용어가 아니다. 그래서 DB에 저장되는 것과는 아무런 상관이 없다.

그럼 **DDD**란 뭘까?

**Domain-Driven-Design**의 약어로, 도메인 주도 설계라는 **이름의 도메인과 일치하도록 소프트웨어를 모델링하는 데 중점**을 둔 소프트웨어 설계 접근 방식이다.

자세한 내용은 [해당 링크](https://ittrue.tistory.com/252)에서 확인하길 바란다. 

Repository는 대부분 같은 테이블에 모든 데이터를 저장한다. 그런데 패턴은 요구 되지않는다.

근데 단일 타입의 데이터만 처리하기 때문에 하나의 기본 테이블에 논리적으로 연결된다.

### DAO

**Dato-Access-Object**의 약어로, **데이터베이스에 접근하는 역할을 하는 객체**이다.

데이터의 CRUD 작업을 실행하는 클래스, CRUD 기능을 전담하는 오브젝트가 있다.

```java
public class UserDao {
	public void add(User user) throws ClassNotFoundException, SQLException {
        Class.forName("com.mysql.jdbc.Driver");
        Connection c = DriverManager.getConnection("jdbc:mysql://localhost:3306/spring?autoReconnect=true&amp;useSSL=false\";","root","@min753951");

        PreparedStatement ps = c.prepareStatement(
                "insert into users(id, name, password) values(?,?,?)");

        ps.setString(1, user.getId());
        ps.setString(2, user.getName());
        ps.setString(3, user.getPassword());

        ps.executeUpdate();

        ps.close();
        c.close();

    }
	
	public User get(String id) throws ClassNotFoundException, SQLException {
		Class.forName("com.mysql.jdbc.Driver");
        Connection c = DriverManager.getConnection("jdbc:mysql://localhost:3306/spring?autoReconnect=true&amp;useSSL=false\";","root","@min753951");

        PreparedStatement ps = c.prepareStatement(
                "select * from users where id = ?");
		ps.setString(1,  id);
		
		ResultSet rs = ps.executeQuery();
		rs.next();
		User user = new User();
		user.setId(rs.getString("id"));
		user.setName(rs.getString("name"));
		user.setPassword(rs.getString("password"));
		
		rs.close();
		ps.close();
		c.close();
        
        return user;
	}

}
```

### DAL

**DAO**와 **Repository**는 모두 **DAL**(**Data Access Layer**)의 **구현체**이다.

**DAL**이란 DB와 연결된 객체지향 애플리케이션들은 반드시 DB관련 정보들을 처리해야 한다. 그러기 위해서 해당 코드를 깔끔하게 모듈화 해야하는데, **Layerd Architecuture에서 사용하는 모듈**이 **DAL**이다.

이 DAL을 어떻게 모듈화 하냐?

코드로 구현 → 스프링의 Hibernate

**DAO패턴**은 **DAL를 생성하는 한 방법**이고, **보통 도메인 엔티티마다 자신의 DAO**를 가지고 있다.

Repository는 DAO와 비슷하게 Repository 또한 DAL를 구현하는 방법 중 하나이다.

이 패턴의 중요한 포인트는 사용자의 관점에서의 collection처럼 보고 행동한다는 것이다.

여기서 collection은 자바 객체 collection이 아닌 더하고 빼고 포함하는 것과 같은 연산들이다.

Hibernate는 Repository의 패턴이 DAO로 실체화되어 있다. 이것은 DAL이며 DAO 패턴임과 동시에 Repository이다. Repository 패턴이 반드시 DAO 상위에 구축되는 것은 아니다. 위에 언급된 연산들을 지원하는 인터페이스로 설계된 DAO는 Repository 패턴의 객체이다.

정리하자면, 둘다 비슷하지만, **DAL을 구현하는 방식 차이**이고, **목적 및 설계 차이**가 있다.

**Repository**는 **DDD** 용어이기 때문에 **도메인 중심**으로 설계되어 Domain의 Aggregate당 하나씩 보통 생성된다. 그래서 자연스럽게 Aggregate Root와 관계된 **Objects들의 연산(Collection)으로 구성**되어 있으며, 그 타입은 Aggregate Root 하나인 것이다.

**DAO**는 **Table**로 생각하는 **Model에 하나씩 생성**하면 된다.

## DAO, VO, Entity, Domain 차이

![Untitled](https://github.com/Heo-y-y/development-blog/assets/112863029/5b81f5f3-4e54-4eb6-a9e1-8e6f8500616b)

### DTO

DTO는 계층 간 데이터 교환을 하기 위해 사용하는 객체이다. 로직을 가지고 있지 않고, 순수 데이터 객체로써 getter, setter 메서드만 가지고 있는 클래스이다. 주로 Clinet(View)와 Controller 사이에서 데이터를 주고받는다.

### VO

값 그자체인 객체이다. 로직을 포함할 수 있으며, 불변성 보장을 위해 생성자를 사용해야 한다. VO는 서로 다른 이름을 갖는 인스턴스끼리라도 모든 속성 값이 같다면 같은 객체인게 핵심이다.

### Domain, Entity 차이

Domain은 앱의 기능, 기획의 요구사항을 개발하는 영역(비즈니스 로직)을 도메인이라고 정의한다.

Entity는 실체, 데이터베이스 테이블 또는 데이터베이스 테이블에 해당하는 객체라고 보면된다.

정리하자면, **DTO**는 **getter**, **setter**를 가지고 있어서, **변할 수 있는 클래스**이며 **계층간 데이터 교환이 주목적**이다.

**VO**는 **읽기만 가능한 객체**(setter X)로써, **데이터 그 자체에 주목적**이다.

**Domain**은 **기능, 비즈니스 로직을 정의하는 영역**이다.

**Entity**는 **실체, 객체나 테이블**이라고 보면 된다.

**참고 자료**

- [https://www.inflearn.com/questions/111159/domain과-repository-질문](https://www.inflearn.com/questions/111159/domain%EA%B3%BC-repository-%EC%A7%88%EB%AC%B8) https://bbbicb.tistory.com/44
- [https://bperhaps.tistory.com/entry/Repository와-Dao의-차이점](https://bperhaps.tistory.com/entry/Repository%EC%99%80-Dao%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)
- <https://ccomccomhan.tistory.com/35>
- <https://bbbicb.tistory.com/46>
- [https://maenco.tistory.com/entry/Java-DTO와-VO의-차이](https://maenco.tistory.com/entry/Java-DTO%EC%99%80-VO%EC%9D%98-%EC%B0%A8%EC%9D%B4)
- [https://velog.io/@yeahg_dev/Domain과-Entity의-개념](https://velog.io/@yeahg_dev/Domain%EA%B3%BC-Entity%EC%9D%98-%EA%B0%9C%EB%85%90)
