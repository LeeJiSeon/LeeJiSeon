# OS (Operating System)

## 1. 프로세스 & 스레드
### _1) 프로세스_
> : 컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램
* 메모리에 올라와 실행되고 있는 프로그램의 인스턴스 (독립적인 개체)
* 운영체제로부터 시스템 자원을 할당받는 작업의 단위

#### [할당받는 시스템 자원의 예]
* CPU 시간
* 운영되기 위해 필요한 주소 공간
* Code, Data, Stack, Heap의 구조로 되어 있는 독립된 메모리 영역

![process](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/process.png "process")

#### [특징]
* 프로세스는 각각 독립된 메모리 영역(Code, Data, Stack, Heap의 구조)을 할당받는다.
* 기본적으로 프로세스당 최소 1개의 스레드(메인 스레드)를 가지고 있다.
* 각 프로세스는 별도의 주소 공간에서 실행되며, 한 프로세스는 다른 프로세스의 변수나 자료구조에 접근할 수 없다.
* 한 프로세스가 다른 프로세스의 자원에 접근하려면 프로세스 간의 통신(IPC, inter-process communication)을 사용해야 한다.
  * 파이프, 파일, 소켓 등을 이용한 통신 방법 이용
  
### _2) 스레드_
> : 프로세스 내에서 실행되는 여러 흐름의 단위
* 프로세스의 특정한 수행 경로
* 프로세스가 할당받은 자원을 이용하는 실행의 단위

![thread](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/thread.png "thread")

#### [특징]
* 스레드는 프로세스 내에서 각각 Stack만 따로 할당받고 Code, Data, Heap 영역은 공유한다.
* 각각의 스레드는 별도의 레지스터와 스택을 갖고 있지만, 힙 메모리는 서로 읽고 쓸 수 있다.
* 한 스레드가 프로세스 자원을 변경하면, 다른 이웃 스레드(sibling thread)도 그 변경 결과를 즉시 볼 수 있다.

### _3) 멀티 프로세스 대신 멀티 스레드를 사용하는 이유_
> : 프로그램을 여러 개 키는 것보다 하나의 프로그램 안에서 여러 작업을 해결하는 것이 효율적
* 자원의 효율성 증대
  * 멀티 프로세스로 실행되는 작업을 멀티 스레드로 실행할 경우, 프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어들어 자원을 효율적으로 관리할 수 있다.
  * 스레드는 프로세스 내의 메모리를 공유하기 때문에 독립적인 프로세스와 달리 스레드 간 데이터를 주고 받는 것이 간단해지고 시스템 자원 소모가 줄어들게 된다.
* 처리 비용 감소 및 응답 시간 단축
  * 프로세스 간의 통신(IPC)보다 스레드 간의 통신의 비용이 적으므로 작업들 간의 통신의 부담이 줄어든다.
  * 프로세스 간의 전환 속도보다 스레드 간의 전환 속도가 빠르다.

![multi](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/multi-thread.png "multi thread")

## 2. 교착상태

### _1) 교착상태 (Deadlock)_
* 첫 번째 스레드는 두 번째 스레드가 들고 있는 객체의 락이 풀리기를 기다리고 있고, 두 번째 스레드 역시 첫 번째 스레드가 들고 있는 객체의 락이 풀리기를 기다리는 상황을 일컷는다.
* 모든 스레드가 락이 풀리기를 기다리고 있기 때문에, 무한 대기 상태에 빠지게 된다. 이런 스레드를 교착상태에 빠졌다고 한다.

#### [조건]
* 상호 배제(mutual exclusion)
  * 한 번에 한 프로세스만 공유 자원을 사용할 수 있다.
  * 좀 더 정확하게는, 공유 자원에 대한 접근 권한이 제한된다. 자원의 양이 제한되어 있더라도 교착상태는 발생할 수 있다.
* 들고 기다리기(hold and wait) = 점유대기
  * 공유 자원에 대한 접근 권한을 갖고 있는 프로세스가, 그 접근 권한을 양보하지 않은 상태에서 다른 자원에 대한 접근 권한을 요구할 수 있다.
