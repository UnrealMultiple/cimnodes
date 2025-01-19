cmake_minimum_required(VERSION 3.1)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../cimgui ./cimgui)

project(cimnodes)

#general settings
file(GLOB IMGUI_SOURCES
    ../cimgui/cimgui.h
    ../cimgui/imgui/imgui.h
    cimnodes.h
	cimnodes.cpp
    imnodes/imnodes.h
    imnodes/imnodes_internal.h
    imnodes/imnodes.cpp
)

set(IMGUI_STATIC "no" CACHE STRING "Build as a static library")
set(CMAKE_CXX_STANDARD 11)

#add library and link
if (IMGUI_STATIC)
    add_library(cimnodes STATIC ${IMGUI_SOURCES})
else (IMGUI_STATIC)
    add_library(cimnodes SHARED ${IMGUI_SOURCES})
endif (IMGUI_STATIC)

add_definitions("-DIMGUI_USER_CONFIG=\"../cimconfig.h\"")
target_compile_definitions(cimnodes PUBLIC IMGUI_DISABLE_OBSOLETE_FUNCTIONS=1 IMNODES_NAMESPACE=imnodes)


target_include_directories(cimnodes PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/imnodes)
target_include_directories(cimnodes PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../cimgui/imgui)
target_include_directories(cimnodes PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../cimgui)
target_link_libraries(cimnodes PRIVATE cimgui)
set_target_properties(cimnodes PROPERTIES PREFIX "")

#install
install(TARGETS cimnodes
    RUNTIME DESTINATION  .
    LIBRARY DESTINATION  .
    ARCHIVE DESTINATION  .
)

