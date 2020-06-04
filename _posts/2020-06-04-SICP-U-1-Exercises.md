---

layout: post
title: SICP exercises
subtitle: U1
tags: [SICP, Lisp]

---

### 1.2 写前缀形式

```Lisp
(/ (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 5)))))
   (* 3 (- 6 2) (- 2 7)))
```

### 1.3 定义一个过程,以三个数为参数,返回其中较大两个数的平方和.
```LISP
(define (sum-of-squares x y)
  (+ (* x x) (* y y)))

(define (f a b c)
  (cond ((> a b) (cond ((> b c) (sum-of-squares a b))
                       ((< b c) (sum-of-squares a c))))
        ((< a b) (if (< a c)
                     (sum-of-squares b c)
                     (sum-of-squares a b)))))
```

### 1.4 描述以下过程

```
(define (a-plus-abs-b a b)
	((if (> b 0) + -) a b))
```

根据b是否大于零来决定a和b的运算符.

### 1.5 解释正则序和应用序执行下列代码发生什么

```
(define (p) (p))

(define (test x y)
  (if (= x 0)
      0
      y))

(test 0 (p))
```

(define (p) (p))是一个不断调用自己的过程,因此一旦执行,就会无限循环.那么正则序和应用序的不同在于,先调用还是先展开,如果是正则序,先展开,再执行,则会输出0,因为(p)始终没有调用.而应用序则相反,先调用,那么就会进入无限循环.