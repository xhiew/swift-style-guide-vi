
# Kodeco Swift Style Vietnamese Version 🇻🇳(Unofficial) 

Đây chỉ là phần tóm tắt + một số ví dụ của mình sau khi mình tìm hiểu [The Official Kodeco Swift Style Guide](https://github.com/kodecocodes/swift-style-guide.git). ← Bấm vào nếu bạn muốn tìm hiểu chi tiết.

## Table of Contents

* [Correctness](#correctness)
* [Using SwiftLint](#using-swiftlint)
* [Naming](#naming)
  * [Prose](#prose)
  * [Delegates](#delegates)
  * [Use Type Inferred Context](#use-type-inferred-context)
  * [Generics](#generics)
  * [Class Prefixes](#class-prefixes)
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
- Sử dụng tên theo vai trò, không theo kiểu dữ liệu (vd: **numberOfLists** thay vì *intCounter*).
- Bắt đầu tên của Factory Methods với `make`, vd: `x.makeIterator()`.
- 
