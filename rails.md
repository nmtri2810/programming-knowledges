# Rails

Ôn tập về Ruby on Rails

## MVC

- Bao gồm Model, View, Controller
- Cách hoạt động

## RESTful

### Giao thức HTTP

- Là 1 giao thức giữa hoạt động dựa trên mô hình Client – Server

### Kiến trúc REST

- Viết tắt cho **RE**presentational **S**tate **T**ransfer
- `REST` là một kiến trúc phần mềm được thiết kế để tạo ra các website thông qua giao thức HTTP
- Tuân thủ 4 nguyên tắc:
  - Sử dụng các HTTP method (GET, POST, PUT/PATCH, DELETE)
  - Stateless
  - Hiển thị cấu trúc thư mục như URI
  - Tập trung vào **tài nguyên**

### RESTful trong rails

- `RESTful` là tên gọi của các ứng dụng được phát triển dưới kiến trúc `REST`
- TRong rails, routes sử dụng các phương thức HTTP một cách rõ ràng, có 7 action chính
- Controller chuẩn RESTful

## CFRS/XSS

### CFRS

- Cross-Site Request Forgery (CSRF) là một kỹ thuật tấn công sử dụng quyền chứng thực của người dùng để thực hiện các hành động trên một website khác
- Rails sinh ra token, dc nhúng bên HTML và lưu trong session ở serer. Khi user tạo request lên server thì sẽ so sánh token, nếu giống nhau mới tiếp tục

### XSS

- Cross-site scripting (XSS) là lỗ hổng cho phép kẻ tấn công chèn và thực thi client-side script trong trang web
- Rails đã hỡ trợ phòng tránh XSS với cơ chế tự động escape

## Migration

- `Migration` là một tính năng của `Active record` cho phép thay đổi cấu trúc trong database
- File `schemas` là file ánh xạ với DB
- Thứ tự: quản lý thông qua `timestamp` ,tạo những bảng cha trc rồi ms đến bảng con

## Gemfile/Gemfile.lock

- `Gemfile` là nơi lưu những gem mình muốn sử dụng, có thể lựa chọn phiên bản hoặc không
- `Gemfile.lock` sẽ quản lý chính xác version của gem và các dependency của chúng
- **Lưu ý**: không nên chạy `bundle update` vì sẽ cập nhật tất cả lên phiên bản mới nhất

## I18n

- Hỗ trợ tạo 1 trang web với nhiều ngôn ngữ khác nhau
- Lazy lookup: xếp từ dịch thành nhiều tầng

## Helper

- Hỗ trợ xử lý logic trong `view`, giúp code tái sử dụng và tối giản logic trong `view`

## Gem Bcrypt

### has_secure_password

- `has_secure_password` hỗ trợ băm và lưu mật khẩu trong DB, ngoài ra còn có authenticate lại password. Để sử dụng được yêu cầu phải có attribute `XXX_digest`
- Một số validations sẽ tự động được thêm vào
- Mục đích: Đảm bảo an toàn cho mật khẩu

### Cơ chế mã hóa

- `bcrypt` kết hợp 3 khái niệm: `hashing`, `salting` và `streching`

## Action filter trong controller

3 loại filters:

