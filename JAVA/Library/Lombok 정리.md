> **처음 배우는 스프링 부트2** 책을 읽고 정리한 내용입니다.

# Lombok
자바 컴파일 시점에 특정 어노테이션에 해당하는 코드를 추가/변경하는 라이브러리

- 장점
  - 코드 경량화
  - 가독성

## @Getter와 @Setter

```java
public class Human {
    private String name;
    private int age = 0;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAge() {
        return age;
    }

    public void setAge() {
        this.age = age;
    }
}
```

```java
@Getter @Setter
public class Human {
    private String name;
    private int age = 0;
}
```

## @EqualsAndHashCode
- 자바의 `equals()` 메서드와 `Hashcode()` 메서드를 구현

```java
@Getter @Setter
public class Human {
    private String name;
    private int age = 0;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Human human = (Human) o;
        return age == human.age && Object.equals(name, human.name); 
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

```java
@Getter @Setter
@EqualsAndHashCode
public class Human {
    private String name;
    private int age = 0;
}
```

```java
@Getter @Setter
@EqualsAndHashCode(of = {"name"})
public class Human {
    private String name;
    private int age = 0;
}
```

## @AllArgsConstructor, @NoArgsConstructor, @RequiredArgsConstructor
생성자를 만드는 어노테이션

### @AllArgsConstructor
- 객체의 모든 필드값을 인자로 받는 생성자를 만든다.
 
```java
@Getter @Setter
@AllArgsConstructor
public class Human {
    private String name;
    private int age = 0;

    public Human(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

```java
@Getter @Setter
@AllArgsConstructor
public class Human {
    private String name;
    private int age = 0;
}
```

### @NoArgsConstructor
- 기본 생성자를 만든다.
- 자바에서 인자값이 존재하는 생성자를 하나라도 추가했으면 기본 생성자를 수동으로 추가해줘야 한다.

```java
@Getter @Setter
@AllArgsConstructor
@NoArgsConstructor
public class Human {
    private String name;
    private int age = 0;
}
```

### @RequiredArgsConstructor
- `@NonNull`이 적용된 필드값만 인자로 받는 생성자를 만든다.

```java
@Getter @Setter
@AllArgsConstructor
@NoArgsConstructor
@RequiredArgsConstuctor
public class Human {

    @NonNull
    private String name;

    private int age = 0;
}
```

## @Data
- `@ToString`, `@EqualsAndHashCode`, `@Getter`, `@Setter`, `@RequiredArgsConstructor` 어노테이션을 합쳐놓은 어노테이션

```java
@Getter @Setter
@EqaulsAndHashCode
@AllArgsConstructor
@NoArgsConstructor
@RequiredArgsConstuctor
public class Human {

    @NonNull
    private String name;

    private int age = 0;
}
```

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Human {

    @NonNull
    private String name;

    private int age = 0;
}
```

## @Builder

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Human {
    
    private String name;
    private int age = 0;

    public static HumanBuilder builder() {
        return new HumanBuilder();
    }

    public static class HumanBuilder d{
        private String name;
        private int age;

        private HumanBuilder() {

        }

        public HumanBuilder name(String name) {
            this.name = name;
            return this;
        }

        public HumanBuilder age(int age) {
            this.age = age;
            return this;
        }

        @Override
        public String toString() {
            return "HumanBuilder(name = " + name + ", age = " + age + ")";
        }

        public Human build() {
            return new Human(name, age);
        }
    }
}
```

```java
@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class Human {
    
    @NonNull
    private String name;
    private int age = 0;
}
```

# 참고자료
- [처음 배우는 스프링 부트2](https://book.naver.com/bookdb/book_detail.nhn?bid=14031681)
