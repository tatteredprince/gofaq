# Map в Go

Какими типами ограничены ключи в `map`?\
\
Можно ли использовать объект типа структуры состоящей из полей типов `string`, `int` и `bool` как ключ в `map`?\
\
Почему программа паникует при попытке записи ключа в неинициализированный `map`?\
\
Можно ли получить адрес элемента `map` используя оператор `&`? Элементы в `map` адресуемые?\
\
Можно ли изменять значения полей структуры как элемента `map`?\
\
Может ли поиск по `map` стать сложнее чем `O(1)`?\
\
В каком порядке оператор `range` возвращает пары ключ-значение при итерации по `map`?

<details open>
  <summary><h3>Что выведет код?</h3></summary>

```go
m := map[string]int{"a": 1, "b": 0, "c": 3}
e := new(m["b"])
e = &m["b"]
*e = 2
fmt.Println(m)
```

```go
m := map[string][]int{"s": []int{1, 0, 3}}
e := new(m["s"][1])
e = &m["s"][1]
*e = 2
fmt.Println(m)
```

```go
type User struct {
	Name  string
}
m := map[string]User{"admin": {"John"}, "accountant": {"James"}}
m["admin"].Name = "Alexander"
```

```go
type User struct {
	Name string
}
m1 := map[string]*User{"": {}}
m1[""].Name = "John"
m2 := map[string]User{"": {}}
m2[""].Name = "Kevin"
```

```go
package main

type User struct {
	Name string
}

func (u *User) SetUsername(name string) {
	u.Name = name
}

func main() {
	m := map[string]User{"admin": {"John"}}
	m["admin"].SetUsername("Alexander")
}
```

</details>
