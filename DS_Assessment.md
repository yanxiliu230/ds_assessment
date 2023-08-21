Data Science Assessment
================
Nora Liu
2023-08-21

``` r
library(rvest)
library(stringr)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(ggplot2)
```

## Problem 1

``` r
html <- read_html("https://en.wikipedia.org/wiki/List_of_natural_disasters_by_death_toll") # read in
```

``` r
t_20 <- html_nodes(html, "table")[2] %>% html_table() # 20th century table
t_21 <- html_nodes(html, "table")[3] %>% html_table() # 21st century table
t <- rbind(t_20[[1]], t_21[[1]])                      # merge
```

``` r
temp <- t$`Death toll`                              # placeholder so we do not directly work on t
temp <- str_replace_all(temp, "\\[\\S*\\]", "")     # remove brackets
temp <- str_replace_all(temp, ",", "")              # remove comma
temp <- str_replace_all(temp, "\\+", "")            # remove plus sign
temp <- lapply(str_split(temp, "\\D+"), as.numeric) # as numeric
temp <- lapply(temp, function(x) {
  if (length(x) > 1) {
    (x[1] + x[2])/2
  } else {
    x[1]
  }
})                                                  # take midpoint of two numbers
t$`Death toll` <- unlist(temp)                      # put back
```

Below we have a bar plot of death toll over the years. The different
colors represent different types of natural disasters. The outstanding
feature of this plot is the 1931 China floods that cause over 2 million
deaths. And since this number is overwhelmingly high, the other bars are
compressed. So in the next plot, we are plotting the same graph without
the 1931 disaster to get a picture of other years’ situations

``` r
ggplot(data=t, aes(x=Year, y=`Death toll`, fill=Type)) + 
  geom_bar(stat="identity") +
  ggtitle("Death Toll of Natural Disasters by Year")  + 
  theme(text = element_text(size = 20)) 
```

![](Figs/unnamed-chunk-5-1.png?raw=true)<!-- -->

In the following plot, we can see the top death tolls are 1976 Tangshan
earthquake with over 400,000 deaths, 1970 Bhola cyclone with 300,000
deaths, 1920 Haiyuan earthquakes with over 200,000 deaths, 1975 Typhoon
Nina with over 200,000 deaths.

The graphs give us a clear image that earthquake causes much higher
damage than other disasters, followed by cyclone/typhoon in general.
They both happen the most frequently, and cases higher death tolls.
Removing the 1931 disasters, the next most severe disasters are floods
and heat waves, shown in green color in the plot. Volcanic eruption,
avalanche, landslide, and limnic eruption are less frequent and less
severe comparatively.

``` r
t[t$`Death toll` < 2000000,] %>% group_by(Type) %>% 
  summarise(count = n(), death = sum(`Death toll`)) %>%
  arrange(desc(count))
```

    ## # A tibble: 10 × 3
    ##    Type                    count   death
    ##    <chr>                   <int>   <dbl>
    ##  1 Earthquake                 58 1808636
    ##  2 Tropical cyclone           37 1341296
    ##  3 Flood                      12  160316
    ##  4 Heat wave                   8  158165
    ##  5 Earthquake, Tsunami         4  254487
    ##  6 Volcanic eruption           2   52000
    ##  7 Avalanche                   1    6000
    ##  8 Landslide                   1    2700
    ##  9 Limnic eruption             1    1746
    ## 10 Tropical cyclone, Flood     1    3123

``` r
ggplot(data=t[t$`Death toll` < 2000000,], aes(x=Year, y=`Death toll`, fill=Type)) + 
  geom_bar(stat="identity")  +
  ggtitle("Death Toll of Natural Disasters by Year (1931 Removed)")  + 
  theme(text = element_text(size = 20)) 
```

![](Figs/unnamed-chunk-7-1.png)<!-- -->

## Problem 2

``` r
gradient_descent <- function(x, y, 
                             e = 0.001,        # learning rates
                             b_cur = 0.1,      # starting estimate of b
                             threshold = 1e-5, # stopping threshold
                             steps = 100000    # maximum number of steps in case of infinite loop
) {
  i <- 1
  for (i in seq_len(steps)) {
    y_pred <- b_cur * x                         # predicted y values
    l_derivative <- -2 * sum(x * (y-y_pred))    # derivative of loss function
    b_prev <- b_cur                             # saving the value of current b value
    b_cur <- b_cur - e * l_derivative           # update estimate of b with gradient descnet
    if (b_cur - b_prev < threshold) {           # test if threshold has been met
      break
    }
  }
  c(b_cur, i)                                   # output
}
```

