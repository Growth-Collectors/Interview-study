# DI와 IoC

질문
- 실제 스프링 사용하면서 DI로 인해 뭔가 이점을 본적이 있는지?

# DI(Dependency Injection)

`DI(Dependency Injection)`란 **스프링이 다른 프레임워크와 차별화되어 제공하는 의존 관계 주입 기능**으로,**객체를 직접 생성하는 게 아니라 외부에서 생성한 후 주입 시켜주는 방식**이다.

**DI(의존성 주입)를 통해서 모듈 간의 결합도가 낮아지고 유연성이 높아진다.**

![https://velog.velcdn.com/images%2Fgillog%2Fpost%2F08489bda-549e-4dae-851b-8ae1734bf85e%2F21373937580AEF9B37.jpg](https://velog.velcdn.com/images%2Fgillog%2Fpost%2F08489bda-549e-4dae-851b-8ae1734bf85e%2F21373937580AEF9B37.jpg)

방법 1. 은 A객체가 B와 C객체를 New 생성자를 통해서 직접 생성하는 방법이고,

방법 2. 는 **외부에서 생성 된 객체를 setter()를 통해 사용하는 방법**이다.

이러한 두번째 방식이 **의존성 주입의 예시**인데, `A 객체`에서 **`B, C객체`를 사용(의존)할 때** `A 객체`에서 **직접 생성 하는 것이 아니라** **`외부(IOC컨테이너)`에서 생성된 `B, C객체`를 조립(주입)시켜 `setter` 혹은 `생성자`를 통해 사용하는 방식**이다.

![https://velog.velcdn.com/images%2Fgillog%2Fpost%2F41f2eb24-fce2-4b7e-b9ac-d5c3ce97d213%2F22535642580C4AF12C.jpg](https://velog.velcdn.com/images%2Fgillog%2Fpost%2F41f2eb24-fce2-4b7e-b9ac-d5c3ce97d213%2F22535642580C4AF12C.jpg)

**스프링에서는 객체를** `Bean`이라고 부르며, 프로젝트가 실행될때 사용자가 Bean으로 관리하는 객체들의 생성과 소멸에 관련된 작업을 자동적으로 수행해주는데 객체가 생성되는 곳을 스프링에서는 **Bean 컨테이너라**고 부른다.

[[Spring] DI, IoC 정리](https://velog.io/@gillog/Spring-DIDependency-Injection)

# Ioc(Inversion of Control)

`IoC(Inversion of Control)`란 "**제어의 역전**" 이라는 의미

**프로그램의 제어 흐름(메소드나 객체의 호출작업)을 개발자가 결정하는 것이 아니라, 외부에서 관리하는 것을 의미**한다.

`IoC`는 **제어의 역전**이라고 말하며, 간단히 말해 "제어의 흐름을 바꾼다"라고 한다.

객체의 **의존성을 역전시켜 객체 간의 결합도를 줄이고 유연한 코드를 작성**할 수 있게 하여 **가독성 및 코드 중복, 유지 보수를 편하게** 할 수 있게 한다.

**기존 객체가 만들어지고 실행되는 순서**

1. 객체 생성
2. 의존성 객체 생성 **클래스 내부에서 생성**
3. 의존성 객체 메소드 호출

**스프링에서 객체가 만들어지고 실행되는 순서**

1. 객체 생성
2. 의존성 객체 주입 스스로가 만드는것이 아니라 **제어권을 스프링에게 위임하여 스프링이 만들어놓은 객체를 주입**한다.
3. 의존성 객체 메소드 호출

**스프링이 모든 의존성 객체를 스프링이 실행될때 다 만들어주고 필요한곳에 주입**시켜줌으로써 **Bean들은 `싱글턴 패턴`의 특징**을 가지며,

**제어의 흐름을 사용자가 컨트롤 하는 것이 아니라 스프링에게 맡겨 작업을 처리**하게 된다.

---

[DI와 IoC 관련 유투브 영상 추천](https://www.youtube.com/watch?v=8lp_nHicYd4)

### IOC 가 왜 필요할까?

- **객체지향 원칙**을 잘 지키기 위해.
- 역할과 관심을 분리해 **응집도를 높이고 결합도를 낮추며**, 이에 따라 변경에 **유연한 코드**를 작성할 수 있는 구조가 될 수 있기 때문
- **in GoF** 디자인 패턴에서는 할리우드 법칙으로도 불린다.
    
    연락하지마라 알아서 불러줄게 Don’t call us, we’ll call you
    

### **DIP: 의존 역전 원칙 Dependency Inversion Principle**

- **상위 레벨의 모듈은 절대 하위 레벨 모듈에 의존하지 않는다.**
- (상위, 하위레벨 모듈) 둘 다 **추상화에 의존**해야 한다.

### **IoC & DIP의 목적**

- **클래스 간 결합을 느슨히** 하기 위함
- 한 클래스의 **변경**에 따른 다른 클래스들의 영향을 최소화

→ 애플리케이션을 **지속가능하고 확장성** 있게 만든다. 

시너지가 높은 **IoC & DIP** 를 함께 쓰도록 권장되고 있다.

### **DI: 의존성 주입 Dependency Injection**

- IoC를 달성하는 **디자인패턴** 중 하나
- 의존성: 클래스 간에 의존 관계가 있다는 것
    
    = 한 클래스가 바뀔 때 다른 클래스가 영향을 받는다는 것
    
- 의존성 주입: 의존성을 다른 곳으로부터 주입해주는 것
- **의존성 주입 패턴**
    1. **생성자 주입**
    2. Setter 주입
    3. Interface 주입
- 의존성 분리
    - 상위계층이 하위계층에 의존하는 상황을 Interface를 이용해 반전시켜 하위계층의 구현으로부터 독립시킨다
    

![444](https://user-images.githubusercontent.com/64303211/196603635-784d66ea-c54d-4c1a-9526-f0a770a38a2c.png)

![22222](https://user-images.githubusercontent.com/64303211/196603671-e187cd5b-0a54-426e-919f-f388b1729c77.png)



### **Spring DI - 자동 주입**

- 스프링 빈으로 등록되면 스프링이 자동으로 의존성 주입해준다.
- DI를 적용할 때 DIP를 따른다.

### Spring에서 의존성을 주입하는 방법 3가지

1. **생성자 주입 (Constructor Injection)**
    - 생성자를 통해 생성
    - **스프링 공식 추천 방법**
    - **NullPointerException 방지** 가능 (객체 생성 시점에 모든 의존성 주입하기 때문)
    - **객체의 불변성**
        - 생성자 주입의 경우에는 객체를 할당하려면 생성자밖에 방법이 없다.
        - setter의 경우에는 객체 생성 후에도 언제든지 의존성 주입이 가능 → 의존 관계의 변경이 필요하다면 setter가 필요할 수 있음
        - **변경이 필요없도록 설계하는 것이 좋다**
    - **순환참조 문제** 방지 가능
2. 필드 주입 (Field Injection) - 문제점 존재
    - @Autowired 어노테이션이 달린 필드 주입
    - 직접 의존성 넣어줄 수 없다, 의존성이 framework에 강하게 종속됨
    - intelliJ가 경고해주기까지 함
3. setter 주입
    - @Autowired 어노테이션이 달린 메서드 호출

### **의존성 주입 방법을 동시에 여러개 사용한다면?**

Spring이 어느 것을 이용해서 주입시킬까

- 호출 순서 **: 생성자 → 필드 → setter**

### 의존성을 주입하는 두 가지 방식

작업을 수행하는 쪽에서 객체를 직접 생성하는 것이 아닌, 주입받는 방식을 통해 제어 흐름 구조를 바꾸는 것


**DL (Dependency Lookup) : 의존성 검색**

- 저장소에 저장되어 있는 Bean에 접근하기 위해 개발자들이 컨테이너에서 제공하는 API를 이용해서 Bean을 검색하는 방식

**DI (Dependency Injection) : 의존성 주입**

- 클래스에서 필요한 의존 객체를 컨테이너가 자동으로 제공해주는 방식
- Bean 설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는 방식 (DL의 컨테이너 의존성을 줄인 방식)
- DI 기반으로 개발을 하면 특정 인터페이스 규격만 맞추면 다양한 형태의 결과 생성 가능

### **IOC 용어**

**Bean** 

- Spring이 제공하는 Container를 통해서 관리되는 인스턴스

**Bean Factory** 

- 스프링 IOC를 담당하는 컨테이너

- Bean 등록, 생성, 조회, 반환, 관리 담당 Bean Factory를 바로 사용하지 않고 이를 확장한 application context 를 이용

**Application context** 

- Bean Factory를 확장한 IOC 컨테이너 (Bean Factory보다 부가 기능 제공)

Configuration metadata 

- application context, bean factory 가 Bean 생성하고, 주입하는데 필요한 설정 정보를 보유

Container 

- IOC 방식으로 Bean을 관리하는 의미에서 Bean Factory, application context를 의미

- 객체를 관리하는 컨테이너, 컨테이너에서 객체를 담아 두고, 필요할 때 컨테이너로부터 객체를 가져와 사용할 수 있음

[출처]


[의존성을 주입하는 방식 @Autowired, @Resource, property 
비교](https://lkhlkh23.tistory.com/72

[[면접준비] IOC / DI](https://lkhlkh23.tistory.com/124?category=833712)

[[Spring] 생성자 주입 vs 필드 주입 vs 수정자 주입](https://yeonyeon.tistory.com/m/220)
[[Spring] DI, IoC 정리](https://velog.io/@gillog/Spring-DIDependency-Injection)

[DI와 IoC 관련 유투브 영상 추천](https://www.youtube.com/watch?v=8lp_nHicYd4)

----
태그: spring

날짜: 2022/10/13

자료조사: Yerim, Jioo Jung
