# Что выведет программа?

## Горутины и каналы

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

```go
worker := func() chan int {
	ch := make(chan int)
	go func() {
		time.Sleep(3 * time.Second)
		ch <- 1
	}()
	return ch
}
t := time.Now()
_, _ = <-worker(), <-worker()
fmt.Println(int(time.Since(t).Seconds()))
```

```go
worker := func() chan int {
	ch := make(chan int)
	go func() {
		time.Sleep(3 * time.Second)
		ch <- 1
	}()
	return ch
}
t := time.Now()
ch1, ch2 := worker(), worker()
_, _ = <-ch1, <-ch2
fmt.Println(int(time.Since(t).Seconds()))
```

```go
ch := make(chan int, 1)
for i := range 10 {
	select {
	case ch <- i:
	case num := <-ch:
		fmt.Println(num)
	}
}
```
