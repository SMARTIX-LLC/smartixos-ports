PORTNAME=	virtualenv
PORTVERSION=	20.20.0
PORTREVISION=	1
CATEGORIES=	devel python
PKGNAMEPREFIX=	${PYTHON_PKGNAMEPREFIX}

MAINTAINER=	nivit@FreeBSD.org
COMMENT=	Tool for creating isolated Python environments
WWW=		https://virtualenv.pypa.io

LICENSE=	MIT

BUILD_DEPENDS=	${PYTHON_PKGNAMEPREFIX}distlib>=0.3.6:devel/py-distlib@${PY_FLAVOR}
RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}distlib>=0.3.6:devel/py-distlib@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}filelock>=3.4.1:sysutils/py-filelock@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}platformdirs>=2.4:devel/py-platformdirs@${PY_FLAVOR}
TEST_DEPENDS=	${PYTHON_PKGNAMEPREFIX}covdefaults>=2.2.2:devel/py-covdefaults@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}coverage_enable_subprocess>=1.0:devel/py-coverage_enable_subprocess@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}flaky>=3.7.0:devel/py-flaky@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}packaging>=21.3:devel/py-packaging@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}pytest-env>=0.6.2:devel/py-pytest-env@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}pytest-freezegun>=0.4.2:devel/py-pytest-freezegun@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}pytest-mock>0:devel/py-pytest-mock@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}pytest-randomly>=3.10.3:devel/py-pytest-randomly@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}pytest-timeout>=2.1.0:devel/py-pytest-timeout@${PY_FLAVOR}

USES=		cpe python:3.7+
USE_GITHUB=	yes
GH_ACCOUNT=	pypa
USE_PYTHON=	autoplist concurrent pep517 pytest

PEP517_BUILD_DEPEND=	${PYTHON_PKGNAMEPREFIX}hatch-vcs>=0.3:devel/py-hatch-vcs@${PY_FLAVOR}
PEP517_BUILD_CMD=	${PYTHON_CMD} -m hatchling build --target wheel
PEP517_INSTALL_DEPEND=	${PKGNAMEPREFIX}pip>=22.3.1:devel/py-pip@${PY_FLAVOR}
PEP517_INSTALL_CMD=	${PYTHON_CMD} -m pip install --no-color --no-compile \
			--ignore-installed --no-deps --no-warn-script-location \
			--prefix ${PREFIX} --progress-bar off --root \
			${STAGEDIR} --root-user-action ignore --verbose \
			${BUILD_WRKSRC}/dist/${PORTNAME:C/[-_]+/_/g}-${PORTVERSION}-*.whl

# Fix the LookupError: setuptools-scm was unable to detect version (build target)
MAKE_ENV=	SETUPTOOLS_SCM_PRETEND_VERSION="${PORTVERSION}"

.if make(test)
TEST_DEPENDS+=	python2:lang/python2
TEST_ENV=	PYTHONIOENCODING=utf-8 \
		PYTHONPATH="${STAGEDIR}${PYTHON_SITELIBDIR}"
QA_ENV=		IGNORE_QA_depends_blacklist=yes # we also need lang/python2 to pass some tests
.endif

NO_ARCH=	yes

OPTIONS_DEFINE=	DOCS

DOCS_BUILD_DEPENDS=	${PYTHON_PKGNAMEPREFIX}click-default-group>0:devel/py-click-default-group@${PY_FLAVOR} \
			${PYTHON_PKGNAMEPREFIX}filelock>0:sysutils/py-filelock@${PY_FLAVOR} \
			${PYTHON_PKGNAMEPREFIX}furo>=2022.12.7:textproc/py-furo@${PY_FLAVOR} \
			${PYTHON_PKGNAMEPREFIX}platformdirs>0:devel/py-platformdirs@${PY_FLAVOR} \
			${PYTHON_PKGNAMEPREFIX}proselint>0:devel/py-proselint@${PY_FLAVOR} \
			${PYTHON_PKGNAMEPREFIX}sphinx-argparse>0:textproc/py-sphinx-argparse@${PY_FLAVOR} \
			${PYTHON_PKGNAMEPREFIX}sphinx>=1.8.0,1:textproc/py-sphinx@${PY_FLAVOR} \
			${PYTHON_PKGNAMEPREFIX}towncrier>=22.8.0:textproc/py-towncrier@${PY_FLAVOR}
DOCS_BINARY_ALIAS=	towncrier=towncrier-${PYTHON_VER}
DOCS_PLIST_FILES=	man/man1/virtualenv.1.gz
DOCS_PORTDOCS=		* .buildinfo

# Fix the Exception:
#
# File "/build/lib/virtualenv/run/plugin/discovery.py", line 20, in get_discover
#    default=next(iter(choices)),
# StopIteration
#
# Apparently it doesn't work setting PYTHONPATH environment variable...
VIRTUALENV_WHEEL=	${WRKSRC}/dist/${PORTNAME}-${PORTVERSION}-py${PYTHON_MAJOR_VER}-none-any.whl

post-patch:
	@${SED} -i.bak -e 's,%%VIRTUALENV_WHEEL%%,${VIRTUALENV_WHEEL},1' \
	-e 's,%%PYTHON_VER%%,${PYTHON_VER},1' \
	${WRKSRC}/docs/conf.py

post-install:
	${PYTHON_CMD} -m compileall -d ${PYTHON_SITELIBDIR} ${STAGEDIR}${PYTHON_SITELIBDIR}
	${PYTHON_CMD} -O -m compileall -d ${PYTHON_SITELIBDIR} ${STAGEDIR}${PYTHON_SITELIBDIR}
	@cd ${STAGEDIR}${PREFIX} && ${FIND} lib -name '*.pyc' >> ${_PYTHONPKGLIST}

post-install-DOCS-on:
	${LOCALBASE}/bin/sphinx-build-${PYTHON_VER} -d ${WRKDIR} -b html -n ${WRKSRC}/docs ${STAGEDIR}${DOCSDIR}
	${LOCALBASE}/bin/sphinx-build-${PYTHON_VER} -d ${WRKDIR} -b man -n ${WRKSRC}/docs ${STAGEDIR}${MANPREFIX}/man/man1

.include <bsd.port.pre.mk>

.if "${PYTHON_VER}" == "3.7"
RUN_DEPENDS+=	${PYTHON_PKGNAMEPREFIX}importlib-metadata>=4.8.1:devel/py-importlib-metadata@${PY_FLAVOR}

.endif

.include <bsd.port.post.mk>