* 선취(preemption) 불가능 = 비선점
  * 한 프로세스가 다른 프로세스의 자원 접근 권한을 강제로 취소할 수 없다.
* 대기 상태의 사이클(circular wait) = 순환대기
  * 두 개 이상의 프로세스가 자원 접근을 기다리는데, 그 관계에 사이클이 존재한다.
 
#### [방지 방법]
* 4가지 조건들 가운데 하나를 제거하면 된다.
* 공유 자원 중 많은 경우가 한 번에 한 프로세스만 사용할 수 있기 때문에(예를 들어, 프린트) 1번 조건은 제거하기 어렵다.
* 대부분의 교착상태 방지 알고리즘은 4번 조건, 즉 대기 상태의 사이클이 발생하는 일을 막는 데 초점이 맞춰져 있다.

## 3. Context Switching

### _1) Context Switching_
> : 현재 진행하고 있는 Task(Process, Thread)의 상태를 저장하고 다음 진행할 Task의 상태 값을 읽어 적용하는 과정을 말한다.

#### [과정]
1. Task의 대부분 정보는 Register에 저장되고 PCB(Process Control Block)로 관리된다.
1. 현재 실행하고 있는 Task의 PCB 정보를 저장한다. (Process Stack, Ready Queue)
1. 다음 실행할 Task의 PCB 정보를 읽어 Register에 적재하고 CPU가 이전에 진행했던 과정을 연속적으로 수행할 수 있다.

#### [비용 비교]
* Process Context Switching 비용 > Thread Context Switching 비용
* Thread는 Stack 영역을 제외한 모든 메모리를 공유하므로 Context Switching 수행 시 Stack 영역만 변경하면 되기 때문에 비용이 적게 든다.

## 4. Scheduling (스케줄링)
* CPU가 하나의 프로세스 작업이 끝나면 다음 프로세스 작업을 수행해야 한다.
* 이때 다음 프로세스가 어느 프로세스인지를 선택하는 알고리즘을 CPU Scheduling 알고리즘이라고 한다.

#### [Preemptive VS Non-preemptive]
* Preemptive (선점)
  * 프로세스가 CPU를 점유하고 있는 동안 I/O나 인터럽트가 발생한 것도 아니고 모든 작업을 끝내지도 않았는데, 다른 프로세스가 해당 **CPU를 강제로 점유** 할 수 있다.
  * 즉, 프로세스가 정상적으로 수행중인 가운데 다른 프로세스가 CPU를 강제로 점유하여 실행할 수 있다.
* Non-preemptive (비선점)
  * 한 프로세스가 한 번 CPU를 점유했다면, I/O나 인터럽트 발생 또는 프로세스 종료가 될 때까지 다른 프로세스가 CPU를 점유하지 못한다.

#### [Scheduling criteria (척도)]
> : 스케줄링의 효율을 분석하는 기준
* CPU Utilization (이용률, %): CPU가 수행되는 비율
* Throughput (처리율, jobs/sec): 단위시간당 처리하는 작업의 수(처리량)
* Turnaround time (반환시간): 프로세스의 처음 시작 시간부터 모든 작업을 끝내고 종료하는데 걸린 시간이다.(CPU, waiting, I/O 등 모든 시간을 포함한다.) 반환시간은 짧을 수록 좋다.
* Waiting time (대기시간): CPU를 점유하기 위해서 ready queue에서 기다린 시간을 말한다.(다른 큐에서 대기한 시간은 제외한다.)
* Response time (응답시간): 일반적으로 대화형 시스템에서 입력에 대한 반응 시간을 말한다.

### _1) First-Come, First-Served (FCFS / FIFO)_
> : 먼저 온 프로세스가 먼저 CPU를 점유하는 방식

#### [특징]
* 비선점형 스케줄링
* 매우 단순하고 많이 사용하는 방법이지만, 모든 부분에서 효율적인 것은 아니다.

