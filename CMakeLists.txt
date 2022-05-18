cmake_minimum_required(VERSION 3.16...3.21)

if(${CMAKE_VERSION} VERSION_LESS 3.12)
	cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

project(ImGui LANGUAGES C CXX)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/backends ${CMAKE_CURRENT_SOURCE_DIR})

add_library(imgui_core 
	${CMAKE_CURRENT_SOURCE_DIR}/imgui.cpp 
	${CMAKE_CURRENT_SOURCE_DIR}/imgui_demo.cpp 
	${CMAKE_CURRENT_SOURCE_DIR}/imgui_draw.cpp 
	${CMAKE_CURRENT_SOURCE_DIR}/imgui_tables.cpp 
	${CMAKE_CURRENT_SOURCE_DIR}/imgui_widgets.cpp
	)

add_library(ImGui::imgui_core ALIAS imgui_core)
set_target_properties(core PROPERTIES LINKER_LANGUAGE CXX)

find_package(OpenGL REQUIRED)

add_library(glfw_opengl3 
	${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_glfw.cpp 
	${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp
	)

set_target_properties(glfw_opengl3 PROPERTIES LINKER_LANGUAGE CXX)
target_link_libraries(glfw_opengl3 PRIVATE ImGui::imgui_core OpenGL::GL)
add_library(ImGui::glfw_opengl3 ALIAS glfw_opengl3)
