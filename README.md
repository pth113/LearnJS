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
<details>
  <summary>
    <a href="#5-mô-hình-module">5. Mô hình module</a>
  </summary>
</details>
<details>
  <summary>
    <a href="#6-class">6. Class</a>
  </summary>
</details>
<details>
  <summary>
    <a href="#7-hiểu-về-this">7. Hiểu về this</a>
  </summary>
</details>
<details>
  <summary>
    <a href="#8-first-class-và-closure">8. First class và closure</a>
  </summary>
</details>
<details>
  <summary>
    <a href="#9-promise">9. Promise</a>
  </summary>
</details>
<details>
  <summary>
    <a href="#10-asyncawait">10. Async/Await</a>
  </summary>
</details>
<details>
  <summary>
    <a href="#11-javascript-runtime-environment">11. Javascript Runtime Environment</a>
  </summary>
</details>

## `1. Khai báo biến`
<img src="https://preview.redd.it/2rxjxqw43qw41.png?width=1080&crop=smart&auto=webp&s=717464c5dca4767ef4a67c67a4723e8e7dbc3fb2" width="400px"/>

Ví dụ khi khai báo lại tên biến trong block scope

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

## `2. Các cách khai báo function`
### 2.1 Khai báo tường minh
```
function Add(num1,num2){
    let sum = num1+ num2; 
    return sum; 
}
let res = Add(7,8);
console.log(res); // 15
``` 
### 2.2 Khai báo khuyết danh
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
### 2.3 Khai báo arrow function
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
### 2.4 Khai báo function constructor
```
var add = Function('num1','num2','num1++;return num1+num2');
let res = add (7,8);
console.log(res); // 16
```

Hàm không có return hoặc dùng return; thì sẽ trả về undefined

## `3. Tất cả đối tượng đều là tham chiếu`
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

## `4. Prototype`
### 4.1. \_\_proto\_\_ và prototype
**\_\_proto\_\_** là thuộc tính đặc biệt, tất cả đối tượng đều có thuộc tính này

**prototype** là đối tượng đặc biệt, tất cả function đều có đối tượng này, vì cũng là đối tượng nên nó cũng có thuộc tính **\_\_proto\_\_**

**Ví dụ tìm hiểu về \_\_proto\_\_**
```
var test_obj = {};
test_obj.__proto__;
  | Prototype of function Object
    | __proto__ : null
```

Thuộc tính \_\_proto\_\_ của đối tượng chủ yếu dùng để trỏ đến prototype của function
```
var test_obj = {};
function test_func() {};
test_func.prototype.hello = () => console.log('Hello World')
test_obj.__proto__ = test_func.prototype
test_obj.hello()
```
**Output: Hello World**

Khi dùng từ khóa new function thì đối tượng được tạo ra sẽ có \_\_proto\_\_ trỏ đến prototype của function đó
```
function Person(){}
Person.prototype.sayHello = () => console.log('Hello');
var person = new Person();
person.sayHello()
```
**Output: Hello**

Trong đối tượng sẽ không có chứa đối tượng prototype
```
var test_obj = {};
test_obj.prototype;
```
**Output: undefined** 


**Ví dụ tìm hiểu về prototype**
```
function test_func() {}
test_func.prototype;
  | __proto__
    |  Prototype of function Object
      | __proto__ : null
```
Mọi đối tượng, function trong JS đều có liên kết đến prototype của Object, có thể xem function Object là gốc của mọi thể loại trong JS

Tất cả function đều có thuộc tính \_\_proto\_\_ cùng trỏ đến native code, vì là native code nên không xem được bên trong, có thể hiểu là đây là prototype private của function, chỉ có function sử dụng chung với nhau được, đối tượng không dùng được
```
function test_func() {}
test_func.__proto__;
```
**Output: nativecode** 

Đây cũng là \_\_proto\_\_ của function Object

Các function dùng chung prototype private này với nhau ok
```
function test_func() {}
test_func.__proto__.hello = () => console.log('Hello');
Object.hello();
```
**Output: Hello**

Đối tượng sẽ không dùng được
```
function test_func() {}
test_func.__proto__.hello = () => console.log('Hello');
Object.hello();
var test_func_obj = new test_func();
test_func_obj.hello();
```
**Output: Uncaught TypeError: test_func_obj.hello is not a function**

Nếu khai báo dạng function.prototype (ngầm hiểu như là public) sẽ ok
```
function test_func() {}
test_func.prototype.hello = () => console.log('Hello World')
Object.hello();
var test_func_obj = new test_func();
test_func_obj.hello();
```
**Output: Hello World**

