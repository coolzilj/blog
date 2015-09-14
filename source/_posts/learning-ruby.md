title: Ruby 学习记录 (更新中)
date: 2015-09-07 10:53:17
tags: ruby
categories: 学习记录
---

<blockquote class="blockquote-center">我的初恋💗</blockquote>

## 常用命令
1. `rails generate scaffold User name:string email:string`
2. `bundle exec rake db:migrate `
之所以使用`bundle exec rake`，而不是使用`rake`，是希望使用在Gemfile指定的rake版本
3. `rails new sample_app --skip-test-unit`
4. `rails server -p 7777` rails启动使用其他端口
5. `rails console —sandbox` 保证退出console的时候数据会清空
6. `rails g model Post title:text content:text -p`
`-p` 表示 `pretend`会列出来这个命令会执行什么操作，会生成什么文件，但不会去真正执行这个命令。
<!-- more -->

----------

## 最佳实践
1. [Why is it bad style to `rescue Exception => e` in Ruby?](http://stackoverflow.com/questions/10048173/why-is-it-bad-style-to-rescue-exception-e-in-ruby)

---

## 文章
1. [Weird Ruby Part 1: The Beginning of the End](https://blog.newrelic.com/2014/11/13/weird-ruby-begin-end/)

---

## Rails
### 1. 写route的时候，action并不是必须的，view才是必须的
如项目中的choose页面，routes里面指定了，

    match "/choose" => "decks#choose", :as => :choose

因为不需要传过去views任何变量，我们就不需要在 decks_controller 里面定义 choose 方法，只要views文件夹里面有choose.html.haml 文件即可。
[参考文章：](http://stackoverflow.com/questions/4477061/rails-3-0-adding-new-action-to-a-controller)

Add this to routes.rb:

    get 'mycontroller/foobar'

This will route the URL http://mysite.com/foobar to the foobar action using HTTP GET.

Some more info:
- Note that defining a def foobar in the controller is not a strict requirement (unless you need to do something in foobar before the view is displayed) - but the view must exist. In other words, even if def foobar method does not exist in the controller, the view foobar.html.erb will still be rendered.
- Here is a good overview of routes in Rails 3.
- Also, in case you don't already know, you can list all the routes that you app knows about usingrake routes. Consequently, if the output of rake routes does not list the route to some controller/action, then the 'No route matches' error will occur.

### 2. Array/Hash 都继承了 Enumerable 的方法，但`map`,`collect`,`map!`,`collect!` 只有Array才支持
Hash想实现则需要 `inject` 或者 `each_with_object`, 破坏性方法可以用 `each`

    my_hash = { a: 'foo', b: 'bar' }
    # => {:a=>"foo", :b=>"bar"}
    a_new_hash = my_hash.inject({}) {|h, (k, v)| h[k.upcase] = v.upcase; h }
    # => {:A=>"FOO", :B=>"BAR"}
    # OR
    a_new_hash = my_hash.each_with_object({}) {|(k,v), h| h[k.upcase] = v.upcase }
    # => {:A=>"FOO", :B=>"BAR"}
    # OR
    a_new_hash = Hash[my_hash.map { |k, v| [k.upcase, v.upcase] }]
    # => {:A=>"FOO", :B=>"BAR"}
    # OR破坏性,不过不能修改key
    my_hash.each {|k,v| my_hash[k] = v.upcase }
    # => {:a=>"FOO", :b=>"BAR"}

### 3. Hash to URL query
`to_query`:

    "http://www.example.com?" + { language: "ruby", status: "awesome" }.to_query
    # => "http://www.example.com?language=ruby&status=awesome"

`CGI.parse`:

    require 'cgi' # Only needed for IRB, Rails already has this loaded
    CGI::parse "language=ruby&status=awesome"
    # => {"language"=>["ruby"], "status"=>["awesome"]}
    Both methods support nested values.

### 4. rails console tips

  `_` 返回前一个输出，如：`a = [1,2,3] # => [1,2,3] _ # => [1,2,3]`

### 5. 非rails项目使用 ActiveSupport

  `require "active_support/all"`

Since using Rails should handle this automatically I'm going to assume you're trying to add ActiveSupport to a non-Rails script.

ActiveSupport's methods got broken into smaller groups in Rails 3, so we don't end up loading a lot of unneeded stuff with a simple `require 'activesupport'`. Now we have to do things like require `'active_support/core_ext/object/blank'`

If you don't care about granularity, you can choose to load bigger chunks. If you want everything in one big gulp use...

For 1.9.2:

    rvm 1.9.2
    irb -f
    irb(main):001:0]]
    >
    require 'active_support/all'
    =>true
    irb(main):002:0]]
    >
    1.week.ago
    =>2010-11-1417:56:16-0700


