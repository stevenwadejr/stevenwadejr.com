---
_eb_reusable_block_ids: []
_edit_last: "1"
_wp_old_date: "2020-12-15"
author: "Steven Wade"
categories:
  - Engineering
date: "2020-05-02T00:00:00+00:00"
guid: http://stevenwadejr.com/?p=41
parent_post_id: null
post_id: "41"
tags:
  - php
title: TIL how to use preg_filter() to add a prefix or suffix to array elements
url: /2020/05/02/til-how-to-use-preg_filter-to-add-a-prefix-or-suffix-to-array-elements/

---
You can add a prefix or suffix to every element of an array succinctly using `preg_filter()`.

```
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
