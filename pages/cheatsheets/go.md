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

## Reuse a variable in `fmt` call

<pre><code class="line-numbers language-go">
func main() {
	s := "World"

	fmt.Printf(`Hello %[1]s Goodbye %[1]s`, s)
}
</code></pre>

## Run a single suite test

As described in the the above table, single suite test can be run with the `go test ./... -run TestToRun -testify.m SuiteTestToRun` command.

However, you should set the test path correctly (i.e. not `./...`) because if you run tests for a package that doesn't use `testify` you'll get a `flag provided but not defined` error.
