# Log4j
Log4j는 크게 Logger, Appender, Layout 3가지 요소로 구성되어 있다.

## Log Level
- FATAL : 치명적인 에러
- ERROR : 에러
- WARN : 경고
- INFO : 정보
- DEBUG : 상세정보

## Appender
- ConsoleAppender
  - `org.apache.log4j.ConsoleAppender
  - 콘솔에 로그 메시지 출력
- FileAppender
  - org.apache.log4j.FileAppender
  - 파일에 로그 메시지 기록
- RollingFileAppender
  - org.apache.log4j.rolling.RollingFileAppender
  - 파일 크기가 일정 수준 이상이 되면 기존 파일을 백업파일로 바꾸고 처음부터 기록
- DailyRollingFileAppender
  - org.apache.log4j.DailyRollingFileAppender
  - 일정 기간 단위로 로그 파일을 생성하고 기록
- JDBCAppender
  - org.apache.log4j.jdbc.JDBCAppender
  - DB에 로그를 출력. 하위에 Driver, URL, User, Password, Sql과 같은 parameter를 정의할 수 있음
- SMTPAppender
  - 로그 메시지를 이메일로 전송
- NTEventAppender
  - 윈도우 시스템 이벤트 로그로 메시지 전송

## ConversionPattern
- %d : 로깅 이벤트가 발생한 시간을 기록
  - %d{HH:mm:ss, SSS} , %d{yyyy MMM dd HH:mm:ss, SSS}같은 형태로 사용
  - SimpleDateFormat에 따른 포맷팅을 하면 된다.
    - %d{ABSOLUTE}
    - %d{DATE}
    - %d{ISO8601}
- %p : debug, info, warn, error, fatal 등의 로깅레벨이 출력된다.
- %t : 로깅 이벤트가 발생된 쓰레드의 이름을 출력한다.
- %% : % 표시를 출력하기 위해 사용한다.
- %C : 클래스명을 표시한다.
  - 클래스 구조가 org.apache.xyz.SomeClass처럼 되어있다면 %C{2}는 xyz.SomeClass가 출력된다(뒤에서부터 선택)
- %F : 로깅이 발생한 클래스 파일명
- %ㅣ : 로깅이 발생한 caller의 정보
- %L : 로깅이 발생한 caller의 라인
- %M : 로깅이 발생한 method명
- %r : 어플리케이션 시작 이후부터 로깅이 발생한 시점의 시간 (milliseconds)
- %x : 로깅이 발생한 thread와 관련된 NDC(nasted diagnostic context)를 출력한다.
- %X : 로깅이 발생한 thread와 관련된 MDC(mapped diagnostic context)를 출력한다.
- %m : 로그내용(코드상 설정한 내용)이 출력된다. 
- %n : 플랫폼 종속적인 개행문자가 출력된다. \r\n 또는 \n일 것이다.

### 공백 패딩
- %5p : 우측 정렬로 로그 레벨을 남김. 로그레벨이 5글자가 안되면 왼쪽에 공백을 추가해 5글자 맞춘다.
- %-5p : 좌측 정렬

## Configuration
- 설정 파일은 xml 또는 properties(key=value) 형식으로 작성할 수 있다.
### 1. Java 소스에 직접 경로 설정
`PropertyConfigurator.configure("파일 패스"); `
```java
import com.foo.Bar;

 import org.apache.log4j.Logger;
 import org.apache.log4j.PropertyConfigurator;

 public class MyApp {

   static Logger logger = Logger.getLogger(MyApp.class.getName());

   public static void main(String[] args) {


     // BasicConfigurator replaced with PropertyConfigurator.
     PropertyConfigurator.configure(args[0]);

     logger.info("Entering application.");
     Bar bar = new Bar();
     bar.doIt();
     logger.info("Exiting application.");
   }
 }
```

```properties
# Set root logger level to DEBUG and its only appender to A1.
log4j.rootLogger=DEBUG, A1

# A1 is set to be a ConsoleAppender.
log4j.appender.A1=org.apache.log4j.ConsoleAppender

# A1 uses PatternLayout.
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%-4r [%t] %-5p %c %x - %m%n
```

```properties
log4j.rootLogger=debug, stdout, R

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout

# Pattern to output the caller's file name and line number.
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] (%F:%L) - %m%n

log4j.appender.R=org.apache.log4j.RollingFileAppender
log4j.appender.R.File=example.log

log4j.appender.R.MaxFileSize=100KB
# Keep one backup file
log4j.appender.R.MaxBackupIndex=1

log4j.appender.R.layout=org.apache.log4j.PatternLayout
log4j.appender.R.layout.ConversionPattern=%p %t %c - %m%n
```


### 2. 어플리케이션 실행시 Java 옵션에 추가
`-Dlog4j.configuration=file:/c:/foobar.properties`

```
java -Dlog4j.configuration=file:/c:foobar.properties  -jar foobar.jar
```


# 참고자료
- http://logging.apache.org/log4j/1.2/manual.html
- https://kwonnam.pe.kr/wiki/java/log4j/pattern
- https://beomhh.tistory.com/3