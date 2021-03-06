# Computer structure
### 컴퓨터 하드웨어 구성
##### 중앙처리장치 (CPU) - 컴퓨터의 뇌
1. ALU(Arithmetic Logic Unit) - 연산을 담당하는 주체, 산술연산, 논리연산
2. 컨트롤 유닛(Control Unit) - CPU가 처리해야할 명령어를 해석, 그 결과에 따라 적절한 신호를 CPU의 다른 블록으로 보내는 역할을 함.
3. CPU 내부 다양한 register들
- 임시로 데이터를 저장하는 작은 메모리 공간
- 이들은 각각의 용도가 미리 정해져있고 매우 다양함.
- IR(Instruction Register): 이동된 명령어를 저장하기 위한 레지스터
- PC(Program Counter): 다음에 가져와야 할 명령어가 어디에 존재하는지 그 메모리 주소를 기억하기 위한 용도
4. 버스 인터페이스(Bus Interface)
- 버스가 어떻게 데이터를 전송하는지에 대한 프로토콜 또는 방식을 알고 있는 것
- CPU는 버스 인터페이스를 통해 I/O 버스에 데이터를 주고 받을 수 있다. 
5. 클럭신호 (Clock Pulse)
- 클럭이 초시계처럼 똑딱 거리는데 이 신호에 맞춰서 CPU가 동작함.
- 이렇게 클럭 신호에 맞추는 이유는 동기화 때문임.
##### 메인 메모리 (Main Memory)
- 램 (RAM) : 컴파일이 완료된 프로그램 코드가 올라가서 실행되는 영역, 프로그램 실행을 위해 존재하는 메모리
##### 입출력 버스 (I/O Bus)
- 컴퓨터를 구성하는 구성요소간 데이터를 주고 받는 통로
1. Address Bus: 주소 이동
2. Data Bus: 데이터 이동, 명령어나 피연산자
3. Control Bus: 컨트롤 신호 이동, CPU와 메모리가 서로 특별한 신호를 주고 받는 통로

### 프로그램의 기본 실행은 Fetch, Decode, Execution
- Fetch: 메모리상에 존재하는 명령어를 CPU로 가져오는 작업
- Decode: 가져온 명령어를 CPU가 해석함, 컨트롤 유닛
- Execution: 해석된 명령어를 CPU가 실행, 보통 ALU가 연산을 담당

##### [출처]
- (https://eunguru.tistory.com/59)