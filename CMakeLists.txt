function(write_licence_disclaimer FILE_NAME PACKAGES)
  file(WRITE ${FILE_NAME} "")
  set(PRINT_DELIMITER OFF)
  foreach(package ${PACKAGES})
    if(PRINT_DELIMITER)
      file(APPEND ${FILE_NAME} "\n-----\n")
    endif()
    file(GLOB licences "${${package}_SOURCE_DIR}/LICENSE*" "${${package}_SOURCE_DIR}/LICENCE*")
    list(LENGTH licences LICENSE_COUNT)
    if(LICENSE_COUNT GREATER_EQUAL 1)
      list(GET licences 0 licence)
      file(READ ${licence} LICENCE_TEXT)
      file(APPEND ${FILE_NAME}
           "The following software may be included in this product: ${package}\n\n${LICENCE_TEXT}\n"
      )
      set(PRINT_DELIMITER ON)
    else()
      message("WARNING: no licence files found for package \"${package}\".")
    endif()
    if(LICENSE_COUNT GREATER 1)
      message(
        "WARNING: multiple licence files found for package \"${package}\": ${licences}. Only first file will be used."
      )
    endif()
  endforeach()
  message("wrote licences to ${FILE_NAME}")
endfunction()

set(CPMLicences_SCRIPT_PATH
    ${CMAKE_CURRENT_LIST_FILE}
    CACHE INTERNAL ""
)

function(create_write_licence_disclaimer_target TARGET_NAME FILE_NAME PACKAGES)
  message("add target for licences: ${PACKAGES}")

  set(PACKAGE_SOURCES "")
  foreach(package ${PACKAGES})
    list(APPEND PACKAGE_SOURCES "-D${package}_SOURCE_DIR=\"${${package}_SOURCE_DIR}\"")
  endforeach()

  add_custom_target(
    ${TARGET_NAME}
    COMMAND
      ${CMAKE_COMMAND} -DPRINT_LICENCE_FILE_NAME=${FILE_NAME}
      "-DPRINT_LICENCE_PACKAGES=\"${PACKAGES}\"" ${PACKAGE_SOURCES} -P ${CPMLicences_SCRIPT_PATH}
  )
endfunction()

if(PRINT_LICENCE_FILE_NAME)
  write_licence_disclaimer(${PRINT_LICENCE_FILE_NAME} "${PRINT_LICENCE_PACKAGES}")
endif()
