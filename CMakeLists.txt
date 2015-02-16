cmake_minimum_required(VERSION 2.8.11)
set(CMAKE_VERBOSE_MAKEFILE OFF)

project(hello)

################################################################################
# Qt library
################################################################################
set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
find_package(Qt5Core REQUIRED)
find_package(Qt5Gui REQUIRED)
find_package(Qt5Quick REQUIRED)
find_package(Qt5Widgets REQUIRED)

################################################################################
# QML resources
################################################################################
file(GLOB_RECURSE hello_qml qml/*.qml qml/*.js)
set(hello_qml_qrc ${CMAKE_CURRENT_BINARY_DIR}/qml.qrc)
file(WRITE ${hello_qml_qrc} "<RCC>\n    <qresource prefix=\"/\">\n")
foreach(qml_file ${hello_qml})
  file(RELATIVE_PATH qml_file_relative_path ${CMAKE_CURRENT_BINARY_DIR} ${qml_file})
  file(RELATIVE_PATH qml_file_short_name ${CMAKE_SOURCE_DIR}/qml ${qml_file})
  file(APPEND ${hello_qml_qrc} "        <file alias=\"${qml_file_short_name}\">${qml_file_relative_path}</file>\n")
endforeach()
file(APPEND ${hello_qml_qrc} "    </qresource>\n</RCC>")

################################################################################
# App hello
################################################################################
file(GLOB_RECURSE hello_cpp src/*.cpp)
file(GLOB_RECURSE hello_h src/*.h)
qt5_add_resources(hello_rcc ${hello_qml_qrc})
include_directories(AFTER SYSTEM src ${CMAKE_BINARY_DIR})
add_executable(hello ${hello_cpp} ${hello_h} ${hello_rcc} ${hello_qml})
target_link_libraries(hello Qt5::Core Qt5::Gui Qt5::Quick Qt5::Widgets)
