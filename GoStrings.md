# Строки в Go

Составляющие структуры `string`.\
\
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

```go
str := "Hello, 世界"
fmt.Println(len(str))
for i, c := range str {
  fmt.Printf("index %d character %c\n", i, c)
}
```

```go
chars := make([]byte, len(str))  // chars := []byte(str)
fmt.Println(copy(chars, str))
for i, c := range chars {
  fmt.Printf("index %d character %c\n", i, c)
}
```

</details>
