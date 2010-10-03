# Copyright (C) 2007-2009 LuaDist.
# Submitted by David Manura
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

# warning: compilation options might not be optical

PROJECT ( infozip C )
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
INCLUDE ( dist.cmake )

SET ( SRC_UNZIP
  crc32.c crypt.c envargs.c explode.c extract.c fileio.c
  globals.c inflate.c list.c match.c process.c ttyio.c ubz2err.c
  unreduce.c unshrink.c unzip.c zipinfo.c )
IF ( WIN32 AND NOT CYGWIN )
  SET ( SRC_UNZIP ${SRC_UNZIP}
        win32/crc_i386.c win32/nt.c win32/win32.c win32/win32i64.c)
ELSE ( WIN32 AND NOT CYGWIN )
  ADD_DEFINITIONS(-DUNIX)
  SET ( SRC_UNZIP ${SRC_UNZIP} unix/unix.c )
ENDIF ( WIN32 AND NOT CYGWIN )

INCLUDE_DIRECTORIES(.)

INCLUDE(CheckFunctionExists)
CHECK_FUNCTION_EXISTS(lchmod HAVE_LCHMOD)
IF ( NOT HAVE_LCHMOD )
  ADD_DEFINITIONS(-DNO_LCHMOD)
ENDIF ( NOT HAVE_LCHMOD )

ADD_EXECUTABLE ( unzip ${SRC_UNZIP} )

INSTALL ( TARGETS unzip RUNTIME DESTINATION ${INSTALL_BIN} )
INSTALL ( FILES README LICENSE DESTINATION ${INSTALL_DATA} )
