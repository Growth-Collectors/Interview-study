# AOP

tag : spring

---
### 배경 지식

- 스프링의 3대 기반 기술 : AOP, IoC/DI, 서비스 추상화
- 학습 내용 : AOP 등장 배경 , 도입 이유, 장점
- 키워드 : 트랜잭션 서비스 추상화, 프록시와 데코레이터 패턴,

### AOP 등장 배경

- 컴퓨터 프로그래밍 패러다임 변화
    - *절차적 프로그래밍 → 객체 지향 프로그래밍(OOP) → 관점 지향 프로그래밍(AOP)*
- **OOP의 관심사 분리에 대한 한계적인 부분을 해결하고자 AOP가 등장**
- 요즘 웹 애플리케이션을 개발할 때 해킹에 대비한 보안 기능 구현은 필수가 되고 있음
- 로깅 기능을 적용해 사용자의 접속 내역을 로그로 기록함
- 트랜잭션, 예외 처리, 이메일 통보 기능처럼 모든 웹 애플리케이션에서 공통으로 사용하는 기능에 적용
- 주기능을 추가할 때마다 공통 기능을 일일이 구현해야 한다면 배보다 배꼽이 더 큰 결과를 초래하게 됨
⇒ 스프링에서는 관점 지향 프로그래밍(AOP; Aspect Oriented Programming)으로 해결할 수 있음

# AOP

<aside>
💡 **Aspect Oriented Programming**의 약자로 관점 지향 프로그래밍이라고 불린다.

**여러 오브젝트에 나타나는 공통적인 부가기능을 모듈화하여 재사용하는 기법이다.**

</aside>

### AOP 상세 내용

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8a3c38dd-bd2a-49f3-a6b8-7191d2f747ea/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221018T062104Z&X-Amz-Expires=86400&X-Amz-Signature=f30d56fb569f8930bc7c64aaecc55f6a36b3f1b33db7bfdb203c3d24275f362a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

관점 지향은 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 모듈화 하겠다는 것이다. 다음 그림처럼 **AOP는 이 횡단 관심사 코드를 모듈화(Aspect)하여 관리**한다. 따라서 더 이상 기존 비즈니스 코드에 횡단 관심사 코드를 작성하지 않아도 된다.

**관심사의 분리 (Separation of Concerns)**

- 횡단 관심사(Cross-cutting Concerns) : 공통적 부가기능 코드 (트랜잭션/로그/보안/인증 처리 등)
- 핵심 관심사(Core Concerns) : 업무 로직 코드

