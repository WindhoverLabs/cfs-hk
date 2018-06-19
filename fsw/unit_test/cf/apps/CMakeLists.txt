# Build tables for the tests
SET(CMAKE_CXX_ARCHIVE_CREATE "<CMAKE_AR> <OBJECTS> ${CMAKE_CURRENT_BINARY_DIR}")
SET(CMAKE_C_ARCHIVE_CREATE "<CMAKE_AR> <OBJECTS> ${CMAKE_CURRENT_BINARY_DIR}")
#add_app(ut_hk_cf_cpy_tbl STATIC hk_cpy_tbl.c)
#add_app(ut_hk_cf_cpy_tbl2 STATIC hk_cpy_tbl2.c)
#set_target_properties(ut_hk_cf_cpy_tbl PROPERTIES PREFIX "" SUFFIX ".tbl")
#set_target_properties(ut_hk_cf_cpy_tbl2 PROPERTIES PREFIX "" SUFFIX ".tbl")
