# Function Arguments

`arguments` là một object đặc biệt trong function của JavaScript, nó chứa các params của function và share chung bộ nhớ với chúng, vì thế khi thay đổi giá trị của `agruments` thì các params cũng được thay đổi theo.
Ví dụ:
```javascript
function b(x, y, a) {
  arguments[2] = 10;
  console.log(a);
}
b(1, 2, 3); //10
```
# Global window
# Javascript context
Javascript dùng biến `this` để mô tả ngữ cảnh.
## Global Context
Khi được dùng ở Global Context, bên ngoài các function, `this` đại diện cho global context.
```javascript
var a = 5;
if(this.a === a) {
	console.log(a); //5
};
```
Trên browser, `this` chính là đối tượng window.
```javascript
console.log(this.document === document); // true  
// In web browsers, the window object is also the global object:  
console.log(this === window); // true  
this.a = 37; console.log(window.a); // 37
```
Nếu một hàm được gọi bình thường, `this` sẽ trỏ đến đối tượng thuộc global context. Thỉnh thoảng ta muốn `this` trỏ đến 1 context nào đó, đấy là lúc dùng **call** và **apply**. 
**call** dùng các biến theo thứ tự truyền vào, **apply** truyền biến bằng mảng (array)
```javascript
function add(b) { 
	return  this.a + b; 
} 
var o = { a: 5 } 
add.call(o, 5) === 10  // --> true 
add.apply(o, [5]) === 10  // --> true
```

Khi chúng ta truyền tham số đầu tiên vào hàm `call` là `null` hay `undefined` thì JavaScript sẽ tự động truyền global object vào hàm được gọi chứ không truyền `null` hay `undefined`. Vì vậy, đoạn code này sẽ in ra object `window` thay vì `null` nhé.

```javascript
function a() {
  console.log(this);
}
a.call(null); //window object
```

