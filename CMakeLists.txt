cmake_minimum_required(VERSION 3.24)

project(st_win32 C)

include(FetchContent)

FetchContent_Declare(
	fcontext
	GIT_REPOSITORY git@github.com:lichao2014/fcontext.git
)

FetchContent_MakeAvailable(fcontext)

set(CMAKE_C_STANDARD 99)
add_definitions(-DWIN32_LEAN_AND_MEAN)

set(ST_FILES event.c io.c key.c  sched.c sync.c stk.c st.h common.h md.h)

add_library(st STATIC ${ST_FILES})
target_compile_definitions(st PRIVATE _WIN32_WINNT=0x0600 FD_SETSIZE=4096 USE_POLL MALLOC_STACK)
target_link_libraries(st PUBLIC fcontext ws2_32 ntdll powrprof)

add_executable(test_thread test_thread.c)
target_link_libraries(test_thread st)

add_executable(test_socket test_socket.c)
target_link_libraries(test_socket st)