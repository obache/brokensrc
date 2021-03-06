# $NetBSD: Makefile,v 1.32 2020/01/26 17:31:10 rillig Exp $

DISTNAME=	xbeeb-0.4.1
PKGREVISION=	3
CATEGORIES=	emulators x11
MASTER_SITES=	http://bbc.nvg.org/emul/Xbeeb/
EXTRACT_SUFX=	.tgz

MAINTAINER=	dsainty@NetBSD.org
HOMEPAGE=	https://www.cloud9.co.uk/james/BBCMicro/Xbeeb/
COMMENT=	Acorn BBC Micro emulator with an X based interface

USE_IMAKE=	yes
MAKE_FLAGS+=	EXTRA_LDOPTIONS=${LDFLAGS:Q}
USE_TOOLS+=	bdftopcf xmkmf mkfontdir
MAN_PAGE=	${WRKDIR}/xbeeb.1

XBEEBROOT=	share/xbeeb
MESSAGE_SUBST+=	XBEEBROOT=${XBEEBROOT}
PLIST_SUBST+=	XBEEBROOT=${XBEEBROOT:Q}

FONTS_DIRS.x11=	${PREFIX}/share/fonts/X11/xbeeb
REQD_DIRS=	${PREFIX}/share/fonts/X11/xbeeb

INSTALLATION_DIRS=	bin ${PKGMANDIR}/man1

.include "options.mk"

.include "../../mk/oss.buildlink3.mk"

.if ${OSS_TYPE} != "none"

SUBST_CLASSES+=		dsp
SUBST_STAGE.dsp=	pre-configure
SUBST_MESSAGE.dsp=	Adjusting sound device path
SUBST_FILES.dsp=	src/SoundOSS.c
SUBST_SED.dsp=		-e "s|\"/dev/dsp\"|\"${DEVOSSAUDIO}\"|"

SUBST_CLASSES+=		osslib
SUBST_STAGE.osslib=	pre-configure
SUBST_MESSAGE.osslib=	Linking OSS library
SUBST_FILES.osslib=	src/Imakefile
SUBST_SED.osslib=	-e "s|\\(SYS_LIBRARIES[	 ]*=.*\\)|\\1 ${LIBOSSAUDIO}|"

.else

SUBST_CLASSES+=		nooss
SUBST_STAGE.nooss=	pre-configure
SUBST_MESSAGE.nooss=	Disabling sound support: No OSS on this platform
SUBST_FILES.nooss=	src/Config.h
SUBST_SED.nooss=	-e "s|^\#define\\([	 ]*SOUND_OSS.*\\)\$$|\#undef\\1|"

.endif # OSS_TYPE

SUBST_CLASSES+=		beebroot
SUBST_STAGE.beebroot=	pre-configure
SUBST_MESSAGE.beebroot=	Adjusting XBEEBROOT path
SUBST_FILES.beebroot=	src/Config.h
SUBST_SED.beebroot=	-e "s|^\\(\#define[	 ]*XBEEBROOT[	 ]*\\)[^	 ].*\$$|\\1\"${PREFIX}/${XBEEBROOT}/\"|"

INSTALLATION_DIRS=	bin ${PKGMANDIR}/man1 ${XBEEBROOT}/roms ${XBEEBROOT}/docs share/fonts/X11/xbeeb

# xbeeb build starts in the source directory
do-configure:
	cd ${WRKSRC}/src && xmkmf

post-patch:
	sed	-e 's+@XBEEBROOT@+${XBEEBROOT}+g' \
		-e 's+@PREFIX@+${PREFIX}+g' \
		${FILESDIR}/xbeeb.1 > ${MAN_PAGE}

do-build:
	cd ${WRKSRC}/src && env ${MAKE_ENV} ${MAKE_PROGRAM} ${MAKE_FLAGS} xbeeb
	set -e; \
	cd ${WRKSRC}/fonts; \
	for i in *.bdf; do \
		${TOOLS_PATH.bdftopcf} "$$i" -o `${BASENAME} "$$i" .bdf`.pcf; \
	done

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/src/xbeeb ${DESTDIR}${PREFIX}/bin/xbeeb
	${INSTALL_MAN} ${MAN_PAGE} ${DESTDIR}${PREFIX}/${PKGMANDIR}/man1/xbeeb.1
	${INSTALL_DATA} ${WRKSRC}/roms/xdfs0.90.rom ${DESTDIR}${PREFIX}/${XBEEBROOT}/roms/xdfs0.90.rom
	cd ${WRKSRC}/doc && ${INSTALL_DATA} * ${DESTDIR}${PREFIX}/${XBEEBROOT}/docs
	cd ${WRKSRC}/fonts && ${INSTALL_DATA} *.pcf ${DESTDIR}${PREFIX}/share/fonts/X11/xbeeb

.include "../../x11/libXext/buildlink3.mk"
.include "../../x11/libX11/buildlink3.mk"
.include "../../mk/bsd.pkg.mk"
