---
author: StevenTTuD
layout: post
title: ActiveRecord - 更新大量資料
published: true
date: 2016-08-03 02:46
tags:
  - Rails
  - ActiveRecord
comments: true

---
### 前言

本文使用的兩種方法，實際上都是用一個 sql 插入或更新所有的資料。
原因是使用其他的方法都沒有使用一個sql插入快。
如果插入的筆數過多，需要調整 sql buffer 的大小。
本例子的情景是一次更新100筆資料，資料量不大，所以不會遇到這個問題。

### 方法一：純 SQL

1. 因為欄位很多，我要傳送的欄位又是完整的一個不少，所以我用 `Model.attribute_names` 來組合要傳入的欄位名稱。
1. 然後將要更新的 Hash 組成 `VALUES (x1, y1, z1, ...), (x2, y2, z2, ...), ...`字串
1. 最後將要更新的欄位組成 `flag_string=VALUES(flag_string)` 這種格式

想要組出的 sql

```rb
    ActiveRecord::Base.connection.execute("
      INSERT INTO linkouts (#{linkouts_attr_names_string})
        VALUES #{query_string}
        ON DUPLICATE KEY UPDATE #{equal_val_string}
    ")
```
組出來的結果

query string 這邊比較難處理，因為是 raw sql，如果在 integer 型態的欄位塞入 `''`，或是在字串的欄位沒有用引號括起來都會噴錯。這也是使用 raw sql 撰寫的缺點。

```terminal

[1] pry(#<MastersController>)> linkouts_attr_names_string
=> "id,kind,report_id,keyword_id,ezpd_id,url,name,price,full_match,created_at,updated_at,merchant_price,price_check,status,merchant_id,remark,out_of_stock,flag_at,flag_string,remark_status"

[2] pry(#<MastersController>)> query_string
=> "(22442,'0',8,126,'https://www.etungo.com.tw/inside/413/419/608/20711.html','【CLENSURE可蘭秀】美容離子導入儀 (SNOWY)',1580,1/1,'2016-08-01 17:31:20','2016-08-01 18:01:52',2)"

[3] pry(#<MastersController>)> equal_val_string
=> "kind=VALUES(kind),report_id=VALUES(report_id),keyword_id=VALUES(keyword_id),ezpd_id=VALUES(ezpd_id),url=VALUES(url),name=VALUES(name),price=VALUES(price),full_match=VALUES(full_match),created_at=VALUES(created_at),updated_at=VALUES(updated_at),merchant_price=VALUES(merchant_price),price_check=VALUES(price_check),status=VALUES(status),merchant_id=VALUES(merchant_id),remark=VALUES(remark),out_of_stock=VALUES(out_of_stock),flag_at=VALUES(flag_at),flag_string=VALUES(flag_string),remark_status=VALUES(remark_status)"
```

完整程式碼：

```rb
linkouts_attr_names = Linkout.attribute_names
    linkouts_attr_names_string = linkouts_attr_names.join(",")
    string_arr = []

    bm[:linkout_list].each_with_index do |l, i|

      l["status"] = 2
      l["flag_string"] = nil
      l["flag_at"] = nil

      if i == 0 # 這邊只拿出一組來組合sql，以免sql太亂難除錯
        current_arr = []
        l.each_pair do |k, v|
          if k == "created_at" || k == "updated_at"
            current_arr << + "'" + v.strftime('%Y-%m-%d %H:%M:%S') + "'"
          elsif v.class == 'boolean'
            "#{v}"
          elsif v.to_i > 0
            current_arr << "#{v}"
          elsif v != nil
            current_arr << "'#{v}'"
          end
        end
        string_arr << "(" + current_arr.join(",") + ")"

      end
    end

    query_string = string_arr.join(",")
    linkouts_attr_names.delete("id")
    equal_val_string = linkouts_attr_names.inject([]) do |result, attr|
      result << "#{attr}=VALUES(#{attr})"
    end
    equal_val_string = equal_val_string.join(",")
    ActiveRecord::Base.connection.execute("
      INSERT INTO linkouts (#{linkouts_attr_names_string})
        VALUES #{query_string}
        ON DUPLICATE KEY UPDATE #{equal_val_string}
    ")
```

這樣組出的 SQL 結果如下

```
INSERT INTO linkouts (id,kind,report_id,keyword_id,ezpd_id,url,name,price,full_match,created_at,updated_at,merchant_price,price_check,status,merchant_id,remark,out_of_stock,flag_at,flag_string,remark_status)
        VALUES (22442,'0',8,126,'https://www.etungo.com.tw/inside/413/419/608/20711.html','【CLENSURE可蘭秀】美容離子導入儀 (SNOWY)',1580,1/1,'2016-08-01 17:31:20','2016-08-01 18:01:52',2)
        ON DUPLICATE KEY UPDATE kind=VALUES(kind),report_id=VALUES(report_id),keyword_id=VALUES(keyword_id),ezpd_id=VALUES(ezpd_id),url=VALUES(url),name=VALUES(name),price=VALUES(price),full_match=VALUES(full_match),created_at=VALUES(created_at),updated_at=VALUES(updated_at),merchant_price=VALUES(merchant_price),price_check=VALUES(price_check),status=VALUES(status),merchant_id=VALUES(merchant_id),remark=VALUES(remark),out_of_stock=VALUES(out_of_stock),flag_at=VALUES(flag_at),flag_string=VALUES(flag_string),remark_status=VALUES(remark_status)
    )
```

### 方法二：使用 AcitveRecord-import

AcitveRecord Import 是一個專門用來批次新增或是修改資料的 gem。
用法很簡單，在原本的 Model 後面加上要新的陣列，並指定要更新的欄位即可。
要更新的欄位也支援使用 sql 語句。

```rb
Model.import array_to_be_update, on_duplicate_key_update: [:title]
Model.import [book1, book2], on_duplicate_key_update: "author = values(author)"
```

### 結論

組 Sql 不需要先把資料包成物件，效能會比較好。
不過相對來說需要多花一些時間處理資料欄位型態的問題，
而本例中的 mysql 因為是一個指令加上要更新的欄位可能很多，
所以 debug 的難度比較高。
這時候就看取捨了，如果初期趕著功能上線可以先用 activeRecord-import 來寫，
當遇到效能需要最佳化的時候再改成純 sql 會是比較好的處理方式。


