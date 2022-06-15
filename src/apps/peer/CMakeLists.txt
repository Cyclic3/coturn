# Author: Kang Lin (kl222@126.com)

set(SOURCE_FILES
    mainudpserver.c udpserver.c
    )

add_executable(turnutils_peer ${SOURCE_FILES})
target_link_libraries(turnutils_peer PRIVATE turnclient)
INSTALL(TARGETS turnutils_peer
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
        COMPONENT Runtime
    )
