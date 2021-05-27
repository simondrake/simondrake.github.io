---
layout: post
title: Value Receivers VS Pointer Receivers in Go
subtitle: And can I mix and match them?
tags: [go]
---

## What is a Method?

Before we start talking about Value Receivers and Pointer Receivers, let's make sure we have a good grasp of what a Method is.

In [Go](https://golang.org/]), a Method is is a function that is defined on a type using a receiver argument.

The following example is from [Tour of Go](https://tour.golang.org/methods/1):

```go
type Vertex struct {
	X, Y float64
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

## What is a Value Receiver?

When using a Value Receiver, a copy is taken of the type and passed to the Method; similarly to what happens when passing-by-value to a normal function. When the method executes, the object copy sits in a different location in memory; thus any changes made to the object don't effect the original.

Take the following example; even though we change the `Breed` and `Weight`, the original object remains unchanged.

```go
type Dog struct {
	Breed string
	Weight int16
}

func main() {
	dog := Dog{Breed: "Dalmation", Weight: 40}

	fmt.Printf("dog before ValReceiver: %+v\n", dog) // {Breed:Dalmation Weight:40}
	dog.ValReceiver()
	fmt.Printf("dog after ValReceiver: %+v\n", dog) // {Breed:Dalmation Weight:40}
}

func (d Dog) ValReceiver() {
	d.Breed = "Terrier"
	d.Weight = 10
}

```

There are some exceptions to this rule, for example when using slices; which contain a pointer to the underlying array and can lead to the original object being amended. The following is an example of this scenario:

```go
type Dog struct {
	Breed string
	Weight int16
	Friends []string
}

func main() {
	dog := Dog{Breed: "Dalmation", Weight: 40, Friends: []string{"Fred", "Amy"}}

	fmt.Printf("dog before ValReceiver: %+v\n", dog) // {Breed:Dalmation Weight:40 Friends:[Fred Amy]}
	dog.ValReceiver()
	fmt.Printf("dog after ValReceiver: %+v\n", dog) // {Breed:Dalmation Weight:40 Friends:[Chris Amy]}
}

func (d Dog) ValReceiver() {
	d.Breed = "Terrier"
	d.Weight = 10
	d.Friends[0] = "Chris"
}
```

## What is a Pointer Receiver?

When using a Value Receiver, the memory address is passed to the Method; similarly to what happens when passing-by-reference to a normal function. When the method executes, it has a reference to the original object; thus any changes made to the object do affect the original.

Take the following example; When the method changes the `Breed` and `Weight`, the original object is also changed.

```go
type Dog struct {
	Breed string
	Weight int16
}

func main() {
	dog := Dog{Breed: "Dalmation", Weight: 40}

	fmt.Printf("dog before PtrReceiver %+v\n", dog) // {Breed:Dalmation Weight:40}
	dog.PtrReceiver()
	fmt.Printf("dog after PtrReceiver: %+v\n", dog) // {Breed:Terrier Weight:10}
}

func (d *Dog) PtrReceiver() {
	d.Breed = "Terrier"
	d.Weight = 10
}
```

## Can I mix-and-match Value and Pointer Receivers?

It is generally recommended you don't mix-and-match. If a type uses both Value and Pointer receiver, it will be hard for your consumers to know which method uses which type without having to read the code. However, yes you can mix-and-match.

When doing so, remember the following rules ([ref](https://stackoverflow.com/questions/33587227/method-sets-pointer-vs-value-receiver)):

* If you have a `*T` you can call methods that have a receiver type of `*T` as well as methods that have a receiver type of `T`.
* If you have a `T` and it is addressable you can call methods that have a receiver type of `*T` as well as methods that have a receiver type of `T`.
* If you have a `T` and it isn't addressable, you can only call methods that have a receiver type of `T`, not `*T`.
* If you have an interface `I`, and some or all of the methods in `I`'s method set are provided by methods with a receiver of `*T` (with the remainder being provided by methods with a receiver of `T`), then `*T` satisfies the interface `I`, but `T` doesn't. That is because `*T`'s method set includes `T`'s, but not the other way around (see the first bullet).

In short, you can mix and match methods with Value Receivers and Methods with Pointer Receivers, and use them with variables containing values and pointers, without worrying about which is which. Both will work, and the syntax is the same. However, if Methods with Pointer Receivers are needed to satisfy an Interface, then only a Pointer will be assignable to the Interface — a Value won't be valid.
