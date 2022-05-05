# Clean Architecture
## Chapter1 소개
### 1장 설계와 아키텍처란?
> * 설계(design)은 저레벨 세부적인 것이고 아키텍처(Architecture)은 고레벨이지만 잘 설계된 아키텍처는 설계를 포함해 소프트웨어의 기능과 구성요소를 보여준다
> * 아키텍처가 잘된 소프트웨어는 적은 인력으로 쉽게 소프트웨어를 추가, 변경하도록 만든다.
### 2장 두 가지 가치에 대한 이야기
> * 소트프웨어에서 행위와 아키텍처 중에서 아키텍처가 더 중요하다. 왜냐하면 동작은 하지만 아키텍처가 엉망이면 추가, 변경이 어려워 나중엔 동작도 힘들게 되지만 잘 짜여진 아키텍처에선 동작하게 만들면 되기 때문이다.
## Chapter2 벽돌부터 시작하기 : 프로그래밍 패러다임
### 3장 패러다임 개요
> * 적용 순서
> 1. 구조적 프로그래밍 : **제어흐름의 직접적인 전환에 대한 규칙 부과**로 goto문을 금지시 시키고 제어흐름을 코드의 위에서 아래로 흐르게 만들었다. 
> 2. 객체 지향 프로그래밍 : 구조적 프로그래밍보다 먼저 나왔다. 요한 달(Ole Johan Dahl)과 크리스텐 니가드(Kristen Nygaard)에 의해 등장했으며 **함수 호출 스택 프레임을 힙으로 옮기면서 함수가 클래스의 생성자가 되고 지역 변수는 인스턴스 변수, 그리고 중첩 함수는 메서드가 되도록 만들었다. 함수 포인터를 특정 규칙에 따라 사용하는 과정을 통해 다형성이 등장했다.** **제어흐름의 간접적인 전환에 대해 규칙을 부과**한다.
> 3. 함수형 프로그래밍 : 가장 먼저 만들어진 패러다임. 알론조 처치(Alonzo Church)의 람다 계산법이 함수형 프로그래밍에 직접적인 영향을 줬다. 람다 계산법은 불변성으로 심볼의 값이 변경되지 않는다는 개념이다. 즉, 함수형 언어에는 할당문이 없고 변수 값을 변경할 수 있는 방법이 까다롭다. 따라서, **함수형 프로그래밍은 할당문에 대해 규칙을 부과**한다.
> * 정리 : goto문, 함수 포인터, 할당문을 못 쓰게 만들었다.
> * 결론 : 아키텍처 경계를 넘나들기 위한 메커니즘으로 다형성을 이용한다. 함수형 프로그래밍을 이용하여 데이터의 위치와 접근 방법에 대해 규칙을 부과한다. 모듈의 기반 알고리즘으로 구조적 프로그래밍을 사용한다.
### 4장 구조적 프로그래밍
> * 계층구조로 프로그래밍을 작성하여 유클리드 계층처럼 분할 * 정복의 재귀적 방법으로 모듈을 작게 만드는 것이 목적이다. 이 과정 중에 goto문은 직접적인 제어흐름에 방해가 되기 때문에 금지시 된다.
### 5장 객체 프로그래밍
> * 캡슐화 : 데이터를 은닉시켜 오용, 남용 방지
> * 상속 : 어떤 변수와 함수를 하나의 유효 범위로 묶어서 재정의하는 일
> * 다형성 : 함수 포인터를 이용한 재정의 방식. 포인터 함수 정의 파일(드라이브 파일)과 포인터 함수 선언 파일을 인터페이스로 사용(플러그인)
```c
struct FILE{
 void (*open)(char* name, int mode);
 void (*close)();
 int (*read)();
 void (*write)(char);
 void (*seek)(long index, int mode);
};
```
```c
 void open(char* name, int mode) {/*...*/}
 void close() {/*...*/};
 int read() {int c;/*...*/ return c;}
 void write(char c) {/*...*/}
 void seek(long index, int mode) {/*...*/}
 
 struct FILE console = {open, close, read, write, seek};
```
> * 의존성 역전 : 제어흐름과 의존관계의 흐름이 반대가 되는 것이 의존성 역전이다. 의존이란 해당 컴포넌트를 이용(포함)할 때 의존한다고 표현하고 제어흐름은 실제로 실행되는 컴포넌트이다. 이것은 모듈에 대해 독립성을 보장할 수 있다.

### 6장 함수형 프로그래밍

> * 불변성과 아키텍처 : race condition, deadlock, concurrent update문제가 모두 가변 변수로 발생하는데 함수형 프로그래밍은 변수가 변경되지 않는다.
> * 가변성의 분리: 가변 컴포넌트와 트랜잭션 메모리로 분리

## Chapter3 설계 원칙
### 7장 SRP: 단일 책임 원칙

> * 하나의 모듈은 하나의 사용자(액터)만을 책임져야한다. 즉, 하나의 모듈은 단 하나의 사용자 혹은 그룹만이 그 모듈을 정의할 수 있다. 
> * 해결
> > * 만일 하나의 모듈에 많은 액터가 사용한다면 모듈안의 매서드들을 분리시켜 액터가 서로 다른 클래스들에 넣고 이용할 수 있게 만들어야 한다.
> > * 즉, 새로운 클래스들을 만들어 액터들이 사용하는 매서드들을 각기 다른 클래스에 넣는다. 그리고 새로운 클래스를 하나 더 만들어 이전에 만든 클래스들을 인스턴스로 갖게 만들고 asad 패턴을 통해 인스턴스들을 초기화 시킬 수 있다.
