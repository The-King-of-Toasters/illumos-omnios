#
# CDDL HEADER START
#
# The contents of this file are subject to the terms of the
# Common Development and Distribution License (the "License").
# You may not use this file except in compliance with the License.
#
# You can obtain a copy of the license at usr/src/OPENSOLARIS.LICENSE
# or http://www.opensolaris.org/os/licensing.
# See the License for the specific language governing permissions
# and limitations under the License.
#
# When distributing Covered Code, include this CDDL HEADER in each
# file and include the License file at usr/src/OPENSOLARIS.LICENSE.
# If applicable, add the following below this CDDL HEADER, with the
# fields enclosed by brackets "[]" replaced with your own identifying
# information: Portions Copyright [yyyy] [name of copyright owner]
#
# CDDL HEADER END
#
#
# Copyright 2006 Sun Microsystems, Inc.  All rights reserved.
# Use is subject to license terms.
#
# Copyright 2017 Joyent, Inc.
#

FSTYPE=		udfs
LIBPROG=	labelit
SRCS=		labelit.c
ATTMK=		$(LIBPROG)

include		../../Makefile.fstype

LDLIBS += -ladm
CPPFLAGS += -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64
CPPFLAGS += -I../fstyp

LINTFLAGS += -xerroff=E_STATIC_UNUSED
LINTFLAGS64 += -xerroff=E_STATIC_UNUSED

CERRWARN += -_gcc=-Wno-unused-function
CERRWARN += $(CNOWARN_UNINIT)

labelit : labelit.o  ud_lib.o
	$(LINK.c) -o $@ labelit.o ud_lib.o $(LDLIBS)
	$(POST_PROCESS)

labelit.o : labelit.c ../fstyp/ud_lib.h

ud_lib.o : ../fstyp/ud_lib.c ../fstyp/ud_lib.h
	$(COMPILE.c) -o $@ ../fstyp/ud_lib.c
	$(POST_PROCESS_O)

# for messaging catalog
#
POFILE= labelit.po

# for messaging catalog
#
catalog:	$(POFILE)

$(POFILE):	$(SRCS) ../fstyp/ud_lib.h
	$(RM) $@
	$(COMPILE.cpp) $(SRCS) > $(POFILE).i
	$(XGETTEXT) $(XGETFLAGS) $(POFILE).i
	sed "/^domain/d" messages.po > $@
	$(RM) $(POFILE).i messages.po

clean :
	rm -f labelit.o ud_lib.o

lint:
	$(LINT.c) -mu $(SRCS)

putback:
	-cstyle -p labelit.c
	-keywords labelit.c
	-keywords Makefile
