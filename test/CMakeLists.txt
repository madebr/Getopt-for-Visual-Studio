# Test simple usage of getopt
add_executable(getopt_simple getopt_simple.c)
target_link_libraries(getopt_simple getopt::getopt)

add_test(NAME getopt_simple_no_arg COMMAND getopt_simple)
set_tests_properties(getopt_simple_no_arg PROPERTIES WILL_FAIL ON)

add_test(NAME getopt_simple_missing_arg COMMAND getopt_simple -t 1337)
set_tests_properties(getopt_simple_missing_arg PROPERTIES WILL_FAIL ON)

add_test(NAME getopt_simple_all_args COMMAND getopt_simple -t 1337 -n 4)

add_test(NAME getopt_simple_unknown_arg COMMAND getopt_simple -u 666)
set_tests_properties(getopt_simple_unknown_arg PROPERTIES WILL_FAIL ON)

# FIXME: add tests for getopt_long and getopt_long_only
