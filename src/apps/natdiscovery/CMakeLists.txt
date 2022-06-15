# Author: Kang Lin (kl222@126.com)

set(SOURCE_FILES
    natdiscovery.c
    )

add_executable(turnutils_natdiscovery ${SOURCE_FILES})
target_link_libraries(turnutils_natdiscovery PRIVATE turnclient)
INSTALL(TARGETS turnutils_natdiscovery
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
        COMPONENT Runtime
    )
