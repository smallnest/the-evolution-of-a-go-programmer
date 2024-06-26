# The Evolution of a Go Programmer

## Junior Go programmer

```go
package fac

func Factorial(n int) int {
	res := 1

	for i := 1; i <= n; i++ {
		res *= i
	}

	return res
}
```

## Functional Go programmer

```go
package fac

func Factorial(n int) int {
	if n == 0 {
		return 1
	} else {
		return Factorial(n - 1) * n
	}
}
```

## Generic Go programmer

```go
package fac

func Factorial(n interface{}) interface{} {
	v, valid := n.(int)
	if !valid {
		return 0
	}

	res := 1

	for i := 1; i <= v; i++ {
		res *= i
	}

	return res
}
```

## Multithread optimized Go programmer

```go
package fac

import "sync"

func Factorial(n int) int {
	var (
		left, right = 1, 1
		wg sync.WaitGroup
	)

	wg.Add(2)

	pivot := n / 2

	go func() {
		for i := 1; i < pivot; i++ {
			left *= i
		}

		wg.Done()
	}()

	go func() {
		for i := pivot; i <= n; i++ {
			right *= i
		}

		wg.Done()
	}()

	wg.Wait()

	return left * right
}
```

## Discovered Go patterns

```go
package fac

func Factorial(n int) <-chan int {
	ch := make(chan int)

	go func() {
		prev := 1

		for i := 1; i <= n; i++ {
			v := prev * i

			ch <- v

			prev = v
		}

		close(ch)
	}()

	return ch
}
```

## Fix Go weaknesses with mature solutions

```go
package fac

/**
 * @see https://en.wikipedia.org/wiki/Factorial
 */
type IFactorial interface {
	CalculateFactorial() int
}

// FactorialImpl implements IFactorial.
var _ IFactorial = (*FactorialImpl)(nil)

/**
 * Used to find factorial of the n.
 */
type FactorialImpl struct {
	/**
	 * The n.
	 */
	n int
}

/**
 * Constructor of the FactorialImpl.
 *
 * @param n the n.
 */
func NewFactorial(n int) *FactorialImpl {
	return &FactorialImpl{
		n: n,
	}
}

/**
 * Gets the n to use in factorial function.
 *
 * @return int.
 */
func (this *FactorialImpl) GetN() int {
	return this.n
}

/**
 * Sets the n to use in factorial function.
 *
 * @param n the n.
 * @return void.
 */
func (this *FactorialImpl) SetN(n int) {
	this.n = n
}

/**
 * Returns factorial of the n.
 *
 * @todo remove "if" statement. Maybe we should use a factory or somthing?
 *
 * @return int.
 */
func (this *FactorialImpl) CalculateFactorial() int {
	if this.n == 0 {
		return 1
	}

	n := this.n
	this.n = this.n - 1

	return this.CalculateFactorial() * n
}
```

## Senior Go programmer

```go
package fac

// Factorial returns n!.
func Factorial(n int) int {
	res := 1

	for i := 1; i <= n; i++ {
		res *= i
	}

	return res
}
```

## Guru Go Programmer

```go
package fac

import "iter"

// Factorial returns n!.
func Factorial(n int) int {
	res := 1

	for i := range factorial(n) {
		res *= i
	}

	return res
}

func factorial(n int) iter.Seq[int] {
	return func(yield func(int) bool) {
		for i := range n {
			if !yield(i + 1) {
				break
			}
		}
	}
}
```

## Rob Pike

```text
package fac

// Factorial returns n!.
func Factorial(n int) int {
	res := 1

	for i := 1; i <= n; i++ {
		res *= i
	}

	return res
}
```

> Tribute to the Iavor Diatchki's original page "[The Evolution of a Programmer](https://www.ariel.com.au/jokes/The_Evolution_of_a_Programmer.html)".
