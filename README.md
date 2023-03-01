
# Kodeco Swift Style Vietnamese Version 🇻🇳(Unofficial) 

⚠️ Đây chỉ là phần tóm tắt + một số ví dụ của mình sau khi mình tìm hiểu [The Official Kodeco Swift Style Guide](https://github.com/kodecocodes/swift-style-guide.git). ← Click nếu bạn muốn tìm hiểu chi tiết.
⚠️ Mình cũng có bỏ qua một số phần, bản thân cho là không quan trọng lắm.

## Mục Lục

* [Correctness](#correctness)
* [Using SwiftLint](#using-swiftlint)
* [Naming](#naming)
  * [Delegates](#delegates)
  * [Use Type Inferred Context](#use-type-inferred-context)
  * [Generics](#generics)
  * [Language](#language)

## Correctness

Cố gắng để code của bạn khi biên dịch không có cảnh báo ⚠️

## Using SwiftLint

Tìm hiểu [SwiftLint](https://github.com/kodecocodes/swift-style-guide/blob/main/SWIFTLINT.markdown) ở đây.

## Naming

- Cố gắng diễn đạt rõ ràng (Ưu tiên sự rõ ràng hơn là ngắn gọn).
- Sử dụng camelCase thay vì snake_case.
- Sử dụng 'UpperCamelCase' cho `Objects` và `Protocols`, 'lowerCamelCase' cho những thứ còn lại.
- Đảm bảo sự rõ ràng và hiệu quả bằng cách sử dụng những từ cần thiết, tránh sử dụng các từ không cần thiết và lặp lại.
- Sử dụng tên theo vai trò, không theo kiểu dữ liệu (vd: **numberOfLists** thay vì ~~intCounter~~) có lẽ vậy, câu này hơi khó hình dung 🥶.
- Bắt đầu tên của Factory Methods với `make`, vd: `x.makeIterator()`.
- Đặt tên cho phương thức dựa trên tác động của chúng.
    - Thêm đuôi '-ed' hoặc '-ing' cho phương thức non-mutating
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
    - Tên phương thức (danh từ) thì thêm `form` đằng trước, vd: `y.formUnion(z)` ; `c.formSuccessor(&i)`.

    - Kiểu bool nên được đặt như một sự khẳng định, vd: `isEmpty()`
    - Các Protocol với mục đích để diễn tả đối tượng nên được đặt là một danh từ.
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
    - Các Protocol với mục đích để diễn tả khả năng của đối tượng nên kết thúc bằng '-able', 'ible'.
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
- Sử dụng cách đặt tên của hệ thống, vd: `viewDidLoad()` chẳng hạn.
- Ưu tiên sử dụng methods và properties hơn là các hàm tự do, vd: `user.login()`, ~~login(user)~~.
- Đặt cùng tên cho các phương thức có cùng ý nghĩa.
- Tránh việc định nghĩa nhiều phương thức trùng tên với kiểu trả về khác nhau (ở đây khắc phục với **Generics**)
> Generics là kiểu đại diện cho bất kỳ kiểu dữ liệu trong Swift.

- Sử dụng tốt tên tham số miêu tả, vd: `func calculateArea(width: Double, height: Double)`.
- Sử dụng các tham số có giá trị mặc định (nếu có thể) để làm giảm bớt số lượng tham số khi gọi hàm, vd: `func printName(firstName: String, lastName: String = "Hieu")`.

### Delegates
> When creating custom delegate methods, an unnamed first parameter should be the delegate source.

Ưu tiên:
```swift
  func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
  func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```
Không ưu tiên:
```swift
  func didSelectName(namePicker: NamePickerViewController, name: String)
  func namePickerShouldReload() -> Bool
```
### Use Type Inferred Context
> Trình biên dịch có thể tự suy luận ra ngữ cảnh để đoạn code ngắn gọn hơn

Ưu tiên:
```swift
  let selector = #selector(viewDidLoad)
  view.backgroundColor = .red
  let toView = context.view(forKey: .to)
  let view = UIView(frame: .zero)
```
Không ưu tiên:
```swift
  let selector = #selector(ViewController.viewDidLoad)
  view.backgroundColor = UIColor.red
  let toView = context.view(forKey: UITransitionContextViewKey.to)
  let view = UIView(frame: CGRect.zero)
```
### Generics
Nên được đặt tên ở dạng UpperCamelCase, khi tên loại không có mối quan hệ sử dụng chữ cái viết hoa như `T`,`U`,`V`.

Ưu tiên:
```swift
  struct Stack<Element> { ... }
  func write<Target: OutputStream>(to target: inout Target)
  func swap<T>(_ a: inout T, _ b: inout T)
```
Không ưu tiên:
```swift
  struct Stack<T> { ... }
  func write<target: OutputStream>(to target: inout target)
  func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```
