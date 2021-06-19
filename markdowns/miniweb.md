---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# some information about the slides, markdown enabled
info: |
  ## 微型web框架的实现原理简单分析

---
# 微型web框架的实现原理

Rack + DSL

oliver.chen
---

# 一个Web应用需要什么？

<v-clicks>

1. 能够响应 http 请求
2. 定义路由及响应内容
3. 尽可能少写点代码
4. ...

</v-clicks>

---

# Ruby世界中，有个Rack

app.rb 文件

```ruby
require 'rack'

app = proc do |env| 
  [200, {'Content-Type' => 'text/html'}, ['Hello, Rack demo']]
end

Rack::Server.start(app: app)
```

执行 `ruby app.rb`, 访问 http://0.0.0.0:9292/


---

# 什么是Rack？


<!-- <div class="grid grid-cols-2 gap-x-4"> -->
<div grid="~ cols-2 gap-2" m="-t-2">
  <div v-click>
    <p> 1. 是一份 <a href="https://github.com/rack/rack/blob/master/SPEC.rdoc">规范</a></p>

  `应用程序`与`应用服务器`之间的接口规范。

  </div>

  <div v-click>
    <p> 2. 是一个 <a href="https://github.com/rack/rack">gem</a></p>

    封装了对HTTP数据的处理。

    比如：解析path、params, 中间件的处理等等。
  </div>
</div>

---

# Rack返回结构

<v-clicks>

- 一个对象，可以相应 `call` 方法 （可以是 proc 或者 自定义对象）
- 接收一个 环境变量 `env` 作为参数
- 返回一个三个元素的数组
  1. HTTP 状态码
  2. HTTP header
  3. 一个响应 each 方法的对象，用来将内容处理为body

</v-clicks>

<br>

<v-click>

```ruby
app = proc do |env| 
  [200, {'Content-Type' => 'text/html'}, ['Hello, Rack demo']]
end
```

</v-click>

---

# 另一种形式

<div class="grid grid-cols-2 gap-x-4">
  <div>

miniweb.rb

```ruby
module Miniweb
 class Base

  def call(env)
    [200, {'Content-Type' => 'text/html'}, ['Hello, MiniApp demo']
  end

 end
end
```
  </div>

  <div>

app.rb
```ruby
require 'rack'
require_relative 'miniweb'

app = Miniweb::Base.new

Rack::Server.start(app: app)
```
  </div>
</div>

---

# 完成了第一步，响应HTTP请求，那下一步？

<v-clicks>

- 需要定义路由
- 路由对应的处理

</v-clicks>

---

# 来看看平时使用 Grape 定义路由的方式


```ruby
desc "字段详情页"
params do 
  requires :field_id, type: String, desc: "字段 ID"
end
get "/form_fields/show" do 
  form_field = RccForm::Metadata::FormField.where(id: params[:field_id]).first 
  raise NotFound if form_field.nil? 
  render_happy(data: form_field.show_json)
end
```

---

# 我们需要的结构

```ruby
get "/show" do
end

post "/create" do
end

put "/update" do
end

delete "/destory" do
end
```

---


# 定义路由收集的方法

miniweb.rb

```ruby
module Miniweb
  class Base
    attr_reader :routes

    def initialize
      @routes = {}
    end

    def get(path, &handler)
      route("GET", path, &handler)
    end

    def post(path, &handler)
      route("POST", path, &handler)
    end

    # verb: get/post/put/delete
    def route(verb, path, &handler)
      @routes[verb] ||= {}
      @routes[verb][path] = handler
    end
  end
end
```

---


# 进行路由收集

app.rb

```ruby {6-14}
require 'rack'
require_relative 'miniweb'

app = Miniweb::Base.new

app.get "/show" do
  p "show me ..."
  [200, {}, ["mini_app show ..."]]
end

app.post "/create" do
  p "create me ..."
  [200, {}, ["mini_app create ..."]]
end

p app.routes
# {
#   "GET"=>{"/show"=>#<Proc:0x00007fe01b162d00@mini_app.rb:31>},
#   "POST"=>{"/create"=>#<Proc:0x00007fe01b162be8@mini_app.rb:35>}
#}
```

---

# call 方法里面获取请求信息

此时，去访问 http://0.0.0.0:9292/show ，还是得到旧的结果。

那是因为我们还没在 `call` 进行路由的判断。

miniweb.rb

```ruby
module Miniweb
 class Base

  # .....

  def call(env)
    [200, {'Content-Type' => 'text/html'}, ['Hello, MiniApp demo']
  end

 end
end
```

查看协议部分 -- https://github.com/rack/rack/blob/master/SPEC.rdoc

---

# 查看 env 的内容

