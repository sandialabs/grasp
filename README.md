head	4.1;
access;
symbols
	PPPM_Crozier2:4.1.0.4
	PPPM_Crozier:4.1
	Root-of-PPPM_Crozier:4.1
	Monaco:4.1.0.2
	PreMonaco:4.1
	Recovery-2004-04-12:4.0
	BRANCH:4.0.0.6
	PMONACO:4.0.0.4
	ROOT:4.0
	initial:4.0.0.2
	pmonaco:4.0
	Version_4_0:4.0
	Version_3_0:3.0
	BeforeREAXMerge:3.0
	REAX-2:3.0
	P_MonacoDoc:3.0.0.6
	InitialGraspDoc:3.0
	P_Monaco:3.0.0.4
	InitialGrasp:3.0
	REAXFF:3.0.0.2
	cjkimme_version_merge_from_trunk_1:2.2.2.1
	cjkimme_version:2.2.0.2;
locks; strict;
comment	@# @;


4.1
date	2005.05.02.22.44.55;	author athomps;	state Exp;
branches;
next	4.0;

4.0
date	2005.04.05.16.43.30;	author athomps;	state Exp;
branches;
next	3.0;

3.0
date	2004.09.22.17.56.49;	author athomps;	state Exp;
branches;
next	2.2;

2.2
date	2004.03.02.17.49.42;	author athomps;	state Exp;
branches
	2.2.2.1;
next	2.1;

2.1
date	2004.03.01.18.13.03;	author athomps;	state Exp;
branches;
next	2.0;

2.0
date	2003.11.26.22.55.34;	author athomps;	state Exp;
branches;
next	;

2.2.2.1
date	2004.09.13.23.05.46;	author cjkimme;	state Exp;
branches;
next	;


desc
@@


4.1
log
@Recovered from corruption that occurred on April 12, 2005.
@
text
@#-----------------------------------------------------------------------
#
#   G R A S P
#   ____________
#   \           \
#    \ General   \
#     \ Reactive  \
#      \ Atomistic \
#       \ Simulation\
#        \ Program   \
#         \___________\
#
#    Timestamp: April 1, 2005
#    Version: 4.0
#
#    Primary Author: Aidan P. Thompson
#
#    e-mail: athomps@@sandia.gov
#   
#    Copyright (2005) Sandia National Laboratories
#
#    Sandia National Laboratories is a multiprogram laboratory 
#    operated by Sandia Corporation, a Lockheed Martin company,
#    for the United States Department of Energy under contract
#    No. DE-AC04-94AL-85000.
#
#    Under the terms of Contract DE-AC04-94AL85000 with Sandia 
#    Corporation, the U.S. Government retains certain rights in 
#    this software.
#
#    This software is distributed under the terms of the GNU Public 
#    License (GPL). For a copy of the GPL see the file 
#    Grasp/Documentation/LICENSE or visit the GNU website at 
#    http://www.gnu.org/copyleft/gpl.html. Briefly, the GPL 
#    entitles you to use the software, modify it and redistribute it. 
#    The main thing you can not do is apply any other licensing 
#    terms to the software. Also, if any part of this sofware is added 
#    to other software, then that software must also be released under 
#    the GPL.
#
#    This software comes with no warranty of any kind. 
#
#------------------------------------------------------------------------ 
#///:EOH~
This file contains instructions for compiling
and testing GRASP on your machine.

For more information on the code, see Documentation/manual.txt.

To compile and test the source code, go to the directory Grasp/Source.

The GRASP source code and examples in this distribution
should be compiled and run using the GNU make command.
If you don't have this, and can't get it, you
will need special help.

make reads the file Makefile, which  contains compile options for 
a variety of platforms and compilation styles. To select a platform/style, 
set the environment variable _ARCH in the make command to one of the values 
specified in the Makefile.

For example:

make _ARCH=cplant

You can see the currently available choices by doing a
grep on Makefile for _ARCH. 

grep _ARCH Makefile

