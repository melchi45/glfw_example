cmake_minimum_required(VERSION 2.8)

set ( PROJECT_NAME glfw_example )

project( ${PROJECT_NAME} )
set ( ${PROJECT_NAME}_HOME ${CMAKE_CURRENT_SOURCE_DIR} )
message("SourceDir: " ${${PROJECT_NAME}_HOME} )

if ( CMAKE_COMPILER_IS_GNUCXX )
   set ( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fpermissive" )
endif ()

# set where to find additional cmake modules if any
# comment it out if not required
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH})
message ( "CMAKE_MODULE_PATH = ${CMAKE_MODULE_PATH}" )


# set compiler support for C++11 standard
option(USE_CXX11_STD "Product should be build with C++11 compiler option enabled" ON)

if(USE_CXX11_STD)
    set(CMAKE_CXX_STANDARD 11)
endif()

if(MSVC)
	add_definitions(-D_CONSOLE)
else()
    # GCC or Clang
endif()

#set ( THREADS_USE_PTHREADS_WIN32 TRUE )
#find_package(Threads REQUIRED)

#message ( "THREADS_PTHREADS_INCLUDE_DIR = ${THREADS_PTHREADS_INCLUDE_DIR}" )
#message ( "THREADS_PTHREADS_WIN32_LIBRARY = ${THREADS_PTHREADS_WIN32_LIBRARY}" )
#message ( "CMAKE_THREAD_LIBS_INIT = ${CMAKE_THREAD_LIBS_INIT}" )


find_package(OpenGL REQUIRED)
if(OPENGL_FOUND)
message("OPENGL_LIBRARIES = ${OPENGL_LIBRARIES} ")
message("OPENGL_INCLUDE_DIRS = ${OPENGL_INCLUDE_DIRS} ")
endif()

message("CMAKE_INCLUDE_PATH = ${CMAKE_INCLUDE_PATH}")

#find_package(GLFW3) # First try to find using the find_package method (works with vcpkg)
#if(GLFW_FOUND) # Works with some other methods
#	message(STATUS "GLFW3_INCLUDE_PATH=${GLFW3_INCLUDE_PATH}")
#	message(STATUS "GLFW_LIBRARIES=${GLFW_LIBRARIES}")
#endif()

# use standard file endings - rename to cxx if needed
set(CXX_SRC_EXT_PATTERN ".cpp")
set(C_SRC_EXT_PATTERN ".c")
set(HDR_EXT_PATTERN ".h")

# get all *.cpp files from src to build target
file(GLOB SRC_FILES ${PROJECT_SOURCE_DIR}/src/*${CXX_SRC_EXT_PATTERN} ${PROJECT_SOURCE_DIR}/src/*${C_SRC_EXT_PATTERN})
file(GLOB HDR_FILES ${PROJECT_SOURCE_DIR}/src/*${HDR_EXT_PATTERN})
# makes source and header files group
source_group("Source Files" FILES ${SRC_FILES})
source_group("Header Files" FILES ${HDR_FILES})

# build target
add_executable( ${PROJECT_NAME} ${SRC_FILES} ${HDR_FILES} )

target_include_directories( ${PROJECT_NAME} PRIVATE ${OPENGL_INCLUDE_DIRS} )
#target_include_directories( ${PROJECT_NAME} PRIVATE ${GLFW3_INCLUDE_PATH} )

target_link_libraries( ${PROJECT_NAME} ${OPENGL_LIBRARIES} )
#target_link_libraries( ${PROJECT_NAME} ${GLFW_LIBRARIES} )
#target_link_libraries( ${PROJECT_NAME} PRIVATE glfw)

add_custom_command (
    TARGET ${PROJECT_NAME}
    POST_BUILD
    COMMAND ${PROJECT_NAME}
    COMMENT "Execute ${PROJECT_NAME}"
)