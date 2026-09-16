# Слайсы и массивы в Go

В чем отличие слайсов от массивов?\
\
Составляющие структуры слайса.\
\
Можно ли передавать слайс по ссылке?\
\
Какие операции над неинициализированным слайсом можно совершить?\
\
Как работает `append()` и возможно ли им проинициализировать пустой слайс?\
\
Всегда ли `capacity` слайса увеличивается в два раза при использовании `append`?\
\
Почему перед использованием слайс нужно инициализировать, а массив нет?\
\
Как создать многомерный слайс?

<details open>
	<summary><h3>Что выведет код?</h3></summary><br>

```go
x := []int{}
x = append(x, 0)
x = append(x, 1)
x = append(x, 2)      // какие len и cap у слайса x?
y := append(x, 3)     // происходит ли реаллокация массива слайса x? 
z := append(x, 4)     // меняются ли len и cap у слайса x?
fmt.Println(x, y, z)
x = append(x, 5)      // как меняются len и cap у слайса x?
fmt.Println(x, y, z)  // на какой массив ссылаются заголовки слайсов y и z?
```

```go
x := []int{1, 2, 3, 4, 5}
var y []int
y = append(x, 6)   // происходит ли реаллокация массива слайса x?
y = append(x, 7)   // изменяются ли len и cap слайса x?
x = y              // на какой массив ссылается слайс x, и какие у него len и cap? 
y = append(x, 8)   // как меняются len и cap у слайса x?
fmt.Println(x, y)
```

```go
x := []int{1, 2, 3, 4, 5}
y := x[1:3]              // как формируются len и cap среза y?
y = append(y, 6, 7)      // хватит ли cap слайса y для добавляемых элементов?
fmt.Println(x, y)
for i := range 5 {
	y = append(y, i+10)  // произойдет ли реаллиакация нового массива слайса y?
}
fmt.Println(x, y)
```

```go
x := []int{1, 2, 3}
y := x[2:3]        // как формируются len и cap среза y?
y[0] = 4           // у слайсов x и y общий массив?
y = append(y, 5)   // происходит ли реаллокация массива слайса y?
y = append(y, 6)   // непосредственно после реаллокации что находится в массиве слайса y?
y[0] = 7           // теперь у слайсов x и y общий массив?
fmt.Println(x, y)
```

```go
a1 := make([]int, 0, 10)
a1 = append(a1, []int{1, 2, 3, 4, 5}...)
a2 := append(a1, 6)      // происходит ли реаллокация нового массива слайса a1?
a3 := append(a1, 7)      // append здесь изменяет len и cap слайса a1?
fmt.Println(a1, a2, a3)
```

```go
first := []int{1, 2, 3, 4, 5}
second := make([]*int, len(first))
for i, v := range first {  // итератор range возвращает копии или оригиналы объектов?
	second[i] = &v         // куда ссылаются элементы слайса?
	*second[i] *= 10
}
fmt.Println(first)
fmt.Println(*second[0], *second[1], *second[2], *second[3], *second[4])
```

```go
func modify(s []int) {
	s[0] *= 2
	s = append(s, 7)    // происходит ли shadowing переданной переменной?
	s[1] /= 2
}

func modifyp(s *[]int) {
	(*s)[0] *= 2
	*s = append(*s, 7)  // происходит ли shadowing переданной переменной?
	(*s)[1] /= 2
}

func main() {
	s1, s2 := []int{1, 2, 3}, []int{4, 5, 6}
	modify(s1)
	modifyp(&s2)
	fmt.Println(s1, s2)
}
```

```go
var s []int                     // чему равен указатель внутреннего массива слайса?
fmt.Println(s, len(s), cap(s))
for _, v := range s {
	fmt.Println(v)
}
```

```go
_ = make([]int, -5)
```

```go
size := -5
_ = make([]int, size)
```

```go
a, b := [...]int{1, 2, 3}, [3]int{0: 4, 5, 2: 6}
for i, v := range a {  // итератор range возвращает копии или оригиналы объектов?
	if i == 1 {
		a = b
	}
	fmt.Println(v)
}
fmt.Println(a, b)
```

```go
a, b := [3]int{1, 2, 3}, [3]int{4, 5, 6}
for i, v := range &a {  // итератор range по ссылке на массив возвращает копии или оригиналы объектов?
	if i == 1 {
		a = b
	}
	fmt.Println(v)
}
fmt.Println(a, b)
```

</details>
