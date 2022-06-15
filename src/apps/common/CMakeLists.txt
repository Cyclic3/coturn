# Author: Kang Lin (kl222@126.com)

set(SOURCE_FILES
    apputils.c
    ns_turn_utils.c
    stun_buffer.c
    )

set(HEADER_FILES
    apputils.h
    ns_turn_openssl.h
    ns_turn_utils.h
    stun_buffer.h
    )

if(NOT WIN32)
    set(COMMON_LIBS pthread)
endif()
set(libevent_components core extra openssl)
if(NOT WIN32)
    list(APPEND libevent_components pthreads)
endif()
find_package(Libevent COMPONENTS ${libevent_components})
if(Libevent_FOUND)
    foreach(_libevent_com ${libevent_components})
        list(APPEND COMMON_LIBS Libevent::${_libevent_com})
    endforeach()
endif()

find_package(hiredis)
if(hiredis_FOUND)
    list(APPEND SOURCE_FILES hiredis_libevent2.c)
    list(APPEND HEADER_FILES hiredis_libevent2.h)
    list(APPEND COMMON_LIBS hiredis::hiredis)
else()
    list(APPEND COMMON_DEFINED TURN_NO_HIREDIS)
endif()
message("COMMON_LIBS:${COMMON_LIBS}")

add_library(turncommon ${SOURCE_FILES} ${HEADER_FILES})

target_link_libraries(turncommon PUBLIC ${COMMON_LIBS})
target_compile_definitions(turncommon PUBLIC ${COMMON_DEFINED})
target_compile_options(turncommon PUBLIC -Wno-deprecated-declarations)

# See: http://www.it1352.com/478094.html
target_include_directories(turncommon PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/src/server>
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/src>
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/src/client>
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/src/apps/common>
    ${COMMON_INCLUDE_DIR}
    )

# Install head files
set_target_properties(turncommon PROPERTIES
    VERSION ${VERSION}
    )

INSTALL(TARGETS turncommon
    EXPORT turncommonConfig
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
        COMPONENT Runtime
    LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
        COMPONENT Runtime
    ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
    INCLUDES DESTINATION
        ${CMAKE_INSTALL_INCLUDEDIR}
        ${CMAKE_INSTALL_INCLUDEDIR}/turn
        ${CMAKE_INSTALL_INCLUDEDIR}/turn/client
    )

export(TARGETS turncommon
    APPEND FILE ${CMAKE_BINARY_DIR}/turncommonConfig.cmake
    )

# Install cmake configure files
install(EXPORT turncommonConfig
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake"
    )
# Install cmake version configure file
if(DEFINED VERSION)
    write_basic_package_version_file(
        "${CMAKE_BINARY_DIR}/turncommonConfigVersion.cmake"
        VERSION ${VERSION}
        COMPATIBILITY AnyNewerVersion)
    install(FILES "${CMAKE_BINARY_DIR}/turncommonConfigVersion.cmake"
        DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake")
endif()
