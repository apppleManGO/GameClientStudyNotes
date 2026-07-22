# 다형성

## 한 줄 정의

다형성은 같은 코드가 실제 객체 타입에 따라 다르게 동작하는 객체지향의 특징입니다.

## 왜 필요한가

게임에서는 여러 자식 객체를 부모 타입으로 묶어서 한 번에 다루는 일이 많습니다.

```text
Monster
-> Goblin
-> Boss
-> Slime
```

다형성을 사용하면 같은 `Attack()` 호출이라도 실제 객체 타입에 따라 다른 동작을 실행할 수 있습니다.

```text
같은 인터페이스
다른 동작
```

## 기본 예시

```cpp
class Monster
{
public:
    virtual void Attack()
    {
        cout << "Monster Attack" << endl;
    }
};

class Goblin : public Monster
{
public:
    void Attack() override
    {
        cout << "Goblin Attack" << endl;
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
Monster* enemy1 = new Goblin();
Monster* enemy2 = new Boss();

enemy1->Attack();
enemy2->Attack();
```

출력은 다음과 같습니다.

```text
Goblin Attack
Boss Attack
```

코드는 똑같이 `Attack()`을 호출하지만, 실제 객체 타입에 따라 다르게 동작합니다.

## 부모 포인터와 실제 객체 타입

```cpp
Monster* monster = new Boss();
monster->Attack();
```

이 코드는 다음처럼 이해하면 됩니다.

```text
Monster* monster
-> 부모 타입 포인터

new Boss()
-> 실제 생성된 객체는 Boss

virtual Attack()
-> 실제 객체 타입 기준으로 호출

결과
-> Boss::Attack()
```

즉 "몬스터 객체에 보스 객체를 넣었다"라기보다는, "Monster 타입 포인터가 Boss 객체를 가리킨다"라고 표현하는 것이 더 정확합니다.

## 여러 객체를 한 번에 다루기

다형성이 있으면 여러 자식 객체를 부모 타입 컨테이너에 넣어서 다룰 수 있습니다.

```cpp
vector<Monster*> monsters;

monsters.push_back(new Goblin());
monsters.push_back(new Boss());
monsters.push_back(new Slime());
```

반복문에서는 같은 코드만 사용합니다.

```cpp
for (Monster* monster : monsters)
{
    monster->Attack();
}
```

하지만 실제 실행은 객체마다 달라집니다.

```text
Goblin -> Goblin Attack
Boss   -> Boss Attack
Slime  -> Slime Attack
```

## 다형성이 없으면

다형성이 없으면 타입을 하나하나 확인하는 코드가 늘어날 수 있습니다.

```cpp
if (type == "Goblin")
{
    GoblinAttack();
}
else if (type == "Boss")
{
    BossAttack();
}
else if (type == "Slime")
{
    SlimeAttack();
}
```

몬스터 종류가 늘어날수록 조건문도 계속 늘어납니다.

다형성을 사용하면 다음처럼 각 객체가 자기 행동을 알아서 실행하게 만들 수 있습니다.

```cpp
monster->Attack();
```

새로운 자식 클래스를 추가해도 기존 반복문 코드를 크게 바꾸지 않아도 됩니다.

## C++ 런타임 다형성 조건

C++에서 런타임 다형성이 동작하려면 보통 다음 조건들이 필요합니다.

```text
1. 상속 관계
2. 부모 클래스의 virtual 함수
3. 자식 클래스의 override
4. 부모 포인터 또는 부모 참조로 호출
```

예:

```cpp
Monster* monster = new Boss();
monster->Attack();
```

```text
포인터 타입: Monster*
실제 객체 타입: Boss
virtual 함수 호출
-> Boss::Attack() 실행
```

## 오버로딩과 오버라이딩

다형성에서 자주 함께 나오는 개념입니다.

오버로딩은 같은 이름의 함수를 매개변수만 다르게 여러 개 만드는 것입니다.

```cpp
void Attack();
void Attack(int damage);
void Attack(int damage, float range);
```

```text
같은 이름
다른 매개변수
컴파일 시점에 결정
```

오버라이딩은 부모의 `virtual` 함수를 자식이 재정의하는 것입니다.

```cpp
class Monster
{
public:
    virtual void Attack();
};

class Boss : public Monster
{
public:
    void Attack() override;
};
```

```text
부모 virtual 함수를 자식이 재정의
런타임에 실제 객체 타입 기준으로 결정
```

## 게임 개발 연결

다형성은 게임 코드에서 자연스럽게 사용됩니다.

```text
Monster::Attack()
Item::Use()
Skill::Cast()
Projectile::OnHit()
UIWidget::Open()
```

예를 들어 스킬 시스템을 생각할 수 있습니다.

```cpp
class Skill
{
public:
    virtual void Cast() = 0;
};

class Fireball : public Skill
{
public:
    void Cast() override
    {
        cout << "Fireball" << endl;
    }
};

class Heal : public Skill
{
public:
    void Cast() override
    {
        cout << "Heal" << endl;
    }
};
```

```cpp
vector<Skill*> skills;
skills.push_back(new Fireball());
skills.push_back(new Heal());

for (Skill* skill : skills)
{
    skill->Cast();
}
```

같은 `Cast()` 호출이지만 스킬마다 다르게 동작합니다.

## 면접 답변

다형성은 같은 인터페이스를 통해 여러 객체를 다루고, 실제 객체 타입에 따라 다른 동작이 실행되게 하는 객체지향의 특징입니다. C++에서는 주로 상속, `virtual` 함수, `override`, 부모 포인터나 참조를 통해 런타임 다형성을 구현합니다. 이를 통해 조건문을 줄이고, 새로운 자식 클래스를 추가해도 기존 코드를 크게 수정하지 않고 확장할 수 있습니다.

## 확인 질문

- 다형성은 무엇을 의미할까?
- `Monster* monster = new Boss();`는 정확히 어떻게 이해해야 할까?
- 런타임 다형성이 동작하려면 어떤 조건들이 필요할까?
- 오버로딩과 오버라이딩은 어떻게 다를까?