### 4.2. Prototype chain
Trong JS tất cả đối tượng đều liên kết đến prototype của Object
Ví dụ
```
function test_func() {}
Object.prototype.hello = () => console.log('Hello World')
var test_func_obj = new test_func();
test_func_obj.hello();
```
Khi test_func_obj gọi function hello() thì nó xem trong object test_func_obj có key hello không, nếu không có thì nó vào \_\_proto\_\_ (đang trỏ tới prototype của function test_func) kiểm tra xem có không? Thấy không có thì nó vào \_\_proto\_\_ của function test_func (đang trỏ tới prototype của function Object) và tìm thấy key hello nên thực thi

Ưu điểm của prototype là tiết kiệm bộ nhớ vì dùng chung cho các function và object, không nằm trong object

Bất lợi là prototype chỉ lợi cho việc đọc, không lợi cho việc ghi vì làm mất liên kết prototype chain

Ví dụ:
```
function foo() {}
foo.prototype.bar = 123;

var bas = new foo();
var qux = new foo();

bas.bar = 456;
console.log(bas.bar) // 456
console.log(qux.bar) // 123
```

**Lưu ý:**
Prototype trước khi có ES6 thì dùng cho việc kế thừa code, thiết lập phức tạp. Từ ES6 đã hỗ trợ class nên không cần dùng prototype kế thừa nữa. Nên prototype hiện tại để dùng chung các function tiết kiệm bộ nhớ trong một số trường hợp

## `5. Mô hình module`
```
const CounterModule = (function() {
  // Biến riêng tư, không thể truy cập từ bên ngoài module
  let count = 0;

  // Phương thức riêng tư, chỉ có thể gọi bên trong module
  function increment() {
    count++;
  }

  // Phương thức riêng tư, chỉ có thể gọi bên trong module
  function decrement() {
    count--;
  }

  // Phương thức riêng tư, chỉ có thể gọi bên trong module
  function getCount() {
    return count;
  }

  // Trả về một object chứa các phương thức công khai để tương tác với dữ liệu
  return {
    increment: increment,
    decrement: decrement,
    getCount: getCount
  };
})();

// Sử dụng module
console.log(CounterModule.getCount()); // Output: 0
CounterModule.increment();
CounterModule.increment();
console.log(CounterModule.getCount()); // Output: 2
CounterModule.decrement();
console.log(CounterModule.getCount()); // Output: 1
```

Chúng ta chỉ trả về một object từ hàm này, chứa các phương thức mà chúng ta muốn công khai để tương tác với dữ liệu, như increment(), decrement(), và getCount(). Nhờ đó, các phương thức này vẫn có thể truy cập biến count trong phạm vi của module mà không cần phải lo lắng về việc dữ liệu bị thay đổi từ bên ngoài module.

## `6. Class`
Từ ES6 đã hỗ trợ class 

*Javascript ES6 (EcmaScript 6) là một phiên bản của ngôn ngữ lập trình Javascript. ES6 được Ecma International - một tổ chức chuẩn hóa công nghệ, đưa ra vào năm 2015 nhằm cải thiện và bổ sung thêm tính năng cho Javascript.*

```
class Animal {
  constructor(name) {
    this.name = name;
  }

  // Phương thức của lớp cha
  speak() {
    console.log(`${this.name} makes a sound.`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    // Gọi constructor của lớp cha
    super(name);
    this.breed = breed;
  }

  // Override phương thức của lớp cha
  speak() {
    console.log(`${this.name} barks.`);
  }

  // Phương thức của lớp con
  fetch() {
    console.log(`${this.name} fetches the ball.`);
  }
}

// Tạo một đối tượng Dog
const myDog = new Dog("Buddy", "Labrador");

// Gọi phương thức speak() của lớp con
myDog.speak(); // Output: Buddy barks.

// Gọi phương thức fetch() của lớp con
myDog.fetch(); // Output: Buddy fetches the ball.
```
Class chỉ giúp kết thừa tái sử dụng code, không hạn chế truy cập data như module

## `7. Hiểu về this`
this thuộc về object hoặc global

**This thuộc về global khi nằm trong hàm hoặc ở ngoài hàm**
```
this === window
```
**Output: true**

```
function foo() {
    return this === window;
}
foo()
```
**Output: true**

