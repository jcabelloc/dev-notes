# Recurrent R notes

### Attaching and exploring datasets
``` R
# Attaching the dataset and then exploring the data
library(ISLR)
attach(Auto)
names(Auto)

head(Auto)
summary(Auto)

```

### Common utils
```R
# Repetition of 392 0-values
y = rep(0, 392)
# Length of an array or list
length(Auto$mpg)
# Bind two columns
cbind(y, Auto$mpg)
```