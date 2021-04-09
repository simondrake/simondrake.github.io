---
layout: post
title: Debugging with delve
subtitle: Start using the delve debugger
tags: [go]
---

Learn how to use Delve to debug your Go applications and tests.

## What is [delve](https://github.com/go-delve/delve)?

Delve is an open source debugger for the [Go](golang.org) programming language.

## When should I use delve to debug my Go programmes?

If you use one of the popular Code Editors (e.g. VSCode, Golang) they will handle this for you. If you use VIM, and Print statements aren't cutting it, then this tutorial is for you.

## Pre-requisites

* Install [Go](golang.org)
* Have at least a basic working knowledge of the language
* Install [delve](https://github.com/go-delve/delve)


### Setting up

* Create a new directory in a place of your choice
* Run the `go mod init` command (e.g `go mod init github.com/simondrake/biggest-num`)
* Create our `pkg` directory - `mkdir -p pkg/biggestNum`
* Add a `biggestNum.go` file in the `biggestNum` directory with the following contents:

{:.line-numbers}
```go
package biggestNum

func BiggestEvenNum(nums []int) int {
	var biggest int

	for _, n := range nums {
		if n > biggest && isEven(n) {
			biggest = n
		}
	}

	return biggest
}

func isEven(num int) bool {
	isEven := num%2 == 0

	return isEven
}
```

* Create another directory, separate from the one you created at the beginning of this section
* Run `go mod init example.com`
* Change your `go.mod` file to the following, which basically imports our `biggestNum` package without having to push it up to GitHub (change directory names as required):


{:.line-numbers}
```go
module example.com

go 1.16

require (
	github.com/simondrake/biggestNum v1.0.0
)

replace github.com/simondrake/biggestNum => ../biggest-num
```

* Create a `main.go` file with the following contents (change import for `biggestNum` as required):

{:.line-numbers}
```go
package main

import (
	"fmt"

	"github.com/simondrake/biggestNum/pkg/biggestNum"
)

func main() {
	b := biggestNum.BiggestEvenNum([]int{1, 4, 23, 12, 41, 51})

	fmt.Println(b)
}
```

* Run `go run main.go` and make sure you don't get any compilation errors.

## Debugging your Application

We now have a Go application we can debug, so let's start the debugger with `dlv debug` and you should see the following:

```
$ dlv debug
Type 'help' for list of commands.
(dlv)
```

Add a breakpoint at line 10 of our `main.go` file by running `breakpoint main.go:10` (or `b main.go:10` for short).

```
(dlv) b main.go:10
Breakpoint 1 set at 0x10cb9cf for main.main() ./main.go:10
```

Run `continue` (or `c`) to run-to-breakpoint.

```
(dlv) c
> main.main() ./main.go:10 (hits goroutine(1):1 total:1)
     5:
     6:         "github.com/simondrake/biggestNum/pkg/biggestNum"
     7: )
     8:
     9: func main() {
=>  10:         b := biggestNum.BiggestEvenNum([]int{1, 4, 23, 12, 41, 51})
    11:
    12:         fmt.Println(b)
    13: }
```

From here we could continue (`c`), which would run the programme to completion because there are no more breakpoints, but instead let's step-into the `BiggestEvenNum` method by running `step` (or `s`).

```
> github.com/simondrake/biggestNum/pkg/biggestNum.BiggestEvenNum() /path/to/biggest-num/pkg/biggestNum/biggestNum.go:3
     1: package biggestNum
     2:
=>   3: func BiggestEvenNum(nums []int) int {
     4:         var biggest int
     5:
     6:         for _, n := range nums {
     7:                 if n > biggest && isEven(n) {
     8:                         biggest = n
```

Note how the first line gives us a handy reference to where we are (`/path/to/biggest-num/pkg/biggestNum/biggestNum.go:3`).

Now let's add another breakpoint in our `biggestNum` package, on line 7:

```
(dlv) b /path/to/biggest-num/pkg/biggestNum/biggestNum.go:7
Breakpoint 2 set at 0x10cb8ea for github.com/simondrake/biggestNum/pkg/biggestNum.BiggestEvenNum() /path/to/biggest-num/pkg/biggestNum/biggestNum.go:7
```

Run `continue` to get to our next breakpoint.

```
(dlv) c
> github.com/simondrake/biggestNum/pkg/biggestNum.BiggestEvenNum() /path/to/biggest-num/pkg/biggestNum/biggestNum.go:7 (hits goroutine(1):1 total:1)
     2:
     3: func BiggestEvenNum(nums []int) int {
     4:         var biggest int
     5:
     6:         for _, n := range nums {
=>   7:                 if n > biggest && isEven(n) {
     8:                         biggest = n
     9:                 }
    10:         }
    11:
    12:         return biggest
```

You can evaluate expressions by running `print` (or `p`)

```
(dlv) p n
1
```

You can evaluate a function call by running `call`

```
(dlv) call isEven(n)
> github.com/simondrake/biggestNum/pkg/biggestNum.BiggestEvenNum() /path/to/biggest-num/pkg/biggestNum/biggestNum.go:7 (hits goroutine(1):2 total:2)
Values returned:
        ~r1: false
```

You can `continue` to run the programme to the next iteration of the loop, or run `next` (or `n`) to go to the next line, but we're going to skip that by removing this breakpoint. First, get the name of the breakpoint with `breakpoints` (or `bp`) and then delete it

```
(dlv) clear 2
Breakpoint 2 cleared at 0x10cb8ea for github.com/simondrake/biggestNum/pkg/biggestNum.BiggestEvenNum() /path/to/biggest-num/pkg/biggestNum/biggestNum.go:7
```

Then we'll `stepout` (or `so`) of this package

```
(dlv) so
> main.main() ./main.go:10
Values returned:
        ~r1: 12

     5:
     6:         "github.com/simondrake/biggestNum/pkg/biggestNum"
     7: )
     8:
     9: func main() {
=>  10:         b := biggestNum.BiggestEvenNum([]int{1, 4, 23, 12, 41, 51})
    11:
    12:         fmt.Println(b)
    13: }
```

From here you can `restart` if you want to run the same debugger again, or `continue` until completion of the application.

## Debugging tests

So far, we've seen how to debug a Go application. But what if we wanted to debug our `biggestNum` standalone? If you try running `dlv debug` you'll get an error.

```
$ dlv debug pkg/biggestNum/biggestNum.go
could not launch process: not an executable file
```

But, delve does have a `test` subcommand we can use to run our tests in debug mode. So, first, let's add the following test under `biggestNum_test.go`:

{:.line-numbers}
```go
package biggestNum

import (
	"testing"
)

func TestBiggestEvenNum(t *testing.T) {
	expected := 290

	out := BiggestEvenNum([]int{23, 51, 4, 32, 45, 290})

	if out != expected {
		t.Fatalf("'%d' did not match expected '%d'", out, expected)
	}
}
```

To run the tests in debug mode, use `dlv test`

```
$ dlv test pkg/biggestNum/*.go -test.v
Type 'help' for list of commands.
```

Then set a breakpoint as we did in the previous step

```
(dlv) b biggestNum.go:6
Breakpoint 1 set at 0x11607d3 for command-line-arguments.BiggestEvenNum() ./pkg/biggestNum/biggestNum.go:6``
```

Run `continue` as we did before

```
(dlv) continue
> command-line-arguments.BiggestEvenNum() ./pkg/biggestNum/biggestNum.go:6 (hits goroutine(4):1 total:1)
     1: package biggestNum
     2:
     3: func BiggestEvenNum(nums []int) int {
     4:         var biggest int
     5:
=>   6:         for _, n := range nums {
     7:                 if n > biggest && isEven(n) {
     8:                         biggest = n
     9:                 }
    10:         }
    11:
```

And then debug your package, as we did our application, in the previous section.

## Closing thoughts

You've learnt the basics of debugging with delve, but you should refer to the `help` command output to learn about what else delve can do.
