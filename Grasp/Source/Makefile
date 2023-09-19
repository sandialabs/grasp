#-----------------------------------------------------------------------
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
#    e-mail: athomps@sandia.gov
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
#----------------------------------------------------------------------- 
#///:EOH~
#
# This file jumps into the architecture-specific directory and 
# builds a program.
#
# This is taken from Paul Smith, GNU make Guru.
#
SHELL = /bin/tcsh
# The variable DEFAULT_ARCH contains the default value.
# To choose a different value, users can edit this, 
# or invoke make using
#     make _ARCH = <user's architecture>
#
# To add-in ReaxFF code use:
#     make _REAX=true _ARCH = <user's architecture>
DEFAULT_ARCH = osx

ifeq (,$(filter Obj_%,$(notdir $(CURDIR))))
  include Makefile.target
else
#----- End Boilerplate

  # VPATH tells make where to look for source files.

  VPATH	= $(SRCDIR):$(SRCDIR)/SourceReax

  # Make the first (i.e. default) target all, 
  # then use special target to declare it phony to 
  # avoid searching for non-existent file
  all::
  .PHONY: all

  # This section is for the architecture-specific target directory

  # This sub-section is common to all architectures

  all:: grasp.exe

  SRC_MD = \
	box.cpp \
	comm.cpp \
	conjugate_gradient.cpp \
	constants.cpp \
	controller.cpp \
	fix_none.cpp \
	fix_temp_rescale.cpp \
	fixed_object.cpp \
	fixlist.cpp \
	grain_boundary_minimizer.cpp \
	input.cpp \
	input_string.cpp \
	interfacial_minimizer.cpp \
	log.cpp \
	minimizer.cpp \
	neighbor.cpp \
	output.cpp \
	particle.cpp \
	particlelist.cpp \
	particletypelist.cpp \
	profile.cpp \
	stepper.cpp 
  SRC_FF = \
        force_chargeequil.cpp \
	force_eam.cpp \
	force_ewald.cpp \
	force_external.cpp \
	force_pppm.cpp \
	force_reax.cpp \
	force_tersoff.cpp \
	force_threebody.cpp \
	force_twobody.cpp \
	forcefield.cpp \
	forcefieldlist.cpp 
  SRC_REAX = \
	reax_reac.F \
	reax_poten.F \
	reax_inout.F \
	reax_connect.F \
	reax_charges.F \
	reax_grasp.F
  SRC = $(SRC_MD) $(SRC_FF)
  OBJ = $(SRC:.cpp=.o)
  OBJ_REAX = $(SRC_REAX:.F=.o)

  # This flag can be used to enable non-reax compilation
  #
  #	e.g. make _REAX=true
  #
  # Need to touch the following files:
  #            force_chargeequil.cpp force_reax.cpp \
  #            particlelist.cpp forcefieldlist.cpp
  #
  _REAX = false
  ifeq (true,$(_REAX))
	MESSAGE = 'Using ReaxFF'
        REAX_PREPROC_FLAG = -DUSE_REAX
  endif

  SRC_MAIN = grasp_run.cpp
  OBJ_MAIN =  $(SRC_MAIN:.cpp=.o)

  #
  # Note: here are the preprocessor flags
  # These need to be applied manually, using e.g. 
  #
  # touch log.cpp; make
  #
  # log.cpp: NO_FULLPATH NO_STDERR NO_LOGFILE
  # comm.cpp: USE_NODEFILES
  # controller.cpp: NO_STDERR
  # input_string.cpp: GNU_2_96
  # force_chargeequil.cpp: USE_REAX
  # force_reax.cpp: USE_REAX
  # particlelist.cpp: USE_REAX
  # forcefieldlist.cpp: USE_REAX
  # 

  # This subsection contains branches to specific architecture definitions
  #
  # Initialize with values that are guaranteed to fail
  # This is used to catch invalid _ARCH values
    C = "Invalid_ARCH_value"
    CPP = "Invalid_ARCH_value"
    F77 = "Invalid_ARCH_value"
  # Add null defaults for rarely used variables
    RUNDIR = .
    SLEEP = 
    POSTLINK = 

    include $(SRCDIR)/Makefile.arch
  #
  # If ReaxFF not required, turn off references to code and fortran libraries
  #
  ifneq (true,$(_REAX))
        OBJ_REAX = 
        USRLIB_REAX = 
  endif

  # This sub-section is common to all architectures
  .SUFFIXES : .o .cpp .c .F
  # inference rule to get .o targets from .cpp prerequisites
  .cpp.o :
	$(CPP) $(CPPFLAGS) -c $<
  # inference rule to get .o targets from .c prerequisites
  .c.o :
	$(C) $(CFLAGS) -c $<
  # inference rule to get .o targets from .F prerequisites
  .F.o :
	$(F77) $(FFLAGS)  -c $<
  # Target rules for executables specify prerequisite objects
  # and linking command.

  grasp.exe: $(OBJ_MAIN) $(OBJ) $(OBJ_REAX)
	$(CPP) $(OFLAG) $(OBJ_MAIN) $(OBJ) $(OBJ_REAX) \
	$(USRLIB) $(USRLIB_REAX) -o grasp.exe; \
	$(POSTLINK);

  lib: $(OBJ)
	ar -r libgrasp.a $(OBJ) $(OBJ_REAX)

  include $(SRCDIR)/Makefile.testing

  tar_full:
