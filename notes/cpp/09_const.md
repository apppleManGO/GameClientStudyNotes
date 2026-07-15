# const

## 한 줄 정의

`const`는 값이나 객체를 수정하지 않겠다는 의도를 표현하는 키워드입니다.

## 왜 필요한가

`const`를 사용하면 코드에서 "이 값은 바뀌면 안 된다"는 의도를 명확히 표현할 수 있습니다.

또 함수 인자나 멤버 함수에서 `const`를 사용하면 실수로 값을 변경하는 일을 막을 수 있습니다.

## 기본 사용법

```cpp
const int hp = 100;
```

`hp`는 100으로 초기화된 뒤 값을 바꿀 수 없습니다.

```cpp
hp = 50; // 오류
```

## 함수 인자에서 const

객체가 클 때 값을 복사해서 넘기면 비용이 생길 수 있습니다.

```cpp
void PrintPlayer(Player player);
```

위 코드는 `Player` 객체를 복사해서 받습니다.

복사를 피하려면 참조로 받을 수 있습니다.

```cpp
void PrintPlayer(Player& player);
```

하지만 이 경우 함수 안에서 원본을 수정할 수 있습니다.

읽기만 할 목적이라면 `const` 참조를 사용합니다.

```cpp
void PrintPlayer(const Player& player);
```

이 뜻은 다음과 같습니다.

```text
&     -> 복사하지 않고 원본을 참조로 받겠다.
const -> 함수 안에서 player를 수정하지 않겠다.
```

즉 `const Player&`는 객체를 복사하지 않고 참조로 받아 복사 비용을 줄이면서, 함수 안에서는 해당 객체를 수정하지 않겠다는 의미입니다.

## 전달 방식 비교

```text
Player player
-> 복사해서 받음

Player& player
-> 원본을 참조로 받음, 수정 가능

const Player& player
-> 원본을 참조로 받음, 수정 불가
```

큰 객체를 읽기 전용으로 전달할 때는 `const T&`를 자주 사용합니다.

```cpp
void PrintName(const std::string& name);
void PrintPlayerInfo(const Player& player);
```

## const 멤버 함수

클래스의 멤버 함수 뒤에도 `const`를 붙일 수 있습니다.

```cpp
class Player
{
public:
    int GetHp() const
    {
        return hp;
    }

private:
    int hp = 100;
};
```

함수 뒤의 `const`는 이 함수가 객체의 멤버 변수를 수정하지 않는다는 뜻입니다.

```cpp
int GetHp() const
{
    hp = 50; // 오류
    return hp;
}
```

## 포인터와 const

포인터와 `const`가 같이 나오면 처음에는 헷갈릴 수 있습니다.

```cpp
const int* ptr;
```

이 뜻은 `ptr`이 가리키는 값을 수정할 수 없다는 뜻입니다.

```cpp
int a = 10;
int b = 20;

const int* ptr = &a;

ptr = &b;   // 가능
*ptr = 30;  // 불가능
```

반대로 아래 형태는 포인터 자체가 다른 주소를 가리킬 수 없다는 뜻입니다.

```cpp
int* const ptr = &a;

*ptr = 30;  // 가능
ptr = &b;   // 불가능
```

처음에는 이렇게 기억하면 됩니다.

```text
const int* ptr
-> 값 변경 금지

int* const ptr
-> 주소 변경 금지
```

## Unreal 연결

Unreal C++에서도 `const`는 자주 사용됩니다.

```cpp
FVector GetActorLocation() const;
```

이런 함수는 위치를 가져오기만 하고 객체 상태를 바꾸지 않는다는 뜻입니다.

함수 인자에서도 자주 볼 수 있습니다.

```cpp
void SetPlayerName(const FString& Name);
```

`FString`을 복사하지 않고 읽기만 하겠다는 의미입니다.

## 면접 답변

`const`는 값이나 객체를 수정하지 않겠다는 의도를 코드에 표현하는 키워드입니다. 변수에 사용하면 값을 변경할 수 없고, 함수 인자에서 `const T&` 형태로 사용하면 복사 비용을 줄이면서 함수 안에서 수정하지 않도록 보장할 수 있습니다. 멤버 함수 뒤에 붙는 `const`는 해당 함수가 객체의 상태를 변경하지 않는다는 의미입니다.

## 확인 질문

- `const Player& player`에서 `const`와 `&`는 각각 어떤 의미일까?
- `Player player`, `Player& player`, `const Player& player`는 어떻게 다를까?
- 멤버 함수 뒤에 붙는 `const`는 무엇을 의미할까?

