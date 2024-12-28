cmake_minimum_required(VERSION 3.21)

project(minTest)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

find_package(Vulkan REQUIRED)
find_package(VknConfig REQUIRED)

add_executable(Direct-minTest Direct-minTest.cpp)
add_executable(CkBk-minTest CkBk-minTest.cpp)
add_executable(VoxTest VoxTest.cpp VoxTree.cpp)

target_link_libraries(Direct-minTest PRIVATE compiler_flags PRIVATE vulkan-1)
target_link_libraries(CkBk-minTest PRIVATE compiler_flags PRIVATE VknConfig)
target_link_libraries(VoxTest PRIVATE compiler_flags)

# Copy directories to build folders keeping in mind debug configs
function(copyResources)
    if(CMAKE_BUILD_TYPE MATCHES Debug)
        if(NOT EXISTS ${CMAKE_CURRENT_BINARY_DIR}/Debug)
            file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/Debug)
        endif()

        file(COPY resources DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/Debug)
    else()
        file(COPY resources DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
    endif()
endfunction()

copyResources()