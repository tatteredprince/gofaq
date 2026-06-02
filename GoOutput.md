# Что выведет программа?

## Строки

```go
str := "Hello, world!"
fmt.Println(str[0]) 
str[0] = "R"              // в Go можно менять строки?
fmt.Println(str)
```

## Слайсы и массивы

```go
x := []int{}
x = append(x, 0)
x = append(x, 1)
x = append(x, 2)     // какой размер внутреннего массива?
y := append(x, 3)    // при добавлении элемента происходит аллокация нового массива?
z := append(x, 4)    // как изменяются len(x) и cap(x)?
fmt.Println(y, z)
```

### Что происходит при присваивании слайса слайсу?
```go
x := []int{1, 2, 3, 4, 5}
var y []int
y = append(x, 6)
y = append(x, 7)
x = y
y = append(x, 8)
fmt.Println(x, y)
```

### Что происходит при исчерапании `cap` у слайсинга оригинального слайса?
```go
x := []int{1, 2, 3, 4, 5}
y := x[1:3]
y = append(y, 6, 7)
fmt.Println(x, y)
for i := range 5 {
	y = append(y, i+10)
}
fmt.Println(x, y)
```

### Что выведется если `a1` проинициализировать слайсом нулевого размера?
```go
a1 := make([]int, 0, 10)
a1 = append(a1, []int{1, 2, 3, 4, 5}...)
a2 = append(a1, 6)
a3 = append(a1, 7)
fmt.Println(a1, a2, a3)
```

### Итерация по значению массива.
```go
a, b := [...]int{1, 2, 3}, [3]int{0: 4, 5, 2: 6}
for i, v := range a {
	if i == 1 {
		a = b
	}
	fmt.Println(v)
}
fmt.Println(a)
fmt.Println(b)
```

### Итерация по ссылке на массив, `range` возвращает индекс и копию элемента массива?
```go
a, b = [3]int{1, 2, 3}, [3]int{4, 5, 6}
for i, v := range &a {
	if i == 1 {
		a = b
	}
	fmt.Println(v)
}
fmt.Println(a)
fmt.Println(b)
```

### Почему модификация второго слайса не изменяет содержимое первого?
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

## Интерфейсы

## Горутины

```go
var wg sync.WaitGroup
wg.Add(5)
var i int                  // как изменится вывод если объявить i как переменную цикла на следующей строке?
for i = 0; i < 5; i++ {
	go func() {            // инструкция go сразу начинает исполнение горутин?
		fmt.Println(i)
		wg.Done()
	}()
}
wg.Wait()
```

```go
var wg sync.WaitGroup
wg.Add(5)
var i int
for i = 0; i < 5; i++ {
	go func(val int) {
		fmt.Println(val)
		wg.Done()
	}(i)                    // какое значение передается в функцию при каждой итерации?
}
wg.Wait()
```
