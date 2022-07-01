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