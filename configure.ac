AC_PREREQ([2.59])
AC_INIT([sngrep], [1.4.2], [kaian@irontec.com], [sngrep], [http://www.irontec.com/])
AM_INIT_AUTOMAKE([1.9])
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])
AC_CONFIG_HEADERS([src/config.h])

AC_COPYRIGHT("Irontec S.L.")

# Define _GNU_SOURCE etc.
m4_ifdef([AC_USE_SYSTEM_EXTENSIONS], [AC_USE_SYSTEM_EXTENSIONS])

# debug compilation
AC_ARG_ENABLE(debug,
    AC_HELP_STRING(--enable-debug, [Debug compilation (Default = no)]),
    enable_debug=$enableval, enable_debug=no)

if test "$enable_debug" = "yes" ; then
    CFLAGS="$CFLAGS  -g -O0 -Wall -Werror -Wno-unused-but-set-variable"
    CXXFLAGS="$CXXFLAGS $CFLAGS"
fi

# Minimum checks for a C program :)
AC_PROG_CC
AC_PROG_CXX
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_EGREP
AC_LANG(C)

#######################################################################
# Check for other REQUIRED libraries
AC_CHECK_LIB([pthread], [pthread_create], [], [
    AC_MSG_ERROR([ You need to have libpthread installed to compile sngrep.])
])

AC_CHECK_LIB([pcap], [pcap_open_offline], [], [
    AC_MSG_ERROR([ You need to have libpcap installed to compile sngrep.])
])
AC_CHECK_HEADER([pcap.h], [], [
    AC_MSG_ERROR([ You need to have libpcap development files installed to compile sngrep.])
])

####
#### Ncurses Wide character support
####
AC_ARG_ENABLE([unicode],
	AC_HELP_STRING([--enable-unicode], [Enable Ncurses Unicode support]),
	[AC_SUBST(UNICODE, $enableval)],
	[AC_SUBST(UNICODE, no)]
)

AS_IF([test "x$enable_unicode" == "xyes"], [
	# Ncurses with wide-character support
	AC_DEFINE([WITH_UNICODE], [], [Compile With Unicode compatibility])

	AC_CHECK_HEADER([ncursesw/ncurses.h], [], [
	    AC_MSG_ERROR([ You need to have ncurses development files installed to compile sngrep.])
	])

	AC_CHECK_LIB([ncursesw], [initscr], [], [
	    AC_MSG_ERROR([ You need to have libncursesw installed to compile sngrep.])
	])

	AC_CHECK_LIB([panelw], [new_panel], [], [
	    AC_MSG_ERROR([ You need to have ncurses panelw library installed to compile sngrep.])
	])

	AC_CHECK_LIB([formw], [new_form], [], [
	    AC_MSG_ERROR([ You need to have ncurses formsw library installed to compile sngrep.])
	])

	AC_CHECK_LIB([menuw], [new_item], [], [
	    AC_MSG_ERROR([ You need to have ncurses menuw library installed to compile sngrep.])
	])
	], [

	# Ncurses without wide-character support
	AC_CHECK_HEADER([ncurses.h], [], [
	    AC_MSG_ERROR([ You need to have ncurses development files installed to compile sngrep.])
	])

	AC_CHECK_LIB([ncurses], [initscr], [], [
	    AC_MSG_ERROR([ You need to have libncurses installed to compile sngrep.])
	])

	AC_CHECK_LIB([panel], [new_panel], [], [
	    AC_MSG_ERROR([ You need to have ncurses panel library installed to compile sngrep.])
	])

	AC_CHECK_LIB([form], [new_form], [], [
	    AC_MSG_ERROR([ You need to have ncurses forms library installed to compile sngrep.])
	])

	AC_CHECK_LIB([menu], [new_item], [], [
	    AC_MSG_ERROR([ You need to have ncurses menu library installed to compile sngrep.])
	])
])

####
#### GnuTLS Support
####
AC_ARG_WITH([gnutls],
    AS_HELP_STRING([--with-gnutls], [Enable SSL Support (TLS SIP Transport)]),
    [AC_SUBST(WITH_GNUTLS, $withval)],
    [AC_SUBST(WITH_GNUTLS, no)]
)

AS_IF([test "x$WITH_GNUTLS" == "xyes"], [
	AC_CHECK_LIB([gnutls], [gnutls_init], [], [
	    AC_MSG_ERROR([ You need to have gnutls installed to compile sngrep])
	])

	AC_CHECK_LIB([gcrypt], [gcry_md_map_name], [], [
	    AC_MSG_ERROR([ You need to have libgcrypt installed to compile sngrep])
	])
	AC_DEFINE([WITH_GNUTLS],[],[Compile With GnuTLS compatibility])
], [])

####
#### OpenSSL Support
####
AC_ARG_WITH([openssl],
    AS_HELP_STRING([--with-openssl], [Enable SSL Support (TLS SIP Transport)]),
    [AC_SUBST(WITH_OPENSSL, $withval)],
    [AC_SUBST(WITH_OPENSSL, no)]
)

