cmake_minimum_required(VERSION 2.8.3)
project(my_navigation_test)
 
## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  cob_bringup_sim
  cob_navigation_global
  navigation_test_skeleton
  rostest
)

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)


## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()

################################################
## Declare ROS messages, services and actions ##
################################################

## To declare and build messages, services or actions from within this
## package, follow these steps:
## * Let MSG_DEP_SET be the set of packages whose message types you use in
##   your messages/services/actions (e.g. std_msgs, actionlib_msgs, ...).
## * In the file package.xml:
##   * add a build_depend and a run_depend tag for each package in MSG_DEP_SET
##   * If MSG_DEP_SET isn't empty the following dependencies might have been
##     pulled in transitively but can be declared for certainty nonetheless:
##     * add a build_depend tag for "message_generation"
##     * add a run_depend tag for "message_runtime"
## * In this file (CMakeLists.txt):
##   * add "message_generation" and every package in MSG_DEP_SET to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * add "message_runtime" and every package in MSG_DEP_SET to
##     catkin_package(CATKIN_DEPENDS ...)
##   * uncomment the add_*_files sections below as needed
##     and list every .msg/.srv/.action file to be processed
##   * uncomment the generate_messages entry below
##   * add every package in MSG_DEP_SET to generate_messages(DEPENDENCIES ...)

## Generate messages in the 'msg' folder
# add_message_files(
#   FILES
#   Message1.msg
#   Message2.msg
# )

## Generate services in the 'srv' folder
# add_service_files(
#   FILES
#   Service1.srv
#   Service2.srv
# )

## Generate actions in the 'action' folder
# add_action_files(
#   FILES
#   Action1.action
#   Action2.action
# )

## Generate added messages and services with any dependencies listed here
# generate_messages(
#   DEPENDENCIES
#   std_msgs  # Or other packages containing msgs
# )

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES my_navigation_test
#  CATKIN_DEPENDS cob_bringup_sim cob_navigation_global navigation_test_skeleton
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
include_directories(
  ${catkin_INCLUDE_DIRS}
)

## Declare a cpp library
# add_library(my_navigation_test
#   src/${PROJECT_NAME}/my_navigation_test.cpp
# )

## Declare a cpp executable
# add_executable(my_navigation_test_node src/my_navigation_test_node.cpp)

## Add cmake target dependencies of the executable/library
## as an example, message headers may need to be generated before nodes
# add_dependencies(my_navigation_test_node my_navigation_test_generate_messages_cpp)

## Specify libraries to link a library or executable target against
# target_link_libraries(my_navigation_test_node
#   ${catkin_LIBRARIES}
# )

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS my_navigation_test my_navigation_test_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############


#variabels
SET(TEST_NAVIGATION_ARGS 2dnav_ros_eband)
SET(TEST_ROBOTS cob3-6 cob3-3)
SET(TEST_ROBOT_ENVS ipa-kitchen)
SET(TEST_SCENARIOS scene2)
SET(TEST_SIM true)

if(CATKIN_ENABLE_TESTING)

  if(NOT EXISTS $ENV{BAG_PATH}) #checking a basic condition
  
  MESSAGE( FATAL_ERROR "---> ERROR: BAG_PATH NOT SET <---" )

  endif(EXISTS $ENV{BAG_PATH}) 

  if(EXISTS $ENV{WORKSPACE}) 
    MESSAGE( "---> simulation <---" )# jenkins
    
    #execution
    MESSAGE( "---> NAVIGATION TEST <---" )
    MESSAGE( "WITH" )
    FOREACH(TEST_NAVIGATION_ARG ${TEST_NAVIGATION_ARGS})
    
      MESSAGE( "  TEST_NAVIGATION_ARG: ${TEST_NAVIGATION_ARG}" )
      FOREACH(TEST_ROBOT ${TEST_ROBOTS})
      
        MESSAGE( "    TEST_ROBOT: ${TEST_ROBOT}" )
        FOREACH(TEST_ROBOT_ENV ${TEST_ROBOT_ENVS})
        
          MESSAGE( "      TEST_ROBOT_ENV: ${TEST_ROBOT_ENV}" )
          FOREACH(TEST_SCENARIO ${TEST_SCENARIOS})
          
            MESSAGE( "        TEST_SCENARIO: ${TEST_SCENARIO}" )
            
            add_rostest(launch/my_test_param.test ARGS "navigation_arg:=${TEST_NAVIGATION_ARG} robot:=${TEST_ROBOT} robot_env:=${TEST_ROBOT_ENV} scenarioName:=${TEST_SCENARIO}")  
            
            #add_test(NAME ${TEST_ROBOT}_TARG COMMAND "rostest my_navigation_test my_test_param.test navigation_arg:=${TEST_NAVIGATION_ARG} robot:=${TEST_ROBOT} robot_env:=${TEST_ROBOT_ENV} scenarioName:=${TEST_SCENARIO}")  
            
            MESSAGE( "-------------------------" ) 
          
          ENDFOREACH(TEST_SCENARIO)
          
        ENDFOREACH(TEST_ROBOT_ENV)
        
      ENDFOREACH(TEST_ROBOT)
      
    ENDFOREACH(TEST_NAVIGATION_ARG)
          
    
    #navigation_arg:=${TEST_NAVIGATION_ARG} robot:=${TEST_ROBOT} robot_env:=${TEST_ROBOT_ENV} scenarioName:=${TEST_SCENARIO})
  else(EXISTS $ENV{WORKSPACE})
    
    MESSAGE( "---> real robot <---" )# somewhere else
  
  endif(EXISTS $ENV{WORKSPACE})

endif(CATKIN_ENABLE_TESTING)
