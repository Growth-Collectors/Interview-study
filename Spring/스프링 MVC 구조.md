# 스프링 MVC 구조, 설명

태그: spring
    

---

# < Servlet 개념 >

## Servlet이란?

**동적 웹 페이지를 만들 때 사용되는 자바 기반의 웹 애플리케이션 프로그래밍 기술로서**
자바 코드 속에 HTML 코드가 들어가는 형태입니다.


### **1. Servlet의 생명 주기**

- 클라이언트 요청이 들어오면 서블릿 컨테이너는 서블릿이 메모리에 있는지 확인한다. 메모리에 없다면 `init()` 메소드를 호출하여 적재한다.
- 클라이언트 요청에 따라서 `service()` 메소드를 통해 요청에 대한 응답이 `doGet()`, `doPost()`로 분기한다.
- 서블릿 컨테이너가 서블릿에 종료 요청을 하면 `destory()` 메소드가 호출된다. 종료 시 처리해야 하는 작업은 `destory()` 메소드를 오버라이딩하여 구현하면된다. `destory()` 메소드가 끝난 서블릿 인스턴스는 GC에 의해 제거된다.

### **2. Servlet과 일반 자바 객체는 무슨 차이가 있는가?**

JVM에서 호출 방식은 서블릿과 일반 클래스 모두 같으나, 서블릿은 `main()` 메소드로 직접 호출되지 않고, 웹 컨테이너(Servlet Container)에 의해 실행된다.


## Servlet Container란?

구현되어 있는 Servlet 클래스의 규칙에 맞게 서블릿 객체를 생성, 초기화, 호출, 종료하는 생명 주기를 관리한다. (ex -톰캣)
⇒ 개발자가 **웹서버와 통신하기 위한** 복잡한 일들(소켓 생성, 특정 포트 리스닝, 스트림 생성 등)을 할 필요가 없게 해준다.

### **1. Servlet Container의 특징을 설명하라.**

- 개발자가 비즈니스 로직에 집중할 수 있도록 HTTP 요청 메시지 파싱, Content-Type 확인, HTTP 응답 메시지 생성 등 작업을 대신 처리 해준다.
- **서블릿의 생명 주기를 관리**한다.
- 요청이 올 때마다 자바 스레드 하나를 생성하여 **멀티 스레딩 처리**를 한다.

### **2. Servlet의 동작 방식을 설명하라.**

- 사용자가 URL을 입력하면 요청이 서블릿 컨테이너로 전송된다.
- 요청을 전송 받은 서블릿 컨테이너는 Http Request, Http Response 객체를 생성한다.
- 사용자가 요청한 URL이 어느 서블릿에 대한 요청인지 찾는다. 위 예제에서는 helloServlet을 찾게 된다.
- 서블릿의 `service()` 메소드를 호출한 후 클라이언트의 GET, POST 여부에 따라 `doGet()`, `doPost()` 메소드를 호출한다.
- 동적 페이지를 생성한 후 HttpServletResponse 객체에 응답을 보낸다.
- 클라이언트에 최종 결과를 응답한 후 HttpServletRequest, HttpServletResponse 객체를 소멸한다.


### **3. Servlet Container의 동작 과정을 설명하라.**

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ae6f6a5f-d173-41c4-b3c1-1406aa1e81cf/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221019T045807Z&X-Amz-Expires=86400&X-Amz-Signature=c67923aeebc971a1f812ea5bcd5fdb0a2b59898dde43f4f2f4dee4f55e49b640&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

1. 웹 브라우저에서 웹 서버에 HTTP 요청을 보내면, 웹 서버는 받은 HTTP 요청을 WAS의 Web Server로 전달한다.
2. WAS의 웹 서버는 HTTP 요청을 서블릿 컨테이너에 전달한다.
3. 서블릿 컨테이너는 HTTP 요청 처리에 필요한 서블릿 인스턴스가 힙 메모리 영역에 있는지 확인한다. 존재하지 않는다면, 서블릿 인스턴스를 생성하고 해당 서블릿 인스턴스의 `init()` 메소드를 호출하여 서블릿 인스턴스를 초기화한다.
4. 서블릿 컨테이너는 서블릿 인스턴스의 `service()` 메소드를 호출하여 HTTP 요청을 처리하고, WAS의 웹 서버에게 처리 결과를 전달한다.
5. WAS의 웹 서버는 HTTP 응답을 앞 단에 위치한 웹 서버에게 전달하고, 앞 단의 웹 서버는 받은 HTTP 응답을 웹 브라우저에게 전달한다.


