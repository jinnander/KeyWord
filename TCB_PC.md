# TCB - Program Counter (PC)

## Program Counter란?

Program Counter(PC)는 **스레드가 다음에 실행할 명령어(Instruction)의 메모리 주소를 저장하는 CPU 레지스터**입니다.

운영체제는 Context Switching이 발생할 때 현재 스레드의 Program Counter를 TCB(Thread Control Block)에 저장하고, 다시 실행될 때 복원합니다.

즉,

> **TCB의 Program Counter는 스레드가 어디까지 실행되었는지를 기억하기 위한 정보입니다.**

---

# 왜 TCB에도 Program Counter가 필요한가?

하나의 프로세스에는 여러 개의 스레드가 존재할 수 있습니다.

예를 들어

```
Process

├── Thread A
├── Thread B
└── Thread C
```

각 스레드는 서로 다른 코드를 실행하거나, 같은 코드를 다른 위치에서 실행할 수 있습니다.

따라서 **각 스레드는 자신만의 실행 위치를 기억해야 합니다.**

이 정보를 저장하는 것이 TCB의 Program Counter입니다.

---

# 실행 과정

예를 들어

```
Thread A

↓

함수 실행

↓

Program Counter = 0x1020

↓

Context Switching

↓

TCB(A)에 저장

↓

Thread B 실행

↓

Program Counter = 0x2050

↓

Context Switching

↓

TCB(B)에 저장

↓

Thread A 복원

↓

Program Counter = 0x1020

↓

이전 위치부터 계속 실행
```

Program Counter 덕분에 스레드는 실행이 중단되었던 위치부터 다시 실행할 수 있습니다.

---

# TCB에서 Program Counter의 역할

TCB에는 스레드 실행에 필요한 정보가 저장됩니다.

```
TCB

├── Thread ID (TID)
├── Thread State
├── Program Counter
├── CPU Registers
├── Stack Pointer
├── Priority
└── Scheduling Information
```

운영체제는 Thread Context Switching 시 Program Counter를 저장하고 복원합니다.

---

# Thread Context Switching

CPU는 한 번에 하나의 스레드만 실행할 수 있습니다.

운영체제는 매우 빠르게 스레드를 전환합니다.

```
Thread A 실행

↓

Program Counter 저장

↓

TCB(A)

↓

Thread B 선택

↓

TCB(B)에서 Program Counter 복원

↓

Thread B 실행
```

이 과정을 **Thread Context Switching**이라고 합니다.

---

# Program Counter와 Stack Pointer

Program Counter와 Stack Pointer는 함께 저장됩니다.

| 항목 | 역할 |
|------|------|
| Program Counter (PC) | 다음에 실행할 명령어의 주소 |
| Stack Pointer (SP) | 현재 스택의 위치 |

두 정보를 함께 복원해야 스레드는 정확히 중단된 위치에서 다시 실행할 수 있습니다.

---

# CPU 내부에서의 Program Counter

Program Counter는 CPU 내부 레지스터입니다.

```
CPU

+----------------------------+
| Register                   |
|----------------------------|
| Program Counter (PC)       |
| Stack Pointer (SP)         |
| General Registers          |
+----------------------------+
```

Context Switching이 발생하면 이 레지스터 값들이 TCB에 저장됩니다.

---

# TCB와 PCB의 Program Counter 차이

| 구분 | PCB | TCB |
|------|-----|-----|
| 관리 대상 | 프로세스 | 스레드 |
| Program Counter | 프로세스 실행 정보로 설명되는 경우가 많음 | 각 스레드의 실행 위치를 저장 |
| Context Switching | Process Context Switching | Thread Context Switching |

> 현대 운영체제에서는 **실제로 실행되는 단위는 스레드**이므로 Program Counter는 스레드별로 관리됩니다.

---

# Program Counter의 특징

- CPU 레지스터의 값이다.
- 다음에 실행할 명령어의 주소를 저장한다.
- 스레드마다 하나씩 존재한다.
- Context Switching 시 TCB에 저장된다.
- 스레드가 중단된 위치부터 다시 실행할 수 있도록 한다.

---

# 핵심 정리

- Program Counter(PC)는 다음에 실행할 명령어의 주소를 저장하는 CPU 레지스터이다.
- 각 스레드는 자신만의 Program Counter를 가진다.
- Context Switching 시 Program Counter는 TCB에 저장되고, 다시 실행될 때 복원된다.
- Program Counter와 Stack Pointer를 함께 복원해야 스레드는 정확히 이전 상태에서 실행을 이어갈 수 있다.
- 현대 운영체제에서는 Program Counter를 포함한 실행 상태는 스레드 단위로 관리된다.