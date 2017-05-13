---
layout: post
title: 'Generate Safety Query String In ActiveRecord '
published: true
date: 2016-07-07 07:46
tags:
  - Rails
  - ActiveRecord
comments: true

---
## Solution 1: sanitize_sql

```rb
ActiveRecord::Base.send(:sanitize_sql,["select * from my_table where description='%s' and id='%s'","mal'formed", 55], "my_table")
=> "select * from my_table where description='mal\\'formed' and id='55'"
```

Can also use instance method to do the same thing

```rb
MyTable.send(:sanitize_sql,["select * from my_table where description='%s' and id='%s'","mal'formed", 55])
=> "select * from my_table where description='mal\\'formed' and id='55'

```

## Solution 2: ActiveRecord::Base.connection.quote

ActiveRecord::Base.connection.quote only escape quotes of query. So... it's not a good solution.

```rb
[17] pry(main)> ActiveRecord::Base.connection.quote(["select * from my_table where description='%s' and id='%s'","mal'formed", 55])
=> "'---\\n- select * from my_table where description=\\'%s\\' and id=\\'%s\\'\\n- mal\\'formed\\n- 55\\n'"
```

```rb
[20] pry(main)> ActiveRecord::Base.connection.quote("select * from my_table where description='%s' and id='%s'")
=> "'select * from my_table where description=\\'%s\\' and id=\\'%s\\''"
```

## Solution 3: sanitize_sql_array

When you using sanitize_sql_array, the output will the same as when you using sanitize_sql.
But your dont need to send useless table name in third parameter anymore.

```rb
[24] pry(main)> ActiveRecord::Base.send(:sanitize_sql_array, ["select * from my_table where description='%s' and id='%s'","mal'formed", 55])
=> "select * from my_table where description='mal\\'formed' and id='55'"
```

If we call sanitize_sql_array not through send, you'll get error message ` protected method `sanitize_sql_array' called for ActiveRecord::Base:Class`, So we have to use send to avoid this situation.

```rb
[25] pry(main)> ActiveRecord::Base.sanitize_sql_array( ["select * from my_table where description='%s' and id='%s'","mal'formed", 55])
NoMethodError: protected method `sanitize_sql_array' called for ActiveRecord::Base:Class
from /Users/Steven/.rvm/gems/ruby-2.2.2/gems/activerecord-4.2.6/lib/active_record/dynamic_matchers.rb:26:in `method_missing'
```