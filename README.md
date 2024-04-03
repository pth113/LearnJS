# LearnJS
## Table of Contents
<details>
  <summary>
    <a href="#1-khai-báo-biến">1. Khai báo biến let, var và const</a>
  </summary>
</details>
<details>
  <summary>
    <a href="#2-các-cách-khai-báo-function">2. Các cách khai báo function</a>
  </summary>
</details>
<details>
  <summary>
    <a href="#3-tất-cả-đối-tượng-đều-là-tham-chiếu">3. Tất cả đối tượng đều là tham chiếu</a>
  </summary>
</details>
<details>
  <summary>
    <a href="#4-prototype">4. Prototype</a>
  </summary>
</details>

# `1. Khai báo biến`
<img src="https://preview.redd.it/2rxjxqw43qw41.png?width=1080&crop=smart&auto=webp&s=717464c5dca4767ef4a67c67a4723e8e7dbc3fb2" width="400px"/>

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
**Output: 4 2 3**

Với reassign thì sẽ thay đổi biến global(với const thì không reassign được)

Vì var có thể đổi được giá trị trong block scope khi khai báo lại nên tên biến đó ở global là **let** hay **const** thì khai báo lại var sẽ bị lỗi
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
**Error: Uncaught SyntaxError: Identifier 'a3' has already been declared**

# `2. Các cách khai báo function`
## 2.1 Khai báo tường minh
```
function Add(num1,num2){
    let sum = num1+ num2; 
    return sum; 
}
let res = Add(7,8);
console.log(res); // 15
``` 
## 2.2 Khai báo khuyết danh
```
let add = function (num1,num2){
    let sum = num1+ num2; 
    return sum;
}(8,9);
console.log(add); // 17 
```
Function khai báo xong chạy luôn
```
(function (num1,num2){
    let sum = num1+num2; 
    console.log(sum); //17
})(8,9);
```
## 2.3 Khai báo arrow function
```
var add = (num1, num2)=> num1+num2; 
let res = add(5,2);
console.log(res); // 7 
```
Không tham số
```
let greet = () => console.log('hey');
```
Có nhiều dòng lệnh
```
let greet = () => {
    console.log('hey:');
    console.log('QuangTV');
}
```
## 2.4 Khai báo function constructor
```
var add = Function('num1','num2','num1++;return num1+num2');
let res = add (7,8);
console.log(res); // 16
```

Hàm không có return hoặc dùng return; thì sẽ trả về undefined

# `3. Tất cả đối tượng đều là tham chiếu`
Trong JS tất cả object đều sử dụng tham chiếu
Ví dụ
```
var foo = {bas: 123};
var bar = foo;
bar.bas = 456;
console.log(foo.bas);
```
**Output: 456**

Muốn copy object thì phải làm như vậy
```
var foo = {bas: 123};
var bar = {bas: foo.bas};
bar.bas = 456;
console.log(foo.bas);
```
**Output: 123**

# `4. Prototype`
### 4.1. __proto__ và prototype

