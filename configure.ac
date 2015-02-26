AC_INIT(nanocomp,0.0.0,[gt1@sanger.ac.uk],[nanocomp],[http://www.sanger.ac.uk])
AC_CANONICAL_SYSTEM
AC_PROG_LIBTOOL

AM_INIT_AUTOMAKE([std-options subdir-objects])
AM_CONFIG_HEADER(config.h)
AC_CONFIG_MACRO_DIR([m4])

CFLAGS_SAVE="${CFLAGS}"
CFLAGS=""
AC_PROG_CC
CFLAGS="${CFLAGS_SAVE}"

CXXFLAGS_SAVE="${CXXFLAGS}"
CXXFLAGS=""
AC_PROG_CXX
CXXFLAGS="${CXXFLAGS_SAVE}"

PKG_PROG_PKG_CONFIG

AC_LANG_PUSH([C++])
AC_HEADER_STDC
AC_LANG_POP

AC_ARG_WITH([libmaus],
            [AS_HELP_STRING([--with-libmaus@<:@=PATH@:>@], [path to installed libmaus library @<:@default=@:>@])],
            [with_libmaus=${withval}],
            [with_libmaus=])

if test ! -z "${with_libmaus}" ; then
	PKGCONFIGPATHSAVE="${PKG_CONFIG_PATH}"
	if test -z "${PKG_CONFIG_PATH}" ; then
		export PKG_CONFIG_PATH="${with_libmaus}/lib/pkgconfig"
	else
		export PKG_CONFIG_PATH="${with_libmaus}/lib/pkgconfig:${PKG_CONFIG_PATH}"
	fi
fi

PKG_CHECK_MODULES([libmaus],[libmaus >= 0.0.195])

if test ! -z "${with_libmaus}" ; then
	if test ! -z "${PKGCONFIGPATHSAVE}" ; then
		export PKG_CONFIG_PATH="${PKGCONFIGPATHSAVE}"
	fi
fi

LIBMAUSCPPFLAGS="${libmaus_CFLAGS}"
LIBMAUSLIBS="${libmaus_LIBS}"

CPPFLAGS="${CPPFLAGS} ${libmaus_CFLAGS}"
LIBS="${LIBS} ${libmaus_LIBS}"

if test "${CXX}" = "g++" ; then
	CPPFLAGS="${CPPFLAGS} -W -Wall"
fi
if test "${CXX}" = "cl.exe" ; then
	CXXFLAGS="${CXXFLAGS} -EHsc -Gr"
	CPPFLAGS="${CPPFLAGS} -W1" # -Wall
fi

AC_ARG_ENABLE(optimization,
        AS_HELP_STRING([--enable-optimization],[use compiler optimization (default yes)]),
        [optimization=${enableval}],[optimization=yes])
AC_ARG_ENABLE(debug,
        AS_HELP_STRING([--enable-debug],[use compiler debug flags (default no)]),
        [debug=${enableval}],[debug=no])
AC_ARG_ENABLE(profile,
        AS_HELP_STRING([--enable-profile],[use compiler profiling flags (default no)]),
        [profile=${enableval}],[profile=no])
AC_ARG_ENABLE(fast,
        AS_HELP_STRING([--enable-fast],[disable evaluation of assertions (default no)]),
        [fast=${enableval}],[fast=no])

if test "${fast}" = "yes" ; then
	CPPFLAGS="${CPPFLAGS} -DNDEBUG"
fi

AC_ARG_ENABLE(openmp,
        AS_HELP_STRING([--enable-openmp],[use OpenMP (default yes)]),
        [openmp=${enableval}],[openmp=yes])

if test "${openmp}" = "yes" ; then
	AC_LANG_PUSH([C++])
	AX_OPENMP(have_openmp=yes,have_openmp=no)
	AC_LANG_POP
fi

if test "${debug}" = "yes" ; then
        CXXFLAGS="${CXXFLAGS} -g -O0 -rdynamic"
        CFLAGS="${CFLAGS} -g -O0 -rdynamic"
	openmp="no"
else
	if test "${profile}" = "yes" ; then
	        CXXFLAGS="${CXXFLAGS} -g -pg -rdynamic"
	        CFLAGS="${CFLAGS} -g -pg -rdynamic"
	else
		if test "${optimization}" = "yes" ; then
			case ${CXX} in
				g++)
					CXXFLAGS="${CXXFLAGS} -O3 -rdynamic"
					CFLAGS="${CFLAGS} -O3 -rdynamic"
					;;
				*-mingw32msvc-g++)
					CXXFLAGS="${CXXFLAGS} -O3 -rdynamic"
					CFLAGS="${CFLAGS} -O3 -rdynamic"
					;;
				cl.exe)
					CXXFLAGS="${CXXFLAGS} -O2 -Ob2 -Ot -Oy"
					;;
			esac
		fi

	fi
fi

PACKAGE_NAME=${PACKAGE}
PACKAGE_VERSION=${VERSION}

AC_SUBST([PACKAGE_NAME])
AC_SUBST([PACKAGE_VERSION])
AC_OUTPUT(Makefile src/Makefile)