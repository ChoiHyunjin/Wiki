# Process Management

# Job vs Process

## Job

실행할 프로그램 + 데이터

## Process

실행을 위해 시스템(커널)에 등록되어 메모리를 할당 받은 작업(Job). 성능 향상을 위해 커널에 의해 관리된다.

![Untitled](images/Process%20Management/Untitled.png)

### 프로세스의 정의

- 커널에 등록되어 커널의 관리를 받는 작업
- 각종 자원들을 요청하고 할당 받으며 반납할 수 있다.(능동적인 개체)
- 프로세스 관리 블록(PCB)를 할당 받은 개체

# 프로세스의 종류

## 역할에 따른 구분

- 시스템(커널) 프로세스 : 모든 시스템 메모리와 프로세서의 명령에 액세스 할 수 있는 프로세스. 프로세스 실행 순서를 제어하고 다른 사용자 및 커널 영역을 침범하지 못하게 감시하며, 사용자 프로세스를 생성한다.
- 사용자 프로세스 : 사용자 코드를 수행하는 프로세스

## 병행 수행 방법에 따른 구분

- 독립 프로세스 : 다른 프로세스에 영향을 주거나 받지 않는 병행 프로세스
- 협력 프로세스 : 다른 프로세스에 영향을 주거나 받는 병행 프로세스

# 자원이란?

커널의 관리하에 프로세스에 할당/반납되는 수동적 개체

## 분류

- H/W : Processor, memory, disk, monitor, Etc
- S/W : File, Message, Etc

# Process Control Block (PCB)

프로세스를 컨트롤하기 위한 정보가 저장된 공간. 때문에 커널에 존재한다.

프로세스 생성시에 PCB도 생성된다.

![Untitled](images/Process%20Management/Untitled%201.png)

## PCB가 관리하는 정보

![Untitled](images/Process%20Management/Untitled%202.png)

- PID
- 스케줄링 정보
- 프로세스 상태
- 메모리 관리 정보
- 입출력 상태 정보
- context : 프로세스의 레지스터 상태를 저장하는 공간
- 계정 정보

>> *PCB는 OS별로 조금씩 다르다. <<*

>> *PCB 참조 및 갱신 속도는 OS의 성능을 결정하는 중요 요소 중 하나. <<*

# 프로세스의 생성

![Untitled](images/Process%20Management/Untitled%203.png)

프로세스는 프로세스를 생성할 수 있다.

## 생성 시기

- 시스템 초기화
- 다른 프로세스의 시스템 콜 호출에 의해
- 사용자의 생성
- 배치 작업 시

## 생성 과정

1. 프로세스 생성 권한 확인
2. PID 할당
3. 주소 공간, PCB공간 할당
4. PCB초기화
5. 링크(큐에 삽입)

## Process State

자원간의 상호작용에 의해 결정된다.

![Untitled](images/Process%20Management/Untitled%204.png)

### New / Create

작업(job)이 커널에 등록된 상태.

PCB 할당 및 프로세스가 생성된다.

메모리 공간이

- 있으면 : ready 상태로 전환
- 없으면 : suspend 상태로 전환

### Ready

프로세서 할당 대기 상태. 즉시 실행 가능한 상태.

CPU를 할당 받으면 Running 상태가 된다.

### Running

프로세서와 필요 자원을 모두 할당 받은 상태

- Preemption
    
    프로세스를 뺏기는 현상. ready 상태로 돌아간다.
    
    주로 time-out, priority 등의 스케줄링에 의해 일어난다.
    
- Block/sleep
    
    I/O 등 자원 할당 요청 후, 할당 전까지 asleep 상태로 들어간다.
    

### Block/Asleep/Wait

프로세서 외 다른 자원을 기다리는 상태. 자원 할당은 system call에 의해 이루어진다.

다시 자원을 얻은 후 다시 프로세서를 얻기 위한 Ready 상태로 변환한다.

### Suspended

메모리를 할당 받지 못한(빼앗긴) 상태.

