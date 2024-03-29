# 프로세스의 상태

![프로세스의 상태](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbT5UNm%2Fbtq8FUMOdzC%2FKIkmU2wZc9KN0lUpVBPje0%2Fimg.png)

---

## 1. Created State(생성 상태)

![Created State](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFrS1q%2Fbtq8IDbf1xG%2FHzebln9dwU53WLOnEGdo1K%2Fimg.png)

프로세스가 생성되는 단계로 작업을 커널에 등록하고 PCB를 할당한다.

> 커널이란? 운영체제 중 항상 메모리에 올라가 있는 운영체제의 핵심 부분으로써 하드웨어와 응용 프로그램 사이에서 인터페이스를 제공하는 역할을 하며 컴퓨터 자원들을 관리하는 역할을 한다.  
> Process Control Block, PCB, 프로세스 제어 블록이란, 운영체제가 프로세스 관리에 필요한 정보들을 담은 블록이다. 커널은 프로세스 생성시 PCB 개체를 만들고 이를 통해 프로세스들을 관리한다.

사용가능한 메모리 공간을 체크하고 할당받을 수 있는 메모리가 있는지 확인한다. 이때 메모리가 할당되면, Ready State로 넘어가고 할당 받을 수 있는 메모리가 없으면, Suspended Ready 상태로 대기한다.

---

## 2. Ready State(준비/대기 상태)

![Ready State](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPA08t%2Fbtq8FVEUY5H%2F2INHdLQ62oZSXV7vwWd3Mk%2Fimg.png)

프로세스가 생성되면 바로 CPU에 할당되어 실행되는 것이 아니라 준비 상태에 접어든다. 이때 프로세스 관리에 필요한 정보들을 담은 블록인 PCB는 준비 큐에 들어가면 CPU에 할당될 때까지 자신의 차례를 기다린다.

준비 큐에서 어떤 프로세스가 실행 상태로 옮겨지는가는 CPU 스케줄러에 의해 관리된다.

CPU 스케줄러에 의해 준비 상태의 프로세스가 실행 상태로 되는 것을 디스패치(dispatch)라고 한다.

> CPU 스케줄러란? 다중 프로그램 OS의 기본으로 여러 프로세스들이 CPU를 교환하며 사용하기 위해 필요한 스케줄러이다. 스케줄링 대상은 Ready Queue에 있는 프로세스들이다. 기본적으로 다음에 실행될 프로세스를 정하고, 프로세스들을 실행가능한 상태로 만든다. 이런 CPU 스케줄러는 CPU를 선점한 프로세스가 대기하는 시간을 보다 효율적으로 사용하기 위해 사용한다.

---

## 3. Running State(실행 상태)

![Running State](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FWOpkZ%2Fbtq8G5mipfz%2FPIE4V8XInh8pLO3DYvhXMK%2Fimg.png)

CPU 스케줄러에 의해 프로세스가 실행 상태가 되면 CPU를 할당받아 작업을 진행한다.

실행 상태에서 빠져나가는 두 가지 경우가 있다.

1. Preemption: Ready State로 돌아가는 경우로, 주어진 시간 동안 작업을 완료하지 못하면 타임 아웃이 발생하여 실행 상태의 프로세스는 작업을 중단하고 다시 준비 상태가 된다.
2. Block/Sleep: Asleep/Blocked State(중단 상태)로 이동하는 경우이다. 입/출력 등의 특정한 이벤트를 기다리게 된다.

---

## 4. Asleep/Blocked State(중단 상태)

![Asleep/Blocked State(중단 상태)](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FWOpkZ%2Fbtq8G5mipfz%2FPIE4V8XInh8pLO3DYvhXMK%2Fimg.png)

실행 중인 프로세스가 입출력을 요청(인쇄와 같은 요청)하는 경우에는 CPU 작업을 중단하고 입출력을 처리해야 한다. 따라서 CPU는 입출력 관리자에게 입출력을 요청하게 된다. 이 때 Block 명령이 발생하며 프로세스는 입출력이 끝날 때까지 중단 상태가 된다.

즉, 어떠한 이벤트로 인해 프로세스가 중단, 차단된 상태이며 프로세서 외 다른 자원을 기다리는 상태이다.

이때, 다시 자원을 할당 받더라도 중단 상태의 프로세스는 바로 실행 상태로 되는 것이 아니라 준비 상태로 된다. 이 과정은 Wake-up이라고 한다.

---

## 5. Suspended State

![Suspended State](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbcBHHp%2Fbtq8Hdxw677%2Fw0PalPkqnsflGTH63Pdrm0%2Fimg.png)

메모리를 할당받지 못한 상태로 대기 중단 상태와 일시 중단 상태가 있다.

1. 대기 중단 상태(Suspended Ready): 메모리 부족으로 일시 중단된 상태이다.
2. 일시 중단 상태(Suspended Blocked): 중단 상태에서 프로세스가 실행되려고 했지만 메모리 부족으로 일시 중단된 상태이다.

---

## 6. Terminated State(종료 상태)

![Terminated State](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlydoN%2Fbtq8Ht8j79n%2Fwk1HKBvBMVSUJBAQxBxFD0%2Fimg.png)

종료 상태는 메모리와 CPU 소유권을 모두 놓고 가는 상태이다. 이때, 메모리에 적재되었던 프로세스 정보와 프로세스 제어 블럭(PCB)가 제거된다.

커널 내 일부 PCB 정보를 남겨, 프로세스 관리를 위한 정보를 수집하는 과정도 거친다.

---

## 참고

[Process state, 프로세스의 상태](https://nostressdev.tistory.com/16)  
[PCB (Process Control Block) 이란?](https://nostressdev.tistory.com/15)  
[[OS] 커널(Kernel)이란](https://minkwon4.tistory.com/295)  
[[OS] 프로세스 상태](https://lotuslee.tistory.com/85)  
[[운영체제] CPU 스케줄러 - FCFS, SJF, SRT, RR, Priority Scheduling](https://hyunah030.tistory.com/4)  
[[OS 공부] CPU 스케줄러](https://dbstndi6316.tistory.com/181)

---

📅 2022-10-10
