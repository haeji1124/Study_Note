---
layout: post
title: "Rails on Ruby 시작하기"
date: 2023-09-25
desc: "Rails on ruby 프레임워크 사용법"
keywords: "ruby,rails,rails on ruby, website,blog,easy"
categories: [Web]
tags: [ruby, rails on ruby]
icon: icon-ruby-on-rails-alt
typora-root-url: ../
---
<span style='color:blue' >출처 : 판다코인 Rais on Ruby 강의</span>

# 프레임 워크란?
- `Framework?`  
  'FRAME 프레임(틀, 규칙or법칙)'+'WORK 워크(일, 소프트웨어의 목적)'  
  하나의 어플리케이션을 구축할 때, 모든 애플리케이션의 공통적인 개발 환경을 제공해주는 것, 개발에 필요한 화면구현, DB연동, 개발환경들에 공통적인 부분을 제공함으로써 개발 시간과 리소스 비용을 절감해 생산성을 높여주는 것  
  개발자는 애플리케이션 프레임워크가 제공하는 기초적인 코드 위에 독자적인 코드를 추가하여 일정한 품질을 가진 애플리케이션을 쉽게 생성할 수 있음.  

- `vs Library`  
    https://www.youtube.com/watch?v=D_MO9vIRBcA

    "재사용이 가능한 클래스"  

라이브러리 : 사용자 코드에서 호출되어야 함.
프레임워크 : 프레임워크 스스로가 사용자 코드를 호출함.

* **제어반전 :** 프로그램의 실행 주체가 역전되는 것  
Rails는 이러한 애플리케이션 프레임워크 중에서도 웹 애플리케이션을 개발하기 위한 웹 애플리케이션 프레임워크(Web Application Framework; WAF)

- 왜 프레임워크를 사용하는가?
1. 개발 생산성이 향상된다.
2. 유지보수성이 뛰어나다.
3. 최신 기술 트렌드에 대응하기 쉽다.
4. 일정 이상의 품질을 기대할 수 있다.

* `항상 프레임워크의 도입이 옳은 선택인가?`  
프레임워크 = 규칙의 집합, 규칙을 이해하기 위한 일정 수준 이상의 학습이 필요
일회용/소규모 어플리케이션의 경우 프레임워크 도입을 고려해보아야 함.

![](./img/)
![](./img/)

---
# 개발환경 구축
루비는 리눅스에서 잘된대… 그래서 개발환경 따로 설치 필요(참조 : https://haereeroo.tistory.com/8)

1. power shell을 켜서 다음 명령어 실행  
`Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux`
2. 윈도우 재시작
3. microsoft store에서 ubuntu 다운
4. 우분투 실행하여 아래 명령어로 dependency 설치
    
    ```powershell
    sudo apt-get update
    sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev
    ```
    

1. 루비는 버전관리 툴인 rbenv나 rvm을 사용해 설치할 수도 있고 직접 설치 할 수도 있습니다. 저는 가장 무난한 방법인 rbenv를 사용했습니다. 먼저 rbenv를 설치해줍니다.
    
    ```powershell
    git clone https://github.com/rbenv/rbenv.git ~/.rbenv
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(rbenv init -)"' >> ~/.bashrc
    exec $SHELL
    ```
    

1. 그리고 ruby-build를 설치합니다.
    
    ```powershell
    git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
    echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
    exec $SHELL
    ```
    

1. rbenv로 루비를 설치하고 사용할 루비 버전을 설정해줍니다. 저는 2.6.5를 설치했습니다.
    
    ```powershell
    rbenv install 2.7.7
    rbenv global 2.7.7
    ruby -v
    # 
    ```
    

1. bundler를 설치해줍니다.
    
    ```powershell
    gem install bundler
    rbenv rehash
    ```
    

### 레일즈 설치

1) node js를 설치합니다.

- rails 개발을 하는데 node js를 설치하는 게 뜬금 없지만 해야합니다!  
rails가 Asset(CSS, Javascript, Image 등)파일들을 합치고 압축해주는 asset pipeline을 컴파일할 때 자바스크립트 런타임이 필요하기 때문입니다.  
그리고 우분투 위에서 레일즈 개발을 할때에는 자바스크립트 런타임으로 node js를 쓰는게 가장 좋다고 합니다.

```powershell
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y node.js
```

2) 이제 드디어 레일즈를 설치합니다.

