# Note:  Makefile is built automatically from Makefile.in
#
# $Id: GNUmakefile.in,v 1.104.2.25 2004/01/31 01:15:33 oes Exp $
#
# Written by and Copyright (C) 2001 - 2004 the SourceForge
# Privoxy team. http://www.privoxy.org/
#
# Based on the Internet Junkbuster originally written
# by and Copyright (C) 1997 Anonymous Coders and 
# Junkbusters Corporation.  http://www.junkbusters.com
#
# This program is free software; you can redistribute it 
# and/or modify it under the terms of the GNU General
# Public License as published by the Free Software
# Foundation; either version 2 of the License, or (at
# your option) any later version.
#
# This program is distributed in the hope that it will
# be useful, but WITHOUT ANY WARRANTY; without even the
# implied warranty of MERCHANTABILITY or FITNESS FOR A
# PARTICULAR PURPOSE.  See the GNU General Public
# License for more details.
#
# The GNU General Public License should be included with
# this file.  If not, you can view it at
# http://www.gnu.org/copyleft/gpl.html
# or write to the Free Software Foundation, Inc., 59
# Temple Place - Suite 330, Boston, MA  02111-1307, USA.
#

#############################################################################
# Set make command correctly
#############################################################################


#############################################################################
# Version number (for RPM)
#############################################################################

VERSION_MAJOR = 3
VERSION_MINOR = 0
VERSION_POINT = 3
CODE_STATUS   = stable
VERSION       = $(VERSION_MAJOR).$(VERSION_MINOR).$(VERSION_POINT)
RPM_VERSION   = $(VERSION)
RPM_PACKAGEV  = ""
SNAPVERSION   = $(RPM_VERSION)-$(shell date "+%Y%m%d")


#############################################################################
# "make install" directories and variables
#############################################################################

#User Group paras
USER         = 
GROUP	   = 

prefix       = /usr/local
exec_prefix  = ${prefix}
CONF_BASE    = ${prefix}/etc
SBIN_DEST    = ${exec_prefix}/sbin
MAN_DIR      = ${prefix}/share/man
MAN_DEST     = $(MAN_DIR)/man1
SHARE_DEST   = ${prefix}/share
DOC_DEST     = $(SHARE_DEST)/doc/privoxy
VAR_DEST     = ${prefix}/var
LOGS_DEST    = $(VAR_DEST)/log/privoxy
PIDS_DEST    = $(VAR_DEST)/run

# if $prefix = /usr/local then the default CONFDEST change from 
# CONF_DEST = $(CONF_BASE) to CONF_DEST = $(CONF_BASE)/privoxy  
# by the target rule CONF_DEST
#
# also if the $prefix is /usr/local and there is no
# $(SHARE_DEST)/doc, it checks for $prefix/doc and installs there
# instead in this situation
#
# finally if $prefix=/usr/local and VAR_DEST=$prefix/var it 
# changes this to /var for storing the logs and pidfile

# used in source dir only, the install goes to $share_dest/doc/privoxy
DOK_WEB = doc/webserver/

# Install usage should be compatible with install-sh.
INSTALL    = /usr/bin/install -c
# Binaries
BIN_MODE	 = 0755
# Support files, docs, etc.
RA_MODE   = 0664
# Directory
DIR_MODE   = 0755
# Files daemon writes to.
RWD_MODE   = 0660
INSTALL_P  = -m $(BIN_MODE)  
INSTALL_T  = -m $(RA_MODE)
INSTALL_D  = -m $(DIR_MODE) -d
INSTALL_R  = -m $(RWD_MODE)

# install options for superuser install
#INSTALL_S  = -g  -o  

#############################################################################
# Build tools
#############################################################################

PROGRAM    = privoxy
CC         = gcc
ECHO       = echo
GZIP_PROG  = gzip

# id -u is not universal. FIXME: need to set from configure. Breaks on
# Solaris.
#ID         = id -u
ID         = id
LD         = gcc
RM         = rm -f
CP         = cp -f
RMDIR      = rmdir
MKDIR      = ./mkinstalldirs
STRIP_PROG = strip
SED	      = sed
GREP       = grep
CAT        = cat
RPM        = rpm
RPMBUILD   = rpmbuild
MV	      = mv
TAR        = tar
LN         = ln
TOUCH      = touch
KILL       = kill
CHMOD      = chmod
CHOWN      = chown
CHGRP      = chgrp
GROUPS     = groups
WDUMP      = false -dump
JADECAT    = 
JADEBIN    = false
DB         = $(JADEBIN) $(JADECAT) -ihtml -t sgml  -D.. -d ldp.dsl\#html
DB2HTML    = false
MAN2HTML   = false
G2H_CMD    = groff -mandoc -Thtml
TARGET_OS  = i686-pc-linux-gnu
PERL       = perl
DOC_DIR	 = doc/source
DOC_TMP    = $(DOC_DIR)/tmp
DOC_STATUS = p-stable

# Program to do LF->CRLF
#
# The sed version should be the most portable, but it doesn't for for me,
# the other two do.  FIXME.
#   - Jon
#DOSFILTER  = $(SED) -e $$'s,$$,\r,'
#DOSFILTER  = gawk -v ORS='\r\n' '{print $0;}'
DOSFILTER  = $(PERL) -p -e 's/\n/\r\n/'
CVSROOT    = :pserver:anonymous@cvs.ijbswa.sourceforge.net:/cvsroot/ijbswa
TMPDIR     := $(shell mktemp -d /tmp/$(PROGRAM).XXXXXX)

#############################################################################
# Setup for make distribution rh and suse for now 
#############################################################################

TAR_ARCH = /tmp/privoxy-$(RPM_VERSION).tar.gz
RPM_BASE = 

#############################################################################
# We include these files in our distributions
#############################################################################
CONFIGS = config trust default.action standard.action user.action default.filter
# take care that no CVS .cvsignore or other crappy files
# are included here
# and escape every '#' in the find. doh.
CONFIG_FILES = $(CONFIGS) \
		`find templates/ -type f | grep -v "CVS" | grep -v "\.\#" | grep -v ".*~" | grep -v ".cvsignore" | grep -v "TAGS"`

DOC_FILES = AUTHORS LICENSE README ChangeLog INSTALL \
		`find doc/text/ -type f | grep -v "CVS" | grep -v "\.\#" | grep -v ".*~" | grep -v ".cvsignore" | grep -v "TAGS"` \
		`find doc/webserver/ -name "*.html" | grep -v "\(webserver\|team\)\/index\.html"` \
		`find doc/webserver/ -name "*.css"` \
                privoxy.1 \
		doc/pdf/*.pdf

#############################################################################
# Filenames and libraries
#############################################################################

C_SRC  = actions.c cgi.c cgiedit.c cgisimple.c deanimate.c encode.c \
         errlog.c filters.c gateway.c jbsockets.c jcc.c killpopup.c \
         list.c loadcfg.c loaders.c miscutil.c parsers.c ssplit.c \
         urlmatch.c

C_OBJS = $(C_SRC:.c=.o)
C_HDRS = $(C_SRC:.c=.h) project.h actionlist.h

W32_SRC   = #w32log.c w32taskbar.c win32.c
W32_FILES = #w32.res
W32_OBJS  = #$(W32_SRC:.c=.o) $(W32_FILES)
W32_HDRS  = #w32log.h w32taskbar.h win32.h w32res.h
W32_LIB   = #-lwsock32 -lcomctl32
W32_INIS  = #config.txt trust.txt

PCRS_SRC     = pcrs.c
PCRS_OBJS    = $(PCRS_SRC:.c=.o)
PCRS_HDRS    = $(PCRS_SRC:.c=.h)

PCRE_SRC     = pcre/get.c pcre/maketables.c pcre/study.c pcre/pcre.c
PCRE_OBJS    = $(PCRE_SRC:.c=.o)
PCRE_HDRS    = pcre/config.h pcre/chartables.c pcre/internal.h pcre/pcre.h

# No REGEX (maybe because dynamically linked pcreposix):
REGEX_SRC    =
REGEX_SRC = pcre/pcreposix.c

REGEX_OBJS   = $(REGEX_SRC:.c=.o)
REGEX_HDRS   = $(REGEX_SRC:.c=.h)

# Dependencies introduced by #include "project.h".
PROJECT_H_DEPS = project.h $(REGEX_HDRS) $(PCRS_HDRS) pcre/pcre.h

# Socket libraries for platforms that need them explicitly defined
SOCKET_LIB   = 

# PThreads library, if needed.
PTHREAD_LIB  = 

SRCS         = $(C_SRC)  $(W32_SRC)  $(PCRS_SRC)  $(PCRE_SRC)  $(REGEX_SRC)
OBJS         = $(C_OBJS) $(W32_OBJS) $(PCRS_OBJS) $(PCRE_OBJS) $(REGEX_OBJS)
HDRS         = $(C_HDRS) $(W32_HDRS) $(PCRS_HDRS) $(PCRE_OBJS) $(REGEX_HDRS)
LIBS         = -lnsl  $(W32_LIB) $(SOCKET_LIB) $(PTHREAD_LIB)


#############################################################################
# Compiler switches
#############################################################################

# The flag "-mno-win32" can be used by Cygwin to emulate a un?x type build.
# The flag "-mwindows -mno-cygwin" will cause Cygwin to use MingW32 for a
# Win32 GUI build.
# The flag "-pthread" is required if using Pthreads under Linux (and
# possibly other OSs).
SPECIAL_CFLAGS = -pthread

# Add your flags here 
OTHER_CFLAGS =   

CFLAGS = -pipe -O2  $(OTHER_CFLAGS) $(SPECIAL_CFLAGS) -Wall \
          -Ipcre 

LDFLAGS = $(DEBUG_CFLAGS) $(SPECIAL_CFLAGS)


#############################################################################
# Build section.
#
# There should NOT be any targets above this line.
#############################################################################
all: $(PROGRAM) default.action


#############################################################################
# Phony targets
#############################################################################
.PHONY: all inifiles redhat-dist redhat-upload solaris-dist suse-dist \
suse-upload win-dist tarball-dist dok redhat-dok webserver clean clobber tags \
install conectiva-spec conectiva-dist conectiva-upload CONF_DEST LOG_DEST \
PID_DEST check_doc install-strip uninstall GROUP_T

#############################################################################
# Define this explicitly because Solaris is broken!
#############################################################################
%.o: %.c
	$(CC) -c $(CFLAGS) $(CPPFLAGS) $< -o $@


#############################################################################
# Strip master copy comments from default.action:
#############################################################################
default.action: default.action.master
	$(GREP) -v '^#MASTER#' $< > $@

#############################################################################
# Win32 config files
#############################################################################

inifiles: $(W32_INIS)

config.txt: config
	$(SED) -e 's!\trustfile trust!trustfile trust.txt!' \
	       -e 's!\jarfile jarfile!jarfile jar.log!' \
	       -e 's!\logfile logfile!logfile privoxy.log!' \
	       -e 's!#Win32-only: !!' \
	       < $< | \
	       $(DOSFILTER) > $@
	# LF to CRLF in default.action
	$(DOSFILTER) <default.action >default.action.txt && mv default.action.txt default.action
	# LF to CRLF in default.filter
	$(DOSFILTER) <default.filter >default.filter.txt && mv default.filter.txt default.filter

trust.txt: trust
	$(DOSFILTER) < $< > $@ 

#############################################################################
# Pre-dist check:
#############################################################################
dist-check:
	@if [ -d CVS ]; then \
           $(ECHO) "***************************************************"; \
           $(ECHO) "***                                             ***"; \
           $(ECHO) "***                  WARNING                    ***"; \
           $(ECHO) "***                                             ***"; \
           $(ECHO) "*** The presence of a CVS subdirectory suggests ***"; \
           $(ECHO) "*** that you are trying to build a distribution ***"; \
           $(ECHO) "*** package based on a checked out, not an      ***"; \
           $(ECHO) "*** exported copy of the source tree. Please    ***"; \
           $(ECHO) "*** see \"Releasing a new version\" in the        ***"; \
           $(ECHO) "*** developer manual.                           ***"; \
           $(ECHO) "***                                             ***"; \
           $(ECHO) "***************************************************"; \
           $(ECHO) "Type \"yes i am sure\" if you are sure that you"; \
           $(ECHO) -n "really want to proceed: "; \
           read answer; \
           if [ "$$answer" != "yes i am sure" ]; then exit 1; fi \
         fi;


#############################################################################
# create tar.gz from CVS:
# This make-target is usually called through 'create-archive'. If you 
# run 'make create-snapshot' without setting SNAPVERSION, you'll get a
# tar.gz with the current date in the name and as a releasenumber in the 
# spec-file. But the main usage is to run it as follows (Red Hat example):
# make SNAPVERSION=1.6x create-snapshot
# This creates a tar.gz and spec-file for a Red Hat 6.x version.
#############################################################################
create-snapshot:
	@tag=`cvs -d $(CVSROOT) status Makefile | awk ' /Sticky Tag/ { print $$3 } '` 2> /dev/null; \
	[ x"$$tag" = x"(none)" ] && tag=HEAD; \
	echo "*** Creating package from $$tag!"; \
	cd $(TMPDIR) ; cvs -Q -d $(CVSROOT) export -r $$tag current || echo "Um... export aborted."
	@cd $(TMPDIR)/current; \
	TMPFILE=$$(mktemp -q /tmp/$(PROGRAM).XXXXXX); \
	if $(SED) -e 's/^\(Version:\).*/\1 $(RPM_VERSION)/g' \
		-e 's/^\(Release:\).*/\1 $(SNAPVERSION)/g' \
		privoxy-rh.spec > $$TMPFILE ; then \
			$(MV) -f $$TMPFILE privoxy-rh.spec; \
	else \
		$(ECHO) "Could not set version info in specfile."; \
		exit 1;\
	fi;\
	if $(SED) -e 's/^\(Version:\).*/\1 $(RPM_VERSION)/g' \
             -e 's/^\(Release:\).*/\1 $(SNAPVERSION)/g' \
              privoxy-suse.spec > $$TMPFILE ; then \
			$(MV) -f $$TMPFILE privoxy-suse.spec; \
	else \
		$(ECHO) "Could not set version info in specfile."; \
		exit 1;\
	fi; \
	$(RM) $(TMPFILE); \
	cd $(TMPDIR)/current; \
	$(TAR) --exclude ".cvsignore" --exclude "CVS" --exclude \
		"privoxy-suse.spec" -czf $(TMPDIR)/$(PROGRAM)-rh-$(VERSION).tar.gz .; \
	$(TAR) --exclude ".cvsignore" --exclude "CVS" --exclude \
		"privoxy-rh.spec" -czf $(TMPDIR)/$(PROGRAM)-suse-$(VERSION).tar.gz .
	@$(MV) -f $(TMPDIR)/$(PROGRAM)-rh-$(VERSION).tar.gz .
	@$(MV) -f $(TMPDIR)/$(PROGRAM)-suse-$(VERSION).tar.gz .
	@$(RM) -rf $(TMPDIR)
	@echo "Resulting files are $(PROGRAM)-rh-$(VERSION).tar.gz and"
	@echo "                    $(PROGRAM)-suse-$(VERSION).tar.gz"


