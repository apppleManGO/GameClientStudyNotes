# 스마트 포인터

## 한 줄 정의

스마트 포인터는 RAII 원리를 이용해서 힙 메모리를 자동으로 관리해주는 포인터입니다.

## 왜 필요한가

직접 `new/delete`를 사용하면 사람이 실수할 수 있습니다.

```cpp
int* value = new int(10);

delete value;
```

`delete`를 까먹으면 메모리 누수가 발생할 수 있고, `delete` 이후 다시 접근하면 댕글링 포인터 문제가 생길 수 있습니다.

스마트 포인터는 이런 메모리 관리를 객체 수명에 맡깁니다.

```text
스마트 포인터 생성
-> 힙 메모리 소유
-> 스마트 포인터 소멸
-> 힙 메모리 자동 해제
```

## unique_ptr

`unique_ptr`은 하나의 객체에 대해 유일한 소유권을 가지는 스마트 포인터입니다.

```cpp
#include <memory>

std::unique_ptr<int> value = std::make_unique<int>(10);
```

이 코드는 힙에 `int` 값을 만들고, 그 소유권을 `unique_ptr`이 가지게 합니다.

`unique_ptr`이 스코프를 벗어나면 소멸자에서 자동으로 메모리를 해제합니다.

## unique_ptr은 복사할 수 없다

```cpp
std::unique_ptr<int> a = std::make_unique<int>(10);
std::unique_ptr<int> b = a; // 불가능
```

`unique_ptr`은 유일한 소유권을 보장해야 하므로 복사가 금지됩니다.

복사를 허용하면 두 `unique_ptr`이 같은 메모리를 소유하게 되고, 둘 다 소멸될 때 같은 메모리를 두 번 해제하는 문제가 생길 수 있습니다.

소유권 이전은 `std::move`로 합니다.

```cpp
std::unique_ptr<int> a = std::make_unique<int>(10);
std::unique_ptr<int> b = std::move(a);
```

이후 상태는 이렇게 이해하면 됩니다.

```text
처음:
a -> int 10 소유
b -> 없음

std::move(a) 이후:
a -> 비어 있음
b -> int 10 소유
```

## shared_ptr

`shared_ptr`은 여러 곳에서 같은 객체를 공동 소유할 수 있는 스마트 포인터입니다.

```cpp
std::shared_ptr<int> a = std::make_shared<int>(10);
std::shared_ptr<int> b = a;
```

`a`와 `b`는 같은 `int` 값을 공유합니다.

`shared_ptr`은 내부적으로 몇 개의 `shared_ptr`이 객체를 소유하고 있는지 셉니다.

이것을 참조 카운트라고 합니다.

```text
참조 카운트가 0이 되면 메모리 해제
```

편리하지만 참조 카운트 관리 비용이 있고, 잘못 사용하면 순환 참조 문제가 생길 수 있습니다.

## weak_ptr

`weak_ptr`은 `shared_ptr`이 관리하는 객체를 소유하지 않고 관찰만 할 때 사용합니다.

```cpp
std::shared_ptr<int> owner = std::make_shared<int>(10);
std::weak_ptr<int> observer = owner;
```

```text
shared_ptr = 소유함
weak_ptr = 관찰만 함
```

`shared_ptr`끼리 서로를 소유하면 참조 카운트가 0이 되지 않아 메모리가 해제되지 않을 수 있습니다.

이때 한쪽을 `weak_ptr`로 바꾸면 순환 참조를 끊을 수 있습니다.

## 비교

```text
unique_ptr
- 혼자 소유
- 복사 불가
- 소유권 이전은 std::move
- 가장 기본적으로 권장

shared_ptr
- 여러 곳에서 공동 소유
- 참조 카운트 사용
- 마지막 shared_ptr이 사라지면 해제
- unique_ptr보다 비용이 있음

weak_ptr
- 소유하지 않음
- shared_ptr 객체를 관찰
- 순환 참조 방지에 사용
```

## 게임 개발 연결

게임 개발에서는 객체의 수명이 중요합니다.

```text
이 몬스터는 누가 소유하는가?
이 이펙트는 언제 사라져야 하는가?
이 UI는 누가 관리하는가?
이 리소스는 여러 곳에서 공유되는가?
```

스마트 포인터는 "누가 객체를 소유하는가"를 코드에 드러내는 도구입니다.

다만 Unreal에서는 `UObject`, `AActor` 같은 엔진 객체를 무조건 표준 스마트 포인터로 관리하면 안 됩니다.

Unreal에서는 다음 개념을 별도로 배웁니다.

```text
UPROPERTY
TObjectPtr
TWeakObjectPtr
TSharedPtr
TUniquePtr
```

지금 단계에서는 C++ 표준 스마트 포인터의 기본 개념을 이해하는 것이 목표입니다.

## 면접 답변

스마트 포인터는 RAII 원리를 이용해 동적 메모리를 자동으로 관리하는 포인터입니다. `unique_ptr`은 하나의 소유자만 가지며 복사가 금지되고, 소유권 이전은 `std::move`로 가능합니다. `shared_ptr`은 참조 카운트를 통해 여러 소유자가 객체를 공유할 수 있고, `weak_ptr`은 객체를 소유하지 않고 관찰만 하며 순환 참조를 방지할 때 사용합니다.

## 확인 질문

- `unique_ptr`은 왜 복사할 수 없을까?
- `shared_ptr`의 참조 카운트는 무엇일까?
- `weak_ptr`은 왜 필요할까?

