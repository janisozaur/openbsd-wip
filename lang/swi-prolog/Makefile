# $OpenBSD: Makefile,v 1.66 2016/05/12 01:17:13 naddy Exp $

BROKEN-arm =		undefined reference to `__sync_fetch_and_add_4'
BROKEN-hppa =		undefined reference to `__sync_fetch_and_add_4'
BROKEN-sparc64 =	infinite loop or aborts during build

COMMENT =		Prolog for the real world

V =			7.2.3
DISTNAME =		swipl-$V
PKGNAME =		swi-prolog-$V
CATEGORIES =		lang

SHARED_LIBS =		pl	6.0

HOMEPAGE =		http://www.swi-prolog.org/

MAINTAINER =		Edd Barrett <edd@openbsd.org>

# LGPLv2.1
PERMIT_PACKAGE_CDROM =	Yes

WANTLIB += ICE SM X11 Xext Xft Xinerama Xpm Xt c crypto execinfo
WANTLIB += fontconfig freetype gmp jpeg m ncursesw pthread readline ssl
WANTLIB += z

MASTER_SITES =		${HOMEPAGE}download/stable/src/ \
			https://distfiles.nl/

CONFIGURE_STYLE =		gnu
MODGNU_CONFIG_GUESS_DIRS =	${WRKSRC}/src \
				${WRKSRC}/packages/jpl \
				${WRKSRC}/packages/xpce/src

# error: trampoline code generation is not allowed without -ftrampolines
CFLAGS +=		-ftrampolines

CONFIGURE_ENV +=	CIFLAGS=' -I${LOCALBASE}/include' \
			LDFLAGS=' -L${LOCALBASE}/lib' \
			COFLAGS='${CFLAGS}'

CONFIGURE_ARGS =	--enable-mt \
			--disable-custom-flags \
			--with-world \
			--with-jpeg=${LOCALBASE} \
			--enable-shared

USE_GMAKE =		Yes

# Don't pick up bash
MAKE_ENV =		MAKESHELL=/bin/sh

MAKE_FLAGS =		SONAMEOPT= \
			SHAREDV=libpl.so.${LIBpl_VERSION}

BUILD_DEPENDS =		${RUN_DEPENDS}
LIB_DEPENDS =		devel/gmp \
			devel/libexecinfo \
			graphics/jpeg

FULLARCH =		${MACHINE_ARCH:S/amd64/x86_64/}-openbsd${OSREV}
SUBST_VARS +=		V BOOT FULLARCH

post-install:
	cd ${PREFIX}/lib && \
		ln -sf swipl-$V/lib/${FULLARCH}/libpl.so.${LIBpl_VERSION}
	mv ${PREFIX}/lib/swipl-${V}/doc ${PREFIX}/share/doc/swipl

# Swipl multithreading flaky at best on OpenBSD.
# We are not sure if this is our threading implementation or their code.
# The following tests fail (hang). We remove them to allow the rest
# of the tests to execute atleast.
# When updating, uncomment these to see if they still fail.
pre-test:
	rm -f ${WRKBUILD}/src/Tests/core/test_resource_error.pl

.include <bsd.port.arch.mk>

.if ${PROPERTIES:Mlp64}
BOOT =	boot64
.else
BOOT =	boot32
.endif

.include <bsd.port.mk>
