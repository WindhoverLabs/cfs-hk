# Build tables for the tests
SET(CMAKE_CXX_ARCHIVE_CREATE "<CMAKE_AR> <OBJECTS> ${CMAKE_CURRENT_BINARY_DIR}")
SET(CMAKE_C_ARCHIVE_CREATE "<CMAKE_AR> <OBJECTS> ${CMAKE_CURRENT_BINARY_DIR}")
add_library(hk_cpy_tbl STATIC hk_cpy_tbl.c)

set_target_properties(hk_cpy_tbl PROPERTIES PREFIX "" SUFFIX ".tbl")
