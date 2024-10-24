
>이제 프레젠테이션과 데이터 원본 계층이 외부 세계와 연결한다는 면에서 비슷한 점이 많다는 것을 알 수 있을것이다.\
>이것은 모든 시스템을 외부 시스템에 대한 인터페이스에 둘러싸인 시스템으로 시각화 하는 앨리스테어 콕번(Alistair Cockburn)의 헥사고날 아키텍처 패턴의 바탕이 되는 논리다.\
>-martin folower- \
>in Patterns of Enterprise Application Architecture


>헥사고날 아키텍처는 모든 외부 요소를 근본적으로 외부 인터페이스로 나타내므로 비대칭 계층화 체계와는 다른 모양의 대칭 뷰를 보여준다.\
>-martin folower- \
>in Patterns of Enterprise Application Architecture

# Origin
---

https://alistair.cockburn.us/hexagonal-architecture/

Ports and Adaptor architecture라고 알려진 Hexagonal architecture는 Alistair Cockburn에 의 해 2005년도에 고안되었다.

## The Pattern: Ports and Adapters (‘’Object Structural’’)

### Alternative name: ‘’Ports & Adapters’’

### Alternative name: ‘’Hexagonal Architecture’’

> port and adapters == port & adapters == Hexagonal Architecture

### Intent - 의도
---

>Allow an application to equally be driven by users, programs, automated test or batch scripts, and to be developed and tested in isolation from its eventual run-time devices and databases.

애플리케이션이 사용자, 프로그램, 자동화된 테스트 또는 배치 스크립트에 의해 동등하게 구동될 수 있게 하고, 최종 실행 시 사용될 장치나 데이터베이스와는 독립적으로 개발 및 테스트될 수 있도록 한다.

>When any driver wants to use the application at a port, it sends a request that is converted by an adapter for the specific technology of the driver into an usable procedure call or message, which passes that to the application port.\ 
>The application is blissfully ignorant of the driver’s technology.\
>When the application has something to send out, it sends it out through a port to an adapter, which creates the appropriate signals needed by the receiving technology (human or automated).\ 
>The application has a semantically sound interaction with the adapters on all sides of it, without actually knowing the nature of the things on the other side of the adapters.

어떤 드라이버든 애플리케이션의 포트를 사용하고자 할 때, 드라이버의 특정 기술을 위한 어댑터에 의해 요청이 변환되어 사용 가능한 프로시저 호출 또는 메시지로 전달되며, 이는 애플리케이션 포트로 전달된다. 

>[!important]
>애플리케이션은 드라이버의 기술을 전혀 알지 못한 채 작동한다.\
>애플리케이션이 외부로 무언가를 보낼 때, 이를 포트를 통해 어댑터로 보내고, 어댑터는 수신 기술(인간 또는 자동화된 시스템)이 필요로 하는 적절한 신호를 생성한다.\
>애플리케이션은 어댑터와 모든 상호작용을 의미론적으로 타당하게 수행하며, 어댑터 너머의 기술적인 사항은 알지 못한 채 작업을 진행한다.
### Motivation - 동기
---

>One of the great bugaboos of software applications over the years has been infiltration of business logic into the user interface code.\
>The problem this causes is threefold.

수년간 소프트웨어 애플리케이션의 큰 문제 중 하나는 비즈니스 로직이 사용자 인터페이스 코드에 스며드는 현상이다.

이로 인해 세 가지 문제가 발생한다.

- First, the system can’t neatly be tested with automated test suites because part of the logic needing to be tested is dependent on oft-changing visual details such as field size and button placement
- For the exact same reason, it becomes impossible to shift from a human-driven use of the system to a batch-run system
- For still the same reason, it becomes difficult or impossible to allow the program to be driven by another program when that becomes attractive.

첫째, 시스템의 일부 로직이 필드 크기나 버튼 배치와 같은 자주 변하는 시각적 세부 사항에 의존하기 때문에 자동화된 테스트 스위트로는 체계적으로 테스트하기 어렵다.

두 번째로, 동일한 이유로 시스템의 인간 주도 사용 방식에서 배치 실행 방식으로 전환하는 것이 불가능해진다.

세 번째로, 여전히 같은 이유로 다른 프로그램이 시스템을 구동하게 하는 것이 매력적으로 보일 때 그렇게 하기 어렵거나 불가능하다.

>The attempted solution, repeated in many organizations, is to create a new layer in the architecture, with the promise that this time, really and truly, no business logic will be put into the new layer.\
>However, having no mechanism to detect when a violation of that promise occurs, the organization finds a few years later that the new layer is cluttered with business logic and the old problem has reappeared.

여러 조직에서 시도된 해결책은 아키텍처에 새로운 레이어를 추가하는 것이다. 이때 이번에는 정말로 비즈니스 로직을 새 레이어에 넣지 않겠다는 약속을 한다. 

그러나 이를 감지할 메커니즘이 없기 때문에 몇 년 후 새 레이어에도 비즈니스 로직이 엉켜 있는 상태가 되어 기존 문제가 다시 나타나게 된다.

>Imagine now that ‘’every’’ piece of functionality the application offers were available through an API (application programmed interface) or function call. In this situation, the test or QA department can run automated test scripts against the application to detect when any new coding breaks a previously working function.\
>The business experts can create automated test cases, before the GUI details are finalized, that tells the programmers when they have done their work correctly (and these tests become the ones run by the test department).

이제 애플리케이션이 제공하는 모든 기능이 API(애플리케이션 프로그래밍 인터페이스)나 함수 호출을 통해 사용할 수 있다고 상상해 보라.

이 상황에서는 테스트 또는 QA 부서가 자동화된 테스트 스크립트를 사용하여 새로 작성된 코드가 이전에 정상적으로 동작하던 기능을 깨뜨리지 않았는지 감지할 수 있다.

비즈니스 전문가들은 GUI 세부 사항이 확정되기 전에 자동화된 테스트 케이스를 생성하여 프로그래머가 작업을 제대로 완료했는지 알려주며, 이러한 테스트는 테스트 부서에서 실행되는 테스트가 된다.

>The application can be deployed in ‘’headless’’ mode, so only the API is available, and other programs can make use of its functionality — this simplifies the overall design of complex application suites and also permits business-to-business service applications to use each other without human intervention over the web.\
>Finally, the automated function regression tests detect any violation of the promise to keep business logic out of the presentation layer.\
>The organization can detect, and then correct, the logic leak.

애플리케이션은 ‘headless’ 모드로 배포될 수 있으므로 API만 사용할 수 있으며, 다른 프로그램이 이 기능을 사용할 수 있다.

이는 복잡한 애플리케이션 스위트의 전체 설계를 단순화하고, 비즈니스 간 서비스 애플리케이션이 웹을 통해 인간의 개입 없이 서로를 사용할 수 있게 한다.\ 

마지막으로, 자동화된 기능 회귀 테스트는 비즈니스 로직이 프레젠테이션 레이어에 유입되지 않도록 하는 약속을 위반했는지 감지한다.

