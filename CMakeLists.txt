cmake_minimum_required(VERSION 2.8.3)
project(jimmbot_audiofx)

add_compile_options(-std=c++14)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rostest
  roslib
  jimmbot_msgs
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES jimmbot_audiofx
  CATKIN_DEPENDS roscpp rostest roslib jimmbot_msgs 
)

include_directories(
  include/
  ${catkin_INCLUDE_DIRS}
)

add_library(${PROJECT_NAME}_audiofx src/audiofx.cpp)

target_link_libraries(${PROJECT_NAME}_audiofx ${catkin_LIBRARIES})

add_dependencies(${PROJECT_NAME}_audiofx ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

add_executable(${PROJECT_NAME}_audiofx_loader src/audiofx_loader.cpp)

set_target_properties(${PROJECT_NAME}_audiofx_loader PROPERTIES OUTPUT_NAME audiofx_loader PREFIX "")

add_dependencies(${PROJECT_NAME}_audiofx_loader ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(${PROJECT_NAME}_audiofx_loader ${catkin_LIBRARIES} ${PROJECT_NAME}_audiofx)

install(TARGETS ${PROJECT_NAME}_audiofx_loader
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(TARGETS ${PROJECT_NAME}_audiofx
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h*"
  PATTERN ".svn" EXCLUDE
)

install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
  PATTERN ".svn" EXCLUDE
)

# if(CATKIN_ENABLE_TESTING)
#   catkin_add_gtest(audiofx-test test/audiofx_test.cpp)
#   if(TARGET audiofx-test)
#     target_link_libraries(audiofx-test ${catkin_LIBRARIES} ${PROJECT_NAME}_audiofx)
#   endif()

#   add_rostest_gtest(audiofx_loader-test test/audiofx_loader_test.test test/audiofx_loader_test.cpp)
#   if(TARGET audiofx_loader-test)
#     target_link_libraries(audiofx_loader-test ${catkin_LIBRARIES})
#   endif()
# endif(CATKIN_ENABLE_TESTING) # CATKIN_ENABLE_TESTING
