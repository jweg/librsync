project(librsync)

add_library(librsync-popt
  STATIC
  popt/findme.c
  popt/popt.c
  popt/poptconfig.c
  popt/popthelp.c
  popt/poptparse.c
  popt/findme.h
  popt/popt.h
  popt/poptint.h
  popt/system.h
)
target_include_directories(librsync-popt PUBLIC popt) 

if(WIN32)
    target_add_definitions(librsync-popt PRIVATE HAVE_CONFIG_H _CRT_SECURE_NO_WARNINGS)
    #target_link_libraries(zeromq ws2_32.lib)
    if(COMMAND target_compile_options)
        # Avoid some warnings.
        target_compile_options(librsync-popt PRIVATE /wd4267)
    endif()
endif()


if(UNIX)
  # get_system_distribution(_systemdist_name)
  # if(_systemdist_name STREQUAL "Linux64-debian7")
    # configure_file(config.h.wheezy config.h COPYONLY)
  # else()
    # configure_file(config.h.lenny config.h COPYONLY)
  # endif()
elseif(WIN32)
  configure_file("PCbuild/config.h" config.h COPYONLY)
  configure_file("PCbuild/librsync-config.h" librsync-config.h COPYONLY)
endif()

#"WIN32;_DEBUG;_WINDOWS;_USRDLL;LIBRSYNC_EXPORTS;_CRT_SECURE_NO_WARNINGS"

add_library(librsync
  STATIC
  #PCbuild/dllmain.cpp

  base64.c
  buf.c
  checksum.c
  command.c
  delta.c
  emit.c
  fileutil.c
  hex.c
  isprefix.c
  job.c
  mdfour.c
  mksum.c
  msg.c
  netint.c
  patch.c
  prototab.c
  readsums.c
  rollsum.c
  scoop.c
  search.c
  stats.c
  stream.c
  sumset.c
  trace.c
  tube.c
  util.c
  version.c
  whole.c
)
target_import_libraries(librsync librsync-popt)
target_include_directories(librsync PUBLIC . "${CMAKE_CURRENT_BINARY_DIR}")

if(WIN32)
    target_add_definitions(librsync PRIVATE _CRT_SECURE_NO_WARNINGS)
    target_add_definitions(librsync PUBLIC LIBRSYNC_STATIC_LIB _CRT_SECURE_NO_WARNINGS)
    #target_link_libraries(zeromq ws2_32.lib)
    if(COMMAND target_compile_options)
        # Avoid some warnings.
        target_compile_options(librsync PRIVATE /wd4267)
    endif()
endif()

add_subdirectory(examples
#  EXCLUDE_FROM_ALL
)
