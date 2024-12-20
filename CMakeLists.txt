cmake_minimum_required(VERSION 3.16)
project(framelesswidget LANGUAGES CXX)

# Set C++ standard to C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Disable manifest generation by CMake
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /MANIFEST:NO")

# Find Qt6 packages
find_package(Qt6 REQUIRED COMPONENTS Widgets)
get_target_property(Qt6_PREFIX Qt6::Core INTERFACE_INCLUDE_DIRECTORIES)
string(REGEX REPLACE "(.*)/include.*" "\\1" Qt6_PREFIX "${Qt6_PREFIX}")

# Enable automoc for Qt
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

# Source files
set(SOURCES
    src/main.cpp
    src/gui/framelesshelper.cpp
    src/utils/winnativeeventfilter.cpp
)

# Header files
set(HEADERS
    src/gui/framelesshelper.hpp
    src/utils/winnativeeventfilter.hpp
)

# Platform-specific settings
if(WIN32)
    add_definitions(-DWIN32_LEAN_AND_MEAN)
    set(RC_FILE res/resources.rc)
    # Remove manifest.xml from OTHER_FILES since it's included in RC_FILE
    # set(OTHER_FILES res/manifest.xml)
endif()

# Define the executable
add_executable(${PROJECT_NAME}
    ${SOURCES}
    ${HEADERS}
    ${RC_FILE}
)

# Link Qt libraries
target_link_libraries(${PROJECT_NAME} PRIVATE Qt6::Widgets)

# Add definitions
target_compile_definitions(${PROJECT_NAME} PRIVATE
    QT_NO_CAST_FROM_ASCII
    QT_NO_CAST_TO_ASCII
)

# Add Qt private headers
target_include_directories(${PROJECT_NAME} PRIVATE
    ${Qt6_PREFIX}/include
    ${Qt6_PREFIX}/include/QtCore
    ${Qt6_PREFIX}/include/QtGui
    ${Qt6_PREFIX}/include/QtWidgets
    ${Qt6_PREFIX}/include/QtCore/${Qt6Core_VERSION}
    ${Qt6_PREFIX}/include/QtCore/${Qt6Core_VERSION}/QtCore
    ${Qt6_PREFIX}/include/QtGui/${Qt6Gui_VERSION}
    ${Qt6_PREFIX}/include/QtGui/${Qt6Gui_VERSION}/QtGui
    ${Qt6_PREFIX}/include/QtWidgets/${Qt6Widgets_VERSION}
    ${Qt6_PREFIX}/include/QtWidgets/${Qt6Widgets_VERSION}/QtWidgets
)

# Add project include directories
target_include_directories(${PROJECT_NAME} PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${CMAKE_CURRENT_SOURCE_DIR}/src/gui
    ${CMAKE_CURRENT_SOURCE_DIR}/src/utils
)

# Enable GUI private headers
target_compile_definitions(${PROJECT_NAME} PRIVATE
    QT_USE_PRIVATE_HEADERS
)

# Set the subsystem to Windows
if(WIN32)
    set_target_properties(${PROJECT_NAME} PROPERTIES
        WIN32_EXECUTABLE TRUE
    )
endif()