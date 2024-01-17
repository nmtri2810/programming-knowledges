# Ruby

Ôn tập về Ruby

## OOP

### 4 tính chất

Bao gồm 4 tính chất chính:

**1. Tính kế thừa (Inheritance)**

- Class con kế thừa class cha về thuộc tính và phương thức.

**2. Tính đóng gói (Encapsulation)**

- Gói gọn các thuộc tính và phương thức trong 1 class, không cho class con truy cập đến các thuộc tính và phương thức một cách tùy chọn, giúp bảo mật dữ liệu trong quá trình kế thừa dữ liệu của lớp cha và con, nó thể hiện qua các phương thức `public`, `private`, `protected` (Access modifier của ruby)

**3. Tính đa hình (Polymorphism)**

- Một phương thức có thể có những cách triển khai khác nhau (overriding và overloading)
- Ruby không có overloading (một class có nhiều method giống nhau, khác tham số truyền vào), tuy nhiên có thể thực hiện overloading dựa trên: 
```ruby
class Test 
    def self.display(*args) 
        case args.size 
            when 1
                puts "1: Hello #{args[0]}"
            when 2
                puts "2: Hello #{args[0]} #{args[1]}"
            when 3
                puts "3: Hello #{args[0]} #{args[1]} Welcome to #{args[2]} language."
        end
    end
end 

puts "Overloading using Class Method"
Test.display"Geeks!!" 
Test.display"Geeks!!", "Hope you doing great."
Test.display"Geeks!!", "Hope you doing great.", "Ruby"
```

**4. Tính trừu tượng (Abstraction)**

- Ẩn các cài đặt chi tiết và chỉ hiển thị chức năng tới người dùng (`module` trong ruby)

### Phân biệt phương thức `public`, `private`, `protected`

**1. Public**
- Mặc định, tất cả các phương thức trong Ruby đều là `public`
- Các phương thức `public` có thể được gọi từ bên ngoài hoặc bên trong đối tượng

**2. Private**
- Các phương thức `private` chỉ có thể được gọi từ bên trong đối tượng và không thể được gọi từ bên ngoài
- **Chú ý**: Có thể gọi `private` method qua phương thức `send`, `public_send` thuộc metaprogramming của ruby

**3. Protected**
- Các phương thức `protected` có thể được gọi từ bên trong đối tượng hoặc từ các đối tượng của cùng một lớp hoặc lớp con 

**Chú ý: Có thể gọi self.protected_method trong class, nhưng không thể gọi self.private_method trong class**

### Phân biệt `class`, `instance`

- `class`: Bản thiết kế tạo ra đối tượng
- `instance`: Đối tượng được tạo dựa trên bản thiết kế đấy

## Variable

- Có 5 loại biến: 
  - Global variable: `$` &rarr; Có thể được truy cập từ bất kỳ đâu trong chương trình
  - Class variable: `@@` &rarr; Có thể được truy cập từ bất kỳ đâu trong class. **Khi thay đổi giá trị thì tất cả instance đều nhận giá trị thay đổi**
  - Instance variable: `@` &rarr; Chỉ thuộc về một đối tượng riêng lẻ
  - Local variable: phụ thuộc vào phạm vi của nơi khai báo
  - Constance variable: Bắt đầu bằng một chữ cái đầu viết hoa. **Có thể thay đổi giá trị biến constance**

## Method

### Phân biệt `class method` và `instance method`

- `class method`: 
  - Được gọi trên một lớp
  - Từ khóa `self` (3 cách phổ biến)
- `instance method`: 
  - Được gọi trên thể hiện của một lớp (đối tượng)
  - Không cần từ khóa `self`

### Các loại parameters trong method

- Default params
```ruby
def info(name, sex="male")
  puts "Name: #{name}, with sex: #{sex}"
end

info("huy", "female") 
# => Name: huy, with sex: female
info("huy") 
# => Name: huy, with sex: male
```

- Params với dấu `*`: 
  - Được sử dụng khi muốn truyền một mảng các đối số bất kỳ:
  ```ruby
  def f(first, *rest)
    puts rest, rest.class
  end

  f(1, 2, 3, 4) 
  # => [2, 3, 4], Array
  ```

  - Một method có không quá một tham số loại này, tham số kiểu mảng phải theo liền kề sau những tham số mặc định, và có thể đứng trước các tham số bình thường khác:
  ```ruby
  def f(a=1, *args, b)
    puts "a: #{a}, args: #{args}, b: #{b}"
  end

  f(10, 20, 30, 40) 
  # => a: 10, args: [20, 30], b: 40
  ```

  - Ngoài ra, `*` còn được sử dụng khi ta muốn truyền một mảng các đối số khi gọi method:
  ```ruby
  def f(name, age, sex)
    puts name, age, sex
  end

  user = ["sherlock", 20, "male"]
  f(*user)
  # => sherlock, 20, male
  ```

