# Data process

## [How to share data with a statistician](https://github.com/jtleek/datasharing)

### What to share?

1. The raw data.
2. A [tidy data set](http://vita.had.co.nz/papers/tidy-data.pdf)
3. A code book describing each variable and its values in the tidy data set.
4. An explicit and exact recipe you used to go from 1 -> 2,3



**Content**:

- **Raw data**: As the input of data pipeline

- **The tidy data set**: how to construct data. Include some principles, include but not limit to:

  - Each variable you measure should be in one column
  - Each different observation of that variable should be in a different row
  - There should be one table for each "kind" of variable
  - If you have multiple tables, they should include a column in the table that allows them to be joined or merged
  - Contain full row names

- **The code book**: Works like a manual on how to manipulate data. Include

  - Information about the variables (including units!) in the data set not contained in the tidy data
  - Information about the summary choices you made
  - Information about the experimental study design you used

  Sections:

  - Study design: how you collected the data
  - Code book: each variable and its units



**How to code variables**:

1. Continuous(连续数值)
2. Ordinal(分节数值): eg poor, fair, good
3. Categorical(分类): male or female
4. Missing(缺失,且缺失原因不详): NA
5. Censored(缺失，但缺失原因可以推断)：此列保持NA，新增加列`VariableNameCensored` 在`tidy data` 中(`true`/ `false`). 此列存在的原因在于需要让data scientist 了解数据为什么丢掉了。



**The instruction list/script**

- 保证通过脚本`R,Python etc` 能够复现处理步骤
- 每一个row的前世今生可以追踪