**This thuộc về object khi nằm trong object**
```
var foo = {};
function bas() {
    if (this === window) {
        console.log('Called from window');
    }
    if (this === foo) {
        console.log('Called from foo');
    }
}
bas(); // Called from window
foo.bas=bas;
foo.bas(); // Called from foo
```

**Với từ khóa new**
```
function foo() {
    console.log('Is window: ', this === window);
}
foo(); // Is window:  true
var aFoo = new foo(); // Is window:  false
```

```
class Animal {
    checkWindow() {
        console.log('Is window: ', this === window);
        console.log('Is animal1: ', this === animal1);
    }
}
var animal1 = new Animal();
animal.checkWindow()
```

**Output:**

**Is window:  false**

**Is animal1:  true**

## `8. First class và closure`
### First-class
Trong JS first-class function định nghĩa là function có thể

**1. Stored in a variable, object, or array**   
   * Store in a variable
     ```
     var fn = function doSomething() {}
     ```
   * Store in an object
     ```
     var obj = { doSomething : function(){} }
     ```
   * Store in an array
     ```
     arr.push(function doSomething() {})
     ```
    
**2. Passed as an argument to a function**
    ```
    doAction(function doSomething(){});
    ```
    
**3. Returned from a function**
   ```
   function doAction(){
     return function(msg) {
       console.log(msg);
     }
   }
   ```

### Closure
A closure must preserve the arguments and variables in all scopes it references
Ví dụ 1:
```
function outside(x) {
  function inside(y) {
    return x + y;
  }
  return inside;
}
fn_inside = outside(3);
result = fn_inside(5); // #=> 8
```
Mặc dù hàm outside đã chạy xong, nhưng vì có tính chất closure nên biến x được bảo tồn vì còn có tham chiếu của biến **result** tới **fn_inside**, trong **fn_inside** thì đang sử dụng biến x

Ví dụ 2:
```
function say43(){
    let num = 42;
    let say = () => console.log(num);
    num++;
    return say;
}
var sayNum = say43();
sayNum(); // 43
sayNum(); // 43
```
num trong hàm say được bảo tồn cho đến lúc gọi, khi gọi thì tham chiếu đến num để show giá trị ra
Biến num sẽ được bảo tồn suốt vì có sayNum tham chiếu
Muốn gọi xong giải phóng biến num luôn thì làm thế này
```
function say43(){
    let num = 42;
    let say = () => console.log(num);
    num++;
    return say;
}
say43()();
```
Không có biến tham chiếu thì hàm say gọi xong thì biến num được free

Ví dụ 4:
```
<html>
	<body>
		<li class="click">link 1 </li>
		<li class="click">link 2 </li>
		<li class="click">link 3 </li>
		<li class="click">link 4 </li>
		<li class="click">link 5 </li>
		<script>
			var add_the_handlers = function (nodes) {
			  var i;
			  for (i = 0; i < nodes.length; i += 1) {
			    nodes[i].onclick = function (e) {
			      alert(i);
			    };
			  }
			};
			var nodes = document.getElementsByClassName("click");
			add_the_handlers(nodes);
		</script>
	</body>
</html>
```
Kết quả alert ra 5 hết vì khi click mới tham chiếu tới biến i
Fix như sau
```
<html>
	<body>
		<li class="click">link 1 </li>
		<li class="click">link 2 </li>
		<li class="click">link 3 </li>
		<li class="click">link 4 </li>
		<li class="click">link 5 </li>
		<script>
			var add_the_handlers = function (nodes) {
			  var i;
			  for (i = 0; i < nodes.length; i += 1) {
			    nodes[i].onclick = ((i) => {
			    	return (e) => alert(i)
			    })(i);
			  }
			}
			var nodes = document.getElementsByClassName("click");
			add_the_handlers(nodes);
		</script>
	</body>
</html>
```
Khi có tham số của function trùng tên với biến bên ngoài thì nó vẫn xem tham số đó là tham trị, không phải là tham chiếu tới biến bên ngoài

