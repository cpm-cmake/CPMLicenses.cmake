function(write_license_disclaimer FILE_NAME PACKAGES)
  file(WRITE ${FILE_NAME} "")
  set(PRINT_DELIMITER OFF)
  foreach(package ${PACKAGES})
    if(PRINT_DELIMITER)
      file(APPEND ${FILE_NAME} "\n-----\n")
    endif()
    file(GLOB licenses "${${package}_SOURCE_DIR}/LICENSE*" "${${package}_SOURCE_DIR}/license*")
    list(LENGTH licenses LICENSE_COUNT)
    if(LICENSE_COUNT GREATER_EQUAL 1)
      list(GET licenses 0 license)
      file(READ ${license} license_TEXT)
      file(APPEND ${FILE_NAME}
           "The following software may be included in this product: ${package}\n\n${license_TEXT}\n"
      )
      set(PRINT_DELIMITER ON)
    else()
      message("WARNING: no license files found for package \"${package}\".")
    endif()
    if(LICENSE_COUNT GREATER 1)
      message(
        "WARNING: multiple license files found for package \"${package}\": ${licenses}. Only first file will be used."
      )
    endif()
  endforeach()
  message("wrote licenses to ${FILE_NAME}")
endfunction()

set(CPMlicenses_SCRIPT_PATH
    ${CMAKE_CURRENT_LIST_FILE}
    CACHE INTERNAL ""
)

function(cpm_licenses_create_disclaimer_target TARGET_NAME FILE_NAME PACKAGES)
  message("add target for licenses: ${PACKAGES}")

  set(PACKAGE_SOURCES "")
  foreach(package ${PACKAGES})
    list(APPEND PACKAGE_SOURCES "-D${package}_SOURCE_DIR=\"${${package}_SOURCE_DIR}\"")
  endforeach()

  add_custom_target(
    ${TARGET_NAME}
    COMMAND
      ${CMAKE_COMMAND} -DPRINT_license_FILE_NAME=${FILE_NAME}
      "-DPRINT_license_PACKAGES=\"${PACKAGES}\"" ${PACKAGE_SOURCES} -P ${CPMlicenses_SCRIPT_PATH}
  )
endfunction()

if(PRINT_license_FILE_NAME)
  write_license_disclaimer(${PRINT_license_FILE_NAME} "${PRINT_license_PACKAGES}")
endif()