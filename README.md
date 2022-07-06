**<h1>Typealias usage in Swift</h1>**

Typealias được hiểu là đặt tên mới cho type hiện tại, chúng giúp cho mã code của chúng ta dễ hiểu hơn. Nó sẽ thực sự hữu ích nếu sử dụng nó một cách thông minh.

<h2>Declaring a typealias</h2>

A typealias được sử dụng trong Swift bằng cách đặt `typealias` keyword vào phía trước type bạn muốn gán. Một ví dụ cụ thể ở đây chúng ta sẽ đặt tên cụ thể cho một loại tiền tệ, như Dollars. Ví dụ sau là một struct Receipt:

```swift
struct Receipt {
    let totalCosts: Double
}
```

Hiên tại ta chỉ biết type của totalCosts là `Double`. Ta có thể cải thiện source code bằng cách thêm typealias:

```swift
typealias Dollar = Double
```

Điều này giúp struct Receipt nhiều ngữ cảnh hơn và giúp cả thiện khả năng đọc source code:

```swift
struct Receipt {
    let totalCosts: Dollar
}
```

Một typealias có thể là một thay thế đơn giản để tạo một class hoặc subclass tùy chỉnh.

<h2>Is a typealias a new type?</h2>

Bản chất thì typealias chỉ giúp tạo ra 1 bí danh cho type có sẵn. `Dollar` về cơ bản vẫn là  `Double` với một tên khác và do đó, nó có thể dược sử dụng giống như `Double`.

Nó cũng hoạt động theo những cách xung quanh. Nếu bạn tạo một extension cho typealias, về cơ bản bạn đang tạo extension cho type ban đầu. Ví dụ sau đây cho thấy rằng method `toEuro()` vẫn được dùng cho cả `Dollar` và `Double` value.:

```swift
typealias Dollar = Double
typealias Euro = Double

struct Receipt {
    let totalCosts: Dollar
}

extension Dollar {
    func toEuro() -> Euro {
        return self * 0.896
    }
}

let receipt = Receipt(totalCosts: 10)
receipt.totalCosts.toEuro() // 8.96

let doubleNumber: Double = 10
doubleNumber.toEuro() // 8.96
```

<h2>Combining with generics</h2>

Generics cũng có thể được sử dụng kết hợp với typealias. Ví dụ dưới đây, ta có thể sử dụng `Result` type mới để tạo exchange result type:

```swift
typealias Dollar = Double
typealias Euro = Double

struct Receipt {
    let totalCosts: Dollar
}

typealias Currency = Double
typealias ExchangeResult<Currency> = Result<Currency, Error>

enum ExchangeError: Error {
    case invalidInput
}

extension Dollar {
    func toEuro() -> ExchangeResult<Euro> {
        guard self > 0 else {
            return ExchangeResult.failure(ExchangeError.invalidInput)
        }
        return Result.success(self * 0.896)
    }
}

let receipt = Receipt(totalCosts: 10)
receipt.totalCosts.toEuro() // .success(8.96)

let doubleNumber: Double = 10
doubleNumber.toEuro() // .success(8.96)

```

<h2>Other valuable usage examples</h2>

A typealias thường sử dụng trong project như một completion callbacks:

```swift
typealias Completion = () -> Void
```

Tạo delegate conform to multiple types:

```swift
typealias TransitionDelegate = UIViewController & UIViewControllerTransitioningDelegate
```

Or, tạo một bí danh đơn giản của dictionary để sử dụng ở nhiều nơi:

```swift
/// A dictionary containing properties to send with the tracking operation
public typealias TrackingProperties = [String: Any]
```

<h2>Conclusion</h2>

A typealias có thể hữu ích để cải thiện khả năng đọc trong codebase của bạn như bạn có thể thấy. Xem những gì nó có thể làm cho dự án của bạn và cố gắng làm cho source code giống document hơn.