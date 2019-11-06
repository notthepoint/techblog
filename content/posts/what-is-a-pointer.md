---
title: "What is a pointer?"
date: 2019-11-05T17:12:23+01:00
draft: false
---

## Computer memory

Before understanding pointers, it is best to start with a brief explanation of memory. Memory in a computer can be thought of as a row of cells, each with their own address. Each cell stores a value and if you have the cell's address you can retrieve the value from it or set a new value in the cell.

<div style="text-align: center;">
	<img style="margin: 0 auto; display: block;" height="120px" src="what-is-a-pointer/gcmemory-cells.png">
	<small>A row of memory cells with the memory addresses 11, 12, and 13</small>
</div>

## Pointers

A pointer is a value, just like an int or a string. And a pointer value has a pointer type, just like an int value has an int type. The value of a pointer is the memory address of another value.

<div style="text-align: center;">
	<img style="margin: 0 auto; display: block;" height="160px" src="what-is-a-pointer/gcpointer-to-cell.png">
	<small>The pointer 'my_pointer' has the value 13</small>
</div>

Pointer types are parameterized by a secondary type, such an int type. So, for example, you have an int pointer type or a string pointer type.

Below is an example of creating a string pointer in Go. Go uses the `*` to indicate a pointer.

```go
var my_pointer *string

```

In Go, using the `&` operator in front of a variable will return the memory address. You can use this to create a pointer from an existing variable in Go. 

```go
foo := 3  // foo is of type int
fmt.Printf("foo is a variable of type: %T\n", foo)
fmt.Printf("the value of foo is: %v\n", foo)

bar := &foo // bar is a pointer to an int
fmt.Printf("bar is a variable of type: %T\n", bar)
fmt.Printf("the value of bar is: %v\n", bar)
```
Running the above gives you the output

```
foo is a variable of type: int
the value of foo is: 3
bar is a variable of type: *int
the value of bar is: 0x414020
```

