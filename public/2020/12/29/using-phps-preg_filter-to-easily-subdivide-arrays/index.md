# Using PHP's Preg_filter to Easily Subdivide Arrays

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

---

> Author: <no value>  
> URL: http://localhost:1313/2020/12/29/using-phps-preg_filter-to-easily-subdivide-arrays/  

