cmake_minimum_required(VERSION 3.4)
set(CMAKE_GENERATOR_PLATFORM x64)

project (GLTFSample_${GFX_API})

# ouput exe to bin directory
SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_HOME_DIRECTORY}/bin)
foreach( OUTPUTCONFIG ${CMAKE_CONFIGURATION_TYPES} )
    string( TOUPPER ${OUTPUTCONFIG} OUTPUTCONFIG )
    set( CMAKE_RUNTIME_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${CMAKE_HOME_DIRECTORY}/bin )
endforeach( OUTPUTCONFIG CMAKE_CONFIGURATION_TYPES )

set(CMAKE_CXX_STANDARD 17)

# reference libs used by both backends
add_subdirectory(libs/cauldron)

set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})

set(Boost_USE_STATIC_LIBS OFF)
set(Boost_USE_MULTITHREADED ON)
set(Boost_USE_STATIC_RUNTIME OFF)
find_package(Boost REQUIRED)
if (Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIR})
    link_directories(${Boost_LIBRARY_DIRS})
endif()

if(GFX_API STREQUAL DX12)        
    add_subdirectory(src/DX12)   
elseif(GFX_API STREQUAL VK)
    find_package(Vulkan REQUIRED)
    add_subdirectory(src/VK)
else()
    message(STATUS "----------------------------------------------------------------------------------------")
    message(STATUS "")
    message(STATUS "** Almost there!!")
    message(STATUS "")
    message(STATUS " This framework supports DX12 or VULKAN, you need to invoke cmake in one of these ways:")
    message(STATUS "")
    message(STATUS " Examples:")
    message(STATUS "    cmake <project_root_dir> -DGFX_API=DX12")
    message(STATUS "    cmake <project_root_dir> -DGFX_API=VK")
    message(STATUS "")
    message(STATUS "----------------------------------------------------------------------------------------")
    message(FATAL_ERROR "")
endif()

