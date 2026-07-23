# 접근 지정자

## 한 줄 정의

접근 지정자는 클래스의 멤버를 어디서 사용할 수 있는지 정하는 규칙입니다.

## 왜 필요한가

클래스 내부 데이터를 아무 곳에서나 수정할 수 있게 두면 객체 상태가 쉽게 망가질 수 있습니다.

접근 지정자를 사용하면 외부에 공개할 기능과 숨길 데이터를 구분할 수 있습니다.

```text
public
protected
private
```

## public

`public`은 클래스 외부에서도 접근할 수 있습니다.

```cpp
class Player
{
public:
    int hp;
};
```

```cpp
Player player;
player.hp = 100;
```

```text
public = 클래스 밖에서도 접근 가능
```

하지만 모든 데이터를 `public`으로 열어두면 위험할 수 있습니다.

```cpp
player.hp = -999;
```

이런 이상한 값도 외부에서 마음대로 넣을 수 있기 때문입니다.

## private

`private`은 클래스 내부에서만 접근할 수 있습니다.

```cpp
class Player
{
private:
    int hp;

public:
    void SetHp(int value)
    {
        hp = value;
    }
};
```

클래스 밖에서는 직접 접근할 수 없습니다.

```cpp
Player player;
player.hp = 100; // 오류
```

하지만 클래스 내부 함수에서는 접근할 수 있습니다.

```cpp
void SetHp(int value)
{
    hp = value; // 가능
}
```

```text
private = 클래스 내부에서만 접근 가능
```

보통 데이터는 `private`으로 숨기고, 필요한 기능만 `public` 함수로 제공합니다.

## protected

`protected`는 클래스 밖에서는 접근할 수 없지만, 자식 클래스에서는 접근할 수 있습니다.

```cpp
class Monster
{
protected:
    int hp;
};

class Boss : public Monster
{
public:
    void SetHp()
    {
        hp = 1000; // 가능
    }
};
```

하지만 외부에서는 접근할 수 없습니다.

```cpp
Boss boss;
boss.hp = 500; // 오류
```

```text
protected = 외부에서는 접근 불가, 자식 클래스에서는 접근 가능
```

## 세 가지 비교

```text
public
-> 클래스 밖에서도 접근 가능
-> 누구나 사용해도 되는 기능에 사용

protected
-> 클래스 밖에서는 접근 불가
-> 자식 클래스에서는 접근 가능
-> 상속 구조에서 자식에게만 열어주고 싶을 때 사용

private
-> 클래스 내부에서만 접근 가능
-> 외부에 숨기고 싶은 데이터에 사용
```

짧게 기억하면 다음과 같습니다.

```text
public = 모두에게 공개
protected = 자식에게 공개
private = 자기 자신에게만 공개
```

## class와 struct의 기본 접근 차이

C++에서 `class`와 `struct`는 기본 접근 지정자가 다릅니다.

```cpp
class Player
{
    int hp;
};
```

`class`는 기본 접근 지정자가 `private`입니다.

즉 위 코드는 사실상 다음과 같습니다.

```cpp
class Player
{
private:
    int hp;
};
```

반면 `struct`는 기본 접근 지정자가 `public`입니다.

```cpp
struct Vector3
{
    float x;
    float y;
    float z;
};
```

게임 개발에서는 보통 다음 느낌으로 사용합니다.

```text
class
-> 동작과 캡슐화가 중요한 객체

struct
-> 단순 데이터 묶음
```

## 캡슐화와 연결

플레이어 체력은 외부에서 직접 수정하게 두면 위험합니다.

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

이렇게 하면 외부에서는 정해진 함수로만 체력을 바꿀 수 있습니다.

```cpp
player.TakeDamage(10);
```

이것이 캡슐화입니다.

```text
데이터는 private
외부에 필요한 기능은 public 함수
```

## protected 사용 시 주의

`protected`는 자식 클래스에서 접근 가능해서 편하지만, 너무 많이 쓰면 부모 내부 구현에 자식이 강하게 묶일 수 있습니다.

예를 들어 부모의 `hp` 구조가 바뀌면, `hp`에 직접 접근하던 모든 자식 클래스가 영향을 받을 수 있습니다.

그래서 보통은 다음 형태가 더 안전한 경우가 많습니다.

```text
private 데이터
protected 또는 public 함수
```

예:

```cpp
class Monster
{
private:
    int hp;

protected:
    void SetHp(int value)
    {
        hp = value;
    }
};
```

이렇게 하면 자식이 직접 `hp`를 만지는 대신, 부모가 제공한 함수로만 수정하게 만들 수 있습니다.

## 면접 답변

`public`, `protected`, `private`은 클래스 멤버의 접근 범위를 정하는 접근 지정자입니다. `public`은 클래스 외부에서도 접근 가능하고, `private`은 클래스 내부에서만 접근 가능합니다. `protected`는 외부에서는 접근할 수 없지만 상속받은 자식 클래스에서는 접근할 수 있습니다. 일반적으로 데이터는 `private`으로 숨기고, 필요한 기능만 `public` 함수로 제공해 캡슐화를 지키는 것이 좋습니다.

## 확인 질문

- `public`, `protected`, `private`은 각각 어디서 접근할 수 있을까?
- `protected` 멤버는 클래스 외부에서 접근할 수 있을까?
- `class`와 `struct`의 기본 접근 지정자는 어떻게 다를까?
- 캡슐화를 위해 데이터는 보통 어떤 접근 지정자로 두는 것이 좋을까?

