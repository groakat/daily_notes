# Select every X file in a folder

Use `ls | awk 'NR % X == 0'`, i.e. if `X` is 1000, use `ls | awk 'NR % 1000 == 0'`