Here we are testing our gradient descent function with a random generated vector of 10 numbers, and set gradient to be 3

``` r
# test
x <- runif(n=10, min=1, max=20)
y <- x * 3
sum(x * y) / sum(x^2) # the solution
```

    ## [1] 3

``` r
e_vec <- c(seq(from = 0.1, by = -0.02, length.out = 5),
           seq(from = 0.01, by = -0.002, length.out = 5),
           seq(from = 0.001, by = -0.0002, length.out = 5),
           seq(from = 0.0001, by = -0.00002, length.out = 5),
           seq(from = 0.00001, by = -0.000002, length.out = 5))   # learning rates

b <- c()
steps <- c()

for (i in e_vec) {
  b <- c(b, gradient_descent(x, y, e = i)[1])
  steps <- c(steps, gradient_descent(x, y, e = i)[2])
}

rbind(e_vec, b, steps)
```

    ##            [,1]       [,2]      [,3]      [,4]      [,5]      [,6]      [,7]
    ## e_vec       0.1       0.08      0.06      0.04      0.02     0.010     0.008
    ## b     -260367.3 -166356.01 -93313.66 -41240.19 -10135.60 -2446.641 -1537.915
    ## steps       2.0       2.00      2.00      2.00      2.00     2.000     2.000
    ##            [,8]      [,9]     [,10]     [,11]     [,12]  [,13]    [,14]
    ## e_vec    0.0060    0.0040   0.00200  0.001000  0.000800 0.0006 0.000400
    ## b     -838.8778 -349.5297 -69.87043 -8.674109 -2.725511 1.1262 2.881021
    ## steps    2.0000    2.0000   2.00000  2.000000  2.000000 2.0000 2.000000
    ##           [,15]     [,16]     [,17]     [,18]    [,19]      [,20]      [,21]
    ## e_vec  0.000200  0.000100  0.000080  0.000060  0.00004   0.000020   0.000010
    ## b      2.999997  2.999978  2.999972  2.999958  2.99993   2.999849   2.999685
    ## steps 15.000000 33.000000 42.000000 56.000000 83.00000 159.000000 299.000000
    ##            [,22]      [,23]      [,24]       [,25]
    ## e_vec   0.000008   0.000006   0.000004    0.000002
    ## b       2.999599   2.999462   2.999186    2.998351
    ## steps 365.000000 472.000000 676.000000 1239.000000

Here we are testing our gradient descent algorithm on randomly generated
vectors with a known value of b = 3. We are able to see that as the
learning rate e lowers, the estimates of b remains stable for a while
and then maybe drops a little, while the number of steps taken to reach
that final estimates also remains at hundreds at first, and then
increases exponentially. In a nutshell, the performance of the algorithm
does not necessarily improves with a smaller step size/lower learning
rate. In fact, a small step size will cause the number of steps taken to
increase quite a lot, becoming computationally expensive.

The algorithms clearly fails at high learning rate till around 0.001. In
these cases, the gradient descent overshoots and misses the real
solution. With a big step size, every time we update the estimates for
b, we change by a lot. But again, whether or not the algorithm
overshoots also depends on the starting point we chooses. Here we did
not test multiple starting points.

``` r
par(mar=c(5, 4, 4, 8) + 0.3, xpd=TRUE)

plot(b,  axes=FALSE,  xlab="", ylab="", type="o", pch=20)
axis(2, ylim=c(2,3))
mtext("Estimates for b",side=2,line=2)
box()

par(new=TRUE)
plot(steps,  xlab="", ylab="",  
    axes=FALSE, type="o", pch=18, col="red")
axis(4, ylim=c(0,20000),col.axis="red")
mtext("Steps", side=4, line=2, col="red")

mtext("Learning Rate (e)",side=1)

legend("topright", inset = c(-0.3, 0), legend=c("b","Steps"),
  text.col=c("black","red"),pch=c(20,18),col=c("black","red"))

title("Estimates of b and Number of Steps Taken")
```

![](Figs/unnamed-chunk-11-1.png)<!-- -->
