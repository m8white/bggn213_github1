# Class 7: Machine Learning 1
Matthew White

Before we get into clustering methods lets make up some sample data to
cluster in which we know what the answer should be.

To help with this, use the `rnorm()` function.

``` r
#combine plots of two different random sets of values clustered around a set mean by vectorizing the rnorm values within hist() plot
hist(c(rnorm(150000, mean=-3), rnorm(150000, 3)))
```

![](Class07machineLearning_files/figure-commonmark/unnamed-chunk-1-1.png)

``` r
n=30
x <- c(rnorm(n, mean=-3), rnorm(n, 3))
x
```

     [1] -4.272552 -2.376709 -2.436617 -3.326155 -2.774315 -4.128438 -1.192524
     [8] -3.356821 -3.461308 -3.651514 -4.364045 -3.932306 -2.280858 -2.887234
    [15] -3.165141 -2.250565 -2.521038 -5.040697 -4.222225 -2.868755 -3.128448
    [22] -2.632391 -2.135772 -3.949366 -2.579522 -4.035208 -3.087411 -1.849666
    [29] -1.570262 -3.875902  3.821225  2.284707  2.631190  2.088303  1.798968
    [36]  4.086688  3.703387  3.472859  4.375783  4.551539  2.043071  3.730105
    [43]  3.252310  2.530193  3.315980  2.454060  4.136731  1.356069  3.510525
    [50]  3.999453  2.652862  3.062300  3.321544  2.836824  3.070923  3.765385
    [57]  2.087818  2.545418  3.930937  2.929735

``` r
#to build up our second sample data cluster, could either rewrite the above code but flipped, or we can just reverse "x" that we already built
y <- rev(x)
y
```

     [1]  2.929735  3.930937  2.545418  2.087818  3.765385  3.070923  2.836824
     [8]  3.321544  3.062300  2.652862  3.999453  3.510525  1.356069  4.136731
    [15]  2.454060  3.315980  2.530193  3.252310  3.730105  2.043071  4.551539
    [22]  4.375783  3.472859  3.703387  4.086688  1.798968  2.088303  2.631190
    [29]  2.284707  3.821225 -3.875902 -1.570262 -1.849666 -3.087411 -4.035208
    [36] -2.579522 -3.949366 -2.135772 -2.632391 -3.128448 -2.868755 -4.222225
    [43] -5.040697 -2.521038 -2.250565 -3.165141 -2.887234 -2.280858 -3.932306
    [50] -4.364045 -3.651514 -3.461308 -3.356821 -1.192524 -4.128438 -2.774315
    [57] -3.326155 -2.436617 -2.376709 -4.272552

