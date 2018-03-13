# Ruby >= 2.5

#### yeld_self and &method example:

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
