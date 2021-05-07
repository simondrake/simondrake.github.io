---
layout: page
permalink: /cheatsheets/regex/
---
<h1>Regex</h1>
<h3><small class="text-muted">A sequence of characters that specifies a search pattern</small></h3>

|Character|Description|
|:--|:--|
|`\d`|Shorthand for `[0-9]`|
|`\w`|Word character (`[A-Za-z0-9_]`)|
|`\s`|Whitespace character (`[ \t\r\n\f]`)|
|`^`|Start of line|
|`$`|End of line|
|`?=`|Positive Lookahead|

<br><br>

### Two conditions

`foo` must appear anywhere and `baz` must appear anywhere, not neccessarily in that order.

<pre class="command-line"><code class="language-bash">
(?=.*foo)(?=.*baz)
</code></pre>

<br><br>