## `9. Promise`
Chi tiết xem tại page 120 https://docs.google.com/document/d/1sbM3gLMdbRE390vi1NUfgH8q85oJfvLlzl1QgTWCVnk/edit
* **Promise cơ bản**
  * **Return a new fulfilled promise trong các trường hợp sau**
    * new Promise((resolve, reject) => { resolve('test1')}); hoặc Promise.resolve('test1')

      Return a new fulfilled promise có payload là test1
      ```
      new Promise((resolve, reject) => resolve('test1'))
      ```
      **Output: Promise {\<fulfilled\>: 'test1'}**
    * Trong .then((result) => {})

      Return a new fulfiled promise ko có payload 
    * Trong .then((result) => { return ‘test2’;})

      Return a new fulfiled promise có payload là ‘test2’
      
    Các thằng .then tiếp theo trong chuỗi chỉ thực khi khi nhận ra promise là a new fulfiled promise. Nếu không nó sẽ không thực thi, mà mang có promise cho thằng tiếp theo trong chuỗi xử lý

  * **Return a new rejected promise trong trường hợp sau**

    new Promise((resolve, reject) => { reject('test1')}); hoặc Promise. reject('test1')
    Return a new reject promise với payload là test1 
    ```
    new Promise((resolve, reject) => reject('test1'))
    ```
    **Output: Promise {\<rejected\>: 'test1'}**

    Return a new rejected promise không có payload
    ```
    new Promise((resolve, reject) => reject())
    ```
    **Output: Promise {\<rejected\>: undefined}**
    
    Mấy thằng .catch phía sau chỉ bắt promise vào xử lý khi promise là a new rejected promise
    **Lưu ý:**
    
    Nếu trong .catch mà return sẽ là return ra a new fulfiled promise có payload là giá trị return. 
    Nếu trong .catch mà ko return sẽ là return ra a new fulfiled promise ko có payload.
    ```
    new Promise((resolve, reject) => reject()).catch(() => {})
    ```
    **Output: Promise {\<fulfilled\>: undefined}**

    ```
    new Promise((resolve, reject) => reject()).catch(() => { return 'test1'})
    ```
    **Output: Promise {\<fulfilled\>: 'test1'}**

  * **Finally thì mặc kệ promise là gì thì nó cũng xử lý, xử lý xong, nó sẽ truyền promise trước nó cho thằng sau nếu có thằng sau đang đứng**
    
    ```
    Promise.resolve('Reject DATA!')
      .then(result=> {
	console.log('[1] then', result);
	return 'new Reject DATA!';
      })
      .finally(()=>{
        console.log('[1] finally');
	return 'return cho vui, chu ko lam gi';
      })
      .then(result=>{
	console.log('[2] then: ', result);
	return 'the end';
      })
    ``` 
    Kết quả
    ```
    [1] then Reject DATA!
    [1] finally
    [2] then:  new Reject DATA!
    ```

* **Thực hành Promise**
  
  Cho biết kết quả đoạn code sau
  ```
  Promise.reject( 'Reject DATA!' )
  .then((result) => {
    console.log( '[1] then', result ); // won't be called
    return '[2] then payload';
  })
  .finally(() => {
    console.log( '[1] finally' ); // first finally will be called
    return '[1] finally payload';
  })
  .then((result) => {
    console.log( '[2] then', result ); // won't be called
    return '[2] then payload';
  })
  .catch((error) => {
    console.log( '[1] catch', error );  // first catch will be called
    return '[1] catch payload';
  })
  .catch((error) => {
    console.log( '[2] catch', error );  // won't be called
    return '[2] catch payload';
  })
  .then((result) => {
    console.log( '[3] then', result ); // will be called
    return '[3] then payload';
  })
  .finally(() => {
    console.log( '[2] finally' ); // will be called
    return '[2] finally payload';
  })
  .catch((error) => {
    console.log( '[3] catch', error );  // won't be called
    return '[3] catch payload';
  })
  .then((result) => {
    console.log( '[4] then', result ); // will be called
    return '[4] then payload';
  });
  ```
  Kết quả
  ```
  [1] finally
  [1] catch Reject DATA!
  [3] then [1] catch payload
  [2] finally
  [4] then [3] then payload
  Promise {<fulfilled>: '[4] then payload'}
  ```
    
