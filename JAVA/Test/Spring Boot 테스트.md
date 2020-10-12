# 스프링 부트 테스트

> 처음 배우는 스프링 부트2를 읽고 정리한 내용입니다.

- `@SpringBootTest`
- `@WebMvcTest`
- `@DataJpaTest`
- `@RestClientTest`
- `@JsonTest`

## @SpringBootTest
- 통합 테스트를 제공하는 기본적인 스프링 부트 어노테이션
- 스프링 부트 1.4 버전부터 제공
- 실제 구동되는 애플리케이션과 똑같이 애플리케이션 컨텍스트를 로드하여 테스트한다.
  - 애플리케이션에 설정된 모든 빈을 로드한다 => 애플리케이션 규모가 클수록 속도가 느려진다. 

```java
@SpringBootTest(value = "value=test", properties = { "property.value=propertyTest" }, classes = {
		SpringBootTestApplication.class }, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class SpringBootTestApplicaionTests {

}
```
- value : 테스트가 실행되기 전에 적용할 프로퍼티를 주입시킬 수 있다. 즉, 기존의 프로퍼티를 오버라이드한다.
- properties : 테스트가 실행되기 전에 {key=value} 형식으로 프로퍼티를 추가할 수 있다.
- classes : 애플리케이션 컨텍스트에 로드할 클래스를 지정할 수 있다. 따로 지정하지 않으면 @SpringBootConfiguration을 찾아서 로드한다.
- webEnvironment : 애플리케이션이 실행될 때의 웹 환경을 설정할 수 있다. 기본값은 Mock 서블릿을 로드하여 구동된다.


**프로파일 환경(개발, QA, 운영 환경)마다 다른 데이터소스(DataSource)를 갖는다면 어떻게 해야 할까?**
- `@ActiveProfiles("local")`과 같은 방식으로 원하는 프로파일 환경값을 부여하면 된다.


## @WebMvcTest
- MVC를 위한 테스트. 웹에서 테스트하기 힘든 컨트롤러를 테스트하는데 적합하다.
- 웹 상에서 요청과 응답에 대해 테스트할 수 있다.
- `@WebMvcTest` 어노테이션을 사용하면 MVC 관련 설정인 `@Controller`, `@ControllerAdvice`, `@JsonComponent`와 Filter, WebMvcConfigurer, HandlerMethodArgumentResolver만 로드되기 때문에 `@SpringBootTest` 어노테이션보다 가볍게 테스트할 수 있다.


## @DataJpaTest
- JPA 관련 테스트 설정만 로드한다. 
- 데이터소스(DataSource)의 설정이 정상적인지, JPA를 사용하여 데이터를 제대로 생성, 수정, 삭제하는지 등의 테스트가 가능하다.
- 내장형 데이터베이스를 사용해 실제 데이터베이스를 사용하지 않고 테스트 데이터베이스로 테스트할 수 있다.

`@DateJpaTest`는 기본적으로 인메모리 임베디드 데이터베이스를 사용하며, @Entity 클래스를 스캔하여 스프링 데이터 JPA 저장소를 구성한다. 

- EntityManager의 대체재로 만들어진 테스트용 TestEntityManager를 사용하면 persist, flush, find 등과 같은 기본적인 JPA 테스트를 할 수 있다. 

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@BootstrapWith(DataJpaTestContextBootstrapper.class)
@ExtendWith(SpringExtension.class)
@OverrideAutoConfiguration(enabled = false)
@TypeExcludeFilters(DataJpaTypeExcludeFilter.class)
@Transactional
@AutoConfigureCache
@AutoConfigureDataJpa
@AutoConfigureTestDatabase
@AutoConfigureTestEntityManager
@ImportAutoConfiguration
public @interface DataJpaTest {
    //...(생략)
}
```

## @RestClientTest
- REST 관련 테스트를 도와주는 어노테이션
- REST 통신의 데이터형으로 사용되는 JSON 형식이 예상대로 응답을 반환하는지 테스트할 수 있다.

## @JsonTest
- Gson과 Jackson API 테스트를 제공한다.
- Gson => GsonTester
- Jackson => JacksonTester

# 참고자료
- [처음 배우는 스프링 부트2](https://book.naver.com/bookdb/book_detail.nhn?bid=14031681)