cmake_minimum_required(VERSION 3.11.0 FATAL_ERROR)
project(sdf_atlas VERSION 0.1.0 LANGUAGES C CXX)

include(GNUInstallDirs)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_BINDIR})

find_package(glfw3 3.3 QUIET)
if(NOT TARGET glfw)
    include(FetchContent)
    FetchContent_Declare(glfw3
            URL https://github.com/glfw/glfw/archive/3.3.1.tar.gz
            URL_MD5 ffa7c0d8547f56b0897276c59efdd641
            )
    FetchContent_GetProperties(glfw3)
    if(NOT glfw3_POPULATED)
        foreach(_glfw3_option "GLFW_BUILD_TESTS" "GLFW_BUILD_EXAMPLES" "GLFW_BUILD_DOCS" "GLFW_INSTALL")
            set(${_glfw3_option} OFF CACHE INTERNAL "")
        endforeach()
        FetchContent_Populate(glfw3)
        add_subdirectory(${glfw3_SOURCE_DIR} ${glfw3_BINARY_DIR})
    endif()
endif()

add_subdirectory(third_party/glad)

add_executable(sdf_atlas
		src/gl_utils.cpp
		src/parabola.cpp
		src/args_parser.cpp
		src/sdf_gl.cpp
		src/glyph_painter.cpp
		src/sdf_atlas.cpp
		src/font.cpp
		src/main.cpp
		)
target_include_directories(sdf_atlas PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)
target_link_libraries(sdf_atlas glfw glad)
install(TARGETS sdf_atlas
		RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
		)
