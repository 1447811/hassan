AUTOMAKE_OPTIONS = subdir-objects

DISTCHECK_CONFIGURE_FLAGS = \
	--enable-more-warnings=yes

libexec_PROGRAMS =

noinst_LTLIBRARIES =

EXTRA_DIST =

CLEANFILES =

noinst_PROGRAMS =

SUBDIRS = \
	. \
	po

plugindir = $(libdir)/NetworkManager
plugin_LTLIBRARIES =

dbusservicedir = $(sysconfdir)/dbus-1/system.d
dbusservice_DATA = nm-l2tp-service.conf

nmvpnservicedir = $(NM_VPN_SERVICE_DIR)
nmvpnservice_DATA = nm-l2tp-service.name

BUILT_SOURCES =

###############################################################################

shared_sources = \
	shared/nm-default.h \
	shared/nm-utils/gsystem-local-alloc.h \
	shared/nm-utils/nm-macros-internal.h \
	shared/nm-utils/nm-shared-utils.c \
	shared/nm-utils/nm-shared-utils.h

EXTRA_DIST += \
	README.md \
	shared/README

src_cppflags = \
	$(GLIB_CFLAGS) \
	$(LIBNM_CFLAGS) \
	-I$(top_srcdir)/shared \
	-I$(builddir)/src \
	-DNM_L2TP_LOCALEDIR=\"$(datadir)/locale\" \
	-DNM_L2TP_STATEDIR=\"$(l2tp_statedir)\" \
	-DPLUGINDIR=\"$(PPPD_PLUGIN_DIR)\" \
	-DRUNSTATEDIR=\"$(runstatedir)\" \
	-DNETWORKMANAGER_COMPILATION=NM_NETWORKMANAGER_COMPILATION_DEFAULT

###############################################################################

noinst_LTLIBRARIES += src/libnm-l2tp-pppd-service-dbus.la

nodist_src_libnm_l2tp_pppd_service_dbus_la_SOURCES = \
	src/nm-l2tp-pppd-service-dbus.c \
	src/nm-l2tp-pppd-service-dbus.h

src_libnm_l2tp_pppd_service_dbus_la_CPPFLAGS = \
	$(src_cppflags) \
	$(filter-out -DGLIB_VERSION_MAX_ALLOWED%,$(AM_CPPFLAGS))

src/nm-l2tp-pppd-service-dbus.h: $(top_srcdir)/src/nm-l2tp-pppd-service.xml
	$(AM_V_GEN) gdbus-codegen \
		--generate-c-code $(basename $@) \
		--c-namespace NMDBus \
		--interface-prefix org.freedesktop.NetworkManager \
		$<

src/nm-l2tp-pppd-service-dbus.c: src/nm-l2tp-pppd-service-dbus.h
	@true

BUILT_SOURCES += \
	src/nm-l2tp-pppd-service-dbus.h \
	src/nm-l2tp-pppd-service-dbus.c

CLEANFILES += \
	src/nm-l2tp-pppd-service-dbus.h \
	src/nm-l2tp-pppd-service-dbus.c

EXTRA_DIST += \
	src/nm-l2tp-pppd-service.xml

###############################################################################

libexec_PROGRAMS += src/nm-l2tp-service

src_nm_l2tp_service_SOURCES = \
	$(shared_sources) \
	shared/nm-service-defines.h \
	shared/nm-l2tp-crypto-openssl.h \
	shared/nm-l2tp-crypto-openssl.c \
	shared/nm-l2tp-crypto-nss.h \
	shared/nm-l2tp-crypto-nss.c \
	shared/utils.h \
	shared/utils.c \
	src/nm-ppp-status.h \
	src/nm-l2tp-service.h \
	src/nm-l2tp-service.c
src_nm_l2tp_service_CPPFLAGS = $(src_cppflags) $(NSS_CFLAGS) $(OPENSSL_CFLAGS)
src_nm_l2tp_service_LDADD = \
	src/libnm-l2tp-pppd-service-dbus.la \
	$(GLIB_LIBS) \
	$(LIBNM_LIBS) \
	$(OPENSSL_LIBS) \
	$(NSS_LIBS)

###############################################################################

pppd_plugindir = $(PPPD_PLUGIN_DIR)
pppd_plugin_LTLIBRARIES = src/nm-l2tp-pppd-plugin.la

src_nm_l2tp_pppd_plugin_la_SOURCES = \
	$(shared_sources) \
	src/nm-l2tp-pppd-plugin.c \
	src/nm-ppp-status.h
src_nm_l2tp_pppd_plugin_la_CPPFLAGS = $(src_cppflags)
src_nm_l2tp_pppd_plugin_la_LDFLAGS = \
	-module -avoid-version
