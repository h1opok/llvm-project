add_header_library(
  futex_word_type
  HDRS
    futex_word.h
)

if(NOT TARGET libc.src.__support.OSUtil.osutil)
  return()
endif()

add_header_library(
  mutex
  HDRS
    mutex.h
  DEPENDS
    .futex_word_type
    libc.include.sys_syscall
    libc.src.__support.CPP.atomic
    libc.src.__support.OSUtil.osutil
    libc.src.__support.threads.mutex_common
)

add_object_library(
  thread
  SRCS
    thread.cpp
  DEPENDS
    .futex_word_type
    libc.config.linux.app_h
    libc.include.sys_syscall
    libc.src.errno.errno
    libc.src.__support.CPP.atomic
    libc.src.__support.CPP.stringstream
    libc.src.__support.CPP.string_view
    libc.src.__support.common
    libc.src.__support.error_or
    libc.src.__support.threads.thread_common
  COMPILE_OPTIONS
    -O3
    -fno-omit-frame-pointer # This allows us to sniff out the thread args from
                            # the new thread's stack reliably.
)
