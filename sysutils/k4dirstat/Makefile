PORTNAME=	k4dirstat
PORTVERSION=	3.4.2
CATEGORIES=	sysutils kde

MAINTAINER=	ports@FreeBSD.org
COMMENT=	KDE utility that sums up disk usage for directory trees
WWW=		https://github.com/jeromerobert/k4dirstat

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/COPYING

USES=		cmake compiler:c++11-lang cpe desktop-file-utils gettext-tools \
		kde:5 qt:5 tar:bzip2

USE_GITHUB=	yes
GH_ACCOUNT=	jeromerobert

CPE_VENDOR=	kdirstat_project
CPE_PRODUCT=	kdirstat
USE_KDE=	auth codecs completion config configwidgets coreaddons doctools \
		i18n iconthemes jobwidgets kio service widgetsaddons xmlgui
USE_QT=		buildtools:build core gui dbus network qmake:build widgets xml

post-patch:
	@${REINPLACE_CMD} -e \
		'/^Categories/s|=.*|=Qt;KDE;Utility;|' \
		${WRKSRC}/src/k4dirstat.desktop
	@${REINPLACE_CMD} -e \
		's|<sys/errno.h>|<errno.h>|' \
		${WRKSRC}/src/kdirreadjob.cpp

.include <bsd.port.mk>
