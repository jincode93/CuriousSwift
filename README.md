# Attributes

## Attributes(속성)이란?
- Swift에는 선언에 적용하는 속성과 타입에 적용하는 속성이 있는데 선언 또는 타입에 추가적인 정보를 제공하는 것이라고 한다. 물론 이렇게만 말하면 바로 이해가 안될 수는 있으나 각 속성마다 필요한 곳이 다르고 사용법이 다르기 때문에 이렇게만 설명하고 뒤에 각각에 대해서 자세하게 공부하려고 한다.
- 속성을 지정하려면 @ 기호 다음에 속성의 이름과 속성이 허용하는 인수를 작성하면 된다.
- 특정 선언 속성은 속성에 대한 추가 정보와 특정 선언에 어떻게 적용하는지 지정하는 인수를 허용하는데 이러한 속성 인수 (attribute arguments)는 소괄호로 둘러싸이고 해당 형식이 속한 속성에 의해서 정의된다.

## 선언 속성 (Declaration Attributes)

- 선언 속성은 당연하게도 선언에만 적용할 수 있다.

### @available

- 이 속성을 적용하여 특정 Swift 언어 버전 또는 특정 플랫폼과 운영체제 버전과 관련된 선언의 라이프 사이클을 나타낸다고 하는데 쉽게 말해서 버전이나 운영체제에 따라서 선언을 사용가능하게 할지 말지를 정의할 수 있다.
- @available은 #available과 다르게 컴파일타임에 경고 또는 오류를 생성한다.
- 첫번째 인수는 플랫폼 또는 언어 이름 중 하나로 시작하거나 *을 사용하여 모든 플랫폼에 대해 적용할 수 있다.
- 나머지 인수는 순서에 상관없이 나타낼 수 있으며 중요한 이정표를 도입하여 선언의 라이프 사이클에 대한 추가 정보를 지정할 수 있다고 하는데 아래와 같다.
    - **unavailable** - 해당 인수는 선언이 지정된 플랫폼에서 사용 가능하지 않음을 나타낸다. 이 인수는 Swift 버전 가용성을 지정할 때 사용될 수는 없다고 하는데 보통 해당 인수는 버전 업데이트나 기능 추가시 기존에 사용하던 선언이 더이상 필요하지 않게 되었을 때 기존의 선언과 새로 생성될 선언을 혼동하지 않기 위해 사용하는 것 같다. - 영에게 질문해보기
    
    ```swift
    @available(*, unavailable)
    func loginButton() { }
    // => loginButton()은 모든 플랫폼에서 사용할 수 없음
    
    @available(*, unavailable, message: "사용불가능")
    func loginButton() { }
    // => loginButton()을 모든 플랫폼에서 사용하려고 하면 "사용불가능"이라는 오류메세지 발생
    
    @available(macOS, unavailable)
    func loginButton() { }
    // => loginButton()을 macOS에서는 사용할 수 없음
    ```
    
    - **introduced** - 해당 인수는 선언이 도입된 지정된 플랫폼 또는 언어의 첫번째 버전을 나타낸다. 즉, version number에 적힌 숫자 버전부터 적용이 됬다는 말이고 그래서 해당 버전 이후부터 사용이 가능하다. 다음과 같은 형식을 가진다.
    
    ```swift
    introduced: <#version number#>
    
    // 예)
    @available(iOS, introduced: 14.0)
    func loginButton() { }
    // => loginButton()은 iOS 14.0 버전에서 도입되서 14.0 이상에서만 사용 가능 (이하에서 사용시 오류 발생)
    ```
    
    - **deprecated** - 해당 인수는 선언이 사용되지 않는 지정된 플랫폼 또는 언어의 첫번째 버전을 나타낸다. 즉, version number에 적힌 숫자 버전보다 이하일 경우 ‘경고’를 발생시킨다. 다음과 같은 형식을 가진다.
    
    ```swift
    deprecated: <#version number#>
    
    // 예)
    @available(iOS, deprecated: 12.0)
    func loginButton() { }
    // => loginButton()은 iOS 12.0 버전보다 이하의 버전에서 사용 시 '경고'가 발생
    ```
    
    - **obsoleted** - 해당 인수는 선언이 폐기된 지정된 플랫폼 또는 언어의 첫번째 버전을 나타낸다. 선언이 폐기되면 지정된 플랫폼 또는 언어에서 제거되고 더이상 사용이 불가능하다.
    
    ```swift
    obsoleted: <#version number#>
    
    // 예)
    @available(iOS, obsoleted: 13.0)
    func loginButton() { }
    // => loginButton()은 iOS 13.0 버전보다 이하의 버전에서 사용 시 '오류'가 발생
    ```
    
    - **message** - 해당 인수는 지원 중단되거나 폐기된 선언 사용에 대한 경고나 에러를 생성할 때 컴파일러가 표시하는 텍스트 메세지를 제공한다.
    
    ```swift
    message: <#message#>
    
    // 예)
    @available(iOS, deprecated: 12.0, message: "사용불가")
    func loginButton() { }
    // => loginButton()은 iOS 12.0 버전보다 이하의 버전에서 사용 시 "사용불가"라는 메세지의 '경고'가 발생
    ```
    
    - **renamed** - 해당 인수는 이름이 변경된 새로운 선언을 나타내는 텍스트 메세지를 제공한다. 컴파일러는 이름이 변경된 선언 사용에 대한 에러를 생성할 때 새로운 이름을 표시한다.
    
    ```swift
    renamed: <#new name#>
    @available(iOS, deprecated: 12.0, renamed: "signInButton")
    func loginButton() { }
    // => loginButton()은 iOS 12.0 버전보다 이하의 버전에서 사용 시 "signInButton"으로 바꼈다는 '경고'가 발생
    ```
    