- Params với dấu `**`: Khai báo tham số kiểu hash
```ruby
def f(**hash)
  puts hash
end

f(a: 1, b: 2)
# => {:a=>1, :b=>2}
f()
# Nếu không truyền đối số nào, mặc định cho `hash` sẽ là `{}`
# => {}
```

- Params với dấu `&`: 
  - Truyền vào 1 `block`
  - Nếu trong danh sách tham số không có `&`, `block` vẫn có thể được truyền vào qua `{}` hoặc `do; ;end`
  - **Lưu ý**: tham số với `&` chỉ được định nghĩa một lần duy nhất, và nó phải được đặt ở vị trí cuối cùng trong danh sách
```ruby
def f(name, &block)
  yield name
end

f("sherlock") { |name| puts name }
# => sherlock
```

## Symbol/String

### Phân biệt `symbol` và `string`

- `string` có thể thay đổi được, còn `symbol` thì không (chuỗi bất biến)
- `symbol` chia sẻ chung vùng nhớ
- Hiệu suất xử lý của `symbol` tốt hơn và nhanh hơn `string`

## Collection

### Phân biệt `hash` và `array`

- `hash` lưu trữ items theo kiểu key-value, `array` lưu trữ items theo index (đánh số thứ tự)
- Lí do lựa chọn `hash` hoặc `array`:
  - Dữ liệu có cần được liên kết với một label cụ thể không? Nếu có, hãy sử dụng `hash`.
  - Thứ tự có quan trọng không? Nếu có, hãy sử dụng `array`.

### Các method thường sử dụng với collection: select, each, map, inject, reject,...

**1. `Map`**
- Ánh xạ đối tượng, trả về **mảng mới** khiến đối tượng trong collection bị thay đổi
```ruby
[1,2,3,4,5,6,7,8,9,10].map{ |e| e * 5 }

# =>  [5, 10, 15, 20, 25, 30, 35, 40, 45, 50]
```

```ruby
[1,2,3,4,5,6,7,8,9,10].map{ |e| e <= 3 }

# =>  [true, true, true, false, false, false, false, false, false, false]
```

**2. `Select`**
- Chọn đối tượng trong mảng, trả về **mảng mới** với các giá trị thỏa mãn
```ruby
[1,2,3,4,5,6,7,8,9,10].select{ |e| e > 5 }

# =>  [6 ,7, 8, 9, 10]
```

```ruby
[1,2,3,4,5,6,7,8,9,10].select{ |e| e * 3 }

# => [1,2,3,4,5,6,7,8,9,10]
```

**3. `Each`**
- Lặp qua và **xử lý** đối tượng bên trong block. Trả về **mảng ban đầu**
```ruby
[1,2,3,4,5,6,7,8,9,10].each{ |e| print "#{e}!" }

1!2!3!4!5!6!7!8!9!10! => [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

```ruby
[1,2,3,4,5,6,7,8,9,10].each{ |e| e <= 3 }

# => [1,2,3,4,5,6,7,8,9,10]
```

**4. `Collect`**
- Alias của `map` :joy:

**5. `Reduce`**
- Giống `reduce` trong js :upside_down_face:. Tính tổng các kiểu
```ruby
[1,2,3,4,5,6,7,8,9,10].reduce{ |sum, e| sum += e }
# => 55

[1,2,3,4,5,6,7,8,9,10].reduce(15){ |sum, e| sum += e }
# => 70
```

- Trong VD trên: gán gtri khởi tạo = 0, `sum` là giá trị khởi tạo ban đầu, `i` là giá trị lặp của collection, mỗi lần lặp sẽ gán gtri mới cho `sum`

**6. `Inject`**
- Alias của `reduce` :smiling_face_with_tear:

**7. `Detect`**
- Trả về phần tử đầu tiên trong mảng thỏa mãn với 1 điều kiện trong block
```ruby
[1,2,3,4,5,6,7,8,9,10].detect{ |e| e > 5 }

# => 6
```

- Đối với những biểu thức (không có điều kiện) thì nó sẽ trả về phần tử đầu tiên của mảng
```ruby
[1,2,3,4,5,6,7,8,9,10].detect{ |e| e * 3 }

# => 1
```

## Module

### Khái niệm
Giống class, chỉ khác là module không thể tạo các đối tượng và không thể kế thừa

### Kỹ thuật mixin
Trong `class` có thể sử dụng gộp vào nhiều module, các module sử dụng theo cách này gọi là các mixins

- `include`: include module vào trong `class` thì các phương thức trong module sẽ được xem như là các instance method.

- `extend`: extend module vào trong `class` thì các phương thức trong module sẽ được xem như là các class method.

```ruby
module MyModule
  def say_hello
    p "Hello!"
  end
