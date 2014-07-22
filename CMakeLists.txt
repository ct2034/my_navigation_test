cmake_minimum_required(VERSION 2.8.3)
project(my_navigation_test)
 
find_package(catkin REQUIRED COMPONENTS
  cob_bringup_sim
  cob_navigation_global
  navigation_test_skeleton
  navigation_test_helper
  rostest
)

catkin_package(
)

include_directories(
  ${catkin_INCLUDE_DIRS}
)

#########################
## Testing: Parameters ##
#########################

#variabels to be defined here
#................................................................
SET(TEST_NAVIGATION_ARGS 2dnav_ipa_eband 2dnav_ros_eband)
SET(TEST_ROBOTS raw3-4 cob3-6)
SET(TEST_ROBOT_ENVS ipa-kitchen)
SET(TEST_SCENARIOS scene_doku)
SET(TEST_SIM true)
#''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

if(NOT (EXISTS $ENV{BAG_PATH})) #checking bag path for usability

  MESSAGE(WARNING "---> WARNING: BAG_PATH NOT SET <---" )

endif(NOT (EXISTS $ENV{BAG_PATH})) 

########################
## Testing: Execution ##
########################

FOREACH(TEST_NAVIGATION_ARG ${TEST_NAVIGATION_ARGS})
  FOREACH(TEST_ROBOT ${TEST_ROBOTS})
    FOREACH(TEST_ROBOT_ENV ${TEST_ROBOT_ENVS})
      FOREACH(TEST_SCENARIO ${TEST_SCENARIOS})
       
      MESSAGE( "---> A NAVIGATION TEST <---" )
      if(${TEST_SIM}) 
        MESSAGE( "in simulation" )# jenkins
      else(${TEST_SIM})
        MESSAGE( "!! ON THE ROBOT !!" )# somewhere else
      endif(${TEST_SIM})
      MESSAGE( "with" )
      MESSAGE( "  TEST_NAVIGATION_ARG: ${TEST_NAVIGATION_ARG}" )
      MESSAGE( "    TEST_ROBOT: ${TEST_ROBOT}" )
      MESSAGE( "      TEST_ROBOT_ENV: ${TEST_ROBOT_ENV}" )
      MESSAGE( "        TEST_SCENARIO: ${TEST_SCENARIO}" )
      if(${TEST_NAVIGATION_ARG} MATCHES "ipa")
        MESSAGE("        (AN IPA TEST)")
        SET(TEST_NAVIGATION_IPA true)   
      else(${TEST_NAVIGATION_ARG} MATCHES "ipa") 
        MESSAGE("        (A COB/ROS TEST)")
        SET(TEST_NAVIGATION_IPA false)  
      endif(${TEST_NAVIGATION_ARG} MATCHES "ipa")
      
      add_rostest(launch/my_test_param.test ARGS "navigation_arg:=${TEST_NAVIGATION_ARG} navigation_ipa:=${TEST_NAVIGATION_IPA} robot:=${TEST_ROBOT} robot_env:=${TEST_ROBOT_ENV} scenarioName:=${TEST_SCENARIO} sim:=${TEST_SIM} ") 
                   
      MESSAGE( "-------------------------" ) 
       
      ENDFOREACH(TEST_SCENARIO)
    ENDFOREACH(TEST_ROBOT_ENV)
  ENDFOREACH(TEST_ROBOT)  
ENDFOREACH(TEST_NAVIGATION_ARG)
