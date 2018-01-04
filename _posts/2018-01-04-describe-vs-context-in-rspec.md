---
title: describe vs context in Rspec
subtitle: 釐清Rspec中describe context的用法差異
categories: Ruby, Rspec
---

In Rspec world, you often see people using both “describe” blocks and “context” blocks together, like this

```ruby
describe "launch the rocket" do
  context "all ready" do
  end

  context "not ready" do
  end
end
```

So what’s the difference between “describe” and “context” really?

According to the rspec source code, “context” is just a alias method of “describe”, meaning that there is no functional difference between these two methods. However, there is a contextual difference that’ll help to make your tests more understandable by using both of them.

The purpose of “describe” is to wrap a set of tests against one functionality while “context” is to wrap a set of tests against one functionality under the same state. Here’s an example

```ruby
describe "launch the rocket" do
  before(:each) do
    #prepare a rocket for all of the tests
    @rocket = Rocket.new
  end

  context "all ready" do
    before(:each) do
      #under the state of ready
      @rocket.ready = true
    end

    it "launch the rocket" do
      @rocket.launch().should be_true
    end
  end

  context "not ready" do
    before(:each) do
      #under the state of NOT ready
      @rocket.ready = false
    end

    it "does not launch the rocket" do
      @rocket.launch().should be_false
    end
  end
end
```

This code is more readable than wrapping all the stuffs with “describe” blocks. Because when you read the tests under “context”, you know they are all testing the same thing the “describe” talk about. And you know a “context” is to setup the state of your object, in this case @rocket.ready which will effects the behavior of @rocket.launch() that gives you a conclusion, right away, without scanning the code back and forth.
