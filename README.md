

# pipeR

[![Build Status](https://travis-ci.org/renkun-ken/pipeR.png?branch=master)](https://travis-ci.org/renkun-ken/pipeR)

High-performance pipeline operator and object

## What's new in 0.4?

- **Major API Change**: `%>>%` operator now handles all pipeline mechanisms and other operators are deprecated.
- Add `Pipe` object that supports object-based pipeline operation.

[Release notes](https://github.com/renkun-ken/pipeR/releases)

## Installation

Install from CRAN:

```r
install.packages("pipeR")
```

Install the development version from GitHub (`devtools` package is required):

```r
devtools::install_github("pipeR","renkun-ken")
```

## Usage

The form of the object following `%>>%` determines which piping mechanism is used. If the operator is followed by, for example, 

- `foo` or `foo(...)`: Pipe to its first argument;
- `{ expression }` or `( expression )`: Pipe to symbol `.`;
- `(x -> expression)` or `(x ~ expression)`: Pipe to symbol `x`.

## Examples

### `%>>%`

Pipe as first-argument to a function:

```r
rnorm(100,mean=10) %>>%
  log %>>%
  diff %>>%
  plot(col="red")
```

Pipe as `.` to an expression enclosed by `{}` or `()`:

```r
rnorm(100) %>>% {
  x <- .[1:50]
  y <- .[51:100]
  lm(y ~ x)
}
```

```r
mtcars %>>%
  (lm(mpg ~ ., data = .))
```

Pipe by lambda expression enclosed by `()`:

```r
mtcars %>>%
  (df -> lm(mpg ~ ., data = df))

mtcars %>>%
  (df ~ lm(mpg ~ ., data = df))
```

### `Pipe()`

`Pipe()` creates a Pipe object. 

- `$` chains functions by first-argument piping and always returns a Pipe object.
- `fun()` evaluates an expression with `.` or by lambda expression.
- `[]` extracts the final value of the Pipe object.

Pipe as first-argument to a function:

```r
Pipe(rnorm(100,mean=10))$
  log()$
  diff()$
  plot(col="red")
```

```r
Pipe(1:10)$
  fun(x -> x + rnorm(1))$
  mean() []
```

## Performance

[Benchmark tests](http://cran.r-project.org/web/packages/pipeR/vignettes/Performance.html) show that pipeR operator and Pipe object can achieve high performance especially when they are intensively called.

- If you do not care about the performance of intensive calling and need heuristic distinction between different piping mechanisms, you may use `%>%` in [magrittr](https://github.com/smbache/magrittr) which also provides additional aliases of basic functions. 
- If you care about performance issues and are sure which type of piping you are using, pipeR can be a helpful choice.

## Vignettes

The package also provides the following vignettes:

- [Introduction](http://cran.r-project.org/web/packages/pipeR/vignettes/Introduction.html)
- [Examples](http://cran.r-project.org/web/packages/pipeR/vignettes/Examples.html)
- [Performance](http://cran.r-project.org/web/packages/pipeR/vignettes/Performance.html)


## Help overview

```r
help(package = pipeR)
```

## License

This package is under [MIT License](http://opensource.org/licenses/MIT).