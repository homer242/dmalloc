#
# Process this file with autoconf to produce a configure script.
#
# Copyright 2003 by Gray Watson
#
# This file is part of the dmalloc package.
#
# Permission to use, copy, modify, and distribute this software for
# any purpose and without fee is hereby granted, provided that the
# above copyright notice and this permission notice appear in all
# copies, and that the name of Gray Watson not be used in
# advertising or publicity pertaining to distribution of the
# document or software without specific, written prior permission.
#
# Gray Watson makes no representations about the suitability of the
# software described herein for any purpose.  It is provided "as is"
# without express or implied warranty.
#
# The author may be contacted via http://dmalloc.com/
#
# $Id: configure.ac,v 1.37 2007/05/14 17:23:37 gray Exp $
#
AC_REVISION($Revision: 1.38 $)
AC_INIT(dmalloc, 5.5.2, [https://github.com/homer242/dmalloc])
AC_CONFIG_SRCDIR(dmalloc.c)
AC_CONFIG_HEADER(conf.h)
AC_CONFIG_MACRO_DIR([m4])

AC_MSG_NOTICE([configurations for the dmalloc library])

AM_INIT_AUTOMAKE([foreign])

##############################################################################
# arguments

AC_ARG_ENABLE(cxx,
    [ AC_HELP_STRING([--disable-cxx],
		[do not build in C++ support (enabled by default)] ) ],
    [ # enable_cxx set by default ],
    [ enable_cxx=yes ]
)
AC_ARG_ENABLE(threads,
    [ AC_HELP_STRING([--enable-threads],
		[build in thread support (disabled by default)] ) ],
    [ # enable_threads set by default ],
    [ enable_threads=no ]
)
AC_ARG_ENABLE(shlib,
    [ AC_HELP_STRING([--enable-shlib],
		[build shared libraries (disabled by default)] ) ],
    [ # enable_shlib set by default ],
    [ enable_shlib=no ]
)

##############################################################################
AC_MSG_NOTICE([build utilities])

AC_PROG_CC
AC_PROG_CXX

# see if we actually have a CXX program
if test "$ac_cv_prog_CXX" = "" -o ! -x "$ac_cv_prog_CXX"; then
    AC_MSG_WARN(could not find C++ compiler $ac_cv_prog_CXX)
    enable_cxx=no
fi

# hopefully we have a stdc c-compiler
if test "$ac_cv_prog_cc_stdc" = "no" ; then
	AC_MSG_WARN()
	AC_MSG_WARN(no ansi compiler.  this build may fail.)
	AC_MSG_WARN()
fi
AC_PROG_INSTALL
AC_PROG_LIBTOOL
AC_C_CONST

# we need this for various settings
AC_HEADER_STDC

AH_TEMPLATE([HAVE_STDARG_H],
            [check stdarg.h header])
AH_TEMPLATE([HAVE_SYS_MMAN_H],
            [check sys/mman.h header])
AH_TEMPLATE([HAVE_ERRNO_H],
            [check errno.h header])

AC_CHECK_HEADER([stdarg.h],
		[AC_DEFINE(HAVE_STDARG_H,1) AC_SUBST([HAVE_STDARG_H],1)],
		[AC_DEFINE(HAVE_STDARG_H,0) AC_SUBST([HAVE_STDARG_H],0)])
AC_CHECK_HEADER([stdlib.h],
		[AC_DEFINE(HAVE_STDLIB_H,1) AC_SUBST([HAVE_STDLIB_H],1)],
		[AC_DEFINE(HAVE_STDLIB_H,0) AC_SUBST([HAVE_STDLIB_H],0)])
AC_CHECK_HEADER([string.h],
		[AC_DEFINE(HAVE_STRING_H,1) AC_SUBST([HAVE_STRING_H],1)],
		[AC_DEFINE(HAVE_STRING_H,0) AC_SUBST([HAVE_STRING_H],0)])
AC_CHECK_HEADER([unistd.h],
		[AC_DEFINE(HAVE_UNISTD_H,1) AC_SUBST([HAVE_UNISTD_H],1)],
		[AC_DEFINE(HAVE_UNISTD_H,0) AC_SUBST([HAVE_UNISTD_H],0)])
AC_CHECK_HEADER([errno.h],
		[AC_DEFINE(HAVE_ERRNO_H,1) AC_SUBST([HAVE_ERRNO_H],1)],
		[AC_DEFINE(HAVE_ERRNO_H,0) AC_SUBST([HAVE_ERRNO_H],0)])
AC_CHECK_HEADER([sys/types.h],
		[AC_DEFINE(HAVE_SYS_TYPES_H,1) AC_SUBST([HAVE_SYS_TYPES_H],1)],
		[AC_DEFINE(HAVE_SYS_TYPES_H,0) AC_SUBST([HAVE_SYS_TYPES_H],0)])
AC_CHECK_HEADER([sys/mman.h],
		[AC_DEFINE(HAVE_SYS_MMAN_H,1) AC_SUBST([HAVE_SYS_MMAN_H],1)],
		[AC_DEFINE(HAVE_SYS_MMAN_H,0) AC_SUBST([HAVE_SYS_MMAN_H],0)])

###############################################################################

AC_MSG_CHECKING([C++ support])
if test "$enable_cxx" = "yes"; then
	AC_SUBST([CXX_ON],[])
	AC_SUBST([CXX_OFF],[#])
	AC_MSG_RESULT([enabled])
else
	AC_SUBST([CXX_ON],[#])
	AC_SUBST([CXX_OFF],[])
	AC_MSG_RESULT([disabled])
fi

AC_MSG_CHECKING([thread support])
if test "$enable_threads" = "yes"; then
	AC_SUBST([TH_ON],[])
	AC_SUBST([TH_OFF],[#])
	AC_MSG_RESULT([enabled])
else
	AC_SUBST([TH_ON],[#])
	AC_SUBST([TH_OFF],[])
	AC_MSG_RESULT([disabled])
fi

# shlib support
AC_MSG_CHECKING(shlib support)
if test "$enable_shlib" = "yes"; then
	AC_SUBST([SL_ON],[])
	AC_SUBST([SL_OFF],[#])
	AC_MSG_RESULT(enabled)
else
	AC_SUBST([SL_ON],[#])
	AC_SUBST([SL_OFF],[])
	AC_MSG_RESULT(disabled)
fi

###############################################################################
#
# check for size_t
#
AH_TEMPLATE([DMALLOC_SIZE],
            [size_t existence])

AC_TYPE_SIZE_T
if test "$ac_cv_type_size_t" = "yes"; then
	AC_DEFINE(DMALLOC_SIZE,size_t)
else
	AC_DEFINE(DMALLOC_SIZE,unsigned long)
fi

#
# check for unsigned-ness
#
AH_TEMPLATE([DMALLOC_SIZE_UNSIGNED],
            [size_t unsigned])

AC_MSG_CHECKING([dmalloc size unsigned])
AC_RUN_IFELSE([AC_LANG_SOURCE([[
#if HAVE_SYS_TYPES_H
#  include <sys/types.h>
#endif
#if HAVE_STDLIB_H
#  include <stdlib.h>
#endif
main() { DMALLOC_SIZE x = -1; if (x >= 0) exit(0); else exit(1); }
]])],
[AC_DEFINE(DMALLOC_SIZE_UNSIGNED,1)  AC_MSG_RESULT([yes])],
[AC_DEFINE(DMALLOC_SIZE_UNSIGNED,0)  AC_MSG_RESULT([no])],
[AC_DEFINE(DMALLOC_SIZE_UNSIGNED,0)  AC_MSG_RESULT([no])]
)

#
# check for strdup macro (linux)
#
AC_MSG_CHECKING([strdup macro])
AC_RUN_IFELSE([AC_LANG_SOURCE([[
#if HAVE_STDLIB_H
#  include <string.h>
#endif

#ifdef strdup
main() { exit(0); }
#else
main() { exit(1); }
#endif
]])],
[ac_cv_strdup_macro=yes],
[ac_cv_strdup_macro=no],
[ac_cv_strdup_macro=no]
)
AC_MSG_RESULT([$ac_cv_strdup_macro])

#
# check for existance of mprotect and associated defines
#
AH_TEMPLATE([PROTECT_ALLOWED],
            [Set to 1 if the mprotect function was found and the PROT_NONE,
             PROT_READ, and PROT_WRITE defines were found in sys/mman.h.  This
             is so that we can restrict access to certain blocks of memory.])

AC_CHECK_FUNCS(mprotect)
AC_MSG_CHECKING([mprotect works])
AC_LINK_IFELSE([AC_LANG_PROGRAM([[
#if HAVE_SYS_TYPES_H
#  include <sys/types.h>
#endif
#if HAVE_SYS_MMAN_H
#  include <sys/mman.h>
#endif
]],[[
  int prot = PROT_NONE | PROT_READ | PROT_WRITE;
  (void)mprotect(0, 0, prot);
]])],
[ AC_DEFINE(PROTECT_ALLOWED,1) AC_MSG_RESULT([yes]) ],
[ AC_DEFINE(PROTECT_ALLOWED,0) AC_MSG_RESULT([no]) ]
)

##############################################################################
AC_MSG_NOTICE([important functionality])

AC_CHECK_FUNCS(mmap)
AC_CHECK_FUNCS(sbrk)

if test x$ac_cv_func_mmap != xyes && x$ac_cv_func_sbrk != xyes; then
	AC_MSG_WARN()
	AC_MSG_WARN(no mmap nor sbrk function.  See INTERNAL_MEMORY_SPACE in settings.dist.)
	AC_MSG_WARN()
fi

AH_TEMPLATE([USE_MMAP],
            [(void *)mmap(...) is another heap-memory allocation routine that
             systems employ.  On newer systems it is often preferable over sbrk.
             It allocates a block of memory in the virtual-memory system.  The
             USE_MMAP define is set if the standard mmap call works.

             NOTE: If configure generates a 0 for this and HAVE_SBRK on your
             system, you should see the INTERNAL_MEMORY_SPACE setting in the
             settings.h file which is created from the settings.dist file.mmaps usage])

AC_MSG_CHECKING([mmap usage])
AC_RUN_IFELSE([AC_LANG_SOURCE([[
#if HAVE_SYS_TYPES_H
#  include <sys/types.h>
#endif
#if HAVE_SYS_MMAN_H
#  include <sys/mman.h>
#endif
main() {
  /* we could open /dev/zero and map to that but I did not want the library
     to open another file-descriptor */
  char *dest, *src, *src_p, *dest_p;
  src = "0123456789";
  dest = (char *)mmap(0L, 10, PROT_READ | PROT_WRITE | PROT_EXEC,
	              MAP_PRIVATE | MAP_ANON, -1 /* no fd */,
		      0 /* no offset */);
  src_p = src; dest_p = dest;
  while (src_p < src + 10) { *dest_p++ = *src_p++; }
  src_p = src; dest_p = dest;
  while (src_p < src + 10) {
    if (*src_p++ != *dest_p++) { exit(1); }
  }
  exit(0);
}
]])],
[ AC_DEFINE(USE_MMAP, 1) ac_cv_use_mmap=yes],
[ AC_DEFINE(USE_MMAP, 0) ac_cv_use_mmap=no],
[ AC_DEFINE(USE_MMAP, 0) ac_cv_use_mmap=no]
)
AC_MSG_RESULT([$ac_cv_use_mmap])

#
# check for basic block size
#
AH_TEMPLATE([BASIC_BLOCK],
            [This is the basic block size in bits.  If possible, the configure
             script will set this to be the value returned by the getpagesize()
             function.  If not then some sort of best guess will be necessary.
             15 (meaning basic block size of 32k) will probably be good.
             
             NOTE: some sbrk functions round to the correct page-size.  No
             problems aside from a possible small increase in the administration
             overhead should happen if this value is too high.])

AC_CHECK_FUNCS(getpagesize)
AC_MSG_CHECKING([basic-block size])
ac_cv_page_size=0

AC_ARG_WITH(pagesize, 
    [ AC_HELP_STRING([--with-pagesize=nbits], 
                [log base 2 of machine page size] ) ], 
    [ case "$withval" in ?|1?) ac_cv_page_size=$withval; esac ], 
    [ : # default to probing, set above ],
) 

if test $ac_cv_page_size = 0; then
   AC_RUN_IFELSE([AC_LANG_SOURCE([main() { if (getpagesize()<=2048) exit(0); else exit(1); }])],
	[ ac_cv_page_size=11 ] )
fi
if test $ac_cv_page_size = 0; then
   AC_RUN_IFELSE([AC_LANG_SOURCE([main() { if (getpagesize()<=4096) exit(0); else exit(1); }])],
	[ ac_cv_page_size=12 ] )
fi
if test $ac_cv_page_size = 0; then
   AC_RUN_IFELSE([AC_LANG_SOURCE([main() { if (getpagesize()<=8192) exit(0); else exit(1); }])],
	[ ac_cv_page_size=13 ] )
fi
if test $ac_cv_page_size = 0; then
   AC_RUN_IFELSE([AC_LANG_SOURCE([main() { if (getpagesize()<=16384) exit(0); else exit(1); }])],
	[ ac_cv_page_size=14 ] )
fi
if test $ac_cv_page_size = 0; then
    ac_cv_page_size=15
fi
AC_DEFINE_UNQUOTED([BASIC_BLOCK],[$ac_cv_page_size])
AC_MSG_RESULT([$ac_cv_page_size])

#
# data-alignment size...
#
AH_TEMPLATE([ALLOCATION_ALIGNMENT],
            [The alignment value of all allocations in number of bytes for
             loading admin information before an allocation.  If possible, the
             configure script will set this to be the value returned by
             sizeof(long) which in most systems is the register width.
             
             NOTE: the value will never be auto-configured to be less than 8
             because some system (like sparc for instance) report the sizeof(long)
             == 4 while the register size is 8 bytes.  Certain memory needs to be of
             the same base as the register size (stack frames, code, etc.).  Any
             ideas how I can determine the register size in a better (and portable)
             fashion?
             
             NOTE: larger the number the more memory may be wasted by certain
             debugging settings like fence-post checking.])

AC_CHECK_SIZEOF(long)
AC_MSG_CHECKING([data-alignment size])
if test $ac_cv_sizeof_long = 4; then
	# we have to make a special case for sun sparc idiocy
	ac_cv_data_align=8
else
	ac_cv_data_align=$ac_cv_sizeof_long
fi
AC_DEFINE_UNQUOTED([ALLOCATION_ALIGNMENT],[$ac_cv_data_align])
AC_MSG_RESULT([$ac_cv_data_align])

#
# check the safety of the abort function
#
AH_TEMPLATE([ABORT_OKAY],
            [This checks to see if the abort routine does extensive cleaning up
             before halting a program.  If so then it may call malloc functions
             making the library go recursive.  If abort is set to not okay then
             you should tune the KILL_PROCESS and SIGNAL_INCLUDE options in
             settings.h if you want the library to be able to dump core.])

AC_CHECK_FUNCS(abort)
AC_MSG_CHECKING([abort safe])
AC_RUN_IFELSE([AC_LANG_SOURCE([
static int main_b = 0;
static char heap_mem[102400], *heap_p = heap_mem;
free () { if (main_b) _exit(0); }
char *malloc (int size) {
  char *pnt;
  if (main_b) _exit(0);
  pnt = heap_p;
  heap_p += size;
  return pnt;
}
char *calloc (int number, int size) {
  char *start, *pnt, *end;
  if (main_b) _exit(0);
  /* it should be already 0s */
  start = malloc (number * size);
  pnt = start;
  end = start + size;
  while (pnt < end) { *pnt++ = '\0'; }
  return start;
}
char *realloc (char *old_pnt, int new_size) {
  char *start, *pnt, *end;
  if (main_b) _exit(0);
  start = malloc (new_size);
  pnt = start;
  end = start + new_size;
  while (pnt < end) { *pnt++ = *old_pnt++; }
  return start;
}
main() { main_b = 1; abort(); _exit(1); }
])],
# NOTE: this is reversed because abort core dumps and doesn't exit with
# 0 which means that it should fail
[ AC_DEFINE(ABORT_OKAY, 0) AC_MSG_RESULT([no]) ],
# if it succeeds then something called free on the way out
[ AC_DEFINE(ABORT_OKAY, 1) AC_MSG_RESULT([yes]) ],
[ AC_DEFINE(ABORT_OKAY, 0) AC_MSG_RESULT([no]) ]
)

# signals
AH_TEMPLATE([HAVE_SIGNAL_H],
            [check signal.h header])
AH_TEMPLATE([SIGNAL_OKAY],
            [This checks to see if we can include signal.h and get SIGHUP,
             SIGINT, and SIGTERM for the catch-signals token.  With this token,
             you can have the library do an automatic shutdown if we see the
             above signals.])

AC_TYPE_SIGNAL
AC_MSG_CHECKING([signal works])
AC_CHECK_HEADER([signal.h],
		[AC_DEFINE(HAVE_SIGNAL_H,1) AC_SUBST([HAVE_SIGNAL_H],1)],
		[AC_DEFINE(HAVE_SIGNAL_H,0) AC_SUBST([HAVE_SIGNAL_H],0)])
AC_LINK_IFELSE([AC_LANG_PROGRAM([[
#if HAVE_SIGNAL_H
  #include <signal.h>
#endif
]], [[
	(void)signal(SIGHUP, SIG_IGN);
	(void)signal(SIGINT, SIG_IGN);
	(void)signal(SIGTERM, SIG_IGN);
]])],
[ AC_DEFINE(SIGNAL_OKAY, 1) AC_MSG_RESULT([yes]) ],
[ AC_DEFINE(SIGNAL_OKAY, 0) AC_MSG_RESULT([no]) ]
)

#
# check the safety of the getenv function
#
AH_TEMPLATE([GETENV_SAFE],
            [Under the Cygwin environment, when malloc calls getenv, it core
             dumps.  This is because Cygwin, as far as I know, is loading the
             shared libraries for the various system functions and goes
             recursive with a call to getenv.  Ugh.
             
             So we have to delay the getenv call.  This sets when we can do the
             getenv call so the environmental processing is delayed.])

AC_CHECK_FUNCS(getenv)
AC_MSG_CHECKING([getenv safe])
AC_RUN_IFELSE([AC_LANG_SOURCE([
static char heap_mem[102400], *heap_p = heap_mem;
free (char *pnt) {
}
char *malloc (int size) {
  char *pnt;
  getenv("PATH");
  pnt = heap_p;
  heap_p += size;
  return pnt;
}
char *calloc (int number, int size) {
  char *start, *pnt, *end;
  getenv("PATH");
  /* it should be already 0s */
  start = malloc (number * size);
  pnt = start;
  end = start + size;
  while (pnt < end) { *pnt++ = '\0'; }
  return start;
}
char *realloc (char *old_pnt, int new_size) {
  char *start, *pnt, *end;
  getenv("PATH");
  start = malloc (new_size);
  pnt = start;
  end = start + new_size;
  while (pnt < end) { *pnt++ = *old_pnt++; }
  return start;
}
main() { malloc(10); _exit(0); }
])],
[ AC_DEFINE(GETENV_SAFE, 1)
  AC_MSG_RESULT([yes])
  getenv_safe=yes ],
# if it succeeds then something called free on the way out
[ AC_DEFINE(GETENV_SAFE, 0)
  AC_MSG_RESULT([no])
  getenv_safe=no ],
[ AC_DEFINE(GETENV_SAFE, 1)
  AC_MSG_RESULT([yes])
  getenv_safe=yes ]
)

# now check to see if we can replace getenv with GetEnvironmentVariableA
AH_TEMPLATE([HAVE_SYS_CYGWIN_H],
            [check sys/cygwin.h header])
AH_TEMPLATE([HAVE_W32API_WINDEF_H],
            [check w32api/windef.h header])
AH_TEMPLATE([HAVE_W32API_WINBASE_H],
            [check w32api/winbase.h header])
AH_TEMPLATE([HAVE_GETENVIRONMENTVARIABLEA],
            [See if we have the GetEnvironmentVariableA Cygwin function.  This
             is used as a getenv replacement.])

AC_CHECK_HEADER([sys/cygwin.h],
		[AC_DEFINE(HAVE_SYS_CYGWIN_H,1) AC_SUBST([HAVE_SYS_CYGWIN_H],1)],
		[AC_DEFINE(HAVE_SYS_CYGWIN_H,0) AC_SUBST([HAVE_SYS_CYGWIN_H],0)])
AC_CHECK_HEADER([w32api/windef.h],
		[AC_DEFINE(HAVE_W32API_WINDEF_H,1) AC_SUBST([HAVE_W32API_WINDEF_H],1)],
		[AC_DEFINE(HAVE_W32API_WINDEF_H,0) AC_SUBST([HAVE_W32API_WINDEF_H],0)])
AC_CHECK_HEADER([w32api/winbase.h],
		[AC_DEFINE(HAVE_W32API_WINBASE_H,1) AC_SUBST([HAVE_W32API_WINBASE_H],1)],
		[AC_DEFINE(HAVE_W32API_WINBASE_H,0) AC_SUBST([HAVE_W32API_WINBASE_H],0)],
		[[
#if HAVE_W32API_WINDEF_H
# include <w32api/windef.h>
#endif]])
AC_MSG_CHECKING([for GetEnvironmentVariableA])
AC_RUN_IFELSE([AC_LANG_SOURCE([
#if HAVE_SYS_CYGWIN_H
# include <sys/cygwin.h>
#endif
#if HAVE_STDARG_H
# include <stdarg.h>
#endif
#if HAVE_W32API_WINDEF_H
# include <w32api/windef.h>
#endif
#if HAVE_W32API_WINBASE_H
# include <w32api/winbase.h>
#endif
main() {
  char env_buf[256];
  GetEnvironmentVariableA("PATH", env_buf, sizeof(env_buf));
}
])],
[ AC_DEFINE(HAVE_GETENVIRONMENTVARIABLEA, 1)
  AC_MSG_RESULT([yes])
  have_getenvironmentvariablea=yes ],
[ AC_DEFINE(HAVE_GETENVIRONMENTVARIABLEA, 0)
  AC_MSG_RESULT([no])
  have_getenvironmentvariablea=no ],
[ AC_DEFINE(HAVE_GETENVIRONMENTVARIABLEA, 1)
  AC_MSG_RESULT([yes])
  have_getenvironmentvariablea=yes ]
)

if test x$getenv_safe != xyes && x$have_getenvironmentvariablea != xyes ; then
  AC_MSG_WARN([Since getenv is not safe, you MUST add the following])
  AC_MSG_WARN([code to the front of main():])
  AC_MSG_WARN([[  #ifdef DMALLOC]])
  AC_MSG_WARN([[    dmalloc_debug_setup(getenv("DMALLOC_OPTIONS"));]])
  AC_MSG_WARN([[  #endif]])
  AC_MSG_WARN([You will need to compile your program with: -DDMALLOC])
fi

############

#
# check for constructor attribute support
#
AH_TEMPLATE([CONSTRUCTOR_WORKS],
            [See whether support exists for the constructor attribute which
             allows the library to run code before main() is called.  I know
             that later versions of gcc have support for this and maybe other
             compilers do as well.])

AC_MSG_CHECKING([constructor attribute])
AC_RUN_IFELSE([AC_LANG_SOURCE([
/* if we call the loc_con constructor then exit with error code 0 */
static void loc_con() __attribute__((constructor));
static void loc_con() { exit(0); }
int main() { exit(1); }
])],
[ AC_DEFINE(CONSTRUCTOR_WORKS, 1) AC_MSG_RESULT([yes]) ],
[ AC_DEFINE(CONSTRUCTOR_WORKS, 0) AC_MSG_RESULT([no]) ],
[ AC_DEFINE(CONSTRUCTOR_WORKS, 1) AC_MSG_RESULT([yes]) ]
)

#
# check for destructor attribute support
#
AH_TEMPLATE([DESTRUCTOR_WORKS],
            [See whether support exists for the destructor attribute which
             allows the library to run code after main() is exited.  I know
             that later versions of gcc have support for this and maybe other
             compilers do as well.])

AC_MSG_CHECKING([destructor attribute])
AC_RUN_IFELSE([AC_LANG_SOURCE([
/* if we call the loc_decon destructor then exit with error code 0 */
static void loc_decon() __attribute__((destructor));
static void loc_decon() { exit(0); }
int main() { return 1; }
])],
[ AC_DEFINE(DESTRUCTOR_WORKS, 1)
  AC_MSG_RESULT([yes])
  destructor_works=yes ],
[ AC_DEFINE(DESTRUCTOR_WORKS, 0)
  AC_MSG_RESULT([no])
  destructor_works=no ],
[ AC_DEFINE(DESTRUCTOR_WORKS, 1)
  AC_MSG_RESULT([yes])
  destructor_works=yes ],
)

##############################################################################

#
# check if the return.h macros work
#
AH_TEMPLATE([RETURN_MACROS_WORK],
            [This checks to see if we can include return.h and use the assembly
             macros there to call the callers address for logging.  If you do
             not want this behavior, then set the USE_RETURN_MACROS to 0 in the
             settings.h file.])

AC_MSG_CHECKING([return.h macros work])
AC_RUN_IFELSE([AC_LANG_SOURCE([

#define __CONF_H__
#define USE_RETURN_MACROS 1
#define RETURN_MACROS_WORK 1

#include "return.h"

static void foo (void)
{
  char	*ret_addr;
  GET_RET_ADDR(ret_addr);
}

main()
{
  foo();
  exit(0);
}
])],
[ AC_DEFINE(RETURN_MACROS_WORK, 1) AC_MSG_RESULT([yes]) ],
[ AC_DEFINE(RETURN_MACROS_WORK, 0) AC_MSG_RESULT([no]) ],
[ AC_DEFINE(RETURN_MACROS_WORK, 0) AC_MSG_RESULT([no]) ]
)

##############################################################################

#
# check to see if ident workds
#
AH_TEMPLATE([IDENT_WORKS],
            [Why can't the compiler folks agree about this.  I really hate Unix
             sometimes for its blatant disregard for anything approaching a
             standard.])

AC_MSG_CHECKING([if ident works])
AC_COMPILE_IFELSE([AC_LANG_SOURCE([
#ident "$Id: configure.ac,v 1.37 2007/05/14 17:23:37 gray Exp $"
static void foo (void) { }
])],
[AC_DEFINE(IDENT_WORKS, 1) AC_MSG_RESULT([yes])],
[AC_DEFINE(IDENT_WORKS, 0) AC_MSG_RESULT([no])]
)

##############################################################################
AH_TEMPLATE([HAVE_PTHREADS_H],
            [Which pthread include file to use.])
AH_TEMPLATE([HAVE_PTHREAD_H],
            [Which pthread include file to use.])
AH_TEMPLATE([THREAD_MUTEX_T],
            [What is the pthread mutex type?  Usually (always?) it is
             pthread_mutex_t.])
AH_TEMPLATE([THREAD_LOCK_INIT_VAL],
            [On some systems, you initialize mutex variables with NULL.  Others
             require various stupid non-portable incantations.  The OSF 3.2 guys
             should be ashamed of themselves.  This only is used if the
             LOCK_THREADS setting is enabled in the settings.h.])

AC_MSG_NOTICE([pthread particulars])

XX_OLD_LIBS=$LIBS

AC_SEARCH_LIBS(pthread_mutex_init, pthread pthreads c_r)
AC_CHECK_HEADER([pthread.h],
		[AC_DEFINE(HAVE_PTHREAD_H,1) AC_SUBST([HAVE_PTHREAD_H],1)],
		[AC_DEFINE(HAVE_PTHREAD_H,0) AC_SUBST([HAVE_PTHREAD_H],0)])
AC_CHECK_HEADER([pthreads.h],
		[AC_DEFINE(HAVE_PTHREADS_H,1) AC_SUBST([HAVE_PTHREADS_H],1)],
		[AC_DEFINE(HAVE_PTHREADS_H,0) AC_SUBST([HAVE_PTHREADS_H],0)])

AC_CHECK_FUNCS(pthread_mutex_init pthread_mutex_lock pthread_mutex_unlock)

AC_MSG_CHECKING([pthread mutex type])
AC_LINK_IFELSE([AC_LANG_PROGRAM([[
#if HAVE_PTHREAD_H
#  include <pthread.h>
#endif
#if HAVE_PTHREADS_H
#  include <pthreads.h>
#endif
]],[[
   pthread_mutex_t dmalloc_mutex;
]])],
[AC_DEFINE(THREAD_MUTEX_T,pthread_mutex_t) AC_MSG_RESULT([pthread_mutex_t])],
[AC_MSG_RESULT([unknown])]
)

AC_MSG_CHECKING([pthread attribute initialization])
AC_LINK_IFELSE([AC_LANG_PROGRAM([[
#if HAVE_PTHREAD_H
#  include <pthread.h>
#endif
#if HAVE_PTHREADS_H
#  include <pthreads.h>
#endif
]], [[
	THREAD_MUTEX_T dmalloc_mutex;
	pthread_mutex_init(&dmalloc_mutex, pthread_mutexattr_default);
]])],
[AC_DEFINE(THREAD_LOCK_INIT_VAL,pthread_mutexattr_default)
 AC_MSG_RESULT([pthread_mutexattr_default])],
[AC_DEFINE(THREAD_LOCK_INIT_VAL,0)
 AC_MSG_RESULT([0])]
)

LIBS=$XX_OLD_LIBS

##############################################################################
AC_MSG_NOTICE([functions])

# auto-shutdown functions
AC_CHECK_FUNCS(atexit on_exit)
if test $ac_cv_func_atexit = no \
   && test $ac_cv_func_on_exit = no \
   && test $destructor_works = no; then
	AC_MSG_WARN()
	AC_MSG_WARN(The library cannot automatically shut itself down without)
	AC_MSG_WARN(atexit, on_exit, or destructor support.  You will need to)
	AC_MSG_WARN(call dmalloc_shutdown directly before exit to get get the)
	AC_MSG_WARN(final statistics and unfreed memory information.)
	AC_MSG_WARN()
fi

# other bells and whistles
AC_CHECK_FUNCS(fork gethostname getpid getuid time ctime)
AC_CHECK_FUNCS(vprintf snprintf vsnprintf)
AC_CHECK_FUNCS(recalloc memalign valloc)

# required although we have code for them in compat.c
AC_CHECK_FUNCS(memcmp memcpy memmove memset)
AC_CHECK_FUNCS(strchr strrchr)
AC_CHECK_FUNCS(strlen strcmp strcpy strsep)

##############################################################################
AC_MSG_NOTICE([various functions for argv files])

# NOTE: some duplicated from above
AC_CHECK_FUNCS(atoi atol getenv)
AC_CHECK_FUNCS(strchr strcmp strcpy strlen strncmp strncpy strsep)

##############################################################################
AC_MSG_NOTICE([various functions for argument checking])

# NOTE: some duplicated from above
AC_CHECK_FUNCS(atoi atol)
AC_CHECK_FUNCS(bcmp bcopy bzero)
AC_CHECK_FUNCS([[index]])
AC_CHECK_FUNCS(memccpy memchr memcmp memcpy memmove memset)
AC_CHECK_FUNCS(rindex)
AC_CHECK_FUNCS(strcasecmp strcat strchr strcmp strcpy strcspn strlen)
AC_CHECK_FUNCS(strncasecmp strncat strncmp strncpy)
AC_CHECK_FUNCS(strpbrk strrchr strspn strstr strtok)

# outputs
AC_CONFIG_FILES([
dmalloc.pc
Makefile
])

AC_CONFIG_COMMANDS([dmalloc_defs.h],
[
echo '#ifndef __DMALLOC_CFG_H__' > dmalloc_defs.h
echo '#define __DMALLOC_CFG_H__' > dmalloc_defs.h
echo '' >> dmalloc_defs.h

echo '/* produced by configure */' >> dmalloc_defs.h
echo '' >> dmalloc_defs.h

if test "$ac_cv_c_const" = "yes"; then
	echo '/* const is available */' >> dmalloc_defs.h
else
        echo '/* const is not available */' >> dmalloc_defs.h
	echo '#ifndef const' >> dmalloc_defs.h
	echo '#define const' >> dmalloc_defs.h
	echo '#endif' >> dmalloc_defs.h
fi

if test "$ac_cv_strdup_macro" = "yes"; then
	echo '/* strdup is a macro */' >> dmalloc_defs.h
	echo '#define DMALLOC_STRDUP_MACRO' >> dmalloc_defs.h
else
	echo '/* strdup is not a macro */' >> dmalloc_defs.h
	echo '#undef DMALLOC_STRDUP_MACRO' >> dmalloc_defs.h
fi
echo '' >> dmalloc_defs.h

echo '/*' >> dmalloc_defs.h
echo ' * the definition of DMALLOC_SIZE' >> dmalloc_defs.h
echo ' *' >> dmalloc_defs.h
echo ' * NOTE: some architectures have malloc, realloc, etc.' >> dmalloc_defs.h
echo ' * using unsigned instead of unsigned long.  You may' >> dmalloc_defs.h
echo ' * have to edit this by hand to fix any compilation' >> dmalloc_defs.h
echo ' * warnings or errors.' >> dmalloc_defs.h
echo ' */' >> dmalloc_defs.h
if test "$ac_cv_type_size_t" = "yes"; then
	echo "#include <sys/types.h>" >> dmalloc_defs.h
	echo "#define DMALLOC_SIZE size_t" >> dmalloc_defs.h
else
	echo "/* no include file needed */" >> dmalloc_defs.h
	echo "#define DMALLOC_SIZE unsigned long" >> dmalloc_defs.h
fi
echo '' >> dmalloc_defs.h
echo '/*' >> dmalloc_defs.h
echo ' * We use stdarg.h for the dmalloc_message and' >> dmalloc_defs.h
echo ' * dmalloc_vmessage functions.' >> dmalloc_defs.h
echo ' */' >> dmalloc_defs.h
if test "$ac_cv_header_stdarg_h" = "yes"; then
	echo "#include <stdarg.h>" >> dmalloc_defs.h
	echo "#define DMALLOC_STDARG 1" >> dmalloc_defs.h
else
	echo "/* no stdarg.h available */" >> dmalloc_defs.h
	echo "#define DMALLOC_STDARG 0" >> dmalloc_defs.h
fi
],
[
# initialization commands
ac_cv_c_const=$ac_cv_c_const
ac_cv_strdup_macro=$ac_cv_strdup_macro
ac_cv_type_size_t=$ac_cv_type_size_t
ac_cv_header_stdarg_h=$ac_cv_header_stdarg_h
])

AC_CONFIG_COMMANDS([settings.h],
[
newfile=settings.h
rm -f $newfile.t
echo '/*' > $newfile.t
echo ' * WARNING: this file was produced from settings.dist' >> $newfile.t
echo ' * by the configure program.  The configure script, when' >> $newfile.t
echo ' * run again, will overwrite changed made here.' >> $newfile.t
echo ' */' >> $newfile.t
echo '' >> $newfile.t
cat $srcdir/settings.dist >> $newfile.t

if cmp -s $newfile.t $newfile 2>/dev/null; then
	AC_MSG_NOTICE([$newfile is unchanged])
	rm -f $newfile.t
else
	rm -f $newfile
	mv $newfile.t $newfile
fi
])

AC_OUTPUT
