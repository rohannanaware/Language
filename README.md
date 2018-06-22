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