#############################################################################
# looks at the version of Makefile and exports a corresponding source-tree
# example: if the Makefile has the sticky tag v_2_9_13, you'll get 
# privoxy-*-2.4.13.tar.gz. Two different tar files will be written, one for
# Red Hat and one for SuSe (different spec-files)
#############################################################################
create-archive:
	make SNAPVERSION=$(SNAPVERSION) create-snapshot

#############################################################################
# RPM specifice stuff (SuSE or Redhat, ..)
#############################################################################
rpm-stuff: dist-check clean clobber 
	for dir in RPMS SRPMS BUILD SOURCES SPECS; do \
		if [ ! -w $(RPM_BASE)/$$dir ]; then \
			$(ECHO) "$(RPM_BASE)/$$dir is not writable for you. Maybe try as root."; \
			$(ECHO) "Or add a suitable path to .rpmmacros like."; \
			$(ECHO) "%_topdir /home/foo/rpm-build"; \
			exit 1; \
		fi; \
	done; \

check-release:
	@if [ "$(RPM_PACKAGEV)" = "" ]; then \
		echo ; \
		echo "	ERROR: NO RPM_PACKAGEV VALUE"; \
		echo "	No value given for RPM_PACKAGEV. Please use:"; \
		echo "	make dist-upload RPM_PACKAGEV=release"; \
		echo "	where \"release\" is the release number you want to and"; \
		echo "	where \"dist\" is the name of the distro (redhat or suse)"; \
		echo ; \
		echo "	Ex: make redhat-upload RPM_PACKAGEV=1"; \
		echo ""; \
		echo "ATTENTION: If your distribution use a specific tag on the"; \
		echo "           release field (like \"cl\" for Conectiva, and"; \
		echo "           \"mdk\" for Mandrake), DO NOT put it on the value"; \
		echo "           given to RPM_PACKAGEV. It will be added automaticaly."; \
		echo "           Do it like you would do for a redhat package,"; \
		echo "           (i.e. just the number)."; \
		echo ; \
		exit 1; \
	fi


#############################################################################
# Create Conectiva specfile from RedHat specfile
#############################################################################
conectiva-spec:
	$(RM) privoxy-cl.spec
	chmod a+x genclspec.sh
	./genclspec.sh

#############################################################################
# Conectiva distribution for x86
#############################################################################
conectiva-dist: rpm-stuff conectiva-spec

	$(TAR) --exclude ".cvsignore" --exclude "CVS" --exclude "privoxy-suse.spec" --exclude "privoxy-rh.spec" --exclude "PACKAGERS" -czf $(TAR_ARCH) .
	$(RPMBUILD) --clean -ta  $(TAR_ARCH)
	if [ -f $(TAR_ARCH) ]; then  $(RM) $(TAR_ARCH); fi

conectiva-upload: check-release
	make redhat-upload RPM_PACKAGEV=$(RPM_PACKAGEV)cl

#############################################################################
# redhat distribution alpha and x86
#############################################################################
redhat-dist: rpm-stuff
	echo $(CONFIG_FILES)
	$(TAR) --exclude ".cvsignore" --exclude "CVS" --exclude "privoxy-suse.spec" --exclude "privoxy-cl.spec" --exclude "PACKAGERS" -czf $(TAR_ARCH) .
	$(RPMBUILD) --clean -ta  $(TAR_ARCH)
	if [ -f $(TAR_ARCH) ]; then  $(RM) $(TAR_ARCH); fi

# For testing build issues only! Use redhat-dist for official releases.
redhat-test: 
	echo $(CONFIG_FILES)
	$(TAR) --exclude ".cvsignore" --exclude "CVS" --exclude "privoxy-suse.spec" --exclude "privoxy-cl.spec" --exclude "PACKAGERS" -czf $(TAR_ARCH) .
	$(RPMBUILD) --clean -tb  $(TAR_ARCH)
	if [ -f $(TAR_ARCH) ]; then  $(RM) $(TAR_ARCH); fi
	@echo "WARNING: This target is only for testing. Use redhat-dist for releases!!!"

# anonymously ncftps the rpms to sourceforge
redhat-upload: check-release
	ncftpput -u anonymous -p ijbswa-developers@lists.sourceforge.net upload.sourceforge.net /incoming $(RPM_BASE)/SRPMS/privoxy-$(RPM_VERSION)-$(RPM_PACKAGEV).src.rpm
# better should use `arch` here instead of ix86 to support other platforms too
	ncftpput -u anonymous -p ijbswa-developers@lists.sourceforge.net upload.sourceforge.net /incoming $(RPM_BASE)/RPMS/*/privoxy-$(RPM_VERSION)-$(RPM_PACKAGEV).*.rpm
	@$(ECHO) -------------------------------------------------------
	@$(ECHO) Now goto
	@$(ECHO) http://sourceforge.net/project/admin/editpackages.php?group_id=11118
	@$(ECHO) ... and release the files.
	@$(ECHO) -------------------------------------------------------
     # w3m http://sourceforge.net/project/admin/editpackages.php?group_id=11118


#############################################################################
# Creates a Red Hat sourcepackage from CVS (not from the current sources
# on disk)
#############################################################################
redhat-srpm: 
	make create-archive
	$(MV) $(PROGRAM)-rh-$(VERSION).tar.gz $(PROGRAM)-$(VERSION).tar.gz
	$(RPMBUILD) -ts --nodeps $(PROGRAM)-$(VERSION).tar.gz


#############################################################################
# suse distribution. works fine. no need to be root. 
#############################################################################
suse-dist: rpm-stuff
# 	TMPFILE=$$(mktemp -q /tmp/$(PROGRAM).XXXXXX); \
# 	if $(SED) -e 's/^\(Version:\).*/\1 $(RPM_VERSION)/g' \
#              -e 's/^\(Release:\).*/\1 $(RPM_PACKAGEV)/g' \
#               privoxy-suse.spec > $$TMPFILE ; then \
# 	$(MV) -f $$TMPFILE privoxy-suse.spec; \
# 	else \
# 		$(ECHO) "Could not set version info in specfile."; \
# 	exit 1;\
# 	fi

	$(TAR) --exclude ".cvsignore" --exclude "CVS" --exclude "privoxy-rh.spec" --exclude "privoxy-cl.spec" --exclude "PACKAGERS" -czf $(TAR_ARCH) .
	$(RPM) --clean -ta  $(TAR_ARCH)
	if [ -f $(TAR_ARCH) ]; then  $(RM) $(TAR_ARCH); fi

# anonymously ncftps the rpms to sourceforge
suse-upload: check-release
	ncftpput -u anonymous -p ijbswa-developers@lists.sourceforge.net upload.sourceforge.net /incoming $(RPM_BASE)/SRPMS/privoxy-suse-$(RPM_VERSION)-$(RPM_PACKAGEV).src.rpm
