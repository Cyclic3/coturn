# Author: Kang Lin (kl222@126.com)

find_package(OpenSSL REQUIRED)

set(HEADER_FILES
    ${PROJECT_SOURCE_DIR}/src/ns_turn_defs.h
    ${PROJECT_SOURCE_DIR}/src/client++/TurnMsgLib.h
    ns_turn_ioaddr.h
    ns_turn_msg.h
    ns_turn_msg_defs.h
    ns_turn_msg_defs_experimental.h
    ns_turn_msg_addr.h
)

set(SOURCE_FILES
    ns_turn_ioaddr.c
    ns_turn_msg_addr.c
    ns_turn_msg.c
)

add_library(turnclient ${SOURCE_FILES} ${HEADER_FILES})
target_include_directories(turnclient PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)
target_link_libraries(turnclient PUBLIC turncommon OpenSSL::SSL OpenSSL::Crypto)

# Install head files
set_target_properties(turnclient PROPERTIES
    PUBLIC_HEADER "${HEADER_FILES}"
    VERSION ${VERSION}
    )

INSTALL(TARGETS turnclient
    EXPORT turnclientConfig
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
        COMPONENT Runtime
    LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
        COMPONENT Runtime
    ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/turn/client
    INCLUDES DESTINATION
        ${CMAKE_INSTALL_INCLUDEDIR}
        ${CMAKE_INSTALL_INCLUDEDIR}/turn
        ${CMAKE_INSTALL_INCLUDEDIR}/turn/client
    )

export(TARGETS turnclient
    APPEND FILE ${CMAKE_BINARY_DIR}/turnclientConfig.cmake
    )

# Install cmake configure files
install(EXPORT turnclientConfig
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake"
    )
# Install cmake version configure file
if(DEFINED VERSION)
    write_basic_package_version_file(
        "${CMAKE_BINARY_DIR}/turnclientConfigVersion.cmake"
        VERSION ${VERSION}
        COMPATIBILITY AnyNewerVersion)
    install(FILES "${CMAKE_BINARY_DIR}/turnclientConfigVersion.cmake"
        DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake")
endif()

add_library("${PROJECT_NAME}::turnclient" ALIAS turnclient)
