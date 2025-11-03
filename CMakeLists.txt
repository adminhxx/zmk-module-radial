zephyr_include_directories(include)

add_subdirectory(drivers)
add_subdirectory(lib)

set(PATCH_DIR "${CMAKE_SOURCE_DIR}/patches")

file(GLOB PATCH_FILES "${PATCH_DIR}/*.patch")

# 应用补丁
foreach(PATCH ${PATCH_FILES})
  message("应用补丁: ${PATCH}")
  execute_process(
    COMMAND git apply ${PATCH}
    WORKING_DIRECTORY ${MODULE_DIR}
    RESULT_VARIABLE APPLY_RESULT
    ERROR_VARIABLE APPLY_ERROR
  )
  if(APPLY_RESULT GREATER 0)
    message(FATAL_ERROR "补丁应用失败: ${PATCH}, 错误: ${APPLY_ERROR}")
  endif()
endforeach()
