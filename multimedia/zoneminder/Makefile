# $OpenBSD:$
# XXX needs work on manpage installation for this to succeed
USE_NINJA =		No

MULTI_PACKAGES = -main -web
COMMENT-main =	security camera DVR/NVR recorder/alarm monitor
COMMENT-web =	PHP scripts for ZoneMinder

V =		1.34.6
DISTFILES =	${V}.tar.gz

GH_ACCOUNT =	ZoneMinder
GH_PROJECT =	zoneminder
GH_TAGNAME =	${V}

PKGNAME-main =	zoneminder-${V}
PKGNAME-web =	zoneminder-web-${V}

CATEGORIES =	multimedia security

HOMEPAGE =	https://zoneminder.com

CAKE_V =	ea90c0cd7f6e24333a90885e563b5d30b793db29
CRUD_V =	0bd63fb464957080ead342db58ca9e01532cf1ef

DISTFILES +=	cake-${CAKE_V:C/(........).*/\1/}{${CAKE_V}}.tar.gz:0
DISTFILES +=	crud-${CRUD_V:C/(........).*/\1/}{${CRUD_V}}.tar.gz:1

MASTER_SITES0 = https://github.com/ZoneMinder/CakePHP-Enum-Behavior/archive/
MASTER_SITES1 = https://github.com/ZoneMinder/crud/archive/

WANTLIB += ${COMPILER_LIBCXX} avcodec avdevice avformat avutil
WANTLIB += c crypto curl gcrypt gnutls jpeg m pcre ssl
WANTLIB += swresample swscale vlc x264 z

WANTLIB-web += ${COMPILER_LIBCXX} avcodec avdevice avformat avutil
WANTLIB-web += c crypto curl gcrypt gnutls jpeg m pcre ssl swresample
WANTLIB-web += swscale vlc x264 z

# c++11
COMPILER =	base-clang ports-gcc

RUN_DEPENDS-main = ${BUILD_DEPENDS} \
		devel/desktop-file-utils \
		multimedia/zoneminder,-web=${V}
RUN_DEPENDS-web = ${MODPHP_RUN_DEPENDS} \
		lang/php/${MODPHP_VERSION},-mysqli \
		lang/php/${MODPHP_VERSION},-pdo_mysql
BUILD_DEPENDS =	converters/p5-DateManip \
		databases/p5-DBI \
		databases/p5-DBD-mysql \
		devel/glib2 \
		devel/cmake \
		devel/p5-Getargs-Long \
		devel/p5-Sys-Mmap \
		devel/p5-Time-TimeDate \
		graphics/ffmpeg \
		www/p5-LWP-UserAgent-Determined
LIB_DEPENDS =	devel/pcre \
		graphics/ffmpeg \
		graphics/jpeg \
		net/curl \
		security/gnutls \
		security/libgcrypt \
		x11/vlc

# GPLv2
PERMIT_PACKAGE =	Yes

MODULES =		devel/cmake \
			lang/php

CONFIGURE_STYLE =	cmake

SEPARATE_BUILD =	Yes
NO_TEST =		Yes

PREFIX-main =		${LOCALBASE}
PREFIX-web =		${VARBASE}/www

INSTDIR =		${PREFIX-web}

SUBST_VARS =		INSTDIR

post-extract:
	rmdir ${WRKSRC}/web/api/app/Plugin/CakePHP-Enum-Behavior
	mv ${WRKDIR}/CakePHP-Enum-Behavior-* \
		${WRKSRC}/web/api/app/Plugin/CakePHP-Enum-Behavior
	rmdir ${WRKSRC}/web/api/app/Plugin/Crud
	mv ${WRKDIR}/crud-* ${WRKSRC}/web/api/app/Plugin/Crud

post-install:
	${INSTALL_DATA_DIR} ${WRKINST}${INSTDIR}/zm/cache
	${INSTALL_DATA_DIR} ${WRKINST}${INSTDIR}/zm/logs
	${INSTALL_DATA_DIR} ${WRKINST}${INSTDIR}/zm/run
	${INSTALL_DATA_DIR} ${WRKINST}${INSTDIR}/zm/spool
	${INSTALL_DATA_DIR} ${WRKINST}${INSTDIR}/zm/tmp
	@touch ${WRKINST}${INSTDIR}/zm/logs/web_php.log
	@touch ${WRKINST}${INSTDIR}/zm/shm
	@cd ${WRKINST}${INSTDIR}/htdocs/zm && ln -s ../../zm/cache cache
	@chmod 1700 ${WRKINST}${INSTDIR}/zm/cache
	@chmod 1700 ${WRKINST}${INSTDIR}/zm/tmp
	@chown -R www:www ${WRKINST}${INSTDIR}/zm ${WRKINST}${INSTDIR}/htdocs/zm

.include <bsd.port.mk>
