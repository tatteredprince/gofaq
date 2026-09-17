# Функции в Go

Функции в `Go` считаются типами?\
\
Как в `Go` реализована поддержка замыканий?\
\
Когда отрабатывает отложенная функция в `defer`?\
\
Какой порядок вызова отложенных функций используемых с `defer`?\
\
Чем чревато множественное использование  `defer`?\
\
Опишите принцип работы функции `recover()`.\
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

</details>