- #available과 차이점
    - #available은 여러 플랫폼에서 서로 다른 논리 처리를 결정하기 위해 if 또는 guard문과 함께 사용하는데 Bool 값을 반환하는 런타임 검사라고 한다.
    - 가장 큰 차이점은 @available은 컴파일타임에 동작, #available은 런타임에 동작이라는 점이다.
    
    ```swift
    if #available(iOS 11.0, *) {
    	// do something.
    } else {
    	// do something.
    }
    // => 이렇게 특정 플랫폼의 버전에 따라서 분기처리를 할 수 있다.
    ```
    

### @frozen

- 타입에 변경사항을 제한하기 위해 구조체 또는 열거형 선언에 이 속성을 적용한다고 한다.
- 어쩌다 한번씩 보여서 공부를 진행했는데 enum에 적용하게 되면 더 이상 case가 추가 되지 않는다는 것을 명시해주기 때문에 컴파일 속도가 빨라진다고 한다. 다만 swift에서는 library evolution mode만 제외하고는 암시적으로 enum에 @frozen이 붙어있기 때문에 굳이 사용해줄 필요는 없고 objective-c의 enum의 경우 @frozen을 사용하지 않으면 switch 구문을 작성할 때 컴파일 에러가 발생하게 되고 이럴때는 enum에 @frozen을 붙여주거나 switch 문에 @unkown default를 추가해주면 에러가 해결되고 @frozen을 붙여주는 것이 컴파일에는 더 효율적이라고 한다.

### @main

- 프로그램 흐름에 대해 취상위 시작 지점을 포함하는 것을 나타내기 위해 구조체, 클래스 또는 열거형 선언에 이 속성을 적용할 수 있는데 타입은 인수가 없고 Void를 반환하는 main 타입 함수를 제공해야한다고 한다.


# Concurrency

## 동시성 프로그래밍이란?

- 동시성 프로그래밍이란 서로 다른 Task를 Main Threads가 아닌 다른 Threads에서 분산처리할 수 있도록 해주는 것
- iOS에서는 Task를 Queue로 보내기만 하면 OS가 알아서 다른 Threads에서 작업함 → 직접적으로 Threads를 관리하는 개념이 아닌 Queue를 이용해서 작업을 분산처리하고 OS에서 알아서 Threads 숫자를 관리

## 동시성 프로그래밍을 하는 이유

- 작업 시간이 긴 여러가지 Task를 하나의 Threads에서 처리하려면 과부하가 생기기 때문에 Main Threads 외의 Threads를 이용해서 분산해줌으로써 부하를 나누기 위해 사용 (네트워크 통신 등)
- 성능, 반응성, 최적화와 관련 버벅거리는 것을 해결하기 위해 사용

