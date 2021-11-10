#  Swift - Simple Cheat Sheet

Swift5를 기반으로 작성된 Swift Cheat Sheet 입니다. 본인이 성장하며 모르는 내용들을 하나씩 적어놨기 때문에 약간 내용이 파편화되어있고 당연히 모든 내용이 들어있지 않습니다. 



## Use

- Markdown으로 작성됐습니다. star 후 찾아 보시거나 download후 찾아보시면 됩니다.
- 검색으로 궁금한 키워드를 입력해봐서 지금 해결가능한 문법이 있나 찾아봅니다 `(ex. reduce, 자르기, index)`
- 해당 레포지토리만 보고 학습한다기 보다는 문법이 알쏭달쏭할 때 Cheat Sheet를 찾아보시고 꼭 [Apple Documentation](https://developer.apple.com/documentation/technologies)을 참고해주세요



## 같이 보면 좋을 Roadmap

[mobile-developer-roadmap](https://github.com/godrm/mobile-developer-roadmap)



## 목차

- Basic Operator
- Primitive Types
    - String
    - Int
    - CharacterSet
    - Float
    - Double
    - Bool
    - ASCII
    - etc.
- Collections
    - Array
    - Dictionary
    - Set
    - etc.
- Control Flow
    - for
    - if
    - guard
    - forEach
    - Switch
    - while, do-while
    - Checking API Availability
    - Error Handle
- Function
- Closure

정리되는데로 추가됩니다.



# Basic operators

### 연산자  계산 순서

| 구분        | 연산자     | 우선순위 |
| ----------- | ---------- | -------- |
| 단항 연산자 | +. -, !. ~ | 1        |
| 산술 연산자 | *, /, %    | 2        |
| 산술 연산자 | +, -       | 3        |



### Swift Math : abs, sqrt, pow, floor, ceil

```swift
abs(-9) // 9 절대값
sqrt(9) // 3 루트
// 참고로 sqrt는 func squrt(_: Double) -> Double
pow(3, 2) // 9 제곱근
floor(9.1) // 9 소수점 버림
ceil(9.1) // 10 소수점 올림
```



# Primitive Types





## String



- swift는 String을 만드는 자체가 비교적 느리다

- String끼리 더하는 것보다 `String Interpolation`이 더 효율적이다



### Array to String

```swift
print(String(["a", "b", "d"]))
// "abd"
```



### 앞뒤 공백 제거

```swift
.trimmingCharacters(in: .whitespaces)
```





### String에서 Int 추출하기

```swift
extension Int {
    static func parse(from string: String) -> Int? {
        return Int(string.components(separatedBy: CharacterSet.decimalDigits.inverted).joined())
    }
}
```





### Int를 해당 진법으로 변환 - init(_:radix:)

- 해당 진법으로 변환(반환도 `String`)

    ```swift
    print(String(2, radix: 2))
    // "01"
    ```



### String 첫번째 element가 같은지 비교 - starts(with:)

```swift
let a = 1...3
let b = 1...10

print(b.starts(with: a))
// Prints "true"
```



### String - format

```swift
// 세 자리까지 표현
print(String(format: "%.3d", Int.random(in: 0...1)))

// 001
또는 
// 000

// 소수 첫째자리까지 표현
// %.2d 는 정수 두자리까지
String(format: "%.1f", (degree * 9 / 5) + 32)
```



### String - describing

describing은 강제성과 표현하려는 성질이 있다

```swift
struct Foo {
    
}

print(String(describing: Foo.self))
//Foo
```



#### Coercion, Representation

```swift
struct Person {
    var first: String
    var last: String
}

let p = Person(first:"Matt", last:"Neuburg")
print("p is \(String(describing: p))")

// p is Person(first: "Matt", last: "Neuburg")
```





### String을 Int로 씌우기

문자열을 Int로 형변환 하려해도 오류가 나지 않는다(강제로 하지 않는 이상)

```swift
var a = 1
var b = "hoho"
var num = Int(b)

print(num)

// nil
```



### Components vs split 



```swift
var str = "OOOXX"
print(str.components(separatedBy: "X"))
print(str.split(separatedBy: "X"))

// ["OOO", "", ""]
// ["OOO"]
```



### String indexing

```swift
var str = "abc"
var index = str.index(str.startIndex, offsetBy: 0)
print(str[index])
// "a"
```



### string 문자 제거

```swift
var compareStr = "abcde"
if let index = compareStr.firstIndex(of: "c") {
    compareStr.remove(at: index)
}

print(compareStr)

//abde
```



### String index를 아예 extension으로 구현해버리기



```swift
extension String {
    func index(from: Int) -> Index {
        return self.index(startIndex, offsetBy: from)
    }

    func substring(from: Int) -> String {
        let fromIndex = index(from: from)
        return String(self[fromIndex...])
    }

    func substring(to: Int) -> String {
        let toIndex = index(from: to)
        return String(self[..<toIndex])
    }

    func substring(with r: Range<Int>) -> String {
        let startIndex = index(from: r.lowerBound)
        let endIndex = index(from: r.upperBound)
        return String(self[startIndex..<endIndex])
    }
}

let str = "Hello, playground"
print(str.substring(from: 7))         // playground
print(str.substring(to: 5))           // Hello
print(str.substring(with: 7..<11))
```

[출처](https://stackoverflow.com/questions/39677330/how-does-string-substring-work-in-swift)



### 문자열의 인덱싱 - Array와 다르다........... String.index

```swift
let str = "abcde"
print(str[0]) 
// 'subscript(_:)' is unavailable: cannot subscript String with an Int, use a String.Index instead.
// Array와 같다고 생각하면 오산

print(str[str.startIndex])
// "a"
// 이렇게 해야한다

print(str[str.endIndex])
// String index is out of bounds
// String.index는 5를 반환한다

let endIndex = str.index(before: str.endIndex) // before - 1, after + 1
print(str[endIndex])
// 이렇게 해야 마지막 인덱스를 설정 가능하다

let startIndex = str.index(str.startIndex, offsetBy: 2) // 세 번째 숫자
print(str[startIndex])
// 중간에 있는 문자 인덱싱은 이렇게
```

[reference](http://seorenn.blogspot.com/2018/05/swift-string-index.html)



### String을 Array로 변환 시 주의사항

```swift
let arrayofstring = Array("Hello") // 각 value는 string.character로 변환되니 주의!!!
```



### 특정 문자를 대체 - String.replacingOccurrences

```swift
var str = "adcadc"
str.replacingOccurrences(of: "d", with: "b") // "abcabc"
```



### 조건에 부합하는 끝 자르기 - String.trimmingCharacters(in: ["!"])

- 조건에 부합하는 끝을 잘라준다

```swift
var str = "Hello World!"

print(str.trimmingCharacters(in: ["!"]))
```





### 문자 반복 - repeat

```swift
let str = String(repeating: "a", count: 5)
// "aaaaa"
```



### String.write

```swift
let str = "Super long string here"
let filename = getDocumentsDirectory().appendingPathComponent("output.txt")

do {
    try str.write(to: filename, atomically: true, encoding: String.Encoding.utf8)
} catch {
    // failed to write file – bad permissions, bad filename, missing permissions, or more likely it can't be converted to the encoding
}

func getDocumentsDirectory() -> URL {
    let paths = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)
    return paths[0]
}
```

[reference](https://www.hackingwithswift.com/example-code/strings/how-to-save-a-string-to-a-file-on-disk-with-writeto)



### 문자 읽고 쓰기

```swift
let url = "www.naver.com"
let fileManager = FileManager.default

let documentURL = fileManager.urls(for: .documentDirectory, in: .userDomainMask)[0]
let tokenURL = documentURL.appendingPathComponent("urlToken")
if !fileManager.fileExists(atPath: tokenURL.path) {
    try! url.write(to: tokenURL, atomically: false, encoding: .utf8)
}

let text = try! String(contentsOf: tokenURL, encoding: .utf8)
print(text)
```





### String sort vs Int sort

- String과 Int는 정렬할 때 다르다

```swift
var intArray = [11, 42, 17, 30, 5]
var stringArray = ["11", "42", "17", "30", "5"]
print(intArray.sorted())
print(stringArray.sorted())

// [5, 11, 17, 30, 42]
// ["11", "17", "30", "42", "5"]
```





## Int



### 떨어진 거리 - self.distance

 

```swift
1.distance(to: 2)
// 1
```





### ternary to decimal(3진수를 10진수로 변환)

```swift
var ternary = String(10, radix: 3) // String으로 반환
print(Int(ternary, radix: 3)!) // Int로 반환, Optional
// 10
```



### String - NSString을 사용해 Int로 변환

```swift
//2진수를 Int로 변환
String(format: "%.\(total)d", (String(3, radix: 2) as NSString).integerValue)
```



### String to Int - can be minus 



```swift
print(Int("-123")!)
// -123
```



### Uint

- UInt는 양수만 저장하는 타입(부호가 없는 정수)

[apple](https://developer.apple.com/documentation/swift/uint)



### 숫자 읽기 편하게하기

```swift
var foo = 111111var bar = 111_111print(foo == bar)
//true
```





## CharacterSet

### letters?

- 유니코드 일반 범주 L* 및 M*에 해당
- A character set containing the characters in Unicode General Category L* & M*.

[apple](https://developer.apple.com/documentation/foundation/nscharacterset/1408569-letters)



### 숫자만 골라내기 - characterSet.decimalDigits.inverted

- 십진수의 반대가(문자열) 해당된다

    ```swift
    print("1a3b5".components(separatedBy: CharacterSet.decimalDigits.inverted).joined())
    // 135
    ```



### 유니코드, Unicode



```swift
print(UnicodeScalar(90)!) // Z

print(UnicodeScalar("a").value) // 97
```





### indices

- index의 복수

```swift
let greeting = "Guten Tag"
for index in greeting.characters.indices {
  print(greeting[index])
}
// G
// u
// t
// e
// n
// 
// T
// a
// g
```



## Float

부호(1자리) + 지수(8자리) + 가수(23자리) = 32비트

```
single-precision 32-bit IEEE 754 floating point
```



### Float 모듈러 연산

```swift
let x = 8.625
print(x / 0.75)
// Prints "11.5"

let q = (x / 0.75).rounded(.towardZero)
// q == 11.0
let r = x.truncatingRemainder(dividingBy: 0.75)
// r == 0.375

let x1 = 0.75 * q + r
// x1 == 8.625
```





## Double

부호(1자리) + 지수(11자리) + 가수(52자리) = 64비트

```
double-precision 64-bit IEEE 754 floating point
```



## Bool

### Set variable to the Bool

- 신기한 클로저의 세계

```swift
var comparator: (Int, Int) -> Bool = (<)
```



## ASCII

```swift
var str = "Ho"

extension StringProtocol {
  var asciiValues: [UInt8] {
    compactMap(\.asciiValue) 
  }
}

print(str.asciiValues) 

// [72, 111]

print(str.utf16.map( {Int($0)} )) 

// [72, 111]
```



## etc.

### NumberFormatter

```swift
// 소수점 반올림 없이 자르기
let numberFormatter = NumberFormatter()
numberFormatter.roundingMode = .floor         // 형식을 버림으로 지정
numberFormatter.minimumSignificantDigits = 2  // 자르길 원하는 자릿수
numberFormatter.maximumSignificantDigits = 2
let originalNum = 1.6759402                   // 원하는 숫자
let newNum = numberFormatter.string(from: originalNum) // result 1.67
```

[reference](https://medium.com/@twih1203/swift5-numberformatter%EB%A1%9C-%EC%86%8C%EC%88%98%EC%A0%90-%EC%95%84%EB%9E%98-%EC%9E%90%EB%A6%BF%EC%88%98-%EB%B0%98%EC%98%AC%EB%A6%BC-%EC%97%86%EC%9D%B4-%EC%9E%90%EB%A5%B4%EA%B8%B0-ee33219e3cdd)



# Collections

## Array



### Index + range

```swift
var arr = [1, 2, 3]
print(arr[1...2])

// [2, 3]
// 출력된 타입은 Array<Int>.SubSequence임을 주의
```





### index 찾기

```swift
let arr = ["a","b","c","a"]

let indexOfA = arr.firstIndex(of: "a") // 0
let indexOfB = arr.lastIndex(of: "a") // 3

// Optional(0) Optional(3)


var arr = [3, 4, 5]
arr.firstIndex(where: {$0 == 3})

// Optional(0)
```





### .append(contentsOf:) 

- 해당 Array element와 다른 형식일때 컴파일러는 `contentsOf:` 를 권한다.

[reference](https://soooprmx.com/archives/7045)



### Array.capacity - 메모리 관련

> 배열에 요소를 추가할 때, 해당 배열이 예약된 용량을 초과하기 시작하면 배열은 더 큰 메모리 영역을 할당하고, 요소를 방금 할당한 새 메모리에 복사합니다. 이때 새로운 저장소는 이전 저장소 크기의 2배입니다.

```swift
var arr = [Int]()
arr.append(1)
arr.capacity 
// 2

arr.append(1)
arr.capacity 
// 2

arr.append(1)
arr.capacity
// 4
```



[reference]( https://zeddios.tistory.com/117?category=685736)





### 배열을 생성하는 여러가지 방법들

```swift
// 빈 배열 선언하기
var empty: [Int] = []
var empty2 = [Int]()
var empty3: Array<Int> = []

// 임의의 데이터가 들어가 있는 배열 만들기
var a : Array<Int>  = [1,2,3,4]
var b : [Int] = [1,2,3,4]
var c = [1,2,3,4] //타입유추
var d = Array<Int>(1...4) //[1,2,3,4]

// Range
let intArray: [Int] = Array(min...max)

// 응용 - 2차원 배열
var arr = [[Int]](repeating: Array(repeating: 0, count: 1), count: 15)
```

[reference](https://zeddios.tistory.com/114)



### Dictionary in Array 에서 Value 찾아 삭제

```swift
if let index = csvData.index(where: { $0["email"] as! String == email }) {
    csvData.remove(at: index)
    return true
}
```



### 조건에 맞는 것이 들어있느냐 - contain(where:)

```swift
var foo = [1, 2, 3]
print(foo.contains(where: { $0 > foo[0] }))

// true
```





### Enumerate에서 index로도 array 삭제가 가능

```swift
 for (index,word) in wordsCopy.enumerated() {
        //만약 begin과 wodrdCopy에 있는 단어와 한글자만 다르다면
        if isJustOneDifferent(begin, word) {
            //해당 word를 삭제해줌
            wordsCopy.remove(at: index)
            //word와 target이 똑같다면
            if word == target {
                //answer를 반환
                return answer
            //다르다면
            }else{
                //다시 changeWord에 begin을 word로 해주고 반복
                return changeWord(word, target)
            }
        }
    }
    return 0


```

[reference](https://gist.github.com/fomagran/fa7378c0b9aec387c08fe53b3625250b)







### rotate two dimensional array

[reference](https://stackoverflow.com/questions/42519/how-do-you-rotate-a-two-dimensional-array)



### reduce

combining the elements of the sequence

```swift
monthDay[0..<a-1].reduce(0, +) + b
```





#### reduce활용 - (인접)그래프 입력 받기

```swift
var graph = (0..<n).reduce(into: [[Int]]()) { result, _ in
    result.append(readLine()!.split(separator: " ").map { Int(String($0))! })
}
```





### filter

- Bool type 중 false 는 return하지 않는다

```swift
print([true, true, false, false].filter({ $0 }))
// [true, true]
```





### CompactMap, flatMap

> Swift 4.1부터는 1차원 배열에서 nil을 제거하고 옵셔널 바인딩을 하고싶으실때는 compactMap 사용.
> 2차원 배열을 1차원 배열로 flatten하게 만들때 flatMap을 사용.

```swift
let array1 = [1, nil, 3, nil, 5, 6, 7]
let flatMapTest1 = array1.flatMap{ $0 }
// 'flatMap' is deprecated: Please use compactMap(_:) for the case where closure returns an optional value
let compactMapTest1 = array1.compactMap { $0 }

print("flatMapTest1 :", flatMapTest1) 
print("compactMapTest1 :", compactMapTest1)

<출력>
flatMapTest1 : [1, 3, 5, 6, 7]
compactMapTest1 : [1, 3, 5, 6, 7]
```

[reference](https://jinshine.github.io/2018/12/14/Swift/22.%EA%B3%A0%EC%B0%A8%ED%95%A8%EC%88%98(2)%20-%20map,%20flatMap,%20compactMap/)

[reference2](https://zeddios.tistory.com/448)





### ArraySlice

> ArraySlice always uses contiguous storage and does not bridge to Objective-C.
>
> Warning: Long-term storage of ArraySlice instances is discouraged
>
> Because a ArraySlice presents a view onto the storage of some larger array even after the original array's lifetime ends, storing the slice may prolong the lifetime of elements that are no longer accessible, which can manifest as apparent memory and object leakage. To prevent this effect, use ArraySlice only for transient computation.

- ArraySlice는 일시적인 저장에만 사용해라



### 연속되는 수(`ClosedRange<Int>`) 배열로 바로 만들기 - Array Initializer with range



```swift
var continuousNumberArr = Array<Int>(1...5)
print(continuousNumberArr)
// [1, 2, 3, 4, 5]
```





### array에 해당 인덱스 안에 있나요?

```swift
let isIndexValid = array.indices.contains(index)
```





### 어서와 3D는 처음이지?

```swift
let zArry = [Int](repeating: 0, count: 10)
let yArry = [[Int]](repeating: zArry, count: 20)
let xArry = [[[Int]]](repeating: yArry, count: 30)
let val = xArry[2][1][1]
```







## Dictionary

### initialize

```swift
var dic : [Int : String] = [:]
var dic2 = [Int : String]()
var dic3 : Dictionary = [Int:String]()
var dic4 : Dictionary<Int, String> = Dictionary<Int, String>()
```

[reference](https://zeddios.tistory.com/129)



### Dictionary 해당 key의 value 초기값을 설정하고 싶으면

```swift
var countVAlueDict: [String:Int] = [:]
var inputWords = ["a", "b", "b", "c"]
for value in inputWords {
    countValueDict[value, default: 0] += 1 // default를 써주면 된다
}
```



### 해당 key를 넣었을 때 값이 없을 경우를 대비해

```swift
var gradeDic = ["a" : 90, "b" : 80, "c" : 70, "d" : 60]
print(gradeDic["a"] ?? 0) // Nil 병합 연산자, 값이 없을시 0을 출력
```



## Set

- 순서없이 저장, Array와 비슷하되 `: Set` 붙여줘야 한다
- 고유한 element를 가지도록 보장된다.

```swift
var aSet: Set = [11, 12, 13]
aSet.contains(12)
```



## etc.



### zip

- 두 개의 sequence로 구성된 한개의 sequence쌍을 만든다
- 하나만 sequence 넣어도 된다.

```swift
func solution(_ progresses:[Int], _ speeds:[Int]) -> [Int] {
    return zip(progresses, speeds)
        .map { (100 - $0) / $1 }
        .reduce(([], 0)) { (tuple, day) in
            let (list, lastMax) = tuple
            guard let lastCount = list.last else {
                return ([1], day)
            }
            if lastMax >= day {
                return (list.dropLast() + [lastCount + 1], lastMax)
            }
            return (list + [1], day)
        }.0
}
```





# Control Flow

## for



### 바깥 Loop를 지정해 continue

```swift
func solution2(_ n:Int) -> Int {
    var total = 0
    Loop :for i in 2...n {
        for j in 2...i{
            if i % j == 0 && j != i{
                continue Loop
            }
        }
        total += 1
    }
    return total
}
```





### for case let + where

```swift
let arrayOfOptionalInts : [Int?] = [nil, 2, 3, nil, 5]
for case let number? in arrayOfOptionalInts where number > 2 {
    print ("Found a\(number)")
}

//Found a3
//Found a5
```







## if



### Ternary Conditional Operator - 삼항 조건 연산자

```swift
var arr = array == [] ? [-1] : arrayvar isOverPoint = doBlindDateFlag = (point>=80) ? true : false
```





## guard

- 가드는 무엇보다 조건을 빨리 쳐내는 목적이 있는 구문
- 들여쓰기를 줄여 옵셔널 바인딩이 가능하다

```swift
func hoho() {
    var foo = Optional(1)
    guard let item = foo, foo == 0 else {
        print("HO")
        return
    }
}
hoho()
```





## forEach

### for와 차이점

- **forEach**에서는 `break`, `continue` 구문을 사용할 수 없고,
- `return`을 통해서 빠져나갈 수 있습니다. **(continue처럼 동작한다고 일단 생각해보기)**

```swift
array.forEach {
    print($0)
}

//1
//2
//3
//4
//5
```



## Switch

### where 절의 활용

```swift
let tuples = [(1,2), (1,-1), (1,0), (0,2)]

for tuple in tuples {
    switch tuple {
    case let (x,y) where x == y: print("같네")
    case let (x,y) where x == -y: print("마이너스")
    case let (x,y) where x>y: print("크다")
    case (1, _): print("x=1")
    default: print("\(tuple.0),\(tuple.1)")
    }
}

//x=1
//마이너스
//크다
//0,2
```



### where 절의 활용2

```swift
switch str {
case _ where str.hasPrefix("Foo"):
    print("prefix")
case _ where str.contains("Foo"):
    print("contains")
default:
    print("nope")
}
```

[reference](https://stackoverflow.com/questions/47305372/i-dont-understand-why-it-doesnt-let-me-check-contains-on-switch-cases)



### switch binding

```swift
let someValue = 5
let someOptional: Int? = nil

switch someOptional {
case .some(someValue):
    println("the value is \(someValue)")
case .some(let val):
    println("the value is \(val)")
default:
    println("nil")
}
```

[reference](https://stackoverflow.com/questions/26941529/swift-testing-against-optional-value-in-switch-case)





## while, do-while

- do-while은 먼저 실행하고 조건을 검사한다
- while은 검사하고 실행

```swift
do {
    square2 += board2[square2]
    if ++diceRoll2 == 7 { diceRoll2 = 1 }
    square2 += diceRoll2    
} while square2 < finalSquare
```



## Checking API Availability

Swift에서는 기본으로 특정 플랫폼 (iOS, macOS, tvOS, watchOS)과 특정 버전을 확인하는 구문을 제공해 줍니다. 이 구문을 활용해 특정 플랫폼과 버전을 사용하는 기기에 대한 처리를 따로 할 수 있습니다. 구문의 기본 형태는 다음과 같습니다.

```swift
if #available(iOS 10, macOS 10.12, *) {
    // Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS
} else {
    // Fall back to earlier iOS and macOS APIs
}
```





## Error Handle - let

```swift
var vendingMachine = VendingMachine()
vendingMachine.conisDeposited = 8

do {
  try buyFavoriteSnack(person: "Alice", vendingMachine: vendingMachine)
  print("Success! Yum.")
} catch VendingMachineError.invalidSelection {
  print("Invalid Selection.")
} catch VendingMachineError.outOfStock {
  print("Out of Stock.")
} catch VendingMachineError.insufficientFunds(let coinsNeeded) {
  print("Insufficient funds. Please insert an additional \(coinsNeeded) coins.")
} catch {
  print("Unexpected error: \(error).")
}
// "Insufficient funds. Please insert an additional 2 coins." 를 출력합니다.
```

[reference](https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html)





# Functions



### inout - 파라미터로 받은 값을 변경 가능하게

```swift
func exampleFunction(inout value: String, index: inout Int) -> Bool
```



- inout의 경우에는 그 파라미터가 변경될 수 있음을 암시한다
- swift에서 inout은 가급적 쓰지 않도록 하자 - 메모리 관리가 복잡

#### 원리

1. 함수가 호출되면, 매개변수로 넘겨진 변수가 복사됩니다.
2. 함수 몸체에서, 복사했던 값을 변화시킵니다.
3. 함수가 반환될 때, 이 변화된 값을 원본 변수에 재할당합니다.

[reference](https://jcsoohwancho.github.io/2019-12-19-inout/)



### for use function

`_ = testInterest(unitDay: 5)`



### variadic parameter - 가변인자 파라미터

파라미터 갯수를 여러개 넣을 수 있다

```swift
func calculateAverage(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / max(1, Double(numbers.count))
}

print(calculateAverage(1, 2, 3))
// 2
```





# Closure

클로저는 일반적으로 이런 형태를 띕니다.

```
{  (parameters) -> return type in
    statements
}
```



밑에서 리턴하는 결과는 모두 똑같다

```swift
var names: Array<String> = ["Chris", "Alex", "Ewa", "Barry", "Danialla"]

func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var reverseNames = names.sorted(by:backward)

var reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})

var reversedNames = names.sorted(by: { s1, s2 in return s1 > s2}) // Inferring Type From context

var reversedNames = names.sorted(by: {s1, s2 in s1 > s2}) // Implicit Returns from Single-Expression Closures

var reversedNames = names.sorted(by: { $0 > $1 }) // Shorthand Argument Names
```

[reference](https://docs.swift.org/swift-book/LanguageGuide/Closures.html#:~:text=Swift%20automatically%20provides%20shorthand%20argument,%2C%20%242%20%2C%20and%20so%20on.)



### 비교 연산자를 클로저로 할당

```swift
var comparator: (Int, Int) -> Bool = (>)
```



### defer

- defer는 클로저 안에 현재 스코프의 맨 뒤에 실행되어야 하는 코드를 넣어 사용한다
- defer는 클로져안에 현재 스코프의 맨 끝에 실행되어야 하는 코드 일부를 넣어서 사용한다.



```swift
var a = "Hello"

func b() -> String {
  defer { a.append(" world") }
  return a
}
func d() -> String {
  a.append(" world")
  return a
}
a = "Hello"
print(b())
a = "Hello"
print(d())

// Hello
// Hello world
```

[reference](https://kor45cw.tistory.com/269)



### @escaping(탈출 클로저)

함수가 종료된 뒤에 클로저가 필요한 경우

- 클로저 저장: 글로벌 변수에 클로저를 저장하여 함수가 종료된 후에 사용되는 경우
- 대표적인 예로 비동기 작업이 있는데 디스패치 큐 등을 사용하여 비동기 작업을 수행하는 경우, 함수가 종료된 뒤에도 클로저를 메모리에 잡아두어 비동기 작업을 이어가도록 해야 함



```swift
func work(after second: Int, completion: @escaping (() -> Void)) {
    let deadline = DispatchTime.now() + .seconds(second)
    DispatchQueue.main.asyncAfter(deadline: deadline) {
        completion()
    }
}

```

[reference](https://oaksong.github.io/2018/03/02/escaping-closure/)



### 읽기 전용 계산된 프로퍼티(Read-Only Computed Properties)와 클로저로 값을 할당하는건 다르다

```swift
var text = "Hello, World!"

//closure로 변수 할당
var foo: String = {
    return text
}()

//Read-Only Computed Properties
var bar: String {
    return text
}

print(foo) //Hello, World!
print(bar) //Hello, World!

text = "Hello, okstring!"

print(foo) //Hello, World!
print(bar) //Hello, okstring!
```

