end

class A
  include MyModule
end

class B
  extend MyModule
end

# => call method of module
A.new.say_hello
B.say_hello
```

### Namespace 
- `module` ngoài chức năng gộp thành nhóm các phương thức để sử dụng bởi lớp, nó cũng dùng để nhóm các lớp vào một tên module, kỹ thuật này gọi là Namespace. Để truy cập đến một lớp trong Module dùng ký hiệu ::

```ruby
module Mammal
  class Dog
    def speak
      puts "Woof!"
    end
  end

  class Cat
    def speak
      puts "Meow"
    end
  end
end

a = Mammal::Dog.new
b = Mammal::Cat.new

a.speak  # "Woof"
b.speak  # "Meow"
```

## Metaprogramming

### Khái niệm
- "Metaprogramming is a programming technique in which computer programs have the ability to treat programs as their data"
- "Metaprogramming is a set of techniques that Ruby offers us to write code that dynamically writes other code for us. Rather than working on data, metaprogramming works on other code. This is great because it means we can write more dynamic, flexible, and adaptable code if the situation calls for it"
- `Metaprogramming` là một kỹ thuật giúp code "động" hơn, ngắn gọn hơn, mở hơn, tránh bị lặp lại những thứ giống nhau

### Basic techniques

#### send()
- `send()` là một instance method của Object `class`
- Dùng để **gọi các hàm một cách "động"**, tránh lặp lại nội dung tương tự nhau
- VD: để tránh lặp lại các hàm `authenticated?` như dưới:
```ruby
def password_authenticated?(token)
  digest = self.password_digest
  return false if digest.nil?
  BCrypt::Password.new(digest).is_password?(token)
end

def reset_authenticated?(token)
  digest = self.reset_digest
  return false if digest.nil?
  BCrypt::Password.new(digest).is_password?(token)
end

def activation_authenticated?(token)
  digest = self.activation_digest
  return false if digest.nil?
  BCrypt::Password.new(digest).is_password?(token)
end
``` 

- Ta sẽ dùng một hàm `authenticated?` chung cho cả 3 chức năng với tên chức năng được truyền vào từ tham số, từ đó có thể rút gọn và tổng quát hóa được các hàm có cùng chức năng.
```ruby
def authenticated?(attribute, token)
  digest = send("#{attribute}_digest")
  return false if digest.nil?
  BCrypt::Password.new(digest).is_password?(token)
end
``` 

- ngoài ra, `send()` cũng có thể dùng để gọi một private method của class:
```ruby
class Klass
  private
  def hello(*args)
    "Hello " + args.join(' ')
  end
end

k = Klass.new
k.hello "gentle", "readers" #=> NoMethodError: undefined method `hello' for #<Klass:0x0000000004822888>
k.send :hello, "gentle", "readers"   #=> "Hello gentle readers"
``` 
- Để đảm bảo sự an toàn, nên dùng `public_send` để không thể gọi `private method`

#### define_method()
- `define_method()` là một private instance method của class `Module`
- Dùng để **tạo ra các method một cách "động"**
- Tạo ra các method giống ví dụ trên: 

```ruby
%w(password reset activation).each do |attribute|
  define_method("#{attribute}_authenticated?") do |token|
    digest = self.send("#{attribute}_digest")
    return false if digest.nil?
    BCrypt::Password.new(digest).is_password?(token)
  end
end
``` 

#### method_missing()
- `method_missing()` được trả về khi xảy ra lỗi `NoMethodError`
- Là một cách để không raise errors khi `method` không được khai báo

```ruby
class Example
  def method_missing(method_name, *args, &block)
    puts "Phương thức #{method_name} không tồn tại. Đang xử lý..."
  end
end

obj = Example.new
obj.undefined_method # Sẽ gọi đến method_missing
``` 

## Exception

### Khái niệm
- Exception (Ngoại lệ) là cách giải quyết những lỗi không mong muốn trong lập trình
```ruby
1 / 0
# Program crashes and outputs: "ZeroDivisionError: divided by 0"
``` 
- Nếu không xử lý `exception` này thì ứng dụng sẽ "chết" mà không đưa ra bất cứ hướng dẫn và giải thích gì cho người dùng

### Sử dụng

#### Cú pháp cơ bản: `begin/rescue/end`

```ruby
begin
  #  khối lệnh được thực thi
rescue
  #  xử lý ngoại lệ ở đây
end
``` 

- VD:
```ruby
begin
  file = open("not_exist_file.txt")
  if file
    puts "File opened successfully"
  end
rescue
  puts "File not found"
end

print file, "\n"

