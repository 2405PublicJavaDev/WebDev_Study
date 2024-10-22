# 자바 메모리관리 (JVM의 Heap 크기 튜닝)
## 힙(Heap) 영역

- **목적**: 동적으로 생성된 객체와 배열이 저장되는 영역.
- **특징**: 가비지 컬렉터(Garbage Collector)에 의해 관리되며, 메모리 부족 시 GC가 실행되어 사용하지 않는 객체를 제거.
- **설정**: `-Xms`(초기 힙 크기)와 `-Xmx`(최대 힙 크기)로 힙 메모리의 크기를 설정할 수 있음.

## 힙 영역 튜닝
### `-Xms`

- **의미**: JVM이 시작할 때 할당할 힙 메모리의 초기 크기입니다.
- **기본값**: 특정 JVM 구현에 따라 다르며, 일반적으로 1/64의 물리적 메모리 또는 1MB입니다.
- **설정 예시**: `-Xms512m`는 JVM이 시작될 때 512MB의 힙 메모리를 할당하도록 합니다.
- **용도**: 애플리케이션 시작 시점부터 많은 메모리를 필요로 하는 경우, 초기 메모리 부족으로 인한 성능 저하를 방지하기 위해 설정합니다.

### `-Xmx`

- **의미**: JVM이 실행 중에 사용할 수 있는 힙 메모리의 최대 크기입니다.
- **기본값**: 특정 JVM 구현에 따라 다르며, 일반적으로 물리적 메모리의 절반 또는 1GB입니다.
- **설정 예시**: `-Xmx1024m`는 JVM이 최대 1024MB의 힙 메모리를 사용할 수 있도록 합니다.
- **용도**: 애플리케이션이 사용할 수 있는 최대 메모리 양을 제한하여 시스템의 다른 프로세스와의 메모리 경합을 방지합니다. 과도한 메모리 사용으로 인한 OutOfMemoryError를 예방하는 데 도움이 됩니다.

### 설정 예시

sh : `java -Xms512m -Xmx1024m -jar MyApplication.jar`

위 명령은 `MyApplication.jar` 애플리케이션을 JVM이 시작될 때 512MB의 힙 메모리를 할당하고, 실행 중에는 최대 1024MB의 힙 메모리를 사용할 수 있도록 설정하여 실행한다.

### 중요한 점

- **적절한 설정 필요**: `-Xms`와 `-Xmx` 값은 애플리케이션의 메모리 요구 사항과 시스템의 가용 메모리에 따라 적절하게 설정해야 한다. 너무 작게 설정하면 메모리 부족으로 성능 저하나 애플리케이션 충돌이 발생할 수 있고, 너무 크게 설정하면 시스템의 다른 프로세스에 악영향을 미칠 수 있다.
- **GC(가비지 컬렉션) 영향**: 힙 메모리 크기는 가비지 컬렉션의 빈도와 성능에 영향을 미친다. 큰 힙 메모리는 가비지 컬렉션의 빈도를 줄일 수 있지만, 가비지 컬렉션이 실행될 때 더 많은 시간을 소요할 수 있다.

이러한 이유로, 애플리케이션의 성능 요구 사항과 실행 환경을 고려하여 적절한 `-Xms`와 `-Xmx` 값을 설정하는 것이 중요함






# 주요 JVM 메모리 튜닝 옵션
자료를 검색하면서 발견한 주요 JVM 메모리 튜닝 옵션들

### 힙 메모리 설정

1. **-Xms**: 초기 힙 메모리 크기
    - 예: `-Xms512m` (512MB로 설정)
2. **-Xmx**: 최대 힙 메모리 크기
    - 예: `-Xmx1024m` (1024MB로 설정)

### 스택 메모리 설정

3. **-Xss**: 각 스레드의 스택 크기
    - 예: `-Xss1m` (각 스레드의 스택 크기를 1MB로 설정)

### 메타스페이스 설정 (Java 8 이상)

4. **-XX**: 초기 메타스페이스 크기
    - 예: `-XX:MetaspaceSize=128m` (128MB로 설정)
5. **-XX**: 최대 메타스페이스 크기
    - 예: `-XX:MaxMetaspaceSize=256m` (256MB로 설정)

### 가비지 컬렉션 튜닝

6. **-XX:+UseSerialGC**: Serial 가비지 컬렉션 사용
    - 예: `-XX:+UseSerialGC`
7. **-XX:+UseParallelGC**: Parallel 가비지 컬렉션 사용 (또는 -XX:+UseParallelOldGC)
    - 예: `-XX:+UseParallelGC`
8. **-XX:+UseConcMarkSweepGC**: Concurrent Mark-Sweep (CMS) 가비지 컬렉션 사용
    - 예: `-XX:+UseConcMarkSweepGC`
9. **-XX:+UseG1GC**: Garbage First (G1) 가비지 컬렉션 사용 (Java 9 이상 기본)
    - 예: `-XX:+UseG1GC`
10. **-XX**: GC와 애플리케이션 실행 시간 비율 설정
    - 예: `-XX:GCTimeRatio=19` (애플리케이션 실행 시간의 5%를 GC에 할당)
    - Ratio값 공식 : GCTimeRatio = (처리율 목표 ) / (1 - 처리율 목표)  //보통 19(GC 5%)를 사용
1. **-XX**: 최대 GC 일시 중지 시간 설정 (높으면 GC의 처리량 증가, 낮으면 지연시간 최소화)
    - 예: `-XX:MaxGCPauseMillis=200` (최대 200ms)

### 기타 메모리 설정

12. **-XX**: 초기 Young Generation 크기 설정
    - 예: `-XX:NewSize=256m` (256MB로 설정)
13. **-XX**: 최대 Young Generation 크기 설정
    - 예: `-XX:MaxNewSize=512m` (512MB로 설정)
14. **-XX**: Eden 공간과 Survivor 공간의 비율 설정
    - 예: `-XX:SurvivorRatio=8` (Eden = 8:1)
