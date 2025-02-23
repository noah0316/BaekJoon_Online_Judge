# Apple Developer Documentation

# Arrays

An ordered, random-access collection.

## Declaration

`@frozen struct Array<Element>`

## Overview

배열은 앱에서 가장 일반적으로 사용되는 데이터 타입 중 하나입니다.

배열은 앱의 데이터를 구성하는 데 사용합니다.

배열은 정수, 문자열, 클래스 등 모든 종류의 Type을 저장할 수 있습니다.

> 배열에 저장할 요소의 타입에는 제약이 없지만,
> 하나의 배열에 저장하는 요소의 타입은 모두 같아야 합니다.

Swift는 배열 리터럴을 사용하여 배열을 쉽게 만들 수 있습니다.
Swift는 지정된 값이 포함된 배열을 작성하여 배열의 요소 유형을 자동으로 추론합니다.

For example:

```swift
// An array of 'Int' elements
let oddNumbers = [1, 3, 5, 7, 9, 11, 13, 15]

// An array of 'String' elements
let streets = ["Albemarle", "Brandywine", "Chesapeake"]
```

선언에 배열의 요소 타입을 지정하여 빈 배열을 작성할 수 있습니다.

For example:

```swift
// Shortened forms are preferred
var emptyDoubles: [Double] = []

// The full type name is also allowed
var emptyFloats: Array<Float> = Array()
```

고정된 수의 default value로 미리 초기화된 배열이 필요한 경우 `Array(repeating:count:)`
이니셜라이저를 사용합니다.

For example:

```swift
var digitCounts = Array(repeating: 0, count: 10)
print(digitCounts)
// Prints "[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]"
```

## **Accessing Array Values**

배열의 모든 요소에 대해 작업을 수행해야 하는 경우 for-in loop를 사용하여 배열의 내용을 반복합니다.

```swift
for street in streets {
    print("I don't live on \(street).")
}
// Prints "I don't live on Albemarle."
// Prints "I don't live on Brandywine."
// Prints "I don't live on Chesapeake."
```

`isEmpty` 속성을 사용하여 배열에 요소가 있는지 확인하거나,
카운트 속성을 사용하여 배열의 요소 수를 확인할 수 있습니다.

```swift
if oddNumbers.isEmpty {
    print("I don't know any odd numbers.")
} else {
    print("I know \(oddNumbers.count) odd numbers.")
}
// Prints "I know 8 odd numbers."
```

배열의 첫 번째 및 마지막 요솟값에 안전하게 액세스하려면 `first` , `last` 속성을 사용합니다.
배열이 비어 있으면 이러한 속성은 `nil`입니다

```swift
if let firstElement = oddNumbers.first, let lastElement = oddNumbers.last {
    print(firstElement, lastElement, separator: ", ")
}
// Prints "1, 15"

print(emptyDoubles.first, emptyDoubles.last, separator: ", ")
// Prints "nil, nil"
```

`subscript`를 통해 개별 배열 요소에 접근할 수 있습니다.
비어 있지 않은 배열의 첫 번째 요소의 인덱스 항상 0입니다.
배열 요소의 count보다 크거나 같은 인덱스 또는 음수 인덱스를 사용하면 런타임 오류가 발생합니다.

For example:

```swift
print(oddNumbers[0], oddNumbers[3], separator: ", ")
// Prints "1, 7"

print(emptyDoubles[0])
// Triggers runtime error: Index out of range
```

## **Adding and Removing Elements**

강의 중인 수업에 등록한 학생 이름 목록을 저장해야 한다고 해봅시다.
등록 기간 동안 학생들이 수업을 추가 및 삭제함에 따라 이름을 추가 및 삭제해야 하는 기능을 지원해야 한다고 가정해봅시다.

```swift
var students = ["Ben", "Ivy", "Jordell"]
```

배열의 끝에 단일 요소를 추가하려면 `append(_:)` 메서드를 사용합니다.

배열의 끝에 여러 요소를 동시에 추가하려면 `append(contentsOf:)` 메서드에 다른 배열 또는 임의의 시퀀스를 전달하여 여러 요소를 동시에 추가합니다.

```swift
students.append("Maxime")
students.append(contentsOf: ["Shakia", "William"])
// ["Ben", "Ivy", "Jordell", "Maxime", "Shakia", "William"]
```

배열의 중간에 단일 요소를 삽입하려면 `insert(_:at:)` 메소드를 사용하고, 배열의 중간에 다른 컬렉션, 배열 리터럴을 삽입하려면 `insert(contentsOf:at:)` 메소드를 사용합니다.
해당 인덱스와 이후 인덱스의 요소는 공간을 확보하기 위해 다시 이동됩니다.

```swift
students.insert("Liam", at: 3)
// ["Ben", "Ivy", "Jordell", "Liam", "Maxime", "Shakia", "William"]
```

