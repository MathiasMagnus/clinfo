cmake_minimum_required(VERSION 3.7) # OpenCL::OpenCL imported target syntax

project(clinfo VERSION 2.2.180406
               LANGUAGES C)

if (MSVC)
  string(REGEX REPLACE "/W[0-9]" "/W4" CMAKE_C_FLAGS ${CMAKE_C_FLAGS})	
endif (MSVC)

find_package(OpenCL REQUIRED)

set(Hdrs src/ctx_prop.h
         src/error.h
		 src/ext.h
		 src/fmtmacros.h
		 src/info_loc.h
		 src/info_ret.h
		 src/memory.h
		 src/ms_support.h
		 src/opt_out.h
		 src/strbuf.h)

set(Srcs src/clinfo.c)

add_executable(${PROJECT_NAME} ${Hdrs} ${Srcs})

target_include_directories(${PROJECT_NAME} PRIVATE src)

target_link_libraries(${PROJECT_NAME} PRIVATE OpenCL::OpenCL)

target_compile_options(${PROJECT_NAME} PRIVATE $<$<OR:$<CXX_COMPILER_ID:GNU>,$<CXX_COMPILER_ID:Clang>>:-Wall -Wextra -pedantic>)

set_target_properties(${PROJECT_NAME} PROPERTIES C_STANDARD 99
                                                 C_STANDARD_REQUIRED ON
                                                 C_EXTENSIONS OFF)

if(CMAKE_GENERATOR MATCHES "Visual Studio")
  set_property(DIRECTORY PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})
endif(CMAKE_GENERATOR MATCHES "Visual Studio")