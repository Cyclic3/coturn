# Author: Kang Lin (kl222@126.com)

set(SOURCE_FILES
    stunclient.c
    )

add_executable(turnutils_stunclient ${SOURCE_FILES})
target_link_libraries(turnutils_stunclient PRIVATE turnclient)
INSTALL(TARGETS turnutils_stunclient
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
        COMPONENT Runtime
    )