For 1.8.7:

    rvm 1.8.7
    irb -f
    irb(main):001:0]]
    >
    require 'rubygems'
    =>true
    irb(main):002:0]]
    >
    require 'active_support/all'
    =>true
    irb(main):003:0]]
    >
    1.week.ago
    =>SunNov1417:54:19-07002010

### 6. Rspec

1. 准备数据 `bundle exec rake db:test:prepare`
2. 跑起来 `rspec` / `rspec spec/models/upyun_file_spec.rb`

### 7. Unicorn

在`development`得时候，unicorn默认的log是像nginx一样的，没有多大得debug价值，要想webrick那样的log只好自己动下手了。
[参考链接](http://dave.is/unicorn.html)：

使用`rails server`一样的logger:

    # config/environments/development.rb
    Hackerschool::Application.configure do
      # ...
      config.logger = Logger.new(STDOUT)
      config.logger.level = Logger.const_get(
        ENV['LOG_LEVEL'] ? ENV['LOG_LEVEL'].upcase : 'DEBUG'
      )
    end

去掉unicorn默认的类nginx的logger：

    $ RACK_ENV=none RAILS_ENV=development unicorn -c config/unicorn.rb
    # If you’re using foreman to run your app locally, you can put these into your .env file.


----------


## 代码片段

### 1. 找出notes字段长度小于8的记录
`Photographer.where("CHARACTER_LENGTH(notes) < 8")`

### 2. 检查数组是不是全部非空白
`["", nil].all?(&:blank?)`

### 3. 在console删除记录
`User.find(2).destroy`

### 4. use `begin..end` for cached value
```ruby
def cached_value
  @cache ||= begin
    # a multi-line calculation
    # of cache here, ending with
    # the value
  end
end
```

----------


## Ruby

### Hash

#### {}省略的情况

如果方法最后一个参数是hash，`{}`可以省略掉，如：

    link_to(body, url, html_options = {})
    link_to "Click me", object_path, :class=>"my_css_class", :id=>"my_css_id"
    link_to "Click me", object_path, { :class=>"my_css_class", :id=>"my_css_id" }

#### rocket style 是不是已经废弃掉了？

Ref: [JSON STYLE 吐槽](http://logicalfriday.com/2011/06/20/i-dont-like-the-ruby-1-9-hash-syntax/)
[RubyChina翻译文](http://ruby-china.org/topics/2662)

（1.9版本引入JSON STYLE）当然不是啦，有些时候还一定要使用 rocket style！

1. You must use the rocket for symbols that require quoting:

     有效：`:'where.is' => x`

     无效：`'where.is': x`
2. You must use the rocket for symbols that are not valid labels:

    有效：`:$set => x`

    无效：`$set: x`
3. You must use the rocket if you use keys in your Hashes that aren't symbols:

    有效：`'s' => x`

    无效：`'s': x`
4. 变量当然也不行，如：

    def generate_token(column)
      begin
        self[column] = SecureRandom.urlsafe_base64
      end while User.exists?(column => self[column])
    end
    这里的 column => self[column]
    不能写成
    column: self[column]
    因为column是一个变量，而不是symbol

总结来说，JSON style 只是key是symbol的hash的一种简洁写法，使用范围有效，但也足够，因为hash都提倡key用symbol。

#### 为什么提倡使用symbol作为hash的key？

[StackOverflow Ref](http://stackoverflow.com/questions/8189416/why-use-symbols-as-hash-keys-in-ruby)


### 元编程

1. 查看某个类的类方法：`OauthUser.methods`

2. 查看某个类的实例方法： `OauthUser.instance_methods`

3. 查看某个类有没有含有特定关键字的方法： `OauthUser.methods.grep /account/`

4. 类也是一个对象，是 **Class** 类的实例，类方法实质上是储存在 **eigenclass** 里面的单件方法

5. `extend self`的使用（参考balalaika的 **legacy_migrate.rb** ）

  一般用在Module中，因为Module无法实例化，所以想像调用类方法一样调用实例方法，就extend 自己，extend 本质上等同于
  ```
  class << self
    include Module
  end
  ```
  如：
  ```
  module M
    extend self      ##等同于 extend M
    def greeting
      puts "hi!"
    end
  end
  ```
  这样的话就可以调用 `M.greeting`, greeting既是实例方法，也是单件方法。
  如果只是单纯想调用 `M.greeting`的话，也可以这样写
  ```
  module M
    def self.greeting
      puts "hi!"
    end
  end
  ```
  这时候greeting只是一个类方法（单件方法）

6. extend vs include

  `extend Module` => 类方法

  `include Module` => 实例方法


### 操作符

1. `&` 符号

    - `&`符号是把Proc转换成Block的操作符，

            feet_over_4 = Proc.new { |height| height >= 4 }
            some_array.collect(&feet_over_4) #注意是括号而不是大括号

    - `&:symbol`

        因为`&`是把Proc转换成Block，所以`:symbol`要先转换成Proc

            def to_proc
                proc { |obj, *args| obj.send(self, *args) }
            end

        所以

            some_array.map{|a| a.upcase}

        等于

            some_array.map(&:upcase)


### Block, Proc, Lambda

#### Block

`Block` 只是一些代码片段集合，不是`object`

#### Proc

`Proc` 是对象，拥有对象所有特性

#### Lambda

`Lambda` 也是对象

#### Proc vs Lambda

Another important but subtle difference is in the way procs created with lambda and procs created with Proc.new handle the return statement:

In a lambda-created proc, the return statement returns only from the proc itself
In a Proc.new-created proc, the return statement is a little more surprising: it returns control not just from the proc, but also from the method enclosing the proc!
Here's lambda-created proc's return in action. It behaves in a way that you probably expect:

    def whowouldwin

        mylambda = lambda {return "Freddy"}
        mylambda.call

        # mylambda gets called and returns "Freddy", and execution
        # continues on the next line

        return "Jason"

    end


    whowouldwin
    => "Jason"
Now here's a Proc.new-created proc's return doing the same thing. You're about to see one of those cases where Ruby breaks the much-vaunted Principle of Least Surprise:

    def whowouldwin2

      myproc = Proc.new {return "Freddy"}
      myproc.call

      # myproc gets called and returns "Freddy",
      # but also returns control from whowhouldwin2!
      # The line below *never* gets executed.

      return "Jason"

    end


    whowouldwin2
    => "Freddy"
Thanks to this surprising behaviour (as well as less typing), I tend to favour using lambda over Proc.new when making procs.

### Module Mixin

`Module/Class`一般包含3种方法，`Macro`, `ClassMethods`, `IntanceMethods`
`Macro`其实就是执行省略掉self的方法，如:

    has_many :tags
    等于
    self.has_many :tags

#### 经典做法

    module TagLib

      module ClassMethods
        def find_by_tags()
          # ...
        end
      end

      module InstanceMethods
        def tags()
          # ...
        end
      end

      def self.included(base)
        base.send :include, InstanceMethods
        base.send :extend, ClassMethods
        base.class_eval do
          #这里使用intance_eval也是同样的效果，因为self都是base
          logger.warn...
        end
      end

    end

    class ActiveRecord::Base
      include TagLib
    end

#### 善用ActiveSupport::Concern

##### Rail3

    module TagLib
      extend ActiveSupport::Concern

      included do
        logger.warn ...
      end

      module ClassMethods
        def find_by_tags()
          # ...
        end
      end

      module InstanceMethods
        def tags()
          # ...
        end
      end
    end

    class ActiveRecord::Base
      include TagLib
    end

##### Rails4

不再需要把实例方法包在`Module InstanceMethods`里，直接写在外面就可以

    module TagLib
      extend ActiveSupport::Concern

      included do
        logger.warn...
      end

      module ClassMethods
        def find_by_tags()
          # ...
        end
      end

      def tags()
        # ...
      end

    end

    class ActiveRecord::Base
      include TagLib
    end

##### self.included(base) 是怎么回事？

这个方法是在module被include的时候执行的，base就是上下文对象，如：

    module B
      def self.included(base)
        puts base
      end
    end

    module A
      include B
    end
    => A

    所以上面例子的

    def self.included(base)
    base.send :include, InstanceMethods
    base.send :extend, ClassMethods
    base.class_eval do
        #这里使用intance_eval也是同样的效果，因为self都是base
        logger.warn...
    end
    end

    base 就是 ActiveRecord::Base， 所以 intance_eval 和 class_eval
    的效果都是一样的，

    参考下面的 `instance_eval` vs `class_eval`



### `instance_eval` vs `class_eval`

     Foo = Class.new
     Foo.instance_eval { puts self }
     => Foo
     Foo.class_eval { puts self }
     => Foo

     foo = Foo.new
     foo.instance_eval { puts self }
     => #<Foo:0x007fc49aa42ea8>
     => 可用于创造单例方法

     foo.class_eval { puts self }
     => #<Class:#<Foo:0x007fc49aa42ea8>>
     => 这个应该是等于Foo吧？这样就相当于打开类了，可用于创造实例方法