# Array Length
Trong JavaScript [Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#Relationship_between_length_and_numerical_properties), khi set một thuộc tính cho một array, nếu thuộc tính đó là một `valid array index` (chỉ mục của mảng hợp lệ) thì `length` của array đó sẽ được tính toán lại.
```javascript
var arr = [];
arr[0]  = 'a';
arr[1]  = 'b';
arr.foo = 'c';
console.log(arr.length); //2
```

# Boolean Type Conversion
Operator `+` sẽ có độ ưu tiên cao hơn operator `>` vì thế `+` sẽ được thực hiện trước. Khi `+` number với boolean hoặc 2 boolean với nhau, JavaScript sẽ chuyển đổi boolean về number, `true -> 1` và `false -> 0`, vì vậy ta có thể viết thành `1 + 0 > 2 + 1`, kết quả `1 > 3` là `false`.
```javascript
console.log(true + false > 2 + true); //false
```

# Function Precedence
Theo [operator precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence), các lời gọi function sẽ có độ ưu tiên cao hơn operator `+`, vì vậy `'foo'.split('')` sẽ thực hiện trước, ta có thể viết lại như sau `[] + [] + ['f', 'o', 'o']`, JavaScript sẽ chuyển đổi array sang string trước khi `+` nên ta có kết quả là `'f,o,o'`.
```javascript
console.log([] + [] + 'foo'.split('')); //f,o,o
```

# In Operator
[`in operator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/in) sẽ kiểm tra 1 thuộc tính có phải là của object đó hay không, một array cũng là một object và có các thuộc tính index (`myArray[2] === myArray['2']`), vậy kết quả là `true`.

```javascript
var myArr = ['foo', 'bar', 'baz'];
console.log('2' in myArr); //true
```

# Labeling
Trong JavaScript cũng có [`labled statement`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label) nên đoạn code trên không có lỗi gì đâu nhé. Phần tử `baz` và `bar` không có trong object `foo` nên ta có thể viết lại như sau `undefined + undefined + 1` và cho kết quả là `NaN`.
```javascript
var bar = 1,
    foo = {};

foo: {
    bar: 2;
    baz: ++bar;
};

console.log(foo.baz + foo.bar + bar); //NaN
```
# Named Functions
Chúng ta có thể khai báo `function expression` cùng với `named function` và dùng `named function` bên trong function đó để gọi tới chính nó. Nhưng chúng ta không thể dùng `named function` bên ngoài được, điều đó có nghĩa khi gọi `bar()` sẽ gây ra lỗi `ReferenceError`.
```javascript
var foo = function bar() {
    console.log(foo === bar);
};
foo(); //true
bar(); //ReferenceError
```
# Postfix Operation
Postfix Operator `--` chỉ thực hiện với biến, trong trường hợp này `'1'` không phải là biến nên sẽ gây ra lỗi `TypeError`. Nếu đoạn code trên được viết thành `'1' - - '1'` (chú ý có space giữa 2 dấu `-`) thì sẽ cho kết quả là `2`.
```javascript
console.log('1' -- '1'); //TypeError
```
# String Constructor
Khi gọi String [constructor function](https://duthaho.com/prototype-in-javascript-2/) với từ khóa `new` sẽ cho kết quả là một object, trong khi đó, không có từ khóa `new` kết quả sẽ là một primitive string, so sánh `===` giữa 2 kiểu dữ liệu khác nhau sẽ cho kết quả là `false`.
```javascript
console.log(new String('hello') === String('hello'));//false
```

# Json
`b = JSON.parse(JSON.stringify(a))` sẽ thực hiện [deep copy](https://duthaho.com/blogs/js-cloning-array) trên object `a`. Tất cả các thuộc tính là các kiểu dữ liệu nguyên thủy (Boolean, String, Number) sẽ được copy một cách chính xác, tuy nhiên đối với các thuộc tính có giá trị không phải là giá trị JSON (Date, undefined, Function, và không phải kiểu dữ liệu nguyên thủy) sẽ không được copy đúng. Trong ví dụ trên, object Date sẽ được chuyển đổi sang string, chúng ta có thể xem thêm về [JSON.stringify](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description) để hiểu rõ hơn.
```javascript
const a = {
  stringField: 'Joe',
  numberField: 123,
  dateField: new Date('1995-12-17T03:24:00'),
  nestedField: { field: 'Nested' }
};

const b = JSON.parse(JSON.stringify(a));

console.log(
  a.stringField === b.stringField, //true 
  a.numberField === b.numberField, //true 
  a.dateField === b.dateField, //false 
  a.nestedField.field === b.nestedField.field //true
);
```
# Object Assign
`b = Object.assign({},a);` sẽ thực hiện một `shallow copy` trên object `a`, bất kỳ thuộc tính nào của `b` là object đều tham chiếu đến cùng thuộc tính trong `a`. Vì vậy khi chúng ta thay đổi nested field của `b`, thì nested field của `a` cũng thay đổi theo.
```javascript
const a = {
  stringField: 'Joe',
  nestedField: { field: 'Nested' },
  functionField: () => 'aReturn'
};

const b = Object.assign({}, a);

b.stringField = 'Bob';
b.nestedField.field = 'Changed';
b.functionField = () => 'bReturn';

console.log(
  a.stringField, //Joe
  a.nestedField.field, //Changed 
  a.functionField() // aReturn
);
```

# Object Keys
`Object.keys` sẽ chuyển đổi keys của object sang string `['1', '2', '3']` và `Object.values` sẽ giữ nguyên values của object `[1, 2, 3]`.
```javascript
const obj = {
  1: 1,
  2: 2,
  3: 3
};

console.log(Object.keys(obj), Object.values(obj)); 
//["1", "2", "3"] [1, 2, 3]
```
# Operator Associativity
Các toán tử  `<`  và  `>`  có cùng độ ưu tiên và sẽ được thực hiện từ trái qua phải.

Dòng đầu tiên chúng ta có thể viết lại như sau  `(1 < 2) < 3`,  `1 < 2`  được thực hiện trước và trả về  `true`, sau đó thực hiện  `true < 3`, khi so sánh với number, boolean sẽ được chuyển đổi sang number,  `true`  trở thành  `1`, vậy  `true < 3`  cho kết quả  `true`.

Ở dòng thứ hai  `(3 > 2) > 1`,  `(3 > 2)`  cũng được thực hiện trước và trả về  `true`, tuy nhiên sau đó  `true > 1`  sẽ được chuyển đổi thành  `1 > 1`  và cho kết quả  `false`.
```javascript
console.log(1 < 2 < 3); //true 
console.log(3 > 2 > 1); //false
```

# Promise all
[Promise.all](http://duthaho.com/blogs/js-promise) không quan tâm đến thứ tự thời gian hoàn thành xong các Promise, nó sẽ chờ cho tất cả các Promise hoàn thành xong và kết quả của nó sẽ là một array với thứ tự giữ nguyên với thứ tự của tham số truyền vào.

```javascript
const timer = a => {
  return new Promise(res =>
    setTimeout(() => {
      res(a);
    }, Math.random() * 100)
  );
};

const all = Promise.all([
  timer('first'),
  timer('second')
]).then(data => console.log(data)); /["first", "second"]
```
# Promise race
[Promise.race()](http://duthaho.com/blogs/js-promise) sẽ trả về kết quả của một Promise nào hoàn thành trước. Trong ví dụ trên, `p3` sẽ hoàn thành trước, nó sẽ gọi `reject` sau 10ms và sẽ rơi vào `catch`.
```javascript
const p1 = new Promise((resolve, reject) =>
  setTimeout(resolve, 100, 'Hello')
);

const p2 = new Promise((resolve, reject) =>
  setTimeout(resolve, 120, 'Goodbye')
);

const p3 = new Promise((resolve, reject) =>
  setTimeout(reject, 10, 'Oops!')
);

Promise.race([p1, p2, p3])
  .then(result => console.log(result)) //Something went wrong...
  .catch(reason => console.log('Something went wrong...'));
```

# Prototype 
Khi gọi đến thuộc tính hay phương thức của một object, đầu tiên nó sẽ tìm trong object trước, nếu không tìm thấy, mới tiếp tục tìm trong [Prototype](http://duthaho.com/blogs/prototype-in-javascript-2) của nó.
```javascript
function Dog(name) {
  this.name = name;
  this.speak = function() {
    return 'woof';
  };
}

const dog = new Dog('Pogo');

Dog.prototype.speak = function() {
  return 'arf';
};

console.log(dog.speak()); //woof
```

# Reduce
Hàm  `reduce`  của array cho phép chúng ta truyền vào một giá trị ban đầu ở tham số thứ hai, trong trường hợp này là  `1`  và ta có các bước tính toán sau:

1 + 1 * 1 = 2  
2 + 2 * 2 = 6  
6 + 6 * 3 = 24  
24 + 24 * 4 = 120
```javascript
const arr = [
  x => x * 1,
  x => x * 2,
  x => x * 3,
  x => x * 4
];

console.log(arr.reduce((agg, el) => agg + el(agg), 1)); //120
```

# Arrow Functions
`this` trong một `Arrow functions` không được `bind` như trong một function bình thường mà `this` được thừa hưởng từ scope bên ngoài của nó (`lexical scoping`). Đó là lý do tại sao `this` trong function `getBreed` không phải là object `dog` mà là global object, ở trình duyệt là window object, nên `this.breed` trả về `undefined`.
```javascript
let dog = {
  breed: 'Border Collie',
  sound: 'Wooh',
  getBreed: () => {
    return this.breed;
  },
  getSound: function() {
    return this.sound;
  }
};
console.log(dog.getBreed(), dog.getSound()); //undefined, Wooh
```

# Closure - Hoisting
`Closure` là khi một inner function có thể ghi nhớ và truy cập đến các thành phần ở scope của outer function, thậm chí outer function đã thực thi xong. Ở trong ba ví dụ trên, thì function `b` vẫn ghi nhớ và truy cập đến biến `a` ở bên ngoài scope của nó, mặc dù các outer function đã được thực thi xong.
```javascript
function withVar() {
  const b = () => a;
  var a = 24;
  return b;
}

function withLet() {
  const b = () => a;
  let a = 24;
  return b;
}

function changingValue() {
  let a = 24;
  const b = () => a;
  a = 42;
  return b;
}

console.log(withVar()()); // 24
console.log(withLet()()); // 24
console.log(changingValue()()); // 42
```
# Fetch
Tùy vào environment bạn chạy đoạn code trên mà kết quả sẽ khác nhau. Nếu chạy trên [trình duyệt đã hổ trợ fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API#Browser_compatibility) thì kết quả là `the fetch function`, còn nếu chạy trên các trình duyệt cũ như `IE` hoặc trên môi trường `node`, chúng ta sẽ thấy lỗi `ReferenceError`.
```javascript
console.log(fetch); //It depends
```

# Floating-Point
Như chúng ta đã biết trong hệ thập phân, chúng ta không thể biểu diễn chính xác  `1/3`  dưới dạng dấu phẩy động (`floating-point`). Kết quả của  `0.333 + 0.333 + 0.333`  không thể nào bằng  `1`  được.

Tương tự, trong máy tính các số được biểu diễn dưới dạng nhị phân. Đôi khi chúng chỉ biểu diễn được gần đúng số thực tế chứ không thể biểu diễn một các chính xác được, ví dụ như  `0.1`,  `0.2`  hay  `0.3`. Điều này dẫn đến các kết quả không mong muốn, trong trường hợp  `0.1 + 0.2`, kết quả ta nhận được là  `0.30000000000000004`.
```javascript
const a = 0.1;
const b = 0.2;
const c = 0.3;

console.log(a + b === c); // false
```

# Date Constructor
Date Constructor `new Date()` trong JavaScript được dùng để tạo một date object, dựa vào tham số đầu vào mà sẽ cho các kết quả khác nhau. Nếu tham số là một string, JavaScript sẽ tự động parse chuỗi string thành ngày tương ứng, trong trường hợp này `"2019,1,1"` được parse thành ngày `1/1/2019`, nếu tham số là ba numbers thì ố thứ nhất là năm, số thứ hai là tháng, số thứ ba là ngày, tuy nhiên cần chú ý, tháng ở đây được bắt đầu từ `0`, vậy truyền vào `1` có nghĩa là tháng `2`.
```javascript
let a = new Date('2019,1,1').toLocaleDateString();
let b = new Date(2019, 1, 1).toLocaleDateString();
console.log(a, b); // 1/1/2019 2/1/2019
```

# Function Hoisting
Cơ chế  `hoisting`  trong JavaScript được áp đụng khi khai báo biến (`variable declaration`) và khai báo function (`function declaration`), trừ khi gán một function cho biến (`function expression`).

Function declaration sẽ có độ ưu tiên hơn variable declaration khi  `hoisting`, vì thế function  `bar`  sau khi  `hoisted`  sẽ giống thế này:

```javascript
function bar() {
  console.log('first');
}

bar(); // 'first'

bar = function() {
  console.log('second');
};

bar(); // 'second'
```

Trong trường hợp chúng ta có các khai báo trùng lặp (`duplication declaration`) hoặc gặp một phép gán (`assignment`) thì giá trị sẽ của biến hay function sẽ được thay thế, Vì vậy function  `foo`  sẽ giống thế này:

```javascript
function foo() {
  console.log(1);
}

function foo() {
  console.log(3);
}

foo(); // 3

foo = function() {
  console.log(2);
};

foo(); // 2
```

# Logical Operators
Toán tử `&&` sẽ dừng và trả về kết quả `false` nếu gặp bất kỳ điều kiện nào sai, vì thế `notifications !== 1 && 's'` sẽ trả về `false`. Nếu bạn muốn ví dụ trên chạy đúng như ý muốn, ta có thể dùng ternary operator: `notifications !== 1 ? 's' : ''`.
```javascript
const notifications = 1;

console.log(
  `You have ${notifications} notification${notifications !==
    1 && 's'}`
); // You have 1 notificationfalse
```
# Number With Dots
Khi muốn truy cập đến một thuộc tính hay method của một object ta có thể dùng dot notation (một dấu `.`), còn đối với number, ta có thể dùng hai dấu `.`, vì khi dùng một dấu `.` thì JavaScript sẽ bị nhầm lẫn với decimal number. Trong trường hợp này, `1..n` sẽ truy cập đến thuộc tính `n` của number `1`, nó trả về `undefined`. Một ví dụ cụ thể là khi gọi `1..toString()` sẽ cho kết quả là `"1"`.

```javascript
const n = 5;

console.log(1..n); // undefined
```

# Null - Undefined - NaN
Trong JavaScript, khi sử dụng Triple Equals (`===`) thì `null` và `undefined` chỉ cho kết quả `true` khi so sánh với chính nó, `NaN` thì luôn cho kết quả `false` khi so sánh với bất kỳ object nào, kể cả chính nó, còn `[NaN]` là một array bình thường chỉ chứa một phần tử là `NaN`.
```javascript
const compare = a => a === a;

console.log(compare(null)); //true 
console.log(compare(undefined)); //true 
console.log(compare(NaN)); // false
console.log(compare([NaN])); // true
```

# Object Freeze
Để tránh bất kỳ sự thay đổi nào trên các thuộc tính của một object, ta có thể dùng hàm `Object.freeze`, tuy nhiên hàm này chỉ có thể thực hiện `shallow freeze` trên object đó mà thôi, điều đó có nghĩa bất kỳ sự thay đổi nào trên các thuộc tính của object con đều được cho phép. Trong ví dụ này, chúng ta không thể thay đổi `user.age`, nhưng không có vấn đề gì khi thay đổi `user.pet.name`. Nếu chúng ta không muốn thay đổi bất kỳ thuộc tính nào của object, có thể dùng đệ quy `Object.freeze` cho các thuộc tính con hoặc dùng các chức năng `deep freeze` của các thư viện có sẵn.
```javascript
const user = {
  name: 'lao Hac',
  age: 69,
  pet: {
    type: 'cho',
    name: 'vang'
  }
};

Object.freeze(user);

user.pet.name = 'shiba';

console.log(user.pet.name); // shiba
```
# Object Values
Nếu key trong object là number thì `Object.values` sẽ sắp xếp lại các value theo thứ tự, với key không phải number thì thứ tự vẫn được giữ nguyên.
```javascript
const scrambled = {
  2: 'e',
  5: 'o',
  1: 'h',
  4: 'l',
  3: 'l'
};

const result = Object.values(scrambled).reduce(
  (agg, el) => agg + el,
  ''
);

console.log(result); //hello
```
# Operation Precedence
Toán tử `++` (Postfix Increment) được ưu tiên hơn toán tử `+`, đầu tiên toán tử Postfix Increment sẽ chuyển đổi `b` từ string `'4'` sang number `4`, sau đó nó sẽ chờ phép toán `4 + 3` thực hiện xong mới thực hiện tăng `b` lên một đơn vị.
```javascript
let b = '4';

console.log(b++ + 3, b); // 7 5
```
# Reference Types
Khi chúng ta truyền tham số vào một function là một kiểu dữ liệu tham chiếu (`reference types`) thì những thay đổi đối với tham số bên trong hàm sẽ thay đổi đến chính object chúng ta truyền vào. Vì vậy khi xóa một thuộc tính của tham số `input` cũng chính là xóa luôn thuộc tính của object `a`.
```javascript
const a = { something: 1, someOtherThing: 2 };

const deleteSomething = input => {
  delete input.something;
  return input.something;
};

const result = deleteSomething(a);

console.log(result); // undefined
```

# Semicolon
Mặc dù trông có vẻ hai function trong ví dụ trên hoàn toàn giống nhau. Nhưng JavaScript có một số quy tắc để tự động thêm vào dấu  `;`  (semicolon) sau một số câu lệnh, mà cụ thể ở đây là câu lệnh  `return`.

Ở function  `foo`, câu lệnh  `return`  và dấu  `{`  mở đầu một code block nằm trên cùng một dòng, vì vậy JavaScript chỉ thêm các dấu  `;`  vào sau các dấu  `}`  đóng code block:

```javascript
const foo = () => {
  return {
    foo: 'foo'
  };
};
```

Tuy nhiên, với function  `bar()`  lại là một câu chuyện khác, câu lệnh  `return`  nằm riêng lẽ trên một dòng, vậy nên JavaScript sẽ tự động thêm dấu  `;`  vào sau câu lệnh  `return`  này:

```javascript
const bar = () => {
  return;
  {
    bar: 'bar';
  }
};
```

Nó làm cho function  `bar`  giờ có thể viết như thế này:

```javascript
const bar = () => {
  return;
};
```
```javascript
const foo = () => {
  return {
    foo: 'foo'
  }
}

const bar = () => {
  return
  {
    bar: 'bar'
  }
}

console.log(foo(), bar()); //{ foo: "foo" } undefined
```
# Set Timeout
Rõ ràng `1` và `4` sẽ được in ra đầu tiên vì `console.log()` mà không có delay. `2` sẽ được in ra sau `3` vì `2` bị delay 1 giây còn `3` bị delay sau 0 giây. Có một điểm chú ý là vì sao `3` bị delay là `0 giây`, nhưng lại được in ra sau `4`? vì `callback` trong `setTimeout` sẽ được đẩy vào `event queue` và nó chỉ được gọi sau khi `call stack` rỗng. Nếu bạn chưa rõ các khái niệm này, đọc thêm về [JS concurrency model/event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop).
```javascript
(function() {
  console.log(1);
  setTimeout(function() {
    console.log(2);
  }, 1000);
  setTimeout(function() {
    console.log(3);
  }, 0);
  console.log(4); 
})();
// 1, 4, 3, 2
```
# Template Literals
Trong JavaScript, mảng rỗng `[]` và function là `truthy`. Nhưng chú ý ở ví dụ trên, function này được thực thi và nó trả về `false`.
```javascript
const output = `Soon we must all choose between what is ${
  [] ? 'right' : 'wrong'
} and what is ${(() => false)() ? 'difficult' : 'easy'}`;

console.log(output); //Soon we must all choose between what is right and what is easy
```
# Typeof
Object, Array và Number đều là các [Function Constructor](https://duthaho.com/blogs/prototype-in-javascript-2), chúng dùng để tạo ra các object với từ khóa `new`.
```javascript
console.log(typeof Object, typeof Array, typeof Number); 
//object object number
```
# Variable Hoisting
```javascript
var x = 5;

(function() {
  console.log(x);
  var x = 10;
  console.log(x);
})();
```
Biến  `x`  sẽ được  `hoist`  bên trong function, chúng ta có thể xem function được thực thi như sau:

```javascript
var x = 5;

(function() {
  var x;
  console.log(x);
  x = 10;
  console.log(x); //undefined 10
})();
```
# Bind - Call
Chúng ta đã biết cả hai hàm  `call`  và  `bind`  đều được dùng để thay đổi  [this context](https://duthaho.com/blogs/js-this-context)  của hàm.

Tuy nhiên, với  `call`  thì hàm sẽ được gọi ngay lập tức, còn  `bind`  thì nó sẽ trả về một hàm mới với context mình truyền vào chứ không gọi ngay lúc đó.
```javascript
const person = { name: 'duthaho' };

function sayHi(age) {
  return `${this.name} is ${age}`;
}

console.log(sayHi.call(person, 69)); // duthaho is 69 
console.log(sayHi.bind(person, 69)); // function
```
# Function Constructor
Khi gọi một [Function Constructor](https://duthaho.com/blogs/prototype-in-javascript-2), `this` sẽ được tạo và trả về một cách ngầm định nếu chúng ta gọi bằng từ khóa `new`, nếu không `this` sẽ không được tạo và sẽ là global window object (trong trình duyệt).
```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const ti = new Person('du', 'ti');
const teo = Person('du', 'teo');

console.log(ti); //Person {firstName: "du", lastName: "ti"} 
console.log(teo); //undefined
```
# Immediately Invoked Function
`immediately invoked function (IIFE)` là hàm được gọi ngay lập tức lúc khai báo, có cú pháp như trong ví dụ trên `(() => 0)()`. Ở đây hàm `sayHi` trả về một `IIFE` và nó được gọi ngay lập tức khi hàm `sayHi` được gọi, vì thế `sayHi()` sẽ trả về số `0` với type là `number`.
```javascript
function sayHi() {
  return (() => 0)();
}

console.log(typeof sayHi()); //number
```
# Object Duplicate Keys
Nếu bạn có một object với nhiều keys có cùng tên, thì chúng sẽ được đè lên nhau, giá trị chính là giá trị sau cùng nhưng thứ tự lại là thứ tự đầu tiên của key.
```javascript
const obj = { a: 'one', b: 'two', a: 'three' };
console.log(obj); // {a: "three", b: "two" }
```
# Object String Keys 2
Tất cả các keys của một object đểu được tự động chuyển thành string (trừ  `Symbol`).

Khi một object chuyển sang string, nó có giá trị  `"[object Object]"`, vậy  `a[b] = 123`  có thể viết thành  `a["object Object"] = 123`, tương tự với  `a[c] = 456`  sẽ là  `a["object Object"] = 456`.
```javascript
const a = {};
const b = { key: 'b' };
const c = { key: 'c' };

a[b] = 123;
a[c] = 456;

console.log(a[b]); // 456
``` 
# Object String Keys
Tất cả các keys của một object đểu được tự động chuyển thành string (trừ  `Symbol`). Vì thế  `obj.hasOwnProperty('1')`  cho kết quả  `true`.

Nhưng điều đó không đúng với  `Set`, set phân biệt giữa string và number nên  `set.has('1')`  sẽ trả về  `false`  còn  `set.has(1)`  trả về  `true`.
```javascript
const obj = { 1: 'a', 2: 'b', 3: 'c' };
const set = new Set([1, 2, 3, 4, 5]);

obj.hasOwnProperty('1');
obj.hasOwnProperty(1);
set.has('1');
set.has(1);
//true true false true
```
# Prototype
Khi tìm hiểu về  [prototype trong JavaScript](https://duthaho.com/blogs/prototype-in-javascript), muốn thêm một function vào prototype và share cho tất cả các object dùng chung thì làm như sau:

```javascript
Person.prototype.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};
```
```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person('du', 'ho');
Person.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};

