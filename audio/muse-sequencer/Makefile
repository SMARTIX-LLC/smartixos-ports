PORTNAME=	muse-sequencer
DISTVERSION=	4.1.0
PORTREVISION=	1
CATEGORIES=	audio

MAINTAINER=	yuri@FreeBSD.org
COMMENT=	Digital audio workstation (DAW) with support for both Audio and MIDI
WWW=		https://muse-sequencer.org/

LICENSE=	GPLv2+
LICENSE_FILE=	${WRKSRC}/COPYING

BROKEN_i386=	compilation breaks: unknown type name 'intptr_t'; did you mean '__intptr_t', see https://github.com/muse-sequencer/muse/issues/1081

BUILD_DEPENDS=	${LOCALBASE}/include/ladspa.h:audio/ladspa \
		lv2>0:audio/lv2
LIB_DEPENDS=	libharfbuzz.so:print/harfbuzz \
		libinstpatch-1.0.so:audio/libinstpatch \
		libjack.so:audio/jack \
		liblash.so:audio/lash \
		liblilv-0.so:audio/lilv \
		liblo.so:audio/liblo \
		librtaudio.so:audio/rtaudio \
		liblrdf.so:textproc/liblrdf \
		libsamplerate.so:audio/libsamplerate \
		libserd-0.so:devel/serd \
		libsndfile.so:audio/libsndfile \
		libsord-0.so:devel/sord \
		libsratom-0.so:audio/sratom

USES=		cmake compiler:c++11-lib desktop-file-utils gl gnome localbase:ldflags qt:5 pkgconfig python shared-mime-info shebangfix
USE_GL=		gl
USE_QT=		core gui svg xml widgets buildtools:build linguisttools:build qmake:build uitools:build
USE_GNOME=	atk atkmm cairo cairomm gdkpixbuf2 glib20 glibmm gtk20 gtkmm24 libsigc++20 pango pangomm
USE_LDCONFIG=	yes

USE_GITHUB=	yes
GH_PROJECT=	muse

SHEBANG_FILES=	share/scripts/* utils/muse-find-unused-wavs utils/muse-song-convert.py

CMAKE_OFF=	ENABLE_ALSA ENABLE_DSSI

WRKSRC_SUBDIR=	src

DATADIR=	${PREFIX}/share/muse-${DISTVERSION:R}
DOCSDIR=	${PREFIX}/share/doc/muse-${DISTVERSION:R}

OPTIONS_DEFINE=		FLUIDSYNTH DOCS
OPTIONS_DEFAULT=	FLUIDSYNTH
OPTIONS_SUB=		yes

FLUIDSYNTH_DESC=	Enable fluidsynth softsynth plugins
FLUIDSYNTH_CMAKE_BOOL=	ENABLE_FLUID
FLUIDSYNTH_LIB_DEPENDS=	libfluidsynth.so:audio/fluidsynth

PORTDOCS=	*

.include <bsd.port.mk>