src_nm_l2tp_pppd_plugin_la_LIBADD = \
	$(GLIB_LIBS) \
	$(LIBNM_LIBS)

###############################################################################

properties/resources.h: properties/gresource.xml
	$(AM_V_GEN) $(GLIB_COMPILE_RESOURCES) $< --target=$@ --sourcedir=$(srcdir)/properties --generate-header --internal

properties/resources.c: properties/gresource.xml $(shell $(GLIB_COMPILE_RESOURCES) --sourcedir=$(srcdir)/properties --generate-dependencies $(srcdir)/properties/gresource.xml)
	$(AM_V_GEN) $(GLIB_COMPILE_RESOURCES) $< --target=$@ --sourcedir=$(srcdir)/properties --generate-source --internal

plugin_sources = \
	shared/nm-utils/nm-vpn-plugin-macros.h \
	shared/nm-l2tp-crypto-openssl.c \
	shared/nm-l2tp-crypto-openssl.h \
	properties/nm-l2tp-editor-plugin.c \
	properties/nm-l2tp-editor-plugin.h

editor_sources = \
	$(shared_sources) \
	shared/nm-utils/nm-vpn-plugin-utils.h \
	shared/nm-l2tp-crypto-openssl.c \
	shared/nm-l2tp-crypto-openssl.h \
	shared/utils.h \
	shared/utils.c \
	properties/resources.c \
	properties/resources.h \
	properties/ppp-dialog.c \
	properties/ppp-dialog.h \
	properties/ipsec-dialog.c \
	properties/ipsec-dialog.h \
	properties/nm-l2tp-editor.c \
	properties/nm-l2tp-editor.h

properties_cppflags = \
	-DLOCALEDIR=\"$(datadir)/locale\" \
	-I$(top_srcdir)/shared \
	$(OPENSSL_CFLAGS)

CLEANFILES += \
	properties/resources.c \
	properties/resources.h

EXTRA_DIST += \
	properties/nm-l2tp-dialog.ui \
	properties/gresource.xml \
	properties/libnm-l2tp-properties.ver \
	properties/libnm-vpn-plugin-l2tp.ver \
	properties/libnm-vpn-plugin-l2tp-editor.ver

###############################################################################

plugin_LTLIBRARIES += properties/libnm-vpn-plugin-l2tp.la

properties_libnm_vpn_plugin_l2tp_la_SOURCES = \
	shared/nm-utils/nm-vpn-plugin-utils.c \
	$(plugin_sources)
properties_libnm_vpn_plugin_l2tp_la_CFLAGS = \
	$(GLIB_CFLAGS) \
	$(LIBNM_CFLAGS) \
	$(OPENSSL_CFLAGS)
properties_libnm_vpn_plugin_l2tp_la_CPPFLAGS = \
	-DNETWORKMANAGER_COMPILATION=NM_NETWORKMANAGER_COMPILATION_LIB_BASE \
	-DNM_PLUGIN_DIR=\"$(NM_PLUGIN_DIR)\" \
	$(properties_cppflags)
properties_libnm_vpn_plugin_l2tp_la_LIBADD = \
	$(GLIB_LIBS) \
	$(LIBNM_LIBS) \
	$(OPENSSL_LIBS)
properties_libnm_vpn_plugin_l2tp_la_LDFLAGS = \
	-avoid-version \
	-Wl,--version-script="$(srcdir)/properties/libnm-vpn-plugin-l2tp.ver"

###############################################################################

if WITH_GNOME
plugin_LTLIBRARIES += properties/libnm-vpn-plugin-l2tp-editor.la
endif

properties_libnm_vpn_plugin_l2tp_editor_la_SOURCES = \
	$(editor_sources)
properties_libnm_vpn_plugin_l2tp_editor_la_CFLAGS = \
	$(GLIB_CFLAGS) \
	$(GTK_CFLAGS) \
	$(LIBNM_CFLAGS) \
	$(LIBNMA_CFLAGS) \
	$(OPENSSL_CFLAGS)
properties_libnm_vpn_plugin_l2tp_editor_la_CPPFLAGS = \
	-DNETWORKMANAGER_COMPILATION=NM_NETWORKMANAGER_COMPILATION_LIB_EDITOR \
	$(properties_cppflags)
properties_libnm_vpn_plugin_l2tp_editor_la_LIBADD = \
	$(GLIB_LIBS) \
	$(GTK_LIBS) \
	$(LIBNM_LIBS) \
	$(LIBNMA_LIBS) \
	$(OPENSSL_LIBS)
properties_libnm_vpn_plugin_l2tp_editor_la_LDFLAGS = \
	-avoid-version \
	-Wl,--version-script="$(srcdir)/properties/libnm-vpn-plugin-l2tp-editor.ver"

###############################################################################

if WITH_LIBNM_GLIB
plugin_LTLIBRARIES += properties/libnm-l2tp-properties.la
endif

