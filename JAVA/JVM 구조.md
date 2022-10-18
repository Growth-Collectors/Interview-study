# JVM 구조

날짜: 2022/10/06

자료조사: Hwang Hanah, Yerim

태그: java, jvm

## JVM, GC 관련 질문

1. JVM
    1. 정의 
    2. 특징 
    3. 구조 (특히 메모리 영역 구조) 
    4. Java의 실행 방식 
    5. JVM 원자성이란?
    6. JVM 모니터링 방법 
    
2. GC (정의, 하는 역할, 필요한 이유, Java 버전 별 동작 방식)
    1. 정의 
    2. 하는 역할 
    3. 필요한 이
    4. Java 버전 별 동작 방식 (8과 11을 중점으로 자세하게) 
    5. gc 모니터링 방법 
        1. JVM 응용 프로그램을 개발한 후에 운영하다 보면 out of memory가 나올 수 있는데, 이 에러 대처 방안 및 원인

---

## **자바 가상 머신(JVM)의 동작 방식**

![https://blog.kakaocdn.net/dn/cQRqku/btru0vJ6Ixx/9qCTW7ChXc80fGfQUrT4B0/img.png](https://blog.kakaocdn.net/dn/cQRqku/btru0vJ6Ixx/9qCTW7ChXc80fGfQUrT4B0/img.png)

1. 자바로 개발된 프로그램을 실행하면 JVM은 OS로부터 메모리를 할당합니다.

2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 자바 바이트코드(.class)로 컴파일합니다.

3. Class Loader를 통해 JVM Runtime Data Area로 로딩합니다.

4. Runtime Data Area에 로딩 된 .class들은 Execution Engine을 통해 해석합니다.

5. 해석된 바이트 코드는 Runtime Data Area의 각 영역에 배치되어 수행하며 이 과정에서 Execution Engine에 의해 GC의 작동과 스레드 동기화가 이루어집니다.

---

## **JVM의 구조**

### **클래스 로더(Class Loader)**

![https://blog.kakaocdn.net/dn/IXgMz/btrvglUCR65/MaffyNzf1y1oV8BTBKADKk/img.png](https://blog.kakaocdn.net/dn/IXgMz/btrvglUCR65/MaffyNzf1y1oV8BTBKADKk/img.png)

자바는 동적으로 클래스를 읽어오므로, 프로그램이 실행 중인 **런타임에서야 모든 코드가 자바 가상 머신과 연결**됩니다. 

이렇게 **동적으로 클래스를 로딩해주는 역할**을 하는 것이 바로 **클래스 로더(class loader)**입니다. 

자바에서 소스를 작성하면 .java파일이 생성되고 .java소스를 컴파일러가 컴파일하면 .class파일이 생성되는데 **클래스 로더는 .class 파일을 묶어서 JVM이 운영체제로부터 할당받은 메모리 영역인 Runtime Data Area로 적재**합니다.

### **실행 엔진(Execution Engine)**

앞서 말씀드렸듯 클래스 로더에 의해 JVM으로 로드된 .class 파일(바이트코드)들은 Runtime Data Areas의 Method Area에 배치되는데, 배치된 이후에 **JVM은 Method Area의 바이트 코드를 실행 엔진(Execution Engine)에 제공**하여, 정의된 내용대로 바이트 코드를 실행시킵니다. 

이때, **로드된 바이트코드를 실행하는 런타임 모듈**이 **실행 엔진(Execution Engine)**입니다. 

실행 엔진은 바이트코드를 **명령어 단위로 읽어서 실행**합니다.

### **가비지 컬렉터(Garbage Collector)**

![https://blog.kakaocdn.net/dn/esrrQe/btrvdvpVX3u/TuWioGNha4g8vgGDKD3tQk/img.png](https://blog.kakaocdn.net/dn/esrrQe/btrvdvpVX3u/TuWioGNha4g8vgGDKD3tQk/img.png)

자바 가상 머신은 **가비지 컬렉터**(garbage collector)를 이용하여 더는 사용하지 않는 메모리를 자동으로 회수해 줍니다. 

따라서 개발자가 따로 메모리를 관리하지 않아도 되므로, 더욱 손쉽게 프로그래밍을 할 수 있도록 도와줍니다. 

**Heap 메모리 영역에 생성(적재)된 객체들 중에 참조되지 않은 객체들을 탐색 후 제거**하는 역할을 하며 해당 역할을 하는 시간은 정확히 언제인지를 알 수 없습니다. 

**GC역할을 수행하는 스레드를 제외한 나머지 모든 스레드들은 일시정지상태가 됩니다. (stop-the-world)**

### ***※ 가비지 컬렉터에 대해 더 자세히 알고 싶다면 아래 글을 참고해주세요.***

[[Java] 가비지 컬렉션(GC, Garbage Collection) 총정리](https://coding-factory.tistory.com/829)

## **런타임 데이터 영역 (Runtime Data Area)**

![https://blog.kakaocdn.net/dn/bZR97z/btrvtdBl1Md/LbUk2NVlgDmsKMcBiQ9f4K/img.png](https://blog.kakaocdn.net/dn/bZR97z/btrvtdBl1Md/LbUk2NVlgDmsKMcBiQ9f4K/img.png)

런타임 데이터 영역은 JVM의 메모리 영역으로 **자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역**입니다.

- **모든 스레드가 공유해서 사용하는 영역**
    - Method Area(메서드 영역)
    - Heap Area(힙 영역) - GC의 대상
- **스레드(Thread) 마다 하나씩 생성**
    - Stack Area(스택 영역)
    - PC Register(PC 레지스터)
    - Native Method Stack(네이티브 메서드 스택)

### 1. 모든 스레드가 공유해서 사용하는 영역

### 1-1. **메서드 영역 (Method Area)**

클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보와 같은 각종 필드 정보들과 
메서드 정보, 데이터 Type 정보, Constant Pool, static변수, final class 등이 생성되는 영역입니다.

### 1-2. **힙 영역 (Heap Area)**

**1.** new 키워드로 생성된 **객체와 배열이 생성되는 영역**입니다.

**2.** 주기적으로 **GC가 제거하는 영역**입니다.

![https://blog.kakaocdn.net/dn/cmeYY6/btrveK8fwH2/NwdYae5gEQHih3lyV83as0/img.png](https://blog.kakaocdn.net/dn/cmeYY6/btrveK8fwH2/NwdYae5gEQHih3lyV83as0/img.png)

- Heap Area는 효율적인 GC를 위해 위와 같이 크게 3가지의 영역으로 나뉘게 됩니다.
- **Young Generation 영역**은 자바 객체가 생성되자마자 저장되고, 생긴지 얼마 안되는 객체가 저장되는 공간입니다.
    
     Heap 영역에 객체가 생성되면 최초로 Eden 영역에 할당됩니다. 그리고 이 영역에 데이터가 어느정도 쌓이게 되면 참조정도에 따라 Servivor의 빈 공간으로 이동되거나 회수됩니다.
    
    **Young Generation(Eden+Servivor) 영역**이 차게 되면 또 참조정도에 따라 Old영역으로 이동 되게 되거나 회수됩니다. 
    
    이렇게 Young Generation과 Tenured Generation 에서의 GC를 Minor GC 라고 합니다. 
    
- **Old영역**에 할당된 메모리가 허용치를 넘게 되면, Old 영역에 있는 모든 객체들을 검사하여 참조되지 않는 객체들을 한꺼번에 삭제하는 GC가 실행됩니다.
    
    시간이 오래 걸리는 작업이고 이 때 **GC를 실행하는 쓰레드를 제외한 모든 스레드는 작업을 멈추게 됩니다. 이를 'Stop-the-World'** 라 합니다. 
    
    그리고 이렇게 **'Stop-the-World'가 발생하고 Old영역의 메모리를 회수하는 GC를 Major GC**라고 합니다.

### 2. 스레드**(Thread) 마다 하나씩 생성**

### 2-1. **스택 영역 (Stack Area)**

지역변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값 등이 생성되는 영역입니다.

### 2-2. **PC 레지스터 (PC Register)**

Thread가 생성될 때마다 생성되는 영역으로 프로그램 카운터, 즉 현재 스레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역입니다.

### 2-3. **네이티브 메서드 스택 (Native Method Stack)**

**1.** 자바 이외의 언어(C, C++, 어셈블리 등)로 작성된 네이티브 코드를 실행할 때 사용되는 메모리 영역으로 일반적인 C 스택을 사용합니다.

**2.** 보통 C/C++ 등의 코드를 수행하기 위한 스택을 말하며 (JNI) **자바 컴파일러에 의해 변환된 자바 바이트 코드를 읽고 해석하는 역할을 하는 것이 자바 인터프리터(interpreter)**입니다.

### 출처

[[Java] 자바 JVM 내부 구조와 메모리 구조에 대하여](https://coding-factory.tistory.com/828)

---

## *JVM이란?*

JVM 메모리 구조를 설명하기 전에 JVM이 무엇인지 알아야 합니다. 

JVM은 **Java Virtual Machine의 약**자로, 자바 가상 머신이라고 부릅니다. 그리고 **자바와 운영체제 사이에서 중개자 역할을 수행**하며, 자바가 운영체제에 구애 받지 않고 프로그램을 실행할 수 있도록 도와줍니다. 

또한, 가비지 컬렉터를 사용한 메모리 관리도 자동으로 수행하며, 다른 하드웨어와 다르게 레지스터 기반이 아닌 **스택 기반**으로 동작합니다.

아래는 자바 프로그램의 실행 단계입니다.

![https://blog.kakaocdn.net/dn/o2kwL/btqSmzWHdHV/OeIODqCVTN97ioNDCVjiU0/img.png](https://blog.kakaocdn.net/dn/o2kwL/btqSmzWHdHV/OeIODqCVTN97ioNDCVjiU0/img.png)

먼저, 자바 컴파일러에 의해 자바 소스 파일은 바이트 코드로 변환됩니다. 그리고 이러한 **바이트 코드를 JVM에서 읽어 들인 다음에, 이것저것 복잡한 과정을 거쳐서 어떤 운영체제든간에 프로그램을 실행할 수 있도록 만드는 것**입니다.

만약, 자바 소스 파일은 리눅스에서 만들었고 윈도우에서 이 파일을 실행하고 싶다면, 윈도우용 JVM을 설치만 하면 됩니다. 여기서 JVM은 운영체제에 종속적이라는 특징을 알 수 있습니다.

<aside>
💬 어떤 운영체제간에 실행할 수 있는데 왜 종속적일까?

JVM 자체는 운영체제별로 따로 만들어져야 되는 거 같아요. JVM 상위에 올라가는 자바 애플리케이션의 경우에는 높은 호환성을 갖겠지만요!

</aside>

**JVM 은 자바 프로그램이 어느 기기 또는 어느 운영체제에서도 실행될 수 있게 해준다**

(자바는 JVM이 운영체제와 프로그램 사이에서 해당 운영체제에 맞게 변환하여 전달하기 때문에 운영체제에서 자유롭다.

JVM 은 운영체제에 종속적이기 때문에 **운영체제에 맞는 JVM을 필요로 한다**. )

## *JVM 메모리 구조*

위에서 자바 프로그램의 실행 단계를 아주 간략하게 소개하였는데, JVM의 구체적인 수행 과정은 언급하지 않았습니다. 이제부터는 JVM이 정확히 어떻게 동작을 하고 구조가 어떤지 알아 봅시다.

아래는 자바 프로그램의 실행 단계입니다. 분량상 운영체제 도형은 생략하였습니다. JVM의 구조는 크게 보면, Garbage Collector, Execution Engine, Class Loader, Runtime Data Area로, 4가지로 나눌 수 있습니다.

![https://blog.kakaocdn.net/dn/pjywN/btqSduBXLIK/2QEL5c2nEJXRm0cyhvwxF1/img.png](https://blog.kakaocdn.net/dn/pjywN/btqSduBXLIK/2QEL5c2nEJXRm0cyhvwxF1/img.png)

위에서 설명하였듯이, 자바 소스 파일은 자바 컴파일러에 의해서 바이트 코드 형태인 클래스 파일이 됩니다. 그리고 이 클래스 파일은 클래스 로더가 읽어들이면서 JVM이 수행됩니다.

**(1) Class Loader**

**JVM 내로 클래스 파일을 로드**하고, 링크를 통해 배치하는 작업을 수행하는 모듈입니다. 런타임 시에 동적으로 클래스를 로드합니다.

**(2) Execution Engine**

클래스 로더를 통해 **JVM 내의 Runtime Data Area에 배치된 바이트 코드들을 명렁어 단위로 읽어서 실행**합니다. 최초 JVM이 나왔을 당시에는 **인터프리터 방식**이었기때문에 속도가 느리다는 단점이 있었지만 **JIT 컴파일러** 방식을 통해 이 점을 보완하였습니다. JIT는 바이트 코드를 어셈블러 같은 네이티브 코드로 바꿈으로써 실행이 빠르지만 역시 변환하는데 비용이 발생하였습니다. 이 같은 이유로 JVM은 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고, 인터프리터 방식을 사용하다가 일정한 기준이 넘어가면 JIT 컴파일러 방식으로 실행합니다.

**(3) Garbage Collector**

**Garbage Collector(GC)는 힙 메모리 영역에 생성된 객체들 중에서 참조되지 않은 객체들을 탐색 후 제거**하는 역할을 합니다. 이때, GC가 역할을 하는 시간은 언제인지 정확히 알 수 없습니다.

**(4) Runtime Data Area**

**JVM의 메모리 영역**으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역입니다. 이 영역은 크게 Method Area, Heap Area, Stack Area, PC Register, Native Method Stack로 나눌 수 있습니다.

![https://blog.kakaocdn.net/dn/kOOdl/btqR1E0kWdB/7El4pzDEIvx0UGXLVanKjK/img.png](https://blog.kakaocdn.net/dn/kOOdl/btqR1E0kWdB/7El4pzDEIvx0UGXLVanKjK/img.png)

(**1) Method area**

**모든 쓰레드가 공유하는 메모리 영역**입니다. 메소드 영역은 클래스, 인터페이스, 메소드, 필드, Static 변수 등의 바이트 코드를 보관합니다.

**2. Heap area**

**모든 쓰레드가 공유**하며, **new 키워드로 생성된 객체와 배열이 생성되는 영**역입니다. 또한, 메소드 영역에 로드된 클래스만 생성이 가능하고 Garbage Collector가 참조되지 않는 메모리를 확인하고 제거하는 영역입니다.

**3. Stack area**

![https://blog.kakaocdn.net/dn/ulBPu/btqSmAVJhzs/t5uaU3DyAmRUbNKnu10bak/img.png](https://blog.kakaocdn.net/dn/ulBPu/btqSmAVJhzs/t5uaU3DyAmRUbNKnu10bak/img.png)

메서드 호출 시마다 각각의 스택 프레임(그 메서드만을 위한 공간)이 생성합니다. 그리고 메서드 안에서 사용되는 값들을 저장하고, 호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장합니다. 마지막으로, 메서드 수행이 끝나면 프레임별로 삭제합니다.

**4. PC Register**

쓰레드가 시작될 때 생성되며, 생성될 때마다 생성되는 공간으로 쓰레드마다 하나씩 존재합니다. 쓰레드가 어떤 부분을 무슨 명령으로 실행해야할 지에 대한 기록을 하는 부분으로 현재 수행중인 JVM 명령의 주소를 갖습니다.

**5. Native method stack**

자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역입니다.

## *정리*

지금까지 간단하게 JVM 메모리 구조를 알아 보았습니다. 사실, 힙은 또 몇 가지의 영역으로 나뉘고 가비지 컬렉터는 어떻게 동작하는지도 알아 보아야합니다. 하지만, 처음부터 모든 내용을 다 다루기에는 무리가 있어서 이렇게 처음에는 겉핥기 식으로 설명을 하고, 추후 각각의 영역에 대해서 깊은 내용을 다루려고 합니다.

### 출처

- [https://steady-coding.tistory.com/305](https://steady-coding.tistory.com/305)
- [https://coding-factory.tistory.com/828](https://coding-factory.tistory.com/828)
- [https://www.tcpschool.com/java/java_array_memory](https://www.tcpschool.com/java/java_array_memory)
- [https://velog.io/@dev_isaac/JVM#runtime-data-areas](https://velog.io/@dev_isaac/JVM#runtime-data-areas)

# 추가 링크

- 추천

[JVM(Java Virtual Machine)의 구조 - 메모리](https://thisisprogrammingworld.tistory.com/m/179)

- 참고

[JVM에 관하여 - Part 3, Run-Time Data Area](https://tecoble.techcourse.co.kr/post/2021-08-09-jvm-memory/)

[[JVM구조]JVM으로 보는 java 프로그램의 실행 과정](https://technote-mezza.tistory.com/72)
