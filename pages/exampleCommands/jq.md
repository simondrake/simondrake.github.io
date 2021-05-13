---
layout: page
permalink: /example-commands/jq/
---

<h1>jq</h1>
<h3><small class="text-muted"><a href="https://stedolan.github.io/jq/">jq</a> is a lightweight and flexible command-line JSON processor.</small></h3>

#### Filter on Object key/value
Select the array in `.items`, loop through and select the items which have a `.name` equal to `"thisname"`, then return the `.name` and `.status`

<pre class="command-line"><code class="language-bash" data-prismjs-copy="Copy">
cat file.json | jq '.items | .[] | select(.name == "thisname") | {name: .name, status: .status}'
</code></pre>

#### Filter on Object key/value (contains)
Loop through an array and select the items which have a `.name` which contains `"someText"`, then return the `.name` and `.status`

<pre class="command-line"><code class="language-bash">
cat file.json | jq '.[] | select( .name | contains("someText") ) | {name: .name, status: .status}'
</code></pre>

#### Filter on Object key existing
Select the array in `.items`, loop through and select the items which have the specified key, then return the `.name`

<pre class="command-line"><code class="language-bash">
cat file.json | jq -r '.items | .[] | select(.a.path.to.an.array[]?.key != null) | .name'
</code></pre>

#### Filter with multiple conditions
Use `or` in a `select` statement, to filter on multiple conditions

<pre class="command-line"><code class="language-bash">
cat file.json | jq -r '.items | .[] | select((.status == "Running") or (.status == "Scheduled") or .status == "ConfirmationNeeded") | .name'
</code></pre>

#### Filter on Object key/value (not contains)
Loop through an array and select the items which don't have a `.name` which contains `"some-text"`, then return the `.name`.

<pre class="command-line"><code class="language-bash">
cat file.json | jq -r '.items | .[] | select(.name | contains("some-text") | not) | .name'
</code></pre>

#### Get length of an array
<pre class="command-line"><code class="language-bash">
cat file.json | jq -r '.items | length'
</code></pre>


#### Group by and count
<pre class="command-line" data-filter-output="(out)"><code class="language-bash">
echo '{"items": [ {"Priority": "High"},{"Priority": "High"},{"Priority": "High"},{"Priority": "High"},{"Priority": "Low"},{"Priority": "Low"},{"Priority": "Low"} ]}' | jq -r '.items | group_by(.Priority)[] | {Priority: .[0].Priority, Count: length}'
(out){
(out)  "Priority": "High",
(out)  "Count": 4
(out)}
(out){
(out)  "Priority": "Low",
(out)  "Count": 3
(out)}
</code></pre>


#### Select, Group by and count
<pre class="command-line" data-filter-output="(out)"><code class="language-bash">
echo '{"items": [ {"Priority": "High"},{"Priority": "High"},{"Priority": "High"},{"Priority": "High"},{"Priority": "Medium"},{"Priority": "Medium"},{"Priority": "Low"},{"Priority": "Low"},{"Priority": "Low"} ]}' | jq -r '.items | .[] | select((.Priority == "High") or (.Priority == "Medium"))' | jq -s 'group_by(.Priority)[] | {Priority: .[0].Priority, Count: length}'
(out){
(out)  "Priority": "High",
(out)  "Count": 4
(out)}
(out){
(out)  "Priority": "Medium",
(out)  "Count": 2
(out)}
</code></pre>


#### Select fields starting with
Select all fields that start with `prefixA`, from within the `anotherField` object

<pre class="command-line" data-filter-output="(out)"><code class="language-bash">
echo '{"aField": {}, "anotherField": { "prefixAOne": "foo", "prefixATwo": "foo", "prefixAThree": "foo", "prefixBOne": "bar", "prefixBTwo": "bar" }, "lastField": true}' | jq -r '.anotherField | to_entries[] | select(.key|startswith("prefixA"))'
(out){
(out)  "key": "prefixAOne",
(out)  "value": "foo"
(out)}
(out){
(out)  "key": "prefixATwo",
(out)  "value": "foo"
(out)}
(out){
(out)  "key": "prefixAThree",
(out)  "value": "foo"
(out)}
</code></pre>


#### Add a field to an object
Assign `bar` to argument `foo` and then add the `"foo": "bar"` key/value to the object

<pre class="command-line" data-filter-output="(out)"><code class="language-bash">
echo '{"hello": "world"}' | jq --arg foo bar '. + {foo: $foo}'
(out){
(out)  "hello": "world",
(out)  "foo": "bar"
(out)}
</code></pre>

Preserve nested objects with `*`
<pre class="command-line" data-filter-output="(out)"><code class="language-bash">
echo '{"hello": {"value": "world"}}' | jq '. * {"hello": {foo: ("bar")}}'
(out){
(out)  "hello": {
(out)    "value": "world",
(out)    "foo": "bar"
(out)  }
(out)}
</code></pre>

Using `+` will not preserve nested objects
<pre class="command-line" data-filter-output="(out)"><code class="language-bash">
echo '{"hello": {"value": "world"}}' | jq '. + {"hello": {foo: ("bar")}}'
(out){
(out)  "hello": {
(out)    "foo": "bar"
(out)  }
(out)}
</code></pre>


#### Concat strings

<pre class="command-line" data-filter-output="(out)"><code class="language-bash">
echo '{"hello": {"value": "world"}}' | jq --arg foo bar '. * {"hello": {foo: ("not" + $foo)}}'
(out){
(out)  "hello": {
(out)    "value": "world",
(out)    "foo": "notbar"
(out)  }
(out)}
</code></pre>
