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