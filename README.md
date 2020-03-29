<!-- README.md is generated from README.Rmd. Please edit that file -->

interactionR
============

<!-- badges: start -->
<!-- badges: end -->

Produces a publication-ready table that includes all effect estimates
neccessary for full reporting effect modification or interaction
analysis. It also estimates confidence interval for the trio of additive
interaction measures using the delta method, variance recovery method,
or bootstrapping.

Installation
------------

You can install the development version from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("epi-zen/interactionR")
```

Example
-------

This is an example showing how to use the main function of the package:

``` r
library(interactionR)
data (OCdata) ## Case-control data from Rothman and Keller (1972) evaluating the joint effect of alcohol and smoking on oral cancer risk is included in the package (cited in Hosmer and Lemeshow (1992) and Zou (2008))

## fit the interaction model
model.glm <- glm(oc ~ alc*smk, family = binomial(link = "logit"), data = OCdata)

## Then pass the fitted model to the function which saves a microsoft word publication-ready table to the user's working directory and prints this to the console, if the object is not stored
interactionR(model.glm, exposure_names = c("alc", "smk"), ci.type = "delta", ci.level = 0.95, em = F, recode = F)
#>                          Interaction of alc and smk                          
#>                      smk absent         smk present        Effect of smk     
#>                                                            within the        
#>                                                            strata of alc     
#>                      OR [95% CI]        OR [95% CI]        OR [95% CI]       
#>   alc absent         1 [Reference]      2.96 [0.68,        2.96 [0.68,       
#>                                         12.91]             12.91]            
#>   alc present        3.33 [0.7,         9.04 [2.64,        2.71 [1, 7.37]    
#>                      15.86]             30.91]                               
#>   Effect of alc      3.33 [0.7,         3.05 [1.29,                          
#>   within the         15.86]             7.18]                                
#>   strata of smk                                                              
#>   Multiplicative     0.91 [0.15,                                             
#>   scale              5.42]                                                   
#>   RERI               3.74 [-1.84,                                            
#>                      9.32]                                                   
#>   AP                 0.41 [-0.07,                                            
#>                      0.9]                                                    
#>   SI                 1.87 [0.65,                                             
#>                      5.42]                                                   
#> 
#> Column names: c1, c2, c3, c4
#> [1] "The file 'interaction_table.docx' has been saved to C:/Users/tunsm/Documents/interactionR"
```

If we want, we could use the variance recovery method to estimate the CI
for additive interaction measures by specifying “mover” for the
‘ci.type’ argument

``` r
interactionR(model.glm, exposure_names = c("alc", "smk"), ci.type = "mover", ci.level = 0.95, em = F, recode = F)
#>                          Interaction of alc and smk                          
#>                      smk absent         smk present        Effect of smk     
#>                                                            within the        
#>                                                            strata of alc     
#>                      OR [95% CI]        OR [95% CI]        OR [95% CI]       
#>   alc absent         1 [Reference]      2.96 [0.68,        2.96 [0.68,       
#>                                         12.91]             12.91]            
#>   alc present        3.33 [0.7,         9.04 [2.64,        2.71 [1, 7.37]    
#>                      15.86]             30.91]                               
#>   Effect of alc      3.33 [0.7,         3.05 [1.29,                          
#>   within the         15.86]             7.18]                                
#>   strata of smk                                                              
#>   Multiplicative     0.91 [0.15,                                             
#>   scale              5.42]                                                   
#>   RERI               3.74 [-11.43,                                           
#>                      21.87]                                                  
#>   AP                 0.41 [-0.38,                                            
#>                      0.81]                                                   
#>   SI                 1.87 [0.65,                                             
#>                      5.42]                                                   
#> 
#> Column names: c1, c2, c3, c4
#> [1] "The file 'interaction_table.docx' has been saved to C:/Users/tunsm/Documents/interactionR"
```