You can run the code yourself here 👉 [https://play.golang.org/p/KA7LYRaaqfd](https://play.golang.org/p/KA7LYRaaqfd).

<!--As `bar` is 'pointing' to `foo`, if `foo` were to change then the value you get from dereferencing `bar` 
-->

## Dereferencing a pointer
    
To access the value that a pointer 'points' to you need to dereference it. Dereferencing a pointer allows you to read, or write, the value in the location it was pointing to.

Below is an example of dereferencing a pointer in Go. Go uses the `*` operator to dereference.

```go
foo := 3
bar := &foo
fmt.Printf("dereferencing bar gives the type: %T\n", *bar)
fmt.Printf("dereferencing bar gives the value: %v\n", *bar)
```

Running the above gives you the output

```
dereferencing bar gives the type: int
dereferencing bar gives the value: 3
```

You can run the code yourself here 👉 [https://play.golang.org/p/epFy9VWMD9W](https://play.golang.org/p/epFy9VWMD9W).

### Changing values

In the above example as `bar` is 'pointing' to `foo`, if `foo` were to change then the value you get from dereferencing `bar` would be the new value.

We can use our representation of memory as a row of cells to understand why this is the case.

<div style="text-align: center; margin: 20px 0;">
	<img style="margin: 0 auto; display: block;" height="220px" src="what-is-a-pointer/gcpointer-to-val.png">
	<small>
		The variable 'my_var' has the int value 3<br/>
		The variable 'my_pointer' has a pointer value which is the memory address of 'my_var'
	</small>
</div>

<div style="text-align: center; margin: 20px 0;">
	<img style="margin: 0 auto; display: block;" height="190px" src="what-is-a-pointer/gcpointer-to-int.png">
	<small>
		The pointer 'my_pointer' points to the memory address of 'my_var'
	</small>
</div>

<div style="text-align: center; margin: 20px 0;">
	<img style="margin: 0 auto; display: block;" height="225px" src="what-is-a-pointer/gcupdate-val.png">
	<small>
		The pointer 'my_pointer' points to the same memory location, which now stores the new value of 'my_var'<br/>
		Deferencing 'my_pointer' would now give the value 9
	</small>
</div>

We can see this in action in go.

```go
foo := 3
bar := &foo
fmt.Printf("the value of foo is: %v\n", foo)
fmt.Printf("dereferencing bar gives the type: %T\n", *bar)
fmt.Printf("dereferencing bar gives the value: %v\n", *bar)

foo = 9
fmt.Printf("the value of foo is now: %v\n", foo)
fmt.Printf("deferencing bar gives the value: %v\n", *bar)
```

Running the above gives you the output

```
the value of foo is: 3
dereferencing bar gives the type: int
dereferencing bar gives the value: 3
the value of foo is now: 9
deferencing bar gives the value: 9
```

You can run the code yourself here 👉 [https://play.golang.org/p/_HrxOEStvEX](https://play.golang.org/p/_HrxOEStvEX)

## A note on pass by value and pass by reference

Pass by reference is when a reference to a value is passed to a function and pass by value is when a copy of the value is passed.

Pointers are sometimes confused with the term 'pass by reference' because pointers can be thought of as references. They reference another value. I specifically only used the term 'pointer' throughout this post so as to not confuse references and pointers.

References are different to pointers and can be thought of like aliases. They are another name for the same variable.

In the c++ code below we are assigning the value 3 to the variable `foo`. Then we are creating a reference to `foo` called `bar`. In other words, `foo` and `bar` are both names for the same variable that has the value 3.


```c++
int foo = 3;
int& bar = foo;

std::cout << "the value of foo is " << foo << std::endl;
std::cout << "the value of bar is " << bar << std::endl;
bar = 4;
std::cout << "the value of foo is " << foo << std::endl;
std::cout << "the value of bar is " << bar << std::endl;
```
Running the above gives you the output

```
the value of foo is 3
the value of bar is 3
the value of foo is 4
the value of bar is 4
```
You can run the code yourself here 👉 [https://repl.it/@notthepoint/ReferencesExample1](https://repl.it/@notthepoint/ReferencesExample1)

Because they are two names for the same variable, changing the value of `bar` meant we were also changing the value of `foo`.

Pass by reference is when you pass a reference to a function. Below is an example of pass by reference to a function, written in c++.

```c++
void swap(int& a, int& b) {
  int tmp = a;
  a = b;
  b = tmp;
}

int main() {
  int foo = 3;
  int bar = 9;
  std::cout << "the value of foo is " << foo << std::endl;
  std::cout << "the value of bar is " << bar << std::endl;
  swap(foo, bar);
  std::cout << "the value of foo is " << foo << std::endl;
  std::cout << "the value of bar is " << bar << std::endl;
}
```

Running the above gives you the output

```
the value of foo is 3
the value of bar is 9
the value of foo is 9
the value of bar is 3
```

You can run the code yourself here 👉 [https://repl.it/@notthepoint/ReferencesExample2](https://repl.it/@notthepoint/ReferencesExample2)

Within the `swap` function we have references to the variables `foo` and `bar`. So any updates to `a` and `b` within this function will affect `foo` and `bar` as they are different names for the same variable. It is like having a new name for the same variable within the function.

Not many mainstream languages support pass by reference. For example, it is not possible to write the above swap function in Java, Ruby, Python, C or Go. They only support pass by value.

Below is an example of the same swap function but without references, written in Go.

```go
func swap(a int, b int) {
 	tmp := a
	a = b
	b = tmp
}

func main() {
	foo := 3
	bar := 9
	fmt.Printf("the value of foo is: %v\n", foo)
	fmt.Printf("the value of bar is: %v\n", bar)
	swap(foo, bar)
	fmt.Printf("the value of foo is: %v\n", foo)
	fmt.Printf("the value of bar is: %v\n", bar)
}

```

Running the above gives you the output

```
the value of foo is: 3
the value of bar is: 9
the value of foo is: 3
the value of bar is: 9
```

You can run the code yourself here 👉 [https://play.golang.org/p/vmGMVXmTJYX](https://play.golang.org/p/vmGMVXmTJYX)

The variables are passed by value, so outside of the function the values aren't swapped.

## It isn't all about Go

The examples I have used have mostly focused on Go. This is because Go is one of the languages that surfaces pointers to the user.

Other languages use pointers, the principle behind them remains the same, and sometimes pointers are implicit in languages so you may not notice they are being used.

In languages that make pointers explicit, like C and Go, you can write a swap function using pointers. You couldn't do this in a language with implicit pointers.

Below is my final example, swapping variables using pointers in Go. Enjoy! 😋

```go
func swap(a *int, b *int) {
    tmp := *a
    *a = *b
    *b = tmp
}

func main() {
	foo := 3
	bar := 9
	fmt.Printf("the value of foo is: %v\n", foo)
	fmt.Printf("the value of bar is: %v\n", bar)
	swap(&foo, &bar)
	fmt.Printf("the value of foo is: %v\n", foo)
	fmt.Printf("the value of bar is: %v\n", bar)
}
```

Running the above gives you the output

```
the value of foo is: 3
the value of bar is: 9
the value of foo is: 9
the value of bar is: 3
```
You can run the code yourself here 👉 [https://play.golang.org/p/PSlh2xmeqSA](https://play.golang.org/p/PSlh2xmeqSA)

Note: The line `*a = *b` dereferences `b` to get the value that `b` was 'pointing' to and then dereferences `a` to write that value into the address that `a` points to.
