# Функции в Go

Знать как работают "под капотом" функции `append()`, `len()`, `cap()`, `clear()`, `copy()` и `delete()`. И уметь их применять на практике.\
\
Функции в `Go` считаются типами?\
\
Как в `Go` реализована поддержка замыканий?\
\
Когда отрабатывает отложенная функция в `defer`?\
\
Какой порядок вызова отложенных функций?\
\
Чем чревато множественное использование отложенных функций?\
\
Может ли отложенная функция отработать после паники?\
\
В каких случаях может не отработать отложенная функция?\
\
Опишите принцип работы функций `panic()` и `recover()`.\
\
В каком случае для `generic` функций не нужно указывать тип?

<details open>
  <summary><h3>Что выведет код?</h3></summary>

```go
defer fmt.Println("Deferred one")
defer fmt.Println("Deferred two")
fmt.Println("Starting")
go func() {
  panic("golang is dead")
}()
fmt.Println("Ending")
```

```go
defer fmt.Println("Deferred one")
defer func() {
  if r := recover(); r != nil {
    fmt.Println("Recovered from panic, message:", r)
  } else {
    fmt.Println("Recovered from panic")
  }
}()
defer fmt.Println("Deferred two")
fmt.Println("Starting")
go func() {
  panic("golang is dead")
}()
fmt.Println("Ending")
```

```go
defer fmt.Println("Deferred one")
defer fmt.Println("Deferred two")
fmt.Println("Starting")
go func() {
  defer func() {
    if r := recover(); r != nil {
      fmt.Println("Recovered from panic, message:", r)
    } else {
      fmt.Println("Recovered from panic")
    }
  }()
  panic("golang is dead")
}()
fmt.Println("Ending")
```

```go
defer fmt.Println("Deferred one")
defer func() {
  if r := recover(); r != nil {
    fmt.Println("Recovered from panic, message:", r)
  } else {
    fmt.Println("Recovered from panic")
  }
}()
defer fmt.Println("Deferred two")
fmt.Println("Starting")
panic("golang is dead")
fmt.Println("Ending")
```

</details>
