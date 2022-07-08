# Mockito

## [Mockito란?](https://site.mockito.org/)

- mocking 프레임워크이다.
- 깔끔하고 간결한 API로 아름다운 테스트를 작성할 수 있다.
- mockito로 만든 코드는 가독성이 뛰어나며 깔끔한 검증 에러를 생성할 수 있다.
- stackoverflow가 뽑은 최고의 Java mocking 프레임워크.
    - Java 프레임워크 10위권 안에 든다고 스스로 뿌듯해함

## Feature

- EasyMock, jMock에 비해 더 간결하며 직관적인 접근법을 사용한다.
    - [EasyMock vs. Mockito](https://github.com/mockito/mockito/wiki/Mockito-vs-EasyMock)
- 원하는 것을 검증할 수 있다
- expect-run-verify 라이브러리는 관련없는 인터렉션들에 대하여 관리해야한다
    - expectation이 assertion과 동일한 취급을 받는다
    - 모든 과정을 stubbing 하여야 한다
- Mockito는 stub-run-assert 구조로 더 test drive하다
    - test-drive함은 테스트 실행 코드가 실행된 뒤에 어떤 상태가 되어야 하는지 검증하는 것이다.
    - 필요하지 않는 부분은 stubing 하지 않아도 된다.
- 인터페이스 뿐만 아닌 구체 클래스도 mocking 가능
- `@Mock` 에노테이션 지원
- stack trace로 실패한 테스트 정보 제공
- 유연한 검증 order
- `exact-number-of-times`와 `at-least-once` 지원
- arugment matcher를 통해 유연한 stubbing, verification
- custom argument matchers 생성 허용

## How to drink

### dependency

현재 4.6.1 까지 지원

```xml
<dependency>
  <groupId>org.mockito</groupId>
  <artifactId>mockito-core</artifactId>
  <version>${mockito.version}/version>
</dependency>
```

### 인터렉션 검증

```java
import static org.mockito.Mockito.*;

// mock 리스트 생성
List mockedList = mock(List.class);

// mock 객체를 통한 인터렉션
// "unexpected interaction" 예외는 발생하지 않는다
mockedList.add("one");
mockedList.clear();

// 검증
verify(mockedList).add("one");
verify(mockedList).clear();
```

### 함수 호출 stub

```java
// 인터페이스가 아닌 구현 클래스도 모킹 가능
LinkedList mockedList = mock(LinkedList.class);

// 실제 호출 이전 stubbing
when(mockedList.get(0)).thenReturn("first");

// stubbing한 호출에 대해서는 "first" 출력
System.out.println(mockedList.get(0));

// get(999)는 stubbed 되지 않았으므로, "null" 출력
System.out.println(mockedList.get(999));
```

## More Info

- **mock()/@Mock**: mock 객체 생성
    - Optional 하게 [Answer](http://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/stubbing/Answer.html)/[MockSettings](http://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/MockSettings.html) 을 통해 동작 방식을 지정한다
        - Answer
            - thenAnswer 구성을 위해 사용
            - argument에 따른 return 값을 설정하고 싶을때 사용
        - MockSettings
            - mock 생성과 함께 추가적인 설정을 하고싶을 때 사용
            - mockSetting 사용 대신 간단한 테스트를 작성하는것을 권장
            - 기본 answer 설정, 추가 인터페이스 설정, strictness 레벨 등을 설정 가능하다
    - [when()](http://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html#when-T-)/[given()](http://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/BDDMockito.html#given-T-) 을 통해 mock 객체가 어떻게 동작해야하는지 명시한다
- **spy()/@Spy**: 일부만 mocking 한다. 기본적으로 실제 메서드가 호출되지만 stub이나 verify가 가능하다.
- **@InjectMocks**: 자동으로 @Mock, @Spy 필드를 주입한다.
- **verify()**: 명시한 인자로 메서드가 호출되었는지를 확인한다.
    - `any*()`를 통해 유연하게 인자를 매칭할 수 있다.
    - `@Captor` 를 통해 인자를 캡쳐할 수 있다
- BDDMockito 패키지를 통해 BDD 방식으로 테스트를 작성 가능하다.
    - `when` → `given`
    - `verify` → `then`

---

### 참고링크

- [Mockito 공식 홈페이지](https://site.mockito.org/)
- [Mockito Features](https://github.com/mockito/mockito/wiki/Features-And-Motivations)
- [No expect-run-verify](https://szczepiq.wordpress.com/2008/02/01/deathwish/)
- [왜 Mockitor가 더 좋은가(vs. EasyMock)](http://egloos.zum.com/kwon37xi/v/4165915)