``` r
#cbind will combine columns of two different vectors/matrices/data frames. basically making the x and y data values into one singular dataset. 
z <- cbind(x,y)
z
```

                  x         y
     [1,] -4.272552  2.929735
     [2,] -2.376709  3.930937
     [3,] -2.436617  2.545418
     [4,] -3.326155  2.087818
     [5,] -2.774315  3.765385
     [6,] -4.128438  3.070923
     [7,] -1.192524  2.836824
     [8,] -3.356821  3.321544
     [9,] -3.461308  3.062300
    [10,] -3.651514  2.652862
    [11,] -4.364045  3.999453
    [12,] -3.932306  3.510525
    [13,] -2.280858  1.356069
    [14,] -2.887234  4.136731
    [15,] -3.165141  2.454060
    [16,] -2.250565  3.315980
    [17,] -2.521038  2.530193
    [18,] -5.040697  3.252310
    [19,] -4.222225  3.730105
    [20,] -2.868755  2.043071
    [21,] -3.128448  4.551539
    [22,] -2.632391  4.375783
    [23,] -2.135772  3.472859
    [24,] -3.949366  3.703387
    [25,] -2.579522  4.086688
    [26,] -4.035208  1.798968
    [27,] -3.087411  2.088303
    [28,] -1.849666  2.631190
    [29,] -1.570262  2.284707
    [30,] -3.875902  3.821225
    [31,]  3.821225 -3.875902
    [32,]  2.284707 -1.570262
    [33,]  2.631190 -1.849666
    [34,]  2.088303 -3.087411
    [35,]  1.798968 -4.035208
    [36,]  4.086688 -2.579522
    [37,]  3.703387 -3.949366
    [38,]  3.472859 -2.135772
    [39,]  4.375783 -2.632391
    [40,]  4.551539 -3.128448
    [41,]  2.043071 -2.868755
    [42,]  3.730105 -4.222225
    [43,]  3.252310 -5.040697
    [44,]  2.530193 -2.521038
    [45,]  3.315980 -2.250565
    [46,]  2.454060 -3.165141
    [47,]  4.136731 -2.887234
    [48,]  1.356069 -2.280858
    [49,]  3.510525 -3.932306
    [50,]  3.999453 -4.364045
    [51,]  2.652862 -3.651514
    [52,]  3.062300 -3.461308
    [53,]  3.321544 -3.356821
    [54,]  2.836824 -1.192524
    [55,]  3.070923 -4.128438
    [56,]  3.765385 -2.774315
    [57,]  2.087818 -3.326155
    [58,]  2.545418 -2.436617
    [59,]  3.930937 -2.376709
    [60,]  2.929735 -4.272552

``` r
plot(z)
```

![](Class07machineLearning_files/figure-commonmark/unnamed-chunk-2-1.png)

\##K-means clustering

The function in base R for k-means clustering is called `kmeans()`

