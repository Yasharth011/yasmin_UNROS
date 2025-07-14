cmake_minimum_required(VERSION 3.5)
project(yasmin VERSION  3.3.0)

# Default to C++17
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

# C++
include_directories(src)
include_directories(include)

set(SOURCES
  src/yasmin/blackboard/blackboard.cpp
  src/yasmin/logs.cpp
  src/yasmin/state.cpp
  src/yasmin/cb_state.cpp
  src/yasmin/state_machine.cpp
  src/yasmin/concurrence.cpp
)

add_library(${PROJECT_NAME} SHARED ${SOURCES})
target_include_directories(${PROJECT_NAME} PUBLIC
  "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
  "$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/include>"
  "$<INSTALL_INTERFACE:include/${PROJECT_NAME}>")

install(TARGETS ${PROJECT_NAME}
  EXPORT ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

install(DIRECTORY include/ 
	DESTINATION include)

install(EXPORT ${PROJECT_NAME}
	FILE ${PROJECT_NAME}.cmake
	DESTINATION lib/cmake/${PROJECT_NAME})

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
    "yasminConfigVersion.cmake"
    VERSION ${yasmin_VERSION}
    COMPATIBILITY AnyNewerVersion)

install(FILES "yasminConfig.cmake" "${CMAKE_CURRENT_BINARY_DIR}/yasminConfigVersion.cmake"
    DESTINATION lib/cmake/yasmin)
