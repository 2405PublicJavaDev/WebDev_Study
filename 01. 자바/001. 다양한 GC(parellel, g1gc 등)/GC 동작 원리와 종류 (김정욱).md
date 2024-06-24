# Garbage Collection(GC)
- 프로그램 실행 중 필요없어진 객체를 짬처리 해준다
- 정해진 룰에 따라 자동으로 치워주기 때문에 개발자가 편해짐
- JAVA의 장점으로 자꾸 어필하는데 C#, 파이썬, 자바스크립트, GO 등등 최신 언어에도 많이 존재

## GC의 단점
GC가 동작하는 동안에는 애플리케이션에서 GC 관련 스레드를 제외한 모든 스레드가 잠깐 멈추는 STW(Stop The World) 현상이 있다.
이와 관련된 튜닝으로 여러가지가 있는데 저번에 알아봤던 heap 크기 설정 -Xms, -Xmx은 GC의 동작에 간접적인 영향을 준다.
- heap이 작으면 GC가 짧게짧게 동작하는 대신 호출 횟수가 잦아지고
- heap이 크면 GC 실행이 적어지는 대신 동작 시간이 길어진다.

## GC가 처리하는 대상을 구분하는 기준
인스턴스가 참조되고 있는지의 여부를 판별하여 삭제 한다!
Heap area에 존재하는 객체의 메모리 주소를 가지고 있는 참조 변수가 없어진다면 Unrechable객체로 판별해버림

## GC의 동작 매커니즘
기본적인 동작 방식으로 Mark and Sweep 이 있다
#### Mark 동작
Heap의 Root Space로부터 그래프 순회(알고리즘에 따라 각 객체가 참조하는 모든 객체를 따라가며 마킹, 알고리즘 종류에 따라 GC의 종류가 나뉨)
#### Sweep 동작
Heap 메모리를 선형적으로 검사하면서 마크되지 않은 객체를 메모리에서 제거

## GC의 종류
#### Serial GC
- 서버의 CPU 코어가 1개이던 시절의 가장 단순한 GC
- GC 처리 스레드가 1개라 가잔 STW 시간이 길다
- 코어가 여러개면 굳이 쓸 필요가 없다
#### Parallel GC
- Minor GC(Heap의 Young 영역 대상 GC)을 멀티 스레드(병렬처리)로 수행
- Old 영역은 그대로 싱글 스레드
- Serial GC에 비해 STW 시간 감소
#### Parallel Old GC
- Old 영역도 멀티 스레드로 치운다!
- 청소방식으로 Mark-Summary-Compact 방식 채택
- Summary 단계에서 메모리의 사용 상태 파악
- Compat 단계에서 연속된 메모리 공간으로 압축(디스크 조각모음 같은거)
#### CMS GC (Concurrent Mark Sweep)
- GC 실행중에도 어플리케이션의 스레드가 일부 같이 실행되어 STW를 줄이기 위해 고안됨
- 단점으로  GC과정의 복잡함, 그로인한 CPU 사용량 증가, 메모리 파편화 등이 있다.
- 단점 때문에 Java9 버전부터 deprecated(없어지거나 대체 예정) 되었고 Java14에서 사용 중지
#### G1 GC(Garbage First)
- CMS GC 대체를 위해 jdk 7버전 부터 release된 GC(이후 Java 9+ 버전 디폴트 GC가 됨)
- 힙 메모리 4GB 이상, STW 시간이 0.5초 정도 필요한 상황에 쓴다고 함(Heap이 너무 작으면 지양)
- 기존 GC에서 Young/Old 물리적으로 고정 이었던 것을 동적으로 부여
- 쪼개진 Heap 영역의 Garbage비율에 따라 GC 우선순위가 정해진다
- 기존 GC에서는 Eden->Survivor0->Survivor1->Old로 순차 이동하던것에서 효율성에 따라 뒤로 보내기도 함
#### Shenandoah GC
- Java 12 release
- Redhat에서 개발함
- CMS의 단편화, G1이 가진 pause 이슈 해결
#### ZGC (Z Garbage Collector)
- Java 15 release
- 대량의 메모리(8MB ~ 16TB)를 low-latency로 잘 처리하기 위해 디자인된 GC라고 함
- G1에 고정크기로 쓰던 Region말고 동적 크기인 ZPage 라는 영역을 사용(객체의 크기에 따라)
- 힙 크기가 증가하더라도 STW 시간이 절대 10ms를 넘지 않는게 가장 큰 장점