- Suspended ready : 메모리를 할당 받지 못한 ready 상태.
- Suspended block
    - block 상태에서 프로세스 뿐 아니라 메모리까지 빼앗긴 상태.
    - 메모리를 빼앗길 때, 기존 메모리 정보를 임시 저장하기 위한 Memory Image를 생성해서, Swap device에 저장한다(Swap-out = suspended).
    - 메모리를 다시 할당 받으면 Swap device에서 Memory Image를 받아와 적용한다(Swap-in = resume).

### Terminated(Zombie)

프로세스의 수행이 끝난 상태.

모든 자원 반납 후, 커널 내에 일부 PCB 정보만 남겨둔다.

# Interrupt

**예상치** 못한 **외부**에서 발생한 이벤트

### 종류

- I/O
- Clock : CPU 의 클럭에서 나는 인터럽트
- Console : 콘솔 창에서 발생
- Program Check
- Machine Check
- Inter-process
- System Call

## 인터럽트 처리 과정

1. 인터럽트
2. (커널 개입) 프로세스 중단
3. 인터럽트 처리
    1. 발생 장소, 원인 파악
    2. 서비스 할 것인지 파악
    3. 서비스

![Untitled](images/Process%20Management/Untitled%205.png)

1. 인터럽트 발생
2. Pi 정지
3. 현재 정보 PCB에 저장(Context saving)
4. 커널의 Interrupt Handler가 인터럽트 핸들링(발생 장소, 원인 파악, 서비스 결정 등)
5. Interrupt Service 시작 - 이 또한 작은 프로세스로, 프로세서에 들어간다.
6. Service 가 끝나면 Pi가 아닌, 다른 대기중인 Pj 가 프로세서에 들어간다.
7. 이 때, PCB j에서 Context를 가져와 Processor에 로드한다.

## System Call

![Untitled](images/Process%20Management/Untitled%206.png)

1. (사용자 모드에서) 프로세스 A 실행
2. OS에 시스템 콜 호출 후 A를 처리하는 프로세서는 Trap 인터럽트를 통해 Kernel mode로 전환한다.
3. 이후 시스템 콜을 수행한다.

## Processor Mode

### 커널 모드

Ring 0. Supervisor Mode

- 모든 명령어 사용 가능
- Kernel
- Mode bit 0
- 시스템 공간, 사용자 공간

### 사용자 모드

Ring 3

- 일부 명령어
    - I/O 명령 불가(커널의 영역이므로). 그래서 시스템 콜을 통해 OS 에게 요청
    - 시스템 콜 : Mode bit를 변경하여 Kernel모드로 바뀌어 시스템 명령을 수행하고 끝나면 Mode bit 복구
- 응용 프로그램
- 사용자 메모리 공간

## Context Switching

### Context

프로세스와 관련된 정보의 집합

- CPU : register context
- Memory : Code, Data, Stack, PCB 등

### Context Switching

- 프로세서가 프로세스 P1를 동작 하던 중, 프로세스 P2로 교체를 위해 현재 P1 정보를 저장하고, P2 정보를 로드하는 것.

# 스케줄링 성능요소

- CPU 사용량
- 처리량
- 반환 시간
- waiting time
- response time

## Preemption vs Non-Preemption

### Preemption (선점형)

OS가 프로세서를 관리하기 때문에, interrupt 등의 이유로 프로세스에게 할당/회수하여 스케줄링 하는 방법

### Non-Preemption (비선점형)

OS가 프로세서를 관리하지 않기 때문에, 프로세스에게 할당되면 끝날때 까지 프로세서를 소유함.

## First Come, First Served (FCFS)

먼저 온 놈 먼저.

단점 : 먼저 온 작업이 엄청 길다면?

### Shortest Job First(SJF)

짧은 작업 먼저.

짧다는 기준 : 이전 프로세스의 작업들의 통계치를 기준으로 선정.

선점, 비선점 모두 가능

단점 : 사용 시간이 긴 녀석들은 영원히 CPU 받을 수 없을지도

### Priority

우선순위가 높은 친구들부터.

선점, 비선점 모두 가능

단점 : 우선 순위가 낮은 친구들은 무기한 대기할 수도 있다. → Aging을 높여 해결하는 방식도 있다.

### Round Robin

일정한 시간으로 번갈아가면서 수행. 수행 후에는 Queue의 젤 뒤로 이동.

단점 : 너무 빈번하게 되면 Context Switching에 의한 Overhead발생.