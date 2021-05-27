AC_PREREQ(2.52)

AC_INIT([Apertium frp], [1.0.0], [hectoralos@gmail.com], [apertium-frp], [https://wiki.apertium.org/wiki/Apertium-frp])
AM_INIT_AUTOMAKE
AC_PROG_AWK

PKG_CHECK_MODULES(APERTIUM, apertium >= 3.7.0)
PKG_CHECK_MODULES(LTTOOLBOX, lttoolbox >= 3.5.3)
PKG_CHECK_MODULES(CG3, cg3 >= 1.3.2)

AP_MKINCLUDE

AC_OUTPUT([Makefile apertium-frp.pc])