# better should use `arch` here instead of ix86 to support other platforms too
	ncftpput -u anonymous -p ijbswa-developers@lists.sourceforge.net upload.sourceforge.net /incoming $(RPM_BASE)/RPMS/*/privoxy-suse-$(RPM_VERSION)-$(RPM_PACKAGEV).*.rpm
	@$(ECHO) -------------------------------------------------------
	@$(ECHO) Now goto
	@$(ECHO) http://sourceforge.net/project/admin/editpackages.php?group_id=11118
	@$(ECHO) ... and release the files.
	@$(ECHO) -------------------------------------------------------

# handle with care. use with root.
suse-clean:
	$(RPM) -e junkbuster-suse || true
	$(RM) -r /etc/junkbuster
	$(RM) -r /etc/rc.d/junkbuster*
	$(RM) -r /var/run/junkbuster.pid 
	$(RM) -r /var/log/junkbuster
	$(RM) /etc/init.d/junkbuster
	$(RM) /usr/sbin/junkbuster
	$(RM) /usr/sbin/rcjunkbuster
	$(RM) /usr/share/man/man1/junkbuster.1.gz
	$(RPM) -e privoxy-suse || true
	$(RM) -r /etc/privoxy
	$(RM) -r /etc/rc.d/privoxy*
	$(RM) -r /var/run/privoxy.pid 
	$(RM) -r /var/log/privoxy
	$(RM) /etc/init.d/privoxy
	$(RM) /usr/sbin/privoxy
	$(RM) /usr/sbin/rcprivoxy
	$(RM) /usr/share/man/man1/privoxy.1.gz

#############################################################################
# generic distribution
#############################################################################
gen-dist: dist-check
	@$(ECHO) ""
	@$(ECHO) "You have run autoconf && autoheader && ./configure right?"
	@$(ECHO) ""
	$(MAKE) $(PROGRAM)
	$(STRIP_PROG) $(PROGRAM)
	$(LN) -s current ../privoxy-$(VERSION)-$(CODE_STATUS)
# add program
	(cd .. && $(TAR) -cvhf --exclude "PACKAGERS" privoxy-$(TARGET_OS)-$(VERSION)-$(CODE_STATUS)-src.tar privoxy-$(VERSION)-$(CODE_STATUS)/$(PROGRAM))
# add config files
	for foo in $(CONFIG_FILES); do \
		(cd .. && $(TAR) -uvhf --exclude "PACKAGERS" privoxy-$(TARGET_OS)-$(VERSION)-$(CODE_STATUS)-src.tar privoxy-$(VERSION)-$(CODE_STATUS)/$$foo;) \
	done; 
# add documentation
	for foo in $(DOC_FILES); do \
		(cd .. && $(TAR) -uvhf --exclude "PACKAGERS" privoxy-$(TARGET_OS)-$(VERSION)-$(CODE_STATUS)-src.tar privoxy-$(VERSION)-$(CODE_STATUS)/$$foo;) \
	done;
# and zip the archive
	$(RM) ../privoxy-$(VERSION)-$(CODE_STATUS)
	$(GZIP_PROG) ../privoxy-$(TARGET_OS)-$(VERSION)-$(CODE_STATUS)-src.tar
	@$(ECHO) Distribution with binary created.

# anonymously ncftps the package to sourceforge
gen-upload:
	ncftpput -u anonymous -p ijbswa-developers@lists.sourceforge.net upload.sourceforge.net /incoming ../privoxy-$(TARGET_OS)-$(VERSION)-$(CODE_STATUS)-src.tar.gz
	@$(ECHO) -------------------------------------------------------
	@$(ECHO) Now goto
	@$(ECHO) http://sourceforge.net/project/admin/editpackages.php?group_id=11118
	@$(ECHO) ... and release the files.
	@$(ECHO) -------------------------------------------------------

# use with care
gen-clean:
	$(RM) privoxy-$(TARGET_OS)-$(VERSION)-$(CODE_STATUS)-src.tar*

#############################################################################
# solaris distribution. verified on SF machines by swa.
#############################################################################
solaris-dist: gen-dist
	@$(ECHO) Done.
# anonymously ncftps the package to sourceforge
solaris-upload: gen-upload
	@$(ECHO) Done.
# use with care
solaris-clean: gen-clean
	@$(ECHO) Done.

#############################################################################
# hpux distribution
#############################################################################
hpux-dist:
	@$(ECHO) coming soon. 
hpux-upload:
	@$(ECHO) coming soon. 

#############################################################################
# debian distribution
#############################################################################
debian-dist:
	@$(ECHO) coming soon. 
debian-upload:
	@$(ECHO) coming soon. 

#############################################################################
# macosx distribution
#############################################################################
macosx-dist:
	@$(ECHO) coming soon. 
macosx-upload:
	@$(ECHO) coming soon. 

#############################################################################
# amiga distribution
#############################################################################
amiga-dist:
	@$(ECHO) coming soon. 
amiga-upload:
	@$(ECHO) coming soon. 

#############################################################################
# freebsd distribution. verified on SF machines by swa.
#############################################################################
freebsd-dist: gen-dist
	@$(ECHO) Done.
# anonymously ncftps the package to sourceforge
freebsd-upload: gen-upload
	@$(ECHO) Done.
# use with care
freebsd-clean: gen-clean
	@$(ECHO) Done.

#############################################################################
# Windows distribution
#############################################################################
win-dist:
	$(ECHO) Not implemented.


#############################################################################
# Tarball distribution: No CVS dirs, dotfiles, debian build dir,
# (FIXME:) only parts of the static / generated docs mix in doc/webserver
#############################################################################

tarball-dist: dist-check clean clobber
	$(LN) -s current ../privoxy-$(VERSION)-$(CODE_STATUS)

	for i in `find . -type f -a -not \( -path "*/CVS*" -o -name ".*" \
	-o -path "*/debian/*" -o -path "*/actions/*" -o -name "*.php" -o \
	-name "PACKAGERS" -o -path "*/pdf/*" \)`; do \
	   files="$$files privoxy-$(VERSION)-$(CODE_STATUS)/$$i"; \
	done &&  \
	cd .. && $(TAR) -cvhf privoxy-$(VERSION)-$(CODE_STATUS)-src.tar $$files ; \

# and zip the archive
	$(RM) ../privoxy-$(VERSION)-$(CODE_STATUS) 
	$(GZIP_PROG) ../privoxy-$(VERSION)-$(CODE_STATUS)-src.tar
	@$(ECHO) Tarball distribution created.

# anonymously ncftps the tarball to sourceforge
tarball-upload:
	ncftpput -u anonymous -p ijbswa-developers@lists.sourceforge.net upload.sourceforge.net /incoming ../privoxy-$(VERSION)-$(CODE_STATUS)-src.tar.gz
	@$(ECHO) -------------------------------------------------------
	@$(ECHO) Now goto
	@$(ECHO) http://sourceforge.net/project/admin/editpackages.php?group_id=11118
	@$(ECHO) ... and release the files.
	@$(ECHO) -------------------------------------------------------

tarball-clean:
	$(RM) ../privoxy-$(VERSION)-$(CODE_STATUS)-src.tar.gz

#############################################################################
#
# Documentation
#
# converts doc/source/*.sgml into html, text and man pages
#
#############################################################################

# developer manual
dok-devel: 
	$(RM) doc/webserver/developer-manual/*.html
	$(RM) -r doc/source/developer-manual
	mkdir -p doc/text doc/source/developer-manual
	cd doc/source/developer-manual && $(DB) ../developer-manual.sgml && cd .. && cp developer-manual/*.html ../webserver/developer-manual/
	cd doc/source && $(DB) -V nochunks developer-manual.sgml > tmp.html && $(WDUMP) tmp.html > ../text/developer-manual.txt && $(RM) -r tmp.html developer-manual

# user manual
dok-user: 
	$(RM) doc/webserver/user-manual/*.html
	$(RM) -r doc/source/user-manual/
	mkdir -p doc/text doc/source/user-manual
	cd doc/source/user-manual && $(DB) -iuser-man ../user-manual.sgml && cd .. && cp user-manual/*.html ../webserver/user-manual/
	cd doc/source && $(DB) -iuser-man -V nochunks user-manual.sgml > tmp.html && $(WDUMP) tmp.html > ../text/user-manual.txt && $(RM) -r tmp.html user-manual

# faq
dok-faq: 
	$(RM) doc/webserver/faq/*.html
	$(RM) -r doc/source/faq
	mkdir -p doc/text doc/source/faq
	cd doc/source/faq && $(DB) ../faq.sgml && cd .. && cp faq/*.html ../webserver/faq/
	cd doc/source && $(DB) -V nochunks faq.sgml > tmp.html && $(WDUMP) tmp.html > ../text/faq.txt && $(RM) -r tmp.html faq

# man page, one variation. Try to use the next target, just 'make man'. 
dok-man: 
	$(RM) doc/man/* doc/webserver/man-page/*.html
ifneq ($(MAN2HTML),false)
	$(ECHO) "<html><head><title>Privoxy Man page</title><link rel=\"stylesheet\" type=\"text/css\" href=\"../p_web.css\"></head><body><H2>NAME</H2>" > doc/webserver/man-page/privoxy-man-page.html
	man ./privoxy.1 | $(MAN2HTML) -bare >> doc/webserver/man-page/privoxy-man-page.html
	$(ECHO) "</body></html>" >> doc/webserver/man-page/privoxy-man-page.html
else
	$(MAKE) groff2html
endif

# Build man page from sgml. This requires the SGMLSpm perl module.
# See CPAN, or your favorite perl repository. This is the preferred 
# target for man page generation!
man: dok-release
	mkdir -p doc/source/temp && cd doc/source/temp && $(RM) * ;\
	nsgmls ../privoxy-man-page.sgml  | sgmlspl ../../../utils/docbook2man/docbook2man-spec.pl &&\
	perl -pi.bak -e 's/ <URL:.*>//; s/\[ /\[/g' privoxy.1 ;\
	$(DB) ../privoxy-man-page.sgml && $(MV) -f privoxy.1 ../../../privoxy.1

# For those with man2html ala RH7s.
man2html:
	mkdir -p doc/webserver/man-page
ifneq ($(MAN2HTML),false)
	$(MAN2HTML) privoxy.1 |grep -v "^Content-type" > tmp.html
	$(PERL) -pi.bak -e 's/<A .*Contents<\/A>//; s/<A .*man2html<\/A>/man2html/' tmp.html
	$(PERL) -pi.bak -e 's/(<\/HEAD>)/<LINK REL=\"STYLESHEET\" TYPE=\"text\/css\" HREF=\"..\/p_doc.css\"><\/HEAD>/' tmp.html
# Twice because my version of man2html is pulling in commas and periods in URLs.
	$(PERL) -pi.bak -e 's/(<A.*),(">)/$$1$$2/g' tmp.html
	$(PERL) -pi.bak -e 's,\.">,">,g' tmp.html
# Get rid of spurious  from conversion. (How to do this with perl?)
	$(SED) -e 's///g' tmp.html > doc/webserver/man-page/privoxy-man-page.html && $(RM) tmp.*
else
	$(MAKE) groff2html
endif


# Otherwise we get plain groff conversion.
groff2html:
	$(G2H_CMD) ./privoxy.1 | $(SED) -e 's@</head>@<link REL="STYLESHEET" TYPE="text/css" HREF="../p_doc.css"></head>@' > doc/webserver/man-page/privoxy-man-page.html


# readme page and INSTALL file
dok-readme: dok-release
	cd doc/source && $(DB)-notoc -V nochunks readme.sgml > tmp.html &&\
	$(WDUMP) tmp.html > ../../README ;\
	$(DB)-notoc -V nochunks install.sgml > tmp.html &&\
	$(WDUMP) tmp.html > ../../INSTALL ;\
	$(RM) tmp.*

# index.sgml is used to create both the Home Page, and a local index
# for documentation, etc.
#
# index.html for webserver:
dok-webserver: 
	cd doc/source/webserver && $(DB)-notoc -ip-homepage -V nochunks index.sgml > ../../webserver/index.html
	$(PERL) -pi.bak -e 's/..\/p_doc.css/p_doc.css/;\
     s/<\/HEAD/\n<meta name=\"description\" content=\"Privoxy helps consumers reduce unwanted junk email and protect their privacy from direct marketing companies.\"><\/HEAD/;\
	s/<\/HEAD/\n<meta name="MSSmartTagsPreventParsing" content="TRUE"><\/HEAD/;\
	s/\.\d\. //;\
	s/__copy/&copy;/'\
     doc/webserver/index.html && $(RM) doc/webserver/*.bak

# privoxy-index.html for local documentation:
dok-index: 
	cd doc/source/webserver && $(DB)-notoc -ip-index -V nochunks index.sgml > ../../webserver/privoxy-index.html
	$(PERL) -pi.bak -e 's/..\/p_doc.css/p_doc.css/;\
     s/<\/HEAD/\n<meta name=\"description\" content=\"Privoxy helps consumers reduce unwanted junk email and protect their privacy from direct marketing companies.\"><\/HEAD/;\
	s/<\/HEAD/\n<meta name="MSSmartTagsPreventParsing" content="TRUE"><\/HEAD/;\
	s/\.\d\. //;\
	s/__copy/&copy;/' \
     doc/webserver/privoxy-index.html && $(RM) doc/webserver/*.bak

# Main documentation target.
dok: dok-release dok-devel dok-user dok-faq dok-readme dok-webserver dok-authors dok-index
	@$(ECHO) Documentation created.

#
# an alternative to the above dok. disabled man page creation for the moment
#
redhat-dok: dok-release dok-devel dok-user dok-faq redhat-readme dok-webserver dok-authors
	@$(ECHO) Documentation created.

## Make README
redhat-readme: 
	cd doc/source && $(DB)-notoc -V nochunks readme.sgml > tmp.html && $(WDUMP) \
	  tmp.html > ../../README && $(RM) -r tmp.html

## Make AUTHORS file
dok-authors: 
	cd doc/source && $(DB) -V nochunks authors.sgml > tmp.html && $(WDUMP) \
	  tmp.html > ../../AUTHORS && $(RM) tmp.html

# Set doc entities for VERSION and CODE_STATUS in sgml docs. Toggle content
# exceptions accordingly. This needs to go before any doc building (doh).
dok-release:
	@$(ECHO) Setting doc version and status to $(VERSION), $(CODE_STATUS)
	@$(PERL) -pi.bak -e 's/<!entity +p-version.*>/<!entity p-version "$(VERSION)">/;\
     s/<!entity +p-status.*>/<!entity p-status "$(CODE_STATUS)">/' \
     doc/source/*sgml doc/source/*/*sgml
	$(RM) -r doc/source/*bak doc/source/*/*bak
