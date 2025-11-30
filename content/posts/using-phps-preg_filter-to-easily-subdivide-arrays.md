---
_eb_reusable_block_ids: []
_edit_last: "1"
_oembed_4c092f67c6f1acb9f45e1620e2f36386: '<blockquote class="wp-embedded-content" data-secret="fpdjFZmzRr"><a href="https://stevenwadejr.com/2020/05/02/til-how-to-use-preg_filter-to-add-a-prefix-or-suffix-to-array-elements/">TIL how to use preg_filter() to add a prefix or suffix to array elements</a></blockquote><iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" style="position: absolute; clip: rect(1px, 1px, 1px, 1px);" title="&#8220;TIL how to use preg_filter() to add a prefix or suffix to array elements&#8221; &#8212; Steven Wade" src="https://stevenwadejr.com/2020/05/02/til-how-to-use-preg_filter-to-add-a-prefix-or-suffix-to-array-elements/embed/#?secret=fpdjFZmzRr" data-secret="fpdjFZmzRr" width="600" height="338" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>'
_oembed_46f776e5c94f52b8fbde5ced38b929bb: '<blockquote class="wp-embedded-content" data-secret="hbXS2Z4Qvz"><a href="https://stevenwadejr.com/2020/05/02/til-how-to-use-preg_filter-to-add-a-prefix-or-suffix-to-array-elements/">TIL how to use preg_filter() to add a prefix or suffix to array elements</a></blockquote><iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" style="position: absolute; clip: rect(1px, 1px, 1px, 1px);" title="&#8220;TIL how to use preg_filter() to add a prefix or suffix to array elements&#8221; &#8212; Steven Wade" src="https://stevenwadejr.com/2020/05/02/til-how-to-use-preg_filter-to-add-a-prefix-or-suffix-to-array-elements/embed/#?secret=tEzIKiIKkC#?secret=hbXS2Z4Qvz" data-secret="hbXS2Z4Qvz" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>'
_oembed_2108247391591c79b51447feaa0a6701: '<blockquote class="wp-embedded-content" data-secret="9VRrozAWkl"><a href="https://stevenwadejr.com/2020/05/02/til-how-to-use-preg_filter-to-add-a-prefix-or-suffix-to-array-elements/">TIL how to use preg_filter() to add a prefix or suffix to array elements</a></blockquote><iframe class="wp-embedded-content" sandbox="allow-scripts" security="restricted" style="position: absolute; clip: rect(1px, 1px, 1px, 1px);" title="&#8220;TIL how to use preg_filter() to add a prefix or suffix to array elements&#8221; &#8212; Steven Wade" src="https://stevenwadejr.com/2020/05/02/til-how-to-use-preg_filter-to-add-a-prefix-or-suffix-to-array-elements/embed/#?secret=LJ9MHLddIu#?secret=9VRrozAWkl" data-secret="9VRrozAWkl" width="500" height="282" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>'
_oembed_time_4c092f67c6f1acb9f45e1620e2f36386: "1609253562"
_oembed_time_46f776e5c94f52b8fbde5ced38b929bb: "1712356541"
_oembed_time_2108247391591c79b51447feaa0a6701: "1708540114"
author: "Steven Wade"
categories:
  - Engineering
date: "2020-12-29T14:51:32+00:00"
guid: https://stevenwadejr.com/?p=56
parent_post_id: null
post_id: "56"
tags:
  - php
title: Using PHP's preg_filter to Easily Subdivide Arrays
url: /2020/12/29/using-phps-preg_filter-to-easily-subdivide-arrays/
keywords: ['preg_filter', 'php']

---
PHP's [`preg_filter`](https://www.php.net/manual/en/function.preg-filter.php) function is great for filtering array elements that match a given expression. The official documentation states that the function "only returns the (possibly transformed) subjects where there was a match" meaning if no elements match the given pattern, an empty array is returned (when dealing with an array of course).

For example, take an array of multiple type of entity types and their IDs:

```
$ids = [123, 'team:4', 52, 'pond:16', 'team:12', 6];
```

In the array above, we have users (integer IDs), teams (prefixed by "team:") and a "pond" entity prefixed by "pond:". If we want to process teams and ponds we could use a traditional loop checking for prefixes and parsing out the integer IDs like so:

```
$teamIds = [];
$pondIds = [];
foreach ($ids as $id) {
    if (strpos($id, 'team') !== false) {
        $teamIds[] = (int) preg_replace('/\D/', '', $id);
    } elseif (strpos($id, 'pond') !== false) {
        $pondIds[] = (int) preg_replace('/\D/', '', $id);
    }
}
```

Or, we could clean that up with a single line per entity type with `preg_filter`:

```
$teamIds = array_values(array_map('intval', preg_filter('/team\:/', '', $ids)));
$pondIds = array_values(array_map('intval', preg_filter('/pond\:/', '', $ids)));
```

In the code above, `preg_filter` will look for array items matching the given prefix pattern and return the values after that pattern (ie: the IDs). This not only searches our array, but it also strips the entity prefix for us in a single function call. Combined with the `intval` map loop, the code cleans up nicely.

The new arrays are reset with the `array_values` function because `preg_filter` will return an array of matches and preserve the original indexes. This can cause an issue sometimes, especially when combined with PDO parameters so it's best to just reset it and forget it.