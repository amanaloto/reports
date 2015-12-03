# Handling `nil`
---
## Problem / Motivation
- We get `NoMethodError` a lot.
- `nil` could _mean_ a lot of things.

## Solutions
Example method:
``` ruby
class JamesReid
  def hello
    puts 'hello'
  end
end

class CrossFitMan
  def hello
    puts 'CrossFit is the best!'
  end
end

def get_mr_right type
  case type
  when :pogi
    JamesReid.new
  when :athletic
    CrossFitMan.new
  else
    nil
  end
end

get_mr_right(:pogi).hello
get_mr_right(:macho).hello #NoMethodError
```

1. Raise an error.
```ruby
# case block...
else
  raise 'Walang bagay sa iyo!'
end
```

2. Use `#try`.
```ruby
get_mr_right(:macho).try :hello
```

3. Use __NullObjectPattern__
```ruby
class NoMrRight
  def hello
    puts 'No more fish in the sea.'
  end
end

def get_mr_right
  # case block...
  else
    NoMrRight.new
  end
end

get_mr_right(:bla).hello
```

More examples:

a.
```ruby
class NoMrRight
  def method_missing *args, &block
    nil
  end
end

NoMrRight.new.hello #nil
```

b.
```ruby
class NoMrRight
  def method_missing *args, &block
    self
  end
end

NoMrRight.new.foo.bar.bong.bong.dantes #NoMrRight instance
```

### NullObjectPattern Caveats
- Too much typing
- Creation of 'empty' classes
- Impossible to simulate 'falsiness' in Ruby

References:
- https://robots.thoughtbot.com/rails-refactoring-example-introduce-null-object
- https://robots.thoughtbot.com/if-you-gaze-into-nil-nil-gazes-also-into-you
- https://en.wikipedia.org/wiki/Null_Object_pattern
- http://devblog.avdi.org/2011/05/30/null-objects-and-falsiness/