배열에서 요소를 제거하려면 `remove(at:)`, `removeSubrange(_:)` 및 `removeLast()` 메서드를 사용합니다.

```swift
// Ben's family is moving to another state
students.remove(at: 0)
// ["Ivy", "Jordell", "Liam", "Maxime", "Shakia", "William"]

// William is signing up for a different class
students.removeLast()
// ["Ivy", "Jordell", "Liam", "Maxime", "Shakia"]
```

새로운 값을 `subscript`에 할당하여 기존 요소를 새로운 값으로 바꿀 수 있습니다.

```swift
if let i = students.firstIndex(of: "Maxime") {
    students[i] = "Max"
}
// ["Ivy", "Jordell", "Liam", "Max", "Shakia"]
```

## **Growing the Size of an Array**

모든 배열은 콘텐츠를 저장하기 위해 일정량의 메모리를 예약합니다.
요소를 배열에 추가하고 해당 배열이 예약된 용량을 초과하기 시작하면 배열은 더 큰 메모리 영역을 할당하고,
해당 요소를 새 스토리지에 복사합니다.
새 스토리지는 이전 스토리지 크기의 배수입니다.

> Array Doubling과 관련 있습니다.
> [https://zeddios.tistory.com/62](https://zeddios.tistory.com/62) 이 글을 같이 읽어보시면 도움이 될 것입니다!

이러한 기하급수적인 growth 전략은 요소를 추가하는 작업이 일정한 시간 내에 수행되어 많은 추가 작업의 성능을 평균화합니다.
재할당을 트리거 하는 추가 작업은 성능비용이 발생하지만 배열이 커질수록 발생하는 빈도는 점점 줄어듭니다.

> 하지만, 우리는 일반적으로 용량에 대해 걱정할 필요가 없어요.
> Swift는 효율적인 재할당기법을 사용하기 때문에 재할당은 거의 성능 문제가 되지 않습니다:)

저장해야 할 요소의 개수를 대략적으로 알고 있는 경우 중간에 재할당하는 것을 피하기 위해
배열에 추가하기 전에 `reserveCapacity(_:)` 메서드를사용하십시오.
`capacity` 및 `count` 속성을 사용하여 배열이 더 큰 스토리지를 할당하지 않고 저장할 수 있는 요소의 수를 결정합니다.

여기서 말하는 스토리지는 연속적인 메모리 블록입니다.
Element Type이 클래스 또는 @objc 프로토콜 유형인 배열의 경우 이 저장소는 메모리의 연속 블록 또는 NSArray 인스턴스가 될 수 있습니다.
NSArray의 임의의 하위 클래스는 배열이 될 수 있기 때문에 이 경우 representation이나 효율성에 대한 보장은 없습니다.

## **Modifying Copies of Arrays**

각 배열에는 모든 요소의 값이 포함된 independent value가 있습니다.

즉, integer와 같은 단순 type의 경우 한 배열에서 값을 변경할 때

해당 요소의 값은 배열의 복사본에서 변경되지 않습니다.

> Value Type의 특성과 같네요!

For example:

```swift
var numbers = [1, 2, 3, 4, 5]
var numbersCopy = numbers
numbers[0] = 100
print(numbers)
// Prints "[100, 2, 3, 4, 5]"
print(numbersCopy)
// Prints "[1, 2, 3, 4, 5]"
```

배열의 요소들이 클래스의 인스턴스라면, 처음에는 다르게 보일지라도 semantics는 동일하다.

> semantics는 문장이나 단위프로그램을  
> 컴퓨터에서 실행한 효과를 명세하여 문장이나 프로그램의 의미를 서술한 것을 말한다.

- **3학년 2학기 프로그래밍언어 수업 中** -
  >

이 경우 배열에 저장된 값은 배열 외부에 있는 객체에 대한 참조입니다.
한 배열의 객체에 대한 참조를 변경하면 해당 배열만 새 객체에 대한 참조를 갖게 됩니다.
그러나 두 배열에 동일한 객체에 대한 참조가 포함되어 있으면

두 배열에서 해당 개체의 속성이 변경되는 것을 관찰할 수 있습니다.

For example:

```swift
// An integer type with reference semantics
class IntegerReference {
    var value = 10
}
var firstIntegers = [IntegerReference(), IntegerReference()]
var secondIntegers = firstIntegers

// Modifications to an instance are visible from either array
firstIntegers[0].value = 100
print(secondIntegers[0].value)
// Prints "100"

// Replacements, additions, and removals are still visible
// only in the modified array
firstIntegers[0] = IntegerReference()
print(firstIntegers[0].value)
// Prints "10"
print(secondIntegers[0].value)
// Prints "100"
```