properties_libnm_l2tp_properties_la_SOURCES = \
	shared/nm-utils/nm-glib.h \
	$(plugin_sources) \
	$(editor_sources)
properties_libnm_l2tp_properties_la_CFLAGS = \
	$(GLIB_CFLAGS) \
	$(GTK_CFLAGS) \
	$(LIBNM_GLIB_CFLAGS) \
	$(LIBNM_GTK_CFLAGS)
properties_libnm_l2tp_properties_la_CPPFLAGS = \
	-DNETWORKMANAGER_COMPILATION='NM_NETWORKMANAGER_COMPILATION_LIB|NM_NETWORKMANAGER_COMPILATION_WITH_LIBNM_UTIL' \
	$(properties_cppflags)
properties_libnm_l2tp_properties_la_LIBADD = \
	$(GLIB_LIBS) \
	$(GTK_LIBS) \
	$(LIBNM_GLIB_LIBS) \
	$(LIBNM_GLIB_LIBS)
properties_libnm_l2tp_properties_la_LDFLAGS = \
	-avoid-version \
	-Wl,--version-script="$(srcdir)/properties/libnm-l2tp-properties.ver"

###############################################################################

if WITH_GNOME
libexec_PROGRAMS += auth-dialog/nm-l2tp-auth-dialog
endif

auth_dialog_nm_l2tp_auth_dialog_SOURCES = \
	$(shared_sources) \
	shared/nm-l2tp-crypto-openssl.c \
	shared/nm-l2tp-crypto-openssl.h \
	shared/nm-utils/nm-secret-utils.c \
	shared/nm-utils/nm-secret-utils.h \
	properties/resources.c \
	properties/resources.h \
	auth-dialog/main.c
auth_dialog_nm_l2tp_auth_dialog_CFLAGS = \
	$(GLIB_CFLAGS) \
	$(GTK_CFLAGS) \
	$(LIBNM_CFLAGS) \
	$(LIBNMA_CFLAGS) \
	$(LIBSECRET_CFLAGS) \
	$(OPENSSL_CFLAGS)
auth_dialog_nm_l2tp_auth_dialog_CPPFLAGS = \
	-DNETWORKMANAGER_COMPILATION=NM_NETWORKMANAGER_COMPILATION_DEFAULT \
	-I$(top_srcdir)/shared
auth_dialog_nm_l2tp_auth_dialog_LDADD = \
	$(GLIB_LIBS) \
	$(GTK_LIBS) \
	$(LIBNM_LIBS) \
	$(LIBNMA_LIBS) \
	$(LIBSECRET_LIBS) \
	$(OPENSSL_LIBS)

###############################################################################

if WITH_LIBNM_GLIB
# Install a file with full path to plugins for an old gnome-shell
# https://bugzilla.gnome.org/show_bug.cgi?id=693590
install-data-hook:
	mkdir -p $(DESTDIR)$(sysconfdir)/NetworkManager/VPN
	sed -e "1s|^|# This file is obsoleted by a file in $(NM_VPN_SERVICE_DIR)\n\n|" \
	    -e 's|[@]LIBEXECDIR[@]|$(libexecdir)|g' \
	    -e 's|[@]PLUGINDIR[@]|@NM_PLUGIN_DIR@|g' \
	    <$(srcdir)/nm-l2tp-service.name.in \
	    >$(DESTDIR)$(sysconfdir)/NetworkManager/VPN/nm-l2tp-service.name

uninstall-hook:
	 rm -f $(DESTDIR)$(sysconfdir)/NetworkManager/VPN/nm-l2tp-service.name
endif

appdatadir = $(datadir)/appdata
appdata_files = $(appdata_in_files:.xml.in=.xml)
if WITH_GNOME
appdata_DATA = $(appdata_files)
endif
appdata_in_files = appdata/network-manager-l2tp.metainfo.xml.in
@INTLTOOL_XML_RULE@

nm-l2tp-service.name: $(srcdir)/nm-l2tp-service.name.in
	$(AM_V_GEN) sed -e 's|[@]LIBEXECDIR[@]|$(libexecdir)|g' \
	    -e 's|[@]PLUGINDIR[@]/|@NM_PLUGIN_DIR_NAME_FILE@|g' \
	    $^ >$@

EXTRA_DIST += \
	nm-l2tp-service.name.in \
	$(dbusservice_DATA) \
	$(appdata_in_files) \
	$(appdata_files) \
	intltool-extract.in \
	intltool-merge.in \
	intltool-update.in

CLEANFILES += \
	$(nmvpnservice_DATA) \
	$(appdata_files)

DISTCLEANFILES = \
	intltool-extract \
	intltool-merge \
	intltool-update

ACLOCAL_AMFLAGS = -I m4
