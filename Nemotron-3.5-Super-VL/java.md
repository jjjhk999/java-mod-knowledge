# Java Migration Knowledge
- OX 정확도: 80.7% (67/83)
- 근거 포함 정확도: 73.5% (61/83)
## Base64
- `sun.misc.BASE64Encoder`는 Java 9에서 제거되었다.
- `sun.misc.BASE64Decoder`는 Java 9에서 제거되었다.
- 두 클래스 모두 Java 21의 `jdk.unsupported` 모듈에 존재하지 않는다.
- 대체 API인 `java.util.Base64`는 Java 8에서 도입되었다.

## Java EE Tool Modules
- `jdk.xml.ws`는 Java 9, 10에 존재했던 JAX-WS 도구 모듈이다.
- `jdk.xml.bind`는 Java 9, 10에 존재했던 JAXB 도구 모듈이다.
- 두 모듈 모두 Java 11에서 JEP 320에 의해 제거되었다.
- `java.xml.ws`와 `jdk.xml.ws`는 별개의 모듈이다.
- `java.xml.bind`와 `jdk.xml.bind`는 별개의 모듈이다.

## Annotation Processing
- `javax.annotation.processing`은 `java.compiler` 모듈 소속이다.
- `jdk.compiler` 모듈 소속이 아니다.
- Java 21에도 존재한다.

## Security Policy
- `javax.security.auth.Policy`는 과거 표준 Java API였다.
- `javax.security.auth.Policy`는 Java 11에서 제거되었다.
- `java.security.Policy`는 Java 17에서 deprecated for removal 지정되었다.
- `java.security.Policy`는 Java 21에도 존재한다.
- 두 클래스는 서로 다른 API이다.

## Thread API
- `Thread.destroy()`는 Java 11에서 제거되었다.
- `Thread.stop(Throwable)`은 Java 11에서 제거되었다.
- `Thread.stop()`은 Java 21에도 존재한다.
- `Thread.suspend()`는 Java 21에도 존재한다.
- `Thread.resume()`은 Java 21에도 존재한다.
- Java 20~25에서 `Thread.stop()`은 항상 `UnsupportedOperationException`을 발생시킨다.
- Java 20~22에서 `Thread.suspend()`는 항상 `UnsupportedOperationException`을 발생시킨다.
- Java 20~22에서 `Thread.resume()`은 항상 `UnsupportedOperationException`을 발생시킨다.
- 플랫폼 스레드와 가상 스레드 모두 동일하게 적용된다.
- `Thread.suspend()`와 `Thread.resume()`은 Java 23에서 제거되었다.
- `Thread.stop()`은 Java 26에서 제거되었다.

## Applet API
- Applet API는 Java 9에서 deprecated 지정되었다.
- appletviewer와 Java Plug-in은 Java 11에서 제거되었다.
- Applet API 자체는 Java 11에서 제거되지 않았다.
- JEP 398은 Java 17에서 Applet API를 deprecated for removal 지정했다.
- Applet API는 Java 17에서 제거되지 않았다.
- `java.applet.Applet`은 Java 21에도 존재한다.
- `java.applet`은 `java.desktop` 모듈 소속 패키지이다.
- Applet API의 실제 제거 시점은 Java 26(JEP 504)이다.

## Strong Encapsulation
- Java 17에서 JEP 403에 의해 강력한 캡슐화가 적용되었다.
- Java 17부터 `--illegal-access` 옵션은 효력이 없다.
- `--illegal-access=permit`도 접근 제한을 완화하지 않는다.
- 해당 옵션은 Java 21에서 경고만 발생시킨다.
- `--add-exports`와 `--add-opens`는 여전히 사용 가능하다.

## Unsafe API
- `sun.misc.Unsafe` 클래스는 Java 21에도 존재한다.
- `sun.misc.Unsafe.defineAnonymousClass()`는 Java 17에서 제거되었다.
- 해당 메서드는 Java 21에 존재하지 않는다.
- 대체 API는 다음과 같다.
  - `MethodHandles.Lookup.defineHiddenClass()`
  - `MethodHandles.Lookup.defineHiddenClassWithClassData()`

## Distributed Transaction
- `java.transaction` 모듈은 Java 11에서 제거되었다.
- `java.transaction.xa` 모듈은 Java 21에도 존재한다.
- `javax.transaction.xa` 패키지는 Java 21에도 존재한다.
- `javax.transaction.xa.XAResource`는 Java 21에도 존재한다.
- `javax.transaction.xa.XAException`은 Java 21에도 존재한다.
- `javax.transaction.xa.Xid`는 Java 21에도 존재한다.

## Security Manager
- Security Manager는 Java 17에서 deprecated for removal 지정되었다.
- Security Manager는 Java 21에서도 명시적으로 활성화하면 사용 가능하다.
- JVM 시작 시 `-Djava.security.manager`로 기본 Security Manager를 활성화할 수 있다.
- 시작 시 활성화하지 않고 런타임에 `System.setSecurityManager()`로 설치하려면 JVM 시작 옵션 `-Djava.security.manager=allow`가 필요하다. 기본 실행 상태에서는 이 호출이 `UnsupportedOperationException`을 발생시킨다.
- Security Manager는 Java 24에서 JEP 486에 의해 영구 비활성화되었다.
- 영구 비활성화와 API 자체의 제거는 별개의 변경이다.