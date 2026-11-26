# Spring Framework / Spring Boot Migration Knowledge

## Transaction Management

- Spring Framework 6.2부터 어노테이션 기반 트랜잭션의
  기본 롤백 정책을 전역적으로 변경할 수 있다.
- 다음 설정은 checked exception도 기본 롤백 대상으로 지정한다.

  @EnableTransactionManagement(
      rollbackOn = RollbackOn.ALL_EXCEPTIONS
  )

- 위 설정은 기존 XML의 `<tx:advice>`로 정의한 롤백 규칙까지
  자동으로 변경하지는 않는다.
- RollbackOn은 org.springframework.transaction.annotation
  패키지에 속한다.
- 별도 설정이 없는 경우 기존 기본 롤백 정책은 유지된다.
  - RuntimeException: 롤백
  - Error: 롤백
  - Checked Exception: 기본적으로 롤백하지 않음
- 개별 @Transactional의 rollbackFor, noRollbackFor 설정은
  전역 기본 롤백 정책보다 우선한다.

## MyBatis Transaction Management

- Spring이 관리하는 SqlSession 및 SqlSessionTemplate에서
  다음 메서드를 직접 호출할 수 없다.
  - commit()
  - rollback()
  - close()
- 위 메서드를 직접 호출하면 UnsupportedOperationException이
  발생한다.
- Spring 트랜잭션 외부에서 수행되는 Spring 관리 Mapper의
  데이터베이스 작업은 자동으로 커밋된다.

## MyBatis Version Compatibility

- MyBatis Spring Boot Starter 3.0.x의 지원 환경:
  - Spring Boot 3.2 ~ 3.5
  - MyBatis-Spring 3.0
  - MyBatis 3.5
  - Java 17 이상

- MyBatis Spring Boot Starter 4.0.x의 지원 환경:
  - Spring Boot 4.0
  - MyBatis-Spring 4.0
  - MyBatis 3.5
  - Java 17 이상

- Spring Boot 4.0에서 사용하는 공식 지원 Starter는
  mybatis-spring-boot-starter 4.0.x이다.

## Jackson 3

- Spring Boot 3.x는 Jackson 2를 기본으로 사용한다.
- Spring Boot 4.0은 Jackson 3을 기본으로 사용한다.
- Jackson 3의 최소 Java 버전은 Java 17이다.
- Jackson 3의 주요 패키지는 com.fasterxml.jackson에서
  tools.jackson으로 변경되었다.

- ObjectMapper 패키지 변경:
  - Jackson 2: com.fasterxml.jackson.databind.ObjectMapper
  - Jackson 3: tools.jackson.databind.ObjectMapper

- JsonMapper 패키지 변경:
  - Jackson 2: com.fasterxml.jackson.databind.json.JsonMapper
  - Jackson 3: tools.jackson.databind.json.JsonMapper

- jackson-annotations는 기존 패키지명을 유지한다.
  - com.fasterxml.jackson.annotation.JsonProperty
  - com.fasterxml.jackson.annotation.JsonIgnore
  - com.fasterxml.jackson.annotation.JsonCreator
  - com.fasterxml.jackson.annotation.JsonInclude

- jackson-annotations의 Maven groupId는
  com.fasterxml.jackson.core를 유지한다.

- Spring Boot 4.0은 Jackson 2와 Jackson 3의 공존을 지원한다.
- Jackson 2와 Jackson 3 모두 의존성 버전 관리 대상이다.
- spring-boot-jackson2 모듈을 통해 기존 Jackson 2 기반
  자동 설정을 사용할 수 있다.
- spring-boot-jackson2 모듈은 deprecated 상태이다.
- Jackson 3 설정 접두사는 spring.jackson이다.
- Jackson 2 설정 접두사는 spring.jackson2이다.
- spring.jackson.use-jackson2-defaults=true 설정은
  Jackson 3의 기본 동작을 Boot 3.x의 Jackson 2 설정에
  최대한 가깝게 조정한다.

## Spring Boot 4 Modularization

- Spring Boot 4.0에서는 기능별 모듈 분리가 확대되었다.
- Spring MVC용 spring-boot-starter-webmvc가 제공된다.
- 기존 spring-boot-starter-web은 deprecated 상태이다.
- Spring Boot 4.0에서 권장되는 Spring MVC Starter는
  spring-boot-starter-webmvc이다.
- 모듈 분리로 인해 일부 Auto-configuration 클래스의
  패키지 위치가 변경되었다.

## Embedded Servlet Container

- Spring Boot 4.0은 Servlet 6.1 이상을 요구한다.
- Spring Boot 4.0에서 Undertow 내장 서버 지원이 제거되었다.
- spring-boot-starter-undertow도 제거되었다.
- Spring Boot 4.0의 내장 Servlet 컨테이너 지원:
  - Tomcat 11.0
  - Jetty 12.1

## Logging

- Spring Framework 7.0에서 spring-jcl 모듈이 제거되었다.
- Spring Framework 7.0에서는 Apache Commons Logging
  1.3.0 이상을 사용한다.
- org.apache.commons.logging.Log 및 LogFactory API는
  계속 사용할 수 있다.
- spring-jcl 제거가 기존 Commons Logging API 자체의
  제거를 의미하지는 않는다.

## Spring Test and Mockito

- Spring Boot의 @MockBean과 @SpyBean은
  Spring Boot 3.4에서 deprecated for removal 지정되었다.
- 두 어노테이션은 Spring Boot 4.0에서 실제 제거되었다.

- 기존 어노테이션 패키지:
  - org.springframework.boot.test.mock.mockito.MockBean
  - org.springframework.boot.test.mock.mockito.SpyBean

- Spring Framework 6.2에서 다음 어노테이션이 도입되었다.
  - @MockitoBean
  - @MockitoSpyBean

- 새로운 어노테이션 패키지:
  - org.springframework.test.context.bean.override.mockito.MockitoBean
  - org.springframework.test.context.bean.override.mockito.MockitoSpyBean

- @MockBean의 대체 API는 @MockitoBean이다.
- @SpyBean의 대체 API는 @MockitoSpyBean이다.
- 기존 테스트 클래스 필드에 선언된 @MockBean과 @SpyBean은
  대상 Bean의 존재 여부 및 후보 선택 조건을 확인한 뒤
  새로운 어노테이션으로 대체할 수 있다.
- 기존 @Configuration 클래스에 선언된 @MockBean과
  @SpyBean은 단순 치환만으로 동일하게 동작하지 않는다.
- @MockitoBean은 테스트 클래스에서 Bean을 Mock으로
  대체하거나 등록한다.
- @MockitoSpyBean은 기존 Bean 인스턴스를 Spy로 감싼다.
- 기존 @SpyBean은 대상 Bean이 없으면 새로 등록하지만,
  @MockitoSpyBean은 기존 대상 Bean이 없으면 실패한다.
- @MockitoSpyBean은 대상 Bean을 하나로 특정할 수 있어야 한다.
  후보가 여러 개라면 @Qualifier 또는 name 속성으로
  대상을 명확히 지정할 수 있다.