console.log(member.getFullName()); // TypeError
```
# This Context
```javascript
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2;
  },
  perimeter: () => 2 * Math.PI * this.radius
};

console.log(shape.diameter()); // 20 
console.log(shape.perimeter()); // NaN
```
Trong ví dụ trên,  `diameter`  là một function shorthand bình thường, còn  `perimeter`  là arrow function.

Khi tìm hiểu về  [this context](https://duthaho.com/blogs/js-this-context)  trong JavaScript, với arrow function thì  `this`  được  `auto binding`  và  `this`  chính là scope bên ngoài chính function đó. Điều đó có nghĩa là khi chúng ta gọi function  `perimeter`,  `this`  bây giờ không phải là object  `shape`  mà là object global  `window`  (trong trình duyệt), window không có biến  `radius`  nên  `this.radius`  trả về  `undefined`.
# Var And Let
Trong JavaScript, khi một function được thực thi, sẽ trải qua hai giai đoạn. Giao đoạn đầu tiên là  `creation phase`, ở giai đoạn này, các biến khai báo trong function được cấp phát bộ nhớ và gán các giá trị mặc định, giai đoạn thứ hai là  `execute phase`, giai đoạn này sẽ chạy từng dòng code trong function đó.

Sự khác biệt khi chúng ta khai báo biến giữa  `var`  và  `let`  là với biến được khai báo với  `var`, chúng sẽ được cấp phát bộ nhớ và gán giá trị mặc định là  `undefined`  ngay ở giai đoạn  `creation phase`, vì thế, khi JavaScript chạy dòng  `console.log(name)`  sẽ in ra giá trị  `undefined`.

Cơ chế này gọi là  `hoisting`  trong JavaScript. Còn đối với biến được khai báo với  `let`, chúng cũng được  `hoisting`  nhưng có hơi khác một chút với  `var`. Đó chính là trong giai đoạn  `creation phase`, biến  `let`  cũng được cấp phát bộ nhớ nhưng không được gán giá trị mặc định, chúng ta không thể truy cập đến biến này trước khi chúng được gán một giá trị nào đó (`temporal dead zone`). Vì vậy, khi  `console.log(age)`  trước khi  `age`  được gán giá trị, sẽ văng ra lỗi  `ReferenceError`.
```javascript
function sayHi() {
  console.log(name);
  console.log(age);
  var name = 'duthaho';
  let age = 69;
}

