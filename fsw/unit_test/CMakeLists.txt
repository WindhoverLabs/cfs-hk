include(CTest)
find_program( MEMORYCHECK_COMMAND valgrind )
find_package(Threads)
set( MEMORYCHECK_COMMAND_OPTIONS "--trace-children=yes --leak-check=full" )

# Set include directories
include_directories(
	cfe_hdr_files
	../mission_inc
	../platform_inc
	$ENV{CFE_DIR}/fsw/mission_inc
	$ENV{CFE_DIR}/fsw/platform_inc/cpu1
	${CMAKE_CURRENT_SOURCE_DIR}
	${CFE_INC_DIRS}
	${OSAL_INC_DIRS}
	${UTF_INCS}
	${APP_INC_DIRS})

add_definitions(-DCFE_LINUX)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -O0 -Wformat=0 -Wno-pointer-to-int-cast -Wno-int-to-pointer-cast -fprofile-arcs -ftest-coverage")

# Add the executables
add_executable(HK-UT 
		${CMAKE_CURRENT_SOURCE_DIR}/utf_test_hk.c
		${APP_SRC}
		${UTF_SRC})
target_link_libraries(HK-UT -fprofile-arcs gcov ${CMAKE_THREAD_LIBS_INIT})

# We need to both run the test and compare the as-run with the expected output
# with a diff command. 
add_test(NAME hk-ut
    COMMAND ${CMAKE_COMMAND}
    	-DTEST_PROG=$<TARGET_FILE:HK-UT>
    	-DEXPECTED_OUTPUT=${CMAKE_CURRENT_SOURCE_DIR}/sbsim.out
    	-DACTUAL_OUTPUT=${CMAKE_CURRENT_BINARY_DIR}/sbsim.out
    -P ${CMAKE_CURRENT_SOURCE_DIR}/runtest.cmake)

# Copy input files to build directory
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/sbsim.in ${CMAKE_CURRENT_BINARY_DIR} COPYONLY)
    
# Build tables for the tests
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/cf/apps)
SET(CMAKE_CXX_ARCHIVE_CREATE "<CMAKE_AR> <OBJECTS> ${CMAKE_CURRENT_BINARY_DIR}/cf/apps")
SET(CMAKE_C_ARCHIVE_CREATE "<CMAKE_AR> <OBJECTS> ${CMAKE_CURRENT_BINARY_DIR}/cf/apps")
add_library(hk_cpy_tbl STATIC ../tables/hk_cpy_tbl.c)
set_target_properties(hk_cpy_tbl PROPERTIES PREFIX "" SUFFIX ".tbl")
add_custom_command(
	TARGET hk_cpy_tbl 
    POST_BUILD
    COMMAND ${CMAKE_COMMAND} copy -E ${CMAKE_CURRENT_BINARY_DIR}/cf/apps/hk_cpy_tbl.tbl ${CMAKE_CURRENT_BINARY_DIR}/cf/apps/hk_cpy_tbl2.tbl)
            
