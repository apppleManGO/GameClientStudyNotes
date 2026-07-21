# virtual 함수와 override

## 한 줄 정의

`virtual` 함수는 부모 포인터나 참조로 자식 객체를 다룰 때, 실제 객체 타입에 맞는 함수를 호출하게 해주는 기능입니다.

## 왜 필요한가

게임에서는 여러 자식 클래스를 부모 타입으로 한 번에 다루는 경우가 많습니다.

```cpp
Monster* enemy = new Boss();
enemy->Attack();
```

이때 포인터 타입은 `Monster*`이지만, 실제 객체는 `Boss`입니다.

`virtual` 함수는 이런 상황에서 실제 객체 타입인 `Boss`의 함수를 호출할 수 있게 해줍니다.

## virtual이 없는 경우

```cpp
class Monster
{
public:
    void Attack()
    {
        cout << "Monster Attack" << endl;
    }
};

class Boss : public Monster
{
public:
    void Attack()
    {
        cout << "Boss Attack" << endl;
    }
};
```

```cpp
Monster* enemy = new Boss();
enemy->Attack();
```

`Attack()`이 `virtual`이 아니면 포인터 타입인 `Monster*`를 기준으로 함수가 호출될 수 있습니다.

따라서 `Monster Attack`이 출력됩니다.

## virtual이 있는 경우

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

```cpp
Monster* enemy = new Boss();
enemy->Attack();
```

출력은 다음과 같습니다.

```text
Boss Attack
```

`Monster`의 `Attack()`이 `virtual` 함수이기 때문에, `Monster*` 타입 포인터로 호출하더라도 포인터 타입이 아니라 실제 객체 타입인 `Boss`를 기준으로 함수가 호출됩니다.

## virtual과 override의 관계

`virtual`과 `override`는 역할이 다릅니다.

```text
virtual
-> 부모 클래스에 붙임
-> 이 함수는 자식에서 재정의될 수 있음
-> 부모 포인터/참조로 호출해도 실제 객체 타입의 함수를 실행하게 함

override
-> 자식 클래스에 붙임
-> 부모의 virtual 함수를 올바르게 재정의했는지 컴파일러가 확인하게 함
```

짧게 기억하면 다음과 같습니다.

```text
virtual은 부모 쪽 약속
override는 자식 쪽 확인
```

또는:

```text
virtual이 작동시키고,
override가 확인해준다.
```

## override만 쓰면 될까?

`override`만으로는 동적 바인딩이 동작하지 않습니다.

부모 함수가 `virtual`이 아닌데 자식 함수에 `override`를 붙이면 컴파일 오류가 납니다.

```cpp
class Monster
{
public:
    void Attack()
    {
    }
};

class Boss : public Monster
{
public:
    void Attack() override // 오류
    {
    }
};
```

`override`는 부모의 `virtual` 함수를 재정의하고 있는지 확인하는 표시입니다.

따라서 부모 함수가 먼저 `virtual`이어야 합니다.

## override를 안 쓰면 어떻게 될까?

부모 함수가 `virtual`이면 자식 함수에 `override`를 쓰지 않아도 동적 바인딩은 동작할 수 있습니다.

하지만 실무에서는 `override`를 붙이는 것이 좋습니다.

이유는 오타나 함수 시그니처 실수를 컴파일러가 잡아줄 수 있기 때문입니다.

```cpp
class Boss : public Monster
{
public:
    void Attak() override // 부모에 Attak이 없으므로 오류
    {
    }
};
```

## 자식이 재정의하지 않으면 어떻게 될까?

일반 `virtual` 함수는 자식 클래스가 반드시 재정의해야 하는 것은 아닙니다.

부모가 기본 구현을 가지고 있다면, 자식이 재정의하지 않았을 때 부모 함수가 호출됩니다.

```cpp
class Monster
{
public:
    virtual void Attack()
    {
        cout << "Monster Attack" << endl;
    }
};

class Slime : public Monster
{
};
```

```cpp
Monster* enemy = new Slime();
enemy->Attack();
```

출력은 다음과 같습니다.

```text
Monster Attack
```

정리하면 다음과 같습니다.

```text
부모 virtual 함수에 기본 구현이 있음
-> 자식이 재정의 안 하면 부모 함수 실행
-> 자식이 재정의하면 자식 함수 실행
```

## 순수가상함수

자식 클래스가 반드시 함수를 구현하게 만들고 싶다면 순수가상함수를 사용합니다.

```cpp
class Monster
{
public:
    virtual void Attack() = 0;
};
```

`= 0`을 붙이면 부모 클래스는 함수의 기본 구현을 제공하지 않고, 자식이 반드시 구현해야 합니다.

이런 클래스를 추상 클래스라고 합니다.

```text
virtual void Attack()
-> 재정의해도 되고 안 해도 됨
-> 안 하면 부모 함수 사용

virtual void Attack() = 0
-> 반드시 자식이 재정의해야 함
-> 부모 클래스는 추상 클래스가 됨
```

## virtual 소멸자

부모 포인터로 자식 객체를 삭제할 수 있다면 부모 소멸자는 보통 `virtual`이어야 합니다.

```cpp
class Monster
{
public:
    virtual ~Monster()
    {
    }
};
```

```cpp
Monster* enemy = new Boss();
delete enemy;
```

부모 소멸자가 `virtual`이 아니면 자식 소멸자가 제대로 호출되지 않을 수 있습니다.

상속을 통해 다형적으로 사용할 클래스라면 다음처럼 기억하면 좋습니다.

```text
virtual 함수를 가진 부모 클래스는 소멸자도 virtual로 만들자.
```

## 게임 개발 연결

게임에서는 부모 타입으로 다양한 자식 객체를 한 번에 다루는 일이 많습니다.

```cpp
vector<Monster*> monsters;

monsters.push_back(new Goblin());
monsters.push_back(new Boss());

for (Monster* monster : monsters)
{
    monster->Attack();
}
```

`virtual`이 있으면 실제 객체 타입에 맞게 함수가 호출됩니다.

```text
Goblin Attack
Boss Attack
```

이렇게 같은 코드로 여러 타입의 객체를 다룰 수 있는 것이 다형성입니다.

## 면접 답변

`virtual` 함수는 부모 클래스 포인터나 참조로 자식 객체를 다룰 때, 실제 객체 타입에 맞는 재정의 함수를 호출하게 해주는 기능입니다. 이를 통해 런타임 다형성을 구현할 수 있습니다. `override`는 자식 클래스에서 부모의 `virtual` 함수를 올바르게 재정의했는지 컴파일러가 확인하게 해주는 키워드입니다. 일반 `virtual` 함수는 자식이 재정의하지 않으면 부모 구현이 호출되고, 순수가상함수는 자식이 반드시 구현해야 합니다. 또한 다형적으로 삭제될 수 있는 부모 클래스는 소멸자를 `virtual`로 선언해야 합니다.

## 확인 질문

- `virtual`과 `override`의 역할은 어떻게 다를까?
- 자식 클래스가 일반 `virtual` 함수를 재정의하지 않으면 어떤 함수가 호출될까?
- 순수가상함수는 일반 `virtual` 함수와 무엇이 다를까?
- 부모 소멸자를 `virtual`로 만들어야 하는 이유는 무엇일까?

