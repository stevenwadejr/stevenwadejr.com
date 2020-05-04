---
title: "TIL how to use preg_filter() to add a prefix or suffix to array elements"
date: 2020-05-02T22:17:47-04:00
draft: true
tags:
  - QuickTip
---

You can add a prefix or suffix to every element of an array succinctly using `preg_filter()`.<!--more-->

```php
<?php
$ids = range(1, 5);

$prefixedIds = preg_filter('/^/', 'prefix_', $ids);
$suffixedIds = preg_filter('/$/', '_suffix', $ids);

print_r($prefixedIds);
print_r($suffixedIds);
```

The print statements above will output:

```
Array
(
    [0] => prefix_1
    [1] => prefix_2
    [2] => prefix_3
    [3] => prefix_4
    [4] => prefix_5
)

Array
(
    [0] => 1_suffix
    [1] => 2_suffix
    [2] => 3_suffix
    [3] => 4_suffix
    [4] => 5_suffix
)
```

Simple and clean solution for prefixing or appending to every element in an array.

Credit goes to [Dávid Horváth](https://stackoverflow.com/users/3948862/d%c3%a1vid-horv%c3%a1th) and his [answer on StackOverflow](https://stackoverflow.com/a/28115783).
