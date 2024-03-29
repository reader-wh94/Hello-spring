## 스프링 입문 - 코드로 배우는 스프링 부트, 웹MVC, DB 접근 기술

### 1. 프로젝트 환경설정

1. 프로젝트 생성

   - ```mavenCentral()``` 에서 필요한 라이브러리들을 다운받는다.

2. 라이브러리 살펴보기

   * ```spring-boot-starter-web``` 라이브러리를 Gradle한다면 해당 라이브러리도 의존 관계를 유지하고 있기 때문에 관련 라이브러리들까지 같이 가져와진다.
   * 최근에는 라이브러리 하나 불러오면 웹서버까지 자동으로 설치되어 :8080에도 접속할 수 있다.
   * 현업에 종사할 때에는 로그를 ```System.out.println()``` 으로 출력하는 것이 아니라 ```log``` 를 사용해야 한다.
   * ```spring-boot-starter-logging``` 을 땡겨오면 자동으로 ```logback``` 과 ```slf4j``` 가 설치된다. 
     * System.out.println 대신 로그를 확인하기 위해 사용되는 라이브러리들이다.
   * test 관련 라이브러리 >> junit

3. View 환경 설정

   * spring-boot에서는 Welcome Page를 제공해준다
     * 모르는게 있으면 ```spring.io``` 에서 바로바로 찾아볼 수 있어야 한다.
   * ```<html xmlns:th="http://www.thymeleaf.org">``` 을 넣어주면 thymeleaf를 템플릿 엔진으로써 사용할 수 있다.

4. 빌드하고 실행하기

   * 빌드는 ```gradlew build``` 
     * Window 기준
* 서버에 배포 할 때에는 ```{프로젝트 이름}-0.0.1-SNAPSHOT.jar``` 만 넘겨주면 된다.
   * 혹시 빌드가 잘 안된다면 ```gradlew clean build``` 사용하면 된다.



### 2. 스프링 웹 개발 기초

1. 정적 컨텐츠
   * 서버에서 하는 것 없이 파일을 그대로 웹 브라우저에 띄움
   * MVC와 템플릿 엔진
     * JSP, PHP 등이 템플릿 엔진
     * 서버에서 프로그래밍해서 html을 동적으로 바꿔서 띄운다
   * 정적 컨텐츠는 파일을 그대로 고객에게 전달, MVC와 템플릿 엔진 서버에서 변형을 해서 html을 바꿔서 넘겨주는 방식이다.
   * API
     * JSON이라는 데이터 포맷 구조를 사용
     * 서버끼리 데이터를 주고 받을 때 API 방식이라고 한다.
2. MVC와 템플릿 엔진
   * MVC는 Model, View, Controller이다.
   * 과거에는 View에서 모든 걸 해결함 (Model 1 방식)
   * View는 화면을 그리는 데에 집중
   * 나머지 Model과 Controller은 비즈니스 로직을 처리하는 데에 더 치중한다.
3. API
   * html로 내리지 않으면 API 방식으로 데이터를 내린다.
   * 이 전의 템플릿 엔진은 View라는 템플릿을 가지고 조작하는 방식이라면 API는 데이터를 그대로 내려준다. 
   * 과거에는 xml 방식을 많이 사용했었다. 최근에는 JSON을 사용함



### 3. 회원 관리 예제 - 백엔드 개발

1. 비즈니스 요구사항 정리
   * 조건: 아직 데이터 저장소가 선정되지 않음(가상의 시나리오)
     * 데이터 저장소가 선정되지 않아서, 우선 인터페이스로 구현 클래스를 변경할 수 있도록 설계
     * 개발을 진행하기 위해서 초기 개발 단계에서는 구현체로 가벼운 메모리 기반의 데이터 저장소 사용
   
2. 회원 서비스 테스트

   * MemberSerivce에 있는 MemberRepository는 new 해서 다른 객체

   * test에서 하는 new MemoryMemberRepository()는 위와 다른 객체

     * 굳이 두 개를 쓸 필요가 없다
     * MemoryMemberRepository에서 static으로 쓰고 있는데, static을 지우면 에러가 발생할 것

   * MemberSerivce에서 사용하고 있는 new MemoryMemberRepository()와 Testcase에서 만든 new MemoryMemberRepository()가 서로 다른 리퍼지토리(다른 인스턴스)이다.

     * 같은 걸로 테스트하는 것이 맞음
     * 현재 다른 리퍼지토리 이용 중

   * ```java
     private final MemberRepository memberRepository = new MemoryMemberRepository();
     
     // 에서
     
     private final MemberRepository memberRepository;
     
         public MemberService(MemoryMemberRepository memberRepository) {
             this.memberRepository = memberRepository;
         }
         
     // 로 변경
     ```

   * MemberService 관점에서 자신이 직접 new를 하는 것이 아니라 MemberRepository를 외부에서 넣어준다. 

     * 이를 DI라고 함



### 4. 스프링 빈과 의존관계

1. 컴포넌트 스캔과 자동 의존관계 설정
   * ```@Component``` ```@Controller``` 등등..
   * 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록한다.
2. 자바 코드로 직접 스프링 빈 등록하기
   * SpringConfig 파일 만들어서 ```@Bean``` 사용



### 5. 회원 관리 예제

1. 회원 웹 기능 - 등록
   * URL 입력하는 방식은 GetMapping, 전달 할 때 Get 사용
   * Post 방식은 (```@PostMapping```) 보통 데이터를 폼에 넣어서 전달할 때 사용



### 6. 스프링 DB 접근 기술

1. 순수 JDBC
   * JDBC는 20년 전 쯤에 쓰던 방법 최근에는 잘 쓰지 않는다.
2. 스프링 통합 테스트
   * 테스트는 반복할 수 있어야 한다.
   * DB는 트랜잭션이라는 것이 있어서 COMMIT을 하기 전까지는 DB에 반영이 되지 않는다.
   * ```@Transactional``` 애노테이션을 테스트 케이스에 달면 테스트를 실행할 때 트랜잭션을 먼저 실행하고 다음에 DB에 insert query하고 다 넣은 다음에 ROLLBACK을 해준다.
     * DB에 넣었던 데이터가 저장이 안되고 깔끔하게 지워진다.
3. 스프링 JdbcTemplate
   * 생성자가 딱 하나 있으면 빈으로 등록될 때 ```@Autowired``` 생략 가능하다.
4. JPA
   * JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환할 수 있다.
   * JPA는 인터페이스이고 구현은 Hibernate로 사용한다. (구현체는 더 다양함)
   * ORM(Object Relationship Mapping) >> Member에 ```@Entity``` 사용 (jpa가 관리하는 객체)
   * JPA를 쓸려면 EntityManager를 주입받아야 한다.
   * JPA를 사용할려면 항상 서비스에 ```@Transactional```이 있어야 함



### 7. AOP

1. AOP가 필요한 상황

   * 전체 메소드에 시간을 재는 기능을 추가해야 한다면?

   * 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern)으로 나눌 수 있다.

2. AOP 적용

   * aop 적용 시 ```@Aspect``` 사용
   * aop를 bean으로 등록해야한다 -> ```@Component``` 사용