## 병렬(Parallel) VS 동시성(Concurrency)

- 물리적은 Threads와 소프트웨어적인 Threads의 개념은 서로 다름
- 병렬은 물리적인 Threads에서 실제 동시에 일을 하는 개념 - 내부적으로 알아서 동작함
- 동시성은 Main Threads가 아닌 다른 소프트웨어적인 Threads에서 동시에 일을 하는 개념

## 직렬(Serial) vs 동시(Concurrent)의 개념

- 직렬큐는 분산처리 시킨 Task를 한개의 Threads로만 보내서 Task를 처리

```swift
let serialQueue = DispatchQueue(label: “문자열”)

serialQueue.async {
	task1()
}

serialQueue.async {
	task2()
}

// => 직렬큐이기 때문에 순서대로 실행
```

- 동시큐는 분산처리 시킨 Task를 여러개의 Threads로 보내서 Task를 처리

```swift
let concurrentQueue = DispatchQueue.global()

serialQueue.async {
	task1()
}

serialQueue.async {
	task2()
}

// => 동시큐이기 때문에 순서가 랜덤하게 실행
```

- 직렬큐는 순서가 중요한 작업을 처리할 때 사용하고 동시큐는 각자 독립적이지만 유사한 여러개의 작업을 처리할 때 사용

## DispatchQueue(GCD)의 종류

- (글로벌) 메인큐 - DispatchQueue.main : Main Threads를 사용, 직렬큐
- 글로벌큐 - DispatchQueue.global() : 종류가 여러개, 기본 설정은 동시큐 - 여러개의 Threads 사용, QoS(큐의 서비스 품질)에 따라 Threads의 수를 조절해서 중요한 Task일수록 작업을 빨리 끝내도록 해줄 수 있음(다만 많은 Threads를 쓴다고 꼭 먼저 끝나는건 아님)
- 프라이빗(Custom)큐 - DispatchQueue(label: “…”) : 커스텀으로 만드는 큐, 기본적으로는 직렬큐 - 하나의 Threads 사용

## GCD 주의사항

- 화면(UI)와 관련된 Task는 반드시 Main Threads로 보내줘야됨

## 비동기(Async)처리란?

- Task를 다른 Threads에서 하도록 시킨 후, Task가 끝날 때 까지 기다리지 않고 다음 Task를 진행하는 것

## 동기(Sync)처리란?

- Task를 다른 Threads에서 하도록 시킨 후, Task가 끝날 때 까지 기다렸다가 다음 Task를 진행하는 것
- 동시성 프로그래밍에서는 동기 처리를 거의 따로 해줄 필요는 없음

## 올바른 비동기함수 설계

- 다른 Threads에서 task를 처리할 때 task를 시작하고 바로 return을 보내기 때문에 return을 반환하는 함수 형태로 사용하면 안됨(항상 return이 nil로 나올 가능성이 높음) → 올바른 콜백함수를 만들기 위해서 completion handler를 사용할 수 있음
- 객체 내에서 비동기코드를 사용할 때 self 키워드에 접근해야 할 때 캡처리스트 안에서 weak self로 선언하지 않으면 강한 참조가 발생하는데 이때 서로를 가리키는 경우라면 retain cycle(참조 사이클)이 발생해 메모리 누수가 발생할 수 있고 그렇지 않더라도 클로저의 수명주기가 길어지는 현상이 발생할 수 있음 → 캡처리스트 안에서 weak self로 선언하는 것을 권장 (약한 참조)
- 다만 무분별한 weak self 사용은 런타임 오버헤드를 발생시킬 수 있고, nil을 체크해야되는 귀찮음을 발생

## Async / await

- @escaping 클로저를 활용한 비동기처리를 할 때 여러개의 비동기 함수를 이어서 사용해야할 때 클로저가 계속 늘어나서 보기 어려워짐(무한 들여쓰기, 콜백 지옥)
- completion handler가 없어도 함수 자체를 return방식으로 설계를 하면서도 비동기 처리를 할 수 있게 됨 → 코드처리가 깔끔해짐
- async : 비동기 함수임을 나타내는 키워드
- await : async 키워드가 표시된 메소드나 함수의 리턴을 기다리게 하는 키워드
- async 함수는 비동기적으로 동작할 수 있고, await 키워드를 사용해 비동기 함수의 결과를 대기할 수 있음
- Sync에서의 Threads 관리
    - 호출 - A함수에서 B함수(sync)를 호출하면, A함수가 실행되던 Threads의 제어권을 B에게 전달
    - 진행 - B함수가 끝날때까지 해당 Threads는 점유되어 다른 일을 수행하지 않게 됨
    - 종료 - B함수가 종료되면 A함수에게 다시 Threads 제어권을 반납
