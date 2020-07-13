# RESTful API
>-  Tiêu chuẩn quản lý các resource trong các ứng dụng web.
> - Sử dụng các HTTP method (như _GET_, _POST_, _PUT_, _DELETE_...) và cách định dạng các URL cho ứng dụng web để quản các resource.

# Xử lý bất đồng bộ
> **Javascript là** ngôn ngữ lập trình **bất đồng bộ** và chỉ chạy trên một luồng (**single-thread**). 
> Các cách xử lý bất đồng bộ:
> - **Dùng callback**
>    - **Callback** là một hàm sẽ được thực hiện sau khi một hàm khác đã thực hiện xong
>    - **Callback** là một cách để đảm bảo code sau không thực thi cho đến khi code trước thực hiện xong.
> - **Dùng Promise**
>  	- Để tránh Callback Hell,
>  	- Cú pháp
> ```js
>let promise = new Promise(function(resolve, reject) {
>  // Code here
>});
>```
>Ban đầu, Promise có state là  _pending_  và kết quả  _value_  là  **undefined**. Khi executor kết thúc công việc, nó sẽ gọi đến 1 trong 2 hàm được truyền vào:
>	  **resolve(value)**: để xác định rằng công việc đã thực hiện  thành công
	 >  	 -   state chuyển thành  _fulfilled_
	 > 		  -   kết quả là  _value_
>  **reject(error)**: để xác định rằng đã có lỗi xảy ra
	>   	 -   state chuyển thành  _rejected_
	>   	 -   kết quả là  _error_
> -  **Async/await**
> **Async/await** là một cú pháp đặc biệt giúp bạn làm việc với Promise dễ dàng hơn. Khi sử dụng async/await, cấu trúc chương trình xử lý bất đồng bộ sẽ giống với chương trình xử lý đồng bộ hơn.
> [Code](https://repl.it/@lntduong/bat-dong-bo)

# Closure
> Là hàm được tạo ra từ bên trong của một hàm khác ( hàm cha), nó có thể sử dụng các biến toàn cục, biến cục bộ của hàm cha và biến cục bộ của chính nó.
> [Kham khảo](https://completejavascript.com/tim-hieu-javascript-closures)

# Scoped
> - Block scoped: Phạm vi hoạt động nằm trong một cặp dấu ngoặc nhọn được tạo ra bởi if, for, while.
> - Function scoped: Phạm vi hoạt động nằm trong function
> - Lexical scoped: Hàm lồng bên trong có thể truy suất đến biến được khai báo từ hàm bên ngoại.

# Declare variables
> - var: Phạm vi là function/ locally scoped hoặc globally scoped. Có thể cập nhật lại giá trị/ khai báo trùng tên biến trong cùng phạm vi
>  - let: Phạm vi là block scoped. Có thể cập nhật lại giá trị, không thể khai báo trùng tên biến trong cùng phạm vi.
>  - const: Giống như let, nhưng có giá trị không đổi, không thể cập nhật hay khai báo lại.

# Hoisting
> Là hành động mặc định của JS, nó sẽ chuyển phần khai báo lên phía trên cùng trong file JS. (**sử dụng trước, khai báo sau**!)

# Arrow function
>  - Cú pháp:
>  ``` js 
> let sayHi = () => {
>  //code
> }
> sayHi()
> ```
> - Hàm có 1 tham số, bỏ {}
>  ``` js 
> let greeting = name => {
> //code
> }
> greeting('Darren');
> ```
> - Loại bỏ từ khóa return
>  ``` js 
> let double = x => x * 2;
> double(1); //2
> ```
> - Ưu - khuyết điểm
>   - Arrow function thường ngắn gọn hơn function
>   - Arrow function không bind this
>   - Arrow function không bind arguments
>   - Arrow function không phù hợp làm method cho object
>   - Arrow function không thể sử dụng làm hàm constructor
>   - Arrow function không có thuộc tính prototype
>   - Arrow function không được hoisted

# Event Time
> - setInterval() : Thiết lập độ trễ cho các hàm sẽ được thực hiện lặp đi lặp lại.
> ``` js 
> setInterval(function, milliseconds);
> ```
> - clearInterval(): Kết thúc việc thực thi của phương thức setInterval()
> ``` js
> var timerId = setInterval(function, milliseconds);
> clearInterval(timerId);
> ```
> - setTimeout(): Hàm sẽ thực thi bao nhiêu mili giây kể từ khi gọi method
> ``` js
> setTimeout(function, milliseconds);
> ```
> - clearTimeout(): Kết thúc việc thực thi của phương thức setTimeout()
> ``` js
> var timerId = setTimeout(function, milliseconds);
> clearTimeout(timerId);
> ```

# "use strict"
> - Quản lý syntax nghiêm ngặt hơn.
> - Khi sử dụng "use strict"
>     - Gán giá trị cho var chưa khai báo name = 'Darren Lei'
>     - Báo lỗi khi sử dụng delete
>     - Các tham số của hàm không được trùng nhau
``function func(name, name, age)``
>     - Không được khai báo biến dưới dạng nhị phân
``var num = 01010;``
>     - Không được phép ghi đè lên thuộc tính chỉ được phép đọc
``Object.defineProperty(obj, 'ver', {value: 1, writeable: false});``
>     - Không được sử dụng with
>     - Không cho phép khai báo biến trong eval
``eval("var x = 4");``
>     - Không khai báo tên biến trùng với keyword

# Null vs Undefined
> - null
>   -   `null`  là empty hoặc không tồn tại giá trị.
>   -  `null`  phải được assign.
>   ``let a = null; console.log(a); // null``
>  - Undefined  thường có nghĩa là một biến được khai báo, nhưng không được định nghĩa.
> `` let b; console.log(b); // undefined``
>     - Có thể assign một biến bằng undefined:
> ``let c = undefined; console.log(c); // undefined``
>    - Khi tìm kiếm properties không tồn tại của một object
>    ``var d = {}; console.log(d.fake); // undefined``
# Data type
> - Primitive type: Boolean, Null, Undefined, Number, String, Symbol
> - Objects type: objects, functions. arrays
