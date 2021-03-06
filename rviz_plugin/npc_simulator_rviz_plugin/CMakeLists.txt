cmake_minimum_required(VERSION 2.8.3)
project(npc_simulator_rviz_plugin)

find_package(catkin REQUIRED COMPONENTS
  rviz
  tf
  npc_simulator
)

catkin_package(
  LIBRARIES npc_simulator_rviz_plugin
  CATKIN_DEPENDS
    rviz
    tf
    npc_simulator
)

include_directories(
  ${catkin_INCLUDE_DIRS}
)

if(rviz_QT_VERSION VERSION_LESS "5")
  message(STATUS "Using Qt4 based on the rviz_QT_VERSION: ${rviz_QT_VERSION}")
  find_package(Qt4 ${rviz_QT_VERSION} EXACT REQUIRED QtCore QtGui)
  include(${QT_USE_FILE})
else()
  message(STATUS "Using Qt5 based on the rviz_QT_VERSION: ${rviz_QT_VERSION}")
  find_package(Qt5 ${rviz_QT_VERSION} EXACT REQUIRED Core Widgets)
  set(QT_LIBRARIES Qt5::Widgets)
endif()

set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
add_definitions(-DQT_NO_KEYWORDS)

## Declare a C++ library
add_library(npc_simulator_rviz_plugin
  src/tools/pedestrian_pose.cpp
  src/tools/car_pose.cpp
  src/tools/delete_all_objects.cpp
)

target_link_libraries(npc_simulator_rviz_plugin ${QT_LIBRARIES} ${catkin_LIBRARIES})

install(TARGETS
    ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(
  DIRECTORY
    icons
    plugins
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
