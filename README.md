# assert [![Godoc Reference](https://pkg.go.dev/badge/github.com/pchchv/go-assert)](https://pkg.go.dev/github.com/pchchv/go-assert)

Package `go-assert` is a Basic Assertion library used along side native go testing

# Usage

```go
package maint

import (
	"errors"
	"testing"
	. "github.com/pchchv/assert"
)

func AssertCustomErrorHandler(t testing.TB, errs map[string]string, key, expected string) {
	val, ok := errs[key]

	// using EqualSkip and NotEqualSkip as building blocks for my custom Assert function
	EqualSkip(t, 2, ok, true)
	NotEqualSkip(t, 2, val, nil)
	EqualSkip(t, 2, val, expected)
}

func TestEqual(t *testing.T) {
	// error comes from your package/library
	err := errors.New("my error")
	NotEqual(t, err, nil)
	Equal(t, err.Error(), "my error")

	err = nil
	Equal(t, err, nil)

	fn := func() {
		panic("omg omg omg!")
	}

	PanicMatches(t, func() { fn() }, "omg omg omg!")
	PanicMatches(t, func() { panic("omg omg omg!") }, "omg omg omg!")

	// errs would have come from your package/library
	errs := map[string]string{}
	errs["Name"] = "User Name Invalid"
	errs["Email"] = "User Email Invalid"

	AssertCustomErrorHandler(t, errs, "Name", "User Name Invalid")
	AssertCustomErrorHandler(t, errs, "Email", "User Email Invalid")
}
```