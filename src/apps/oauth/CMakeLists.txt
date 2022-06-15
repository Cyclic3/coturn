# Author: Kang Lin (kl222@126.com)

set(SOURCE_FILES
    oauth.c
    )

add_executable(turnutils_oauth ${SOURCE_FILES})
target_link_libraries(turnutils_oauth PRIVATE turnclient)
INSTALL(TARGETS turnutils_oauth
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
        COMPONENT Runtime
    )
