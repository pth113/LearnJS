# LearnJS
## Table of Contents
<details>
  <summary>
    <a href="#1-khai-báo-biến">1. Khai báo biến let, var và const</a>
  </summary>
</details>

# `1. Khai báo biến`
<img src="https://preview.redd.it/2rxjxqw43qw41.png?width=1080&crop=smart&auto=webp&s=717464c5dca4767ef4a67c67a4723e8e7dbc3fb2" width="400px"/>
Bảng trên áp dụng khi khai báo lại cú pháp với tên biến trùng với global, còn việc set lại giá trị(không khai báo mới) thì ngoài const không được set, **let** và **var** set lại là biến global sẽ bị thay đổi
Ví dụ khi khi khai báo lại tên biến trong block scope

```
var a1 = 1;
let a2 = 2;
const a3 = 3;
{
    var a1 = 4;
    let a2 = 5;
    const a3 = 6
}
console.log(a1,a2,a3);
```
Output: 4 2 3

Vì var có thể đổi được giá trị trong block scope khi khai báo lại nên nên biến đó ở global là **let** hay **const** thì khai báo lại var sẽ bị lỗi
Ví dụ:
```
var a1 = 1;
let a2 = 2;
const a3 = 3;
{
    var a1 = 4;
    const a2 = 5;
    var a3 = 6
}
```
Error: Uncaught SyntaxError: Identifier 'a3' has already been declared
