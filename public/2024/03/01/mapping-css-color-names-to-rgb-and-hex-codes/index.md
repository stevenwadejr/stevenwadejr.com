# Mapping CSS Color Names to RGB and Hex Codes

I recently found myself needing to enforce color values in a hex code format after discovering a mix of hex codes and CSS color names in my data. The easiest way to do this was to build a static map. My application is built with PHP so I needed to get the full list of CSS color names as listed on the [W3's website](https://www.w3.org/TR/css-color-4/#named-colors) into an associative PHP array.

This was my journey.

Start with the W3's list of colors and using Chrome's inspector, copy the HTML table.

{{< figure src="/wp-content/uploads/sites/2/2024/03/GIF-Recording-2024-03-01-at-2.00.59-PM.gif" alt="Animated GIF showing an HTML table being copied with Chrome's inspector tools." caption="Animated GIF showing an HTML table being copied with Chrome's inspector tools." >}}

Then, using an [online HTML table-to-CSV converter](https://www.convertcsv.com/html-table-to-csv.htm), paste the table's HTML code and download a formatted CSV. You could use a [table-to-JSON converter](https://www.convertjson.com/html-table-to-json.htm), but CSVs are easier to manipulate and customize via something like Excel or Numbers. For me, I wanted to delete the first two columns as they are for HTML display purposes and hold no real data or value for my needs.

[colors.csv](/wp-content/uploads/sites/2/2024/03/colors.csv)

Once the CSV was tidied up, it was on to a [CSV-to-JSON converter](https://csvjson.com/csv2json). Here, I chose the "hash" instead of the "array" option as I wanted a keyed/hashed list which would make it easier to target specific colors.

[colors.json](/wp-content/uploads/sites/2/2024/03/colors.json)

Last but not least, it was on to another free online too - a [JSON-to-PHP array converter](https://jsontophp.com/). True, PHP can easily parse a JSON file into an associative array, but if it's configuration and something likely not to change ever, it's better to just use a native array and put it in your source code.

[colors.php](/wp-content/uploads/sites/2/2024/03/colors.php_.zip)

Now I can do this:

```php
$hex = $colors['hex']['aquamarine'];
// or
$rgb = $colors['rgb']['aquamarine'];
```

That's it. It was a short walk with multiple jumps. Were there better ways to get here? Possibly, but what matters is that I got there and I'm sharing the files here in case anyone (ie: future me) finds themselves needing to translate CSS color names into hex codes or an RGB value.

Cheers!


---

> Author: <no value>  
> URL: http://localhost:1313/2024/03/01/mapping-css-color-names-to-rgb-and-hex-codes/  

