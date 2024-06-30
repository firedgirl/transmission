foreach(P create edit remote show)
    add_executable(${TR_NAME}-${P})

    target_sources(${TR_NAME}-${P}
        PRIVATE
            ${P}.cc)

    target_link_libraries(${TR_NAME}-${P}
        PRIVATE
            ${TR_NAME}
            CURL::libcurl
            fmt::fmt-header-only
            libevent::event)

    if (CMAKE_HOST_WIN32)
        # add_link_options("/NODEFAULTLIB:msvcrt.lib")
        target_link_options(${TR_NAME}-${P}
            PRIVATE
                "/NODEFAULTLIB:msvcrt.lib")

        target_link_libraries(${TR_NAME}-${P}
            PRIVATE
                wldap32.lib
                Normaliz.lib)
    elseif(ANDROID)
        target_link_libraries(${TR_NAME}-${P}
            PRIVATE
                z)
    endif()

    tr_win32_app_info(${TR_NAME}-${P}
        "Transmission Utility ('${P}')"
        "${TR_NAME}-${P}"
        "${TR_NAME}-${P}.exe")

    install(
        TARGETS ${TR_NAME}-${P}
        DESTINATION ${CMAKE_INSTALL_BINDIR})

    if(INSTALL_DOC)
        install(
            FILES ${TR_NAME}-${P}.1
            DESTINATION ${CMAKE_INSTALL_MANDIR}/man1)
    endif()
endforeach()