sayHi(); // undefined ReferenceError
```
# Array Constructor
```javascript
let i = 0;

const arr = new Array(5);
arr.forEach(() => i++);

console.log(i); //0
```
Bạn nghĩ là `5` à, không, bạn nên xem lại cách hoạt động của [Array constructor](http://duthaho.com/blogs/js-creating-array), khi truyền vào một tham số là number từ 0 đến (2^32 - 1), nó sẽ tạo ra một array chỉ có thuộc tính `length` là number vừa truyền vào chứ nó không chứa các phần tử nào (empty slots), array này còn được gọi là `sparse array`. Vì array không chứa phần tử nào nên hàm `forEach` sẽ không được duyệt qua bất kỳ lần nào, vậy kết quả là `0`.
# Array Loop
Function dưới đây luôn luôn trả về phần tử lớn nhất trong array đúng không No
```javascript
function greatestNumberInArray(arr) {
  let greatest = 0;
  for (let i = 0; i < arr.length; i++) {
    if (greatest < arr[i]) {
      greatest = arr[i];
    }
  }
  return greatest;
}
```
Coi chừng bị lừa nhé, function trên chỉ đúng trong trường hợp các phần tử của mảng lớn hơn `0` mà thôi, còn nhỏ hơn `0` thì chắc là sai rồi, vì biến `greatest` được gán mặc định bằng `0` mà.
# Array Sort
Hàm  `sort`  sẽ sắp xếp lại array và đồng thời trả về chính tham chiếu đến array đó. Vì vậy  `arr1.sort()`  và  `arr1`  tham chiếu đến cùng một object trong bộ nhớ, điều này cũng đúng cho  `arr2.sort()`  và  `arr2`.

Với  `arr1.sort()`  và  `arr2.sort()`  thì rõ ràng chúng tham chiếu đến hai object khác nhau trong bộ nhớ.
```javascript
const arr1 = ['a', 'b', 'c'];
const arr2 = ['b', 'c', 'a'];

