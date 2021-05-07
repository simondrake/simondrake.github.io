---
layout: page
permalink: /cheatsheets/go/
---
<h1>Go</h1>
<h3><small class="text-muted">Go is an open source programming language that makes it easy to build simple, reliable,
        and efficient software</small></h3>

|Command|Description|
|:--|:--|
|`go test ./... -run TestToRun`|Run a single test|
|`go test ./... -run TestToRun -testify.m SuiteTestToRun`|Run a single suite test|
|`go test -tags=integration ./...`|Run tests with a custom build tag|

<br><br>

<h5>Reuse a variable in <code>fmt</code> call</h5>

<pre><code class="line-numbers language-go">
func main() {
	s := "World"

	fmt.Printf(`Hello %[1]s Goodbye %[1]s`, s)
}
</code></pre>
