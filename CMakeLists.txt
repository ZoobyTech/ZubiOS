cmake_minimum_required(VERSION 3.16)
project(ZubiOS)

# Set output directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# Include submodules
add_subdirectory(Source/ZubiLinux)
add_subdirectory(Source/z.so)
add_subdirectory(Source/zstdlib)

# Define paths
set(ZUBILINUX_KERNEL ${CMAKE_SOURCE_DIR}/Source/ZubiLinux/vmlinuz)
set(ZSO_BINARY ${CMAKE_SOURCE_DIR}/Source/z.so/z.so)

set(OUTPUT_KERNEL ${CMAKE_BINARY_DIR}/Build/ZubiOS/System/zSys64OS)
set(OUTPUT_LDSO ${CMAKE_BINARY_DIR}/Build/ZubiOS/ld.so)

# Create output directories
file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/Build/ZubiOS/System)

# Copy artifacts after build
add_custom_command(
  OUTPUT ${OUTPUT_KERNEL}
  COMMAND ${CMAKE_COMMAND} -E copy ${ZUBILINUX_KERNEL} ${OUTPUT_KERNEL}
  DEPENDS ${ZUBILINUX_KERNEL}
  COMMENT "Copying vmlinuz → zSys64OS"
)

add_custom_command(
  OUTPUT ${OUTPUT_LDSO}
  COMMAND ${CMAKE_COMMAND} -E copy ${ZSO_BINARY} ${OUTPUT_LDSO}
  DEPENDS ${ZSO_BINARY}
  COMMENT "Copying z.so → ld.so"
)

# Create dummy targets to trigger copy
add_custom_target(copy_kernel ALL DEPENDS ${OUTPUT_KERNEL})
add_custom_target(copy_ldso ALL DEPENDS ${OUTPUT_LDSO})