console.log(
  arr1.sort() === arr1, // true
  arr2.sort() == arr2, // true
  arr1.sort() === arr2.sort() //false
);
```
# This Binding
Hàm `bind` (tương tự cho `apply` và `call`) trong JavaScript cho phép chúng ta thay đổi ngữ cảnh của biến `this` ([this context](https://duthaho.com/blogs/js-this-context)). Trong trường hợp này, hàm `map` sau khi được `bind` sẽ có biến `this` là `[1, 2, 3]` chứ không phải là `['a', 'b', 'c']`.
```javascript
const map = ['a', 'b', 'c'].map.bind([1, 2, 3]);
map(el => console.log(el)); // 1 2 3
```
# Array Compare
Khi so sánh Double Equals (`==`) giữa array và string, cụ thể là  `a`  hoặc  `b`  với  `c`, JavaScript sẽ tự động gọi  `arr.toString()`  để chuyển đổi array sang string trước khi so sánh, hai mảng  `a`  và  `b`  convert sang string sẽ là  `'1,2,3'`, vì thế  `a == c`  và  `b == c`  cho kết quả  `true`.

Khi so sánh Double Equals (`==`) hay Triple Equals (`===`) giữa các đối tượng là kiểu dữ liệu tham chiếu (`Reference Type`), như object, array, function, chúng ta không quan tâm đến giá trị mà đối tượng đang chứa, mà chỉ quan tâm đến chúng có cùng trỏ đến một địa chỉ ô nhớ hay không mà thôi. Trong trường hợp này,  `a`  và  `b`  là hai array trỏ đến hai ô nhớ khác nhau, vì thế  `a == b`  cho kết quả  `false`.
```javascript
var a = [1, 2, 3];
var b = [1, 2, 3];
var c = '1,2,3';

