Need based links

- [data.table merge by multiple columns](https://stackoverflow.com/questions/30370879/data-table-merge-by-multiple-columns)
- [Order by using dplyr](https://stackoverflow.com/questions/47144143/dplyr-arrange-by-reverse-alphabetical-order)
- [Package installation error - unable to move temporary installation](https://stackoverflow.com/questions/5700505/windows-7-update-packages-problem-unable-to-move-temporary-installation)
- [Edit labels in ggplot2 boxplot](https://stackoverflow.com/questions/1330989/rotating-and-spacing-axis-labels-in-ggplot2)
- [Cut column by quantiles | create partition](https://stackoverflow.com/questions/4126326/how-to-quickly-form-groups-quartiles-deciles-etc-by-ordering-columns-in-a)
# R reference codes
- Regular expressions

## Regular expressions

- Creating new column with regexpr

```R

# Apply regexpr on a string - Gives back the position of 1st occurence of a pattern in the string, in case the pattern is not found returns -1
regexpr(" ", "The Quick Brown Box")
# [1] 4
# attr(,"match.length")
# [1] 1
# attr(,"useBytes")
# [1] TRUE

regexpr(" ", "TheQuickBrownBox")
# [1] -1
# attr(,"match.length")
# [1] -1
# attr(,"useBytes")
# [1] TRUE

# To get all positions where the pattern is found
gregexpr(" ", "The Quick Brown Box")
# [[1]]
# [1]  4 10 16
# attr(,"match.length")
# [1] 1 1 1
# attr(,"useBytes")
# [1] TRUE

# creating new column - notice no indexing is done on top of regexpr formula
df$new_col <- substr(df$col,
                     <start_num>,
                     regexpr(<pattern>, df$col))
                     

```

# Plotting in R

- ggplot2 [cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf)

# Apply family

```R
#Author : Rohan M. Nanaware 
#Date C.: 07th Aug 2017
#Date M.: 07th Aug 2017
#Purpose: Understand the apply family, syntax, when to use, etc.
#Ref.   : https://www.datacamp.com/community/tutorials/r-tutorial-apply-family#family

{
  #Syntax
  apply(X, MARGIN, FUN, ...)
  # where: X is an array or a matrix if the dimension of the array is 2;
  #        MARGIN is a variable defining how the function is applied: when MARGIN=1, it applies over rows, 
  #               whereas with MARGIN=2, it works over columns. 
  #               Note that when you use the construct MARGIN=c(1,2), it applies to both rows and columns;
  #        FUN, which is the function that you want to apply to the data. It can be any R function, including a User Defined Function (UDF).
  # Construct a 5x6 matrix
  X <- matrix(rnorm(30), nrow=5, ncol=6)
  # Sum the values of each column with `apply()`
  apply(X, 2, sum)
  
  {
    # > X <- matrix(rnorm(30), nrow=5, ncol=6)
    # > X
    # [,1]       [,2]       [,3]        [,4]       [,5]       [,6]
    # [1,] -1.94906932 -1.3878370 -0.1958298 -0.53938847 -0.2420091  1.8336326
   # [2,] -0.06625763 -0.3724634 -0.2571309 -0.01875991 -0.7428842 -0.4180169
    # [3,]  0.44975736  0.2005075 -2.3643830 -0.58094495 -1.7548130 -1.0533094
    # [4,] -0.75873543 -0.9859626  0.4494635 -0.71736076 -1.1165637 -0.6567909
    # [5,] -1.13292328  1.3785268 -0.1441591 -0.25306394 -1.4937538 -0.3722877
    # > apply(X, 2, sum)
    # [1] -3.4572283 -1.1672288 -2.5120393 -2.1095180 -5.3500238 -0.6667723
  }#res
  {
    X = iris
    head(X)
    Y = apply(X, 2, unique)
    Y = apply(X, 2, function(x) length(unique(x)))
  }#test
  
}#01. apply()
{
  # You want to apply a given function to every element of a list and obtain a list as result. 
  #        When you execute ?lapply, you see that the syntax looks like the apply() function.
  # The difference is that:  #   
  #        It can be used for other objects like dataframes, lists or vectors; and
  #        The output returned is a list (which explains the “l” in the function name), 
  #        which has the same number of elements as the object passed to it
  # Create a list of matrices
  A = iris
  B = iris
  C = iris
  MyList <- list(A,B,C)
  # Extract the 2nd column from `MyList` with the selection operator `[` with `lapply()`
  lapply(MyList,"[", , 2)
  # Extract the 1st row from `MyList`
  lapply(MyList,"[", 1, )
  
  {
    MyList <- list(X, iris, cars)
    #lapply(MyList, function(x) apply(x, 1, length))
    lapply(MyList, length)
    lapply(iris, length)
    lapply(iris[,1], length)
  }#test
  
}#02. lapply()
{
  # sapply() is a ‘wrapper’ function for lapply()
  # Applying the lapply() function would give us a list, unless you pass simplify=FALSE as parameter to sapply(). 
  # Then, a list will be returned
}#03. sapply()
{
  # Initialize `Z`
  Z <- sapply(MyList,"[", 1,1 )
  
  # Return `Z`
  Z
  
  # Replicate the values of `Z`
  Z <- rep(Z,c(3,1,2))
  
  # Return `Z`
  Z
  
  # You see that the code above replicates the values of Z a number of times as established by c(3,1,2): 
  #       three times the first, one time the second and two times the third
}#04. rep()
{
  # The sweep() function is probably the closest to the apply() family. 
  # You use it when you want to replicate different actions on the MARGIN elements that you have chosen 
  #     (limiting here to the matrix case).
  # A typical scenario occurs in clustering, where you may need to repetitively produce normalized and centered 
  #      or “standardized” data
  dataPoints <- matrix(rnorm(25), nrow=5, ncol=5)
  
  # Find means per column with `apply()`
  dataPoints_means <- apply(dataPoints, 2, mean)
  
  # Find standard deviation with `apply()`
  dataPoints_sdev <- apply(dataPoints, 2, sd)
  
  # Center the points 
  dataPoints_Trans1 <- sweep(dataPoints, 2, dataPoints_means,"-")
  print(dataPoints_Trans1)

  # Return the result
  dataPoints_Trans1
  
  # You produced the centered points with one call to sweep(). This function expects the following elements:
  #   an input array, which in this case is a matrix;
  #   a MARGIN, 2 to indicate the columns;
  #   a summary statistics (here mean); and
  #   a function to be applied. You use the arithmetic operator “-” for subtraction
  
  # Normalize
  dataPoints_Trans2 <- sweep(dataPoints_Trans1, 2, dataPoints_sdev, "/")
  
  # Return the result
  dataPoints_Trans2
  
  {
    sweep(dataPoints, 2, c(1,2,3,4,5), "*")
  }#test
}#05. sweep()
{
  aggregate()
  mapply()
}#Pending

```