조직은 로직 유출을 감지하고 이를 수정할 수 있다.

>An interesting similar problem exists on what is normally considered “the other side” of the application, where the application logic gets tied to an external database or other service.\
>When the database server goes down or undergoes significant rework or replacement, the programmers can’t work because their work is tied to the presence of the database.\
>This causes delay costs and often bad feelings between the people.

애플리케이션의 ‘다른 측면’에서 유사한 문제가 존재한다. 

이 측면에서는 애플리케이션 로직이 외부 데이터베이스나 다른 서비스에 묶여 있다. 

데이터베이스 서버가 다운되거나 대대적인 재작업이나 교체가 발생하면, 프로그래머들은 데이터베이스가 없으면 작업을 진행할 수 없기 때문에 작업이 지연되고 종종 사람들 간의 불만을 초래하게 된다.

>t is not obvious that the two problems are related, but there is a symmetry between them that shows up in the nature of the solution.

이 두 문제가 관련이 있는 것은 명백하지 않지만, 해결책의 본질에서 대칭성이 나타난다.

### Nature of the Solution - 해결방안의 본질
---

>Both the user-side and the server-side problems actually are caused by the same error in design and programming — the entanglement between the business logic and the interaction with external entities.\
>The asymmetry to exploit is not that between ‘’left’’ and ‘’right’’ sides of the application but between ‘’inside’’ and ‘’outside’’ of the application.\
>The rule to obey is that code pertaining to the ‘’inside’’ part should not leak into the ‘’outside’’ part.

사용자 측 문제와 서버 측 문제는 사실 설계 및 프로그래밍에서 동일한 오류, 즉 비즈니스 로직과 외부 엔티티와의 상호작용 간의 얽힘에 의해 발생한다.\ 

활용해야 할 비대칭성은 애플리케이션의 '왼쪽'과 '오른쪽' 사이의 비대칭성이 아니라 애플리케이션 '내부'와 '외부' 사이의 비대칭성이다.\ 

준수해야 할 규칙은 '내부'에 해당하는 코드가 '외부' 부분으로 누출되지 않도록 하는 것이다.

>Removing any left-right or up-down asymmetry for a moment, we see that the application communicates over ‘’ports’’ to external agencies.\
>The word “port” is supposed to evoke thoughts of ‘’ports’’ in an operating system, where any device that adheres to the protocols of a port can be plugged into it;
>and ‘’ports’’ on electronics gadgets, where again, any device that fits the mechanical and electrical protocols can be plugged in.

좌우 또는 상하 비대칭성을 잠시 제거하고 나면, 애플리케이션이 외부 기관과 '포트'를 통해 통신한다는 것을 알 수 있다.

여기서 '포트'라는 단어는 운영 체제에서의 '포트'를 연상하게 하며, 이 포트의 프로토콜을 준수하는 모든 장치는 여기에 연결될 수 있다.

또한, 전자 기기에서의 '포트' 역시 기계적 및 전기적 프로토콜에 맞는 장치는 모두 여기에 연결될 수 있다.

- The protocol for a port is given by the _purpose of the conversation_ between the two devices.

포트의 프로토콜은 두 장치 간 대화의 목적에 의해 결정된다.

>The protocol takes the form of an application program interface (API).

이 프로토콜은 애플리케이션 프로그램 인터페이스(API)의 형태를 취한다.

>For each external device there is an ‘’adapter’’ that converts the API definition to the signals needed by that device and vice versa. A graphical user interface or GUI is an example of an adapter that maps the movements of a person to the API of the port. Other adapters that fit the same port are automated test harnesses such as FIT or Fitnesse, batch drivers, and any code needed for communication between applications across the enterprise or net.

각 외부 장치마다 API 정의를 해당 장치에 필요한 신호로 변환하는 '어댑터'가 존재하며, 그 반대도 가능하다. 

그래픽 사용자 인터페이스(GUI)는 사용자의 움직임을 포트의 API에 매핑하는 어댑터의 예시이다. 

같은 포트에 맞는 다른 어댑터로는 FIT나 Fitnesse와 같은 자동화된 테스트 하네스, 배치 드라이버, 그리고 애플리케이션 간 통신을 위한 코드가 있다.

>On another side of the application, the application communicates with an external entity to get data. The protocol is typically a database protocol. From the application’s perspective, if the database is moved from a SQL database to a flat file or any other kind of database, the conversation across the API should not change. Additional adapters for the same port thus include an SQL adapter, a flat file adapter, and most importantly, an adapter to a “mock” database, one that sits in memory and doesn’t depend on the presence of the real database at all.

애플리케이션의 또 다른 측면에서는 애플리케이션이 외부 엔티티와 통신하여 데이터를 가져온다.

이 프로토콜은 일반적으로 데이터베이스 프로토콜이다.

애플리케이션의 관점에서 SQL 데이터베이스가 평문 파일이나 다른 유형의 데이터베이스로 변경되더라도, API를 통한 대화는 변하지 않아야 한다.  

따라서 동일한 포트에 대한 추가 어댑터로는 SQL 어댑터, 평문 파일 어댑터, 그리고 가장 중요한 '모의' 데이터베이스 어댑터가 포함되며, 이 모의 데이터베이스는 메모리 내에 존재하고 실제 데이터베이스의 유무와 무관하다.

>Many applications have only two ports: the user-side dialog and the database-side dialog. This gives them an asymmetric appearance, which makes it seem natural to build the application in a one-dimensional, three-, four-, or five-layer stacked architecture.

많은 애플리케이션은 사용자 측 대화와 데이터베이스 측 대화의 두 가지 포트만을 가지고 있다.  

이로 인해 비대칭적인 모습이 나타나며, 애플리케이션을 일차원적, 삼계층, 사계층 또는 오계층 스택 아키텍처로 구축하는 것이 자연스럽게 보일 수 있다.

>There are two problems with these drawings. First and worst, people tend not to take the “lines” in the layered drawing seriously. They let the application logic leak across the layer boundaries, causing the problems mentioned above. Secondly, there may be more than two ports to the application, so that the architecture does not fit into the one-dimensional layer drawing.

이러한 도식에는 두 가지 문제가 있다.  

첫 번째이자 가장 심각한 문제는, 사람들이 계층화된 도식에서 '선'을 진지하게 받아들이지 않는다는 것이다.  

애플리케이션 로직이 계층 경계를 넘나들며 누출되면 앞서 언급한 문제가 발생한다.  

두 번째 문제는 애플리케이션에 두 개 이상의 포트가 있을 수 있기 때문에, 아키텍처가 일차원적인 계층 도식에 맞지 않는다는 것이다.

>The hexagonal, or ports and adapters, architecture solves these problems by noting the symmetry in the situation: there is an application on the inside communicating over some number of ports with things on the outside. The items outside the application can be dealt with symmetrically.

헥사고날(육각형) 또는 포트와 어댑터 아키텍처는 상황의 대칭성을 주목함으로써 이러한 문제를 해결한다.  