``` r
km <- kmeans(z, centers = 2)
km
```

    K-means clustering with 2 clusters of sizes 30, 30

    Cluster means:
              x         y
    1 -3.111792  3.111563
    2  3.111563 -3.111792

    Clustering vector:
     [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2
    [39] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2

    Within cluster sum of squares by cluster:
    [1] 43.91913 43.91913
     (between_SS / total_SS =  93.0 %)

    Available components:

    [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    [6] "betweenss"    "size"         "iter"         "ifault"      

``` r
km$centers
```

              x         y
    1 -3.111792  3.111563
    2  3.111563 -3.111792

> Q: Print out the cluster membership vector (i.e. our main answer)

``` r
km$cluster
```

     [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2
    [39] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2

Plot of data with the clustering result

``` r
plot(z, col = km$cluster)
points(km$centers, col = "blue", pch = 15, cex = 2)
```

![](Class07machineLearning_files/figure-commonmark/unnamed-chunk-6-1.png)

> Can you cluster our data in `z` into four clusters?

``` r
km4 <- kmeans(z, 4)
km4
```

    K-means clustering with 4 clusters of sizes 13, 17, 13, 17

    Cluster means:
              x         y
    1  2.418882 -2.516921
    2 -3.566694  3.641261
    3 -2.516921  2.418882
    4  3.641261 -3.566694

    Clustering vector:
     [1] 2 2 3 3 2 2 3 2 2 2 2 2 3 2 3 3 3 2 2 3 2 2 3 2 2 3 3 3 3 2 4 1 1 1 1 4 4 1
    [39] 4 4 1 4 4 1 1 1 4 1 4 4 4 4 4 1 4 4 1 1 4 4

    Within cluster sum of squares by cluster:
    [1] 11.26035 13.53319 11.26035 13.53319
     (between_SS / total_SS =  96.0 %)

    Available components:

    [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    [6] "betweenss"    "size"         "iter"         "ifault"      

``` r
plot(z, col = km4$cluster)
points(km4$centers, col = "blue", pch = 15, cex = 2)
```

![](Class07machineLearning_files/figure-commonmark/unnamed-chunk-7-1.png)

``` r
##must be careful with kmeans clustering, as it will return what you ask for. You can force data with clearly 2 clusters into trying to show 4 distinct clusters. 
```

## Hierarchical Clustering

The main function for hierarchical clustering in base R is call
`hclust()`

Unlike `kmeans()` I can not just pass in my data as input. I first need
a distance matrix from my data.

``` r
d <- dist(z)
hc <- hclust(d)
hc
```


    Call:
    hclust(d = d)

    Cluster method   : complete 
    Distance         : euclidean 
    Number of objects: 60 

There is a specific hclust plot() method

``` r
plot(hc)
#To get main clustering result (membership vector), give a height at which to cut the tree and what falls together is a cluster.  To do so we can use `cutree()` 
abline(h=10, col = "red")
```

![](Class07machineLearning_files/figure-commonmark/unnamed-chunk-9-1.png)

``` r
grps <- cutree(hc, h =10)
grps
```

     [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2
    [39] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2

``` r
plot(z, col = grps)
```

![](Class07machineLearning_files/figure-commonmark/unnamed-chunk-10-1.png)

\#Principal Component Analysis

\##PCA of UK food data

``` r
url <- "https://tinyurl.com/UK-foods"
#reading dataset with `row.names = 1` argument to not conisder row names as a column value
x <- read.csv(url, row.names = 1)
dim(x)
```

    [1] 17  4

``` r
head(x)
```

                   England Wales Scotland N.Ireland
    Cheese             105   103      103        66
    Carcass_meat       245   227      242       267
    Other_meat         685   803      750       586
    Fish               147   160      122        93
    Fats_and_oils      193   235      184       209
    Sugars             156   175      147       139

``` r
tail(x)
```

                      England Wales Scotland N.Ireland
    Fresh_fruit          1102  1137      957       674
    Cereals              1472  1582     1462      1494
    Beverages              57    73       53        47
    Soft_drinks          1374  1256     1572      1506
    Alcoholic_drinks      375   475      458       135
    Confectionery          54    64       62        41

Now some plots of our data (not very useful ones for visualization)

``` r
barplot(as.matrix(x), beside = T, col = rainbow(nrow(x)))
```

![](Class07machineLearning_files/figure-commonmark/unnamed-chunk-12-1.png)

``` r
#beside argument can change to stacked bars. default is False
barplot(as.matrix(x), beside = F, col = rainbow(nrow(x)))
```

![](Class07machineLearning_files/figure-commonmark/unnamed-chunk-12-2.png)

\##PCA to the rescue

The main function to do PCA in base R is called `prcomp()`.

Note that I need to take the transpose of this particular data because
that is what `prcomp()` help page was asking for

``` r
x
```

                        England Wales Scotland N.Ireland
    Cheese                  105   103      103        66
    Carcass_meat            245   227      242       267
    Other_meat              685   803      750       586
    Fish                    147   160      122        93
    Fats_and_oils           193   235      184       209
    Sugars                  156   175      147       139
    Fresh_potatoes          720   874      566      1033
    Fresh_Veg               253   265      171       143
    Other_Veg               488   570      418       355
    Processed_potatoes      198   203      220       187
    Processed_Veg           360   365      337       334
    Fresh_fruit            1102  1137      957       674
    Cereals                1472  1582     1462      1494
    Beverages                57    73       53        47
    Soft_drinks            1374  1256     1572      1506
    Alcoholic_drinks        375   475      458       135
    Confectionery            54    64       62        41

``` r
#get transposed version of x data frame with t() function
t(x)
```

              Cheese Carcass_meat  Other_meat  Fish Fats_and_oils  Sugars
    England      105           245         685  147            193    156
    Wales        103           227         803  160            235    175
    Scotland     103           242         750  122            184    147
    N.Ireland     66           267         586   93            209    139
              Fresh_potatoes  Fresh_Veg  Other_Veg  Processed_potatoes 
    England               720        253        488                 198
    Wales                 874        265        570                 203
    Scotland              566        171        418                 220
    N.Ireland            1033        143        355                 187
              Processed_Veg  Fresh_fruit  Cereals  Beverages Soft_drinks 
    England              360         1102     1472        57         1374
    Wales                365         1137     1582        73         1256
    Scotland             337          957     1462        53         1572
    N.Ireland            334          674     1494        47         1506
              Alcoholic_drinks  Confectionery 
    England                 375             54
    Wales                   475             64
    Scotland                458             62
    N.Ireland               135             41

``` r
pca <- prcomp(t(x))
summary(pca)
```

    Importance of components:
                                PC1      PC2      PC3       PC4
    Standard deviation     324.1502 212.7478 73.87622 2.921e-14
    Proportion of Variance   0.6744   0.2905  0.03503 0.000e+00
    Cumulative Proportion    0.6744   0.9650  1.00000 1.000e+00

Lets see what is inside our result object `pca` that we just calculated

``` r
attributes(pca)
```

    $names
    [1] "sdev"     "rotation" "center"   "scale"    "x"       

    $class
    [1] "prcomp"

``` r
pca$x
```

                     PC1         PC2        PC3           PC4
    England   -144.99315   -2.532999 105.768945 -9.152022e-15
    Wales     -240.52915 -224.646925 -56.475555  5.560040e-13
    Scotland   -91.86934  286.081786 -44.415495 -6.638419e-13
    N.Ireland  477.39164  -58.901862  -4.877895  1.329771e-13

To make our main result figure, called a “PC plot” or “score plot,
or”ordination plot” or “PC1 vs PC2 plot”.

``` r
plot(pca$x[,1], pca$x[,2], col=c("black", "red", "blue", "darkgreen"), 
     pch=16,
     xlab = "PC1 (67.4%)", ylab = "PC2 (29%)"
     )
```

![](Class07machineLearning_files/figure-commonmark/unnamed-chunk-16-1.png)

``` r
#now understand why had to transpose the dataset. we can see a point for each of the countries since they are rows now rather than columns.
```

\##Variable Loadings plot

Can give us insight into how the original variables, in this case the
foods, contribute to our new PC axis

``` r
par(mar=c(10, 3, 0.35, 0))
barplot( pca$rotation[,1], las=2 )
```

![](Class07machineLearning_files/figure-commonmark/unnamed-chunk-17-1.png)

``` r
pca$rotation
```

                                 PC1          PC2         PC3          PC4
    Cheese              -0.056955380  0.016012850  0.02394295 -0.409382587
    Carcass_meat         0.047927628  0.013915823  0.06367111  0.729481922
    Other_meat          -0.258916658 -0.015331138 -0.55384854  0.331001134
    Fish                -0.084414983 -0.050754947  0.03906481  0.022375878
    Fats_and_oils       -0.005193623 -0.095388656 -0.12522257  0.034512161
    Sugars              -0.037620983 -0.043021699 -0.03605745  0.024943337
    Fresh_potatoes       0.401402060 -0.715017078 -0.20668248  0.021396007
    Fresh_Veg           -0.151849942 -0.144900268  0.21382237  0.001606882
    Other_Veg           -0.243593729 -0.225450923 -0.05332841  0.031153231
    Processed_potatoes  -0.026886233  0.042850761 -0.07364902 -0.017379680
    Processed_Veg       -0.036488269 -0.045451802  0.05289191  0.021250980
    Fresh_fruit         -0.632640898 -0.177740743  0.40012865  0.227657348
    Cereals             -0.047702858 -0.212599678 -0.35884921  0.100043319
    Beverages           -0.026187756 -0.030560542 -0.04135860 -0.018382072
    Soft_drinks          0.232244140  0.555124311 -0.16942648  0.222319484
    Alcoholic_drinks    -0.463968168  0.113536523 -0.49858320 -0.273126013
    Confectionery       -0.029650201  0.005949921 -0.05232164  0.001890737
