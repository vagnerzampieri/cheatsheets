# Ruby

#### is_power_of_two:

The best performance to resolve this problem

```ruby
  def is_power_of_two?(num)
    num != 0 && (num & (num - 1)) == 0
  end
```

##### references:

  - https://stackoverflow.com/questions/600293/how-to-check-if-a-number-is-a-power-of-2
