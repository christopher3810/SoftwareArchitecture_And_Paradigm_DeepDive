
[Glyphs: Flyweight Objects for User Interfaces.Paul R. Calder and Mark A.Linton.Center for Integrated Systems Stanford University Stanford, California 94305](https://dl.acm.org/doi/pdf/10.1145/97924.97935)

Flyweight Pattern 은 위 논문에서 Paul R. Calder와 Mark A.Linton에 의해 먼저 소개되었고,  
추후 Gang of four , 소프트웨어 패턴 운동가 들에 의해서 널리 알려졌다.

#### Why Flyweight?

---

간단하게 먼저 설명을 하고 들어가 보자.

왜 Flyweight 인가?

![](https://blog.kakaocdn.net/dn/Azhqb/btsI6ZKlAUZ/fkbRlSC9b3380wxIk7Bhe0/img.png)

복싱 체급 카테고리이다.

Flyweight는 매우 가벼운 선수들이 뛰는 구간임을 알 수 있다.

가볍기 때문이다.

**무엇이? 객체가 (light weight objects)**

Flyweight 패턴은 객체지향 프로그래밍에서 사용되는 구조적 디자인 패턴으로, **많은 수의 유사한 객체를 효율적으로 공유하여 메모리 사용을 최적화하는 기법이다.**

이 패턴의 핵심 아이디어는 객체의 내부 상태를 **intrinsic** **state와** **extrinsic state** 상태로 분리하는 것이다.  
  
**Intrinsic state** : 여러 인스턴스 간에 **공유될 수 있는 불변의 데이터**  
**Extrinsic state** : 각 인스턴스마다 고유한  데이터

### Flyweight와 Glyphs의 설계 배경

---

**문제 인식**

  
1980년대 후반과 1990년대 초반, 그래픽 사용자 인터페이스(GUI)가 발전하면서 개발자들은 새로운 도전에 직면하게 된다.

  
1. 복잡한 사용자 인터페이스 구현의 어려움  
2. 대량의 그래픽 객체 처리로 인한 메모리 부족  
3. 성능 저하 문제  
  
당시의 사용자 인터페이스 툴킷들은 복잡하고 무거운 컴포넌트를 제공했다.

이러한 컴포넌트들은 **다음과 같은 한계**를 가지고 있다.

1. 많은 메모리 사용  
2. 구현의 복잡성  
3. 제한된 재사용성

**해결 방안 모색**

  
이러한 문제를 해결하기 위해, Calder와 Linton은 다음과 같은 접근 방식을 제안한다.  
  
1. 객체의 세분화 : 사용자 인터페이스를 매우 작은 단위의 객체들로 구성  
2. 객체의 경량화 : 각 객체가 최소한의 상태만을 유지하도록 설계  
3. 객체의 공유 : 동일한 객체를 여러 곳에서 재사용  
  
텍스트 편집기에서 각 문자를 객체로 표현한다고 예를 든다면.

**비효율적 방식**

각 문자 객체가 폰트, 크기, 스타일 정보를 모두 포함한다.

**Flyweight 활용 시**

문자 코드만 개별 객체에 저장하고, 폰트 정보는 공유한다.

**장점**

**1. 메모리 사용 최적화**  
대규모 문서나 복잡한 그래픽 인터페이스에서 수많은 객체를 생성해야 할 때, 각 객체가 독립적으로 모든 상태를 유지한다면 메모리 사용량이 급격히 증가하는데.  
Flyweight 패턴은 이 문제를 **공유 가능한 상태를 분리**하여 해결한다.

  
**2. 객체 생성 및 관리의 간소화**  
복잡한 사용자 인터페이스에서 수많은 컴포넌트를 개별적으로 생성하고 관리하는 것은 어려운 작업.   
Glyphs 개념은 이 문제를 **단순하고 일관된 인터페이스를 가진 작은 객체들로 해결**한다.  
  
**3. 유연성과 확장성**

기존의 무거운 컴포넌트들은 특정 용도에 맞춰 설계되어 있어, 새로운 요구사항에 적응하기 어려웠다.  
Flyweight 패턴과 Glyphs 접근 방식은 **작은 단위의 객체들을 조합하여 다양한 인터페이스를 구성**할 수 있게 함으로써 이 문제를 해결한다.  
  
이렇게 대량의 객체를 효율적으로 관리함으로써, 렌더링 속도와 전반적인 애플리케이션 성능을 개선할 수 있었다.

초기의 논문 코드들을 한번 확인해 보자.

### Flyweight Objects for User Interfaces.

---

코드는 cpp 이다.

초기 코드

```cpp
void TextView::draw(
    Canvas* canvas,
    const Painter& p,
    const Allocation& a
) {
    Font* f = p.font();
    Coord x0 = a.x();
    Coord x = x0;
    Coord y = a.top() - f->ascent();
    Coord line_height = f->ascent() + f->descent();
    rewind(file);
    int c;
    while ((c = getc(file)) != EOF) {
        if (c == '\n') {
            x = x0;
            y -= line_height;
        } else {
            p.character(canvas, c, x, y);
            x += f->width(c);
        }
    }
}
```

파일의 내용을 직접 읽어 화면에 그리는 방식을 사용.

매번 draw 호출 시 파일을 처음부터 다시 읽어야 하므로 비효율적이다.

```cpp
TextView::TextView (FILE* file) {
    TBBox* page = new TBBox();
    LRBox* line = new LRBox();
    int c;
    while ((c = getc(file)) != EOF) {
        if (c == '\n') {
            page->append(line);
            line = new LRBox();
        } else {
            line->append(new Character(c));
        }
    }
    body(page);
}
```

개선된 코드는 Glyph 객체들을 사용하여 텍스트 뷰를 구성한다.

각 문자는 Character 객체로 표현되고, 이들은 LRBox(왼쪽에서 오른쪽으로 배열)에 담겨 행을 형성한다.

여러 행은 TBBox(위에서 아래로 배열)에 담겨 전체 페이지를 구성한다.

일본어 텍스트 지원, 공백문자 시각화, 텍스트 포맷팅 지원 등 다양하게 변경된다.

```cpp
/*Glyph 로 활용된 Character 구조*/
void Character::request(const Painter& painter, Requisition& requisition) {
    requisition.require(
        Dimension_X,
        Requirement(_font->width(_c), 0, 0)
    );
    requisition.require(
        Dimension_Y,
        Requirement(_font->ascent() + _font->descent(), 0, 0)
    );
}

void Character::draw(
    Canvas* canvas, const Painter& painter, const Allocation& allocation
) {
    Painter p(painter);
    p.font(_font);
    p.character(canvas, _c, allocation.x(), allocation.y());
}
```

1. **request** 메서드:  
   - **X Dimension** : 문자의 너비를 요청  
   - **Y Dimension** : 폰트의 ascent와 descent의 합을 높이로 요청.  
  
2. **draw** 메서드:  
   - 새로운 **Painter 객체를 생성**하여 그리기 컨텍스트를 설정.  
   - 폰트를 설정하고, 지정된 위치에 문자를 그림.  
  
**Intrinsic State** : **_c**(문자)와 **_font**(폰트)는 **Character 객체 내부**에 저장.  
**Extrinsic State** : **allocation (위치 정보)** 은 **draw 메서드**의 **매개변수**로 전달.

이 설계로 인해 **동일한 문자와 폰트**를 가진 **Character 객체를 여러 위치에서 재사용할** 수 있다.

### GoF의 Flyweight Pattern 설명

---

GoF(Gang of Four)의 Design Pattern에서는 어떻게 설명할까?

좀 더 세분화되고 구조가 정형화 되어서 설명할 뿐이지 크게 다르지 않다.

텍스트 편집기를 예시로 설명한다.

> 책에서 플라이급객체라고 번역된 부분은 Flyweight Object로 이해할 수 있다.

> 공유된 하나의 플라이급 객체를 두고 이를 문서의 다른 문맥마다 나타나게 하는 것입니다.  
> 각각의 글자를 인스턴스로 갖기보다 는 문서에 각 글자들이 나타날 때마다 플라이급 객체의 공유된 풀(pool)에 존재하는 인스턴스에 대한 참조자를 갖도록 관리합니다.  
> Glyph는 그래픽 객체에 대한 추상 클래스이며, 어떤 때는 플라이급 객체가 될 수도 있습니다.  
> 부가적 상태에 따라 달라지는 연산은 해당 상태를 매개변수로 전달받습니다.

![](https://blog.kakaocdn.net/dn/bhARjr/btsI7HbfF1z/W7zSd7VaM1dWsGWdMqrk91/img.png)

출처-https://www.cs.unc.edu/~stotts/GOF/hires/pat4ffso.htm

> Glyph는 그래픽 객체에 대한 추상 클래스이며, 어떤 때는 플라이급 객체가 될 수도 있습니다. 부가적 상태에 따라 달라지는 연산은 해당 상태를 매개변수로 전달받습니다

![](https://blog.kakaocdn.net/dn/bN9OdV/btsI7HWBXM5/F6IMDzvsAHXRXm6FKiKNx0/img.png)

출처-https://www.cs.unc.edu/~stotts/GOF/hires/pat4ffso.htm

논문 코드와 크게 다르지 않다.

**결국 'a'라는 글자를 갖고 있는 객체는 글자 코드를 저장하지만 위치나 폰트는 저장하지 않는다.**

논문과 책의 약간의 차이가 존재하는데 software pattern 구조가 좀 더 정형화되었다.

### Flyweight Pattern의 주요 구성 요소

---

Paul R. Calder와 Mark A.Linton의 논문에서는 Flyweight 객체의 **생성과 관리**에 대한 명확한 메커니즘이 제시되지 않았다.

Gof Design Pattern에서는 FlyweightFactory를 활용하여 Flyweight 객체의 **생성, 관리, 공유**를 담당하는 컴포넌트를 정의하며 패턴의 실제 구현을 더 체계적으로 만들었다.

![](https://blog.kakaocdn.net/dn/Y7UZd/btsI6RTjug9/7vllsk4uks9hZqjdgjrhp0/img.png)

출처-https://www.cs.unc.edu/~stotts/GOF/hires/pat4ffso.htm

![](https://blog.kakaocdn.net/dn/bMuTFk/btsI7dBJvx6/6kyQgKQhqAa1QQ2nDzHL9K/img.png)

출처-https://www.cs.unc.edu/~stotts/GOF/hires/pat4ffso.htm


**Flyweight(Glyph)**\ 
extrinsic state에 대해서 동작할 수 있는 인터페이스.

**ConcreteFlyweight(Character)**\
Flyweight 인터페이스를 구현하고 내부적으로 intrinsic state에 대한 저장소를 정의, 반드시 sharable 즉 공유 가능해야 한다.

**UnsharedConcreteFlyWeight(Row, Column)**\ 
모든 플라이급 서브클래스들이 공유될 필요는 없다.\
Flyweight 인터페이스는 공유는 할 수 있다.\
UnsharedConcreteFlyweight 객체가 Concrete- Flyweight 객체를 자신의 자식으로 갖는 것은 흔한 일이다.

**FlyweightFactory**\
플라이급 객체를 생성하고 관리하며, 플라이급 객체가 제대로 공유되도록 보장한다.\ 
사용자가 플라이급 객체를 요청하면 Flyweight- Factory 객체는 이미 존재하는 인스턴스를 제공, 만약 존재하지 않는다면 새로 생성.

**Client**\
플라이급 객체에 대한 참조자를 관리하며 플라이급 객체의 부가적 상태를 저장합니다.

**Intrinsic State** 는 **ConcreteFlyweight** 에 저장해야 하고,

**Extrinsic State**는 사용자가 저장하거나, 연산되어야 하는 다른 상태로 관리해야 한다.

사용자는 연산을 호출할 때 자신에게만 필요한 **Extrinsic State**를 플라이급 객체에 **매개변수로 전달**

사용 시에도 역시 포인트는 **Intrinsic State를 Flyweight 객체에 저장하고,** **Extrinsic State**를 플라이급 객체에 **매개변수로** 전달하는 것이다.

다만 사용자는 **Flyweight Object**가 공유될 수 있도록 **ConcreteFlyweight**의 인스턴스를 직접 만들 수 없으며,

사용자는 **ConcreteFlyweight 객체를 FlyweightFactory 객체에서 얻어야 한다**

#### 예제 코드

---

예제 코드를 작성해 보았다.

```java
enum PotionType {
    HEALING, MANA
}

// Extrinsic state
class PotionContext {
    private final int characterLevel;

    public PotionContext(int characterLevel) {
        this.characterLevel = characterLevel;
    }

    public int getCharacterLevel() { return characterLevel; }
}

// Flyweight(Glyph)
interface Potion {
    void drink(PotionContext context);
}

// ConcreteFlyweight(Character)
@Slf4j
public class HealingPotion implements Potion {
    // Intrinsic state
    private final int baseHealingValue = 20;

    @Override
    public void drink(PotionContext context) {
        int healingAmount = baseHealingValue + context.getCharacterLevel();
        log.info("You feel healed for {} HP. (Potion={})", healingAmount, System.identityHashCode(this));
    }
}

@Slf4j
public class ManaPotion implements Potion {
    // Intrinsic state
    private final int baseManaValue = 15;

    @Override
    public void drink(PotionContext context) {
        int manaAmount = baseManaValue + context.getCharacterLevel();
        log.info("You restored {} Mana. (Potion={})", manaAmount, System.identityHashCode(this));
    }
}

// FlyweightFactory
class PotionFactory {
    private final Map<PotionType, Potion> potions;

    public PotionFactory() {
        potions = new EnumMap<>(PotionType.class);
    }

    Potion createPotion(PotionType type) {
        Potion potion = potions.get(type);
        if (potion == null) {
            potion = switch (type) {
                case HEALING -> new HealingPotion();
                case MANA -> new ManaPotion();
            };
            potions.put(type, potion);
        }
        return potion;
    }
}

// Client
@Slf4j
class AlchemistShop {
    private final List<Potion> topShelf;
    private final List<Potion> bottomShelf;

    public AlchemistShop(PotionFactory factory) {
        topShelf = new ArrayList<>();
        bottomShelf = new ArrayList<>();

        topShelf.add(factory.createPotion(PotionType.HEALING));
        topShelf.add(factory.createPotion(PotionType.HEALING));
        topShelf.add(factory.createPotion(PotionType.MANA));

        bottomShelf.add(factory.createPotion(PotionType.MANA));
        bottomShelf.add(factory.createPotion(PotionType.HEALING));
    }

    public void drinkPotions() {
        log.info("Drinking top shelf potions");
        var lowLevelContext = new PotionContext(1);
        topShelf.forEach(potion -> potion.drink(lowLevelContext));

        log.info("Drinking bottom shelf potions");
        var highLevelContext = new PotionContext(50);
        bottomShelf.forEach(potion -> potion.drink(highLevelContext));
    }
}
```

### **어디에 활용할까?**

---

1. 응용프로그램이 대량의 객체를 사용해야 할 때  
2. 객체의 수가 너무 많아져 저장 비용이 너무 높아질 때  
3. 대부분의 객체 상태를 부가적인 것으로 만들 수 있을 때
4. 부가적인 속성들을 제거한 후 객체들을 조사해 보니 객체의 많은 묶음이 비교 적 적은 수의 공유된 객체로 대체될 수 있을 때. 현재 서로 다른 객체로 간주한 이유는 이들 부가적인 속성 때문이었지 본질이 달랐던 것은 아닐 때
5. 응용프로그램이 객체의 정체성에 의존하지 않을 때. 플라이급 객체들은 공유될 수 있음을 의미하는데, 식별자가 있다는 것은 서로 다른 객체로 구별해야 한다는 의미이므로 플라이급 객체를 사용할 수 없다.

#### Domain Model과 Flyweight Pattern

---

Flyweight Pattern 같은 구조와 장점을 잘 알겠다.

그렇다면 갑자기 DDD로 확 넘어가서

Flyweight Pattern은 **도메인 모델과 연관이 있을까?**

Eric Evans는 Domain Driven Design 12장 Model과 Design Pattern의 연결에서 

> 앞서 (5장에서） FLYWEIGHT 패턴에 관해 언급한 바 있으므로 아마 FLYWEIGHT 패턴이 도메인 모델에 적용되는 패턴이라고 짐작했을지도 모르겠다.  
> 사실 FLYWEIGHT는 도메인 모델과는 전혀 관련이 없는 디자인 패턴의 좋은 예다.  
> Eric Evans in Domain Driven Design

**Domain Model 과 전혀 관련이 없는 디자인 패턴의 좋은 예**라고 말을 한다.

**왜 그럴까?**

Flyweight 패턴은 주로 **구현 레벨의 최적화**를 위해 Design 된 Pattern이다.

이 Pattern의 주요 목적은 메모리 사용을 최적화하는 것으로, **도메인 모델의 개념적 구조와는 직접적인 관련이 없다**.

**VO의 경우에는 Pattern을 활용하는 것이 적절하다.**

주택 설계의 전기 배선과 같이 반복적으로 사용되는 표준화된 요소들,

즉 제한된 수의 Value Object 집합이 자주 사용되는 경우에 적합하다.

**그러나 Domain Entity에는 적용할 수가 없다.**

모델과 구현에 모두 적용되는 것은 도메인 패턴의 본질적인 특성인데.

**Flyweight Pattern은 구현에만 적용**이 가능하다.

좀 더 자세하게 확인해 보자.

#### 도메인 모델에 Flyweight Pattern을 적용할 수 없는 이유

---

**1. 가변성 (Mutability)과 식별자**

> 고유 식별자와 변화 가능성(mutability)이라는 특징이 엔터티와 값 객체 사이의 차이점이다.  
> Vaughn Vernon in Implementing Domain-Driven-Design

위에서 적어둔 것과 같이 플라이급 객체들은 공유될 수 있음을 의미하는데,

도메인 엔티티는 식별자가 존재하고 다른객체로 구별되어야 하기 때문에 플라이급 객체를 사용할 수 없다.

엔티티는 생명주기 동안 상태가 변할 수 있다.

Flyweight 객체는 일반적으로 불변(immutable)이어야 하므로, 이는 엔티티의 가변성과 직접적으로 충돌한다.

**2. 도메인 생명주기 (Domain LifeCycle)**

도메인 엔티티는 **복잡한 생명주기**를 가질 수 있으며, 이는 **도메인 규칙에 따라 관리**되어야 한다.

**DDD에서는 도메인의 생명주기도 도메인이다.**

Flyweight Object는 일반적으로 정적이며 생명주기 관리가 필요 없다.

**3. 도메인 로직의 캡슐화 (Domain Logic Encapsulation)**

엔티티는 중요한 도메인 로직과 비즈니스 규칙을 캡슐화한다.

Flyweight Object는 주로 데이터 저장에 초점을 맞추며, 복잡한 도메인 로직을 포함하기에는 적합하지 않다.