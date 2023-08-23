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
