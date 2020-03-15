---
title: "[운영체제] 1장 2장 요약"
date: 2018-12-29 20:29:20
categories:
    - 운영체제
tags: 
- 운영체제
- OS
---
학교에 가기전 전공과목에 대해서 공부하고 있다. 중요한 과목들은 다 파악이 됬고
훑어보는 목적으로 각 과목에 대해서 알아보고자 한다. 강의는 경성대 양희재 교수님의 강의로 주로 공부를 하며
도서관에서 빌린 <운영체제 : 그림으로 배우는 원리와 구조>라는 책을 읽고 모자란 설명을 채울 생각이다.

1장과 2장은 서론같은 느낌이라서 한 포스트에 요약하겠다.

1장. 컴퓨터 시스템 소개



1. 컴퓨터 시스템 구성요소

1) 프로세서
- 중앙처리장치(CPU) : 레지스터, 산술논리연산장치(ALU), 제어장치

2) 버스

3) 레지스터
- 프로그램 카운터(PC: Program Counter): 프로그램 수행을 제어하는 명령어 실행 순서 보관(다음에 실행할 명령어 주소 저장)
- 명령어 레지스터(IR: Instrument Register): 현재 수행하는 명령어를 저장, 명령어의 연산자 부분만 저장한다.
- 프로그램 상태 레지스터(PSR: Program Status Register): 플래그 같은 상태 정보 저장
- 메모리 주소 레지스터(MAR: Memory Address Register): 접근하려는 메모리의 주소 저장
- 메모리 버퍼 레지스터(MBR: Memory Buffer Register): 메모리에서 정보를 읽거나 저장할 때 사용

4) 메모리
- 메모리의 지역성: 실행 중인 프로세서가 실행기간 동안 메모리 정보를 균일하게 접근하지 않고 일부만 집중적을 참조
- 메모리 속도: 메모리 사이클 시간 & 메모리 접근 시간
- 가상 메모리(Virtual Memory): 보조기억장치에 저장했다가 주기억장치에 필요할 때 실행
- 메모리의 논리적 주소 -> 물리적 주소 : Memory Mapping (메모리 관리 장치 MMU: Memory Management Unit이 해줌)
- 캐시(Cache): 처리 속도가 빠른 프로세서와 상대적으로 느린 주기억장치 사이에서 데이터를 저장하여 속도 차이를 줄여줌. 주기억장치
에서 일정 블록의 데이터를 가져와 워드(Word) 단위로 프로세서에 전달
- 프로세서는 주기억장치 접근이 필요하면 먼저 캐시를 조사
- 캐시 태그와 원하는 메모리 주소 태그가 일치하면 캐시 적중(Cache Hit)라 한다 <-> 캐시 실패(Cache Miss)

5) 주변장치
- 키보드, 마우스 등

2. 컴퓨터 시스템의 동작

1) 명령어 구성
- 실행할 연산을 나타내는 연산 코드(Operation Code) & 처리할 데이터가 저장된 주소를 나타내는 오퍼랜드(Operand)로 이루어짐
- 직접 주소와 간접 주소

2) 인터럽트
- 인터럽트를 받은 프로그램은 실행을 멈추고 다른 프로그램을 실행한다
- 입출력장치가 새로운 입출력 연산을 수행하려고 하면 프로세서는 폴링(Polling)을 통해 각 장치의 상태 비트 검사 (폴링 방식)
- 인터럽트가 발생했을 때 프로세서에게 알려줌. 버스 중 인터럽트 요청 회선(IRQ: Interrupt Request Line)이 이런 용도로 쓰임(인터럽트 방식)




2장. 운영체제 소개


1. 운영체제의 기능

- 버퍼링과 스풀링 모두 입출력장치의 느린 속도를 보완하기 위해 이용하고 프로세서와 입출력장치를 항상 분주하게, 유휴 시간이 없게 만듬
- 버퍼링(Buffering): 주기억장치(보통 캐시메모리)를 버퍼로 사용
- 스풀링(Spooling): 보조기억장치(하드디스크)를 버퍼로 사용