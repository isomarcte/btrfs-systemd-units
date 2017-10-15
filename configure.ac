#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([btrfs-systemd-daemons], [1.0.0], [isomarcte@gmail.com])
AM_INIT_AUTOMAKE([foreign dist-xz tar-pax no-dist-gzip])

# Misc #

# Systemd will, by default, only look for installed unit files in a standard and
# fixed set of directories. At present, the default prefix for unit files
# installed by packages should be /usr.
AC_PREFIX_DEFAULT([/usr])

# Checks for programs.
AC_PATH_PROG([SYSTEMCTL], [systemctl])
AS_IF([AS_VAR_TEST_SET([SYSTEMCTL])],,
      [AC_MSG_ERROR([unable to find systemctl binary])])
AC_PATH_PROG([BTRFS], [btrfs])
AS_IF([AS_VAR_TEST_SET([BTRFS])],,
      [AC_MSG_ERROR([unable to find btrfs binary])])

AC_CONFIG_FILES([
  btrfs-balance@.service
  btrfs-scrub@.service
  btrfs-scrub@.timer
  btrfs-snapshot@.service
  btrfs-snapshot@.timer
  Makefile
])
AC_OUTPUT
