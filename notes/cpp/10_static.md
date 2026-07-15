# static

## 한 줄 정의

`static`은 객체마다 따로 가지는 값이 아니라, 공유되거나 오래 유지되는 값을 만들 때 사용합니다.

## 왜 필요한가

`static`은 위치에 따라 의미가 조금 다르지만, 공통적으로 일반적인 지역 변수나 객체 멤버와 다른 수명 또는 공유 방식을 가집니다.

신입 C++ 면접 기준으로는 다음 세 가지를 먼저 이해하면 좋습니다.

```text
1. static 지역 변수
2. static 멤버 변수
3. static 멤버 함수
```

## static 지역 변수

일반 지역 변수는 함수가 호출될 때마다 새로 만들어지고, 함수가 끝나면 사라집니다.

```cpp
void Count()
{
    int count = 0;
    count++;

    cout << count << endl;
}
```

이 함수를 여러 번 호출하면 매번 `count`가 새로 초기화됩니다.

```cpp
Count();
Count();
Count();
```

출력은 다음과 같습니다.

```text
1
1
1
```

반면 `static` 지역 변수는 처음 한 번만 초기화되고, 함수가 끝난 뒤에도 값이 유지됩니다.

```cpp
void Count()
{
    static int count = 0;
    count++;

    cout << count << endl;
}
```

```cpp
Count();
Count();
Count();
```

출력은 다음과 같습니다.

```text
1
2
3
```

이유는 `count`가 함수 호출 때마다 새로 생성되는 것이 아니라, 처음 한 번만 초기화되고 이전 값을 계속 유지하기 때문입니다.

```text
일반 지역 변수
-> 함수 호출 때 생성
-> 함수 종료 때 사라짐

static 지역 변수
-> 처음 한 번만 초기화
-> 함수가 끝나도 값 유지
```

## static 멤버 변수

일반 멤버 변수는 객체마다 따로 존재합니다.

```cpp
class Player
{
public:
    int hp;
};
```

```cpp
Player a;
Player b;

a.hp = 100;
b.hp = 50;
```

`a.hp`와 `b.hp`는 서로 다른 값입니다.

반면 `static` 멤버 변수는 객체마다 따로 생성되지 않고, 클래스 단위로 하나만 존재합니다.

```cpp
class Player
{
public:
    static int Count;
};
```

```text
일반 멤버 변수
-> 객체마다 하나씩 존재

static 멤버 변수
-> 클래스에 하나만 존재
-> 모든 객체가 공유
```

예를 들어 생성된 플레이어 수를 세고 싶다면 이렇게 사용할 수 있습니다.

```cpp
class Player
{
public:
    static int Count;

    Player()
    {
        Count++;
    }
};

int Player::Count = 0;
```

```cpp
Player a;
Player b;
Player c;

cout << Player::Count; // 3
```

`static` 멤버 변수는 `Player::Count`처럼 클래스 이름으로 접근할 수 있습니다.

## static 멤버 함수

`static` 멤버 함수는 객체 없이 호출할 수 있는 함수입니다.

```cpp
class MathUtil
{
public:
    static int Add(int a, int b)
    {
        return a + b;
    }
};
```

```cpp
int result = MathUtil::Add(3, 5);
```

객체를 만들지 않아도 클래스 이름으로 호출할 수 있습니다.

다만 `static` 멤버 함수는 특정 객체에 속한 함수가 아니기 때문에 일반 멤버 변수에 직접 접근할 수 없습니다.

```cpp
class Player
{
public:
    int hp = 100;

    static void PrintHp()
    {
        cout << hp; // 오류
    }
};
```

`hp`는 객체마다 다른 값인데, `static` 함수는 어떤 객체의 `hp`를 말하는지 알 수 없기 때문입니다.

대신 `static` 멤버 변수에는 접근할 수 있습니다.

```cpp
class Player
{
public:
    static int Count;

    static void PrintCount()
    {
        cout << Count;
    }
};
```

## 게임 개발 연결

게임 개발에서는 다음과 같은 곳에서 `static`을 볼 수 있습니다.

```text
전체 몬스터 수
생성된 총알 수
전역 설정값
유틸리티 함수
싱글톤 패턴
Manager 접근
```

다만 `static`을 너무 많이 쓰면 전역 변수처럼 남용될 수 있습니다.

어디서든 접근 가능한 상태가 많아지면 코드 추적이 어려워질 수 있습니다.

따라서 다음을 생각해야 합니다.

```text
정말 공유되어야 하는가?
객체마다 달라야 하는 값은 아닌가?
전역 상태를 너무 많이 만들고 있지는 않은가?
```

## 면접 답변

`static`은 위치에 따라 의미가 조금 다르지만, 공통적으로 객체 수명이나 객체별 소유와 다른 방식으로 값을 유지하거나 공유할 때 사용합니다. 지역 변수에 붙이면 함수가 끝나도 값이 유지되고, 클래스 멤버 변수에 붙이면 객체마다 따로 생성되지 않고 클래스 단위로 하나만 존재합니다. `static` 멤버 함수는 객체 없이 호출할 수 있지만, 특정 객체의 일반 멤버 변수에는 직접 접근할 수 없습니다.

## 확인 질문

- `static` 지역 변수는 일반 지역 변수와 수명이 어떻게 다를까?
- `static` 멤버 변수는 객체마다 따로 존재할까, 클래스에 하나만 존재할까?
- `static` 멤버 함수가 일반 멤버 변수에 직접 접근할 수 없는 이유는 무엇일까?