console.log(a == c); // true
console.log(b == c); // true
console.log(a == b); // false
```
# Array Compare 2
Khi so sánh Double Equals (`==`) giữa array và number, JavaScript sẽ chuyển đổi array sang number trước khi so sánh (`[9] -> 9`  và  `[10] -> 10`), vì thế  `[9] == 9`  và  `[10] == 10`  cho kết quả  `true`.

Nhưng khi so sánh hai array với toán tử  `<`  hoặc  `>`, lúc này array sẽ không được chuyển đổi sang number mà là sang string (`[9] -> "9"`  và  `[10] -> "10"`). Khi so sánh hai string thì sẽ so sánh theo alphabet với từng ký tự một, vì thế  `"9" < "10"`  cho kết quả là  `false`  vì  `"9" < "1"`  là sai.
```javascript
var a = [9];
var b = [10];

console.log(a == 9); // true 
console.log(b == 10); //true 
console.log(a < b); // false
```
# Array Type Conversion
Trong JavaScript, các array (rỗng hoặc có phần tử) đều là `truthy`, tức là khi chúng ta kiểm tra với điều kiện `if ([]) { return true; }` sẽ cho kết quả là `true`. Nhưng xin hãy chú ý khi chúng ta so sánh Double Equals (`==`) giữa array với boolean, JavaScript sẽ chuyển đổi dữ liệu trước khi so sánh (`Type Conversion`), khi đó `arr.toString()` sẽ được gọi `[].toString() = ""`, vì thế `[] == false` cho kết quả `true`.
```javascript
function ArrayBoolean() {
  if ([] == true && [1] == true) return [true, true];
  else if ([] == true && [1] == false) return [true, false];
  else if ([] == false && [1] == true) return [false, true];
  else return [false, false];
}
ArrayBoolean(); // [false, true]
```
# Object Destructuring
```javascript
const url = 'quiz.duthaho.com';
const { length: ln, [ln - 1]: domain = 'quiz' } = url
  .split('.')
  .filter(Boolean);
