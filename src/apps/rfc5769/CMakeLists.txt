# Author: Kang Lin (kl222@126.com)

set(SOURCE_FILES
    rfc5769check.c
    )

add_executable(turnutils_rfc5769check ${SOURCE_FILES})
target_link_libraries(turnutils_rfc5769check PRIVATE turnclient)
INSTALL(TARGETS turnutils_rfc5769check
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
        COMPONENT Runtime
    )
