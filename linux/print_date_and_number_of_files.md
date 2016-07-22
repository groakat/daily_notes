# Printing Timestamp together with number of files in a folder

If you have a process that produces files, it can be useful to print out the number of files already produced to monitor the process. This can is particularly useful, when dealing with a map-reduce type of process where it is hard to keep track of the overall processing speed of the complete system of clusters.

     { ls -1 | wc -l & date; } | cat

Prints the number of files in a directory together with a timestamp.