console.log(domain); // "com"
```
Đấu tiên gán  `quiz.duthaho.com`  cho biến  `url`.

```javascript
const url = 'quiz.duthaho.com';
```

Với toán tử  `=`  (assignment) thì chúng ta quan tâm toán hạng bên phải trước (right side assignment):

```javascript
url.split('.').filter(Boolean);
```

`url.split('.')`  sẽ cắt chuỗi  `url`  thành một array bởi dấu  `.`:  `['quiz', 'duthaho', 'com']`, sau đó array này sẽ gọi  `filter(Boolean)`, đây là cách viết gọn từ:  `filter(el => Boolean(el))`, bởi vì các phần tử trong array đều là string, chúng là  `truthy`  nên  `Boolean(el)`  luôn cho kết quả  `true`, điều đó cũng có nghĩa sau khi  `filter`, các phần tử trong array đều được giữ lại.

Đến đây thì ta có thể viết lại như sau:

```javascript
const { length: ln, [ln - 1]: domain = 'quiz' } = [
  'quiz',
  'duthaho',
  'com'
];
```

Đây rõ ràng là cú pháp của  `Object Destructing`  trong  `ES2015`  vì array cũng là một object, chúng ta có thể truy cập các thuộc tính index và length từ array (`arr["0"], arr["length"]`).

Trong trường hợp này, chúng ta dùng  _aliasing_  để gán thuộc tính  `length`  cho một biến mới là  `ln`. Tiếp theo ta lại gán thuộc tính index thứ  `ln - 1`  cho biến có tên là  `domain`  với giá trị mặc định là  `'quiz'`, có nghĩa là  `domain`  sẽ có giá trị là  `'quiz'`  nếu array không có thuộc tính index thứ  `ln - 1`  nào.

Ở đây,  `length`, được gán cho  `ln`, có giá trị là  `3`, suy ra  `ln - 1`  là  `2`, và phần tử ở vị trí số  `2`  trong array là  `com`. Vì vậy, câu trả lời là  `com`.
# Set
`Set`  được dùng để lưu trữ dữ liệu mà các phần tử trong  `Set`  là duy nhất (unique). Tuy nhiên, nên chú ý với các trường hợp dữ liệu là kiểu dữ liệu tham chiếu, ở đây hai object  `{ a: 1 } và { a: 1 }`  có cùng thuộc tính và giá trị nhưng chúng là hai object hoàn toàn khác nhau và được lưu trong hai ô nhớ khác nhau. Đó cũng là lý do mà  `{ a: 1 } === { a: 1 }`  cho kết quả  `false`.

Trong trường hợp  `Set`  được tạo như sau:  `obj = { a: 1 }`,  `new Set([ obj, obj ])`, khi đó  `Set`  chỉ chứa một phần tử, vì hai object lúc này cùng tham chiếu đến một ô nhớ mà thôi.
```javascript
const mySet = new Set([{ a: 1 }, { a: 1 }]);
const result = [...mySet];
console.log(result); // [{a: 1}, {a: 1}]
```
# Arrays Reverse
```javascript
const ar = [5, 1, 3, 7, 25];
const ar1 = ar;
console.log(ar1.sort());
([5, 25].indexOf(ar[1]) != -1 &&
  console.log(ar.reverse())) ||
  (ar[3] == 25 && console.log(ar));
