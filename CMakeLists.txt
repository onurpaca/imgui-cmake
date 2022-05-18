cmake_minimum_required(VERSION 3.16...3.21)

if(${CMAKE_VERSION} VERSION_LESS 3.12)
	cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

project(ImGui LANGUAGES C CXX)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/third_party/glfw)

include_directories(
	${CMAKE_CURRENT_SOURCE_DIR} 
	${CMAKE_CURRENT_SOURCE_DIR}/backends 
	${CMAKE_CURRENT_SOURCE_DIR}/third_party/glfw/include
	)

add_library(imgui_core STATIC
	${CMAKE_CURRENT_SOURCE_DIR}/imgui.cpp 
	${CMAKE_CURRENT_SOURCE_DIR}/imgui_demo.cpp 
	${CMAKE_CURRENT_SOURCE_DIR}/imgui_draw.cpp 
	${CMAKE_CURRENT_SOURCE_DIR}/imgui_tables.cpp 
	${CMAKE_CURRENT_SOURCE_DIR}/imgui_widgets.cpp
	)

set_target_properties(imgui_core PROPERTIES LINKER_LANGUAGE CXX)
target_include_directories(imgui_core PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
add_library(ImGui::imgui_core ALIAS imgui_core)

find_package(OpenGL REQUIRED)

add_library(glfw_opengl3 STATIC
	${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_glfw.cpp 
	${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp
	)

set_target_properties(glfw_opengl3 PROPERTIES LINKER_LANGUAGE CXX)
target_include_directories(glfw_opengl3 PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/backends ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(glfw_opengl3 PRIVATE ImGui::imgui_core OpenGL::GL glfw ${GLFW_LIBRARIES})
add_library(ImGui::glfw_opengl3 ALIAS glfw_opengl3)
