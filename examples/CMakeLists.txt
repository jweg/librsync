project(librsync-examples)

add_executable(librsync-rdiff ../rdiff.c)
if(WIN32)
    target_add_definitions(librsync-rdiff PRIVATE _CRT_SECURE_NO_WARNINGS)
endif()
target_import_libraries(librsync-rdiff librsync)