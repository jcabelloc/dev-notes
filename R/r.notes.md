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

### Installing and refering a library
```R
install.packages("e1071", "C:/Users/jcabelloc/Anaconda3/Lib/R/library")
library("e1071")
```

### Common utils
```R
# Repetition of 392 0-values
y = rep(0, 392)
# Length of an array or list
length(Auto$mpg)
# Bind two columns
cbind(y, Auto$mpg)
# Obtain dimensions
dim(train_df)
# Print concatenation
cat('test_error_rate: ', test_error_rate)
# Create a vector
costs = c(0.001, 0.01, 0.1, 1, 10, 100)
# Make a predictor a category
y = rep(0, length(Auto$mpg))
y[Auto$mpg> med_mpg] = 1
y = as.factor(y)
# Split dataset into train and test
n = length(AnyDataSet$y)
train = sample(n, n*0.6)
train_df = AnyDataSet[train,]
test_df = AnyDataSet[-train,]
```
