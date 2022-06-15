set(SOURCE_FILES
  TurnMsgLib.h
)

add_library(turnclient++ ${SOURCE_FILES})
set_target_properties(turnclient++ PROPERTIES LINKER_LANGUAGE CXX)
target_link_libraries(turnclient++ PUBLIC ${PROJECT_NAME}::turnclient)
target_include_directories(turnclient++ PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)
add_library("${PROJECT_NAME}::turnclient++" ALIAS turnclient++)
