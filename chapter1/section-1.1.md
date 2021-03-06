# Section 1.1

### Exercise 1.1

```scheme
10 ; -> 10
(+ 5 3 4) ; -> 12
(-9 1) ; -> 8
(/ 6 2) ; -> 3
(+ (* 2 4) (- 4 6)) ; -> 6
(define a 3) ; -> a <- 3 but there is no result from the expression`
(define b (+ a 1)) ; -> b <- 4 but there is no result from the expression`
(+ a b (* a b)) ; -> 19
(= a b) ; -> #f
(if (and (> b a) (< b (* a b))) b a) ; -> 4
(cond ((= a 4) 6) ((= b 4) (+ 6 7 a)) (else 25)) ; -> 16
(+ 2 (if (> b a) b a)) -> 6`
(* (cond ((> a b) a) ((< a b) b) (else -1)) (+ a 1)) ; -> 16
```

### Exercise 1.2

```scheme
(/ 
  (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 5))))) 
  (* 3 (- 6 2) (- 2 7))
) ; -37/150

```

### Exercise 1.3

```scheme
(define (sum-square-largest-two a b c)
  (cond
    ((or (and (>= a b) (>= b c))
        (and (>= b a) (>= a c)))
    (+ (* a a) (* b b)))
    ((and (>= a b)) (+ (* a a) (* c c) ))
    (else (+ ( * b b) (* c c)))))
```

### Exercise 1.4

```scheme
(define (a-plus-abs-b a b)
  ((if (> b 0) + -) a b))
```

The if expression will resolve to either `-` or `+` based on the value of `b` so if `b` is negative then the expression will look like `( - a b)` and if b is positive then the expression will look like `(+ a b)`

### Exercise 1.5

```scheme
(define (p) (p))

(define (test x y)
  (if (= x 0) 
    0
    y))

(test 0 (p))
```

**applicative order**: using applicative order the interpreter will evaluate both arguments to the procedure test before passing them to the procedure thus attempting to evaluate `(p)` before passing it. But since the value of `(p)` is `(p)` then the interpreter will get stuck in a loop trying to evaluate `(p)` and `(test 0 (p)`

**normal order:** using normal order the interpreter will pass the arguments first and evaluate them when needed. So the evaluation will look like this

```scheme
(test 0 (p)) -> (if (= 0 0) 0 (p)) -> 0
```

### Exercise 1.6

The code for computing square roots looks like this:

```scheme
(define (sqrt-iter guess x)
  (if (good-enough? guess x)
      guess
      (sqrt-iter (improve guess x) x)))
```

Currently the interpreter only evaluates the \`else\` part of the statement if `good-enough?` returns a false value. When Alyssa replaces the call to `if` with a call to a procedure `new-if` the interpreter \(which uses applicative order to evaluate the expression\) will try and evaluate both clauses before passing them on to the procedure, which will result in an infinite loop.

### Exercise 1.7

```scheme
(define (sqrt x)
  (define (good-enough? guess old-guess)
    (< (abs (- guess old-guess)) 0.001))
  (define (improve guess)
    (average (/ x guess) guess))
  (define (average x y)
    (/ (+ x y) 2))
  (define (sqrt-iter guess old-guess)
    (if (good-enough? guess old-guess)
        guess
        (sqrt-iter (improve guess) guess)))
  (sqrt-iter 1.0 x))
```

### Exercise 1.8

```scheme
(define (cube-root x)
  (define (cube x) (* x x x))
  (define (square x) (* x x))
  (define (good-enough? guess old-guess)
    (< (abs (-  guess old-guess)) 0.001))
  (define (improve guess)
    (/ (+ (/ x (square guess)) (* 2 guess)) 3))
  (define (cube-root-iter guess old-guess)
    (if (good-enough? guess old-guess)
        guess
        (cube-root-iter (improve guess) guess)))
  (cube-root-iter 1.0 x))
```

