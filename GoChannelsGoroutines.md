# Каналы и горутины в Go

Что такое каналы? Какие виды каналов существуют?\
\
Отличия буферизированного и небуферизированного каналов.\
\
Составляюшие структуры канала.\
\
Какой размер буфера по умолчанию у канала?\
\
Какие существуют ограничения на типы данных при передаче по каналам?\
\
Что произойдет при чтении из закрытого канала?\
\
Что произойдет при записи в закрытый канал?\
\
Что произойдет при записи в неинициализированный канал?\
\
В чем отличие горутин от потоков?\
\
Что такое потоковая безопасность? Как в `Go` обеспечить потоковую безопасность?\
\
Какая операция называется атомарной? Какие операции в `Go` атомарные? Как в `Go` выполняются атомарные операции?\
\
В чем отличия работы системного и `Go` планировщиков? Опишите алгоритм работы планировщика `Go` в терминах `G` `M` `P`.\
\
Возможно ли ограничить максимальное число горутин?\
\
Если установить `runtime.GOMAXPROCS` в единицу, то программа с 1000 простыми горутинами сможет запуститься и завершится?\
\
Можно ли запустит программу с миллионом горутин?\
\
Можно ли вытеснить горутину из очереди исполнения? Можно ли остановить выполняющуюся горутину? Какой инструмент в `Go` можно приспособить для завершения горутин?\
\
Почему важно завершать дочерние горутины по окончании и/или прерывании работы программы?\
\
Когда возникает состояние гонки данных? Как в `Go` избегают таких состояний?\
\
Можно ли использовать один и тот же буфер типа `[]byte` в нескольких горутинах?\
\
Что такое `deadlock`? Когда они возникают в `Go`?\
\
Есть ли в `Go` структуры типа `lock free`? Что собой представляет `sync.Map` и типы из модуля `sync/atomic`? За счет чего атомики такие быстрые?\
\
Паттерны `Fan-In` и `Fan-Out`, уметь реализовать их в коде.\
\
Какой `memory footprint` у горутин?\
\
Отличия `sync.RWMutex` от `sync.Mutex`.\
\
Когда следует использовать `errgroup`?

<details open>
  <summary><h3>Что выведет код?</h3></summary>

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
wg := sync.WaitGroup{}
for i := range 5 {
	wg.Add(1)
	go func() {
		defer wg.Done()
		fmt.Println(i)
	}()
	wg.Wait()
}
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

```go
worker := func(ch chan string, ctx context.Context) {
	for {
		select {
		case v := <-ch:
			fmt.Println("read from channel:", v)
		case <-ctx.Done():
			fmt.Println("done working")
			return
		// что будет если добавить условие default?
		// что будет если в default добавить return?
		}
		fmt.Println("do some work")
		time.Sleep(200 * time.Millisecond)
	}
}
ctx, cancel := context.WithTimeout(context.Background(), time.Second)
defer cancel()
ch := make(chan string)
go worker(ch, ctx)
ch <- "start working"
ch <- "continue work"
time.Sleep(3 * time.Second)
fmt.Println("the end")
```

```go
worker := func(ch <-chan int) {
	for v := range ch {
		fmt.Println("read from channel:", v)
	}
	close(ch) // можно ли закрыть канал для чтения?
}
ch := make(chan int)
go worker(ch)
ch <- 123
time.Sleep(1 * time.Second)
```

</details>
