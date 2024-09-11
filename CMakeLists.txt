cmake_minimum_required(VERSION 3.27)
project(imgui VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(imgui INTERFACE)

target_include_directories(imgui
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include/imgui>
)

target_link_directories(imgui
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib>
  $<INSTALL_INTERFACE:lib/imgui>
)

target_link_libraries(imgui
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib/ImGuiLib.lib>
  $<INSTALL_INTERFACE:ImGuiLib.lib>
)

include(CMakePackageConfigHelpers)
configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/imguiConfig.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/imguiConfig.cmake"
  INSTALL_DESTINATION share/imgui
)

install(TARGETS imgui
  EXPORT imguiTargets
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

file(GLOB IMGUI_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h)
install(FILES
  ${IMGUI_HEADERS}
  DESTINATION include/imgui
)

install(FILES
  "${CMAKE_CURRENT_SOURCE_DIR}/lib/ImGuiLib.lib"
  DESTINATION lib/imgui
)

install(
  EXPORT imguiTargets
  DESTINATION share/imgui
  NAMESPACE imgui::
)

install(
  FILES
  "${CMAKE_CURRENT_BINARY_DIR}/imguiConfig.cmake"
  DESTINATION share/imgui
)