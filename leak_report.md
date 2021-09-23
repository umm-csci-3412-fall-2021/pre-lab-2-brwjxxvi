# Leak report

After running valgrind --leak-check=full on the program check_whitespace.c I get the message that 46 bytes in 6 blocks are definitely lost. In the more detailed report it shows the memory is lost after calloc is called by the check_whitespace program in the function main which calls "is_clean" which then calls on the function "strip". The memory leak is caused in "strip" as it allocates memory for each saved character and one extra but then never deallocates this memory after the characters arent used anymore. The function "is_clean" calls on "strip" to create the variable "cleaned". Now, the saved character memory from "strip" is no longer needed so we can deallocate/free the memory at the end of "is_clean". One case we need to account for is if "strip" passes an empty string then there will be no allocated memory that needs to be deallocated. 

Solution: Add an if statement to see if the cleaned variable is an empty string or not and if not, free(cleaned).