즉, 애플리케이션 내부에서 외부와 여러 포트를 통해 통신하는 구조를 갖는다.  

애플리케이션 외부에 있는 항목들은 대칭적으로 다룰 수 있다.

>The hexagon is intended to visually highlight

헥사곤(육각형)은 다음을 시각적으로 강조하기 위한 것이다.

(a) the inside-outside asymmetry and the similar nature of ports, to get away from the one-dimensional layered picture and all that evokes, and

(a) 내부와 외부의 비대칭성과 포트의 유사한 특성을 강조하여, 일차원적 계층 그림과 그것이 연상시키는 모든 것에서 벗어나기 위함.

(b) the presence of a defined number of different ports – two, three, or four (four is most I have encountered to date).

(b) 정의된 여러 포트의 존재 - 두 개, 세 개, 또는 네 개 (내가 지금까지 본 가장 많은 포트는 네 개이다).

>The hexagon is not a hexagon because the number six is important, but rather to allow the people doing the drawing to have room to insert ports and adapters as they need, not being constrained by a one-dimensional layered drawing. The term ‘’hexagonal architecture’’ comes from this visual effect.

헥사곤이 육각형인 이유는 숫자 6이 중요해서가 아니라, 그림을 그리는 사람들이 필요할 때 포트와 어댑터를 삽입할 수 있는 공간을 제공하기 위해서이며, 일차원적인 계층 구조 도식에 얽매이지 않도록 하기 위함이다.

'헥사고날 아키텍처'라는 용어는 이러한 시각적 효과에서 비롯되었다.

>The term “port and adapters” picks up the ‘’purposes’’ of the parts of the drawing. A port identifies a purposeful conversation. There will typically be multiple adapters for any one port, for various technologies that may plug into that port. Typically, these might include a phone answering machine, a human voice, a touch-tone phone, a graphical human interface, a test harness, a batch driver, an http interface, a direct program-to-program interface, a mock (in-memory) database, a real database (perhaps different databases for development, test, and real use).

'포트와 어댑터'라는 용어는 도식의 '목적'을 설명하는 부분을 포착한다.  

포트는 목적 있는 대화를 식별한다. 하나의 포트에는 일반적으로 다양한 기술을 연결할 수 있도록 여러 어댑터가 존재한다.  

이 어댑터에는 전화 응답기, 인간의 목소리, 터치톤 전화, 그래픽 사용자 인터페이스, 테스트 하네스, 배치 드라이버, HTTP 인터페이스, 프로그램 간 직접 인터페이스, 모의(메모리 내) 데이터베이스, 실제 데이터베이스(개발, 테스트, 실제 사용을 위한 서로 다른 데이터베이스일 수 있음) 등이 포함될 수 있다.

>In the Application Notes, the left-right asymmetry will be brought up again. However, the primary purpose of this pattern is to focus on the inside-outside asymmetry, pretending briefly that all external items are identical from the perspective of the application.

애플리케이션 노트에서 좌우 비대칭성이 다시 언급될 것이다.  

그러나 이 패턴의 주요 목적은 외부 항목이 애플리케이션의 관점에서 모두 동일하다고 간주하며, 내부와 외부의 비대칭성에 집중하는 것이다.

### Structure - 구조

