
> Object-Oriented Programming is Bad 라는 유튜브 영상에서\
> encapsulation 에 대해서 이야기하는 부분이 매우 흥미롭게 다가 와서 정리를 해보았다.

[Object-Oriented Programming is Bad (Brian Will Youtube)](https://www.youtube.com/watch?v=QM1iUe6IofM)

### why does OOP not work ? specifically encapsulation
---

>object is the bundle of encapsulated state and we don't interact with the state of that object directly.

객체는 캡슐화된 상태의 묶음이며, 우리는 그 객체의 상태와 직접 상호작용하지 않는다.


>All interactions with that state from the outside world come in through messages, messages to the object. 

외부 세계에서 그 상태와의 모든 상호작용은 객체에 대한 메시지, 즉 객체로 전달되는 메시지를 통해 이루어진다.


>the object has a defined set of messages which it will receive,\
>called its public interface.

객체는 받을 수 있는 메시지들의 정의된 집합을 가지고 있으며, 이를 public interface 라고 부른다.

>And so we have private information hidden behind the public interface. 

따라서 우리는 public interface 뒤에 숨겨진 private 한 information 을 가지게 된다.

>when an object receives a message,\
>it may in turn send messages to other objects.

객체가 메시지를 받으면, 그 객체는 차례로 다른 객체들에게 메시지를 보낼 수 있다.

>And so we can conceive of an object-oriented programming graph object all communicating with each other by sending messages. 

그래서 우리는 객체 지향 프로그래밍을 서로 메시지를 주고받으며 통신하는 객체들의 그래프로 생각할 수 있다.

>many people today forget, though ,that the original conception of a message is not exactly synonymous with just method call. 

하지만 오늘날 많은 사람들은 메시지의 original conception이 단순히 메서드 호출과 정확히 동의어가 아니라는 것을 잊고 있다.

>Yes, in practice it means calling methods, but a message strictly speaking sends only copies of states. 

실제로는 메서드를 호출한다는 의미지만, 엄밀히 말하면 메시지는 상태의 copy본만을 보내준다.

>It doesn't send references. 

references를 보내는것이 아니다.

>A message sends and returns information about state, 
>not state itself. 

메시지는 상태 자체가 아니라 상태에 대한 정보를 보내고 반환합니다.

>And while wait a minute, objects themselves are state. 

잠시동안  객체 자체가 상태이다.

>And this has some interesting consequences. 

이는 몇 가지 흥미로운 결과를 초래하는데.

>It means that strictly speaking messages cannot pass around object references. 

엄밀히 말하면, 메시지는 객체 참조를 주고받을 수 없다는 의미입니다.

>I've never seen a Java or C sharp code base that ever follow this rule. 

나는 이 규칙을 따르는 Java나 C# 코드베이스를 본 적이 없다.

>Perhaps some small talk programs have, but in general this rule is not observed at all,\
>and probably for good reason, as we'll discuss. 

아마도 일부 Smalltalk 프로그램들이 이를 따랐을 수 있지만, 일반적으로 이 규칙은 전혀 지켜지지 않습니다. 그리고 아마도 그럴 만한 이유가 있을 것이다. 

이에 대해 논의해 보자.

>But anyway, if we take the role seriously,\
>it means then for an object to send a message to another object, the first object must hold a private reference to that other object, because otherwise how is it going to talk to it? 

하지만 어쨌든, 이 규칙을 진지하게 받아들인다면,\
한 객체가 다른 객체에게 메시지를 보내려면,\
첫 번째 객체가 다른 객체에 대한 private 참조를 가지고 있어야 한다는 의미이다.\
그렇지 않으면 어떻게 그 객체와 메시지를 주고받을 수 있겠습니까?

>To talk to an object,\
>the object thereafter has a reference to it.  

객체와 메시지를 주고 받으려면,\
객체는 그에 대한 참조를 가져야 한다.

>And where is an object to need get a reference to another object if it can't get object references from messages.

그리고 메시지에서 객체 참조를 얻을 수 없다면, 객체는 어디서 다른 객체에 대한 참조를 얻어야 할까?

>the references which an object needs have to all be there at the object's inception.

객체가 필요로 하는 참조들은 모두 객체의 생성 시점에 존재해야 한다.

>they have to be there for the whole lifetime of the object, and there's an even deeper consequence.

이 참조들은 객체의 전체 수명 동안 존재해야 하며, 이로 인해 더 깊은 결과가 발생한다.

>which is that if an object is sending messages to another, that other object is part of the first object's private state.

즉, 한 객체가 다른 객체에게 메시지를 보내고 있다면,\
그 다른 객체는 첫 번째 객체의 비공개 상태의 일부이다.

>And by the principle of encapsulation,\
>an object should be responsible for all the objects which it sends messages to. 

그리고 캡슐화 원칙에 따르면,\
객체는 자신이 메시지를 보내는 모든 객체들에 대해 책임을 져야 한다.

![mmrams](https://github.com/user-attachments/assets/4e8af7b2-e13c-41a8-a1d5-a8bf91b66ad0)

>this should be obvious if you consider that messages indirectly read and modify state.

메시지가 간접적으로 상태를 읽고 수정한다는 점을 고려하면 이는 분명해진다.

>when B send a message to A , here it's messing with the state of A, indirectly sure,\ 
>but is still messing with it's state. 

 B가 A에게 메시지를 보낼 때, 여기서 B는 A의 상태를 간접적으로 건드리고 있다.\
 간접적이긴 하지만, 여전히 A의 상태를 건드리고 있는 것이다,

![mmrams2](https://github.com/user-attachments/assets/0d8bb9f6-c058-4a24-9000-e5f31b0b4abc)

>And so happens when other objects come along and send messages to that same object.

그리고 다른 객체들이 와서 같은 객체에 메시지를 보낼 때도 마찬가지이다.

>what's happening here?\
>we have shared state.

여기서 무슨 일이 일어나고 있나요? 
우리는 상태를 공유하고 있다.

>it's hardly any different than if you had a single global variable being shared by, say, ten functions.

이는 예를 들어 열 개의 함수가 공유하는 하나의 전역 변수를 가지고 있는 것과 거의 다르지 않다.

>if you have an object receiving messages from ten other objects, those objects are all effectively tied together because they're implicitly sharing this state.

만약 한 객체가 다른 열 개의 객체로부터 메시지를 받고 있다면,\
이 객체들은 모두 효과적으로 묶여 있다.\
왜냐하면 그들은 암묵적으로 이 상태를 공유하고 있기 때문이다.

>Sure, the interactions with that state indirect through public methods, but those methods are providing ver trivial kinds of coordination of the state. 

물론, 그 상태와의 상호작용은 공개 메서드를 통해 간접적으로 이루어진다.\
하지만 이 메서드들은 상태의 매우 사소한 종류의 조정만을 제공하고 있다,

>you can impose rules throught the access or methods, like saying if you access this field it's a number, well you can only increment that number, you can't mutate it in any other way. 

접근자나 메서드를 통해 규칙을 부과할 수 있다,\
예를 들어, 이 필드에 접근하면 그것은 숫자이고, 당신은 그 숫자를 증가시킬 수만 있고 다른 방식으로는 변경할 수 없다고 말할 수 있다.

>Find, but it's a very trivial kind of protection. 

좋습니다만, 이는 매우 사소한 종류의 보호이다.

>the hard problems of shared state are much, much deeper.

공유 상태의 어려운 문제들은 훨씬, 훨씬 더 복잡하고 깊은 사항이다.

>where in the system of ten objects all sharing the state is the real coordination? 

열 개의 객체가 모두 상태를 공유하는 시스템에서 실제 조정은 어디에 있을까?

>And the answer is, there isn't any. 

답은, 없다는 것이다.

>as soon as you have objects being shared,encapsulation just flies out the window. 

객체들이 공유되는 순간, 캡슐화는 그냥 창 밖으로 날아가 버린다.

>so if we're taking encapsulation seriously, the only real way to structure program, to structure our objects, as a graph. 

그래서 만약 우리가 캡슐화를 진지하게 받아들인다면, 프로그램을 구조화하고 우리의 객체들을 구조화하는 유일한 실제적인 방법은 그래프로서 달성할 수 있다.

![hierachy of references 1](https://github.com/user-attachments/assets/743bc71b-f8b3-4195-adb1-4a536a61bbbf)

>is not as a free-from graph, but as a strict hierarchy.

자유로운 그래프가 아니라, 엄격한 계층 구조로서 말이다.

>at the top of our hierarchy, we have an object representing effectively the whole program. 

우리 계층 구조의 최상위에는 효과적으로 전체 프로그램을 대표하는 객체가 있다.

>it's our god object, and that has its direct children which represent the sub components.

이것이 우리의 god 객체이고, 이 객체는 하위 구성 요소를 나타내는 직접적인 자식들을 가진다.

>And those children in turn have their own sub components, and so on down the line.

그리고 그 자식들은 차례로 자신의 하위 구성 요소를 가지며, 이런 식으로 계속 내려간다.

>And each object in the hierarchy is responsible for its direct children, and the message is being passed strictly only ever go from parent to their direct child.

그리고 계층 구조의 각 객체는 자신의 직접적인 자식들에 대해 책임을 지며, 메시지는 엄격하게 부모에서 직접적인 자식으로만 전달 된다.

>the god object here for example is not supposed to reach down to its grandchild. 

예를 들어, 여기서 신 객체는 손자에게 직접 접근해서는 안 된다.

![hierachy of references2](https://github.com/user-attachments/assets/d4897a6c-ef15-47a3-a47f-80e78ffb89f9)

>it has to do all of its interactions with this grandchild\
>indirectly through the gran child's parent. 

god 객체는 손자와의 모든 상호작용을 손자의 부모를 통해 간접적으로 해야 한다.

>Otherwise, who really is responsible for the object?

그렇지 않으면, 누가 정말로 그 객체에 대해 책임을 지는 걸까?

>who is managing its state? 

누가 그 상태를 관리하고 있나?

>supposed to be the direct parent. 

직접적인 부모여야 한다.

![cross cutting concerns](https://github.com/user-attachments/assets/211b0d53-6566-4663-861f-1ed5bbf2f244)

>And so what happens when we have some sort of cross-cuttiing concern? 

그렇다면 우리가 어떤 종류의 횡단 관심사를 가질 때는 어떻게 될까?

>Llke down in the hierarchy. 

계층 구조 아래에서 말이다.

>it turns out, oh wait, there's some business that that object has with another object and a totally different branch of the hierarchy. 

알고 보니, 어, 잠깐, 그 객체가 다른 객체와 어떤 비즈니스를 가지고 있고, 그 다른 객체는 완전히 다른 계층 구조의 분기에 있다.

>how do they talk to each other? 

그들은 어떻게 서로 메시지를 주고 받을까?

>well not directly. 

직접적으로는 아니다.

![same ancestor](https://github.com/user-attachments/assets/38b42e69-d495-40ae-9164-640ff33085ad)

>everything has to go throught their common ancestor.

모든 것은 그들의 공통 조상을 통해 가야 한다.

>for A to send a message to B here it can't actually directly invoke any kind of method. 

여기서 A가 B에게 메시지를 보내려면 실제로 어떤 종류의 메서드도 직접 호출할 수 없다.

>it has to mutate its own state and way, and then information about that state that new intention of the object, gets returned from a message sent from A's parent. 

A는 자신의 상태와 방식을 변경해야 하고, 그 다음 그 상태에 대한 정보, 즉 객체의 새로운 의도가 A의 부모로부터 보내진 메시지에서 반환된다.

>A's parent,  same thing has to happen. 

A의 부모에서도 같은 일이 일어난다.

>so it gets back up to the common ancestor, 
>and then only finally, when we get to the common ancestor, can that intent be realized as a series of message calls. 

그래서 공통 조상까지 올라가고, 그 다음에야 마침내 그 의도가 일련의 메시지 호출로 실현될 수 있다.

>but not directly down to B, 
>it has to be bucket-brigade it down through the hierarchy. 

하지만 B로 직접 내려가는 게 아니라, 계층 구조를 통해 bucket-brigade 방식으로 내려가야 한다.

![bucketbridge](https://github.com/user-attachments/assets/5f625a1f-be11-471a-aca8-ca070f960120)

>[!note]
>Bucket brigade는 원래 화재 진압 시 사람들이 줄을 서서 물 양동이를 hand-to-hand로 전달하는 방식
>객체 지향 프로그래밍에서 이 용어는 메시지나 데이터가 여러 객체를 거쳐 최종 목적지에 도달하는 과정을 설명할 때 사용하는 것으로 보여짐.

>that is how you handle cross-cutting concerns in a strict encapsulated hierarchy. 

이것이 엄격한 캡슐화 계층 구조에서 횡단 관심사를 처리하는 방법이다.

>obviously no one writes programs this way, or at least no one wirtes whole programs this way. 
>and for good reason, is an absurd way to have to writes whole programs. 

명백히 아무도 이런 방식으로 프로그램을 작성하지 않는다.\
적어도 전체 프로그램을 이런 식으로 작성하는 사람은 없으며,\
합당한 이유로 전체 프로그램을 이런 식으로 작성해야 한다는 것은 터무니없는 방법이라고 말할 수 있다.

>now you might argue that people do follow these principles in practice, they just do so inconsistently. 

이제 당신은 실제로 사람들이 이러한 원칙들을 따르긴 하지만, 그저 일관성 없이 따른다고 주장할 수 있습니다.

>And perhaps there is some value in a code base where you apply these principle inconsistently. 

그리고 아마도 이러한 원칙들을 일관성 없이 적용한 코드베이스에도 어떤 가치가 있을 수 있겠죠.

>[!note]
>Does inconsistent encapsulation provide value?

>perhaps half-assed encapsulation actually gets us something. 

어쩌면 절반 정도의 캡슐화가 실제로 우리에게 뭔가를 가져다 줄 수도 있습니다.

![zoo object](https://github.com/user-attachments/assets/75cb3a17-0630-4c59-9d81-0f088fd9a049)

>so imagine we have some sort of freeform graph of objects making up a program, 
>and we decide, oh well, there's this subsystem of objects that together should be their own self-contained encapsulated hierarchy of objects. 

그래서 우리가 프로그램을 구성하는 일종의 자유형 객체 그래프를 가지고 있다고 상상해 보자.\
"음, 이 객체들의 하위 시스템이 함께 자체 포함된 캡슐화된 객체 계층 구조가 되어야 해."
라고 우리는 결정한다.

![object zoo subsystem](https://github.com/user-attachments/assets/dd080c6f-49a3-4d66-b15a-6601cbbb1244)

>And so we're going to refactor our code Well very often. 

그래서 우리는 코드를 매우 자주 리팩터링 할것이다.

>what that means is not only do we have to do a lot of complicated rethinking of the structure of the relationship here, of that calls what on the other objects, we very typically have to introduce more objects. 

이것이 의미하는 바는  여기서 관계의 구조, 즉 다른 객체에서 무엇을 호출하는지에 대해 매우 복잡하게 다시 생각해야 할 뿐만 아니라 일반적으로 더 많은 객체를 도입해야 한다는 의미입니다. 

![subsystem](https://github.com/user-attachments/assets/95ca5872-f5bb-4041-8ad6-20533dbcf2d2)

>like, say here to represent this whole new subsystem, we probably have to introduce some new sub god object, 
>you know, some ruler of this subsystem. 

예를 들어, 이 전체 새로운 하위 시스템을 표현하기 위해, 우리는 아마도 새로운 하위 god 객체, 즉 이 하위 시스템의 지배자 같은 것을 도입해야 할 것이다.

>now all interactions with the subsystem have to be reconceptiolized as going throught this minor deity.

이제 하위 시스템과의 모든 상호작용은 이 작은 god을 통과하는 것으로 재개념화되어야 한다.

>so say we successfully do this refactoring,\
>and now while our code doesn't follow the principles of encapsulation perfectly,\
>it's doing so in a half consistent way. 

그래서 우리가 이 리팩터링을 성공적으로 수행했다고 해보자.\
이제 우리의 코드가 캡슐화 원칙을 완벽하게 따르지는 않지만, 반쯤 일관된 방식으로 그렇게 하고 있다.

>And maybe there's some benefit there. 

그리고 아마도 거기에 어떤 이점이 있을 수 있다.

![zoo subsystem](https://github.com/user-attachments/assets/d7bc56ca-4e69-4492-b5a6-90e4ae5fc05e)

>Well,  i think what tends to happen is subsequently we decide, oh wait, we need some new interaction between elements of this encapsulated subsystem.

음, 내 생각에 그 후에 일어나는 경향이 있는 일은 우리가 이렇게 결정하는 것이다.\
"어, 잠깐, 우리는 이 캡슐화된 하위 시스템의 요소들 사이에 새로운 상호작용이 필요해."

![zoo subsystem 2](https://github.com/user-attachments/assets/d908017e-27d5-4b89-a237-d1b8c85dad0d)

>And instead of having to do the hard work of figuring out how exactly it all gets coordinated from the roots of that subsystem that temptation is to jus thandle the business directly. 

그리고 그 하위 시스템의 뿌리에서 모든 것이 어떻게 정확히 조정되는지 알아내는 힘든 작업을 하는 대신, 그 유혹은 그냥 직접 비즈니스를 처리하는 것이다..

>but if we want to do the proper thing, we have two options. 

하지만 우리가 올바른 일을 하고 싶다면, 우리에게는 두 가지 선택지가 있다.

![subsystem second option](https://github.com/user-attachments/assets/202e81b2-6934-45b5-93d0-a5930b3b80d5)

>that maybe it turns out that that stuff external to the subsystem actually just needs to get integrated into that subsystem, and so it comes under the purview of the subsystems root. 

아마도 그 하위 시스템 외부의 그 것들이 실제로 그 하위 시스템에 통합되어야 할 필요가 있고, 그래서 그것이 하위 시스템의 루트의 범위 내에 들어오게 되는 것일 수 있다.

>But otherwise, we now have two subsystems that need to coordinate, and who's going to do the coordination? 

그렇지 않으면, 우리는 이제 조정이 필요한 두 개의 하위 시스템을 가지게 되고, 누가 그 조정을 할 것인가?

![new subsystem](https://github.com/user-attachments/assets/cbf35088-2cba-4380-846e-eaa0b1b13a1e)

>Well, now we need a new subsystem god responsible for the collective business of these two subsystems.

음, 이제 우리는 이 두 하위 시스템의 집단적 비즈니스를 책임지는 새로운 하위 시스템 god이 필요하다.

>And now all interactions of these two subsystems have to go through this root object. 

그리고 이제 이 두 하위 시스템의 모든 상호작용은 이 루트 객체를 통과해야 한다.

>But also all interactions with the outside world and these two subsystems have to go through this new root object. 

하지만 또한 외부 세계와 이 두 하위 시스템 간의 모든 상호작용도 이 새로운 루트 객체를 통과해야 한다.

![fuckit](https://github.com/user-attachments/assets/53e2cb84-cd4a-43c2-8072-def5f83e7fc5)

>So as you can see, chances are really good that what you would actually do is say "fuck it" and just do this.

그래서 보시다시피, 당신이 실제로 할 가능성이 매우 높은 것은 "fuck it" 하고 그냥 이렇게 하는 겁니다.

>you would just reach in and have the objects directly interact with each other whether they should properly do so or not. 

당신은 그냥 들어가서 객체들이 서로 직접 상호작용하게 할 것이다. 
그들이 적절히 그렇게 해야 하는지 여부와 상관없이 말이다.

>And now where is the encapsulation? 

그리고 이제 캡슐화는 어디에 있나요?

>What's the point? 

무슨 의미가 있나요?

>Whether you follow the rules strictly or loosely, you're in a bad place. 

당신이 규칙을 엄격하게 따르든 느슨하게 따르든, 당신은 안 좋은 상황에 처해 있다.

>If you follow the rule strictly, most things you do end up at being very unobviously structure and very indirect, and the number of  find entities code base paraphrase the site nature. entities tends to be very abstract and nebulous. 

규칙을 엄격하게 따르면, 당신이 하는 대부분의 일들이 매우 불분명한 구조와 매우 간접적인 것으로 끝나게 되고,\ 코드베이스에서 발견되는 엔티티의 수가 사이트의 본질을 바꿔 말하게 된다.\
엔티티들은 매우 추상적이고 모호한 경향이 있다.

>But alternatively, if you follow the rules loosely, what are you even getting why are you bothering? 

하지만 반대로, 규칙을 느슨하게 따른다면, 당신은 대체 무엇을 얻고 있는 겁니까? 
왜 귀찮게 하고 있나요?

>What is the point? 

어떤게 중요하죠?

>When i look at your object-oriented code base, 
>What i'm going to encounter is either this over-engineered giant tower of abstraction,  or i'm going to be looking at this inconsistently architected pile of objects that are all probably tangled together like christmas lights. 

내가 마주치게 될 것은 과도하게 설계된 거대한 추상화의 탑이거나, 아니면 크리스마스 트리 조명처럼 모두 얽혀있을 것 같은 일관성 없이 설계된 객체들의 더미일 겁니다.

>You'll have all these objects giving you a warm fuzzy feeling of encapsulation, but you're not going to have any real encapsulation of any signification

당신은 이 모든 객체들이 주는 캡슐화의 따뜻하고 모호한 느낌을 가지겠지만, 어떤 의미로도 실제로 캡슐화 하지 못할 것 입니다.