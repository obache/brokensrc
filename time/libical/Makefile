# $NetBSD: Makefile,v 1.32 2020/11/07 09:06:32 mef Exp $

DISTNAME=	libical-3.0.8
PKGREVISION=	3
CATEGORIES=	time
MASTER_SITES=	${MASTER_SITE_GITHUB:=libical/}
GITHUB_RELEASE=	v${PKGVERSION_NOREV}

MAINTAINER=	pkgsrc-users@NetBSD.org
HOMEPAGE=	https://github.com/libical/libical/
COMMENT=	Reference implementation of the iCalendar data type and format
LICENSE=	gnu-lgpl-v2.1 OR mpl-2.0

USE_TOOLS+=	perl pkg-config
USE_CMAKE=	yes

CMAKE_ARGS+=	-DUSE_BUILTIN_TZDATA=OFF
CMAKE_ARGS+=	-DICAL_BUILD_DOCS=OFF
CMAKE_ARGS+=	-DCMAKE_DISABLE_FIND_PACKAGE_BDB=ON
CMAKE_ARGS+=	-DCMAKE_INSTALL_RPATH_USE_LINK_PATH:BOOL=TRUE
CMAKE_ARGS+=	-DCMAKE_INSTALL_RPATH:PATH=${PREFIX}/lib

PKGCONFIG_OVERRIDE+=	libical.pc.in
PKGCONFIG_OVERRIDE+=	src/libical-glib/libical-glib.pc.in

.include "options.mk"

USE_LANGUAGES=	c c++
TEST_TARGET=	test

.include "../../devel/glib2/buildlink3.mk"
.include "../../textproc/icu/buildlink3.mk"
.include "../../textproc/libxml2/buildlink3.mk"
.include "../../mk/pthread.buildlink3.mk"
.include "../../mk/bsd.pkg.mk"
