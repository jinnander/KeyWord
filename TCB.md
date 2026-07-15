# TCB (Thread Control Block)

## TCB란?

TCB(Thread Control Block)는 **운영체제가 스레드를 관리하기 위해 사용하는 자료구조**입니다.

운영체제는 각 스레드마다 하나의 TCB를 생성하여 실행 상태와 필요한 정보를 저장합니다.

즉,

> **TCB는 스레드의 실행 정보를 저장하는 자료구조입니다.**

---

# 왜 TCB가 필요한가?

CPU는 한 번에 하나의 스레드만 실행할 수 있습니다.

여러 스레드를 동시에 실행하는 것처럼 보이게 하기 위해 운영체제는 매우 빠르게 스레드를 전환(Context Switching)합니다.

이때 현재 실행 중인 스레드의 상태를 저장하고, 다음 스레드의 상태를 복원해야 합니다.

이 정보를 저장하는 곳이 **TCB**입니다.

---

# TCB에 저장되는 정보

운영체제마다 조금씩 다르지만 일반적으로 다음과 같은 정보가 저장됩니다.

| 정보 | 설명 |
|------|------|
| Thread ID(TID) | 스레드의 고유 번호 |
| Thread State | Ready, Running, Waiting 등의 상태 |
| Program Counter(PC) | 다음에 실행할 명령어 주소 |
| CPU Register | CPU 레지스터 값 |
| Stack Pointer(SP) | 현재 스택의 위치 |
| Priority | 스레드 우선순위 |
| Scheduling 정보 | 스케줄러가 사용하는 정보 |
| 소속 프로세스 정보 | 어떤 프로세스에 속하는지에 대한 정보 |

---

# TCB와 PCB의 관계

프로세스는 하나 이상의 스레드를 가질 수 있습니다.

운영체제는 **프로세스는 PCB**, **스레드는 TCB**로 관리합니다.

```
Process

↓

PCB

↓

+----------------------+
|      Thread 1        |
|        TCB           |
+----------------------+

+----------------------+
|      Thread 2        |
|        TCB           |
+----------------------+

+----------------------+
|      Thread 3        |
|        TCB           |
+----------------------+
```

PCB는 프로세스 전체를 관리하고,

TCB는 각각의 스레드를 관리합니다.

---

# TCB와 Context Switching

예를 들어

```
Thread A 실행

↓

CPU Register 저장

↓

TCB(A)에 저장

↓

Thread B의 TCB 읽기

↓

CPU Register 복원

↓

Thread B 실행
```

이 과정을 **Thread Context Switching**이라고 합니다.

TCB가 있기 때문에 운영체제는 스레드를 빠르게 전환할 수 있습니다.

---

# PCB와 TCB 비교

| 항목 | PCB | TCB |
|------|-----|-----|
| 관리 대상 | Process | Thread |
| 생성 시점 | 프로세스 생성 | 스레드 생성 |
| 저장 정보 | 프로세스 전체 정보 | 스레드 실행 정보 |
| 메모리 정보 | 포함 | 일반적으로 PCB를 참조 |
| CPU Register | 포함 | 포함 |
| Program Counter | 포함 | 포함 |
| Stack 정보 | 프로세스 전체 | 스레드별 Stack Pointer |

---

# Thread마다 Stack이 필요한 이유

각 스레드는 독립적으로 함수를 실행합니다.

따라서

- 지역 변수(Local Variable)
- 함수 호출 정보
- 반환 주소(Return Address)

를 저장하기 위해 **각 스레드는 자신만의 Stack**을 가집니다.

반면,

- Code
- Data
- Heap

영역은 같은 프로세스 내의 스레드들이 공유합니다.

```
          Process

+---------------------------+
| Code (공유)               |
+---------------------------+
| Data (공유)               |
+---------------------------+
| Heap (공유)               |
+---------------------------+

Thread 1
+-----------+
| Stack     |
+-----------+

Thread 2
+-----------+
| Stack     |
+-----------+

Thread 3
+-----------+
| Stack     |
+-----------+
```

---

# TCB의 특징

- 스레드마다 하나씩 존재한다.
- 운영체제가 생성하고 관리한다.
- Context Switching 시 사용된다.
- CPU 실행 정보를 저장한다.
- PCB와 함께 스케줄링에 사용된다.

---

# 핵심 정리

- TCB(Thread Control Block)는 스레드의 실행 정보를 저장하는 자료구조이다.
- 운영체제는 각 스레드마다 하나의 TCB를 생성한다.
- TCB에는 Thread ID, Program Counter, Register, Stack Pointer, 상태, 우선순위 등의 정보가 저장된다.
- Context Switching 시 현재 스레드의 상태를 TCB에 저장하고, 다음 스레드의 상태를 TCB에서 복원한다.
- PCB는 프로세스를 관리하고, TCB는 스레드를 관리한다.
- 같은 프로세스의 스레드는 Code, Data, Heap을 공유하지만, Stack은 각각 독립적으로 가진다.