console.log(ar1); // [1, 25, 3, 5, 7] [7, 5, 3, 25, 1] [7, 5, 3, 25, 1] [7, 5, 3, 25, 1]
```
`const ar1 = ar`  có nghĩa  `ar1`  và  `ar`  cùng tham chiếu đến một array trong bộ nhớ.  `ar1.sort()`  sẽ sắp xếp chính nó và  `ar1`  cũng sẽ thay đổi theo.

Bạn nên nhớ hàm  [sort](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)  của array sẽ chuyển đổi các phần tử sang string trước khi sắp xếp chúng theo alphabet, vậy dòng  `console.log`  đầu tiên là  `[1, 25, 3, 5, 7]`.

Tiếp theo  `[5, 25].indexOf(ar[1]) != -1`  trả về  `true`  nên (`ar.reverse()`) sẽ được gọi.  `ar.reverse()`  sẽ sắp xếp arr theo chiều ngược lại,  `ar`  bây giờ sẽ là  `[7, 5, 3, 25, 1]`, và được in ra ở  `console.log`  thứ hai.

`console.log`  không trả về giá trị nào nên ta có thể viết lại như sau:

```javascript
undefined || (ar[3] == 25 && console.log(ar));
```

`undefined`  là falsy, nên  `ar[3] == 25`  được gọi và kết quả là  `true`  vì phần tử thứ  `3`  của  `ar`  giờ là  `25`, tiếp theo thì  `console.log(ar)`  thứ ba được in ra với kết quả là  `[7, 5, 3, 25, 1]`.

Cuối cùng vì  `ar1`  và  `ar`  cùng tham chiếu đến một array nên dòng  `console.log(ar1);`  thứ tư cũng sẽ in ra (`[7, 5, 3, 25, 1]`).
# Array Reduce
Cả hai cách `a` và `b` đều tạo ra object cùng thuộc tính và giá trị. Theo bạn thì phương án nào tốt hơn? b
```javascript
const arr = [1, 2, 3];

const a = arr.reduce(
  (acc, el, i) => ({ ...acc, [el]: i }),
  {}
);

const b = {};
for (let i = 0; i < arr.length; i++) {
  b[arr[i]] = i;
}
```
Nhiều bạn nghĩ rằng đây là thời đại của ES2015 rồi, dùng `reduce` sẽ gọn và tối ưu hơn. Nhưng hãy nhìn kỹ vào code, với phương án `b`, qua mỗi vòng lặp ta chỉ việc set một thuộc tính mới vào `b`, còn ở phương án `a`, với mỗi lần lặp, spread operator (`...`) sẽ tạo ra thêm một shallow copy của `acc` và sau đó mới set một thuộc tính mới, điều này rõ ràng làm tốn bộ nhớ và không tối ưu.
