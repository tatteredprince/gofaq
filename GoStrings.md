# Строки в Go

Что собой представляет тип данных `rune`?\
\
Что вернет `len()` с аргументом типа `string`? А `rune`?\
\
Как прочитать посимвольно строку типа `rune`?

<details open>
  <summary><h2>Что выведет код?</h2></summary>

```go
type User struct {
	Name string
}
m1 := map[string]*User{"": {}}
m1[""].Name = "John"
m2 := map[string]User{"": {}}
m2[""].Name = "Kevin"
```

</details>