# < Spring MVC 개념 >

**프론트 컨트롤러 패턴**에 기초한 웹 MVC 프레임워크이자 Spring 프레임워크의 하위 모듈.


### **1. Spring MVC 등장 이전**

### **Model 1**

JSP와 JavaBeans만 사용하는 아키텍쳐

- 특징
    - JSP 파일에서 Controller와 View 기능을 모두 처리한다.
    - JSP 파일에 자바 코드와 마크업 관련 코드들이 섞여있어 디버깅과 유지보수가 어렵다.
    - 대규모 시스템 개발에 사용하기는 부적합한 아키텍처이다.

- 구조
    
    ![https://k.kakaocdn.net/dn/bZnyNe/btq5sSYP1t8/tPDKKs3YydSMd8hplSjZK1/img.png](https://k.kakaocdn.net/dn/bZnyNe/btq5sSYP1t8/tPDKKs3YydSMd8hplSjZK1/img.png)
    

### **Model 2**

Model 1의 단점을 보완하고자 나오게 되었고 일반적으로 알고 있는 MVC(Model View Controller) 아키텍처

- 특징
    - Controller가 등장함
    - Model, View, Controller 각각의 역할이 분리되어 업무를 명확하게 나누어 작업할 수 있다.
    ( View = 웹 디자이너 , Model, Controller = 개발자 )
    - 해당하는 역할별로 나뉘어 있어 디버깅과 유지보수가 상대적으로 쉽다.

- 구조

![https://k.kakaocdn.net/dn/yWRsY/btq5t3ewyaY/xqItpniRcBcyjyYp9LwOXk/img.png](https://k.kakaocdn.net/dn/yWRsY/btq5t3ewyaY/xqItpniRcBcyjyYp9LwOXk/img.png)

- 단점
    
    ![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8bbc473d-f303-477b-b134-fca20a6f59ff/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221019T045752Z&X-Amz-Expires=86400&X-Amz-Signature=630ad54678c29f92cc80d58ca97482b3a6f7c2a83ab2c49d44bc9a37641be334&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)
    
    - URL마다 서블릿을 생성하고 Web.xml로 서블릿을 관리했다.
    - URL마다 서블릿이 필요하다 보니, 매번 서블릿 인스턴스를 만들어야했다.
    - 각 서블릿마다 공통 기능을 하는 코드들이 중복해서 발생하여 유지 보수 하기 어려웠다.
    만약 하나의 서블릿으로 Controller를 구현하면 클라이언트의 모든 요청을 하나의 서블릿이 처리하게 되어 수많은 분기 처리 로직을 가질 수 있다.
    이는 새로운 기능을 추가할 때마다 분기 처리 로직이 계속 늘어나기 때문에 복잡도가 올라가고 유지보수도 쉽지않을 것이다.



## 스프링 MVC의 구조

기본 시스템 모듈이 MVC(Model-View-Controller)로 나눠져 있다.

- **Model** - '데이터' 디자인을 담당한다. (ex. 상품 목록, 주문 내역 등)
- **View** - '실제로 렌더링되어 보이는 페이지' 를 담당한다. (ex. `.JSP` 파일들이 여기에 해당됨.)
- **Controller** - 사용자의 요청을 받고, 응답을 주는 로직을 담당한다. (ex. GET 등의 uri 매핑이 여기에 해당된다.)

## 스프링 MVC의 특징

- **Model, View, Controller를 명확하게 분리하여 매우 유연하고 확장성이 좋다.**
- **DispatcherServlet을 사용한다.**
    - DispatcherServlet (프론트 컨트롤러)
        - Spring MVC의 핵심 요소 중 하나로서 **프론트 컨트롤러 패턴**을 구현한 Servlet이고 클라이언트의 요청을 전달받아 요청에 맞는 컨트롤러가 리턴한 결과값을 View에 전달하여 알맞은 응답을 생성해준다.
        1. 작동원리
            1. 클라이언트로부터 요청이 들어오면 서블릿 컨테이너가 요청을 받는다.
            2. 이후 공통 작업을 DipatcherServlet에 처리하고 이외 작업은 적절한 세부 컨트롤러로 위임한다.
        2. 프론트 컨트롤러 패턴
            - 표현 계층 전면에서 HTTP 프로토콜을 통해 들어오는 모든 요청을 프론트 컨트롤러(하나의 서블릿)에게 보내고 프론트 컨트롤러는 각 요청에 맞는 컨트롤러를 찾아서 호출해주는 패턴. ⇒ 즉, 로직에 들어가기 전에 요청에 대한 선처리 작업을 수행함으로써 중앙 집중 방식으로 수행한다. (ex. 지역 정보 결정, 멀티파트 파일 업로드 처리 등)
            
            ![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ff4eb431-a062-4090-96f4-973b0e8c7cb3/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221019T045735Z&X-Amz-Expires=86400&X-Amz-Signature=f45904496f8dcffe84334f9253abf4ff2ff062064f60d093ded0900d012159c0&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)
            
        3. 특징
            - 공통 기능은 프론트 컨트롤러에서 처리하고 서로 다른 코드들만 각 컨트롤러에서 처리하도록 할 수 있다.
- **스프링 프레임워크이 기존 MVC 패턴의 효율성을 위해 개발과 유지보수의 편의성이 보장되도록 디자인 패턴을 결합했다.**

## **스프링 MVC의 장점**

- 매우 작기 때문에 문제가 적은 고성능을 제공합니다.
- 생산성이 높아 개발이 증가합니다.
- Spring SPI를 사용하므로 매우 안전하며 대부분 웹 애플리케이션을 위해 모든 은행에서 사용됩니다.
- Model View 및 컨트롤러 아키텍처를 지원하므로 모듈 식 애플리케이션을 개발할 수 있습니다.
- 그것은 너무 좋은 완전한 테스트 주도 개발을 지원합니다.
- 지금까지 전 세계 개발자가 애자일 개발 웹 애플리케이션에 가장 적합했습니다.
- 작업을 단순화하는 책임 및 역할 분리 기능이 있습니다.
- RESTful 서비스를 지원합니다.
- 테마, 국제화, 기타 데이터베이스 프레임 워크, JPA, 다중보기 및 커뮤니티 지원을 지원합니다.

## 스프링 MVC의 단점

- Spring은 XML 기반 또는 어노테이션 기반과 같은 변화하는 특성을 가지고 있으며 때때로 후속 조치가 어려워집니다.
- Spring MVC의 사양이 매우 적습니다.
- jar 파일을 사용할 수 없으면 응용 프로그램이 제대로 실행되지 않습니다.
- 매우 큰 구성 문제, 처리 할 많은 컨트롤러, 제어 할 많은 뷰 리졸버 등

## 스프링 MVC의 구성 요소

Spring MVC가 유기적으로 동작되기 위해 내부적으로 다양한 구성요소가 이용된다.

- DispatcherServlet(Front Controller)
클라이언트의 요청을 전달받아 요청에 맞는 컨트롤러가 리턴한 결과값을 View에 전달하여 알맞은 응답을 생성해준다.
- HandlerMapping 객체
Request와 Handler 객체간의 매핑을 정의하고 있는 인터페이스로서
클라이언트의 요청 URL을 어떤 컨트롤러가 처리할지 결정한다.
- Handler(Controller)
클라이언트의 요청을 처리한 뒤, 결과를 DispatcherServlet에게 리턴해주는 역할.
    - 보통 요청의 종류 혹은 로직의 분류에 따라 내부적으로 Service 단위로 나누어 모듈화 한다.
    - 각 서비스에서는 DB 접근할 수 있는 Repository 객체를 이용하여 데이터에 접근할 수 있다.
- ModelAndView
컨트롤러가 처리한 결과 정보 및 뷰 선택에 필요한 정보를 담음
- ViewResolver
리턴해줄 뷰 이름에 해당하는 실제 뷰 컴포넌트를 찾아주는 역할(모델 렌더링을 담당)
(컨트롤러가 서비스에서의 로직 처리 후에 뷰리졸버를 통해 뷰를 찾고 해당 뷰에다가 던져줌)
    - 특정 ViewResolver를 Bean으로 등록하지 않으면, DispatcherServlet은 기본 viewResolver인 InternalResourceView를 사용한다.
- HandlerAdapter
매핑된 핸들러를 실제로 실행하는 역할
- ModelAndView
컨트롤러 처리 결과 후 응답할 view와 view에 전달할 값을 저장 및 전달하는 역할

## **DispatcherServlet 위주의 SpringMVC** 동작 흐름

- 요약
요청 -> 프론트 컨트롤러(**DispatcherServlet**) -> 핸들러 매핑 -> 핸들러 어댑터 -> 컨트롤러 -> 로직 수행(서비스) -> 컨트롤러 -> 뷰 리졸버 -> 응답(jsp, html)

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/41be5019-e557-4825-9e64-349db75a32fe/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221019T045706Z&X-Amz-Expires=86400&X-Amz-Signature=ed2fbeac009e78c8949bffb87238659be4855121dc925db1199a3b755ccdb39e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

1. **DispatcherServlet**으로 클라이언트의 웹 요청이 들어온다.
2. 웹 요청을 **Handler Mapping**에 위임하여 해당 요청을 처리할 Handler(Controller)를 탐색한다.
3. 찾은 Handler를 실행할 수 있는 **HandlerAdapter**를 탐색한다.
4. 찾은 Handler Adapter를 사용해서 Handler의 메소드를 실행한다.
5. Handler의 반환 값은 Model과 View이다.
6. View 이름을 ViewResolver에게 전달하고, ViewResolver는 해당하는 View 객체를 전달한다.
7. DispatcherServlet은 View에게 Model을 전달하고 화면 표시를 요청한다.
이때, Model이 null이면 View를 그대로 사용하고, 그렇지 않으면 View에 Model 데이터를 렌더링한다.
8. 최종적으로 DispatcherServlet은 View 결과(HttpServletResponse)를 클라이언트에게 반환한다.

위 흐름은 @Controller 기준이며, @RestController의 경우 6번과 7번 과정이 생략된다.
즉, ViewResolver를 타지 않고 반환 값에 알맞는 MessageConverter를 찾아 응답 본문을 작성한다.

---

추가로 알면 좋은 사항
- ContextLoaderListener 개념 설명
        
    Spring MVC에서의 루트 애플리케이션 컨텍스트로서
    WAS 구동시에 web.xml을 읽어들여 웹 어플리케이션 설정을 구성하기 위한 초기 셋팅 작업을 해주는 역할입니다.
    
    [[Spring MVC] ContextLoaderListener(전역 Context 설정)](https://pangtrue.tistory.com/m/136)
        
- Spring MVC에서 **web.xml의 역할**
        
    Deployement Descriptor(배포 서술자)로서 프로젝트에 대한 설정에 대한 정보를 가지고 있는 파일.
    Web Application 실행되고 먼저 web.xml가 메모리에 로드가 됩니다.
    이후 ContextLoaderListner에 대한 Servlet 컨테이너에 의해 인스턴스가 생성되고 webApplicationContext가 로드 됩니다.
    (서블릿 3.0 부터는 어노테이션을 지원하게 되면서, 더 이상 서블릿이나 필터를 사용하기 위해서 web.xml에 기술하지 않아도 가능하게 되었습니다. 컨트롤러를 선언할 때 @Controller 어노테이션을 사용하는 것처럼 서블릿을 사용할 때 @WebServlet 어노테이션을 사용하면 됩니다.)

    - 설정 종류
        - **DispatcherServlet**
        - **ContextLoaderListener**
        - **Filter**

---

## 출처

- 전반적인 SpringMVC 구조에 대한 이해
    
    [[부스트코스 웹 프로그래밍] 스프링 MVC](https://dailyheumsi.tistory.com/159)
    
    [[Spring] Servlet, Servlet Container, Spring MVC 정리](https://steady-coding.tistory.com/599)
    
    [[Spring MVC]Model1, Model2 그리고 스프링 MVC 구조](https://code-overflow.tistory.com/m/entry/Spring-MVCModel1-Model2-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%8A%A4%ED%94%84%EB%A7%81-MVC-%EA%B5%AC%EC%A1%B0)
    

- ModelAndView & ViewResolver 관련
    
    [[java spring] ModelAndView 모델앤뷰 / ViewResolver뷰리졸버 란 ?](https://cheershennah.tistory.com/m/107)
    

- ContextListener 관련
    
    [[Spring] contextConfigLocation와 ContextLoaderListener](https://velog.io/@jcrs0907/Spring-contextConfigLocation%EC%99%80-ContextLoaderListener)
    
    [[Spring MVC] ContextLoaderListener(전역 Context 설정)](https://pangtrue.tistory.com/m/136)
    
    [[Spring] ContextLoaderListener 란? RootApplicationContext과 WebApplicationContext란?](https://tlatmsrud.tistory.com/m/43)
    

- SpringMVC 관련 질문
    
    [25+ Top Spring MVC 인터뷰 질문 및 답변 - 다른](https://ko.myservername.com/25-top-spring-mvc-interview-questions)



스터디 날짜: 2022/10/17

자료조사 : @또로리, [@Yerimi11](https://github.com/Yerimi11)
