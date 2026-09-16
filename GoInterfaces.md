# Go: интерфейсы

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
func main() {
	f := func() int { return 1 }
	execute(f)
	deduce(f)
	f = nil
	execute(f)
	deduce(f)
}

func execute(f func() int) {
	if f == nil {  // может ли тип функции принимать значение nil?
		fmt.Println("execute: nil")
		return
	}
	fmt.Println("execute: func() int", f())
}
func deduce(v any) {  // какой тип соответствует интерфейсу any?
	if v == nil {     // когда интерфейс равен nil?
		fmt.Println("deduce: nil")
		return
	}
	if _, ok := v.(func() int); ok {
		fmt.Println("deduce: func() int", v.(func() int)())  // что произойдет при разименовании nil-значения?
	}
}
```

</details>
