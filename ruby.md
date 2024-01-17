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

## Module

## Metaprogramming

## Exception/rescue/raise/ensure

## Block/lambda/proc