# => Thay vì sập nnay: No such file or directory @ rb_sysopen - file_name`.txt (Errno::ENOENT)
# => Thì nó sẽ in ra File not found
```

#### Lệnh `retry`
- Sử dụng `retry` sẽ giúp ta lặp lại việc thực hiện khối lệnh trên trong khối lệnh `begin`. Tuy nhiên, cần phải chú ý đến việc sử dụng lệnh này vì nó sẽ tạo ra vòng lặp vô hạn nếu không handle nó tốt

```ruby
begin
  #  khối lệnh được thực thi
rescue
  # xử lý ngoại lệ ở đây
  retry # thực hiện lại code trong khối begin
end
```

#### Lệnh `raise`
- `raise` là phương thức giúp show ra một ngoại lệ. Gặp lệnh này thì chương trình sẽ ngay lập tức đưa ra `Exception` hiện tại (nếu không gặp ngoại lệ nào thì đưa ra `RuntimeError`)

```ruby
raise # Đưa ra current Exception hoặc RuntimeError nếu không có ngoại lệ nào. Được sử dụng khi bạn muốn chặn trước một ngoại lệ nào đó mà biết biết chắc nó sẽ xảy ra ngay sau đó.

raise "Error Message"  # Đưa ra ngoại lệ kèm theo messages

raise ExceptionType, "Error Message" # Đưa ra ngoại lệ được định dạng sẵn kiểu của ngoại lệ đó kèm theo messages

raise ExceptionType, "Error Message" condition #  Đưa ra ngoại lệ được định dạng sẵn kiểu của ngoại lệ đó kèm theo messages khi điều kiện nào đó xảy ra
```

#### Lệnh `ensure`
- `ensure` đảm bảo một số hành động luôn luôn xảy ra cho dù có hay không có ngoại lệ nào

```ruby
begin
  file = open("/tmp/some_file", "w")
  # ... write to the file ...
rescue
  # ... handle the exceptions ...
ensure
  file.close   # ... and this always happens.
end
```

- Nếu có xuất hiện mệnh đề  `else` trong khối lệnh `begin/end` thì nó sẽ nằm sau khối lệnh `rescue` và trước khối lệnh `ensure` (nếu có). Tất cả code trong `else` sẽ chỉ được chạy khi không có bất kì ngoại lệ nào xảy ra

```ruby
begin
  # raise 'Đưa ra lỗi bất kỳ'
rescue Exception => e
  puts e.message
  puts e.backtrace.inspect
else
  p "else - Chạy khi Không có bất kỳ ngoại lệ nào"
ensure
  puts "ensure - Tôi luôn chạy dù có bất kỳ ngoại lệ nào"
end
```

### Phân cấp Exception
```
Exception
  NoMemoryError
  ScriptError
    LoadError
    NotImplementedError
    SyntaxError
  SignalException
    Interrupt
  StandardError
    ArgumentError
    IOError
      EOFError
    IndexError
    LocalJumpError
    NameError
      NoMethodError
    RangeError
      FloatDomainError
    RegexpError
    RuntimeError
    SecurityError
    SystemCallError
    SystemStackError
    ThreadError
    TypeError
    ZeroDivisionError
 SystemExit
```

## Block/lambda/proc

### Khái niệm
- Block, lambda và proc cho phép bạn chuyển mã lệnh vào một method và thực thi mã đó tại một thời gian sau đó (khi method chứa mã lệnh được gọi)
- Gọi chung là các bao đóng (closure) trong khoa học máy tính

> `closure` : theo ngôn từ lập trình thì nó là 1 chức năng tham chiếu đến 1 chức năng khác cùng môi trường tham chiếu. `closure` cho phép 1 chức năng truy cập đến các biến không trong phạm vi của mình (non-local) thậm chí khi gọi bên ngoài phạm vi trực tiếp của mình.

### Block
- `Block` đơn giản chỉ là tập hợp các lệnh thành một khối (method) được đặt trong dấu `{...}` hoặc `do...end`
- Quy ước chung: sử dụng `{...}` cho các block 1 dòng lệnh và `do...end` cho các block multi-line

```ruby
[1,2,3,4].map! do |n|
  n * n
end
# => [1, 4, 9, 16]

[1,2,3,4].map! { |n| n * n }  #cách viết này ngắn gọn hơn cách dùng do...end
# => [1, 4, 9, 16]
```

#### Từ khóa `yield`
- `yield` được sử dụng để gọi một block, code trong block đó sẽ chạy bth
- Khá giống method

```ruby
def print_once
  yield
end

print_once { puts "Block is being run" }
```

- Mỗi lần gọi yield là trong block sẽ chạy, giống việc gọi lại một method

```ruby
def print_twice
  yield
  yield
end

print_twice { puts "Hello" }

# "Hello"
# "Hello"
```


### Lambda


### Proc