```ruby
{"rack.version"=>[1, 3],
 "rack.errors"=>#<IO:<STDERR>>,
 "rack.multithread"=>true,
 "rack.multiprocess"=>false,
 "rack.run_once"=>false,
 "SCRIPT_NAME"=>"",
 "QUERY_STRING"=>"",
 "SERVER_PROTOCOL"=>"HTTP/1.1",
 "SERVER_SOFTWARE"=>"puma 3.12.6 Llamas in Pajamas",
 "GATEWAY_INTERFACE"=>"CGI/1.2",
 "REQUEST_METHOD"=>"GET",
 "REQUEST_PATH"=>"/show",
 "REQUEST_URI"=>"/show",
 "HTTP_VERSION"=>"HTTP/1.1",
 "HTTP_HOST"=>"0.0.0.0:9292",
 "HTTP_USER_AGENT"=>
  "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.77 Safari/537.36",
 "HTTP_ACCEPT"=>
  "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9",
 "HTTP_ACCEPT_ENCODING"=>"gzip, deflate",
 "HTTP_ACCEPT_LANGUAGE"=>"zh-CN,zh;q=0.9,en;q=0.8,no;q=0.7",
 "HTTP_PURPOSE"=>"prefetch",
 ...
```

---

# 借助 rack 里的工具

<div class="grid grid-cols-2 gap-x-4">
  <div>

初版

```ruby
# miniweb.rb
module Miniweb
  class Base
    def call(env)
      @request = Rack::Request.new(env)
      verb = @request.request_method
      requested_path = @request.path_info

      handler = @routes[verb][requested_path]

      handler.call
    end
  end
end
```
  Rack 中有许多类似 `Rack::Request` 的工具可以使用。
  </div>

  <div>

未找到匹配路由

```ruby {10-15}
# miniweb.rb
module Miniweb
  class Base
    def call(env)
      @request = Rack::Request.new(env)
      verb = @request.request_method
      requested_path = @request.path_info

      handler = @routes[verb][requested_path]

      if handler
        handler.call
      else
        [404, {}, ["url no found ..."]]
      end
    end
  end
end
```

  </div>
</div>


---

最基本的路由匹配已经有了。

<br>
<br>

不过目前对于那种动态路由，还不能正常处理。<br>
而路由的匹配算法，就有很多种了... <br>
有兴趣的小伙伴也可以深挖下，不同语言的不同框架中是如何处理~

<br>
<br>
正则匹配？状态机？基数树？...... 

---

# 让使用者更方便


现在来瞅瞅存在的问题。

app.rb

```ruby
app = Miniweb::Base.new

app.get "/show" do
end

app.post "/create" do
end

Rack::Server.start(app: app)
```

<arrow v-click x1="400" y1="10" x2="260" y2="180" color="#564" width="2" arrowSize="1" />

<arrow v-click x1="400" y1="400" x2="160" y2="300" color="#564" width="2" arrowSize="1" />


更方便地使用？

---

miniweb.rb:
```ruby {5}
module Miniweb 
  class Base 
  end 

  Application = Base.new   # <-- 新增
end
```

app.rb
```ruby {1}
app = Miniweb::Application

app.get "/show" do
end

app.post "/create" do
end

Rack::Server.start(app: app)
```

如果 `get / post` 能直接挂到 `Miniweb::Application` 上，就不用 `app` 了?

---

miniweb.rb
```ruby
module Miniweb
  module Delegator
    def self.delegate(*methods, to:)
      Array(methods).each do |method_name|
        define_method(method_name) do |*args, &block|
          to.send(method_name, *args, &block)
        end

        private method_name
      end
    end

    delegate :get, :post, to: Application
  end
end

include Miniweb::Delegator

```

在引入 `miniweb.rb` 时，就会生成对应方法 `get / post`，
会直接调用 `Application.get / Application.post`。

---

# 于是，变成了

miniweb.rb

```ruby
require 'rack'
require_relative 'miniweb'

get "/show" do
  p "show me ..."
  [200, {}, ["mini_app show ..."]]
end

post "/create" do
  p "create me ..."
  [200, {}, ["mini_app create ..."]]
end

Rack::Server.start(app: Miniweb::Application)
```

只要引入 `miniweb`，然后就可以直接使用 `get / post` 去定义路由了。

---

大概就是这样... 这是 [miniweb gitlab](https://git.rccchina.com/oliver.chen/miniweb) 代码。
<br>
<br>
当然，这里主要讲的是思路，没有太多的细节，功能也很简单。

一些细节还得后面持续研究。比如那个 路由匹配算法....

---

# 资料

- [Rack应用及相关](https://www.yuque.com/chenxiaodong-hvqvm/zbb1us/ke7fps)
- [谈谈 Rack 的协议与实现](https://draveness.me/rack/)
- [lets-build-a-sinatra](https://thoughtbot.com/blog/lets-build-a-sinatra)
