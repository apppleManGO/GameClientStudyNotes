# Game Client Study Notes

신입 게임 클라이언트 개발자를 목표로 공부한 내용을 정리하는 저장소입니다.

목표는 거창한 교재를 만드는 것이 아니라, 공부한 개념을 나중에 다시 확인하고 면접에서 내 말로 설명할 수 있게 정리하는 것입니다.

## 목표

- C++ 기초와 메모리 개념 이해
- Unity / Unreal로 이어지는 게임 클라이언트 핵심 개념 정리
- 면접에서 짧고 명확하게 설명할 수 있는 답변 준비
- 공부한 내용을 꾸준히 문서화

## 현재 진행 상황

### C++ 메모리 기초

| 번호 | 주제 | 상태 |
| --- | --- | --- |
| 01 | [메모리와 주소](notes/cpp/01_memory_address.md) | 완료 |
| 02 | [포인터](notes/cpp/02_pointer.md) | 완료 |
| 03 | [참조자](notes/cpp/03_reference.md) | 완료 |
| 04 | [스택과 힙](notes/cpp/04_stack_heap.md) | 완료 |
| 05 | [new/delete](notes/cpp/05_new_delete.md) | 완료 |
| 06 | [생성자/소멸자](notes/cpp/06_constructor_destructor.md) | 완료 |
| 07 | [RAII](notes/cpp/07_raii.md) | 완료 |
| 08 | [스마트 포인터](notes/cpp/08_smart_pointer.md) | 완료 |
| 09 | [const](notes/cpp/09_const.md) | 완료 |
| 10 | [static](notes/cpp/10_static.md) | 완료 |
| 11 | [virtual 함수와 override](notes/cpp/11_virtual_override.md) | 완료 |
| 12 | [순수가상함수와 추상 클래스](notes/cpp/12_pure_virtual_abstract.md) | 완료 |
| 13 | [상속](notes/cpp/13_inheritance.md) | 완료 |
| 14 | [다형성](notes/cpp/14_polymorphism.md) | 완료 |
| 15 | [OOP 4대 원칙](notes/cpp/15_oop_principles.md) | 완료 |
| 16 | [접근 지정자](notes/cpp/16_access_specifiers.md) | 완료 |
| 17 | [클래스와 객체](notes/cpp/17_class_object.md) | 완료 |
| 18 | [this 포인터](notes/cpp/18_this_pointer.md) | 진행 중 |

### 면접 답변

| 주제 | 파일 |
| --- | --- |
| C++ 기초 면접 답변 | [notes/interview/cpp_basic.md](notes/interview/cpp_basic.md) |

## 정리 방식

각 개념은 아래 흐름으로 정리합니다.

```text
1. 한 줄 정의
2. 왜 필요한가
3. 핵심 설명
4. C++ 예제
5. Unity / Unreal 연결
6. 면접 답변
7. 확인 질문
```

## 폴더 구조

```text
notes/
  cpp/        C++ 기초, 메모리, 포인터, 객체지향
  unity/      Unity 개념
  unreal/     Unreal 개념
  interview/  면접 답변 정리
```

## 공부 원칙

- 개념을 외우기보다 내 말로 설명할 수 있게 정리합니다.
- 처음에는 쉽게 이해하고, 나중에 정확도를 보강합니다.
- 면접 답변은 짧고 명확하게 정리합니다.
- 틀릴 수 있는 설명은 그대로 두지 않고 계속 고칩니다.