- Async에서의 Threads 관리
    - 호출 - A함수에서 B함수(async)를 호출하면, A함수가 Threads의 제어권을 B에게 전달
    - 진행 - B함수는 Async이기 때문에 Threads의 제어권을 포기하는 suspend가 가능(suspend되면 호출한 A함수도 같이 suspend됨)
    - suspend - Threads에 대한 제어권은 system으로 가고 시스템은 Threads를 사용하여 다른 작업을 수행
    - resume - 일시 중단된 비동기 함수 B를 다시 실행하는 단계
    - 종료 - B함수가 종료되면 A함수에게 Threads 제어권을 반납


# Generic

## Generic이란?

- 제네릭이란 타입에 의존하지 않는 범용 코드를 작성할 때 사용하는 것
- 제네릭으로 구현한 기능과 타입은 재사용하기 쉽고, 코드의 중복을 줄일 수 있음
- 만약 제네릭의 개념이 없다면 타입에 따른 함수를 모든 경우마다 다시 정의해야하는 불편함이 있음
- 애플에서는 Swift에서 가장 강력한 기능 중 하나로 제네릭을 말하며 Swift 표준 라이브러리의 대다수가 제네릭으로 선언되있다고 함

## 제네릭 함수

- 제네릭 함수가 없다면?!

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
	let tempA = a
	a = b
	b = tempA
}

func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
	let tempA = a
	a = b
	b = tempA
}

func swapTwoString(_ a: inout String, _ b: inout String) {
	let tempA = a
	a = b
	b = tempA
}

// => 위 3개의 코드 모두 a, b를 swap 해주는 동일한 동작의 코드이지만 타입이 다르기 때문에 하나하나 정의해줘야한다.
```

- 제네릭 함수를 사용한다면?!

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {    // 플레이스홀더 역할(표시 역할) < > 안의 타입과 파라미터의 타입은 같아야함
	let tempA = a
	a = b
	b = tempA
}

// => 타입 파라미터<T>는 함수 내부에서 파라미터의 타입이나 리턴형으로 사용
// => 이렇게 단 하나의 함수로 위의 3개의 함수뿐만 아니라 동일한 기능이 필요한 모든 타입에서 사용이 가능

var string1 = "hello"
var string2 = "world"

var double1 = 0.33
var double2 = 0.77

swapTwoValues(&string1, &string2)
swapTwoValues(&double1, &double2)

// => 위의 제네릭 함수에서 두 파라미터의 타입이 같기 때문에 사용할 때도 두 아규먼트의 타입이 같다면 어떤 타입도 상관없음
// => 함수 파라미터에 inout은 입출력 파라미터를 위해 사용, 호출할 때는 &를 아규먼트 앞에 붙여서 사용
```

## 제네릭 타입

- 생각보다 우리는 Swift에서 많은 제네릭 타입을 접하게 된다. (배열, 딕셔너리, 옵셔널, 고차함수 등등)

```swift
let arr: Array<String> = ["apple", "academy"] // => let arr: [String] = ["apple", "academy"]
let dic: Dictionary<String, Int> = ["zerom": 1, "toughie": 2] // => let dic: [String: Int] = ["zerom": 1, "toughie": 2]
let optionalType: Optional<String> // => let optionalType: String?
```

- 제네릭 타입(클래스, 구조체, 열거형)의 작성

```swift
class SomeColor<T> {
	var red: T
	var green: T
	var blue: T
}

extension SomeColor {
	func getColor() -> T {
		// 동작 코드
	}
}
```

## 제네릭의 타입 제약

- 프로토콜 제약 → 타입 파라미터에 프로토콜을 적용시킬수도 있음

```swift
func findIndex<T: Equatable>(item: T, array: [T]) -> Int? {
	for (index, value) in array.enumerated() {
		if item == value {
			return index
		}
	}
	return nil
}
```