- 관심사 분리의 구체적인 예시
    ![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e0ff815d-2251-4e43-8b52-d34584d8270e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221018T062243Z&X-Amz-Expires=86400&X-Amz-Signature=eef1b82ef8bf9b63d3a517f2f30af0f1ebb1f5d9182224349514a7f71ab9d406&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)
    
    소스 코드상에서 계속 반복해서 사용되는 부분들을 흩어진 관심사(Crosscutting Concerns)라고 한다. AOP는 흩어진 관심사(Crosscutting Concerns)를 모듈화 하는 기법이다.
    
    클래스 A, B, C에서 공통적으로 나타나는 색깔 블록은 중복되는 메서드, 필드, 코드 등이다. 이때 예를 들어 클래스 A의 주황색 블록을 수정한다면 클래스 B, C의 주황색 부분도 일일이 찾아 수정해야 한다. 따라서 주황색, 파란색, 빨간색 블록처럼 모듈화 시켜놓고 어디에 적용시킬지만 정의해줌으로써 해결한다. 이때 모듈화 시켜 놓은 블럭을 Aspect라고 한다.
    
    ![토비의 스프링 vol.1 p.505](https://user-images.githubusercontent.com/69442847/196351827-208411e4-2d82-47c5-a7d6-7bce4ada821f.png)

    
    토비의 스프링 vol.1 p.505
    

---

## AOP 주요 개념

**▶  Aspect**

AOP의 기본 모듈

부가기능 모듈화 작업을 기존의 객체 지향에서 사용하던 Object와 구분하기 위해 Aspect라고 명명되었다.

흩어진 관심사(Crosscutting Concerns)를 묶어서 모듈화 한 것. (하나의 모듈)

핵심 기능을 담고 있지는 않고, 부가됨으로써 의미를 갖는 특별한 **모듈**이다.

**Advice**(부가기능이 정의된 코드) + **Point Cut**(Advice를 적용할지 결정함)의 조합

**▶  Target**

Aspect가 가지고 있는 Advice가 적용되는 대상(클래스, 메서드 등)을 말한다.

**▶  Advice**

실질적인 부가기능을 담은 구현체, 어떤 일을 해야할 지에 대한 것.

**▶  Join Point**

가장 흔한 Join Point는 **메서드 실행 시점**이다.
Advice가 적용될 위치, 끼어들 수 있는 지점, 생성자 호출 직전, 생성자 호출 시, 필드에 접근하기 전, 필드에서 값을 가져갔을 때 등

**▶  Point Cut**

Join Point의 상세한 스펙을 정의한 것. Advice를 어디에 적용해야 하는지에 대한 정보를 가지고 있다. 
“A 클래스에 B 메서드를 적용할 때 호출을 해라.”와 같은 구체적인 정보를 준다.

## AOP 적용 방법

**▶  컴파일 타임에 적용**

컴파일 시점(.java 파일을 .class 파일로 만들 때)에 바이트 코드를 조작하여 조작된(AOP가 적용된) 바이트코드를 생성.

**▶  로드 타임에 적용**

순수하게 컴파일한 뒤, 클래스를 로딩하는 시점에 클래스 정보를 변경(Load Time Weaving, 로드타임 위빙).

**▶  런타임에 적용**

**스프링 AOP가 사용하는 방법**. A 클래스 타입의 Bean을 만들 때 A 타입의 Proxy Bean을 만들어 Proxy Bean이 Aspect 코드를 추가하여 동작.

## **AOP의 장점**

- 어플리케이션 전체에 흩어진 공통기능이 하나의 장소에서 관리됨
- 다른 Service 모듈들이 본인의 목적에만 충실하고 그 외 사항들은 신경 쓰지 않아도 됨
- 핵심 로직의 유지보수가 쉬워짐
- 부가기능을 모듈로 분리함으로써 핵심 로직들이 OOP의 가치를 추구하도록 도움 (배타적X)

---

# 스프링 AOP

### 스프링 AOP의 특징

- 스프링은 IoC/DI 컨테이너와 다이내믹 프록시, 데코레이터 패턴, 프록시 패턴, 빈 오브젝트의 후처리 조작 기법 등을 조합해 AOP를 지원한다.
- **스프링 AOP는 프록시 기반**이다. 프록시로 만들어서 DI로 연결된 빈 사이에 적용해 타깃의 메소드 호출 과정에서 부가기능을 제공하도록 한다. 프록시 객체를 사용하는 이유는 접근 제어 및 부가 기능을 추가하기 위해서이다.
- 스프링 빈에만 AOP를 적용할 수 있다.
- 모든 AOP 기능을 제공하는 것이 목적이 아니라, 스프링 IoC와 연동하여 엔터프라이즈 애플리케이션에서 가장 흔한 문제(중복코드, 프록시 클래스 작성의 번거로움, 객체 간 관계 복잡도 증가 등등…)를 해결하기 위한 솔루션을 제공하는 것이 목적.

### 스프링의 **AOP 어노테이션**

1. @Component
    - 컴포넌트 어노테이션을 명시해 스프링 컨테이너가 객체 생성하도록 한다.
2. @Aspect
    - 스프링 컨테이너에 AOP 담당 객체임을 알린다.
3. @Around
    - 횡단 관심사항의 대상 지정과 적용 시점을 지정한다. (advice, pointcut)
    
    **@Around("within(bit.your.prj.controller.*)")**
    
    - Advice : 횡단 관심사항 적용 시점 ( ex. 메서드 실행 전, 후, 리 턴 시, 예외 발생 시)
    - pointcut : 횡단 관심사항 적용 대상 지정 (execution, within, bean)
    - 

### Target 메서드의 Aspect 실행 시점을 지정할 수 있는 어노테이션

- @Before (이전) : 어드바이스 타겟 메소드가 호출되기 전에 어드바이스 기능을 수행
- @After (이후) : 타겟 메소드의 결과에 관계없이(성공, 예외 관계없이) 타겟 메소드가 완료되면 어드바이스 기능을 수행
- @AfterReturning (정상적 반환 이후)타겟 메소드가 성공적으로 결과값을 반환 후에 어드바이스 기능을 수행
- @AfterThrowing (예외 발생 이후) 타겟 메소드가 수행 중 예외를 던지게 되면 어드바이스 기능을 수행
- @Around (메소드 실행 전후) : 어드바이스가 타겟 메소드를 감싸서 타겟 메소드 호출 전과 후에 어드바이스 기능을 수행

# 프록시 기반 AOP

### 프록시 패턴

Spring AOP는 **프록시 패턴**이라는 디자인 패턴을 사용해서 AOP 효과를 낸다.

**프록시 패턴을 사용하면 어떤 기능을 추가하려 할때 기존 코드를 변경하지 않고 기능을 추가**할수 있다.

어떤 클래스가 Spring AOP의 대상이라면 그 **기존 클래스의 빈이 만들어질때 Spring AOP가 프록시(기능이 추가된 클래스)를 자동으로 만들고 원본 클래스 대신 프록시를 빈으로 등록**한다.

그리고 원본 클래스가 사용되는 지점에서 프록시를 대신 사용한다.

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9b1dd301-f955-4d9d-bfda-ce5207a16f7f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221018T062658Z&X-Amz-Expires=86400&X-Amz-Signature=ebc919664cb421d36c1594683a2a22ceb639ef7a7c1e105d39dff6169d76c367&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

프록시 패턴에는 **interface**가 존재하고 **Client**는 이 interface 타입으로 Proxy 객체를 사용하게 된다.

Proxy 객체는 기존의 타겟 객체(Real Subject)를 참조하고 있다. Proxy 객체와 Real Subject의 타입은 같고, Proxy는 원래 해야 할 일을 가지고 있는 Real Subject를 감싸서 Client의 요청을 처리한다.

### 왜? 프록시 패턴을 사용하는 걸까?

그 이유는 **기존 코드의 변경 없이 접근 제어 또는 부가 기능 추가**를 위해서다.

## Proxy 형태로 동작하는 @Transactional

**AOP 기능 중 가장 많이 활용되는 `@Transactional`**

트랜잭션 처리를 위한 `@Transactional` 애노테이션은 **Spring AOP의 대표적인 예**이다. `@Transactional` 역시 **Proxy 형태로 동작**한다. (Spring은 JDK Proxy, Spring Boot는 CGLIb Proxy를 기본으로 하기 때문에, 사용하는 것에 따라 생성된 프록시 객체 형태는 다를 수 있다.)

![https://velog.velcdn.com/images/ann0905/post/56a48b12-b2d0-4071-b09e-959e585551bb/image.png](https://velog.velcdn.com/images/ann0905/post/56a48b12-b2d0-4071-b09e-959e585551bb/image.png)

1. **target에 대한 호출**이 들어오면 **AOP proxy**가 이를 가로채서(intercept) 가져온다.
2. AOP proxy에서 Transaction Advisor가 commit 또는 rollback 등의 트랜잭션 처리를 한다.
3. 트랜잭션 처리 외에 다른 부가 기능이 있을 경우 해당 **Custom Advisor**에서 그 처리를 한다.
4. 각 Advisor에서 부가 기능 처리를 마치면 Target Method를 수행한다.
5. interceptor chain을 따라 caller에게 결과를 다시 전달한다.

# ⭐️ 결론

- AOP는 흩어진 관심사를 별도의 클래스로 모듈화하는 프로그래밍 방법을 말하며, OOP를 더욱 잘 지킬 수 있도록 도움을 준다.
- Spring AOP는 프록시 객체를 자동으로 생성해주어, Aspect/Advice에 직접적으로 의존하지 않게 해준다.
- `@Transactional` 도 Spring AOP 중 하나로 프록시 방식으로 동작한다.
    - **원본 클래스/인터페이스를 상속 받아 프록시를 생성**하기 때문에 **접근 제어**가 private으로 되어 있으면 안된다.
    - 객체 외부에서 처음으로 진입하는 메서드에 트랜잭션 처리가 되어 있어야, 해당 요청을 프록시 객체가 대신 처리할 수 있다. 따라서 **트랜잭션은 객체 외부에서 처음 진입하는 메서드를 기준으로 동작**한다.

---

- 출처

코드 예시가 잘 나와있음

[스프링 AOP 총정리 : 개념, 프록시 기반 AOP, @AOP :: 개발자 한선우](https://yadon079.github.io/2021/spring/spring-aop-core)

활용

[개발자 곁으로 성큼 다가온 AOP 실무에 활용하기](https://dataonair.or.kr/db-tech-reference/d-lounge/technical-data/?mod=document&uid=235236)

[[Spring] AOP(Aspect Oriented Programming)란? 스프링 AOP란?](https://code-lab1.tistory.com/193)

[](https://goodteacher.tistory.com/415)

[AOP와 @Transactional의 동작 원리](https://velog.io/@ann0905/AOP%EC%99%80-Transactional%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC)

[AOP가 왜 필요할까? | Moon](https://gmoon92.github.io/spring/aop/2019/02/09/why-used-aop.html)

---
스터디 날짜: 2022/10/06

자료조사: [@jioome](https://github.com/jioome), [@namnameeroo](https://github.com/namnameeroo)
