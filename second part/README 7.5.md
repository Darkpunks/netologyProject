__________________________________________________________________________
Домашнее задание к занятию "7.5. Основы golang"
__________________________________________________________________________
Задача 1. Установите golang.
Воспользуйтесь инструкций с официального сайта: https://golang.org/.
Так же для тестирования кода можно использовать песочницу: https://play.golang.org/.

```
[ivan@localhost ~]$ go version
go version go1.19 linux/amd64
```

 <img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/7.5/7.5.1.jpg">


Задача 2. Знакомство с gotour.
У Golang есть обучающая интерактивная консоль https://tour.golang.org/. Рекомендуется изучить максимальное количество примеров. В консоли уже написан необходимый код, осталось только с ним ознакомиться и поэкспериментировать как написано в инструкции в левой части экрана.

Ознакомился


<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/7.5/7.5.2.jpg">

Задача 3. Написание кода.
Цель этого задания закрепить знания о базовом синтаксисе языка. Можно использовать редактор кода на своем компьютере, либо использовать песочницу: https://play.golang.org/.

Напишите программу для перевода метров в футы (1 фут = 0.3048 метр). Можно запросить исходные данные у пользователя, а можно статически задать в коде. Для взаимодействия с пользователем можно использовать функцию Scanf:

```
package main

import "fmt"

func main() {
    fmt.Print("Enter a number: ")
    var input float64
    fmt.Scanf("%f", &input)

    output := input * 0.3048

    fmt.Println(output)    
}

```
```
[ivan@localhost netology-go]$ go run example.go
Enter a number: 30
9.144
```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/7.5/7.5.3.jpg">

Получается: 30 футов получается 9,144 метра

Напишите программу, которая найдет наименьший элемент в любом заданном списке, например:

x := []int{48,96,86,68,57,82,63,70,37,34,83,27,19,97,9,17,}



```
package main
  import "fmt"
  func main() {
    x := []int{48,2, 96,86,3,68,57,82,63,70,37,34,83,27,19,97,9,17,1}
    zero := 0
    for i, less := range x {
      if (i == 0) {
        zero = less
      } else {
        if (less < zero) {
          zero = less
        }
      }
      }
    fmt.Println("Min integer: ", zero)
  }
```

```
[ivan@localhost netology-go]$ go run example1.go
Min integer:  1

```
<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/7.5/7.5.3-1.jpg">


Напишите программу, которая выводит числа от 1 до 100, которые делятся на 3. То есть (3, 6, 9, …).

```
 package main
  import "fmt"
   func main() {
    for i := 1; i <= 100; i++ {
      if (i%3) == 0 {
        fmt.Print("[",i,"]")
      }
    }
  }
```

```
[ivan@localhost netology-go]$ go run example2.go
[3][6][9][12][15][18][21][24][27][30][33][36][39][42][45][48][51][54][57][60][63][66][69][72][75][78][81][84][87][90][93][96][99]

```

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/7.5/7.5.3-2.jpg">


