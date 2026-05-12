# Что выведет программа?

### Что выведется после выполнения программы?
```go
str := "Hello, world!"
fmt.Println(str[0]) 
str[0] = "R"
fmt.Println(str)
```

### Что выведет программа?
```go
x := []int{}
x = append(x, 0)
x = append(x, 1)
x = append(x, 2)
y := append(x, 3)
z := append(x, 4)
fmt.Println(y, z)
```

### Что выведется если `a1` проинициализировать слайсом нулевого размера?
```go
a1 := make([]int, 0, 10)
a1 = append(a1, []int{1, 2, 3, 4, 5}...)
a2 = append(a1, 6)
a3 = append(a1, 7)
fmt.Println(a1, a2, a3)
```

### Почему модификация второго слайса не изменяет содержимое первого?</summary>
```go
func main() {
	first := []int{10, 20, 30, 40, 50}
	second := make([]*int, len(first))
	for i, v := range first {
		second[i] = &v
		*second[i] *= 10
	}
	fmt.Println(first)
	fmt.Println(*second[0], *second[1], *second[2], *second[3], *second[4])
}
```

### Как в `handle()` вернуть ошибку не используя дополнительных пакетов? Возможно используя интерфейс `error`? 
```go
func main() {
  println(handle())
}

func handle() error {
  ...
}
```
