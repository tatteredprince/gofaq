# Что выведет программа?

## Строки

```go
str := "Hello, world!"
fmt.Println(str[0]) 
str[0] = "R"            // в Go можно менять строки?
fmt.Println(str)
```

## Слайсы

```go
x := []int{}
x = append(x, 0)
x = append(x, 1)
x = append(x, 2)   // какой размер массива слайса?
y := append(x, 3)  // при добавлении элемента происходит аллокация нового массива слайса?
z := append(x, 4)  // как изменяются len(x) и cap(x)?
fmt.Println(y, z)
```

```go
x := []int{1, 2, 3, 4, 5}
var y []int
y = append(x, 6)   // если len(x) и cap(x) равны, при добавлении элемента производится аллокация нового массива слайса?
y = append(x, 7)   // тот же вопрос
x = y
y = append(x, 8)   // когда cap достаточно при добавлении элемента, производится аллокация нового массива слайса?
fmt.Println(x, y)
```

```go
x := []int{1, 2, 3, 4, 5}
y := x[1:3]                // какой тип, len и cap у среза слайса?
y = append(y, 6, 7)        // хватит ли cap у среза слайса для добавляемых эдементов?
fmt.Println(x, y)
for i := range 5 {
	y = append(y, i+10)    // при исчерпании cap произойдет аллокация нового массива слайса?
}
fmt.Println(x, y)
```

```go
a1 := make([]int, 0, 10)
a1 = append(a1, []int{1, 2, 3, 4, 5}...)  // cap(a1) хватит для добавляемых элементов?
a2 := append(a1, 6)                       // производится аллокация нового массива слайса?
a3 := append(a1, 7)                       // append создает новый slice header?
fmt.Println(a1, a2, a3)                   // в чем отличия len(a1) от len(a2) и len(a3)?
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

## Массивы

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

## Интерфейсы

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

## Горутины

```go
var wg sync.WaitGroup
wg.Add(5)
var i int                // как изменится вывод если объявить i как переменную цикла на следующей строке?
for i = 0; i < 5; i++ {
	go func() {          // инструкция go сразу начинает исполнение горутин?
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
	}(i)                  // какое значение передается в функцию при каждой итерации?
}
wg.Wait()
```

## Структуры

```go
type A struct {  // какой физический и фактический размеры структуры?
	a bool
	b int32 
	c float64
}

type B struct {  // сумма размеров полей равна фактическому размеру структуры?
	c float64
	b int32
	a bool
}

type C struct {  // как эффективно организовать поля чтобы уменьшить фактический размер структур в памяти?
	a bool
	c float64
	b int32
}

func main() {
	println(unsafe.Sizeof(A{}))
	println(unsafe.Sizeof(B{}))
	println(unsafe.Sizeof(C{}))
}
```