표준 라이브러리의 모든 가변 크기 컬렉션과 마찬가지로 배열도 **copy-on-write** 최적화를 사용합니다.
배열의 여러 복사본은 복사본 중 하나를 수정할 때까지 동일한 스토리지를 공유합니다.

이 경우 수정 중인 배열은 스토리지를 고유하게 소유한 자체 복사본으로 교체한 다음 해당 위치에서 수정됩니다.

복사본의 양을 줄일 수 있는 최적화 기능이 적용되기도 합니다.

즉, 배열이 다른 복사본과 스토리지를 공유하는 경우 해당 어레이의 첫 번째 mutating 작업에서 어레이를 복사하는 비용이 발생합니다.
스토리지의 유일한 소유자인 배열은 해당 스토리지에서 변경 작업을 수행할 수 있습니다.

아래의 예에서 `numbers` 배열이 동일한 스토리지를 공유하는 복사본 두 개와 함께 생성됩니다.
original `numbers` 배열이 수정되면 수정하기 전에 저장소의 고유한 복사본을 만듭니다.
두 복사본이 original 스토리지를 계속 공유하는 동안 `numbers`를 추가로 수정합니다.

```swift
var numbers = [1, 2, 3, 4, 5]
var firstCopy = numbers
var secondCopy = numbers

// The storage for 'numbers' is copied here
numbers[0] = 100
numbers[1] = 200
numbers[2] = 300
// 'numbers' is [100, 200, 300, 4, 5]
// 'firstCopy' and 'secondCopy' are [1, 2, 3, 4, 5]
```

## **Bridging Between Array and NSArray**

Array 대신 NSArray 인스턴스에 데이터가 필요한 API에 액세스해야 하는 경우 type-cast 연산자(as)를 사용하여 인스턴스를 브리지합니다.

브릿징이 가능하려면 배열의 요소 유형이 클래스, @objc 프로토콜(Object-C에서 가져오거나 @objc 특성으로 표시된 프로토콜) 또는 Foundation 유형으로 브리지되는 유형이어야 합니다.

다음 예에서는 어레이 인스턴스를 NSArray에 브리지하여 `write(to:atomically:)` 메소드를 사용하는 방법을 보여 줍니다.
이 예에서 색상 배열은 `colors` 배열의 문자열 요소가 NSString에 브리지되기 때문에
NSArray에 브리지될 수 있습니다.
반면 컴파일러는 `moreColors` 배열의 요소 type이 Foundation 형식으로 브리지되지 않는 `Optional<String>`이기 때문에 `moreColors` 배열의 브릿징을 방지합니다.

```swift
let colors = ["periwinkle", "rose", "moss"]
let moreColors: [String?] = ["ochre", "pine"]

let url = URL(fileURLWithPath: "names.plist")
(colors as NSArray).write(to: url, atomically: true)
// true

(moreColors as NSArray).write(to: url, atomically: true)
// error: cannot convert value of type '[String?]' to type 'NSArray'
```

배열의 요소가 이미 클래스의 인스턴스 또는 @objc 프로토콜인 경우 Array에서 NSAray로 브릿징하려면
O(1) 시간과 O(1) 공간이 필요합니다.
그렇지 않으면 O(n) 시간과 공간이 필요합니다.

destination 배열의 요소 type이 클래스 또는 @objc 프로토콜인 경우 NSArray에서 Array로 브릿징하면 먼저 `copy(with:)(- copyWithZone: in Objective-C)` 메소드를 호출하여 배열에서 immutable(불변) 복사본을 얻은 후 O(1) 시간이 걸리는 Swift bookkeeping작업을 추가로 수행합니다.

이미 immutable(불변)인 NSArray 인스턴스의 경우, `copy(with:)`는 일반적으로 O(1) 시간 내에 동일한 배열을 반환합니다.
`copy(with:)`가 동일한 어레이를 반환하는 경우 NSArray 및 Array 인스턴스는 어레이의 두 인스턴스가 스토리지를 공유할 때 사용되는 것과 동일한 Copy-on-write 최적화를 사용하여 스토리지를 공유합니다.

destination 어레이의 요소 유형이 Foundation 유형에 브리지되는 non-class type인 경우 NSArray에서 어레이로 브릿징되는 요소는 O(n) 시간 내에 인접 스토리지에 브릿징 복사를 수행합니다.
예를 들어 `NSArray`에서 `Array<Int>`로의 브릿징은 이러한 복사를 수행합니다.
Array 인스턴스의 요소에 액세스할 때 추가 브릿징이 필요하지 않습니다.

> ContinuousArray 및 ArraySlice 유형은 브리지되지 않으며, 이러한 type의 인스턴스는 항상 연속적인 메모리 블록을 스토리지로 사용합니다.

### 참고

---

[Swift Array - Apple Devleoper Documentation](https://developer.apple.com/documentation/swift/array)

[Array. count? capacity? - ZeddiOS님](https://zeddios.tistory.com/117)