AS_IF([test "x$WITH_OPENSSL" == "xyes"], [
	AS_IF([test "x$WITH_GNUTLS" == "xyes"], [
	    AC_MSG_ERROR([ GnuTLS and OpenSSL can not be enabled at the same time ])
	], [])
    m4_ifdef([PKG_CHECK_MODULES], [
        PKG_CHECK_MODULES([SSL], [libssl libcrypto])
    ], [
        AC_CHECK_LIB([ssl], [SSL_new], [], [
           AC_MSG_ERROR([ You need to have libssl installed to compile sngrep])
        ])
        AC_CHECK_LIB([crypto], [EVP_get_cipherbyname], [], [
            AC_MSG_ERROR([ You need to have libcrypto installed to compile sngrep])
        ])
    ])
	AC_DEFINE([WITH_OPENSSL],[],[Compile With Openssl compatibility])
], [])


####
#### PCRE Support
####
AC_ARG_WITH([pcre],
    AS_HELP_STRING([--with-pcre], [Enable Perl compatible regular expressions]),
    [AC_SUBST(WITH_PCRE, $withval)],
    [AC_SUBST(WITH_PCRE, no)]
)

AS_IF([test "x$WITH_PCRE" == "xyes"], [
	AC_CHECK_HEADER([pcre.h], [], [
	    AC_MSG_ERROR([ You need libpcre development files installed to compile with pcre support.])
	])
	AC_CHECK_LIB([pcre], [pcre_exec], [], [
	    AC_MSG_ERROR([ You need libpcre library installed to compile with pcre support.])
	])
	AC_DEFINE([WITH_PCRE],[],[Compile With Perl Compatible regular expressions support])
], [])

####
#### IPv6 Support
####
AC_ARG_ENABLE([ipv6],
    AS_HELP_STRING([--enable-ipv6], [Enable IPv6 Support]),
    [AC_SUBST(USE_IPV6, $enableval)],
    [AC_SUBST(USE_IPV6, no)]
)

AS_IF([test "x$USE_IPV6" == "xyes"], [
	AC_CHECK_HEADERS([netinet/in.h netinet/ip6.h], [], [
	    AC_MSG_ERROR([ You dont seem to have ipv6 support (no ip6.h found).])
	], [
#ifdef HAVE_SYS_TYPES_H
#include <sys/types.h>
#endif

#ifdef HAVE_NETINET_IN_H
#include <netinet/in.h>
#endif
	])
	AC_DEFINE([USE_IPV6],[],[Compile With IPv6 support])
], [])


####
#### EEP Support
####
AC_ARG_ENABLE([eep],
    AS_HELP_STRING([--enable-eep], [Enable EEP/HEP Support]),
    [AC_SUBST(USE_EEP, $enableval)],
    [AC_SUBST(USE_EEP, no)]
)

AS_IF([test "x$USE_EEP" == "xyes"], [
	AC_DEFINE([USE_EEP],[],[Compile With EEP support])
], [])


# Conditional Source inclusion 
AM_CONDITIONAL([WITH_GNUTLS], [test "x$WITH_GNUTLS" == "xyes"])
AM_CONDITIONAL([WITH_OPENSSL], [test "x$WITH_OPENSSL" == "xyes"])
AM_CONDITIONAL([USE_EEP], [test "x$USE_EEP" == "xyes"])


######################################################################
# Print Logo
AC_ARG_ENABLE(logo,
    AC_HELP_STRING(--disable-logo, [Disable Irontec Logo from Summary menu]),
    [ enable_logo=$enableval],
    [ enable_logo=yes])

AS_IF([test "x$enable_logo" == "xyes"], [
    echo ''
    echo ' ██╗██████╗  ██████╗ ███╗   ██╗████████╗███████╗ ██████╗'
    echo ' ██║██╔══██╗██╔═══██╗████╗  ██║╚══██╔══╝██╔════╝██╔════╝'
    echo ' ██║██████╔╝██║   ██║██╔██╗ ██║   ██║   █████╗  ██║     '
    echo ' ██║██╔══██╗██║   ██║██║╚██╗██║   ██║   ██╔══╝  ██║     '
    echo ' ██║██║  ██║╚██████╔╝██║ ╚████║   ██║   ███████╗╚██████╗'
    echo ' ╚═╝╚═╝  ╚═╝ ╚═════╝ ╚═╝  ╚═══╝   ╚═╝   ╚══════╝ ╚═════╝'
    echo ''
])

AC_MSG_NOTICE
AC_MSG_NOTICE(   sngrep configure finished                            	)
AC_MSG_NOTICE( ====================================================== 	)
AC_MSG_NOTICE( GnuTLS Support               : ${WITH_GNUTLS}		)
AC_MSG_NOTICE( OpenSSL Support              : ${WITH_OPENSSL} 			)
AC_MSG_NOTICE( Unicode Support              : ${UNICODE}  		)
AC_MSG_NOTICE( Perl Expressions Support     : ${WITH_PCRE}              )
AC_MSG_NOTICE( IPv6 Support                 : ${USE_IPV6}               )
AC_MSG_NOTICE( EEP Support                  : ${USE_EEP}               )
AC_MSG_NOTICE( ====================================================== 	)
AC_MSG_NOTICE


AC_CONFIG_FILES([Makefile])
AC_CONFIG_FILES([src/Makefile])
AC_CONFIG_FILES([config/Makefile])
AC_CONFIG_FILES([doc/Makefile])
AC_CONFIG_FILES([tests/Makefile])
AC_OUTPUT
