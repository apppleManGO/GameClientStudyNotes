# 상속

## 한 줄 정의

상속은 부모 클래스의 기능을 자식 클래스가 물려받아 재사용하고 확장하는 객체지향 문법입니다.

## 왜 필요한가

공통 기능을 부모 클래스에 모아두고, 자식 클래스는 필요한 기능만 추가하거나 다르게 동작하도록 만들기 위해 사용합니다.

게임에서는 여러 객체가 공통 성격을 가질 때 상속을 사용할 수 있습니다.

```text
Monster
-> Goblin
-> Boss
-> Slime
```

## 기본 형태

```cpp
class Monster
{
public:
    int hp;

    void Move()
    {
        cout << "Move" << endl;
    }
};

class Goblin : public Monster
{
};
```

`Goblin`은 `Monster`를 `public` 상속받았습니다.

따라서 `Monster`의 `public` 멤버 함수인 `Move()`를 사용할 수 있습니다.

```cpp
Goblin goblin;
goblin.Move();
```

출력은 다음과 같습니다.

```text
Move
```

## 부모 클래스와 자식 클래스

```cpp
class Goblin : public Monster
```

이 코드는 다음처럼 읽을 수 있습니다.

```text
Goblin은 Monster를 public 상속한다.
```

용어는 다음과 같습니다.

```text
Monster
-> 부모 클래스
-> 기반 클래스
-> Base class

Goblin
-> 자식 클래스
-> 파생 클래스
-> Derived class
```

## is-a 관계

상속은 보통 `is-a` 관계일 때 자연스럽습니다.

```text
Goblin is a Monster
Boss is a Monster
Slime is a Monster
```

즉 다음 문장이 자연스러우면 상속 후보가 될 수 있습니다.

```text
고블린은 몬스터다.
보스는 몬스터다.
슬라임은 몬스터다.
```

반대로 다음은 어색합니다.

```text
Sword is a Player
Inventory is a Monster
```

이런 경우는 상속보다 포함 관계가 더 자연스럽습니다.

```text
Player has a Sword
Player has an Inventory
```

이런 포함 관계를 컴포지션이라고 합니다.

## 상속의 장점

공통 기능을 부모 클래스에 모아둘 수 있습니다.

```cpp
class Monster
{
public:
    int hp;

    void Move()
    {
        cout << "Move" << endl;
    }
};
```

자식 클래스는 공통 기능을 반복해서 작성하지 않아도 됩니다.

```cpp
class Goblin : public Monster
{
public:
    void Steal()
    {
        cout << "Goblin Steal" << endl;
    }
};

class Boss : public Monster
{
public:
    void UseSkill()
    {
        cout << "Boss Skill" << endl;
    }
};
```

```text
공통 기능
-> 부모 클래스에 둠

특수 기능
-> 자식 클래스에 둠
```

## virtual / override와 연결

상속받은 기능을 자식 클래스에서 다르게 동작하게 만들 수도 있습니다.

```cpp
class Monster
{
public:
    virtual void Attack()
    {
        cout << "Monster Attack" << endl;
    }
};

class Boss : public Monster
{
public:
    void Attack() override
    {
        cout << "Boss Attack" << endl;
    }
};
```

```text
상속
-> 부모 기능을 물려받음

virtual / override
-> 물려받은 기능을 자식 방식으로 바꿈
```

## 상속의 위험

상속은 편리하지만 남용하면 코드가 복잡해질 수 있습니다.

부모 클래스가 바뀌면 이를 상속받은 자식 클래스들에도 영향이 갈 수 있습니다.

예를 들어 `Monster`의 구조를 바꾸면 `Goblin`, `Boss`, `Slime`이 전부 영향을 받을 수 있습니다.

따라서 상속을 사용할 때는 다음을 생각해야 합니다.

```text
공통 규칙이 명확한가?
정말 is-a 관계인가?
자식들이 부모의 동작을 자연스럽게 물려받아도 되는가?
```

상속이 자연스럽지 않다면 컴포지션을 고려하는 것이 좋습니다.

## 게임 개발 연결

상속이 자연스러운 예시는 다음과 같습니다.

```text
Monster
-> Goblin
-> Boss
-> Slime

Item
-> Weapon
-> Potion
-> Armor
```

반대로 다음은 상속보다 컴포지션이 자연스럽습니다.

```text
Player
-> Sword
```

플레이어는 검이 아니기 때문입니다.

이 경우는 이렇게 생각하는 것이 더 좋습니다.

```text
Player has a Weapon
```

```cpp
class Player
{
private:
    Weapon* weapon;
};
```

## 면접 답변

상속은 부모 클래스의 멤버 변수와 함수를 자식 클래스가 물려받아 재사용하고 확장하는 객체지향 문법입니다. 공통 기능은 부모 클래스에 두고, 자식 클래스는 필요한 기능을 추가하거나 `virtual` 함수를 재정의해 다르게 동작할 수 있습니다. 다만 상속은 `is-a` 관계가 명확할 때 사용하는 것이 좋고, 남용하면 부모 변경이 자식에게 영향을 주어 코드가 복잡해질 수 있습니다.

## 확인 질문

- 상속은 어떤 상황에서 사용하는 것이 자연스러울까?
- `Goblin : public Monster`는 어떤 의미일까?
- `is-a` 관계와 `has-a` 관계는 어떻게 다를까?
- 상속을 남용하면 어떤 문제가 생길 수 있을까?