ifeq ($(CODE_STATUS),stable)
	@$(ECHO) Setting docs to stable $(VERSION)
	@$(PERL) -pi.bak -e 's/<!entity +% +p-stable.*>/<!entity % p-stable "INCLUDE">/;\
     s/<!entity +% +p-not-stable.*>/<!entity % p-not-stable "IGNORE">/' \
     doc/source/*sgml doc/source/*/*sgml
	$(RM) -r doc/source/*bak doc/source/*/*bak
else
	@$(ECHO) Setting docs to not stable $(VERSION)
	@$(PERL) -pi.bak -e 's/<!entity +% +p-stable.*>/<!entity % p-stable "IGNORE">/;\
     s/<!entity +% +p-not-stable.*>/<!entity % p-not-stable "INCLUDE">/' \
     doc/source/*sgml doc/source/*/*sgml
	$(RM) -r doc/source/*bak doc/source/*/*bak
endif

# Generate single page html. Used only for creating pdf docs (ATM).
# Currently using: See http://www.easysw.com/htmldoc/pdf-o-matic.php.
# If using this generator, remember U-M has a couple of graphics in 
# a parallel directory.
#
dok-shtml: dok-release 
	mkdir -p doc/source/temp # this directory not in cvs
	cd doc/source && $(DB) -iuser-man -V nochunks user-manual.sgml > temp/privoxy-user-manual.html
	cd doc/source && $(DB) -V nochunks developer-manual.sgml > temp/privoxy-developer-manual.html
	cd doc/source && $(DB) -V nochunks faq.sgml > temp/privoxy-faq.html
# one could use html2ps and ps2pdf. well, that does not work. htmlps produces incorrect output.

# Make pdf docs from single page html. Requires htmldoc, see
# (http://www.easysw.com/htmldoc/). Note: 1.8.20 has a TOC bug.
# PDF docs are uploaded to webserver as zip archive.
dok-pdf: dok-shtml
	@$(ECHO) -n "starting htmldoc version: "; htmldoc --version
	cd utils/ldp_print && $(RM) *html *bak *jpg *tmp *pdf *zip
	cp -f doc/source/temp/*html doc/webserver/images/*jpg utils/ldp_print
	cd utils/ldp_print ;\
	$(PERL) -pi.bak -e 's/\.\.\/images\///; s/(<\/?)SUB/$$1small/i;\
	                    s/\.\.\/user-manual\/index\.html/privoxy-user-manual.pdf/;\
					s/\.\.\/developer-manual\/index\.html/privoxy-developer-manual.pdf/;\
					s/\.\.\/faq\/index\.html/privoxy-faq.pdf/' *.html ;\
	for i in developer-manual user-manual faq; do \
		./ldp_print privoxy-$$i.html ;\
		$(ECHO) DONE: privoxy-$$i.pdf ;\
	done ;\
	$(MV) *.pdf  ../../doc/pdf ;\
	$(RM) -r *html *bak *jpg *pdf *zip ../../doc/source/temp

# Create release announcement in text and html, with short and long versions.
# This is a standalone target, and must be invoked directly.
# announce: dok-release
# 	mkdir -p $(DOC_TMP)
# 	cd $(DOC_TMP) && cp -f ../announce.sgml . && $(DB) -iannounce-big announce.sgml &&\
# 	mv -f index.html announce.html && $(WDUMP) announce.html > announce.txt
# 	cd $(DOC_TMP) && $(DB) announce.sgml &&\
# 	mv -f index.html announce-mini.html && $(WDUMP) announce-mini.html > announce-mini.txt &&\
# 	mv -f *html *txt ../../.. 
# 	rm -fr $(DOC_TMP)

# The main Privoxy config file, generated from sgml sources. 
# NOTE: This will require some hand editing. The new file is outputted 
# as config.new so that problem sections can be compared to previous
# version. This is hardcored to w3m for html/text conversion. Also, 
# requires the shell util 'fmt'.
config-file: dok-release
	cd doc/source && $(DB)-notoc -iconfig-file -V nochunks config.sgml > __tmp.html &&\
	w3m -dump __tmp.html |fmt -w 70 > ../../config.new && $(RM) -r __tmp.*
	$(PERL) -pi.bak -e 's/^1\. \@\@TITLE\@\@/     /i;\
                     /^\d\.\d\.\s+/ && tr/[a-z]/[A-Z]/;\
                     $$header_len=0 unless $$hit_header;\
                     if ($$hit_header) {\
                        print "#  ";\
                        for ($$i=1; $$i < $$header_len; $$i++) {print "=";}\
                        print "\n";\
                     };\
                     $$hit_header=0;\
                     $$hit_header=1 if m/^(\d\.)(\d\.)(\d\.)?\s/ && s/^(\d\.)//;\
                     $$header_len = length($$_);\
				 s/^/#  /;  /^#  #{12,}/ && s/^#  #/####/;\
                     s/^.*$$// if $$hit_option;\
                     $$hit_option=0;\
                     s/^\n//;  s/^#\s*-{20,}//; s/ *$$//;\
                     $$hit_option=1 if s/^#\s+@@//;'   config.new
	$(RM) *.bak
	@$(ECHO)  "****************************************************"
	@$(ECHO)  "The output file is config.new."
	@$(ECHO)  "Now -- you need to hand edit the results!!!"
	@$(ECHO)  "In particular, check the Debug levels, the"
	@$(ECHO)  "permit-access, forward & socks examples and the"
	@$(ECHO)  "various user-manual examples, which all"
	@$(ECHO)  "probably got hammered."
	@$(ECHO)  "****************************************************"

# config file, alternate verison using lynx (perl stuff unfinished). Lynx
# does not do so good a job.
config-file-alt: 
	cd doc/source && $(ECHO) -e ".h2 JUSTIFY\\nJUSTIFY:FALSE" > __tmp.lynx_cfg &&\
	$(DB)-notoc -iconfig-file -V nochunks config.sgml > __tmp.html &&\
	lynx -cfg=__tmp.lynx_cfg -width=78 -dump __tmp.html > ../../config.new && $(RM) -r __tmp.*
	$(PERL) -pi -e 's/^(   )//;\
	                s/:$\/:\n/' config.new

#############################################################################
#
# Webserver
#
# moves dokumentation to webserver
#
#############################################################################
webserver: tidy
	@$(ECHO) -------------------------------------------------------
	@$(ECHO) You have run make dok/redhat-dok before, right?
	@$(ECHO) Note that this command scps all stuff to the webserver,
	@$(ECHO) it will not remove obsolete documents.
	@$(ECHO) -------------------------------------------------------

	@$(ECHO) Uploading html
	@cd doc/webserver; \
          upload=`find . -type f -a -not \( -path "*/CVS*" -o -path "*/results*" \)`; \
          $(TAR) c $$upload | ssh ijbswa.sourceforge.net 'cd /home/groups/i/ij/ijbswa/htdocs/; tar xvm 2>&1 | grep -v timestamp'

	@$(ECHO) Uploading pdf
	@cd doc/pdf;\
          zip privoxy-pdf-docs *.pdf  ;\
		scp -q privoxy-pdf-docs.zip ijbswa.sourceforge.net:/home/groups/i/ij/ijbswa/htdocs/pdf

	@$(ECHO) Fixing permissions
	@ssh ijbswa.sourceforge.net 'chmod -R 775 /home/groups/i/ij/ijbswa/htdocs 2>/dev/null; true'
	@ssh ijbswa.sourceforge.net 'find /home/groups/i/ij/ijbswa/htdocs/ -type f | xargs chmod 664 2>/dev/null; true'
	@ssh ijbswa.sourceforge.net 'chmod 666 /home/groups/i/ij/ijbswa/htdocs/actions/results/actions-feedback.txt 2>/dev/null; true'


web-actions: tidy
	@$(ECHO) Uploading 
	@cd doc/webserver/actions; \
          upload=`find . -type f -a -not \( -path "*/CVS*" -o -path "*/results*" \)`; \
          $(TAR) c $$upload | ssh ijbswa.sourceforge.net 'cd /home/groups/i/ij/ijbswa/htdocs/actions; tar xvm'

	@$(ECHO) Fixing permissions
	@ssh ijbswa.sourceforge.net 'find /home/groups/i/ij/ijbswa/htdocs/actions/ -type f | xargs chmod 664 2>/dev/null'
	@ssh ijbswa.sourceforge.net 'chmod 666 /home/groups/i/ij/ijbswa/htdocs/actions/results/actions-feedback.txt 2>/dev/null'

