+++
categories = [clojure,fp]
date = "2016-01-05T16:31:07+08:00"
description = ""
keywords = [clojure]
title = "clojure functional programming"

+++

#Clojure函数式编程

[toc]
##函数
###一等函数
函数在clj里面是一等公民;指的是函数可以被当做普通数据对待，具有以下特性：

- 可以按需创建
- 可以在数据结构中存储
- 可以当做函数实参
- 可以当做函数返回值

这此特性可以为程序提供更多的灵活性.


###闭包
- 闭包指的是函数可以访问成员变量，一般需要函数A返回一个函数B，在返回函数B里持有函数A的成员变量，形成一个闭包。
- 也可以看做是**函数定制**的一种手段
```clojure
(defn repeat-n [n]
    (fn [num] (* n num)))

(def repeat-2 (repeat-n 2))
(repeat-2 5)  ;;=> 10
```


###尾递归
尾递归顾名思义,是指在函数**尾部**进行了递归调用. 与一般的递归区别于返回方式&资源释放.
一般递归需要依次返回,返回值是由首次调用函数返回的,资源需要返回后才能被释放.如递归调用函数A,A1调用A2最终返回值是由A2返回给A1,A1再返回给caller. 并且在A2执行时, A1的资源不会被释放.所以调用时要考虑递归层级问题,如果层级太深会发生堆栈溢出.
尾递归是在A2执行时,A1的资源会被释放,并且最终返回结果是由A2直接返回给caller.所以在调用时,不会出现溢出问题.
jvm并不支持尾递
```clojure
;;一般递归
(defn pow [base exp]
"计算base的exp次幂"
    (if (zero? exp)
        1
        (* base (pow base (dec exp))))
)

;;尾递归版本(clojure版本)
(defn pow (base exp)
    (letfn [(kapow [base exp sum]
            (if (zero? exp)
                sum
                (recure base (dec exp) (* base sum)))]
            (kapow base exp 1)))
```
