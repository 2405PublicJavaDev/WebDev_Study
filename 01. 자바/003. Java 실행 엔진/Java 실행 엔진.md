
- JVM 구성 요소중 하나이다.
	![[what-is-jvm-1.jpg]]
- 클래스로더에 의해 메모리에 적재된 클래스(바이트코드)들을 사용함
- 바이트코드를 기계어로 변경하여 명령어(instruction) 단위로 실행
- 바이트 코드를 운영체제에 맞게 해석해줌(각 운영체제/환경에 맞는 실행엔진이 존재)
- 실행 엔진이 바이트 코드를 명령어 단위로 읽어서 처리하는 2가지 방식
	- 인터프리터(Interpreter) - 한 줄씩 해석하고 기계어로 변경하며 실행
	- JIT(Just In Time) - 반복되는 바이트코드를 운영체제에 맞는 기계어로 바꿔놓고 실행
	                미리 컴파일하는 다른 언어에 비해 느리다는 단점을 해소하는데 도움
- JIT을 위한 컴파일 임계치
	- 코드 컴파일을 수행할 기준이다.
	- 컴파일 임계치를 만족하는 코드는 JIT 컴파일러에 의해 컴파일이 수행
	- JVM 옵션을 통하여 임계치 관련 설정이 가능하다.
		`-XX:CompileThreshold=100
		- CompileThreshold는 method entry counter 임계치를 뜻함
		  특정 메서드가 100번 호출 시 임계치 만족
		`-XX:OnStackReplacePercentage=33
		- OnStackReplacePercentage 값은 CompileThreshold에서 몇퍼센트를 넘기는지를 뜻함
		- CompileThreshold가 100일 때 특정 메서드 내 반복문이 33회 동작하면 임계치를 만족
	- JVM 옵션 사용 예시
		$ java -XX:CompileThreshold=100 -XX:OnStackReplacePercentage=33 src/test/java/blog/in/action/JitCompilerTest.java