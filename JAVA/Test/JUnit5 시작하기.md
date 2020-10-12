# JUnit5
> 더 자바, 애플리케이션을 테스트하는 다양한 방법을 듣고 정리한 내용입니다.

- 자바 개발자가 가장 많이 사용하는 테스팅 프레임워크
- Java8 이상
- Spring Boot 2.2+에서 Junit 버전을 5로 함

**JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage**

Platform : 테스트를 실행해주는 런처 제공. TestEngine API 제공
Jupiter : TestEngine API 구현체로 JUnit5를 제공
Vintage : JUnit 4와 3을 지원하는 TestEngine 구현체

## JUnit5 시작하기
Junit5부터는 테스트 메서드에 public을 안 붙여도 된다.

### 기본 애노테이션
- `@Test`
- `@BeforeAll` / `@AfterAll`
  - 모든 테스트가 실행되기 전(@BeforeAll), 후(@AfterAll) 한 번만 호출된다.
  - 반드시 static 메서드를 사용해야 한다. private 안됨
  - 리턴 타입은 void여야 함
- `@BeforeEach` / `@AfterEach`
  - 각각의 테스트가 실행되기 전(@BeforeEach), 후(@AfterEach)에 호출된다.
- `@Disabled` 
  - 실행하고 싶지 않은 테스트에 마킹한다.

```java
// 모든 테스트가 실행되기 전 한 번만 호출
// @BeforeClass
@BeforeAll
static void beforeAll() {
    System.out.println("before All");
}

// 모든 테스트가 실행된 이후 한 번만 호출된다.
@AfterAll
static void afterAll() {
    System.out.println("after all")
}
```

```java
// 각각의 테스트가 실행되기 전에 호출된다.
// @Before
@BeforeEach
void beforeEach() {
    System.out.println("")

}

// 각각의 테스트가 실행된 후 호출된다.
// @After
@AfterEach
void afterEach() {
    
}
```

```java

// @Ignored
@Test
@Disabled
void create() {

}
```

## JUnit 5 테스트 이름 표기하기
기본 표기 전략 -> 메서드이름

### @DisplayNameGeneration
- Method와 Class 레퍼런스를 사용해서 테스트 이름을 표기하는 방법 설정
- 기본 구현체로 ReplaceUnderscroe 제공

- `DisplasyNameGenerator.Standard`
- `DisplayNameGenerator.Simple`
- `DisplayNameGenerator.ReplaceUnderscores` 
- `DisplayNameGenerator.IndicativeSentences`

```java
@DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscore.class)
class StudyTest {

}
```

### @DisplayName
- 어떤 테스트인지 테스트 이름을 보다 쉽게 표현할 수 있는 방법을 제공하는 애노테이션
- `@DisplayNameGeneration`보다 우선순위가 높다
- Emoji 사용 가능

```java
@DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscore.class)
class StudyTest {

    @Test
    @DisplayName("스터디 만들기")
    void create_new_study() {
        Study study = new Study();
        assertNotNull(study);
        System.out.println("create");
    }
}
```

- `@DisplayName` 권장
- 커서를 메서드에 두고 ctrl + shift + R을 누르고 테스트하면 특정 메서드만 테스트함. (intelij)

# 참고자료
- [더 자바, 애플리케이션을 테스트 하는 다양한 방법](https://www.inflearn.com/course/the-java-application-test) 