![image](https://github.com/user-attachments/assets/e3fa9c36-14c2-4fda-9ee0-d2f10394c997)
reference : https://alistair.cockburn.us/hexagonal-architecture/

Figure2 - 어뎁터가 포함된 헥사고날 아키텍처

>Figure 2 shows an application having two active ports and several adapters for each port.\ 
>The two ports are the application-controlling side and the data-retrieval side.\ 
>This drawing shows that the application can be equally driven by an automated, system-level regression test suite, by a human user, by a remote http application, or by another local application.\ 
>On the data side, the application can be configured to run decoupled from external databases using an in-memory oracle, or ‘’mock’’, database replacement;\ 
>or it can run against the test- or run-time database.\ 
>The functional specification of the application, perhaps in use cases, is made against the inner hexagon’s interface and not against any one of the external technologies that might be used.

Figure2 는 두 개의 활성 포트와 각각 여러 어댑터를 가진 애플리케이션을 보여준다.

두 개의 포트는 애플리케이션을 제어하는 측면과 데이터 검색 측면이다.

이 그림은 애플리케이션이 자동화된 시스템 수준 회귀 테스트 스위트, 인간 사용자, 원격 HTTP 애플리케이션 또는 다른 로컬 애플리케이션에 의해 동일하게 구동될 수 있음을 보여준다.

데이터 측면에서는, 애플리케이션이 메모리 내 오라클 또는 '모의' 데이터베이스 대체를 사용하여 외부 데이터베이스와 분리된 상태로 실행되도록 구성될 수 있으며,
또는 테스트용 또는 실제 실행 시 데이터베이스를 대상으로 실행할 수도 있다.

애플리케이션의 기능 사양은 아마도 유스케이스에서 내부 헥사곤의 인터페이스를 기준으로 작성되며, 사용될 수 있는 외부 기술 중 특정 기술을 기준으로 하지는 않는다.

 ![image](https://github.com/user-attachments/assets/e287f487-839b-4582-bb84-17af817ce8e7)
reference : https://alistair.cockburn.us/hexagonal-architecture/

Figure3 - 헥사고날 아키텍처 barn door 이미지.

>Figure 3 shows the same application mapped to a three-layer architectural drawing.\ 
>To simplify the drawing only two adapters are shown for each port. This drawing is intended to show how multiple adapters fit in the top and bottom layers, and the sequence in which the various adapters are used during system development.\ 
>The numbered arrows show the order in which a team might develop and use the application.

Figure3 는 동일한 애플리케이션을 삼계층 아키텍처 도식에 매핑한 모습을 보여준다.  

그림을 단순화하기 위해 각 포트에 두 개의 어댑터만을 표시했다.  

이 그림은 상위 및 하위 계층에 여러 어댑터가 어떻게 맞춰지는지, 그리고 시스템 개발 중 다양한 어댑터가 사용되는 순서를 보여주기 위한 것이다.  

번호가 매겨진 화살표는 팀이 애플리케이션을 개발하고 사용하는 순서를 보여준다.

1. With a FIT test harness driving the application and using the mock (in-memory) database substituting for the real database;
2. Adding a GUI to the application, still running off the mock database;
3. In integration testing, with automated test scripts (e.g., from Cruise Control) driving the application against a real database containing test data;
4. In real use, with a person using the application to access a live database.

FIT 테스트 하네스가 애플리케이션을 구동하고 모의(메모리 내) 데이터베이스로 실제 데이터베이스를 대체하는 경우.

여전히 모의 데이터베이스를 실행하면서 애플리케이션에 GUI를 추가한 경우.

통합 테스트에서 자동화된 테스트 스크립트(Cruise Control 등)를 사용하여 테스트 데이터를 포함한 실제 데이터베이스로 애플리케이션을 구동한 경우.

실제 사용에서 사용자가 애플리케이션을 사용하여 실제 데이터베이스에 액세스하는 경우.

### Sample Code 
---

>The simplest application that demonstrates the ports & adapters fortunately comes with the FIT documentation.\ 
>It is a simple discount computing application:

포트와 어댑터를 보여주는 가장 간단한 애플리케이션은 다행히도 FIT 문서에 포함되어 있다.

이것은 간단한 할인 계산 애플리케이션이다:

>discount(amount) = amount * rate(amount);

>In our adaptation, the amount will come from the user and the rate will come from a database, so there will be two ports.\ 
>We implement them in stages:

우리의 적용에서는 금액이 사용자로부터 입력되고 비율은 데이터베이스에서 가져올 것이므로 두 개의 포트가 필요하다.  

이것을 단계적으로 구현한다.

- With tests but with a constant rate instead of a mock database,
- then with the GUI,
- then with a mock database that can be swapped out for a real database.

 모의 데이터베이스 대신 상수 비율을 사용하는 테스트 작성.

그런 다음 GUI 추가.

실제 데이터베이스를 대체할 수 있는 모의 데이터베이스 추가.

### Stage 1: FIT App constant-as-mock-database
---

>First we create the test cases as an HTML table (see the FIT documentation for this):

먼저 HTML 표로 테스트 케이스를 작성한다(FIT 문서를 참조):

|   |   |
|---|---|
|TestDiscounter||
|amount|discount()|
|100|5|
|200|10|

>Note that the column names will become class and function names in our program.\ 
>FIT contains ways to get rid of this “programmerese”, but for this article it is easier just to leave them in.

열 이름은 프로그램 내의 클래스 및 함수 이름이 될 것이다.

FIT에는 이러한 "프로그래머 언어"를 제거하는 방법이 있지만, 이 기사에서는 그냥 두는 것이 더 쉬운 방법이다.

>Knowing what the test data will be, we create the user-side adapter, the ColumnFixture that comes with FIT as shipped

테스트 데이터가 무엇인지 알기 때문에, 사용자의 어댑터, FIT와 함께 제공되는 ColumnFixture를 생성한다:

```java

import fit.ColumnFixture; 
public class TestDiscounter extends ColumnFixture 
{ 
   private Discounter app = new Discounter(); 
   public double amount;
   public double discount() 
   { return app.discount(amount); } 
}
```

>That’s actually all there is to the adapter.\ 
>So far, the tests run from the command line (see the FIT book for the path you’ll need).\ 
>We used this one

이것이 어댑터의 전부이다.  

지금까지 테스트는 명령줄에서 실행된다(FIT 책에서 필요한 경로를 확인할 것).  

우리는 이 명령을 사용했다.

```conesole
set FIT_HOME=/FIT/FitLibraryForFit15Feb2005
java -cp %FIT_HOME%/lib/javaFit1.1b.jar;%FIT_HOME%/dist/fitLibraryForFit.jar;src;bin
fit.FileRunner test/Discounter.html TestDiscount_Output.html
```

>FIT produces an output file with colors showing us what passed (or failed, in case we made a typo somewhere along the way).

FIT는 통과한 항목(또는 중간에 오타가 있으면 실패한 항목)을 색으로 표시하여 결과 파일을 생성한다.

>At this point the code is ready to check in, hook into Cruise Control or your automated build machine, and include in the build-and-test suite.

이 시점에서 코드는 체크인하고, Cruise Control 또는 자동 빌드 머신에 연결하여 빌드 및 테스트 스위트에 포함할 준비가 되었다.

### Stage 2: UI App constant-as-mock-database
---

>I’m going to let you create your own UI and have it drive the Discounter application, since the code is a bit long to include here. Some of the key lines in the code are these

UI를 직접 생성하여 Discounter 애플리케이션을 구동하도록 하겠습니다.  

코드가 여기에 포함하기에는 조금 길기 때문에 몇 가지 핵심 코드를 보여드리겠습니다:

```java
...
Discounter app = new Discounter();
public void actionPerformed(ActionEvent event) 
{
    ...
   String amountStr = text1.getText();
   double amount = Double.parseDouble(amountStr);
   discount = app.discount(amount));
   text3.setText( "" + discount );
   ...
```

>At this point the application can be both demoed and regression tested. The user-side adapters are both running.

이 시점에서 애플리케이션은 데모와 회귀 테스트 모두 가능해집니다.  
사용자 측 어댑터는 모두 실행 중입니다.
#### Stage 3: (FIT or UI) App mock database
---

>To create a replaceable adapter for the database side, we create an ‘’interface’’ to a repository, a ‘’RepositoryFactory’’ that will produce either the mock database or the real service object, and the in-memory mock for the database.

데이터베이스 측에 교체 가능한 어댑터를 만들기 위해, 우리는 저장소(repository)에 대한 '인터페이스'와 모의 데이터베이스 또는 실제 서비스 객체를 생성할 'RepositoryFactory'를 만들고, 메모리 내 모의 데이터베이스를 구현합니다.

```java

public interface RateRepository 
{
   double getRate(double amount);
 }
public class RepositoryFactory 
{
   public RepositoryFactory() {  super(); }
   public static RateRepository getMockRateRepository() 
   {
      return new MockRateRepository();
   }
}
public class MockRateRepository implements RateRepository 
{
   public double getRate(double amount) 
   {
      if(amount <= 100) return 0.01;
      if(amount <= 1000) return 0.02;
      return 0.05;
    }
 }
 
```

>To hook this adapter into the Discounter application, we need to update the application itself to accept a repository adapter to use, and the have the (FIT or UI) user-side adapter pass the repository to use (real or mock) into the constructor of the application itself.\ 
>Here is the updated application and a FIT adapter that passes in a mock repository (the FIT adapter code to choose whether to pass in the mock or real repository’s adapter is longer without adding much new information, so I omit that version here).

이 어댑터를 Discounter 애플리케이션에 연결하려면, 애플리케이션 자체를 업데이트하여 사용할 저장소 어댑터를 받도록 하고, (FIT 또는 UI) 사용자 측 어댑터가 실제 또는 모의 저장소를 애플리케이션의 생성자로 전달하도록 해야 합니다.

여기에 업데이트된 애플리케이션 코드와 모의 저장소를 전달하는 FIT 어댑터를 보여드리겠습니다.

(모의 또는 실제 저장소 어댑터를 전달할지 선택하는 FIT 어댑터 코드는 추가적인 정보를 제공하지 않으므로, 해당 버전은 생략하겠습니다).

```java
import repository.RepositoryFactory;
import repository.RateRepository;
public class Discounter 
{
   private RateRepository rateRepository;
   public Discounter(RateRepository r) 
   {
      super();
      rateRepository = r;
    }
   public double discount(double amount) 
   {
      double rate = rateRepository.getRate( amount ); 
      return amount * rate;
    }
}
import app.Discounter;
import fit.ColumnFixture;
public class TestDiscounter extends ColumnFixture 
{
   private Discounter app = 
       new Discounter(RepositoryFactory.getMockRateRepository());
   public double amount;
   public double discount() 
   {
      return app.discount( amount );
   }
}

```

>That concludes implementation of the simplest version of the hexagonal architecture.

이것으로 가장 간단한 버전의 헥사고날 아키텍처 구현이 완료됩니다.

>For a different implementation, using Ruby and Rack for browser usage, see

브라우저 사용을 위한 Ruby와 Rack을 이용한 다른 구현에 대해서는 참조하시기 바랍니다.

### Application Notes

#### The Left-Right Asymmetry

>The ports and adapters pattern is deliberately written pretending that all ports are fundamentally similar. That pretense is useful at the architectural level.\ 
>In implementation, ports and adapters show up in two flavors, which I’ll call ‘’primary’’ and ‘’secondary’’, for soon-to-be-obvious reasons\ 
>They could be also called ‘’driving’’ adapters and ‘’driven’’ adapters.

포트와 어댑터 패턴은 모든 포트가 근본적으로 유사하다는 가정을 기반으로 의도적으로 작성되었다. 

이 가정은 아키텍처 수준에서는 유용하다. 

구현에서는 포트와 어댑터가 두 가지 형태로 나타나는데, 곧 명확해질 이유로 이를 'primary'와 'secondary'이라고 부르겠다. 

또는 'driving' 어댑터와 'driven' 어댑터라고 부를 수도 있다.

>The alert reader will have noticed that in all the examples given, FIT fixtures are used on the left-side ports and mocks on the right.\ 
>In the three-layer architecture, FIT sits in the top layer and the mock sits in the bottom layer.

주의 깊은 독자는 모든 예제에서 FIT 픽스처가 좌측 포트에 사용되고,  
모의 객체(mock)가 우측 포트에 사용된다는 것을 알아차렸을 것이다.

three-layer architecture 에서는 FIT가 상위 계층에, 모의 객체는 하위 계층에 위치한다.

>This is related to the idea from use cases of “primary actors” and “secondary actors”. A ‘’primary actor’’ is an actor that drives the application (takes it out of quiescent state to perform one of its advertised functions).\ 
>A ‘’secondary actor’’ is one that the application drives, either to get answers from or to merely notify.\ 
>The distinction between ‘’primary ‘’and’’ secondary ‘’lies in who triggers or is in charge of the conversation.

이는 유스케이스에서 "primary actors"와 "secondary actors"라는 개념과 관련이 있다.  

"primary actors"는 애플리케이션을 구동하는 actor(광고된 기능 중 하나를 수행하기 위해 정지 상태에서 꺼내는 역할)이다.  

"secondary actors" 는 애플리케이션이 구동하는 대상이며, 답변을 얻기 위해서든 단순히 알리기 위해서든 사용된다.  

'primary'와 'secondary'의 차이는 대화의 트리거 또는 주도권이 누구에게 있는지에 달려 있다.

>The natural test adapter to substitute for a ‘’primary’’ actor is FIT, since that framework is designed to read a script and drive the application.\ 
>The natural test adapter to substitute for a ‘’secondary’’ actor such as a database is a mock, since that is designed to answer queries or record events from the application.

"primary actor"를 대체할 적합한 테스트 어댑터는 FIT이다.  
이 프레임워크는 스크립트를 읽고 애플리케이션을 구동하도록 설계되어 있기 때문이다.  

데이터베이스와 같은 secondary actor를 대체할 적합한 테스트 어댑터는 모의 객체이다.  

모의 객체는 애플리케이션의 질의에 답하거나 이벤트를 기록하도록 설계되었기 때문이다.

>These observations lead us to follow the system’s use case context diagram and draw the ‘’primary ports ‘’and’’ primary adapters’’ on the left side (or top) of the hexagon, and the ‘’secondary ports’’ and ‘’secondary adapters’’ on the right (or bottom) side of the hexagon.

이러한 관찰을 통해 우리는 시스템의 유스케이스 컨텍스트 다이어그램을 따르고,  
'주요 포트'와 '주요 어댑터'를 헥사곤의 좌측(또는 상단)에,  
'부차적 포트'와 '부차적 어댑터'를 헥사곤의 우측(또는 하단)에 배치한다.

>The relationship between primary and secondary ports/adapters and their respective implementation in FIT and mocks is useful to keep in mind, but it should be used as a consequence of using the ports and adapters architecture, not to short-circuit it.\ 
>The ultimate benefit of a ports and adapters implementation is the ability to run the application in a fully isolated mode.

주요 포트/어댑터와 부차적 포트/어댑터 간의 관계 및 FIT와 모의 객체로의 각각의 구현 관계를 염두에 두는 것이 유용하다.  

그러나 이것은 포트와 어댑터 아키텍처를 사용하는 결과로 활용되어야 하며,  
이를 우회하는 방식으로 사용되어서는 안 된다.  

포트와 어댑터 구현의 궁극적인 이점은 애플리케이션을 완전히 독립된 모드에서 실행할 수 있다는 점이다.
#### Use Cases And The Application Boundary

>It is useful to use the hexagonal architecture pattern to reinforce the preferred way of writing use cases.

헥사고날 아키텍처 패턴을 사용하여 유스케이스 작성의 권장 방법을 강화하는 것은 유용하다.

>A common mistake is to write use cases to contain intimate knowledge of the technology sitting outside each port. These use cases have earned a justifiably bad name in the industry for being long, hard-to-read, boring, brittle, and expensive to maintain.

일반적인 실수는 각 포트 외부에 위치한 기술에 대한 상세한 지식을 유스케이스에 포함시키는 것이다.

이러한 유스케이스는 길고, 읽기 어렵고, 지루하며, 유지 비용이 많이 들고 불안정하기 때문에 업계에서 정당하게 나쁜 평판을 얻게 되었다.

>Understanding the ports and adapters architecture, we can see that the use cases should generally be written at the application boundary (the inner hexagon), to specify the functions and events supported by the application, regardless of external technology.\ 
>These use cases are shorter, easier to read, less expensive to maintain, and more stable over time.

포트와 어댑터 아키텍처를 이해하면 유스케이스는 일반적으로  
애플리케이션 경계(내부 헥사곤)에서 작성되어야 한다는 것을 알 수 있으며,  
외부 기술과는 상관없이 애플리케이션이 지원하는 기능과 이벤트를 명시해야 한다.

이러한 유스케이스는 더 짧고 읽기 쉬우며, 유지 비용이 적게 들고 시간이 지나도 더 안정적이다.
#### How Many Ports?

>What exactly a port is and isn’t is largely a matter of taste. At the one extreme, every use case could be given its own port, producing hundreds of ports for many applications.\
>Alternatively, one could imagine merging all primary ports and all secondary ports so there are only two ports, a left side and a right side.

포트가 정확히 무엇인지 아닌지는 대부분 취향의 문제이다. 

한 극단적으로는 모든 유스케이스에 자체 포트를 부여하여 수많은 애플리케이션에 대해 수백 개의 포트를 생성할 수 있다.

반면에, 모든 주요 포트와 모든 부차적 포트를 병합하여 좌측과 우측에 두 개의 포트만 존재할 수도 있다.

>Neither extreme appears optimal.

어느 극단도 최적이라고 보이지는 않는다.

>The weather system described in the Known Uses has four natural ports: the weather feed, the administrator, the notified subscribers, the subscriber database.\ 
>A coffee machine controller has four natural ports: the user, the database containing the recipes and prices, the dispensers, and the coin box.\ 
>A hospital medication system might have three: one for the nurse, one for the prescription database, and one for the computer-controller medication dispensers.

알려진 사용 예시에서 설명된 기상 시스템은 네 개의 자연스러운 포트를 가지고 있다.

기상 정보 피드, 관리자, 알림을 받은 구독자들, 구독자 데이터베이스.  
커피 머신 컨트롤러는 네 개의 자연스러운 포트를 가지고 있다

사용자, 레시피와 가격이 들어 있는 데이터베이스, 디스펜서, 그리고 동전함. 

병원 약물 시스템은 세 개의 포트를 가질 수 있다

간호사, 처방 데이터베이스, 컴퓨터로 제어되는 약물 디스펜서.

>It doesn’t appear that there is any particular damage in choosing the “wrong” number of ports, so that remains a matter of intuition.\ 
>My selection tends to favor a small number, two, three or four ports, as described above and in the Known Uses.

'잘못된' 포트의 개수를 선택해도 특별한 손해가 발생하지 않는 것으로 보이므로,  이는 직관의 문제로 남는다.  

내 선택은 위에서 설명한 바와 같이, 그리고 알려진 사용 예시에서처럼, 
두 개, 세 개 또는 네 개의 포트로 적은 수를 선호하는 경향이 있다.

### Known Uses

![image](https://github.com/user-attachments/assets/8236faae-8907-45eb-ae54-a6a5fee7f67a)

Figure 4 

>Figure 4 shows an application with four ports and several adapters at each port.\ 
>This was derived from an application that listened for alerts from the national weather service about earthquakes, tornadoes, fires and floods, and notified people on their telephones or telephone answering machines.\ 
>At the time we discussed this system, the system’s interfaces were identified and discussed by ‘’technology, linked to purpose’’.\ 
>There was an interface for trigger-data arriving over a wire feed, one for notification data to be sent to answering machines, an administrative interface implemented in a GUI, and a database interface to get their subscriber data.

Figure 4는 각 포트에 여러 어댑터가 있는 네 개의 포트를 가진 애플리케이션을 보여준다.

이 애플리케이션은 국가 기상청에서 지진, 토네이도, 화재 및 홍수에 대한 경고를 듣고, 사람들에게 전화나 전화 응답기를 통해 알리는 시스템에서 파생되었다.

우리가 이 시스템에 대해 논의할 당시, 시스템의 인터페이스는 '목적과 연결된 기술'에 의해 식별되고 논의되었다.

여기에는 전선 피드를 통해 들어오는 트리거 데이터에 대한 인터페이스, 응답기에 보낼 알림 데이터 인터페이스, GUI로 구현된 관리 인터페이스, 그리고 구독자 데이터를 얻기 위한 데이터베이스 인터페이스가 있었다.

>The people were struggling because they needed to add an http interface from the weather service, an email interface to their subscribers, and they had to find a way to bundle and unbundle their growing application suite for different customer purchasing preferences.\ 
>They feared they were staring at a maintenance and testing nightmare as they had to implement, test and maintain separate versions for all combinations and permutations.

사람들은 날씨 서비스로부터 http 인터페이스를 추가해야 하고, 구독자들에게 이메일 인터페이스를 추가해야 했으며,  
다양한 고객 구매 선호도에 맞춰 애플리케이션 모음을 번들로 묶거나 풀 방법을 찾아야 했기 때문에 어려움을 겪고 있었다.

그들은 모든 조합과 순열을 구현, 테스트 및 유지해야 한다는 생각에 유지보수 및 테스트 나이트메어를 마주하고 있다고 두려워했다.

>Their shift in design was to architect the system’s interfaces ‘’by purpose’’ rather than by technology, and to have the technologies be substitutable (on all sides) by adapters.\ 
>They immediately picked up the ability to include the http feed and the email notification (the new adapters are shown in the drawing with dashed lines).\ 
>By making each application executable in headless mode through APIs, they could add an app-to-add adapter and unbundle the application suite, connecting the sub-applications on demand.\ 
>Finally, by making each application executable completely in isolation, with test and mock adapters in place, they gained the ability to regression test their applications with stand-alone automated test scripts.

그들의 설계 전환은 시스템의 인터페이스를 '기술'이 아닌 '목적'에 맞게 아키텍처를 설계하고, 어댑터를 통해 기술을 대체 가능하게 하는 것이었다. 

그들은 즉시 http 피드와 이메일 알림을 포함할 수 있는 능력을 얻었으며(새 어댑터는 점선으로 표시됨), 각 애플리케이션을 API를 통해 헤드리스 모드로 실행 가능하게 함으로써, 애플리케이션 모음을 분리하고 필요에 따라 하위 애플리케이션을 연결할 수 있게 되었다.

마지막으로, 각 애플리케이션을 완전히 독립적으로 실행할 수 있게 하여, 테스트 및 모의 어댑터를 통해 독립적인 자동화 테스트 스크립트로 애플리케이션을 회귀 테스트할 수 있는 능력을 얻었다.

#### Mac, Windows, Google, Flickr, Web 2.0
---

>In the early 1990s, MacIntosh applications such as word processor applications were required to have API-drivable interfaces, so that applications and user-written scripts could access all the functions of the applications. Windows desktop applications have evolved the same ability (I don’t have the historical knowledge to say which came first, nor is that relevant to the point).

1990년대 초, 맥킨토시 애플리케이션(워드 프로세서 애플리케이션 등)은 애플리케이션과 사용자가 작성한 스크립트가 애플리케이션의 모든 기능에 접근할 수 있도록 API 구동 가능한 인터페이스를 가져야 했다.

윈도우 데스크톱 애플리케이션도 같은 기능을 발전시켰다(어느 쪽이 먼저인지는 알 수 없으며, 그 점은 중요하지 않다).

>The current (2005) trend in web applications is to publish an API and let other web applications access those APIs directly. Thus, it is possible to publish local crime data over a Google map, or create web applications that include Flickr’s photo archiving and annotating abilities.

현재(2005년) 웹 애플리케이션의 트렌드는 API를 공개하고 다른 웹 애플리케이션이 해당 API에 직접 접근할 수 있게 하는 것이다.

따라서 구글 맵을 통해 지역 범죄 데이터를 공개하거나, Flickr의 사진 보관 및 주석 기능을 포함하는 웹 애플리케이션을 만들 수 있다.

>All of these examples are about making the ‘’primary ‘’ports’ APIs visible. We see no information here about the secondary ports.

이 모든 예시는 '주요 포트' API를 가시화하는 것에 관한 것이다.

여기에서는 부차적 포트에 대한 정보는 볼 수 없다.

#### Stored Outputs
---

>This example written by Willem Bogaerts on the C2 wiki:

>“I encountered something similar, but mainly because my application layer had a strong tendency to become a telephone switchboard that managed things it should not do. My application generated output, showed it to the user and then had some possibility to store it as well. My main problem was that you did not need to store it always. So my application generated output, had to buffer it and present it to the user. Then, when the user decided that he wanted to store the output, the application retrieved the buffer and stored it for real.

"나는 비슷한 문제를 겪었다.  
주로 내 애플리케이션 레이어가 전화 교환기처럼 행동하면서 애플리케이션이 수행하지 말아야 할 것들을 관리하려고 했기 때문이다.

내 애플리케이션은 출력을 생성하고 사용자에게 표시한 다음 이를 저장할 수 있는 기능도 있었다.

하지만 항상 저장할 필요는 없었기 때문에, 애플리케이션은 출력을 생성하고 버퍼에 저장한 후 사용자에게 보여줬다.

그리고 사용자가 출력을 저장하기로 결정하면, 애플리케이션이 버퍼를 가져와 실제로 저장했다.

>I did not like this at all. Then I came up with a solution: Have a presentation control with storage facilities. Now the application no longer channels the output in different directions, but it simply outputs it to the presentation control. It’s the presentation control that buffers the answer and gives the user the possibility to store it.

나는 이 방식이 마음에 들지 않았다. 

그래서 해결책을 생각해냈다: 저장 기능이 포함된 프레젠테이션 컨트롤을 사용하자.

이제 애플리케이션은 더 이상 출력을 여러 방향으로 전달하지 않고, 단순히 프레젠테이션 컨트롤로 출력하게 되었다.

프레젠테이션 컨트롤이 응답을 버퍼링하고 사용자가 저장할 수 있는 가능성을 제공한다.

>The traditional layered architecture stresses “UI” and “storage” to be different. The Ports and Adapters Architecture can reduce output to being simply “output” again. ”

전통적인 계층형 아키텍처는 'UI'와 '저장'을 별개로 강조한다.

포트와 어댑터 아키텍처는 출력을 단순히 '출력'으로 줄일 수 있다."
#### Anonymous example from the C2-wiki
---

>“In one project I worked on, we used the SystemMetaphor of a component stereo system. Each component has defined interfaces, each of which has a specific purpose. We can then connect components together in almost unlimited ways using simple cables and adapters.”

"내가 작업했던 한 프로젝트에서, 우리는 컴포넌트 오디오 시스템이라는 SystemMetaphor(시스템 은유)를 사용했다.

각 컴포넌트에는 정의된 인터페이스가 있으며, 각 인터페이스는 특정 목적을 가지고 있다.

그런 다음 우리는 단순한 케이블과 어댑터를 사용하여 거의 무제한으로 컴포넌트를 연결할 수 있다."

#### Distributed, Large-Team Development
---

>This one is still in trial use and so does not properly count as a use of the pattern. However, it is interesting to consider.

이것은 아직 시험 중이므로 패턴의 사용으로 제대로 간주되지 않는다.  
그러나 흥미롭게 고려할 만한 사항이다.

>Teams in different locations all build to the Hexagonal architecture, using FIT and mocks so the applications or components can be tested in standalone mode. The CruiseControl build runs every half hour and runs all the applications using the FIT+mock combination. As application subsystem and databases get completed, the mocks are replaced with test databases.

다른 위치에 있는 팀들이 모두 헥사고날 아키텍처에 맞춰 작업하고, FIT와 모의 객체를 사용하여 애플리케이션 또는 컴포넌트를 독립 모드에서 테스트할 수 있다. 

CruiseControl 빌드는 30분마다 실행되며, 모든 애플리케이션을 FIT+모의 객체 조합으로 실행한다. 

애플리케이션 하위 시스템과 데이터베이스가 완료되면, 모의 객체는 테스트 데이터베이스로 대체된다.
#### Separating Development of UI and Application Logic
---

>This one is still in early trial use and so does not count as a use of the pattern. However, it is interesting to consider.

이것은 아직 초기 시험 중이므로 패턴의 사용으로 간주되지 않는다.  
그러나 흥미롭게 고려할 만한 사항이다.

>The UI design is unstable, as they haven’t decided on a driving technology or a metaphor yet. The back-end services architecture hasn’t been decided, and in fact will probably change several times over the next six months. Nonetheless, the project has officially started and time is ticking by.

UI 디자인은 불안정하며, 구동 기술이나 은유를 결정하지 못했다.  
백엔드 서비스 아키텍처도 결정되지 않았으며, 사실 향후 6개월 동안 여러 차례 변경될 가능성이 크다.  
그럼에도 불구하고 프로젝트는 공식적으로 시작되었고 시간은 흐르고 있다.

>The application team creates FIT tests and mocks to isolate their application, and creates testable, demonstrable functionality to show their users. When the UI and back-end services decisions finally get met, it “should be straightforward” to add those elements the application. Stay tuned to learn how this works out (or try it yourself and write me to let me know).

애플리케이션 팀은 FIT 테스트와 모의 객체를 생성하여 애플리케이션을 분리하고, 사용자에게 보여줄 수 있는 테스트 가능한 기능을 생성한다.  
UI와 백엔드 서비스 결정이 최종적으로 이루어지면, 그 요소들을 애플리케이션에 추가하는 것은 '간단할 것'이다.  
결과가 어떻게 나오는지 지켜보거나 직접 시도해보고 나에게 알려달라.

### Related Patterns

#### Adapter

>The ‘’Design Patterns’’ book contains a description of the generic ‘’Adapter’’ pattern: “Convert the interface of a class into another interace clients expect.” The ports-and-adapters pattern is a particular use of the ‘’Adapter’’ pattern.

'디자인 패턴' 책에는 일반적인 '어댑터' 패턴에 대한 설명이 있다.

"클라이언트가 기대하는 다른 인터페이스로 클래스의 인터페이스를 변환하라."  

포트와 어댑터 패턴은 '어댑터' 패턴의 특정한 사용 사례이다.
#### Model-View-Controller

>The MVC pattern was implemented as early as 1974 in the Smalltalk project. It has been given, over the years, many variations, such as Model-Interactor and Model-View-Presenter. Each of these implements the idea of ports-and-adapters on the primary ports, not the secondary ports.

MVC 패턴은 1974년 Smalltalk 프로젝트에서 처음 구현되었다.

그 후, 모델-인터랙터, 모델-뷰-프레젠터와 같은 여러 변형들이 생겨났다.  

이들 각각은 주요 포트에 대한 포트와 어댑터의 아이디어를 구현했으며, 부차적 포트에는 해당하지 않는다.
#### Mock Objects and Loopback

>“A mock object is a “double agent” used to test the behaviour of other objects. First, a mock object acts as a faux implementation of an interface or class that mimics the external behaviour of a true implementation. Second, a mock object observes how other objects interact with its methods and compares actual behaviour with preset expectations. When a discrepancy occurs, a mock object can interrupt the test and report the anomaly. If the discrepancy cannot be noted during the test, a verification method called by the tester ensures that all expectations have been met or failures reported.” — From [http://MockObjects.com](http://mockobjects.com/)

"모의 객체는 다른 객체의 동작을 테스트하기 위해 사용되는 '이중 요원'이다.  

첫째, 모의 객체는 인터페이스나 클래스를 가짜로 구현한 것이며,  
실제 구현체의 외부 동작을 모방한다.  

둘째, 모의 객체는 다른 객체가 자신의 메서드와 상호작용하는 방식을 관찰하고,  
실제 동작을 미리 설정된 기대치와 비교한다.  

불일치가 발생하면, 모의 객체는 테스트를 중단하고 이상을 보고할 수 있다.  

테스트 중에 불일치를 기록할 수 없으면,  
테스터가 호출하는 검증 메서드가 모든 기대치가 충족되었는지 또는 실패가 보고되었는지 확인한다."  

— 출처: [http://MockObjects.com](http://MockObjects.com)

>Fully implemented according to the mock-object agenda, mock objects are used throughout an application, not just at the external interface The primary thrust of the mock object movement is conformance to specified protocol at the individual class and object level. I borrow their word “mock” as the best short description of an in-memory substitute for an external secondary actor.

모의 객체의 의제에 따라 완전히 구현된 모의 객체는 애플리케이션 전반에 걸쳐 사용되며, 외부 인터페이스에만 한정되지 않는다.  
모의 객체 운동의 주요 목표는 개별 클래스 및 객체 수준에서 지정된 프로토콜 준수이다.  

>The Loopback pattern is an explicit pattern for creating an internal replacement for an external device.

나는 외부 부차적 배우의 메모리 내 대체물에 대한 가장 간결한 설명으로 '모의'라는 단어를 차용한다.
#### Pedestals

>In “Patterns for Generating a Layered Architecture”, Barry Rubel describes a pattern about creating an axis of symmetry in control software that is very similar to ports and adapters. The ‘’Pedestal’’ pattern calls for implementing an object representing each hardware device within the system, and linking those objects together in a control layer. The ‘’Pedestal’’ pattern can be used to describe either side of the hexagonal architecture, but does not yet stress the similarity across adapters. Also, being written for a mechanical control environment, it is not so easy to see how to apply the pattern to IT applications.

'계층형 아키텍처 생성 패턴'에서 Barry Rubel은 포트와 어댑터와 매우 유사한 제어 소프트웨어의 대칭 축을 만드는 패턴에 대해 설명한다.  

'페데스탈' 패턴은 시스템 내의 각 하드웨어 장치를 나타내는 객체를 구현하고, 제어 계층에서 해당 객체를 연결하는 것을 요구한다.  

'페데스탈' 패턴은 헥사고날 아키텍처의 양측을 설명하는 데 사용할 수 있지만, 어댑터 간의 유사성을 강조하지는 않는다.  

또한 기계 제어 환경을 위해 작성되었기 때문에 IT 애플리케이션에 패턴을 적용하는 방법을 쉽게 파악할 수는 없다.
#### Checks

>Ward Cunningham’s pattern language for detecting and handling user input errors, is good for error handling across the inner hexagon boundaries.

Ward Cunningham의 사용자 입력 오류 감지 및 처리 패턴 언어는 내부 헥사곤 경계 전반에 걸쳐 오류를 처리하는 데 유용하다.
#### Dependency Inversion (Dependency Injection) and SPRING

>Bob Martin’s Dependency Inversion Principle (also called Dependency Injection by Martin Fowler) states that “High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.” The ‘’Dependency Injection ‘’pattern by Martin Fowler gives some implementations. These show how to create swappable secondary actor adapters. The code can be typed in directly, as done in the sample code in the article, or using configuration files and having the SPRING framework generate the equivalent code.

Bob Martin의 의존성 역전 원칙(Dependency Inversion Principle)은  
"고수준 모듈은 저수준 모듈에 의존해서는 안 된다.  

둘 다 추상화에 의존해야 한다.  

추상화는 세부 사항에 의존해서는 안 되며,  
세부 사항이 추상화에 의존해야 한다."라고 명시한다.  

Martin Fowler의 '의존성 주입(Dependency Injection)' 패턴은 이에 대한 몇 가지 구현을 제공한다.  

이들은 부차적 배우 어댑터를 교체할 수 있는 방법을 보여준다.  
코드는 기사에 있는 샘플 코드처럼 직접 입력할 수 있거나,  
구성 파일을 사용하여 스프링(SPRING) 프레임워크가 동등한 코드를 생성하게 할 수도 있다.

### Configurable Dependencies, Primary and Secondary Actors
---

I tried to make this pattern truly symmetric, hence the hexagon. However, watching several implementations, it slowly became clear that there is an asymmetry (which is one thing that makes this fundamentally different from neighboring patterns such as the onion architecture). As stated above (see The Left-Right Asymmetry), the asymmetry matches Ivar Jacobson’s **primary** and **secondary actors** concept, and affects how the [Configurable Dependency](https://alistair.cockburn.us/Configurable+Dependency) [(discussion: Re: Configurable Dependency)](https://alistair.cockburn.us/Re%3a+Configurable+Dependency) is implemented. (This is shown briefly in the Configurable Dependency sketch:

The difference between a primary and secondary actor lies only in who _initiates_ the conversation. A **primary actor** knows about and initiates the conversation with the system or application; for a **secondary actor**, it is the system or application that knows about and initiates the discussion with the other. That is actually the only difference between the two, in use case land.

In implementation, that difference matters: Whomever will initiate the conversation must be handed the handle for the other.

In the case of **primary actor ports**, the macro constructor will pass to the UI, test framework, or driver the handle for the app and say, “Go talk to that”. The primary actor will call into the app, and the app will probably never know who called it. (That is normal for recipients of a call).

In stark contrast, for **secondary actor ports**, the macro constructor will pass to the UI, test framework, or driver the handle for the secondary actor to be used, that will get passed in as a parameter to the app, and the app will now know who/what is the recipient of the outgoing call. (This is again normal for sending out a call).

Thus, the system or application is constructed differently for primary and secondary actor ports: ignorant and initially passive for the primary actors, and having a way to store and call out to the secondary actor ports.

Both ports implement [Configurable Dependency](https://alistair.cockburn.us/Configurable+Dependency) [(discussion: Re: Configurable Dependency)](https://alistair.cockburn.us/Re%3a+Configurable+Dependency), but differently.

Simple example for a 3-port system, such as a coffee machine or a hospital medical unit dispensing medications intravenously (How odd that they come out the same, architecturally!):

- The purchaser, test harness or hospital admin is a primary actor driving the system
- The recipe database or medical database is a secondary actor, offering its information
- The chemical dispensers in either are secondary actors.

End result: the primary / secondary aspect of a port cannot be ignored.

https://alistaircockburn.com/Hexagonal%20Budapest%2023-05-18.pdf

