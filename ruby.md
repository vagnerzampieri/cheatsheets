# Ruby >= 2.1

#### elapsed time

```ruby
  time = Process.clock_gettime(Process::CLOCK_MONOTONIC)
  # => 2810266.714992
  time / (24 * 60 * 60.0) # time / days
  # => 32.52623512722222

  starting = Process.clock_gettime(Process::CLOCK_MONOTONIC)
  # time consuming operation
  ending = Process.clock_gettime(Process::CLOCK_MONOTONIC)
  elapsed = ending - starting
  elapsed # => 9.183449000120163 seconds
```

##### references:

 -   https://blog.dnsimple.com/2018/03/elapsed-time-with-ruby-the-right-way/


# Ruby >= 2.5

#### yeld_self and &method:

```ruby
  def call
    base_relation.
      join(:care_periods).
      yield_self(&method(:hospital_clause))
  end

  private

  def hospital_clause(relation)
    if clause
      relation.where(...)
    else
      relation
    end
  end
```

##### references:

 -   https://robots.thoughtbot.com/using-yieldself-for-composable-activerecord-relations