```powershell
gem install rails -v 6.0.2.1
rbenv rehash
rails -v
# Rails 6.0.2.1
```

### 테스트

```powershell
rails new sample_project
rails s
```

# 개발 시작하기
## controller 생성

```ruby
rails generate controller blog 
# blog는 컨트롤러의 "이름"
# 자동으로 컨트롤러 파일과 views 하위에 blog 폴더가 생성된다

```

만약 generate 안되고 다음과 같은 에러 나올시 `bundle update`를 실행
```
Running `bundle update` will rebuild your snapshot from scratch, using only
the gems in your Gemfile, which may resolve the conflict.
```

`config/routes.rb`

```ruby
Rails.application.routes.draw do
 	root :to => "blog#index" #메인페이지  root url은 blog컨트롤러의 index액션과 연결됨
	get "/home" => "blog#index" #/home
	get "/test" => "blog#test" #/test
end
```

routers.rb에 route 정보를 기입한다.

`app/controllers/blog_controller.rb`

```ruby
class BlogController < ApplicationController
	def index
	end
	
	def test
	end
end
```

`app/views/blog/index.erb`

```ruby
<center>
	<h1>
		안녕하세요! <br/>
		blog폴더의 index.erb입니다!<br/>
	</h1>
</center>
```

`app/views/test.erb`

```ruby
<center>
	<h1>
		안녕하세요! <br/>
		blog폴더의 test.erb입니다!<br/>
	</h1>
</center>
```

### Controller에서 View로 Ruby 변수 전달하기
    
<span style="color:red"> Ruby 변수는 "@"로 선언할 수 있다.</span>
    

`app/controllers/blog_controller.rb`

```ruby
class BlogController < ApplicationController
	
	def index
		
	end
	
	def test
		@temp = "안녕하세요!"
	end
	
end
```

<span style="color:red"> **View 파일은 <%= %> 로 전달받은 변수를 출력할 수 있다.**</span>

`app/views/test.erb`

```ruby
<center>
	<p>여기는 test.erb입니다!</p>
	<p> <%= @temp %> </p>
</center>
```

<span style="color:red"> '=' 을 생략한다면, view파일이 변수를 가지고 있으나, 출력은 하지 않는다.</span>

---
## form 태그로 정보 주고받기
HTML : Form태그

```html
<form action="" method="get" class="form-example">
  <div class="form-example">
    <label for="name">Enter your name: </label>
    <input type="text" name="name" id="name" required>
  </div>
  <div class="form-example">
    <label for="email">Enter your email: </label>
    <input type="email" name="email" id="email" required>
  </div>
  <div class="form-example">
    <input type="submit" value="Subscribe!">
  </div>
</form>
<!--https://developer.mozilla.org/ko/docs/Web/HTML/Element/form-->
```

controller 생성

```ruby
rails generate controller calculator main
# blog는 컨트롤러의 "이름", main은 액션의 "이름"
```

config/routes.rb

```ruby
Rails.application.routes.draw do
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
	root :to => "calculator#main"
	get "/home" => "blog#index"
	get "/test" => "blog#test"
	post "/result" => "calculator#result"
	get "/plus/:num1/:num2" => "calculator#plus"
end
```

app/controllers/calculator_controller.rb

```ruby
class CalculatorController < ApplicationController
  	def main
  	end
	
	def result
		@fristNum = params[:num1].to_i
		@secondNum = params[:num2].to_i
		@resultNum = @fristNum + @secondNum
		
	end
	
	def plus
		@fristNum = params[:num1].to_i
		@secondNum = params[:num2].to_i
		@resultNum = @fristNum + @secondNum
		
	end
end
```

app/views/calculator/main.erb

```ruby
<form action="/result" method="post">
	<input type="number" name="num1" />
	<span>+</span>
	<input type="number" name="num2" />
	<button type="submit">제출하기</button>
</form>
```

app/views/calculator/result.erb

```ruby
<p>결과페이지입니다.</p>
<p>
	결과는 <span><%= @resultNum %> 입니다.</span>
</p>
```

app/views/calculator/plus.erb

```ruby
<p>
	결과는 <span><%= @resultNum %> 입니다.</span>
</p>
```

Post 시 **ActionController::InvalidAuthenticityToken Error 가 발생한다면**

https://heewon26.tistory.com/272

참고하여 controller에 **protect_from_forgery를 기입하고 view head에** <**%=** csrf_meta_tags %>를 넣어준다.