#### [문제점]
* convoy effect : 소요시간이 긴 프로세스가 먼저 도달하여 효율성을 낮추는 현상

### _2) SJF (Shortest-Job-First)_

#### [특징]
* 비선점형 스케줄링
* 다른 프로세스가 먼저 도착했어도 CPU 점유시간이 짧은 프로세스에게 먼저 할당

#### [문제점]
* starvation : CPU 사용이 짧은 작업을 극단적으로 선호햐여 사용시간이 긴 프로세스는 거의 영원히 CPU를 할당받을 수 없는 현상
  * 효율성을 추구하는 것이 가장 중요하지만 특정 프로세스가 지나치게 차별받으면 안된다.

### _3) SRT(Shortest Remaining Time First)_

#### [특징]
* 선점형 스케줄링
* 새로운 프로세스가 도착할 때마다 새로운 스케줄링이 이루어진다.
* 현재 수행중인 프로세스의 남은 점유시간보다 더 짧은 CPU 점유시간을 가진 새로운 프로세스가 도착하면 CPU를 뺏긴다.

#### [문제점]
* startvation

### _4) Priority Scheduling_

#### [특징]
* 우선순위가 가장 높은 프로세스에게 CPU를 할당한다.
* 선점형 스케줄링 방식
  * 더 높은 우선순위의 프로세스가 도착하면 실행중인 프로세스를 멈추고 CPU를 선점한다.
* 비선점형 스케줄링 방식
  * 더 높은 우선순위의 프로세스가 도착하면 Ready Queue의 Head에 넣는다.
  
#### [문제점]
* startvation = Indefinite blocking

#### [해결책]
* aging : 오래 기다린 프로세스의 우선순위를 높여준다.

### _5) Round Robin_

#### [특징]
* 현대적인 스케줄링
* 각 프로세스는 동일한 크기의 할당시간을 갖게 된다.
* 할당시간이 지나면 프로세스는 선점당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다.

#### [주의할 점]
* 할당시간의 적절한 설정
  * 너무 큰 경우 : FCFS와 같아진다.
  * 너무 작은 경우 : 잦은 context switch로 오베허드가 발생한다.

### _6) Multilevel Queue_

#### [프로세스 분류]
* System processes : 운영체제 커널 수준의 프로세스
* Interactive processes : 유저 수준의 대화형 프로세스
* Interactive editing processes
* Batch processes : 대화형 프로세스의 반대인 것으로 일정량을 한 번에 처리하는 프로세스(ex) 컴파일러)
* Student processes

![multileve](https://user-images.githubusercontent.com/34755287/53879673-5e979880-4052-11e9-9f9b-e8bfec7c9be6.png "multilevel queue")

#### [특징]
* 분류된 프로세스 그룹에 따라 큐를 두어 여러 개의 큐를 사용하는 방식
* 큐마다 우선순위를 지정해줄 수 있다.
* 큐마다 CPU 시간을 다르게 줄 수도 있고, 큐마다 다른 스케줄링 방식을 사용할 수도 있다.

### _7) Multilevel Feedback Queue_

![feedback](https://user-images.githubusercontent.com/34755287/53879675-5f302f00-4052-11e9-86a2-c02ee03bac64.png "multilevel feedback queue")

#### [특징]
* 여러 개의 큐를 사용한다.
* 대기시간을 조정할 수 있다.
  * 먼저 모든 프로세스는 가장 위의 큐에서 CPU의 점유를 대기한다.
  * 이 상태로 진행하다가 이 큐에서 기다리는 시간이 너무 오래 걸린다면 아래의 큐로 프로세스를 옮긴다.
* 각 큐마다 다른 스케줄링, 다른 우선순위 등을 사용할 수 있다.



---
## _* 참고_
1. <https://github.com/WeareSoft/tech-interview/blob/master/contents/os.md#%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B4%80%EB%A6%AC-%EC%A0%84%EB%9E%B5>
1. <https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-6.-CPU-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81>
1. <https://k39335.tistory.com/33>
