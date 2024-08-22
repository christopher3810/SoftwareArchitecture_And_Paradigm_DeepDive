템플릿메서드(Template Method)

객체의 연산에는 알고리즘의 뼈대만을 정의하고 

각 단계에서 수행할 구체적 처리는 서브클래스 쪽으로 미루는 패턴.

알고 리즘의 구조 자체는 그대로 놔둔 채 알고리즘 각 단계의 처리를 서브클래스에서 재정의할 수 있음.

문제

알고리즘의 구조는 동일하지만 세부 단계가 다른 여러 클래스를 어떻게 효율적으로 설계할 것인가?

코드 중복을 어떻게 줄이면서 동시에 유연성을 유지할 것인가?

활용성

어떤 한 알고리즘을 이루는 부분 중 변하지 않는 부분을 한 번 정의해 놓고

다양해질 수 있는 부분은 서브클래스에서 정의할 수 있도록 남겨두고자 할 때

• 서브클래스 사이의 공통적인 행동을 추출하여 하나의 공통 클래스에 몰아둠으 로써 코드 중복을 피하고 싶을 때. 옵다이크(Opdyke)와 존슨이 설명했듯[QJ93] 이는 “일반화를 위한 리팩토링(refactoring to generalize)”의 좋은 예입니다. 먼저, 기존 코드에서 나타나는 차이점을 뽑아 이를 별도의 새로운 연산들로 구분해 놓습니다. 그런 뒤 달라진 코드 부분을 이 새로운 연산을 호출하는 템플릿 메서드로 대체하는 것입니다.

• 서브클래스의 확장을 제어할 수 있습니다. 템플릿 메서드가 어떤 특정한 시점 에 “훅(hook)” 연산(‘결과’절을참고하세요)을 호출하도록 정의함으로써, 그 특정 시점에서만 확장되도록 합니다.

