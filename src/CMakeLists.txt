add_subdirectory(mhook-lib)
add_subdirectory(disasm-lib)

# lcEngineConfig
add_executable(lcEngineConfig WIN32 lcEngineConfig.cpp)
target_link_libraries(lcEngineConfig disasm shlwapi version)


# lcEngine
if(MINGW)
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fpermissive")
set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fpermissive")

## -flto -mabi=sysv -s")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS}")
endif(MINGW)

if (MSVC)
# message(FATAL_ERROR "not surpport msvc build!")
message(WARNING  "you can only program in vs, but not release the dll by msvc!")
endif(MSVC)

# find_package(Boost REQUIRED)
# include_directories(${Boost_INCLUDE_DIRS})
add_library(lcEngine SHARED dllmain.cpp lcEngine.cpp)
set_target_properties(lcEngine PROPERTIES PREFIX "")
target_link_libraries(lcEngine 
  ${PROJECT_SOURCE_DIR}/lib/libclang.lib 
  mhook 
  disasm
  shlwapi
  shell32
  version
)

# copy libclang.dll to built bin directory
# add_custom_command(TARGET lcEngine POST_BUILD        # Adds a post-build event to MyTest
#     COMMAND ${CMAKE_COMMAND} -E copy_if_different  # which executes "cmake - E copy_if_different..."
#         "${PROJECT_SOURCE_DIR}/bin/libclang.dll"      # <--this is in-file
#         $<TARGET_FILE_DIR:lcEngine>)                 # <--this is out-file path