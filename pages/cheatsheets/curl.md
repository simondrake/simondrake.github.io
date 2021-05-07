---
layout: page
permalink: /cheatsheets/curl/
---
<h1>Curl </h1>
<h3><small class="text-muted">curl is used in command lines or scripts to transfer data</small></h3>

## Options

|Option|Description|Example
|:--|:--|:--|
|`-X`                |Request Type                                                         |`-X POST`|
|`-H`                |Header                                                               |`-H "Content-Type: application/json"`|
|`-d` `--data`       |data                                                                 |`-H "Content-Type: application/json" -d '{"message": "a request body example"}'`|
|`-d` `--data`       |data                                                                 |`-H "Content-Type: x-www-form-encoded" -d 'name=bob'`|
|`-i`                |Include response headers/status code in output                       |`-i`|
|`-s`                |Silent or quiet mode                                                 |`-s`|
|`-m` `--max-time`   |Maximum time in seconds that you allow the whole operation to take   |`-m 60`|

## Examples

### POST Request

<pre class="command-line">
    <code class="language-bash">
Authorization: Bearer $TOKEN" -H 'content-Type: application/json' -X POST 'http://localhost:3002/v1/question/' -d '{"question": "what is 1 + 1"}'
    </code>
</pre>