## 
dok-put:
	tar --exclude ".cvsignore" --exclude "CVS" --exclude "source" --exclude ".htaccess" \
	     --exclude "obsolete" --exclude "actions" --exclude "*.zip" --exclude "robots.txt"\
		doc/* INSTALL LICENSE AUTHORS README \
		-czf $(DOC_FILE) ;\
		$(ECHO) "Uploading doc package ..." ;\
		scp $(DOC_FILE) ijbswa.sourceforge.net:/home/groups/i/ij/ijbswa/htdocs/docs/
		@ssh ijbswa.sourceforge.net 'chmod 775 /home/groups/i/ij/ijbswa/htdocs/docs/*gz 2>/dev/null; true'
		$(RM) $(DOC_FILE)

dok-get:
	cd /tmp ;\
	$(WGET) http://privoxy.org/docs/$(DOC_FILE) ;\
	$(TAR) -zxvf $(DOC_FILE)


#############################################################################
# Source file dependencies
#############################################################################

actions.o:   actions.c   actions.h   config.h $(PROJECT_H_DEPS) errlog.h jcc.h list.h loaders.h miscutil.h actionlist.h ssplit.h
cgi.o:       cgi.c       cgi.h       config.h $(PROJECT_H_DEPS) cgiedit.h cgisimple.h list.h pcrs.h encode.h ssplit.h jcc.h filters.h actions.h errlog.h miscutil.h
cgiedit.o:   cgiedit.c   cgiedit.h   config.h $(PROJECT_H_DEPS) cgi.h list.h pcrs.h encode.h ssplit.h jcc.h filters.h actions.h errlog.h miscutil.h
cgisimple.o: cgisimple.c cgisimple.h config.h $(PROJECT_H_DEPS) cgi.h list.h pcrs.h encode.h ssplit.h jcc.h filters.h actions.h errlog.h miscutil.h
deanimate.o: deanimate.c deanimate.h config.h $(PROJECT_H_DEPS)
encode.o:    encode.c    encode.h    config.h
errlog.o:    errlog.c    errlog.h    config.h $(PROJECT_H_DEPS) #w32log.h
filters.o:   filters.c   filters.h   config.h $(PROJECT_H_DEPS) errlog.h encode.h gateway.h jbsockets.h jcc.h loadcfg.h parsers.h ssplit.h cgi.h deanimate.h #win32.h 
gateway.o:   gateway.c   gateway.h   config.h $(PROJECT_H_DEPS) errlog.h jbsockets.h jcc.h loadcfg.h
jbsockets.o: jbsockets.c jbsockets.h config.h $(PROJECT_H_DEPS) filters.h
jcc.o:       jcc.c       jcc.h       config.h $(PROJECT_H_DEPS) errlog.h filters.h gateway.h jbsockets.h killpopup.h loadcfg.h loaders.h miscutil.h parsers.h #w32log.h win32.h cgi.h
killpopup.o: killpopup.c killpopup.h config.h $(PROJECT_H_DEPS) jcc.h loadcfg.h
list.o:      list.c      list.h      config.h $(PROJECT_H_DEPS) list.h miscutil.h
loadcfg.o:   loadcfg.c   loadcfg.h   config.h $(PROJECT_H_DEPS) errlog.h filters.h gateway.h jbsockets.h jcc.h killpopup.h loaders.h miscutil.h parsers.h #w32log.h win32.h
loaders.o:   loaders.c   loaders.h   config.h $(PROJECT_H_DEPS) errlog.h encode.h filters.h gateway.h jcc.h loadcfg.h miscutil.h parsers.h ssplit.h
miscutil.o:  miscutil.c  miscutil.h  config.h
parsers.o:   parsers.c   parsers.h   config.h $(PROJECT_H_DEPS) errlog.h encode.h filters.h jbsockets.h jcc.h loadcfg.h loaders.h miscutil.h ssplit.h
ssplit.o:    ssplit.c    ssplit.h    config.h miscutil.h
urlmatch.o:  urlmatch.c  urlmatch.h  config.h $(PROJECT_H_DEPS) errlog.h miscutil.h ssplit.h

# GNU regex
gnu_regex.o: gnu_regex.c gnu_regex.h config.h

# PCRS
pcrs.o: pcrs.c pcrs.h config.h pcre/pcre.h 

# PCRE
pcre/get.o:        pcre/get.c        pcre/config.h pcre/internal.h pcre/pcre.h
pcre/maketables.o: pcre/maketables.c pcre/config.h pcre/internal.h pcre/pcre.h
pcre/pcre.o:       pcre/pcre.c       pcre/config.h pcre/internal.h pcre/pcre.h pcre/chartables.c 
pcre/pcreposix.o:  pcre/pcreposix.c  pcre/config.h pcre/internal.h pcre/pcre.h pcre/pcreposix.h
pcre/study.o:      pcre/study.c      pcre/config.h pcre/internal.h pcre/pcre.h

# An auxiliary program makes the PCRE default character table source

pcre/chartables.c:   pcre/dftables
		pcre/dftables >pcre/chartables.c

pcre/dftables:       pcre/dftables.c pcre/maketables.c pcre/pcre.h pcre/internal.h pcre/config.h
		$(CC) -o pcre/dftables $(CFLAGS) pcre/dftables.c

# Win32
w32log.o: w32log.c errlog.h config.h jcc.h loadcfg.h miscutil.h pcre/pcre.h pcre/pcreposix.h pcrs.h project.h w32log.h w32taskbar.h win32.h
w32taskbar.o: w32taskbar.c config.h w32log.h w32taskbar.h
win32.o: win32.c config.h jcc.h loadcfg.h pcre/pcre.h pcre/pcreposix.h pcrs.h project.h w32log.h win32.h

w32.res: w32.rc w32res.h icons/ico00001.ico icons/ico00002.ico icons/ico00003.ico icons/ico00004.ico icons/ico00005.ico icons/ico00006.ico icons/ico00007.ico icons/ico00008.ico icons/idle.ico icons/privoxy.ico config.h
	windres -D__MINGW32__=0.2 -O coff -i $< -o $@

# AmigaOS
#OBJS += amiga.o
#CFLAGS += -D__AMIGAVERSION__=\"$(VERSION_MAJOR).$(VERSION_MINOR)$(VERSION_POINT)\" -D__AMIGADATE__=\"`date +%d.%m.%Y`\" -W -m68020 -noixemul -fbaserel -msmall-code
#LDFLAGS += -m68020 -noixemul -fbaserel
#LIBS = -lm /gg/lib/libb/libm020/libnix/swapstack.o
#amiga.o: amiga.c amiga.h config.h


$(PROGRAM): $(OBJS) $(W32_FILES)
	$(LD) $(LDFLAGS) -o $(PROGRAM) $(OBJS) $(LIBS)

clean:
	$(RM) a.out $(OBJS) $(W32_FILES) $(W32_INIS) $(PROGRAM) default.action `find . -name TAGS -o -name tags` 

tidy:
	$(RM) `find . -name "*~"`
#	$(RM) `find . -name "#*#"` # what is this for??
	$(RM) `find . -name ".\#*"`

clobber: tidy
	$(RM) GNUmakefile configure config.h.in config.h config.cache config.status config.log logfile \
              privoxy.log core *.tar.gz *.tar privoxy-cl.spec doc/source/ldp.dsl

#
# FIXME: What is all this? 
#
	$(RM) cscope.*  *.pdb *.lib *.exp 

distclean: clobber

tags: $(SRCS) $(HDRS)
	etags $(SRCS) $(HDRS)

CONF_DEST:=$(shell if [ "$(prefix)" = "/usr/local" ] && [ "$(CONF_BASE)" = "$(prefix)/etc" ];then \
		$(ECHO) "$(CONF_BASE)/privoxy";\
	 else\
		 $(ECHO) "$(CONF_BASE)";\
	 fi)

LOG_DEST:=$(shell if [ "$(prefix)" = "/usr/local" ] && [ "$(LOGS_DEST)" = "$(prefix)/var/log/privoxy" ];then \
		$(ECHO) "/var/log/privoxy" ;\
	 else\
		 $(ECHO) "$(LOGS_DEST)";\
	 fi)

PID_DEST:=$(shell if [ "$(prefix)" = "/usr/local" ] && [ "$(PIDS_DEST)" = "$(prefix)/var/run" ];then \
		$(ECHO) "/var/run" ;\
	 else\
		 $(ECHO) "$(PIDS_DEST)";\
	 fi)

check_doc:=$(shell if [ ! -d "$(SHARE_DEST)/doc" ] && [ "$(prefix)" = "/usr/local" ]  && [ -d "$(prefix)/doc" ];then \
		$(ECHO) "1";\
	 else\
		 $(ECHO) "0";\
	 fi)

# If USER is specified but no GROUP, assume there is a GROUP of same name.
GROUP_T:=$(shell if [ x$(GROUP) = x ] && [ x$(USER) != x ];then \
		$(ECHO) "$(USER)" ;\
	 else\
		 $(ECHO) "$(GROUP)";\
	 fi)

install-strip:
	$(MAKE) install STRIP=-s

# FIXME: Test USER and GROUP on Slack to make sure this works as 
# intended.
#
# FIXME: id handling needs help, probably via configure, since 'id -u' is not 
# universally reliable (eg Solaris). Group handling could be better. 
# Perhaps the whole user/group validation should be done here, and simplified.
PROGRAM_V = Privoxy $(VERSION)
install: CONF_DEST LOG_DEST PID_DEST check_doc GROUP_T
	@# Quick test for valid USER.
	@if [ -n "$(USER)" ]; then \
		$(ID) $(USER) >/dev/null || exit 1;\
	fi
	@# Test for valid group. FIXME. USER does not have to belong to GROUP 
	@# for file ownership purposes.
# 	if [ -n "$(GROUP_T)" ] && [ -n "$(USER)" ] && ! $(GROUPS) $(USER) | $(GREP) "\<$(GROUP_T)\>" >/dev/null; then \
# 		$(ECHO) Group $(GROUP_T) for User $(USER) is invalid && exit 1 ;\
# 	fi

	@$(ECHO) "Creating directories, and preparing $(PROGRAM_V) installation"
	$(CHMOD) $(DIR_MODE) $(MKDIR)
	@$(MKDIR) $(SBIN_DEST) $(prefix) $(CONF_DEST) $(CONF_DEST)/templates $(SHARE_DEST) \
		$(LOG_DEST) $(PID_DEST)
	@# Install the executable binary, strip if invoked as install-strip
	@test -n "$(STRIP)" &&\
	$(ECHO) Installing $(PROGRAM) stripped executable to $(SBIN_DEST) ||\
	$(ECHO) Installing $(PROGRAM) executable to $(SBIN_DEST)
	$(INSTALL) $(INSTALL_P) $(STRIP) $(PROGRAM) $(SBIN_DEST)
     
	@# Install the DOCS and man page. install-sh only does one file at a time.
	-@if [ $(check_doc) = 0 ]; then \
		DOC=$(DOC_DEST) ;\
	else \
		DOC=$(prefix)/doc/privoxy ;\
	fi;\
	$(MKDIR) $$DOC $$DOC/user-manual $$DOC/faq $$DOC/developer-manual \
	     $$DOC/man-page $$DOC/images $(MAN_DEST) ;\
	if [ -d "$(DOK_WEB)" ]; then \
		$(ECHO) Installing FAQ, Manual, and other docs to $$DOC;\
          for i in user-manual developer-manual faq; do \
               for ii in $(DOK_WEB)/$$i/*html; do \
                    $(INSTALL) $(INSTALL_T) $$ii $$DOC/$$i;\
               done ;\
          done ;\
          for i in $(DOK_WEB)/images/*jpg; do \
               $(INSTALL) $(INSTALL_T) $$i $$DOC/images;\
          done ;\
		$(INSTALL) $(INSTALL_T) $(DOK_WEB)/man-page/*html $$DOC/man-page;\
		$(INSTALL) $(INSTALL_T) $(DOK_WEB)/privoxy-index.html $$DOC/index.html;\
		$(INSTALL) $(INSTALL_T) AUTHORS $$DOC;\
		$(INSTALL) $(INSTALL_T) LICENSE $$DOC;\
		$(INSTALL) $(INSTALL_T) README $$DOC;\
		$(INSTALL) $(INSTALL_T) ChangeLog $$DOC;\
		$(INSTALL) $(INSTALL_T) $(DOK_WEB)/p_doc.css $$DOC;\
	fi
	@# Not all platforms support gzipped man pages.
	@$(ECHO) Installing man page to $(MAN_DEST)/privoxy.1
	-$(INSTALL) $(INSTALL_T) privoxy.1  $(MAN_DEST)/privoxy.1

	@# Change the config file default directories according to the configured ones
	@$(ECHO) Rewriting config for this installation
	@if [ -f config.base ] ; then \
		$(CAT) config >config~ ;\
		$(MV) config.base config ;\
	fi
	$(SED) 's+confdir .+confdir $(CONF_DEST)+' config | \
	$(SED) 's+logdir .+logdir $(LOG_DEST)+' >config.updated
	$(MV) config config.base
	$(MV) config.updated config 

	@# Install the config support files. Test for root install, and abort 
	@# if there is no privoxy user, and no other user was enabled during 
	@# configure. Later, install init script if appropriate.
	@$(ECHO) Installing templates to $(CONF_DEST)/templates
	@for i in `find templates -type f`; do \
		$(INSTALL) $(INSTALL_T) $$i $(CONF_DEST)/templates ;\
	done

	@# FIXME: group/user validation is overly convoluted.
	@# If superuser install ... we require a minimum of group ownership
	@# of those files the daemon writes to, to be non-root owned.
	@if [ "`$(ID) |sed 's/(.*//' |sed 's/.*=//'`" = "0" ] ;then\
		if [ x$(USER) = x ] || [ $(USER) = root ]; then \
			if [ x$(GROUP) = x ] || [ $(GROUP) = root ]; then \
				if [ "`$(ID) privoxy`" ] && \
					$(GROUPS) privoxy | $(SED) 's/^.*://' |$(GREP) "\<privoxy\>" >/dev/null; then \
					$(ECHO) "Warning: Setting group owner to privoxy";\
					GROUP_T=privoxy ;\
				else \
					$(ECHO)  "******************************************************************" ;\
					$(ECHO)  " WARNING! WARNING! installing config files as root!" ;\
					$(ECHO)  " It is strongly recommended to run $(PROGRAM) as a non-root user," ;\
					$(ECHO)  " and to install the config files as that user and/or group!" ;\
					$(ECHO)  " Please read INSTALL, and create a privoxy user and group!" ;\
					$(ECHO)  "*******************************************************************" ;\
					exit 1 ;\
				fi ;\
			else \
				GROUP_T=$(GROUP) ;\
			fi ;\
			INSTALL_CONF="$(INSTALL_R) -g $$GROUP_T " ;\
		else \
			$(ECHO) "Superuser install, installing config files as $(USER):$(GROUP_T)" ;\
			INSTALL_CONF="$(INSTALL_R) -o $(USER) -g $(GROUP_T)" ;\
			GROUP_T=$(GROUP_T) ;\
		fi ;\
	else \
		if [ ! "`id $(USER)`" = "`id`" ] ;then \
			$(ECHO) "** WARNING ** current install user different from configured user!!" ;\
			$(ECHO) "Edit may fail." ;\
		fi ;\
		INSTALL_CONF="$(INSTALL_R)" ;\
	fi ;\
	$(ECHO) Installing configuration files to $(CONF_DEST);\
	for i in $(CONFIGS); do \
		if [ -s "$(CONF_DEST)/$$i" ] ; then \
			$(ECHO) Installing $$i as $$i.new ;\
			$(INSTALL) $$INSTALL_CONF $$i $(CONF_DEST)/$$i.new || exit 1;\
			NEW=1;\
		else \
			$(INSTALL) $$INSTALL_CONF $$i $(CONF_DEST) || exit 1;\
		fi ;\
	done ;\
	if [ -n "$$NEW" ]; then \
		$(CHMOD) $(RWD_MODE) $(CONF_DEST)/*.new || exit 1 ;\
		$(ECHO) "Warning: Older config files are preserved. Check new versions for changes!" ;\
	fi ;\
	[ ! -f $(LOG_DEST)/logfile ] && $(ECHO) Creating logfiles in $(LOG_DEST) || \
		$(ECHO) Checking logfiles in $(LOG_DEST) ;\
		$(TOUCH) $(LOG_DEST)/logfile $(LOG_DEST)/jarfile || exit 1 ;\
	if [ x$$USER != x ]; then \
		$(CHOWN) $$USER $(LOG_DEST)/logfile $(LOG_DEST)/jarfile || \
		$(ECHO) "** WARNING ** current install user different from configured user. Logging may fail!!" ;\
	fi ;\
	if [ x$$GROUP_T != x ]; then \
		$(CHGRP) $$GROUP_T $(LOG_DEST)/logfile $(LOG_DEST)/jarfile || \
		$(ECHO) "** WARNING ** current install user different from configured user. Logging may fail!!" ;\
	fi ;\
	$(CHMOD) $(RWD_MODE) $(LOG_DEST)/logfile $(LOG_DEST)/jarfile || exit 1 ;\
	if [ "$(prefix)" = "/usr/local" ] || [ "$(prefix)" = "/usr" ]; then \
		if [ -f /etc/slackware-version ] && [ -d /etc/rc.d/ ] && [ -w /etc/rc.d/ ] ; then \
               $(SED) 's+%PROGRAM%+$(PROGRAM)+' slackware/rc.privoxy.orig | \
               $(SED) 's+%SBIN_DEST%+$(SBIN_DEST)+' | \
               $(SED) 's+%CONF_DEST%+$(CONF_DEST)+' | \
               $(SED) 's+%USER%+$$USER+' | \
               $(SED) 's+%GROUP%+$(GROUP_T)+' >slackware/rc.privoxy ;\
			$(INSTALL) $(INSTALL_P) slackware/rc.privoxy /etc/rc.d/ ;\
               $(ECHO) "Installing for Slackware." ;\
               $(ECHO) "Dont forget to add the rc.privoxy to rc.local if you want it started at every boot" ;\
		elif [ -f /etc/redhat-release ] && [ -d /etc/rc.d/init.d/ ] && [ -w /etc/rc.d/init.d/ ] ; then \
               $(ECHO) "Installing init script to /etc/rc.d/init.d/privoxy" ;\
			$(SED) 's,^PRIVOXY_BIN=.*,PRIVOXY_BIN="/usr/local/sbin/$(PROGRAM)",' privoxy.init |\
			$(SED) 's,^PRIVOXY_CONF=.*,PRIVOXY_CONF="$(CONF_DEST)/config",' |\
			$(SED) "s,^PRIVOXY_USER=.*,PRIVOXY_USER=$$USER," > init.tmp ;\
			$(INSTALL) $(INSTALL_P) init.tmp /etc/rc.d/init.d/privoxy && $(RM) init.tmp;\
			$(MKDIR) /etc/logrotate.d/ ;\
			$(ECHO) "Installing logrotate script to /etc/logrotate.d/" ;\
			$(INSTALL) -m 0644 privoxy.logrotate /etc/logrotate.d/privoxy ;\
		elif [ -d /etc/init.d ] && [ -w /etc/init.d ] ; then \
			$(ECHO) "Installing generic init script to /etc/init.d/privoxy" ;\
			$(ECHO) "Please check that the PATHs are correct, and edit if needed." ;\
			$(INSTALL) $(INSTALL_P) privoxy-generic.init /etc/init.d/privoxy ;\
		fi ;\
	else \
		$(ECHO) "No init script installed, install it manually if needed" ;\
	fi
	@# mmmmm, good.
	@$(ECHO) "$(PROGRAM_V) installation succeeded!"
	@$(ECHO) "The Privoxy configuration files have been installed in $(CONF_DEST)"

# rmdir is used as a precaution since it will not remove non-empty
# directories. RH init script creates lock file and pid file.
uninstall: CONF_DEST LOG_DEST PID_DEST check_doc
	@$(ECHO) Starting Privoxy uninstallation
	@# KILL privoxy if running
	-@if [ -f /etc/redhat-release ] && [ -x /etc/rc.d/init.d/privoxy ]; then \
		/etc/rc.d/init.d/privoxy stop >/dev/null 2>/dev/null ;\
		chkconfig --del $(PROGRAM) 2>/dev/null;\
	fi
	-@test -f $(PID_DEST)/privoxy.pid && $(ECHO) Stopping $(PROGRAM) &&\
         $(KILL) `$(CAT) $(PID_DEST)/privoxy.pid` || :
	-@test -f /var/run/privoxy.pid && $(ECHO) Stopping $(PROGRAM) &&\
          $(KILL) `$(CAT) /var/run/privoxy.pid ` || :

	@# Program binary
	@$(ECHO) Removing $(PROGRAM) binary
	$(RM) $(SBIN_DEST)/$(PROGRAM) $(SBIN_DEST)/$(PROGRAM)~

	@# config files and dir, and maybe old install backups
	-@if [ -d $(CONF_DEST) ]; then \
		$(ECHO) Saving $(PROGRAM) config files to /tmp/$(PROGRAM)-save ;\
		$(MKDIR) /tmp/$(PROGRAM)-save ;\
		cd $(CONF_DEST) ;\
		for i in $(CONFIGS); do \
			[ -f $$i ] && $(CP) $$i /tmp/$(PROGRAM)-save ;\
		done ;\
	fi
	@$(ECHO) Removing $(PROGRAM) config files
	-@for i in $(CONFIGS); do \
		test -f $(CONF_DEST)/$$i && $(ECHO) Removing $$i ;\
		$(RM) $(CONF_DEST)/$$i $(CONF_DEST)/$$i~ $(CONF_DEST)/$$i.new ;\
	done
	-@test -d $(CONF_DEST)/templates && $(RM) -r $(CONF_DEST)/templates &&\
	$(ECHO) "Removing $(CONF_DEST)/templates/*"

	@# man page and docs
	@$(ECHO) Removing $(PROGRAM) docs
	-$(RM) $(MAN_DEST)/privoxy.1*
	-$(RM) -r $(DOC_DEST) || $(RM) -r $(prefix)/doc/privoxy

	@# Log and jarfile and pidfile
	@$(ECHO) Removing $(PROGRAM) logs
	-$(RM) $(LOG_DEST)/logfile $(PID_DEST)/privoxy.pid $(LOG_DEST)/jarfile

	@# Final clean up of unused directories. Special handling of CONF and LOG
     # destinations.
	@$(ECHO) Removing $(PROGRAM) directories
	@for i in $(LOG_DEST) $(CONF_DEST); do \
		if test -d $$i; then \
			$(ECHO) Removing $$i ;\
			$(RMDIR) $$i || $(ECHO) "$$i is not empty, not removed" ;\
		fi;\
	done
	@if [  ! "$(prefix)" = "/usr/local" ] ;then \
		for i in $(MAN_DEST) $(MAN_DIR) $(SHARE_DEST)/doc $(SHARE_DEST) $(SBIN_DEST); do \
			if test -d $$i; then \
				$(ECHO) Removing $$i ;\
				$(RMDIR) $$i || $(ECHO) "$$i is not empty, not removed" ;\
			fi;\
		done;\
		if test $(LOG_DEST) != /var/log/privoxy && test -d $(prefix)/var/log; then \
			$(ECHO) Removing $(prefix)/var/log ;\
			$(RMDIR) $(prefix)/var/log || $(ECHO) "$(prefix)/var/log is not empty, not removed";\
		fi ;\
		if test $(PID_DEST) != /var/run && test -d $(prefix)/var/run; then \
			$(ECHO) Removing $(prefix)/var/run ;\
			$(RMDIR) $(prefix)/var/run || $(ECHO) "$(prefix)/var/run is not empty, not removed";\
		fi ;\
		if test $(prefix)/var != /var && test -d $(prefix)/var; then \
			$(ECHO) Removing $(prefix)/var ;\
			$(RMDIR) $(prefix)/var || $(ECHO) "$(prefix)/var is not empty, not removed" ;\
		fi ;\
		if test $(prefix) != / && test $(prefix) != /usr && test -d $(prefix); then \
			$(ECHO) Removing $(prefix) ;\
			$(ECHO) Removing installation directory $(prefix) ;\
			$(RMDIR) $(prefix) || $(ECHO) "$(prefix) is not empty, not removed" ;\
		fi;\
	fi

	@# init scripts and logrotate
	@if [ "$(prefix)" = "/usr/local" ] || [ "$(prefix)" = "/usr" ]; then \
		$(ECHO) Removing $(PROGRAM) init script ;\
		if [ -f /etc/slackware-version ] && [ -d /etc/rc.d/ ]  && [ -w /etc/rc.d/ ] ; then \
			$(RM) /etc/rc.d/rc.privoxy ;\
		elif [ -f /etc/redhat-release ] && [ -d /etc/rc.d/init.d/ ]  && [ -w /etc/rc.d/init.d/ ] ; then \
			$(RM) /etc/rc.d/init.d/privoxy /etc/logrotate.d/privoxy;\
		elif [ -d /etc/init.d ] && [ -w /etc/init.d ] ; then \
			$(RM) /etc/init.d/privoxy ;\
		else \
			$(ECHO) "Unable to remove privoxy init script, not installed or permission denied" ;\
		fi ;\
	fi
	@$(ECHO) Privoxy uninstalled, bye

coffee:
	 @perl -e 'print pack "C*", (31,139,8,8,153,63,226,60,2,3,99,111,102,102,101,101,0,109,143,205,13,192,32,8,133,\
                  239,78,241,110,234,1,28,160,171,152,208,53,26,117,247,22,165,73,137,125,9,1,62,126,2,128,169,5,243,143,\
                  13,139,49,164,65,100,149,152,102,73,141,88,73,178,116,205,100,69,253,36,102,81,49,83,236,19,225,171,131,\
                  214,172,163,73,4,168,123,115,71,126,247,122,94,128,178,227,95,154,12,86,215,122,197,249,146,187,54,220,125,\
                  193,51,228,11,1,0,0);'|zcat

#############################################################################

## Local Variables:
## tab-width: 3
## end:

# $Log: GNUmakefile.in,v $
# Revision 1.104.2.25  2004/01/31 01:15:33  oes
# Fixed a typo; updated copyright notice
#
# Revision 1.104.2.24  2003/12/03 10:30:02  oes
# - Added new dependency: actions.c -> ssplit.h
# - Excluded PDF docs from src tarball
#
# Revision 1.104.2.23  2003/04/20 17:28:52  hal9
# Strip trailing spaces from config-file generation, bug #724596.
#
# Revision 1.104.2.22  2003/03/28 03:32:01  hal9
# Minor changes for Privoxy home page:
#  - Handle &copy; more sanely
#  - include link to announce.txt
# Also, disable 'make announce' target.
#
# Revision 1.104.2.21  2002/11/04 07:04:03  hal9
# Catch up with main trunk install/uninstall. Quiet output, etc.
#
# Revision 1.104.2.20  2002/10/25 02:44:22  hal9
# Port of make install, etc from main trunk. Needs testing! Add Slackware
# support, and other related changes. Update related docs.
#
# Revision 1.104.2.19  2002/09/26 22:50:02  hal9
# New user-manual examples in config-file are getting wrapped. Add warning.
#
# Revision 1.104.2.18  2002/08/23 12:22:40  oes
# Added warning to broken install target
#
# Revision 1.104.2.17  2002/08/16 03:19:34  hal9
# More (minor) cleanup of html before pdf processing to make some relative
# links work as pdf -> pdf. Upload pdf as zip archive now.
#
# Revision 1.104.2.16  2002/08/14 16:43:27  hal9
# Added pdf docs to make webserver target.
#
# Revision 1.104.2.15  2002/08/11 20:02:41  hal9
# New targets for man page (make man) and pdf (make dok-pdf) targets.
#
# Revision 1.104.2.14  2002/08/10 11:19:37  oes
# - Make -Ipcre (again) conditional on STATIC_PCRE
# - $(RPMBUILD) -> $(RPM) for SuSE
# - Add dependency: pcrs.o deps on config.h
#
# Revision 1.104.2.13  2002/08/07 15:13:54  hal9
# Remove pdf2 target, and make it dok-shtml (single page html for pdf
# conversion).
#
# Revision 1.104.2.12  2002/08/06 11:29:36  oes
# Fixed detection/inclusion of pcre.h, which is in a pcre subdir on RH
#
# Revision 1.104.2.11  2002/07/30 19:38:11  hal9
# Add redhat-test target for testing purposes only. Fix RPM_PACKAGEV to what
# *I think* it was supposed to be (was breaking upload targets since it was
# set to RPM_VERSION).
#
# Revision 1.104.2.10  2002/07/27 22:56:53  kick_
# cleanups of the redhat-srpm target
#
# Revision 1.104.2.9  2002/07/26 15:17:02  oes
# - Added generation of default.action from defaul.action.master
# - Deleted obsolete re_filterfile.txt generation
#
# Revision 1.104.2.8  2002/07/12 10:04:32  kick_
# added helper targets to the makefile. They shouldn't break anything, but
# make my life a lot easier.
#
# The new rpm has been splitted into two parts, one for package installation/
# removal, one for package building.
# Therefore rpm -ta isn't a valid command anymore and needs to be replaced
# by rpmbuild -ta  (this is backwards compatible)
#
# Revision 1.104.2.7  2002/06/07 00:23:47  hal9
# Fixing a quirk of man2html (on my system) that pulls punctuation into URLs,
# thus breaking them completely.
#
# Revision 1.104.2.6  2002/06/02 03:26:25  hal9
# Update CONFIG_FILES (ie update basic.action, etc), and also DOC_FILES (exclude
# index.html and team/index.html)
#
# Revision 1.104.2.5  2002/05/30 15:35:01  hal9
# This is more cleanup on the make config-file target. Most issues for
# automatic generation are taken care of. There are still some problems
# that require hand editing. Namely, some of the examples that are > 80 chars.
#
# Revision 1.104.2.4  2002/05/29 02:12:17  hal9
# Ooops...forgot about perl -pi cygwin problem. Add -pi.bak. Also, the
# new target is 'make config-file', _not_ make config.
#
# Revision 1.104.2.3  2002/05/29 02:05:48  hal9
# 'make config' target added (WIP) for future generation of config file from
# text in u-m so the two are in sync. New generated config, which requires
# some hand editing for the time being.
#
# Revision 1.104.2.2  2002/05/28 02:32:55  hal9
# New target 'make dok-index' for privoxy-index.html. Also, fixed *.bak files
# not being cleaned up in doc/webserver.
#
# Revision 1.104.2.1  2002/05/26 17:19:34  hal9
# Remove Table of Contents from readme with oes's dsl trick.
#
# Revision 1.104  2002/05/24 00:03:49  oes
# Use p_doc.css for the Homepage for consistency
#
# Revision 1.103  2002/05/23 23:19:00  oes
# Use dsl without TOC for the homepage
#
# Revision 1.102  2002/05/16 01:20:17  hal9
# make announce target added.
#
# Revision 1.101  2002/05/15 12:28:46  oes
# Trying to keep Hal happy :)
#
# Revision 1.100  2002/05/08 13:48:18  hal9
# Ooops, that trashed JB v2.0.2 comment. Fixed.
#
# Revision 1.99  2002/05/08 13:42:07  hal9
# This fixes the numbering problem on index.html in contact info section (.1.). Using
# perl, since its way too convoluted to try to fix proper with docbook.
#
# Revision 1.98  2002/05/03 14:33:06  oes
# Replaced ldp(OK).dsl handling with generation via autoconf; handle all file exeptions to src tarball via find
#
# Revision 1.97  2002/04/27 20:27:43  swa
# no longer needed due to new
# PACKAGE_VERSION process
#
# Revision 1.96  2002/04/27 17:44:32  morcego
# - Correcting typo in my name (Rodrigo, not Rodgrigo) :-)
# - Using the RM macro everywhere rm is called (either we use, or don't)
# - Same for RPM
#
# Revision 1.95  2002/04/27 15:37:25  swa
# replacing directory in document creation process
# no longer necessary.
#
# Revision 1.94  2002/04/27 08:23:29  swa
# pdf process reviewed and cleaned up
#
# Revision 1.93  2002/04/27 04:55:53  morcego
# privoxy-cl.spec now gets removed by clobber target
#
# Revision 1.92  2002/04/27 04:53:40  morcego
# Adding --exclude "PACKAGERS" to every tar command that applies (not for
#   webserver target)
#
# Revision 1.91  2002/04/27 04:44:51  morcego
# GNUmakefile.in: The tarball created on redhat-dist and suse-dist now ignore
#   the PACKAGERS file, as well privoxy-cl.spec (in case it was created)
# GNUmakefile.in: New targets -> conectiva-spec, conectiva-dist and
#   conectiva-upload
# genclspec.sh  : New file to generate, from privoxy-rh.spec, a specfile
#   for Conectiva Linux
#
# Revision 1.90  2002/04/26 17:46:53  swa
# be consistent
#
# Revision 1.89  2002/04/26 17:20:54  swa
# just produce single html files to proces them later with Destiller or somesuch. looks prettier.
#
# Revision 1.88  2002/04/25 19:13:57  morcego
# Removed RPM release number declaration on configure.in
# Changed makefile to use given value for RPM_PACKAGEV when on uploading
# targets (will produce an error, explaining who to do it, if no value
# if provided).
#
# Revision 1.87  2002/04/23 14:10:59  swa
# now create pdf documents
#
# Revision 1.86  2002/04/15 04:30:27  hal9
# Missed two -pi.bak's on perl/cygwin problem.
#
# Revision 1.85  2002/04/14 01:05:34  hal9
# Revert dok-webserver change for SF logo.
#
# Revision 1.84  2002/04/13 22:43:25  hal9
# -Fix dok-webserver for SF logo (more perl).
# -Change all perl -pi to perl -pi.bak for Cygwin problem.
#
# Revision 1.83  2002/04/12 09:39:25  oes
# Excluding yet more files from tarball; making dist warning yet more scary
#
# Revision 1.82  2002/04/11 21:07:11  oes
# Excluding more files from tarball build
#
# Revision 1.81  2002/04/11 14:40:27  oes
# Fixed typo -- Thanks, Moritz!
#
# Revision 1.80  2002/04/11 12:50:00  oes
# Fixed tarball-dist target
#
# Revision 1.79  2002/04/11 06:49:28  oes
# webserver target: silenced timestamp warnings resulting from uploading westwards, made permissions fixing independant of screwed local dir permissions, suppress (false alarm) make error if not owner of feedback log
#
# Revision 1.78  2002/04/09 13:37:11  sarantis
# fix tar options typo
#
# Revision 1.77  2002/04/09 13:28:53  swa
# build suse and gen-dist with html docs
#
# Revision 1.76  2002/04/08 22:43:41  oes
# Fix: Include dotfiles in fixing webserver permissions
#
# Revision 1.75  2002/04/08 22:14:59  oes
# Silencing tar warnings in the web* targets
#
# Revision 1.74  2002/04/08 15:22:44  hal9
# This has finishing touches for dok building. Should be ready to go.
# -The main doc build is now 'make dok', should work on Redhat too.
# -Removed man page from main doc build. It is built separately due to
#  perl scripts that most aren't likely to have.
#
# Revision 1.73  2002/04/08 14:03:24  oes
# oes for al: Fix install target
#
# Revision 1.72  2002/04/08 13:42:11  oes
# Added safety check to *-dist targets; fixed permissions for feedback logfile
#
# Revision 1.71  2002/04/07 20:32:03  hal9
# -Add meta data kludge for make dok-webserver via $(PERL).
# -Add subdirs for 'make dok-release'.
#
# Revision 1.70  2002/04/07 08:59:40  swa
# generated files. do NOT edit.
# fixed directory bug in makefile.
#
# Revision 1.69  2002/04/07 08:10:47  swa
# create some of the webserver docs
# automatically (in particular if
# those docs recycle other documentation
# fragments). Now committed webserver's
# index file.
#
# Revision 1.68  2002/04/07 07:58:11  swa
# create some of the webserver docs
# automatically (in particular if
# those docs recycle other documentation
# fragments)
#
# Revision 1.67  2002/04/07 05:31:42  hal9
# Add 'dok-release' target:
# -Set doc entities to VERSION and CODE_STATUS during make.
# -Set doc conditional content flags (stable vs non-stable).
# A separate target for the time being but needs to be incorporated into
# dok build at some point.
# -Filter out a spurious ^G from new man page > html converion in man2html.
#
# Revision 1.66  2002/04/06 20:28:21  jongfoster
# Prettifying groff2html.
# Using GNU Make's conditional makefile feature rather than shell "if"s.
# (The shell "if"s were hiding errors)
# "perl" -> "$(PERL)"
# Spaces->tabs in a couple of places.
#
# Revision 1.65  2002/04/06 05:16:39  hal9
# -Add 'authors' and 'man' targets for AUTHORS and man-page (WIP).
# -Both of these will soon be generated files.
#
# Revision 1.64  2002/04/04 22:14:51  oes
# No longer rely on find honoring -iname
#
# Revision 1.63  2002/04/04 21:06:22  swa
# cosmetics.
#
# Revision 1.62  2002/04/04 20:49:50  swa
# attempt to consolidate the
# different dokbook versions.
#
# Revision 1.61  2002/04/04 19:18:21  swa
# readme was leftover directory. use w3m instead
# of lynx to be consistent among developers. use
# consistent target naming.
#
# Revision 1.60  2002/04/04 12:25:41  oes
# Tidy webserver upload w/o *~ files, CVS dirs and logfiles and with proper dir and file permissions
#
# Revision 1.59  2002/04/04 08:32:45  swa
# wrong name for tarball-dist target. further fixed content of tarball dist
#
# Revision 1.58  2002/04/04 06:32:58  hal9
# New dok targets for make readme.
#
# Revision 1.57  2002/04/04 00:36:36  gliptak
# always use pcre for matching
#
# Revision 1.56  2002/04/03 22:28:03  gliptak
# Removed references to gnu_regex
#
# Revision 1.55  2002/04/03 19:54:29  swa
# freebsd tested to work. attempt to move tarball dist target forward
#
# Revision 1.54  2002/04/03 14:54:07  oes
# Standard clean and clobber semantics II
#
# Revision 1.53  2002/04/03 14:19:16  oes
# Standard clean and clobber semantics
#
# Revision 1.52  2002/04/03 02:56:18  hal9
# Revert previous FAQ numbering kludge.
#
# Revision 1.51  2002/04/02 13:03:56  oes
# Added fix for webserver permissions
#
# Revision 1.50  2002/04/02 03:46:24  hal9
# Rewrite ldpOK.dsl so that sections are NOT numbered on FAQ, in an effort
# to make the Table of Contents not so 'busy' looking. SuSE needs testing :)
#
# Revision 1.49  2002/03/30 22:20:12  swa
# cd didn't work. neither did find.
#
# Revision 1.48  2002/03/30 19:04:06  swa
# people release differently. no good.
# I want to make parts of the docs only.
#
# Revision 1.47  2002/03/30 09:05:21  swa
# better packaging. better rpm building.
# tar failed on sun (no exclude there).
#
# Revision 1.46  2002/03/29 20:09:01  swa
# al's patch
#
# Revision 1.45  2002/03/29 19:45:45  swa
# for lazy swa
#
# Revision 1.44  2002/03/29 17:42:44  gliptak
# Correcting for Solaris tar limitations
#
# Revision 1.43  2002/03/29 07:40:03  swa
# fixed make webserver. doh
#
# Revision 1.42  2002/03/29 06:59:04  swa
# other users could not modify files on webserver
#
# Revision 1.41  2002/03/28 20:43:00  swa
# set make correctly
#
# Revision 1.40  2002/03/28 04:22:44  hal9
# More on man2html stuff.
#
# Revision 1.39  2002/03/28 01:04:14  hal9
# More man2html stuff for docs.
#
# Revision 1.38  2002/03/27 16:02:30  swa
# have a generic target
#
# Revision 1.37  2002/03/27 15:30:26  swa
# have a consistent appearance
#
# Revision 1.36  2002/03/27 14:58:08  swa
# can be used by mutilple targets
#
# Revision 1.35  2002/03/27 14:53:19  swa
# added solaris-dist
#
# Revision 1.34  2002/03/27 10:30:11  swa
# we want a html man file on the webserver
#
# Revision 1.33  2002/03/27 03:05:35  hal9
# Added man2html target for docs (redhat-dok only for now)
#
# Revision 1.32  2002/03/26 22:29:54  swa
# we have a new homepage!
#
# Revision 1.31  2002/03/26 14:00:18  swa
# fixed make tarball, tarball-dist, tarball-clean
#
# Revision 1.30  2002/03/25 12:52:25  swa
# new targets
#
# Revision 1.29  2002/03/24 17:03:55  jongfoster
# Name change
#
# Revision 1.28  2002/03/24 16:19:48  swa
# configure needs to be generated.
#
# Revision 1.27  2002/03/24 16:13:57  swa
# generated files are a nono in cvs
#
# Revision 1.26  2002/03/24 15:36:02  swa
# did not build.
#
# Revision 1.25  2002/03/24 14:31:08  swa
# remove more crappy files. set RPM
# release version correctly.
#
# Revision 1.24  2002/03/24 14:19:55  swa
# set rpm package release in configure.in. nowhere else.
#
# Revision 1.23  2002/03/24 13:06:49  swa
# suse-clean now runs fine
#
# Revision 1.22  2002/03/24 12:56:21  swa
# name change related issues.
#
# Revision 1.21  2002/03/24 12:43:57  swa
# name change
#
# Revision 1.20  2002/03/24 11:39:17  jongfoster
# Renaming config files
#
# Revision 1.19  2002/03/22 20:53:03  morcego
# - Ongoing process to change name to JunkbusterNG
# - configure/configure.in: no change needed
# - GNUmakefile.in:
#         - TAR_ARCH = /tmp/JunkbusterNG-$(RPM_VERSION).tar.gz
#         - PROGRAM    = jbng
#         - rh-spec now references as junkbusterng-rh.spec
#         - redhat-upload: references changed to junkbusterng-* (package names)
#         - tarball-dist: references changed to JunkbusterNG-distribution-*
#         - tarball-src: now JunkbusterNG-*
#         - install: initscript now junkbusterng.init and junkbusterng (when
#                    installed)
# - junkbuster-rh.spec: renamed to junkbusterng-rh.spec
# - junkbusterng.spec:
#         - References to the expression ijb where changed where possible
#         - New package name: junkbusterng (all in lower case, acording to
#           the LSB recomendation)
#         - Version changed to: 2.9.13
#         - Release: 1
#         - Added: junkbuster to obsoletes and conflicts (Not sure this is
#           right. If it obsoletes, why conflict ? Have to check it later)
#         - Summary changed: Stefan, please check and aprove it
#         - Changes description to use the new name
#         - Sed string was NOT changed. Have to wait to the manpage to
#           change first
#         - Keeping the user junkbuster for now. It will require some aditional
#           changes on the script (scheduled for the next specfile release)
#         - Added post entry to move the old logfile to the new log directory
#         - Removing "chkconfig --add" entry (not good to have it automaticaly
#           added to the startup list).
#         - Added preun section to stop the service with the old name, as well
#           as remove it from the startup list
#         - Removed the chkconfig --del entry from the conditional block on
#           the preun scriptlet (now handled on the %files section)
# - junkbuster.init: renamed to junkbusterng.init
# - junkbusterng.init:
#         - Changed JB_BIN to jbng
#         - Created JB_OBIN with the old value of JB_BIN (junkbuster), to
#           be used where necessary (config dir)
#
# Aditional notes:
# - The config directory is /etc/junkbuster yet. Have to change it on the
# specfile, after it is changes on the code
# - The only files that got renamed on the cvs tree were the rh specfile and
# the init file. Some file references got changes on the makefile and on the
# rh-spec (as listed above)
#
# Revision 1.18  2002/03/21 23:00:00  swa
# want to autogenerate stuff.
#
# Revision 1.17  2002/03/19 19:30:04  morcego
# - Fixing stylesheet checking on configure. If it is found, no further checks
#   should be done
#
# - configure will now check for db2html or docbook2html (should work now
#   on SuSe without the docbktls package)
#
# Revision 1.16  2002/03/14 22:32:32  hal9
# Bumped the RPM version.
#
# Revision 1.15  2002/03/08 20:00:28  swa
# some leftovers.
#
# Revision 1.14  2002/03/07 18:25:56  swa
# synced redhat and suse build process
#
# Revision 1.13  2002/03/07 17:17:56  oes
# (Hopefully) fixed for older make versions
#
# Revision 1.12  2002/03/07 15:28:27  swa
# more informative
#
# Revision 1.11  2002/03/06 14:33:18  sarantis
# Use proper temp file, not "abc".
#
# Revision 1.10  2002/03/06 14:19:35  sarantis
# Cleanup PID_FILE_PATH from redhat-dist target
#
# Revision 1.9  2002/03/05 17:31:11  morcego
# Search for docbook.dsl. Should solve portability problems for SuSe.
#
# Revision 1.8  2002/03/05 14:07:42  morcego
# configure now detects rpm topdir, and change GNUmakefile acordingly
#    (based on sugestion by Sarantis Paskalis)
#
# Revision 1.7  2002/03/05 13:43:28  morcego
# Checking for text browser, so redhat-dok can work.
#
# Revision 1.6  2002/03/05 13:10:51  morcego
# Changes to implement redhat-dok (Hal Burgiss)
# Changes to make it work on other distros and out-of-the-shelf configurations
#
# Revision 1.5  2002/02/27 15:30:39  hal9
# Reset $(RPM_PACKAGEV) to 1 (was 2)
#
# Revision 1.4  2002/01/17 21:44:04  jongfoster
# Adding urlmatch.[ch]
#
# Revision 1.3  2002/01/04 15:26:08  oes
# Added tarball-src target
#
# Revision 1.2  2001/12/30 14:07:31  steudten
# - Add signal handling (unix)
# - Add SIGHUP handler (unix)
# - Add creation of pidfile (unix)
# - Add action 'top' in rc file (RH)
# - Add entry 'SIGNALS' to manpage
# - Add exit message to logfile (unix)
#
# Revision 1.1  2001/12/01 11:22:57  jongfoster
# Renaming Makefile.in to GNUmakefile.in so that non-GNU versions of
# make break in a more obvious way.
# Adding .PHONY section.
#
# Revision 1.40  2001/12/01 00:24:11  jongfoster
# Renaming various config files
# Fixing CR->CRLF under Win32 (I hope)
#
# Revision 1.39  2001/11/06 12:07:30  steudten
# Add --clean for building rpm in target redhat-dist.
#
# Revision 1.38  2001/11/05 21:35:23  steudten
# Complete rewrite for the 'redhat-dist' target.
# Checks for writeable RPM build directories for calling user.
# So you must not be root, just set the modes to 1777 to
# build a RH package.
# Fix the upload-target to be arch independant.
# Add target for 'solaris-dist' - coming soon.
#
# Revision 1.37  2001/11/01 00:52:04  hal9
# Redhat-upload stuff per Stefan.
#
# Revision 1.36  2001/10/31 19:26:13  swa
# automate process of uploading new releases
# to sf.
#
# Revision 1.35  2001/10/15 22:14:59  joergs
# Removed -O2 and -Wall from AmigaOS-only CFLAGS since they are now in
#  the general CFLAGS already.
#
# Revision 1.34  2001/10/15 18:28:06  steudten
# remove config.cache for target clobber.
# Cleanup make dist for RH and S.u.S.E.
#
# Revision 1.33  2001/10/10 12:43:33  oes
# Added ugly hack to make install target work at least for some setups.
#
# Revision 1.32  2001/10/09 22:38:19  jongfoster
# Correcting actionsfile filename for Win32 INI build
#
# Revision 1.31  2001/09/23 10:13:48  swa
# upload process established. run make webserver and
# the documentation is moved to the webserver. documents
# are now linked correctly.
#
# Revision 1.30  2001/09/19 17:55:49  oes
# Fixed CFLAGS
#
# Revision 1.29  2001/09/16 17:34:27  jongfoster
# Removing showargs.[ch], adding cgi(simple|edit).[ch]
# Replacing $(OBJEXT) with o - this seems to be a common source
# of build problems.
#
# Revision 1.28  2001/09/13 15:19:08  swa
# we want text files as well.
#
# Revision 1.27  2001/09/13 13:11:37  steudten
#
# Replace DEBUG_CFLAGS with OTHER_CFLAGS
#
# Revision 1.26  2001/09/12 23:44:54  david__schmidt
# Mac OSX (Darwin) support added.
#
# Revision 1.25  2001/09/12 22:55:45  joergs
# AmigaOS support added.
#
# Revision 1.24  2001/09/12 17:28:59  david__schmidt
#
# OS/2 port: update autoconf'd support for the platform.
#
# Revision 1.23  2001/09/12 16:28:42  swa
# added "make dok" section to generate html pages from
# the sgml source documents. note that the we do not want
# generated stuff in cvs.
#
# Revision 1.22  2001/09/10 16:31:23  swa
# buildroot definition in the specfile fucks up the build
# process under suse. hence I moved it to the "rpm -ta"
# command
#
# Revision 1.21  2001/09/10 11:12:49  oes
# Turning on -Wall
#
# Revision 1.20  2001/08/02 22:04:29  jongfoster
# Removing some remaining references to obsolete w32rulesdlg.[ch]
#
# Revision 1.19  2001/07/30 22:14:03  jongfoster
# Removing obsolete w32rulesdlg.c and w32rulesdlg.h
#
# Revision 1.18  2001/07/29 17:09:17  jongfoster
# Major changes to build system in order to fix these bugs:
# - pthreads under Linux was broken - changed -lpthread to -pthread
# - Compiling in MinGW32 mode under CygWin now correctly detects
#   which shared libraries are available
# - Solaris support (?) (Not tested under Solaris yet)
#
# Revision 1.17  2001/07/28 16:44:54  oes
# Fixed sed LF->CRLF conversion and removed deprecated files
#
# Revision 1.16  2001/07/15 19:45:33  jongfoster
# Added support for linking with POSIX threads library
#
# Revision 1.15  2001/07/13 13:48:07  oes
#  - Moved STATIC #define for pcre to (ac)config.h
#  - Made -Ipcre depandant on static pcre compilation to
#    avoid version conflicts
#  - Included compilation and depandancies for new deanimate.c
#  - Made changes to the pcre/pcreposix/pcrs build process
#    as required by the new library autodetection in
#    configure.in
#
# Revision 1.14  2001/07/01 16:27:44  oes
# Fixed misplaced dependancy
#
# Revision 1.13  2001/06/29 13:18:36  oes
# - added depandancy of filters.o on cgi.h
#
# Revision 1.12  2001/06/12 17:15:56  swa
# fixes, because a clean build on rh6.1 was impossible.
# GZIP confuses make, %configure confuses rpm, etc.
#
# Revision 1.11  2001/06/11 11:26:35  sarantis
# RPM version should be the same as ijbswa version.  The rpm release is
# specified in the specfile.
#
# Revision 1.10  2001/06/07 17:27:45  swa
# added suse build section
#
# Revision 1.9  2001/06/04 18:31:58  swa
# files are now prefixed with either `confdir' or `logdir'.
# `make redhat-dist' replaces both entries confdir and logdir
# with redhat values
#
# Revision 1.8  2001/06/04 10:44:57  swa
# `make redhatr-dist' now works. Except for the paths
# in the config file.
#
# Revision 1.7  2001/06/03 17:09:09  swa
# swa for oes: reversed my earlier change
#
# Revision 1.6  2001/06/03 17:07:27  swa
# swa for oes
#
# Revision 1.5  2001/06/03 13:57:26  swa
# compile cgi.c (for andreas' GUI)
#
# Revision 1.4  2001/05/31 21:18:45  jongfoster
# Added files actions.[ch], actionlist.h, list.[ch] to Makefile
#
# Revision 1.3  2001/05/29 20:02:48  joergs
# Changes for AmigaOS added.
#
# Revision 1.2  2001/05/17 22:23:23  oes
#  - Added auto-generation of CRLFs for Win32 config files
#  - Added comment-prefix to all Win32-only options in the config file
#    and provided auto stripping of this prefix for the Win32 platform by make
#
# Revision 1.1.1.1  2001/05/15 13:59:00  oes
# Initial import of version 2.9.3 source tree
#
#
