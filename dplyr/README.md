
[`dplyr`](https://github.com/hadley/dplyr)
==========================================

General
-------

-   Commas (`,`) are read as `&` in `filter()`
-   Pipe `%>%` to send result of one function to another one

Functions
---------

**Note**: The `dplyr` equivalents from the base package base are only for illustration. The functions in either package may behave differently.

### Create a `data.frame`

``` r
# dplyr
d <- data_frame(ID = 1:10)

# base
d <- data.frame(ID = 1:10)
```

See also `as_data_frame()` and `as.data.frame()`.

### `filter()`

Subset the data by rows given a condition.

``` r
# In dplyr
data %>% 
  filter(TIME > 0, MDV == 0)

# In base
data[data$TIME > 0 & data$MDV == 0,]
```

### `slice()`

Subset the data by rows given their position.

``` r
# In dplyr
data %>% 
  slice(1:100)

# In base
data[1:100,]
```

### `select()`

Subset the data by columns.

``` r
# In dplyr
data %>% 
  select(ID, TIME) # Select columns
data %>% 
  select(ID:MDV)   # Range of columns

# In base
data[,'ID']                           # Select columns
data[,which(colnames(data) == 'ID'):
      which(colnames(data) == 'MDV')] # Range of columns
```

### `group_by()`

Define groups on the data that will later be used by other functions.

``` r
# In dplyr
data %>% 
  group_by(ID,OCC) %>% 
  ...

# In base
by(data = data, INDICES = data[,c('ID','OCC')], FUN = ... )
## or
aggregate(~ID+OCC, data = data , FUN = ...)
```

**Important**: if not needed anymore remove groups.

``` r
# In dplyr
data %>% 
  group_by(ID) %>% 
  ... %>% 
  ungroup()
```

### `summarize()`

Summarise information on the dataset. Note the `n()` function is part of `dplyr` and allows to count the `data` as `length` would do in `base`.

``` r
# In dplyr
data %>% 
  group_by(ID) %>% 
  summarize(Cmax = max(DV),
            Nobs = n())

# In base
aggregate(DV~ID, data = data, FUN = max)    # For Cmax
aggregate(DV~ID, data = data, FUN = length) # For count
```

### `mutate()`

Add or edit columns. See also `transmute()` do drop original columns and `mutate_each()` to apply a function on all columns.

``` r
# In dplyr
data %>% 
  mutate(TIME_H = TIME_D * 24)

# In base
data$TIME_H <- TIME_D*24
```

### `rename()`

Rename column.

``` r
# In dplyr
data %>% 
  rename(TAD = TIME)

# In base
colnames(data)[colnames(data) == 'TIME'] <- 'TAD'
```

### `arrange()`

Order rows.

``` r
# In dplyr
data %>% 
  arrange(ID, TIME)

# In base
data[order(data$ID, data$TIME),]
```

Use `arrange(desc(VAR))` to sort in decreasing order.

### `join` functions

#### `full_join()`

Merge and keep all rows from both dataset.

``` r
# In dplyr
data %>% 
  full_join(data2)

# In base
merge(data , data2, all = TRUE)
```

#### `inner_join()`

Merge only rows that are matched in both datasets.

``` r
# In dplyr
data %>% 
  inner_join(data2)

# In base
merge(data, data2, all = FALSE)
```

#### `left_join()`

Merge and keep all rows from the left dataset (i.e. `x`). See also `right_join()`.

``` r
# In dplyr
data %>% 
  left_join(data2)

# In base
merge(data, data2, all.x = TRUE)
```

#### `semi_join()`

Subset x for all rows that match in y (no merge).

#### `anti_join()`

Subset x for all rows that do not match in y (no merge).

### Miscellaneous functions

``` r
between(x, -1, 1)         # Shortcut for x >= -1 & x <= 1
do()                      # Use an outside function with group_by()
n() / n_distinct()        # Counter
fallwith()                # Assign default return value if function crashes
top_n()                   # Select top n rows
first() / last()          # First / Last row or value
nth()                     # n th row or value
lead() / lag()            # Values shifted by 1 (i.e use value from previous or next row)
glimpse()                 # Get a glimpse at the data
group_indices()           # Generate a unique id for each group
group_size() / n_groups() # Calculate group size
row_number()              # Widowed rank functions
```

### Use `dplyr` in functions

``` r
# User side
group     <- c('VAR1', 'VAR')
col_name  <- 'DV'

# Function side
med <- dat %>% 
  group_by_(.dots = group) %>% 
  summarise_(MED = paste0('stats::median(', col_name, ')'))
```
