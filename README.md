# Kodeco Swift Style Vietnamese Version 🇻🇳 (Unofficial)

⚠️ Đây chỉ là phần tóm tắt + một số ví dụ của mình sau khi mình tìm hiểu [The Official Kodeco Swift Style Guide](https://github.com/kodecocodes/swift-style-guide.git). ← Click nếu bạn muốn tìm hiểu chi tiết.

⚠️ Mình cũng có bỏ qua một số phần, mình thấy không quan trọng lắm.

## Mục Lục

* [Correctness](#correctness)
* [Using SwiftLint](#using-swiftlint)
* [Naming](#naming)
  * [Delegates](#delegates)
  * [Use Type Inferred Context](#use-type-inferred-context)
  * [Generics](#generics)
  * [Language](#language)
  * [Code Organization](#code-organization)
  * [Protocol Conformance](#protocol-conformance)
  * [Unused Code](#unused-code)
  * [Minimal Imports](#minimal-imports)
* [Spacing](#spacing)
* [Comments](#comments)
* [Classes and Structures](#classes-and-structures)
  * [Use of Self](#use-of-self)
  * [Protocol Conformance](#protocol-conformance)
  * [Computed Properties](#computed-properties)
* [Function Declarations](#function-declarations)
* [Function Calls](#function-calls)
* [Closure Expressions](#closure-expressions)
* [Types](#types)
  * [Constants](#constants)
  * [Static Methods and Variable Type Properties](#static-methods-and-variable-type-properties)
  * [Optionals](#optionals)
  * [Lazy Initialization](#lazy-initialization)
  * [Type Inference](#type-inference)
  * [Syntactic Sugar](#syntactic-sugar)
* [Functions vs Methods](#functions-vs-methods)
* [Memory Management](#memory-management)
  * [Extending Object Lifetime](#extending-object-lifetime)
* [Access Control](#access-control)
* [Control Flow](#control-flow)
  * [Ternary Operator](#ternary-operator)
* [Golden Path](#golden-path)
  * [Failing Guards](#failing-guards)
* [Semicolons](#semicolons)
* [Parentheses](#parentheses)
* [Multi-line String Literals](#multi-line-string-literals)
* [No Emoji](#no-emoji)
* [No #imageLiteral or #colorLiteral](#no-imageliteral-or-colorliteral)
* [Organization and Bundle Identifier](#organization-and-bundle-identifier)
* [Copyright Statement](#copyright-statement)
* [Smiley Face](#smiley-face)
* [References](#references)

## Correctness

Cố gắng để code của bạn khi biên dịch không có cảnh báo ⚠️

## Using SwiftLint

Tìm hiểu [SwiftLint](https://github.com/kodecocodes/swift-style-guide/blob/main/SWIFTLINT.markdown) tại đây.

## Naming

- Cố gắng diễn đạt rõ ràng (ưu tiên sự rõ ràng hơn là ngắn gọn).
- Sử dụng camelCase thay vì snake_case.
- Sử dụng 'UpperCamelCase' cho `Objects` và `Protocols`, 'lowerCamelCase' cho những thứ còn lại.
- Đảm bảo sự rõ ràng và hiệu quả bằng cách sử dụng những từ cần thiết, tránh sử dụng các từ không cần thiết và lặp lại.
- Sử dụng tên theo vai trò, không theo kiểu dữ liệu (vd: **numberOfLists** thay vì ~~intCounter~~) có lẽ vậy, câu này hơi khó hình dung 🥶.
- Bắt đầu tên của Factory Methods với `make`, vd: `x.makeIterator()`.
- Đặt tên cho phương thức dựa trên tác động của chúng.
    - Thêm đuôi '-ed' hoặc '-ing' cho phương thức **non-mutating**
    
    Ví dụ:
    ```swift
    class TextEditor {
      var text = ""
    
      func capitalized() -> String {
        return text.uppercased()
      }
    
      func appending(_ string: String) -> String {
        return text + string
      }
    }
    ```
    - Tên phương thức (danh từ) thì thêm `form` đằng trước nếu đó là phương thức **mutating**, vd: `y.formUnion(z)` ; `c.formSuccessor(&i)`.

    - Kiểu `bool` nên được đặt như một sự khẳng định, vd: `isEmpty()`
    - Các `protocol` với mục đích để diễn tả đối tượng nên được đặt là một danh từ.
    
    Ví dụ:
    ```swift
    protocol Vehicle {
      var numberOfWheels: Int { get }
      var color: String { get }
    }

    class Car: Vehicle {
      var numberOfWheels: Int { return 4 }
      var color: String { return "red" }
    }

    class Bike: Vehicle {
      var numberOfWheels: Int {return 2}
      var color: String { return "blue"}
    }
    ```
    - Các `protocol` với mục đích để diễn tả khả năng của đối tượng nên kết thúc bằng '-able', 'ible'.
    
    Ví dụ:
    ```swift
    protocol Printable {
      func print()
    }

    class Document: Printable {
      func print() {
        // In ra thông tin của tài liệu
      }
    }
    ```
- Sử dụng thuật ngữ mà mọi dev đều thường dùng, không nên gây hoảng sợ cho người mới 🙂, vd: sử dụng `users` thay vì `clients`. Tuy nhiên thì vẫn còn tuỳ vào trường hợp, mình cho ví dụ để dễ hình dung thôi.
- Tránh sử dụng viết tắt (nếu có thể)
- Đặt tên theo cách đặt tên của hệ thống, vd: `viewDidLoad()` chẳng hạn.
- Ưu tiên sử dụng methods và properties hơn là các hàm tự do, vd: `user.login()`, ~~login(user)~~.
- Đặt cùng tên cho các phương thức có cùng ý nghĩa.
- Tránh việc định nghĩa nhiều phương thức trùng tên với kiểu trả về khác nhau (ở đây khắc phục với **Generics**)
> Generics là kiểu đại diện cho bất kỳ kiểu dữ liệu nào trong Swift.

- Sử dụng tốt tên tham số miêu tả, vd: `func calculateArea(width: Double, height: Double)`.
- Sử dụng các tham số có giá trị mặc định (nếu có thể) để làm giảm bớt số lượng tham số khi gọi hàm, vd: `func printName(firstName: String, lastName: String = "Hieu")`.

### Delegates

> When creating custom delegate methods, an unnamed first parameter should be the delegate source.

Nên:
```swift
  func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
  func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```
Không nên:
```swift
  func didSelectName(namePicker: NamePickerViewController, name: String)
  func namePickerShouldReload() -> Bool
```

### Use Type Inferred Context

> Trình biên dịch có thể tự suy luận ra ngữ cảnh để đoạn code ngắn gọn hơn

Nên:
```swift
  let selector = #selector(viewDidLoad)
  view.backgroundColor = .red
  let toView = context.view(forKey: .to)
  let view = UIView(frame: .zero)
```
Không nên:
```swift
  let selector = #selector(ViewController.viewDidLoad)
  view.backgroundColor = UIColor.red
  let toView = context.view(forKey: UITransitionContextViewKey.to)
  let view = UIView(frame: CGRect.zero)
```

### Generics

> Nên được đặt tên ở dạng UpperCamelCase, khi tên loại không có mối quan hệ sử dụng chữ cái viết hoa như `T`,`U`,`V`.

Nên:
```swift
  struct Stack<Element> { ... }
  func write<Target: OutputStream>(to target: inout Target)
  func swap<T>(_ a: inout T, _ b: inout T)
```
Không nên:
```swift
  struct Stack<T> { ... }
  func write<target: OutputStream>(to target: inout target)
  func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```

### Language

> Sử dụng cách viết Anh Mỹ để phù hợp với API của Apple

Nên:
```swift
  let color = "red"
```
Không nên:
```swift
  let colour = "red"
```

## Code Organization

> Sử dụng `extension` để tổ chức code thành các khối chức năng, mỗi `extension` nên được phân chia bằng `//MARK: ` - giúp dễ dàng khi đọc code.

### Protocol Conformance

Nên:
```swift
  class MyViewController: UIViewController {
    // class stuff here
  }

  // MARK: - UITableViewDataSource
  extension MyViewController: UITableViewDataSource {
    // table view data source methods
  }

  // MARK: - UIScrollViewDelegate
  extension MyViewController: UIScrollViewDelegate {
    // scroll view delegate methods
  }
```
Không nên:
```swift
  class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
    // all methods
  }
```

### Unused Code

- Đoạn code không được sử dụng, bao gồm code mẫu của Xcode, các comment trong thân hàm nên được loại bỏ.
- Các phương thức chỉ gọi lại lớp cha cũng cần được loại bỏ.

Nên:
```swift
  override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return Database.contacts.count
  }
```
Không nên:
```swift
  override func didReceiveMemoryWarning() {
    super.didReceiveMemoryWarning()
    // Dispose of any resources that can be recreated.
  }

  override func numberOfSections(in tableView: UITableView) -> Int {
    // #warning Incomplete implementation, return the number of sections
    return 1
  }

  override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    // #warning Incomplete implementation, return the number of rows
    return Database.contacts.count
  }
```

### Minimal Imports

- Chỉ nên `import` module mà source file yêu cầu, không nên `import UIKit` khi mà `import Foundation` đã đủ, tương tự không `import Foundation` khi đã `import UIKit`.

Nên:
```swift
  import UIKit
  
  var view: UIView
  var deviceModels: [String]
```
Nên:
```swift
  import Foundation
  
  var deviceModels: [String]
```
Không nên:
```swift
  import UIKit
  
  import Foundation
  var view: UIView
  var deviceModels: [String]
```
Không nên:
```swift
  import UIKit
  
  var deviceModels: [String]
```

## Spacing

- Thụt đầu dòng bằng 2 spaces thay vì tab để tiết kiệm không gian, nhưng mình thấy Xcode mặc định (1 tab = 2 spaces) nên phần này không cần quan tâm lắm 😁.
- Đối với phần dấu ngoặc nhọn của `if/else/switch/while/etc`.

Nên:
```swift
  if user.isHappy {
    // Do something
  } else {
    // Do something else
  }
```
Không nên:
```swift
  if user.isHappy
  {
    // Do something
  }
  else {
    // Do something else
  }
```
> Tip: bạn có thể Re-Indent bằng cách chọn đoạn code hoặc chọn tất cả bằng `cmd + A` sau đó `Control + I` hoặc `Editor ▸ Structure ▸ Re-Indent` trên **Menu**
- Nên có một dòng trống để phân cách các phương thức.
- Trong phương thức phân cách các chức năng bằng một dòng trắng, nhưng nếu có nhiều khoảng trắng thì bạn nên cấu trúc lại thành nhiều phương thức có chức năng riêng.
- Không nên có dòng trống sau dấu ngoặc nhọn mở hoặc trước dấu ngoặc nhọn đóng.
- Dấu ngoặc đơn đóng không được tự xuất hiện trên một dòng.

Nên:
```swift
  let user = try await getUser(
    for: userID,
    on: connection)
```
Không nên:
```swift
  let user = try await getUser(
    for: userID,
    on: connection
  ) //<-- xuất hiện một mình trên 1 dòng
```
- Dấu `:` luôn không có khoảng cách ở bên trái, và 1 space ở phía bên phải. Ngoại lệ trong cú pháp toán tử bậc 3 `age >= 18 ? "Yes" : "No"`, một Dictionary rỗng `[:]` và `#selector` cú pháp `addTarget(_:action:)` (vì trường hợp này 2 phía đều 0 space).

Nên:
```swift
  class TestDatabase: Database {
    var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
  }
```
Không nên:
```swift
  class TestDatabase : Database {
    var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
  }
```
- Trên 1 dòng nên được gói gọn với khoảng 70 ký tự.
- Tránh các khoảng trắng ở cuối dòng.
- Ở cuối mỗi source file nên có thừa 1 dòng trống.

## Comments

> Tránh sử dụng comments kiểu C `/* ... */`, thay vào đó hãy sử dụng `//` hoặc `///`.

## Classes and Structures

- Tìm hiểu và phân biệt chúng [tại đây](https://fxstudio.dev/lap-trinh-huong-doi-tuong-oop-voi-swift/)

### Example definition

Đây là một ví dụ khởi tạo class nên dùng:

```swift
  class Circle: Shape {
    var x: Int, y: Int
    var radius: Double
    var diameter: Double {
      get {
        return radius * 2
      }
      set {
        radius = newValue / 2
      }
    }

    init(x: Int, y: Int, radius: Double) {
      self.x = x
      self.y = y
      self.radius = radius
    }

    convenience init(x: Int, y: Int, diameter: Double) {
      self.init(x: x, y: y, radius: diameter / 2)
    }

    override func area() -> Double {
      return Double.pi * radius * radius
    }
  }

  extension Circle: CustomStringConvertible {
    var description: String {
      return "center = \(centerString) area = \(area())"
    }
    private var centerString: String {
      return "(\(x),\(y))"
    }
  }
```
> Áp dụng đúng theo các quy tắc đã nhắc ở trên.

### Use of Self

> Tránh sử dụng `self` nếu trình biên dịch không yêu cầu.
- Sử dụng trong các `closure @escaping` hoặc trong các khởi tạo để làm rõ `thuộc tính` và `đối số`.
> Tóm lại nếu biên dịch được mà không cần `self` thì vứt `self` đi.

### Computed Properties

Để ngắn gọn, nếu một `computed property` là read-only (chỉ đọc) thì có thể bỏ qua `get`, chỉ phải viết đầy đủ khi có cả `get` lẫn `set`.

Nên:
```swift
  var diameter: Double {
    return radius * 2
  }
```
Không nên:
```swift
  var diameter: Double {
    get {
      return radius * 2
    }
  }
```

## Function Declarations

Khai báo `func` ngắn gọn trên một dòng, bao gồm cả dấu ngoặc nhọn mở.
```swift
  func reticulateSplines(spline: [Double]) -> Bool {
    // reticalate code goes here
  }
```
Đối với `func` dài với nhiều tham số thì đặt mỗi tham số ở một dòng mới như sau:
```swift
  func reticulateSplines(
    spline: [Double], 
    adjustmentFactor: Double,
    translateConstant: Int, 
    comment: String
  ) -> Bool {
    // reticulate code goes here
  }
```
Cách sử dụng `(Void)`:

Nên:
```swift
  func updateConstraints() -> Void {
    // magic happens here
  }

  typealias CompletionHandler = (result) -> Void
```
Không nên:
```swift
  func updateConstraints() -> () {
    // magic happens here
  }

  typealias CompletionHandler = (result) -> ()
```

## Function Calls

Đây là cách gọi một `func` phù hợp:

```swift
  let success = reticulateSplines(splines)
```

```swift
  let success = reticulateSplines(
    spline: splines,
    adjustmentFactor: 1.3,
    translateConstant: 2,
    comment: "normalize the display")
```

## Closure Expressions

Nên:
```swift
  UIView.animate(withDuration: 1.0) {
    self.myView.alpha = 0
  }

  UIView.animate(withDuration: 1.0, animations: {
    self.myView.alpha = 0
  }, completion: { finished in
    self.myView.removeFromSuperview()
  })
```
Không nên:
```swift
  UIView.animate(withDuration: 1.0, animations: {
    self.myView.alpha = 0
  })

  UIView.animate(withDuration: 1.0, animations: {
    self.myView.alpha = 0
  }) { f in
    self.myView.removeFromSuperview()
  }
```

Đối với các single-expression closure có ngữ cảnh rõ ràng, sử dụng mà không cần `return`

```swift
  attendeeList.sort { a, b in
    a > b
  }
```

Một ví dụ sử dụng đối số vô danh `$0`,`$1`

```swift
  let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.index(of: 90)

  let value = numbers
    .map {$0 * 2}
    .filter {$0 > 50}
    .map {$0 + 10}
```

## Types

Luôn sử dụng các kiểu dữ liệu và biểu thức có sẵn của Swift.

Nên:
```swift
  let width = 120.0                                    // Double
  let widthString = "\(width)"                         // String
```
Nên:
```swift
  let width = 120.0                                    // Double
  let widthString = (width as NSNumber).stringValue    // String
```
Không nên:
```swift
  let width: NSNumber = 120.0                          // NSNumber
  let widthString: NSString = width.stringValue        // NSString
```

### Constants

Các hằng số được khai báo với từ khoá `let` và các biến số được khai báo với từ khoá `var`, luôn dùng `let` nếu giá trị của biến đó không thay đổi.

> Tip: Luôn khai báo với từ khoá `let` và chỉ chuyển sang `var` khi trình biên dịch kêu gào 😂.

Nên:
```swift
  enum Math {
    static let e = 2.718281828459045235360287
    static let root2 = 1.41421356237309504880168872
  }

  let hypotenuse = side * Math.root2
```
Không nên:
```swift
  let e = 2.718281828459045235360287  // pollutes global namespace
  let root2 = 1.41421356237309504880168872

  let hypotenuse = side * root2 // what is root2?
```
> Lợi ích của việc sử dụng `enum` là nếu không có trường hợp đó nó sẽ không được khởi tạo một cách tuỳ ý.

### Static Methods and Variable Type Properties

- Các `static method` và các `type property` có cách hoạt động tương tự như hàm toàn cục và biến toàn cục nên phải thận trọng khi sử dụng.

### Optionals

- Khai báo biến hoặc hàm có kiểu trả về của hàm là `optional` dùng dấu `?` để có thể chấp nhận giá trị `nil`.

Sử dụng **optional chaining** truy cập vào giá trị `optional`:

 ```swift
  textContainer?.textLabel?.setNeedsDisplay()
 ```
 Sử dụng **optional binding** để truy cập và thực hiện nhiều thao tác:
 
 ```swift
  if let textContainer = textContainer {
    // do many things with textContainer
  }
 ```
 > Swift 5.7 giới thiệu cú pháp ngắn gọn hơn:
  ```swift
  if let textContainer {
    // do many things with textContainer
  }
 ```
 
 Khi đặt tên cho biến và thuộc tính tuỳ chọn, cần tránh đặt theo kiểu `optionalString` hay `maybeView` vì tính tuỳ chọn của chúng đã được khai báo rõ ràng trong kiểu chữ liệu.
 
 Nhưng đối với optional binding, thì có thể sử dụng tên theo kiểu `unwrappedView` hay `actualLabel`.
 
 Nên:
 ```swift
  var subview: UIView?
  var volume: Double?

  // later on...
  if let subview = subview, let volume = volume {
    // do something with unwrapped subview and volume
  }

  // another example
  resource.request().onComplete { [weak self] response in
    guard let self = self else { return }
    let model = self.updateModel(response)
    self.updateUI(model)
  }
 ```
 Không nên:
  ```swift
  var optionalSubview: UIView?
  var volume: Double?

  if let unwrappedSubview = optionalSubview {
    if let realVolume = volume {
      // do something with unwrappedSubview and realVolume
    }
  }

  // another example
  UIView.animate(withDuration: 2.0) { [weak self] in
    guard let strongSelf = self else { return }
    strongSelf.alpha = 1.0
  }
 ```

### Lazy Initialization

Xem xét sử dụng khởi tạo lười biếng (lazy initialization) để kiểm soát chính xác thời gian sống của đối tượng. Điều này đặc biệt đúng đối với cách UIViewController tải view lười biếng. Bạn có thể sử dụng một `closure` được gọi ngay lập tức `{}()` hoặc gọi một `private factory method`.

Ví dụ:

```swift
  lazy var locationManager = makeLocationManager()

  private func makeLocationManager() -> CLLocationManager {
    let manager = CLLocationManager()
    manager.desiredAccuracy = kCLLocationAccuracyBest
    manager.delegate = self
    manager.requestAlwaysAuthorization()
    return manager
  }
```
Trong ví dụ này không cần dùng tới `[unowned self]` vì không tạo ra vòng lặp giữa các đối tượng.

`CLLocationManager` có tác động đến việc hiển thị giao diện người dùng để yêu cầu quyền truy cập vị trí, vì vậy sử dụng khởi tạo lười biếng giúp kiểm soát tốt hơn trong trường hợp này.

### Type Inference

Để code gọn gàng hơn, hãy để trình biên dịch tự suy ra kiểu dữ liệu cho hằng số hoặc biến số đó. Khi cần thiết, hãy chỉ đỉnh kiểu cụ thể như `CGFloat`, `Int16`.

Nên:
```swift
  let message = "Click the button"
  let currentBounds = computeViewBounds()
  var names = ["Mic", "Sam", "Christine"]
  let maximumWidth: CGFloat = 106.5
```
Không nên:
```swift
  let message: String = "Click the button"
  let currentBounds: CGRect = computeViewBounds()
  var names = [String]()
```

### Type Annotation for Empty Arrays and Dictionaries

Đối với các mảng và từ điển rỗng, nên chú thích kiểu dữ liệu (type annotation) rõ ràng. (Đối với một mảng hoặc từ điển được gán cho một biểu thức nhiều dòng lớn, hãy sử dụng chú thích kiểu dữ liệu.)

Nên:
```swift
  var names: [String] = []
  var lookup: [String: Int] = [:]
```
Không nên:
```swift
  var names = [String]()
  var lookup = [String: Int]()
```

### Syntactic Sugar

Sử dụng phiên bản ngắn gọn cho khai báo kiểu thay vì dùng cú pháp **Generics** đầy đủ.

Nên:
```swift
  var deviceModels: [String]
  var employees: [Int: String]
  var faxNumber: Int?
```
Không nên:
```swift
  var deviceModels: Array<String>
  var employees: Dictionary<Int, String>
  var faxNumber: Optional<Int>
```

## Functions vs Methods


