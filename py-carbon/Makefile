# $NetBSD$

DISTNAME=	carbon-0.9.12
PKGNAME=	${PYPKGPREFIX}-${DISTNAME}
CATEGORIES=	databases
MASTER_SITES=	-https://github.com/graphite-project/carbon/archive/${PKGVERSION_NOREV}${EXTRACT_SUFX}

MAINTAINER=	filip@joyent.com
HOMEPAGE=	http://launchpad.net/graphite
COMMENT=	Backend storage application for graphite
LICENSE=	apache-2.0

DEPENDS+=	${PYPKGPREFIX}-twisted-[0-9]*:../../net/py-twisted
DEPENDS+=	${PYPKGPREFIX}-whisper>=${PKGVERSION_NOREV}:../../joyent/py-whisper

USE_LANGUAGES=	# none

PYTHON_VERSIONS_INCOMPATIBLE=	33 35 # py-twisted

.include "../../mk/bsd.prefs.mk"
.include "options.mk"

BUILD_DEFS+=	VARBASE GRAPHITE_USER GRAPHITE_GROUP GRAPHITE_DIR

GRAPHITE_USER?=		graphite
GRAPHITE_GROUP?=	graphite

PKG_GROUPS+=			${GRAPHITE_GROUP}
PKG_USERS+=			${GRAPHITE_USER}:${GRAPHITE_GROUP}
PKG_GECOS.${GRAPHITE_USER}=	Graphite Carbon user

GRAPHITE_DIR?=		${VARBASE}/graphite
GRAPHITE_EGDIR?=	share/examples/graphite

OWN_DIRS_PERMS+=	${GRAPHITE_DIR} ${GRAPHITE_USER} ${GRAPHITE_GROUP} 0755
OWN_DIRS_PERMS+=	${GRAPHITE_DIR}/lists ${GRAPHITE_USER} ${GRAPHITE_GROUP} 0755
OWN_DIRS_PERMS+=	${GRAPHITE_DIR}/log ${GRAPHITE_USER} ${GRAPHITE_GROUP} 0755
OWN_DIRS_PERMS+=	${GRAPHITE_DIR}/rrd ${GRAPHITE_USER} ${GRAPHITE_GROUP} 0755
OWN_DIRS_PERMS+=	${GRAPHITE_DIR}/whisper ${GRAPHITE_USER} ${GRAPHITE_GROUP} 0755

SUBST_CLASSES+=		dirs
SUBST_STAGE.dirs=	pre-configure
SUBST_MESSAGE.dirs=	Patching default storage directory
SUBST_FILES.dirs=	setup.py
SUBST_VARS.dirs=	GRAPHITE_DIR GRAPHITE_EGDIR PREFIX

PKG_SYSCONFSUBDIR=	graphite

GRAPHITE_CONF_FILES=	aggregation-rules.conf			\
			blacklist.conf				\
			carbon.amqp.conf			\
			carbon.conf				\
			relay-rules.conf			\
			rewrite-rules.conf			\
			storage-aggregation.conf		\
			storage-schemas.conf			\
			whitelist.conf

.for file in ${GRAPHITE_CONF_FILES}
CONF_FILES+=		${GRAPHITE_EGDIR}/${file}.example \
			${PKG_SYSCONFDIR}/${file}
.endfor

PYSETUPINSTALLARGS=	--prefix=${PREFIX}			\
			--install-lib=${PREFIX}/${PYSITELIB}

INSTALLATION_DIRS+=	share/examples/graphite

.include "../../lang/python/distutils.mk"
.include "../../mk/bsd.pkg.mk"
