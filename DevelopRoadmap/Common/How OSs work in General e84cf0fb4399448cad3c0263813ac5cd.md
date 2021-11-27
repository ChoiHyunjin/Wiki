# How OSs work in General

OS란? 컴퓨터 하드웨어와 시스템을 관리하는 소프트웨어

## OS의 목적

- 높은 처리능력 : 단위 시간당 처리 갯수
- 낮은 응답 시간 : 작업 시작에서 결과물 출력까지 소요 시간
- 높은 사용 가능성 : 시스템을 얼마나 빨리 사용할 수 있는가
- 높은 신뢰도 : 작업을 얼마나 정확하게 처리하는가

## 구성요소

![Untitled](How%20OSs%20work%20in%20General%20e84cf0fb4399448cad3c0263813ac5cd/Untitled.png)

## System Call Interface

사용자가 OS Kernel에 요청할 수 있는 통로.

![Untitled](How%20OSs%20work%20in%20General%20e84cf0fb4399448cad3c0263813ac5cd/Untitled%201.png)

# OS Kernel

하드웨어와 소프트웨어간 interaction을 원활하게 하고, task 스케줄링을 수행함.

OS의 핵심 부분으로 가장 빈번하게 사용되는 기능들. 때문에 항상 메모리에 상주한다.

*동의어*

- 핵 = 관리자 프로그램 = 상주프로그램 = 제어 프로그램

# Driver

다른 하드웨어와의 interaction을 책임지고 있다.

# UI Compontent

Window, Button 등의 UI 표현을 책임지는 요소. 한쪽에서는 OpenGL, DirectX 등을 기반으로 하고, 다른 쪽은 WPF, Windows Forms, WinAPI 등을 기반으로 한다.

## 구분

### 동시 사용자 수

- 단일 : PC
- 다중 : 서버

### 동시 실행 프로세스 수

- 단일 : MS-DOS
- 다중 : Windows, Mac, Linux 등

### 작업 수행 방식

- 순차 처리 : 초기 형태의 컴퓨터에서 사용되던 방식으로 , 들어오는 일(Task)를 순차적으로 처리하는 방식. 다른 Task를 처리하기 위해서는 설정을 컴퓨터 바꿔줘야 했다.
- 일괄 처리 (Batch) : 매번 Task 마다 컴퓨터 설정을 바꾸기 번거로워서 같은 종류의 Task 를 묶어 일정 갯수 이상 되었을 때 한번에 처리하는 방식. 기계친화적인 방식으로 컴퓨터 설정을 자주 하지 않아도 된다. 반면에 일정 갯수가 되기를 기다려야되어 처리 속도는 떨어진다.
- 시분할 시스템 : Multi-Tasking을 위해 각 Task를 번갈아 가며 일정시간동안 수행하는 방법. 작업 처리 효율이 좋다. 하지만 스케쥴링이 필요한 시스템이다.
- PC : 단일 사용자를 위한 컴퓨터.
- 병렬 처리(Tightly-Coupled system) : 메모리, 저장소 등의 자원을 공유하면서 여러 프로세서를 사용하는 방식. 성능이 올라가고 신뢰성이 향상되지만, CPU들 관리가 필요해진다.
- 분산 처리(Loosely-Coupled system) : 병렬 처리 방식을 보다 크게 확장하기 위하여, 여러 컴퓨터를 네트워크를 이용하여 연결한 시스템. 각 컴퓨터(node)별로 os를 가지고, 이들은 중앙에서 운영하는 **분산운영체제**를 통해 관리된다.
- 실시간 : 실시간으로 처리하기 위한 시스템.
    - Hard RTOS : 발전소 제어, 무기 제어 등
    - Soft RTOS : 동영상 스트리밍 등

# 운영체제의 구조

## 단일 구조

![Untitled](How%20OSs%20work%20in%20General%20e84cf0fb4399448cad3c0263813ac5cd/Untitled%202.png)

커널 내 모듈간 직접 통신이 가능하다.

반먼에 커널이 너무 커진다. 그리고 악성코드 침입시 치명적이다.

## 계층 구조

![Untitled](How%20OSs%20work%20in%20General%20e84cf0fb4399448cad3c0263813ac5cd/Untitled%203.png)

사용자 영역(계층 5)와 커널 영역(계층 0~4)로 구성됨.

관리가 용이하지만, 커널간 통신을 위해 여러 계층을 거쳐야된다.

## 마이크로 커널 구조

![Untitled](How%20OSs%20work%20in%20General%20e84cf0fb4399448cad3c0263813ac5cd/Untitled%204.png)

커널 크기의 최소화를 위한 구조.

# 운영체제의 기능

## 프로세스 관리

### 프로세스

커널에 등록된 실행단위(= 실행 중인 프로그램)

## 프로세서 관리

- 프로세서 스케줄링
- 프로세스에 대한 프로세서 할당 관리

## 메모리 관리

- 주기억장치 = 메모리 = DRAM
- 메모리 할당 방법

## File Management

- 파일 : 논리적 데이터 저장 단위

## I/O Management

파일 입출력 관리

## ETC

- Disk
- Network
- Security