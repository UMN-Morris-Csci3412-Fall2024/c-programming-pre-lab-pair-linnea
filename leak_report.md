# Leak report

in clean_whitespace.c there was only one memory leak that was fixible (at least in my knowledge) in the function itself, and that was in the is_clean function, where a pointer is allocated to the result of calling strip on the string passed into the function at it is called cleaned. to fix the memory error:
  if (cleaned[0] != '\0') { //way to check if string is not empty
  free((void *)cleaned);
  }
  where cleaned is the pointer to free, and we check if it the string is not empty first, and if its not we free it- doing it without checking if the string is empty will cause an error, as will doing it without casting to void *.

  the other thing that can cause memory leaks in clean_whitespace.c is that the memory from result is never cleared in the function strip, and cannot be cleared in that function as we need to return it. the way to fix this like I did multiple times in the test file is to: assign a char* of variable length to the result of string, then use it as part of the test to assert something, then free it. for example:
  

    char* result = strip("frog  ");
    ASSERT_STREQ("frog", result);
    free(result);


