<!-- README.md is generated from README.Rmd. Please edit that file -->

# Grammar of Graphics for base plot

## Aesthetic mapping

`bb_aes()` for aesthetic mapping, that equivalents to `ggplot2::aes()`.

``` r
library(bbplot)

p <- bbplot(mtcars, bb_aes(mpg, disp, col=factor(cyl)))
p + bb_point()
```

![](README_files/figure-gfm/aes-1.png)<!-- -->

## Geometric layer

``` r
p2 <- p + bb_point() + bb_lm(bb_aes(group=cyl), lwd=2)
p3 <- p2 + bb_lm(col="red", lwd=3, lty='dotted')
par(mfrow=c(1,2))
p2; p3
```

![](README_files/figure-gfm/layer-1.png)<!-- -->

### TODO

  - [x] bb\_point
  - [x] bb\_lm
  - more layers need to be added

## Setting labels

``` r
p2 + bb_labs(title = "hello", sub = "just for demo",
              xlab="this is xlab", ylab = "this is ylab") +
  bb_title("hello world") # last one rules            
```

![](README_files/figure-gfm/labs-1.png)<!-- -->

## Theme

``` r
g <- p2 +
     bb_theme(col.main="red", cex.main=2,
             mar = c(4, 4, 3, 1)) +
     bb_title("applying graphics::par")
par(mfrow=c(1,2))
print(g)
p2 + bb_title("theme has no side effect")
```

![](README_files/figure-gfm/theme-1.png)<!-- -->

`bb_theme` has no side effect and will only apply to the `bbplot` that
it added to. This is very important for developing pre-defined themes.

``` r
par(mfrow=c(1,2))
p3 + bb_theme_expand()
print(p3)
```

![](README_files/figure-gfm/theme-expand-1.png)<!-- -->

### TODO

  - [x] bb\_theme\_expand
  - develop more pre-defined themes

## Scale

Not yet implemented

## Legend

Not yet implemented

## Using existing code with bbplot

Suppose we have existing code to plot something:

``` r
plot(mtcars$mpg, mtcars$disp)
abline(lm(disp ~ mpg, data=mtcars), col='red')
```

We can wrap the codes into a function:

``` r
f <- function() {
  plot(mtcars$mpg, mtcars$disp)
  abline(lm(disp ~ mpg, data=mtcars), col='red')
}
```

Then we can convert it to a `bbplot` object. The plot produced by the
function will be used as canvas, and we can apply theme and add layers
to it:

``` r
as.bbplot(f) +
   bb_theme_expand() +
   bb_lm(bb_aes(mpg, disp, group=cyl, col=factor(cyl)), data=mtcars, lwd=2, lty='dashed')
```

![](README_files/figure-gfm/base-1.png)<!-- -->