- `before_filter`: chạy trước khi request được gửi đến action trong `controller`, thường sử dụng để lấy giá trị, validate, authenticate,...
- `after_filter`: chạy sau khi action xử lý request, thường dùng để log thông tin, làm sạch tài nguyên,...
- `around_filter`: bọc xung quanh action, chủ yếu sử dụng cho xử lý ngoại lệ (exception)
![img](https://mevn-public.s3-ap-southeast-1.amazonaws.com/marketenterprise.vn/wp-images/2022/09/11123438/rails-3-filters.png)

## Strong params

### Mục đích sử dụng của strong params

- Tăng bảo mật cho params khỏi tấn công Mass Assignment bằng cách đưa các params vào whitelist các thuộc tính của đối tượng mà user được phép truy cập

```ruby
def book_params
  params.require(:user).permit(:name, :email)
end
```

- require(:user) bắt buộc trong params phải có key là user
- permit(:name, :email) trong key user chỉ cho phép name và email

### permit! và permit

- permit! sẽ cho tất cả thuộc tính vàp trong whitelist => **Không nên**

## Resources/resource/nested resource/collection/member

### Resources và resource

- `Resources` có 7 actions, `routes` có sử dụng id
- `Resource` có 6 action (không có index), `routes` ko sử dụng id

### Nested resources

- `Nested resources` là một kĩ thuật để phản ánh mối quan hệ `has_many` giữa các model trong `routes` và thể hiện nó qua URLs

### Collection và member

- Dùng để tạo ra các `routes` cho các method không phải mặc định của `controller`
- Phân biệt:
  - `Member` sẽ yêu cầu id từ `resources` chứa nó
  - `Collection` không yêu cầu id từ `resources` chứa nó

## Scope/namespace trong routing

### Namespace

`Namespace` sử dụng đơn giản hơn, bằng cách thêm tiền tố URL cho các tài nguyên được chỉ định, và xác định vị trí `controller` theo một module có tên giống như tên `namespace` của bạn

```ruby
namespace :admin do
  resources :books
end
```

### Scope

`Scope` phức tạp hơn 1 chút:

- Sử dụng không có lựa chọn sẽ chỉ thay đổi URI pattern
- Cung cấp 3 lựa chọn: `module`, `path`, a`s
  - `Module`: chỉ ra `controller` sẽ được chứa trong `module` name nào
  - `Path`: đặt tiền tố sẽ xuất hiện trong URI, trước tên tài nguyên
  - `As`: thay đổi tên của đường dẫn sử dụng để xác định tài nguyên

- Vậy nên nếu sử dụng:

```ruby
scope module: "admin", path: "admin", as: "admin" do
  resources :books
end
```

- Tương đương với:

```ruby
namespace :admin do
  resources :books
end
```

## Session/cookies trong rails

### Session

- `Session` là dữ liệu được lưu trữ trên server, sẽ tồn tại cho đến khi browser bị đóng
- Mỗi khi người dùng truy cập vào hệ thống, Rails kiểm tra xem request này đã có `session` chưa thông qua `cookies` mà trình duyệt gửi lên, nếu chưa có `session` thì Rails sẽ tự động tạo ra một `session` mới và set cho `cookies` id của `session` này. Giá trị của cookie lưu `session` id này sẽ bị mất khi trình duyệt đóng => khi mở lại trình duyệt và request lại sẽ tạo ra một `session` mới

### Cookies

- `Cookies` là dữ liệu được lưu trữ trên browser dưới dạng key-value, mỗi `cookie` tồn tại cho đến khi nó bị hết hạn
- **Lưu ý**: khi set `cookie` mà không có thời gian expires thì `cookie` đó sẽ bị expires khi trình duyệt bị đóng (giống `session`)
- Để đặt giá trị `cookie` cho browser thì trong phần headers của response sẽ có thêm các thẻ `Set-Cookie` dùng để đặt giá trị `cookie` cho trang web

## Callback

### Vòng đời chạy callback

...

### Mục đích sử dụng của callback

- Callback là một phương thức của `Active Record` trong `model`, nó sẽ được gọi tới vào một thời điểm nào đó trong vòng đời của một đối tượng (create/update/destroy)
- Callback thường được dùng để thực thi các phương thức logic trước hoặc sau khi đối tượng có một sự thay đổi nào đó

### Một số loại cơ bản + thứ tự chạy

#### Create + update

- before_validation
- after_validation
- before_save
- around_save
- before_create/update
- around_create/update
- after_create/update
- after_save
- after_commit/after_rollback

#### Destroy

- before_destroy
- around_destroy
- after_destroy
- after_commit/after_rollback

### Method gọi callback

- create
- create!
- update
- update_attribute
- update!
- destroy
- destroy!

### Method skip callback

- delete
- delete_all
- update_columns
- update_all
- ...

### Callback có điều kiện

Sử dụng if hoặc unless check điều kiện với:

- Symbol
- Proc
- Nhiều điều kiện

## Validation trong active record

### Phân biệt validates và validate

- `Validates` dùng cho những generic validation, sử dụng Active record Validation
- `Validate` dùng cho những custom validation

### Những validates thường dùng

- `presence`: Tương tự NOT NULL trong Mysql
- `uniqueness`: Không cho phép trùng dữ liệu
- `length`: Độ dài của dữ liệu
- `inclusion`: Bắt buộc dữ liệu phải thuộc trong list có sẵn
- `email`: kiểm chứng tính hợp lệ cho column email
- `numericality`: Validate cho kiểu dữ liệu là số

### Method gọi validates

- create
- create!
- update
- update!
- destroy
- destroy!

### Method skip validates

- update_all
- update_attribute
- update_column
- update_columns
- ...

## Association

### Các loại relationships

- belongs_to
- has_one
- has_many
- has_many :through
- has_one :through
- has_and_belongs_to_many
- Polymorphic Associations:

```ruby
class Picture < ApplicationRecord
  belongs_to :imageable, polymorphic: true
end

class Employee < ApplicationRecord
  has_many :pictures, as: :imageable
end

class Product < ApplicationRecord
  has_many :pictures, as: :imageable
end
```

### Các option khi khai báo relationship

- through: Chỉ định được join thông qua bảng nào
- dependent: chỉ định những mqh sẽ thế nào nếu current model có sự thay đổi
- class_name: Chỉ định tên class khi association name khác tên model
- foreign_key: Cho phép set trực tiếp tên foreign key
- source: rename association cần tùy chọn này

## Scope trong active record

### Default scope và scope

#### Default scope

- Dfault scope là những queries luôn được thêm vào trong model
- **Không nên dùng** vì sẽ gây nên những lỗi dbt đâu ra

#### Scope trong model

- Scopes là custom queries được định nghĩa trong Rails models với scope method
- Nhận 2 tham số: tên scope và lambda thực hiện code
- Bản chất cũng là class method
- Có thể thêm if và unless check điều kiện

### Phân biệt scope và class method

- Scope luôn trả về một ActiveRecord:Relation trong khi class method trả về nil
- Thường sử dụng scope cho những logic đơn giản, class method cho những logic phức tạp hơn

## Query trong active record

### Query cơ bản: where, order, join, select

- where: lọc records theo điều kiện nào đó
- order: sắp xếp theo gì đó
- join: trong rails hiện tại chỉ hỗ trợ join và left join
- ...

### ORM

- Object Relational Mapping là một layer nằm giữa ngôn ngữ lập trình và database, giúp thao tác với database mà không cần query
- Trong rails, Active Record là ORM

### Phân biệt find, find_by! và find_by

- `find` tìm kiếm bằng ID hoặc mảng ID, có thể trả về mảng object, và sẽ bắn ra exception nếu không tìm thấy record
- `find_by` tìm kiếm theo trường nào đó, tìm thấy sẽ trả về 1 object, và sẽ trả về nil nếu không tìm thấy record
- `find_by!` gióng find_by, nhưng không tìm thấy record sẽ bắn ra exception `ActiveRecord::RecordNotFound`

### Phân biệt update và update_columns

- `update(attribute_name: value)`: Có kiểm tra `validations` và chạy `callbacks`, trường `updated_at` được cập nhật nếu thành công
- `update_columns(attribute_name: value)`: Bỏ qua cả `validations` và `callbacks`, trường `updated_at` không được cập nhật

## Delegate

- `Delegate` giúp sử dụng các `public method` của object khác một cách trực tiếp như của mình.
- Các options:
  - `to`: Chỉ định object muốn sử dụng public methods
  - `prefix`: Dùng để quy định tên method của đối tượng delegate tới
  - `allow_nil`: Nếu được set bằng true, trả về nil thay vì NoMethodError khi target của method là nil

```ruby
class Greeter < ActiveRecord::Base
  def hello
    "hello"
  end

  def goodbye
    "goodbye"
  end
end

class Foo < ActiveRecord::Base
  belongs_to :greeter
  delegate :hello, to: :greeter
end

Foo.new.hello   # => "hello"
Foo.new.goodbye # => NoMethodError: undefined method `goodbye" for #<Foo:0x1af30c>
```

## N+1 query

### Định nghĩa n+1 query

- Khi truy vấn không hiệu quả do sử dụng quá nhiều truy vấn. VD:...

### 3 method tránh n+1 query

#### Eager_load

- Sử dụng `left outer join` tạo 1 truy vấn lấy ra model chính và các model liên kết

#### Preload

- Tạo 2 queries, 1 đến model chính và 1 lấy ra các model liên kết

#### Includes

- Includes là tổng hợp của cả 2, Rails sẽ tự lựa chọn sử dụng preload hay eager_load

## Render partial

### Định nghĩa render partial

- `Partial` cho phép dễ dàng trong việc tổ chức và tái sử dụng lại view code trong ứng dụng Rails

### Render partial và render collection partial

- ...

## Send mail

### Phân biệt `deliver_now` và `deliver_later`

- `deliver_now` được sử dụng để gửi email ngay lập tức khi phương thức gửi email được gọi
- `deliver_later` được sử dụng để gửi email trong tương lai thông qua việc đặt công việc vào một hàng đợi (queue) xử lý bất đồng bộ

## Transaction

### Mục đích sử dụng

- `Transaction` là một kỹ thuật trong việc lập trình, nó xử lý có tuần tự các thao tác trên cơ sở dữ liệu
- Khi một `transaction` thực hiện thành công nghĩa là tất cả các thay đổi dữ liệu được thực hiện trong `transaction` được lưu vào database. Ngược lại khi một `transaction` bị lỗi và được rollback thì tất cả các sửa đổi dữ liệu sẽ bị xóa, dữ liệu sẽ khôi phục trở về trạng thái chưa thực hiện `transaction`

### Triển khai transaction

Có 3 cách:

- Call by `ActiveRecord::Base`

```ruby
ActiveRecord::Base.transaction do
  # code
end
```

- Call by `Class`

```ruby
Account.transaction do
  # code
end
```

- Call by `instance`

```ruby
account.transaction do
  # code
end
```

### Method trong active record làm việc với transaction

## Enum trong active record

## Ajax

### Định nghĩa Ajax

AJAX (Asynchronous Javascript and XML) là phương thức trao đổi dữ liệu với server và cập nhật một hay nhiều phần của trang web, không reload lại toàn bộ trang

### Sử dụng trong rails

- Trong rails 7 sử dụng bằng hotwire - turbo

### Phân biệt ajax jquery và ajax rails

### Respond_to

- Lựa chọn format để render. Có thể là html, js, json, turbo_stream, xml,...

## Rake task

- `Rake` là công cụ để quản lí các task trong `rails`, với mục đích gom nhóm các đoạn code `ruby` thường xuyên được sử dụng vào một task chung để sử dụng lại nhiều lần
- Mục đích sd: CRUD model không cần viết nhiều lần, liên quan đến db, ...

## Rails gem

### Devise

#### Các module cơ bản

### Cancancan

- Giúp phân quyền, hạn chế tài nguyên mà một user được phép truy cập => dễ bảo trì và kiểm tra logic phân quyền
- Bao gồm 2 phần chính:
  - Authorizations definition library: Thư viện xác nhận phân quyền cho phép xác nhận rules cho người dùng để truy cập đến các đối tượng khác nhau và nó cung cấp các helpers để check các quyền đó
  - Controller helpers: đơn giản hóa code trong `controller` bằng các thực hiện loading và checking các phân quyền của các `model` trong `controllers`
- Class `Ability` là nơi mà tất cả các phân quyền user được định nghĩa

```ruby
class Ability
  include CanCan::Ability

  def initialize(user)
    can :read, :all
    if user.present?
      can :manage, Post, user_id: user.id
      if user.admin?
        can :manage, :all
      end
    end
  end
end
```

#### Định nghĩa `Abilities`

##### `Can` method

- `can` method định nghĩa các quyền, yêu cầu 2 đối số:
  - Đối số đầu là action cho phép
  - Đối số t2 là class của đối tượng đang thiết lập
- Có thể đặt `:manage` để đại diện cho bất kỳ hành động nào và `:all` đại diện cho bất kỳ đối tượng nào
- Có 4 action thường được sử dụng là `create`, `read`, `update` và `destroy`. Không giống như 7 RESTful action trong `Rails`, `CanCanCan` tự động thêm một số alias thuận tiện cho việc lập map action của `controller`

```ruby
read: [:index, :show]
create: [:new, :create]
update: [:edit, :update]
destroy: [:destroy]
```

- Có thể custom action

##### Hash of Condition

- Có thể được truyền vào để giới hạn những bản ghi nào mà phân quyền này áp dụng
- Ở đây user sẽ chỉ được phép đọc những Project đang active mà họ sở hữu

```ruby
can :read, Project, active: true, user_id: user.id
```

##### Fetching Records

- là việc hạn chế các bản ghi nào được trả lại từ cơ sở dữ liệu dựa trên những gì người dùng có thể truy cập
- Truyền vào `current_ability` trong controller:

```ruby
@articles = Article.accessible_by(current_ability)
```

- Chỉ tìm thấy những record có quyền update:

```ruby
@articles = Article.accessible_by(current_ability, :update)
```

##### Defining Abilities with Blocks

- Sử dụng `block` để lồng điều kiện phức tạp vào 1 hash:

```ruby
can :update, Project do |project|
  project.priority < 3
end
```

- **Không** viết việc checking quyền truy cập vào trong block

#### Kiểm tra `Abilities`

- Sau khi `ability` được định nghĩa, ta có thể sử dụng `can?` hoặc `cannot?` method trong `controller` hoặc `view` để kiểm tra phân quyền user cho một action và object

```ruby
can? :destroy, @project
cannot? :destroy, @project
```

##### Kiểm tra với class

- Truyền vào 1 class thay vì instance:

```ruby
<% if can? :create, Project %>
  <%= link_to "New Project", new_project_path %>
<% end %>
```

- **Lưu ý**: điều kiện của block hoặc hash sẽ bị bỏ qua khi kiểm tra trên class và nó sẽ trả về true:

```ruby
can :read, Project, priority: 3

can? :read, Project # => returns true
```

##### Kiểm tra với non-model

```ruby
class PasswordResetController < ApplicationController
  authorize_resource :class => :controller
end

# Use false if no class
class PasswordResetController < ApplicationController
  authorize_resource :class => false
end

class Ability
  include CanCan::Ability
  def initialize(user)
    if user.customer?
      can :manage, :passwordreset
    else
    #write code...
    end
  end
end

```

#### Controller helpers

##### Authorizations

- Phương thức `authorize!` trong `controller` sẽ đưa ra một ngoại lệ nếu user không có khả năng để thực hiện hành động đó

```ruby
def show
  @article = Article.find(params[:id])
  authorize! :read, @article
end
```

##### Loaders

- Sử dụng method `load_and_authorize_resource` để ủy quyền tự động cho tất cả các hành động trong RESTful. Nó sẽ sử dụng bộ lọc trước để load tài nguyên vào một biến instance và cho phép mọi hành động

```ruby
class ArticlesController < ApplicationController
  load_and_authorize_resource

  def show
    # @article is already loaded and authorized
  end
end
```

#### Phân biệt `load_and_authorize_resource` và `authorize_resource` (có cả `load_resource`)

- `load_and_authorize_resource` sẽ load instance model tự động dựa vào tên `controller` sau đó kiểm tra điều kiện ủy quyền, còn `authorize_resource` sẽ chỉ kiểm tra điều kiện ủy quyền
- Nói cách khác, `load_and_authorize_resource` = `load_resource` + `authorize_resource`

### Ransack

### Pagy

### Rspec

#### 3 khối trong rspec

- `Describe`, `context` và `it`

#### Phân biệt let! và let

- `let` được lazy load, khi nào test (chạy `it`) ms tạo
- `let!` được tạo ngay sau khi `block` dc định nghĩa

#### Factory

- Giúp tạo ra những dữ liệu để test

#### Phân biệt được mock và stub

##### Mock

- Kỳ vọng một phương thức được gọi đến sẽ trả về một hay nhiều giá trị nào đó

```ruby
expect(some_object).to receive(some_method).and_return(some_value)
```

##### Stub

- Chỉ quan tâm đến trạng thái trả về của một đối tượng khi nhận một message nào đó

```ruby
allow(some_object).to receive(some_method).and_return(some_value)
```

#### Subject

##### Implicit subject

- Khi argument đầu tiên của example group ngoài cùng là một `class`, thì một `instance` của `class` đó sẽ được truyền đến mỗi example thông qua `subject`

```ruby
RSpec.describe Array do
  describe "when first created" do
    it "should be empty" do
      expect(subject).to be_empty     # => subject ở đây là []
    end
  end
end
```

##### Explicit subject

- Xác định một giá trị rõ ràng được trả về bởi subject method trong một example

```ruby
RSpec.describe Array, "with some elements" do
  subject { [1, 2, 3] }

  it "has the expected elements" do
    expect(subject).to eq([1, 2, 3])
  end
end
```

#### TDD

##### Định nghĩa TDD

- `Test Driven Development` là một quy trình viết test hiện đại, còn được hiểu là Red - Green - Refactoring liên tục trong quá trình phát triển phần mềm
- `TDD` chạy theo quy trình sau:
  - Viết 1 test cho hàm mới. Đảm bảo rằng test sẽ fail
  - Chuyển qua viết code sơ khai nhất cho hàm đó để test có thể pass
  - Tối ưu hóa đoạn code của hàm vừa viết sao cho đảm bảo test vẫn pass và tối ưu nhất cho việc lập trình kế tiếp
  - Lặp lại

##### Tác dụng của TDD

- Hiểu được requirement rõ ràng hơn khi nhận specs
- Giảm thiểu được bugs trong quá trình phát triển cũng như bảo trì
- Giảm công sức cho QA, giảm thiểu thời gian test lại sau mỗi lần nâng cấp sản phẩm

### Background job

- Hiểu một cách đơn giản thì background job là một nơi xử lý các luồng, không can thiệp vào luồng chính request/response của 1 trang web, và thực hiện trên server
- Phổ biến nhất là `Sidekiq`, `Resque` và `Delayed Job` sử dụng `Redis` để lưu các queue