The default value is osx.

If you set _ARCH to a value not specified in Makefile, you
will see an nasty error containing the string Invalid_ARCH_value.

You will probably need to set up compile options for your machine
by creating a new block in the Makefile. Copy the one closest
to your machine and give it a new _ARCH value. 
You can also set the default value of _ARCH to this value.
Once you have done this, you do not need to specify _ARCH.

To compile the code:

	make _ARCH=<your _ARCH value here>

or if the default _ARCH value is desired:

	make

To compile with the ReaxFF force field:

	make _REAX=true

This compiles and links additional ReaxFF source files, 
which are written in Fortran, 
and so requires a little bit more customization that the standard compile.
By agreement with Adri van Duin, ReaxFF is only provided to *authorized* 
users. Please contact Adri at duin@@wag.caltech.edu if you wish to become 
authorized.
 
To run a test example, type 

	make _ARCH=<your _ARCH value here> testa

The results of the example will be written to $(TESTDIR)/testa.

To run all the test examples, type

        make _ARCH=<your _ARCH value here> testall

The compiled objects are stored in subdirectories named Obj_$(_ARCH).
You can clean up objects by simply deleting the subdirectory,
or else by typing

make ARCH=<your _ARCH value here> clean

If the _REAX=true flag is used, objects are stored in Obj_$(_ARCH)_reax.



@


4.0
log
@This is version 4.0
@
text
@d94 6
a99 2
This compiles and links the Caltech Fortran code files in SourceReax, and
so requires a little bit more customization that the standard compile.
@


3.0
log
@Created a new version number, GRASP 3.0
@
text
@d1 44
d53 1
a53 1
are be compiled and run using the GNU make command.
d58 3
a60 3
a variety of platforms and compilation styles. To select which one, 
set the environment variable _ARCH in the make command to one of 
the specified in the Makefile.
d64 1
a64 1
make all _ARCH=cplant
d67 6
a72 1
grep on Makefile for _ARCH. The default value is sale811.
d78 1
a78 1
to your machine and give it a new _ARCH setting. 
d84 3
a86 1
	make all
d88 9
d99 1
a99 1
	make testa
d105 1
a105 1
        make testall
d111 3
a113 1
make _ARCH=<your _ARCH value here> clean
@


2.2
log
@Updated documentation
@
text
@@


2.2.2.1
log
@Adding Documentation and Testing directories to branch cjkimme_version
@
text
@@


2.1
log
@Added Ewald sum example
@
text
@d6 2
d9 1
a9 1
can be compiled and run using the GNU make command.
d13 19
a31 9
************************************************
THIS IS OUTDATED. NEW DESCRIPTION NEEDED
************************************************

This distribution contains two make files. 

Makefile is the generic version, intended for Linux 
workstations, Linux clusters and the like.  The 
following variables are system specific:
d33 1
a33 5
	MPI_PATH - The root directory for the MPI installation
	GRASPDIR - The directory containing the directory Source

Once these are set, you should be able to compile the
binary grasp.exe by moving to the direcotry Source and typing
d41 1
a41 2
The results of the example will be written to Grasp/Testing/testa.

d43 1
a43 2
Makefile.cplant is customized for use on Cplant.   The 
following variables are user specific:
d45 1
a45 2
	GRASPDIR - The directory containing the directory Source
	ENFSDIR - The directory Testing on the enfs filesystem
d47 3
a49 2
Once these are set, you should be able to compile the
binary grasp.exe by moving to the directory Source and typing
d51 1
a51 8
	make all

To run a test example, first copy the directory Testing to
the enfs filesystem, and make sure ENFSDIR in Makefile.cplant
matches the new location.  Then go to the directory Source
and type 

	make testa
a52 2
The results of the example will be written to $(ENFSDIR)/testa.
**************************************************************8
@


2.0
log
@Moved README from . to ./Documentation
@
text
@d7 8
a14 1
can be compiled and run using the make command.
d55 1
a55 1

@

