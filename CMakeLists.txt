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

#############
## Testing ##
#############

#variabels to be defined here
#................................................................
SET(TEST_NAVIGATION_ARGS 2dnav_ros_eband 2dnav_ipa_eband)
SET(TEST_ROBOTS cob3-6)
SET(TEST_ROBOT_ENVS ipa-kitchen)
SET(TEST_SCENARIOS scene2)
SET(TEST_SIM true)
#''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

if(NOT (EXISTS $ENV{BAG_PATH})) #checking bag path for usability

  MESSAGE(WARNING "---> ERROR: BAG_PATH NOT SET <---" )

endif(NOT (EXISTS $ENV{BAG_PATH})) 

if(${TEST_SIM}) 
#########################
## Testing: Simulation ##
#########################
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
        
        if(${TEST_NAVIGATION_ARG} MATCHES "ipa")
        
          MESSAGE("        (AN IPA TEST)")
          add_rostest(launch/my_test_param_ipa.test ARGS "navigation_arg:=${TEST_NAVIGATION_ARG} robot:=${TEST_ROBOT} robot_env:=${TEST_ROBOT_ENV} scenarioName:=${TEST_SCENARIO} sim:=${TEST_SIM}")  
        
        else(${TEST_NAVIGATION_ARG} MATCHES "ipa") 
        
          MESSAGE("        (A COB/ROS TEST)")
          add_rostest(launch/my_test_param.test ARGS "navigation_arg:=${TEST_NAVIGATION_ARG} robot:=${TEST_ROBOT} robot_env:=${TEST_ROBOT_ENV} scenarioName:=${TEST_SCENARIO} sim:=${TEST_SIM}")  
        
        endif(${TEST_NAVIGATION_ARG} MATCHES "ipa")
                   
        MESSAGE( "-------------------------" ) 
      
      ENDFOREACH(TEST_SCENARIO)
      
    ENDFOREACH(TEST_ROBOT_ENV)
    
  ENDFOREACH(TEST_ROBOT)
  
ENDFOREACH(TEST_NAVIGATION_ARG)
      
else(${TEST_SIM})
####################
## Testing: Robot ##
####################
MESSAGE( "---> real robot <---" )# somewhere else

endif(${TEST_SIM})
