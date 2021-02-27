cmake_minimum_required(VERSION 3.2 FATAL_ERROR)
project(LearnVulkan)

set(DCMAKE_SH="CMAKE_SH-NOTFOUND")

# Add source files
file(GLOB_RECURSE SOURCE_FILES 
	${CMAKE_SOURCE_DIR}/src/*.c
	${CMAKE_SOURCE_DIR}/src/*.cpp)
	
# Add header files
file(GLOB_RECURSE HEADER_FILES 
	${CMAKE_SOURCE_DIR}/src/*.h
  ${CMAKE_SOURCE_DIR}/src/*.hpp)

# Configure assets header file
configure_file(src/helpers/RootDir.h.in src/helpers/RootDir.h)
include_directories(${CMAKE_BINARY_DIR}/src)

# Define the executable

# We need a CMAKE_DIR with some code to find external dependencies
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

find_package(Vulkan REQUIRED) #Vulkan

#glfw3
set(GLFW_BUILD_EXAMPLES  OFF)
set(GLFW_BUILD_TESTS OFF)
set(GLFW_BUILD_DOCS OFF)
add_subdirectory(${CMAKE_SOURCE_DIR}/libs/glfw-3.3.3) 

set(LIBS glfw Vulkan::Vulkan ${GLFW_LIBRARIES})

# Define the include DIRs
include_directories(
	"${CMAKE_SOURCE_DIR}/src"
	"${CMAKE_SOURCE_DIR}/libs/glfw-3.3.3/include"
	"${CMAKE_SOURCE_DIR}/libs/glm-0.9.9.8/"
)

add_executable(${PROJECT_NAME} ${HEADER_FILES} ${SOURCE_FILES})
target_link_libraries(${PROJECT_NAME} ${LIBS})
