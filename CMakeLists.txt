# CMake file for Vulkan Application

cmake_minimum_required(VERSION 3.1)

project(VKDemo)

# Set the name of the ouput binary
set(
	BIN_NAME
	"vkdemo"
)

# Add all files from a src folder
file(GLOB SOURCES "src/*.cpp")

# Add vulkan dependencies using enviornment variable from LunarG SDK Setup
include_directories("$ENV{VULKAN_SDK}/include")
link_directories("$ENV{VULKAN_SDK}/lib") 
link_directories("$ENV{VULKAN_SDK}/etc/explicit_layer.d")

# Add GLFW
if(WIN32)
	set( INCLUDES "path to additional includes" CACHE PATH "path to additional includes")
	set( LIBS "path to additional libs" CACHE PATH "path to additional libraries")
	include_directories("${INCLUDES}")
	link_directories("${LIBS}")
else()
	find_package(glfw3 3.2 REQUIRED)
endif()
# Flags for debug build
set(
	CMAKE_CXX_FLAGS_DEBUG 
	"${CMAKE_CXX_FLAGS_DEBUG} -d -Wall"
)
# Flags for Release build
set(
	CMAKE_CXX_FLAGS_RELEASE 
	"${CMAKE_CXX_FLAGS_RELEASE} -Wall"
)

# Set C++11
set (CMAKE_CXX_STANDARD 11)

# Setup binary output
add_executable(${BIN_NAME} ${SOURCES})

# Link dependencies
if(WIN32)
	target_link_libraries(${BIN_NAME} glfw3 vulkan-1)
else()
	target_link_libraries(${BIN_NAME} glfw vulkan)
endif()