# Excludes Reax source files
	mkdir Grasp_tmp; \
	cd Grasp_tmp; \
	cvs -d :ext:athomps@software.sandia.gov:/space/CVS-grasp \
		checkout Grasp; \
	\rm -r Grasp/CVS; \
	\rm -r Grasp/Documentation/CVS; \
	\rm -r Grasp/Source/CVS; \
	\rm -r Grasp/Source/STUBS/CVS; \
	\rm -r Grasp/Source/SourceReax/CVS; \
	\rm -r Grasp/Testing/CVS; \
	\rm -r Grasp/Testing/*/CVS; \
	\rm -r Grasp/Source/SourceReax; \
	\rm -r Grasp/Testing/testreax; \
	\rm -r Grasp/Testing/testrdx; \
	\rm -r Grasp/Testing/testrdx_reax; \
	\rm -r Grasp/Testing/testrdx222; \
	tar -zcvf Grasp.tgz Grasp/Source \
	Grasp/Documentation/manual.txt \
	Grasp/Documentation/README \
	Grasp/Documentation/LICENSE \
        Grasp/Testing; \
	mv Grasp.tgz $(SRCDIR); \
	cd .. ; \
	\rm -r Grasp_tmp;

  tar_full_reax:
# Includes Reax source files
	mkdir Grasp_tmp; \
	cd Grasp_tmp; \
	cvs -d :ext:athomps@software.sandia.gov:/space/CVS-grasp \
		checkout Grasp; \
	\rm -r Grasp/CVS; \
	\rm -r Grasp/Documentation/CVS; \
	\rm -r Grasp/Source/CVS; \
	\rm -r Grasp/Source/STUBS/CVS; \
	\rm -r Grasp/Source/SourceReax/CVS; \
	\rm -r Grasp/Testing/CVS; \
	\rm -r Grasp/Testing/*/CVS; \
	tar -zcvf Grasp_reax.tgz Grasp/Source \
	Grasp/Documentation/manual.txt \
	Grasp/Documentation/README \
	Grasp/Documentation/LICENSE \
        Grasp/Testing; \
	mv Grasp_reax.tgz $(SRCDIR); \
	cd .. ; \
	\rm -r Grasp_tmp;

   # Individual dependencies

   DEPENDS = $(OBJ:.o=.d)
   -include $(DEPENDS)
   $(OBJ_REAX): reax_defs.h  \
	cbka.blk		cbkdhdc.blk		cbknuval.blk \
	cbkabo.blk              cbkdistan.blk           cbknvlbo.blk \
	cbkatomcoord.blk        cbkdrdc.blk             cbkpairs.blk \
	cbkbo.blk               cbkefield.blk           cbkpres.blk \
	cbkboncor.blk           cbkenergies.blk         cbkqa.blk \
	cbkbopi.blk             cbkeregime.blk          \
	cbkbopi2.blk            cbkff.blk               cbkrbo.blk \
	cbkbosi.blk             cbkfftorang.blk         cbkrestr.blk \
	cbkc.blk                cbkh.blk                cbksrtbon1.blk \
	cbkch.blk               cbkhbond.blk            cbksrthb.blk \
	cbkcha.blk              cbkia.blk               cbktorang.blk \
	cbkcharmol.blk          cbkidbo.blk             cbktorsion.blk \
	cbkchb.blk              cbkimove.blk            cbktregime.blk \
	cbkconst.blk            cbkinit.blk             cbkvalence.blk \
	cbkcovbon.blk           cbklonpar.blk           cbkvregime.blk \
	cbkd.blk                cellcoord.blk \
	cbkdbodc.blk            cbkmolec.blk            control.blk \
	cbkdbopi2ndc.blk        cbknmolat.blk           opt.blk \
	cbkdbopidc.blk          small.blk \
	cbkdbopindc.blk         cbknonbon.blk           valang.blk \
	cbkdcell.blk            cbknubon2.blk 

#----- Begin Boilerplate
endif
