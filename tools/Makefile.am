#
# Copyright (C) 2004-2009 Andrew Beekhof
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License, or (at your option) any later version.
# 
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.
#
MAINTAINERCLEANFILES    = Makefile.in 

INCLUDES                = -I$(top_builddir)/include -I$(top_srcdir)/include   \
			  -I$(top_builddir)/libltdl -I$(top_srcdir)/libltdl

COMMONLIBS	= 							\
		$(top_builddir)/lib/common/libcrmcommon.la		\
		$(top_builddir)/lib/cib/libcib.la			\
		$(CURSESLIBS) $(CLUSTERLIBS) 

headerdir               = $(pkgincludedir)/crm
header_HEADERS		= attrd.h

EXTRA_DIST		= $(sbin_SCRIPTS)

halibdir		= $(CRM_DAEMON_DIR)
halib_SCRIPTS		= haresources2cib.py hb2openais.sh
halib_PROGRAMS		= attrd pingd
halib_PYTHON		= crm_primitive.py hb2openais-helper.py

sbin_PROGRAMS		= crmadmin cibadmin crm_node crm_attribute crm_resource crm_verify \
			 crm_uuid crm_shadow attrd_updater crm_diff crm_mon iso8601

testdir			= $(datadir)/$(PACKAGE)/tests/cli
test_SCRIPTS		= regression.sh
test_DATA		= regression.exp

if BUILD_SERVICELOG
sbin_PROGRAMS		+= notifyServicelogEvent
endif
if BUILD_OPENIPMI_SERVICELOG
sbin_PROGRAMS		+= ipmiservicelogd
endif

if BUILD_HELP
man8_MANS =	$(sbin_PROGRAMS:%=%.8)
%.8:	%
	echo Creating $@
	help2man --output $@ --no-info --section 8 --name "Part of the Pacemaker cluster resource manager" $(top_builddir)/tools/$<
endif

sbin_SCRIPTS		= crm_standby crm_master crm_failcount
#sbin_SCRIPTS		= crm crm_standby crm_master crm_failcount

## SOURCES

#noinst_HEADERS		= config.h control.h crmd.h
noinst_HEADERS		= 

crmadmin_SOURCES	= crmadmin.c
crmadmin_LDADD		= $(COMMONLIBS) $(CLUSTERLIBS) \
			$(top_builddir)/lib/pengine/libpe_status.la

crm_uuid_SOURCES	= crm_uuid.c
crm_uuid_LDADD		= $(top_builddir)/lib/common/libcrmcluster.la

cibadmin_SOURCES	= cibadmin.c
cibadmin_LDADD		= $(COMMONLIBS) 

crm_shadow_SOURCES	= cib_shadow.c
crm_shadow_LDADD	= $(COMMONLIBS) 

crm_node_SOURCES	= ccm_epoche.c
crm_node_LDADD		= $(COMMONLIBS) $(CLUSTERLIBS) \
			$(top_builddir)/lib/common/libcrmcluster.la

crm_diff_SOURCES	= xml_diff.c
crm_diff_LDADD		= $(COMMONLIBS)

crm_mon_SOURCES		= crm_mon.c
crm_mon_LDADD		= $(COMMONLIBS) $(SNMPLIBS) $(ESMTPLIBS) -llrm \
			$(top_builddir)/lib/pengine/libpe_status.la

# Arguments could be made that this should live in crm/pengine
crm_verify_SOURCES	= crm_verify.c
crm_verify_LDADD	= $(COMMONLIBS) \
			$(top_builddir)/lib/pengine/libpe_status.la \
			$(top_builddir)/pengine/libpengine.la

crm_attribute_SOURCES	= crm_attribute.c
crm_attribute_LDADD	= $(COMMONLIBS)

crm_resource_SOURCES	= crm_resource.c
crm_resource_LDADD	= $(COMMONLIBS) \
			$(top_builddir)/lib/pengine/libpe_rules.la  \
			$(top_builddir)/lib/pengine/libpe_status.la \
			$(top_builddir)/pengine/libpengine.la

iso8601_SOURCES		= test.iso8601.c
iso8601_LDADD		= $(COMMONLIBS) 

attrd_SOURCES		= attrd.c
attrd_LDADD		= $(COMMONLIBS) $(top_builddir)/lib/common/libcrmcluster.la

pingd_SOURCES		= pingd.c
pingd_LDADD		= $(COMMONLIBS)

attrd_updater_SOURCES	= attrd_updater.c
attrd_updater_LDADD	= $(COMMONLIBS)

if BUILD_SERVICELOG
notifyServicelogEvent_SOURCES	= notifyServicelogEvent.c
notifyServicelogEvent_CFLAGS	= `pkg-config --cflags servicelog-1`
notifyServicelogEvent_LDFLAGS	= `pkg-config --libs servicelog-1` $(top_builddir)/lib/common/libcrmcommon.la
endif

if BUILD_OPENIPMI_SERVICELOG
ipmiservicelogd_SOURCES	= ipmiservicelogd.c
ipmiservicelogd_CFLAGS	= `pkg-config --cflags OpenIPMI OpenIPMIposix servicelog-1`
ipmiservicelogd_LDFLAGS	= `pkg-config --libs OpenIPMI OpenIPMIposix servicelog-1` $(top_builddir)/lib/common/libcrmcommon.la
endif

clean-generic:
	rm -f *.log *.debug *.xml *~

install-exec-local:

uninstall-local:

.PHONY: install-exec-hook
