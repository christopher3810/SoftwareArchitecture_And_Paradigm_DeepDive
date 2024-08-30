
### SAGA 는 무슨 의미일까?
---

>SAGA는 1987년 Hector Garcia와 Kenneth Selem이 소개한글 Sagas에서 처음 소개되었다.
>[sagas in 1987 by hector gracia, kenneth Selem](https://www.cs.cornell.edu/andru/cs711/2002fa/reading/sagas.pdf)

Saga라는 패턴은 어떤 문제상황 때문에 발생 하였고 어떻게 해결하려 하는가?

Hector Garcia-Molina와 Kenneth Salem은 
데이터베이스 시스템에서 오랫동안 골치거리였던 장기 트랜잭션(Long-Lived Transaction, LLT)의 문제를 해결하기 위한 아이디어를 소개 했다. 

### Saga의 개념과 필요성에 대해서
---

왜 이름이 Saga일까?
위 논문을 보면 맨마지막 감사의 말에서
Saga라는 단어는 Bruce Lindsay 에의해서 제안된것으로 언급이되지만 정확히 어떤 의도로 사용되었다고 나와있지는 않다.

여러가지 추론을 해볼순 있는데.


#### 장기 트랜잭션(LLT)의 정의와 문제점
---

Saga는 LLT가 갖는 문제점을 해결하기 위해 접괸되는 패턴이다.

논문 도입부를 확인하며 LLT가 무엇이고 LLT는 어떤문제들을 야기하는지 확인해보자.

>the malorlty of other transactions either because it accesses many database obJects, it has lengthy computations, it pauses for inputs from the users, or a combmatlon of these factors Examples of LLTs are transactions to produce monthly account statements at a bank, transactions to process claims at an insurance company, and transactions to collect statrstlcs over an entire database [Graysla] In most cases, LLTs present serious performance problems Since they are transactions, the system must execute them as atomic actions, thus preserving the consistency of the database [DateSla,Ullm82a] To make a transaction atonuc, the system usually locks the objects accessed by the transaction until It commits, and this typically occurs at the end of the transactlon As a consequence, other transactions wishing to access the LLT’s objects suffer a long locking delay If LLTs are long because they access many database obJects then other transactions are likely to suffer from an mcreased blockmg rate as well, 1 e they are more likely to conflict with an LLT than with a shorter transaction Furthermore, the transaction abort rate can also be increased by LLTs As discussed m [Gray8lb], the frequency of deadlock 1s very sensitive to the “size” of transactions, that IS, to how many oblects transactions access (In the analysis of [GraySlb] the deadlock frequency grows with the fourth power of the transaction size ) Hence, since LLTs access many oblects, they may cause many deadlocks, and correspondingly, many abortions From the point of view of system crashes, LLTs have a higher probability of encountering a failure (because of their duration), and are thus more likely to encounter yet more delays and more likely to be aborted themselves


LLT는 실행 시간이 매우 긴 트랜잭션을 말한다.

LLT는 아래와 같은 이유로 발생한다.

1. 많은 데이터베이스 객체에 접근
2. 긴 계산 과정 필요
3. 사용자 입력 대기
4. 위의 요소들의 조합

LLT의 예시가 될수 있는 상황들

1. 은행의 월간 계좌 명세서 생성
2. 보험 회사의 청구 처리
3. 전체 데이터베이스에 대한 통계 수집

LLT가 야기하는 주요 문제점

1. 성능 저하
   LLT가 완료될 때까지 다른 트랜잭션들의 실행이 지연됨.
   
2. deadlock 증가
   LLT가 많은 객체에 접근하므로 다른 트랜잭션들과의 충돌 가능성이 높아짐.

3. transaction abort rate increase 
   LLT에 의해서 transaction abort rate(트랜잭션 중단률)이 증가할 수 있음.

#### 그렇다면 Saga는?
---

Saga를 이루는 핵심 아이디어는 LLT를 일련의 더 작은 트랜잭션들로 나누자는 것이다.

