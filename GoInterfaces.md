# Интерфейсы в Go

Что такое интерфейс? Как использование интерфейсов влияет на `code coupling`? Использование интерфейсов реализует контрактное программирование?\
\
Что такое пустой интерфейс? Что собой представляет тип данных `any`?\
\
Как сообщить компилятору что пользовательский тип реализует интерфейс?\
\
Где следует разместить описание интерфейса: в пакете с реализацией или в пакете, где этот интерфейс используется?\
\
Что такое `type switch` и когда используется?\
\
Как узнать внутренний тип переменной типа пустой интерфейс?

<details open>
	<summary><h3>Что выведет код?</h3></summary>

```go
var err error
fmt.Println(err.Error())  // какое значение имеет err по умолчанию?
```

```go
var f1, f2 func() int = func() int { return 1 }, nil
execute := func(f func() int) {
	if f == nil { // может ли тип функции принимать значение nil?
		fmt.Println("nil")
		return
	}
	fmt.Println("func =", f())
}
execute(nil)
execute(f1)
execute(f2)
```

```go
var f1, f2 func() int = func() int { return 1 }, nil
deduce := func(v any) {
	if v == nil { // когда интерфейс равен nil?
		fmt.Println("nil")
		return
	}
	if _, ok := v.(func() int); ok {
		fmt.Println("v =", v.(func() int)()) // что произойдет при разименовании nil-значения?
	}
}
deduce(nil)
deduce(f1)
deduce(f2)
```

```go
type CustomError string
var err CustomError
var v any = err
fmt.Println(v == nil)
fmt.Println(v.(CustomError) == "")
```

```go
type CustomError string

func (e CustomError) Error() string {  // что выведется если не определять этот метод?
	return string(e)
}

func customError() error {   // какой тип возвращаемого значения?
	var err *CustomError     // какой тип у локальной переменной?
	fmt.Println(err == nil)
	return err               // какой внутренний тип у возвращаемого объекта типа error? 
}

func simpleError() error {
	var err error
	fmt.Println(err == nil)  // когда переменная типа error равна nil?
	return err
}

func newError() error {
	err := errors.New("new error")
	fmt.Println(err == nil)
	return err
}

func main() {
	fmt.Println(customError() == nil)
	fmt.Println(simpleError() == nil)
	fmt.Println(newError() == nil)
}
```

</details>
