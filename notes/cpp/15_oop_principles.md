# OOP 4대 원칙

## 한 줄 정의

객체지향의 4대 원칙은 캡슐화, 상속, 추상화, 다형성입니다.

## 왜 필요한가

객체지향은 코드를 객체 단위로 나누고, 변경에 강한 구조를 만들기 위한 방식입니다.

게임 개발에서는 플레이어, 몬스터, 아이템, 스킬, UI 같은 요소를 객체로 나누어 관리하는 경우가 많습니다.

## 1. 캡슐화

캡슐화는 데이터와 기능을 하나로 묶고, 외부에서 데이터를 함부로 수정하지 못하게 보호하는 것입니다.

나쁜 예:

```cpp
class Player
{
public:
    int hp;
};
```

이렇게 `hp`가 `public`이면 외부에서 아무 곳이나 값을 바꿀 수 있습니다.

```cpp
player.hp = -999;
```

이런 값이 들어가면 객체 상태가 망가질 수 있습니다.

그래서 보통 데이터를 `private`으로 숨기고, 정해진 함수를 통해 수정하게 만듭니다.

```cpp
class Player
{
private:
    int hp;

public:
    void TakeDamage(int damage)
    {
        if (damage < 0)
            return;

        hp -= damage;

        if (hp < 0)
            hp = 0;
    }

    int GetHp() const
    {
        return hp;
    }
};
```

```text
데이터는 숨긴다.
접근은 함수로 제한한다.
잘못된 값이 들어오지 않게 보호한다.
```

## 2. 상속

상속은 부모 클래스의 멤버 변수와 함수를 자식 클래스가 물려받아 재사용하고 확장하는 것입니다.

```cpp
class Monster
{
public:
    void Move()
    {
        cout << "Move" << endl;
    }
};

class Goblin : public Monster
{
};
```

`Goblin`은 `Monster`를 상속받았기 때문에 `Move()`를 사용할 수 있습니다.

```cpp
Goblin goblin;
goblin.Move();
```

상속은 보통 `is-a` 관계일 때 자연스럽습니다.

```text
Goblin is a Monster
Boss is a Monster
```

반대로 `Player has a Weapon`처럼 포함 관계가 자연스러운 경우는 상속보다 컴포지션이 더 좋을 수 있습니다.

## 3. 추상화

추상화는 복잡한 내부 구현을 숨기고, 필요한 사용법만 외부에 드러내는 것입니다.

예를 들어 스킬을 사용할 때 내부에서 마나 계산, 이펙트 생성, 쿨타임 확인 등이 일어날 수 있습니다.

하지만 외부에서는 단순히 다음처럼 호출할 수 있습니다.

```cpp
skill->Cast();
```

순수가상함수는 추상화를 표현하는 대표적인 방법입니다.

```cpp
class Skill
{
public:
    virtual void Cast() = 0;
};
```

이 코드는 "스킬이라면 Cast 기능이 있어야 한다"는 규칙만 정하고, 실제 동작은 자식 클래스가 구현하게 합니다.

```text
추상화 = 복잡한 세부 구현은 숨기고 필요한 사용법만 제공
```

## 4. 다형성

다형성은 같은 인터페이스를 통해 호출하더라도 실제 객체 타입에 따라 다른 동작이 실행되는 것입니다.

```cpp
class Monster
{
public:
    virtual void Attack() = 0;
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

```cpp
Monster* monster = new Boss();
monster->Attack();
```

`monster`는 `Monster*` 타입이지만 실제로는 `Boss` 객체를 가리키고 있습니다.

`Attack()`이 `virtual`이므로 실제 객체 타입인 `Boss`의 `Attack()`이 호출됩니다.

```text
다형성 = 같은 호출이 실제 객체 타입에 따라 다르게 동작
```

## 게임 예시로 묶기

몬스터 시스템으로 보면 다음처럼 연결됩니다.

```text
캡슐화
-> hp를 private으로 숨기고 TakeDamage()로만 수정

상속
-> Goblin, Boss가 Monster를 물려받음

추상화
-> Attack(), Move() 같은 사용법만 외부에 공개

다형성
-> Monster*로 묶어도 실제 몬스터별 Attack() 실행
```

예:

```cpp
class Monster
{
private:
    int hp;

public:
    virtual ~Monster() = default;

    void TakeDamage(int damage)
    {
        if (damage < 0)
            return;

        hp -= damage;

        if (hp < 0)
            hp = 0;
    }

    virtual void Attack() = 0;
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

이 예시에는 네 가지 원칙이 모두 들어 있습니다.

```text
hp private
-> 캡슐화

Boss : public Monster
-> 상속

virtual void Attack() = 0
-> 추상화

Attack() override
-> 다형성
```

## 면접 답변

객체지향의 4대 원칙은 캡슐화, 상속, 추상화, 다형성입니다. 캡슐화는 데이터를 숨기고 정해진 함수로만 접근하게 해 객체 상태를 보호하는 것이고, 상속은 부모 클래스의 공통 기능을 자식 클래스가 물려받아 재사용하고 확장하는 것입니다. 추상화는 복잡한 내부 구현을 숨기고 필요한 인터페이스만 제공하는 것이며, 다형성은 같은 인터페이스를 통해 호출하더라도 실제 객체 타입에 따라 다른 동작이 실행되게 하는 특징입니다.

## 확인 질문

- `hp`를 `private`으로 숨기고 `TakeDamage()`로만 수정하게 하는 것은 어떤 원칙일까?
- 상속은 어떤 관계일 때 자연스러울까?
- 추상화와 순수가상함수는 어떻게 연결될까?
- 다형성은 `virtual` 함수와 어떤 관계가 있을까?

