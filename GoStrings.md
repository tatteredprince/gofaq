# Строки в Go

Что собой представляет тип данных `rune`?\
\
Что вернет `len()` с аргументом типа `string`? А `rune`?\
\
Как прочитать посимвольно строку типа `rune`?

<details open>
  <summary><h2>Что выведет код?</h2></summary>

```go
str := "Hello, world!"
fmt.Println(str[0]) 
str[0] = "R"            // в Go можно менять строки?
fmt.Println(str)
```

</details>
