PORTNAME=	purple-plugin-pack
PORTVERSION=	2.8.0
CATEGORIES=	net-im
MASTER_SITES=	https://dl.bintray.com/pidgin/releases/

MAINTAINER=	bofh@FreeBSD.org
COMMENT=	Collection of libpurple plugins
WWW=		https://keep.imfreedom.org/pidgin/purple-plugin-pack

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/COPYING

BUILD_DEPENDS=	pidgin>0:net-im/pidgin
LIB_DEPENDS=	libenchant-2.so:textproc/enchant2 \
		libgtkspell.so:textproc/gtkspell \
		libjson-glib-1.0.so:devel/json-glib \
		libpurple.so:net-im/libpurple
RUN_DEPENDS=	pidgin>0:net-im/pidgin

USES=		gettext gnome meson pkgconfig tar:xz
USE_GNOME=	gdkpixbuf2 glib20 gtk20 pango

.include <bsd.port.mk>
