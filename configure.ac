AC_PREREQ(2.52)

AC_INIT([Apertium frp], [1.0.0], [hectoralos@gmail.com], [apertium-frp], [http://wiki.apertium.org/wiki/Apertium-frp])
AM_INIT_AUTOMAKE

AC_PROG_LN_S
AC_PROG_AWK

m4_define([required_apertium_version], [3.6.0])
m4_define([required_lttoolbox_version], [3.5.0])

PKG_CHECK_MODULES(APERTIUM, apertium >= 3.6.0)
PKG_CHECK_MODULES(LTTOOLBOX, lttoolbox >= 3.5.0)
PKG_CHECK_MODULES(CG3, cg3 >= 1.3.1)


AC_PATH_PROG([CGCOMP], [cg-comp], [false], [$PATH$PATH_SEPARATOR$with_cg_comp/bin])
AS_IF([test x$CGCOMP = xfalse], [AC_MSG_ERROR([You don't have cg-comp installed])])


AP_MKINCLUDE

AC_OUTPUT([Makefile apertium-frp.pc])

