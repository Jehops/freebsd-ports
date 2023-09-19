PORTNAME=	microcode
DISTVERSIONPREFIX=	microcode-
DISTVERSION=	20230808
PORTREVISION=	1
CATEGORIES=	sysutils
PKGNAMEPREFIX=	cpu-
PKGNAMESUFFIX=	-intel

MAINTAINER=	sbruno@FreeBSD.org
COMMENT=	Intel CPU microcode updates
WWW=		https://github.com/intel/Intel-Linux-Processor-Microcode-Data-Files

LICENSE=	EULA
LICENSE_NAME=	Intel Software License Agreement
LICENSE_FILE=	${FILESDIR}/LICENSE
LICENSE_PERMS=	dist-mirror no-dist-sell pkg-mirror no-pkg-sell auto-accept

ONLY_FOR_ARCHS=	amd64 i386

USES=		cpe

CPE_PART=	o
CPE_VENDOR=	intel
CPE_PRODUCT=	microcode
USE_GITHUB=	yes
GH_ACCOUNT=	intel
GH_PROJECT=	Intel-Linux-Processor-Microcode-Data-Files

DATADIR=	${PREFIX}/share/cpucontrol
FWDIR=		/boot/firmware
NO_ARCH=	yes

NO_WRKSUBDIR=	yes

OPTIONS_DEFINE=		RC
OPTIONS_DEFAULT=	RC
OPTIONS_SUB=		yes

RC_DESC=	Depend on RC script port and split microcode files

RC_RUN_DEPENDS=	${LOCALBASE}/etc/rc.d/microcode_update:sysutils/cpu-microcode-rc

RC_VARS_OFF=	NO_BUILD=yes

post-extract:
	${CAT} ${WRKSRC}/Intel-Linux-Processor-Microcode-Data-Files-${GH_TAGNAME}/intel-ucode/* \
	  ${WRKSRC}/Intel-Linux-Processor-Microcode-Data-Files-${GH_TAGNAME}/intel-ucode-with-caveats/* \
	  > ${WRKSRC}/intel-ucode.bin

post-extract-RC-on:
	${CP} -p ${FILESDIR}/Makefile ${FILESDIR}/ucode-split.c ${WRKSRC}
	${REINPLACE_CMD} 's|%%GH_TAGNAME%%|${GH_TAGNAME}|g' ${WRKSRC}/Makefile

do-install:
	${MKDIR} ${STAGEDIR}${FWDIR}
	${INSTALL_DATA} ${WRKSRC}/intel-ucode.bin ${STAGEDIR}${FWDIR}/

do-install-RC-on:
	${MKDIR} ${STAGEDIR}${DATADIR}/
	${INSTALL_DATA} ${WRKSRC}/mcodes/* ${STAGEDIR}${DATADIR}/

.include <bsd.port.mk>
