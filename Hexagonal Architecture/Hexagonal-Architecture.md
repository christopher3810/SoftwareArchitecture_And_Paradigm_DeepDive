
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
>The business experts can create automated test cases, before the GUI details are finalized, that tells the programmers when they have done their work correctly (and these tests become the ones run by the test department).\

이제 애플리케이션이 제공하는 모든 기능이 API(애플리케이션 프로그래밍 인터페이스)나 함수 호출을 통해 사용할 수 있다고 상상해 보라.

이 상황에서는 테스트 또는 QA 부서가 자동화된 테스트 스크립트를 사용하여 새로 작성된 코드가 이전에 정상적으로 동작하던 기능을 깨뜨리지 않았는지 감지할 수 있다.

비즈니스 전문가들은 GUI 세부 사항이 확정되기 전에 자동화된 테스트 케이스를 생성하여 프로그래머가 작업을 제대로 완료했는지 알려주며, 이러한 테스트는 테스트 부서에서 실행되는 테스트가 된다.

>The application can be deployed in ‘’headless’’ mode, so only the API is available, and other programs can make use of its functionality — this simplifies the overall design of complex application suites and also permits business-to-business service applications to use each other without human intervention over the web.\ 
>Finally, the automated function regression tests detect any violation of the promise to keep business logic out of the presentation layer.\ 
>The organization can detect, and then correct, the logic leak.

애플리케이션은 ‘headless’ 모드로 배포될 수 있으므로 API만 사용할 수 있으며, 다른 프로그램이 이 기능을 사용할 수 있다.\ 

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
>The word “port” is supposed to evoke thoughts of ‘’ports’’ in an operating system, where any device that adheres to the protocols of a port can be plugged into it;\
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
Figure2

>Figure 2 shows an application having two active ports and several adapters for each port.\ 
>The two ports are the application-controlling side and the data-retrieval side.\ 
>This drawing shows that the application can be equally driven by an automated, system-level regression test suite, by a human user, by a remote http application, or by another local application.\ 
>On the data side, the application can be configured to run decoupled from external databases using an in-memory oracle, or ‘’mock’’, database replacement;\ 
>or it can run against the test- or run-time database.\ 
>The functional specification of the application, perhaps in use cases, is made against the inner hexagon’s interface and not against any one of the external technologies that might be used.



 ![image](https://github.com/user-attachments/assets/e287f487-839b-4582-bb84-17af817ce8e7)
reference : https://alistair.cockburn.us/hexagonal-architecture/
Figure3

>Figure 3 shows the same application mapped to a three-layer architectural drawing.\ To simplify the drawing only two adapters are shown for each port. This drawing is intended to show how multiple adapters fit in the top and bottom layers, and the sequence in which the various adapters are used during system development.\ 
>The numbered arrows show the order in which a team might develop and use the application:

1. With a FIT test harness driving the application and using the mock (in-memory) database substituting for the real database;
2. Adding a GUI to the application, still running off the mock database;
3. In integration testing, with automated test scripts (e.g., from Cruise Control) driving the application against a real database containing test data;
4. In real use, with a person using the application to access a live database.

### Sample Code 
---

>The simplest application that demonstrates the ports & adapters fortunately comes with the FIT documentation. It is a simple discount computing application:

>discount(amount) = amount * rate(amount);

>In our adaptation, the amount will come from the user and the rate will come from a database, so there will be two ports. We implement them in stages:

- With tests but with a constant rate instead of a mock database,
- then with the GUI,
- then with a mock database that can be swapped out for a real database.

_Thanks to Gyan Sharma at IHC for providing the code for this example._

### Stage 1: FIT App constant-as-mock-database
---

>First we create the test cases as an HTML table (see the FIT documentation for this):

|   |   |
|---|---|
|TestDiscounter||
|amount|discount()|
|100|5|
|200|10|

Note that the column names will become class and function names in our program. FIT contains ways to get rid of this “programmerese”, but for this article it is easier just to leave them in.

Knowing what the test data will be, we create the user-side adapter, the ColumnFixture that comes with FIT as shipped:

import fit.ColumnFixture; 
public class TestDiscounter extends ColumnFixture 
{ 
   private Discounter app = new Discounter(); 
   public double amount;
   public double discount() 
   { return app.discount(amount); } 
}

That’s actually all there is to the adapter. So far, the tests run from the command line (see the FIT book for the path you’ll need). We used this one:

set FIT_HOME=/FIT/FitLibraryForFit15Feb2005
java -cp %FIT_HOME%/lib/javaFit1.1b.jar;%FIT_HOME%/dist/fitLibraryForFit.jar;src;bin
fit.FileRunner test/Discounter.html TestDiscount_Output.html

FIT produces an output file with colors showing us what passed (or failed, in case we made a typo somewhere along the way).

At this point the code is ready to check in, hook into Cruise Control or your automated build machine, and include in the build-and-test suite.

https://alistaircockburn.com/Hexagonal%20Budapest%2023-05-18.pdf

