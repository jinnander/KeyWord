# PCB - Process State (프로세스 상태)

## Process State란?

Process State(프로세스 상태)는 **현재 프로세스가 어떤 상태에 있는지를 나타내는 정보**입니다.

운영체제는 각 프로세스의 상태를 **PCB(Process Control Block)** 에 저장하여 관리합니다.

즉,

> **Process State는 프로세스의 현재 실행 상태를 나타내는 정보입니다.**

---

# 왜 Process State가 필요한가?

CPU는 한 번에 하나의 프로세스만 실행할 수 있습니다.

운영체제는 여러 프로세스를 번갈아 실행하기 위해

- 어떤 프로세스가 실행 중인지
- CPU를 기다리는지
- 입출력을 기다리는지
- 종료되었는지

등을 관리해야 합니다.

이 정보를 PCB의 Process State에 저장합니다.

---

# 프로세스 상태

일반적으로 프로세스는 다음과 같은 상태를 가집니다.

```
           +------+
           | New  |
           +------+
               |
               v
           +-------+
           | Ready |
           +-------+
               |
               v
           +---------+
           | Running |
           +---------+
            |       |
   I/O 요청 |       | 종료
            v       v
       +---------+ +------------+
       | Waiting | | Terminated |
       +---------+ +------------+
            |
     I/O 완료
            |
            v
         Ready
```

---

# 1. New (생성)

프로세스가 처음 생성되는 상태입니다.

운영체제는

- PCB 생성
- 메모리 할당
- 필요한 자원 준비

를 수행합니다.

### 특징

- 아직 실행되지 않음
- 생성 준비 단계

---

# 2. Ready (준비)

프로세스가 실행 준비는 끝났지만

**CPU를 할당받지 못한 상태**입니다.

```
Ready Queue

↓

Process A

Process B

Process C
```

운영체제의 스케줄러는 Ready Queue에서 다음 실행할 프로세스를 선택합니다.

### 특징

- 실행 준비 완료
- CPU 대기 중

---

# 3. Running (실행)

CPU를 할당받아 실제로 실행되는 상태입니다.

프로세스는

- 계산 수행
- 함수 실행
- 메모리 접근

등을 수행합니다.

### 특징

- CPU 사용 중
- 실제 명령어 실행

---

# 4. Waiting (Blocked)

프로세스가 CPU를 사용할 수 없고

특정 이벤트를 기다리는 상태입니다.

예를 들어

- 파일 읽기
- 키보드 입력
- 네트워크 통신
- 디스크 접근

등이 있습니다.

```
파일 읽기 요청

↓

Waiting

↓

파일 읽기 완료

↓

Ready
```

### 특징

- CPU 사용 안 함
- I/O 완료를 기다림

---

# 5. Terminated (종료)

프로세스 실행이 모두 끝난 상태입니다.

운영체제는

- 메모리 해제
- PCB 제거
- 자원 회수

를 수행합니다.

### 특징

- 실행 종료
- 모든 자원 반환

---

# 상태 전이(State Transition)

프로세스는 실행 중 상태가 계속 변경됩니다.

```
New
 ↓
Ready
 ↓
Running
 ↓
Waiting
 ↓
Ready
 ↓
Running
 ↓
Terminated
```

### 주요 전이

| 현재 상태 | 다음 상태 | 이유 |
|-----------|-----------|------|
| New | Ready | 생성 완료 |
| Ready | Running | CPU 할당 |
| Running | Waiting | I/O 요청 |
| Waiting | Ready | I/O 완료 |
| Running | Terminated | 실행 종료 |

---

# PCB에서 Process State의 역할

PCB에는 다양한 정보가 저장됩니다.

```
PCB

├── PID
├── Process State
├── Program Counter
├── CPU Register
├── Memory Information
├── Priority
├── Scheduling Information
└── Open File Information
```

운영체제는 Process State를 확인하여

- 실행 가능한 프로세스인지
- CPU를 기다리는지
- 입출력을 기다리는지

를 판단합니다.

---

# Context Switching과 Process State

예를 들어

```
Running

↓

시간 할당 종료(Time Slice)

↓

Ready

↓

다른 프로세스 Running
```

운영체제는 Context Switching을 수행하면서

PCB의 Process State를 함께 변경합니다.

예를 들어

```
Process A

Running

↓

Ready

----------------

Process B

Ready

↓

Running
```

---

# Process State의 특징

- PCB에 저장된다.
- 운영체제가 관리한다.
- 스케줄링의 기준이 된다.
- Context Switching 시 변경된다.
- 프로세스의 현재 실행 상태를 나타낸다.

---

# 핵심 정리

- Process State는 프로세스의 현재 실행 상태를 나타내는 정보이다.
- Process State는 PCB(Process Control Block)에 저장된다.
- 프로세스는 New, Ready, Running, Waiting, Terminated 상태를 가진다.
- 운영체제는 Process State를 이용하여 CPU 스케줄링을 수행한다.
- Context Switching 과정에서 Process State가 변경된다.