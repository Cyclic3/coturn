# Author: Kang Lin (kl222@126.com)

set(SOURCE_FILES
    uclient.c
    startuclient.c
    mainuclient.c
    )

add_executable(turnutils_uclient ${SOURCE_FILES})
target_link_libraries(turnutils_uclient PRIVATE turnclient)
INSTALL(TARGETS turnutils_uclient
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
        COMPONENT Runtime
    )