* **Promise.all() and Promise.race()**
  Việc dùng promise với cách cơ bản như trên cũng không xử lý được vấn đề như callback hell
  ```
  const a = () => new Promise(resolve => {
    setTimeout(() => resolve('result of a()'), 1000); // 1s delay
  });
  const b = () => new Promise(resolve => {
    setTimeout(() => resolve('result of b()'), 500); // 0.5s delay
  });
  const c = () => new Promise(resolve => {
    setTimeout( () => resolve('result of c()'), 1100); // 1.1s delay
  });

  // call a(), b(), and c() in series
  a().then((result) => {
    console.log( 'a() success:', result );
    b().then((result) => {
        console.log('b() success:', result);
        c().then((result) => {
            console.log('c() success:', result);
        });
    });
  })
  .catch((error) => {
    console.log('a() error:', error);
  });
  ```
  Muốn gọn thì phải dùng Promise với Async, còn chạy parralel thì dùng Promise.all() hoặc Promise.race()
  * **Promise.all()**
    ```
    const a = () => new Promise(resolve => {
      setTimeout(() => resolve('result of a()'), 1000); // 1s delay
    });
    const b = () => new Promise(resolve => {
      setTimeout(() => resolve('result of b()'), 500); // 0.5s delay
    });
    const c = () => new Promise(resolve => {
      setTimeout(() => resolve( 'result of c()'), 1100); // 1.1s delay
    });

    // resolve once a(), b(), c() resolves
    Promise.all([a(), b(), c(), {key: 'I am plain data!'}])
    .then((data) => {
      console.log( 'success: ', data );
    })
    .catch((error) => {
      console.log('error: ', error);
    });
    ```
    Kết quả
    ```
    "success: ", ["result of a()", "result of b()", "result of c()", [object Object] {
      key: "I am plain data!"
    }]
    ```
    Nếu có reject thì báo lỗi, thằng nào chạy ko lỗi thì chạy hết, error chỉ báo lỗi thằng đầu tiên reject
  * **Promise.race()**
 
    Chạy parallel, thằng nào chạy xong trước/error thì báo về, lấy kết quả đó hiển thị
    ```
    const a = () => new Promise(resolve => {
      setTimeout(() => resolve('result of a()'), 1000); // 1s delay
    });
    const b = () => new Promise(resolve => {
      setTimeout(() => resolve('result of b()'), 500); // 0.5s delay
    });
    const c = () => new Promise(resolve => {
      setTimeout(() => resolve('result of c()'), 1100); // 1.1s delay
    });

    // race a(), b(), c()
    Promise.race([ a(), b(), c()])
    .then((data) => {
      console.log( 'success: ', data );
    })
    .catch((error) => {
      console.log( 'error: ', error );
    });
    ```
    Kết quả
    ```
    success: result of b()
    ```
    Giả sử a() reject thì vẫn thông báo là 
    ```
    success: result of b()
    ```

## `10. Async/Await`
  
  Muốn dùng await trong 1 hàm, thì hàm đó phải khai báo là async. Hàm async sẽ tự động trả về là một promise
  ```
  const a = () => new Promise(resolve=> {
    setTimeout(() => resolve('result of a()'), 1000); // 1s delay
  });
  const b = () => new Promise((resolve, reject) => {
    setTimeout(() => resolve('result of b()'), 500); // 0.5s delay
  });
  const c = () => new Promise(resolve => {
    setTimeout(() => resolve('result of c()'), 1100); // 1.1s delay
  });
  const doJobs = async () => {
    try {
      var resultA = await a();
      var resultB = await b();
      var resultC = await c();
      return [ resultA, resultB, resultC ];
    } catch( error ) {
      return [ null, null, null ];
    }
  };

  // doJobs() returns a promise
  doJobs().then((result) => {
    console.log('success:', result);
  })
  .catch((error) => {
    console.log('error:', error);
  });

  // normal flow
  console.log('I am a sync operation!');

  ```
  Kết quả
  ```
  I am a sync operation!
  success: ["result of a()", "result of b()", "result of c()"]
  ```

Tham khảo ví dụ: https://ui.dev/async-javascript-from-callbacks-to-promises-to-async-await

## `11. Javascript Runtime Environment`

![image](https://github.com/pth113/LearnJS/assets/7723612/5817b5a1-f9f7-4275-bee3-780a81740f21)

Có 3 queue là call stack, micro và stack queue
Thứ tự xử lý

* Call Stack trong Engine LIFO
* Micro (Promise, ...)
* Stack queue web API (settimeout, fetch, ...)

Có 2 loại task queue đó là macroTask Queue và microTask Queue, macroTask còn có tên gọi khác là Task Queue và microTask cũng có tên gọi là Job Queue. Hai task này nhìn chung là như nhau , những task trong Job Queue ưu tiên hơn những task có trong Task Queue. Thứ tự chạy của các task trong hai queue này như sau:

Ở mỗi chu kỳ của event-loop, nó sẽ check xem call stack có rỗng không, nếu rỗng:

Lấy một task cũ nhất trong Task Queue ra chạy.
Sau khi task trong Task Queue chạy xong, event loop sẽ nhìn vào Job Queue (chứa microTask) và chạy hết tất cả các tasks trong này.
Kết thúc chu kỳ event-loop quay lại bước 1 cho đến khi chay hết task queue.

