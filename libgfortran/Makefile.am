## Process this file with automake to produce Makefile.in


ACLOCAL_AMFLAGS = -I .. -I ../config

## May be used by toolexeclibdir.
gcc_version := $(shell @get_gcc_base_ver@ $(top_srcdir)/../gcc/BASE-VER)

## Symbol versioning (copied from libssp).
if LIBGFOR_USE_SYMVER
if LIBGFOR_USE_SYMVER_GNU
version_arg = -Wl,--version-script=$(srcdir)/gfortran.map
version_dep = $(srcdir)/gfortran.map
endif
if LIBGFOR_USE_SYMVER_SUN
version_arg = -Wl,-M,gfortran.map-sun
version_dep = gfortran.map-sun
gfortran.map-sun : $(srcdir)/gfortran.map \
		$(top_srcdir)/../contrib/make_sunver.pl \
		$(libgfortran_la_OBJECTS) $(libgfortran_la_LIBADD)
	perl $(top_srcdir)/../contrib/make_sunver.pl \
	  $(srcdir)/gfortran.map \
	  $(libgfortran_la_OBJECTS:%.lo=.libs/%.o) \
	 `echo $(libgfortran_la_LIBADD) | \
	    sed 's,/\([^/.]*\)\.la,/.libs/\1.a,g'` \
	 > $@ || (rm -f $@ ; exit 1)
endif
else
version_arg =
version_dep =
endif

LTLDFLAGS = $(shell $(SHELL) $(top_srcdir)/../libtool-ldflags $(LDFLAGS)) \
	    $(lt_host_flags)

toolexeclib_LTLIBRARIES = libgfortran.la
toolexeclib_DATA = libgfortran.spec
libgfortran_la_LINK = $(LINK) $(libgfortran_la_LDFLAGS)
libgfortran_la_LDFLAGS = -version-info `grep -v '^\#' $(srcdir)/libtool-version` \
	$(LTLDFLAGS) $(LIBQUADLIB) ../libbacktrace/libbacktrace.la \
	$(HWCAP_LDFLAGS) \
	-lm $(extra_ldflags_libgfortran) \
	$(version_arg) -Wc,-shared-libgcc
libgfortran_la_DEPENDENCIES = $(version_dep) libgfortran.spec $(LIBQUADLIB_DEP)

cafexeclib_LTLIBRARIES = libcaf_single.la
cafexeclibdir = $(libdir)/gcc/$(target_alias)/$(gcc_version)$(MULTISUBDIR)
libcaf_single_la_SOURCES = caf/single.c
libcaf_single_la_LDFLAGS = -static
libcaf_single_la_DEPENDENCIES = caf/libcaf.h
libcaf_single_la_LINK = $(LINK) $(libcaf_single_la_LDFLAGS)

if IEEE_SUPPORT
fincludedir = $(libdir)/gcc/$(target_alias)/$(gcc_version)$(MULTISUBDIR)/finclude
nodist_finclude_HEADERS = ieee_arithmetic.mod ieee_exceptions.mod ieee_features.mod
endif

## io.h conflicts with a system header on some platforms, so
## use -iquote
AM_CPPFLAGS = -iquote$(srcdir)/io -I$(srcdir)/$(MULTISRCTOP)../gcc \
	      -I$(srcdir)/$(MULTISRCTOP)../gcc/config $(LIBQUADINCLUDE) \
	      -I$(MULTIBUILDTOP)../../$(host_subdir)/gcc \
	      -I$(srcdir)/$(MULTISRCTOP)../libgcc \
	      -I$(MULTIBUILDTOP)../libgcc \
	      -I$(srcdir)/$(MULTISRCTOP)../libbacktrace \
	      -I$(MULTIBUILDTOP)../libbacktrace \
	      -I../libbacktrace

# Fortran rules for complex multiplication and division
AM_CFLAGS += -fcx-fortran-rules

# Use -ffunction-sections -fdata-sections if supported by the compiler
AM_CFLAGS += $(SECTION_FLAGS)

# Some targets require additional compiler options for IEEE compatibility.
AM_CFLAGS += $(IEEE_FLAGS)
AM_FCFLAGS += $(IEEE_FLAGS)

if LIBGFOR_MINIMAL
AM_CFLAGS += -DLIBGFOR_MINIMAL
endif

gfor_io_src= \
io/size_from_kind.c

if !LIBGFOR_MINIMAL

gfor_io_src+= \
io/close.c \
io/file_pos.c \
io/format.c \
io/inquire.c \
io/intrinsics.c \
io/list_read.c \
io/lock.c \
io/open.c \
io/read.c \
io/transfer.c \
io/transfer128.c \
io/unit.c \
io/unix.c \
io/write.c \
io/fbuf.c

endif

gfor_io_headers= \
io/io.h \
io/fbuf.h \
io/format.h \
io/unix.h

gfor_helper_src= \
intrinsics/associated.c \
intrinsics/abort.c \
intrinsics/args.c \
intrinsics/cshift0.c \
intrinsics/eoshift0.c \
intrinsics/eoshift2.c \
intrinsics/erfc_scaled.c \
intrinsics/extends_type_of.c \
intrinsics/fnum.c \
intrinsics/ierrno.c \
intrinsics/ishftc.c \
intrinsics/mvbits.c \
intrinsics/move_alloc.c \
intrinsics/pack_generic.c \
intrinsics/selected_char_kind.c \
intrinsics/size.c \
intrinsics/spread_generic.c \
intrinsics/string_intrinsics.c \
intrinsics/rand.c \
intrinsics/random.c \
intrinsics/reshape_generic.c \
intrinsics/reshape_packed.c \
intrinsics/selected_int_kind.f90 \
intrinsics/selected_real_kind.f90 \
intrinsics/unpack_generic.c \
runtime/in_pack_generic.c \
runtime/in_unpack_generic.c

if !LIBGFOR_MINIMAL

gfor_helper_src+= \
intrinsics/access.c \
intrinsics/c99_functions.c \
intrinsics/chdir.c \
intrinsics/chmod.c \
intrinsics/clock.c \
intrinsics/cpu_time.c \
intrinsics/ctime.c \
intrinsics/date_and_time.c \
intrinsics/dtime.c \
intrinsics/env.c \
intrinsics/etime.c \
intrinsics/execute_command_line.c \
intrinsics/exit.c \
intrinsics/gerror.c \
intrinsics/getcwd.c \
intrinsics/getlog.c \
intrinsics/getXid.c \
intrinsics/hostnm.c \
intrinsics/kill.c \
intrinsics/link.c \
intrinsics/perror.c \
intrinsics/signal.c \
intrinsics/sleep.c \
intrinsics/system.c \
intrinsics/rename.c \
intrinsics/stat.c \
intrinsics/symlnk.c \
intrinsics/system_clock.c \
intrinsics/time.c \
intrinsics/umask.c \
intrinsics/unlink.c

endif

if IEEE_SUPPORT

gfor_helper_src+=ieee/ieee_helper.c

gfor_ieee_src= \
ieee/ieee_arithmetic.F90 \
ieee/ieee_exceptions.F90 \
ieee/ieee_features.F90

else

gfor_ieee_src=

endif

gfor_src= \
runtime/bounds.c \
runtime/compile_options.c \
runtime/memory.c \
runtime/string.c \
runtime/select.c

if LIBGFOR_MINIMAL

gfor_src+= runtime/minimal.c

else

gfor_src+= \
runtime/backtrace.c \
runtime/convert_char.c \
runtime/environ.c \
runtime/error.c \
runtime/fpu.c \
runtime/main.c \
runtime/pause.c \
runtime/stop.c

endif

i_all_c= \
$(srcdir)/generated/all_l1.c \
$(srcdir)/generated/all_l2.c \
$(srcdir)/generated/all_l4.c \
$(srcdir)/generated/all_l8.c \
$(srcdir)/generated/all_l16.c

i_any_c= \
$(srcdir)/generated/any_l1.c \
$(srcdir)/generated/any_l2.c \
$(srcdir)/generated/any_l4.c \
$(srcdir)/generated/any_l8.c \
$(srcdir)/generated/any_l16.c

i_bessel_c= \
$(srcdir)/generated/bessel_r4.c \
$(srcdir)/generated/bessel_r8.c \
$(srcdir)/generated/bessel_r10.c \
$(srcdir)/generated/bessel_r16.c

i_count_c= \
$(srcdir)/generated/count_1_l.c \
$(srcdir)/generated/count_2_l.c \
$(srcdir)/generated/count_4_l.c \
$(srcdir)/generated/count_8_l.c \
$(srcdir)/generated/count_16_l.c

i_iall_c= \
$(srcdir)/generated/iall_i1.c \
$(srcdir)/generated/iall_i2.c \
$(srcdir)/generated/iall_i4.c \
$(srcdir)/generated/iall_i8.c \
$(srcdir)/generated/iall_i16.c

i_iany_c= \
$(srcdir)/generated/iany_i1.c \
$(srcdir)/generated/iany_i2.c \
$(srcdir)/generated/iany_i4.c \
$(srcdir)/generated/iany_i8.c \
$(srcdir)/generated/iany_i16.c

i_iparity_c= \
$(srcdir)/generated/iparity_i1.c \
$(srcdir)/generated/iparity_i2.c \
$(srcdir)/generated/iparity_i4.c \
$(srcdir)/generated/iparity_i8.c \
$(srcdir)/generated/iparity_i16.c

i_maxloc0_c= \
$(srcdir)/generated/maxloc0_4_i1.c \
$(srcdir)/generated/maxloc0_8_i1.c \
$(srcdir)/generated/maxloc0_16_i1.c \
$(srcdir)/generated/maxloc0_4_i2.c \
$(srcdir)/generated/maxloc0_8_i2.c \
$(srcdir)/generated/maxloc0_16_i2.c \
$(srcdir)/generated/maxloc0_4_i4.c \
$(srcdir)/generated/maxloc0_8_i4.c \
$(srcdir)/generated/maxloc0_16_i4.c \
$(srcdir)/generated/maxloc0_4_i8.c \
$(srcdir)/generated/maxloc0_8_i8.c \
$(srcdir)/generated/maxloc0_16_i8.c \
$(srcdir)/generated/maxloc0_4_i16.c \
$(srcdir)/generated/maxloc0_8_i16.c \
$(srcdir)/generated/maxloc0_16_i16.c \
$(srcdir)/generated/maxloc0_4_r4.c \
$(srcdir)/generated/maxloc0_8_r4.c \
$(srcdir)/generated/maxloc0_16_r4.c \
$(srcdir)/generated/maxloc0_4_r8.c \
$(srcdir)/generated/maxloc0_8_r8.c \
$(srcdir)/generated/maxloc0_16_r8.c \
$(srcdir)/generated/maxloc0_4_r10.c \
$(srcdir)/generated/maxloc0_8_r10.c \
$(srcdir)/generated/maxloc0_16_r10.c \
$(srcdir)/generated/maxloc0_4_r16.c \
$(srcdir)/generated/maxloc0_8_r16.c \
$(srcdir)/generated/maxloc0_16_r16.c

i_maxloc0s_c = \
$(srcdir)/generated/maxloc0_4_s1.c \
$(srcdir)/generated/maxloc0_4_s4.c \
$(srcdir)/generated/maxloc0_8_s1.c \
$(srcdir)/generated/maxloc0_8_s4.c \
$(srcdir)/generated/maxloc0_16_s1.c \
$(srcdir)/generated/maxloc0_16_s4.c

i_maxloc1_c= \
$(srcdir)/generated/maxloc1_4_i1.c \
$(srcdir)/generated/maxloc1_8_i1.c \
$(srcdir)/generated/maxloc1_16_i1.c \
$(srcdir)/generated/maxloc1_4_i2.c \
$(srcdir)/generated/maxloc1_8_i2.c \
$(srcdir)/generated/maxloc1_16_i2.c \
$(srcdir)/generated/maxloc1_4_i4.c \
$(srcdir)/generated/maxloc1_8_i4.c \
$(srcdir)/generated/maxloc1_16_i4.c \
$(srcdir)/generated/maxloc1_4_i8.c \
$(srcdir)/generated/maxloc1_8_i8.c \
$(srcdir)/generated/maxloc1_16_i8.c \
$(srcdir)/generated/maxloc1_4_i16.c \
$(srcdir)/generated/maxloc1_8_i16.c \
$(srcdir)/generated/maxloc1_16_i16.c \
$(srcdir)/generated/maxloc1_4_r4.c \
$(srcdir)/generated/maxloc1_8_r4.c \
$(srcdir)/generated/maxloc1_16_r4.c \
$(srcdir)/generated/maxloc1_4_r8.c \
$(srcdir)/generated/maxloc1_8_r8.c \
$(srcdir)/generated/maxloc1_16_r8.c \
$(srcdir)/generated/maxloc1_4_r10.c \
$(srcdir)/generated/maxloc1_8_r10.c \
$(srcdir)/generated/maxloc1_16_r10.c \
$(srcdir)/generated/maxloc1_4_r16.c \
$(srcdir)/generated/maxloc1_8_r16.c \
$(srcdir)/generated/maxloc1_16_r16.c

i_maxloc1s_c= \
$(srcdir)/generated/maxloc1_4_s1.c \
$(srcdir)/generated/maxloc1_4_s4.c \
$(srcdir)/generated/maxloc1_8_s1.c \
$(srcdir)/generated/maxloc1_8_s4.c \
$(srcdir)/generated/maxloc1_16_s1.c \
$(srcdir)/generated/maxloc1_16_s4.c

i_maxloc2s_c= \
$(srcdir)/generated/maxloc2_4_s1.c \
$(srcdir)/generated/maxloc2_4_s4.c \
$(srcdir)/generated/maxloc2_8_s1.c \
$(srcdir)/generated/maxloc2_8_s4.c \
$(srcdir)/generated/maxloc2_16_s1.c \
$(srcdir)/generated/maxloc2_16_s4.c

i_maxval_c= \
$(srcdir)/generated/maxval_i1.c \
$(srcdir)/generated/maxval_i2.c \
$(srcdir)/generated/maxval_i4.c \
$(srcdir)/generated/maxval_i8.c \
$(srcdir)/generated/maxval_i16.c \
$(srcdir)/generated/maxval_r4.c \
$(srcdir)/generated/maxval_r8.c \
$(srcdir)/generated/maxval_r10.c \
$(srcdir)/generated/maxval_r16.c

i_maxval0s_c=\
$(srcdir)/generated/maxval0_s1.c \
$(srcdir)/generated/maxval0_s4.c

i_maxval1s_c=\
$(srcdir)/generated/maxval1_s1.c \
$(srcdir)/generated/maxval1_s4.c

i_minloc0_c= \
$(srcdir)/generated/minloc0_4_i1.c \
$(srcdir)/generated/minloc0_8_i1.c \
$(srcdir)/generated/minloc0_16_i1.c \
$(srcdir)/generated/minloc0_4_i2.c \
$(srcdir)/generated/minloc0_8_i2.c \
$(srcdir)/generated/minloc0_16_i2.c \
$(srcdir)/generated/minloc0_4_i4.c \
$(srcdir)/generated/minloc0_8_i4.c \
$(srcdir)/generated/minloc0_16_i4.c \
$(srcdir)/generated/minloc0_4_i8.c \
$(srcdir)/generated/minloc0_8_i8.c \
$(srcdir)/generated/minloc0_16_i8.c \
$(srcdir)/generated/minloc0_4_i16.c \
$(srcdir)/generated/minloc0_8_i16.c \
$(srcdir)/generated/minloc0_16_i16.c \
$(srcdir)/generated/minloc0_4_r4.c \
$(srcdir)/generated/minloc0_8_r4.c \
$(srcdir)/generated/minloc0_16_r4.c \
$(srcdir)/generated/minloc0_4_r8.c \
$(srcdir)/generated/minloc0_8_r8.c \
$(srcdir)/generated/minloc0_16_r8.c \
$(srcdir)/generated/minloc0_4_r10.c \
$(srcdir)/generated/minloc0_8_r10.c \
$(srcdir)/generated/minloc0_16_r10.c \
$(srcdir)/generated/minloc0_4_r16.c \
$(srcdir)/generated/minloc0_8_r16.c \
$(srcdir)/generated/minloc0_16_r16.c

i_minloc0s_c = \
$(srcdir)/generated/minloc0_4_s1.c \
$(srcdir)/generated/minloc0_4_s4.c \
$(srcdir)/generated/minloc0_8_s1.c \
$(srcdir)/generated/minloc0_8_s4.c \
$(srcdir)/generated/minloc0_16_s1.c \
$(srcdir)/generated/minloc0_16_s4.c

i_minloc1_c= \
$(srcdir)/generated/minloc1_4_i1.c \
$(srcdir)/generated/minloc1_8_i1.c \
$(srcdir)/generated/minloc1_16_i1.c \
$(srcdir)/generated/minloc1_4_i2.c \
$(srcdir)/generated/minloc1_8_i2.c \
$(srcdir)/generated/minloc1_16_i2.c \
$(srcdir)/generated/minloc1_4_i4.c \
$(srcdir)/generated/minloc1_8_i4.c \
$(srcdir)/generated/minloc1_16_i4.c \
$(srcdir)/generated/minloc1_4_i8.c \
$(srcdir)/generated/minloc1_8_i8.c \
$(srcdir)/generated/minloc1_16_i8.c \
$(srcdir)/generated/minloc1_4_i16.c \
$(srcdir)/generated/minloc1_8_i16.c \
$(srcdir)/generated/minloc1_16_i16.c \
$(srcdir)/generated/minloc1_4_r4.c \
$(srcdir)/generated/minloc1_8_r4.c \
$(srcdir)/generated/minloc1_16_r4.c \
$(srcdir)/generated/minloc1_4_r8.c \
$(srcdir)/generated/minloc1_8_r8.c \
$(srcdir)/generated/minloc1_16_r8.c \
$(srcdir)/generated/minloc1_4_r10.c \
$(srcdir)/generated/minloc1_8_r10.c \
$(srcdir)/generated/minloc1_16_r10.c \
$(srcdir)/generated/minloc1_4_r16.c \
$(srcdir)/generated/minloc1_8_r16.c \
$(srcdir)/generated/minloc1_16_r16.c

i_minloc1s_c= \
$(srcdir)/generated/minloc1_4_s1.c \
$(srcdir)/generated/minloc1_4_s4.c \
$(srcdir)/generated/minloc1_8_s1.c \
$(srcdir)/generated/minloc1_8_s4.c \
$(srcdir)/generated/minloc1_16_s1.c \
$(srcdir)/generated/minloc1_16_s4.c

i_minloc2s_c= \
$(srcdir)/generated/minloc2_4_s1.c \
$(srcdir)/generated/minloc2_4_s4.c \
$(srcdir)/generated/minloc2_8_s1.c \
$(srcdir)/generated/minloc2_8_s4.c \
$(srcdir)/generated/minloc2_16_s1.c \
$(srcdir)/generated/minloc2_16_s4.c

i_minval_c= \
$(srcdir)/generated/minval_i1.c \
$(srcdir)/generated/minval_i2.c \
$(srcdir)/generated/minval_i4.c \
$(srcdir)/generated/minval_i8.c \
$(srcdir)/generated/minval_i16.c \
$(srcdir)/generated/minval_r4.c \
$(srcdir)/generated/minval_r8.c \
$(srcdir)/generated/minval_r10.c \
$(srcdir)/generated/minval_r16.c

i_minval0s_c=\
$(srcdir)/generated/minval0_s1.c \
$(srcdir)/generated/minval0_s4.c

i_minval1s_c=\
$(srcdir)/generated/minval1_s1.c \
$(srcdir)/generated/minval1_s4.c

i_norm2_c= \
$(srcdir)/generated/norm2_r4.c \
$(srcdir)/generated/norm2_r8.c \
$(srcdir)/generated/norm2_r10.c \
$(srcdir)/generated/norm2_r16.c

i_parity_c = \
$(srcdir)/generated/parity_l1.c \
$(srcdir)/generated/parity_l2.c \
$(srcdir)/generated/parity_l4.c \
$(srcdir)/generated/parity_l8.c \
$(srcdir)/generated/parity_l16.c

i_sum_c= \
$(srcdir)/generated/sum_i1.c \
$(srcdir)/generated/sum_i2.c \
$(srcdir)/generated/sum_i4.c \
$(srcdir)/generated/sum_i8.c \
$(srcdir)/generated/sum_i16.c \
$(srcdir)/generated/sum_r4.c \
$(srcdir)/generated/sum_r8.c \
$(srcdir)/generated/sum_r10.c \
$(srcdir)/generated/sum_r16.c \
$(srcdir)/generated/sum_c4.c \
$(srcdir)/generated/sum_c8.c \
$(srcdir)/generated/sum_c10.c \
$(srcdir)/generated/sum_c16.c

i_product_c= \
$(srcdir)/generated/product_i1.c \
$(srcdir)/generated/product_i2.c \
$(srcdir)/generated/product_i4.c \
$(srcdir)/generated/product_i8.c \
$(srcdir)/generated/product_i16.c \
$(srcdir)/generated/product_r4.c \
$(srcdir)/generated/product_r8.c \
$(srcdir)/generated/product_r10.c \
$(srcdir)/generated/product_r16.c \
$(srcdir)/generated/product_c4.c \
$(srcdir)/generated/product_c8.c \
$(srcdir)/generated/product_c10.c \
$(srcdir)/generated/product_c16.c

i_matmul_c= \
$(srcdir)/generated/matmul_i1.c \
$(srcdir)/generated/matmul_i2.c \
$(srcdir)/generated/matmul_i4.c \
$(srcdir)/generated/matmul_i8.c \
$(srcdir)/generated/matmul_i16.c \
$(srcdir)/generated/matmul_r4.c \
$(srcdir)/generated/matmul_r8.c \
$(srcdir)/generated/matmul_r10.c \
$(srcdir)/generated/matmul_r16.c \
$(srcdir)/generated/matmul_c4.c \
$(srcdir)/generated/matmul_c8.c \
$(srcdir)/generated/matmul_c10.c \
$(srcdir)/generated/matmul_c16.c

i_matmulavx128_c= \
$(srcdir)/generated/matmulavx128_i1.c \
$(srcdir)/generated/matmulavx128_i2.c \
$(srcdir)/generated/matmulavx128_i4.c \
$(srcdir)/generated/matmulavx128_i8.c \
$(srcdir)/generated/matmulavx128_i16.c \
$(srcdir)/generated/matmulavx128_r4.c \
$(srcdir)/generated/matmulavx128_r8.c \
$(srcdir)/generated/matmulavx128_r10.c \
$(srcdir)/generated/matmulavx128_r16.c \
$(srcdir)/generated/matmulavx128_c4.c \
$(srcdir)/generated/matmulavx128_c8.c \
$(srcdir)/generated/matmulavx128_c10.c \
$(srcdir)/generated/matmulavx128_c16.c

i_matmull_c= \
$(srcdir)/generated/matmul_l4.c \
$(srcdir)/generated/matmul_l8.c \
$(srcdir)/generated/matmul_l16.c

i_shape_c= \
$(srcdir)/generated/shape_i1.c \
$(srcdir)/generated/shape_i2.c \
$(srcdir)/generated/shape_i4.c \
$(srcdir)/generated/shape_i8.c \
$(srcdir)/generated/shape_i16.c

i_reshape_c= \
$(srcdir)/generated/reshape_i4.c \
$(srcdir)/generated/reshape_i8.c \
$(srcdir)/generated/reshape_i16.c \
$(srcdir)/generated/reshape_r4.c \
$(srcdir)/generated/reshape_r8.c \
$(srcdir)/generated/reshape_r10.c \
$(srcdir)/generated/reshape_r16.c \
$(srcdir)/generated/reshape_c4.c \
$(srcdir)/generated/reshape_c8.c \
$(srcdir)/generated/reshape_c10.c \
$(srcdir)/generated/reshape_c16.c

i_eoshift1_c= \
$(srcdir)/generated/eoshift1_4.c \
$(srcdir)/generated/eoshift1_8.c \
$(srcdir)/generated/eoshift1_16.c

i_eoshift3_c= \
$(srcdir)/generated/eoshift3_4.c \
$(srcdir)/generated/eoshift3_8.c \
$(srcdir)/generated/eoshift3_16.c

i_cshift0_c= \
$(srcdir)/generated/cshift0_i1.c \
$(srcdir)/generated/cshift0_i2.c \
$(srcdir)/generated/cshift0_i4.c \
$(srcdir)/generated/cshift0_i8.c \
$(srcdir)/generated/cshift0_i16.c \
$(srcdir)/generated/cshift0_r4.c \
$(srcdir)/generated/cshift0_r8.c \
$(srcdir)/generated/cshift0_r10.c \
$(srcdir)/generated/cshift0_r16.c \
$(srcdir)/generated/cshift0_c4.c \
$(srcdir)/generated/cshift0_c8.c \
$(srcdir)/generated/cshift0_c10.c \
$(srcdir)/generated/cshift0_c16.c

i_cshift1_c= \
$(srcdir)/generated/cshift1_4.c \
$(srcdir)/generated/cshift1_8.c \
$(srcdir)/generated/cshift1_16.c

i_cshift1a_c = \
$(srcdir)/generated/cshift1_4_i1.c \
$(srcdir)/generated/cshift1_4_i2.c \
$(srcdir)/generated/cshift1_4_i4.c \
$(srcdir)/generated/cshift1_4_i8.c \
$(srcdir)/generated/cshift1_4_i16.c \
$(srcdir)/generated/cshift1_4_r4.c \
$(srcdir)/generated/cshift1_4_r8.c \
$(srcdir)/generated/cshift1_4_r10.c \
$(srcdir)/generated/cshift1_4_r16.c \
$(srcdir)/generated/cshift1_4_c4.c \
$(srcdir)/generated/cshift1_4_c8.c \
$(srcdir)/generated/cshift1_4_c10.c \
$(srcdir)/generated/cshift1_4_c16.c \
$(srcdir)/generated/cshift1_8_i1.c \
$(srcdir)/generated/cshift1_8_i2.c \
$(srcdir)/generated/cshift1_8_i4.c \
$(srcdir)/generated/cshift1_8_i8.c \
$(srcdir)/generated/cshift1_8_i16.c \
$(srcdir)/generated/cshift1_8_r4.c \
$(srcdir)/generated/cshift1_8_r8.c \
$(srcdir)/generated/cshift1_8_r10.c \
$(srcdir)/generated/cshift1_8_r16.c \
$(srcdir)/generated/cshift1_8_c4.c \
$(srcdir)/generated/cshift1_8_c8.c \
$(srcdir)/generated/cshift1_8_c10.c \
$(srcdir)/generated/cshift1_8_c16.c \
$(srcdir)/generated/cshift1_16_i1.c \
$(srcdir)/generated/cshift1_16_i2.c \
$(srcdir)/generated/cshift1_16_i4.c \
$(srcdir)/generated/cshift1_16_i8.c \
$(srcdir)/generated/cshift1_16_i16.c \
$(srcdir)/generated/cshift1_16_r4.c \
$(srcdir)/generated/cshift1_16_r8.c \
$(srcdir)/generated/cshift1_16_r10.c \
$(srcdir)/generated/cshift1_16_r16.c \
$(srcdir)/generated/cshift1_16_c4.c \
$(srcdir)/generated/cshift1_16_c8.c \
$(srcdir)/generated/cshift1_16_c10.c \
$(srcdir)/generated/cshift1_16_c16.c

in_pack_c = \
$(srcdir)/generated/in_pack_i1.c \
$(srcdir)/generated/in_pack_i2.c \
$(srcdir)/generated/in_pack_i4.c \
$(srcdir)/generated/in_pack_i8.c \
$(srcdir)/generated/in_pack_i16.c \
$(srcdir)/generated/in_pack_r4.c \
$(srcdir)/generated/in_pack_r8.c \
$(srcdir)/generated/in_pack_r10.c \
$(srcdir)/generated/in_pack_r16.c \
$(srcdir)/generated/in_pack_c4.c \
$(srcdir)/generated/in_pack_c8.c \
$(srcdir)/generated/in_pack_c10.c \
$(srcdir)/generated/in_pack_c16.c

in_unpack_c = \
$(srcdir)/generated/in_unpack_i1.c \
$(srcdir)/generated/in_unpack_i2.c \
$(srcdir)/generated/in_unpack_i4.c \
$(srcdir)/generated/in_unpack_i8.c \
$(srcdir)/generated/in_unpack_i16.c \
$(srcdir)/generated/in_unpack_r4.c \
$(srcdir)/generated/in_unpack_r8.c \
$(srcdir)/generated/in_unpack_r10.c \
$(srcdir)/generated/in_unpack_r16.c \
$(srcdir)/generated/in_unpack_c4.c \
$(srcdir)/generated/in_unpack_c8.c \
$(srcdir)/generated/in_unpack_c10.c \
$(srcdir)/generated/in_unpack_c16.c

i_pow_c = \
$(srcdir)/generated/pow_i4_i4.c \
$(srcdir)/generated/pow_i8_i4.c \
$(srcdir)/generated/pow_i16_i4.c \
$(srcdir)/generated/pow_r16_i4.c \
$(srcdir)/generated/pow_c4_i4.c \
$(srcdir)/generated/pow_c8_i4.c \
$(srcdir)/generated/pow_c10_i4.c \
$(srcdir)/generated/pow_c16_i4.c \
$(srcdir)/generated/pow_i4_i8.c \
$(srcdir)/generated/pow_i8_i8.c \
$(srcdir)/generated/pow_i16_i8.c \
$(srcdir)/generated/pow_r4_i8.c \
$(srcdir)/generated/pow_r8_i8.c \
$(srcdir)/generated/pow_r10_i8.c \
$(srcdir)/generated/pow_r16_i8.c \
$(srcdir)/generated/pow_c4_i8.c \
$(srcdir)/generated/pow_c8_i8.c \
$(srcdir)/generated/pow_c10_i8.c \
$(srcdir)/generated/pow_c16_i8.c \
$(srcdir)/generated/pow_i4_i16.c \
$(srcdir)/generated/pow_i8_i16.c \
$(srcdir)/generated/pow_i16_i16.c \
$(srcdir)/generated/pow_r4_i16.c \
$(srcdir)/generated/pow_r8_i16.c \
$(srcdir)/generated/pow_r10_i16.c \
$(srcdir)/generated/pow_r16_i16.c \
$(srcdir)/generated/pow_c4_i16.c \
$(srcdir)/generated/pow_c8_i16.c \
$(srcdir)/generated/pow_c10_i16.c \
$(srcdir)/generated/pow_c16_i16.c

i_pack_c = \
$(srcdir)/generated/pack_i1.c \
$(srcdir)/generated/pack_i2.c \
$(srcdir)/generated/pack_i4.c \
$(srcdir)/generated/pack_i8.c \
$(srcdir)/generated/pack_i16.c \
$(srcdir)/generated/pack_r4.c \
$(srcdir)/generated/pack_r8.c \
$(srcdir)/generated/pack_r10.c \
$(srcdir)/generated/pack_r16.c \
$(srcdir)/generated/pack_c4.c \
$(srcdir)/generated/pack_c8.c \
$(srcdir)/generated/pack_c10.c \
$(srcdir)/generated/pack_c16.c

i_unpack_c = \
$(srcdir)/generated/unpack_i1.c \
$(srcdir)/generated/unpack_i2.c \
$(srcdir)/generated/unpack_i4.c \
$(srcdir)/generated/unpack_i8.c \
$(srcdir)/generated/unpack_i16.c \
$(srcdir)/generated/unpack_r4.c \
$(srcdir)/generated/unpack_r8.c \
$(srcdir)/generated/unpack_r10.c \
$(srcdir)/generated/unpack_r16.c \
$(srcdir)/generated/unpack_c4.c \
$(srcdir)/generated/unpack_c8.c \
$(srcdir)/generated/unpack_c10.c \
$(srcdir)/generated/unpack_c16.c

i_spread_c = \
$(srcdir)/generated/spread_i1.c \
$(srcdir)/generated/spread_i2.c \
$(srcdir)/generated/spread_i4.c \
$(srcdir)/generated/spread_i8.c \
$(srcdir)/generated/spread_i16.c \
$(srcdir)/generated/spread_r4.c \
$(srcdir)/generated/spread_r8.c \
$(srcdir)/generated/spread_r10.c \
$(srcdir)/generated/spread_r16.c \
$(srcdir)/generated/spread_c4.c \
$(srcdir)/generated/spread_c8.c \
$(srcdir)/generated/spread_c10.c \
$(srcdir)/generated/spread_c16.c 

m4_files= m4/iparm.m4 m4/ifunction.m4 m4/iforeach.m4 m4/all.m4 \
    m4/any.m4 m4/count.m4 m4/maxloc0.m4 m4/maxloc1.m4 m4/maxval.m4 \
    m4/minloc0.m4 m4/minloc1.m4 m4/minval.m4 m4/product.m4 m4/sum.m4 \
    m4/matmul.m4 m4/matmull.m4 m4/ifunction_logical.m4 \
    m4/ctrig.m4 m4/cexp.m4 m4/chyp.m4 m4/mtype.m4 \
    m4/specific.m4 m4/specific2.m4 m4/head.m4 m4/shape.m4 m4/reshape.m4 \
    m4/eoshift1.m4 m4/eoshift3.m4 \
    m4/pow.m4 \
    m4/misc_specifics.m4 m4/pack.m4 \
    m4/unpack.m4 m4/spread.m4 m4/bessel.m4 m4/norm2.m4 m4/parity.m4 \
    m4/iall.m4 m4/iany.m4 m4/iparity.m4 m4/iforeach-s.m4

gfor_built_src= $(i_all_c) $(i_any_c) $(i_count_c) $(i_maxloc0_c) \
    $(i_maxloc1_c) $(i_maxval_c) $(i_minloc0_c) $(i_minloc1_c) $(i_minval_c) \
    $(i_product_c) $(i_sum_c) $(i_bessel_c) $(i_iall_c) $(i_iany_c) \
    $(i_iparity_c) $(i_norm2_c) $(i_parity_c) \
    $(i_matmul_c) $(i_matmull_c) $(i_shape_c) $(i_eoshift1_c) \
    $(i_eoshift3_c) $(i_cshift1_c) $(i_reshape_c) $(in_pack_c) $(in_unpack_c) \
    $(i_pow_c) $(i_pack_c) $(i_unpack_c) $(i_matmulavx128_c) \
    $(i_spread_c) selected_int_kind.inc selected_real_kind.inc kinds.h \
    $(i_cshift0_c) kinds.inc c99_protos.inc fpu-target.h fpu-target.inc \
    $(i_cshift1a_c) $(i_maxloc0s_c) $(i_minloc0s_c) $(i_maxloc1s_c) \
    $(i_minloc1s_c) $(i_maxloc2s_c) $(i_minloc2s_c) $(i_maxvals_c) \
    $(i_maxval0s_c) $(i_minval0s_c) $(i_maxval1s_c) $(i_minval1s_c)

# Machine generated specifics
gfor_built_specific_src= \
$(srcdir)/generated/_abs_c4.F90 \
$(srcdir)/generated/_abs_c8.F90 \
$(srcdir)/generated/_abs_c10.F90 \
$(srcdir)/generated/_abs_c16.F90 \
$(srcdir)/generated/_abs_i4.F90 \
$(srcdir)/generated/_abs_i8.F90 \
$(srcdir)/generated/_abs_i16.F90 \
$(srcdir)/generated/_abs_r4.F90 \
$(srcdir)/generated/_abs_r8.F90 \
$(srcdir)/generated/_abs_r10.F90 \
$(srcdir)/generated/_abs_r16.F90 \
$(srcdir)/generated/_aimag_c4.F90 \
$(srcdir)/generated/_aimag_c8.F90 \
$(srcdir)/generated/_aimag_c10.F90 \
$(srcdir)/generated/_aimag_c16.F90 \
$(srcdir)/generated/_exp_r4.F90 \
$(srcdir)/generated/_exp_r8.F90 \
$(srcdir)/generated/_exp_r10.F90 \
$(srcdir)/generated/_exp_r16.F90 \
$(srcdir)/generated/_exp_c4.F90 \
$(srcdir)/generated/_exp_c8.F90 \
$(srcdir)/generated/_exp_c10.F90 \
$(srcdir)/generated/_exp_c16.F90 \
$(srcdir)/generated/_log_r4.F90 \
$(srcdir)/generated/_log_r8.F90 \
$(srcdir)/generated/_log_r10.F90 \
$(srcdir)/generated/_log_r16.F90 \
$(srcdir)/generated/_log_c4.F90 \
$(srcdir)/generated/_log_c8.F90 \
$(srcdir)/generated/_log_c10.F90 \
$(srcdir)/generated/_log_c16.F90 \
$(srcdir)/generated/_log10_r4.F90 \
$(srcdir)/generated/_log10_r8.F90 \
$(srcdir)/generated/_log10_r10.F90 \
$(srcdir)/generated/_log10_r16.F90 \
$(srcdir)/generated/_sqrt_r4.F90 \
$(srcdir)/generated/_sqrt_r8.F90 \
$(srcdir)/generated/_sqrt_r10.F90 \
$(srcdir)/generated/_sqrt_r16.F90 \
$(srcdir)/generated/_sqrt_c4.F90 \
$(srcdir)/generated/_sqrt_c8.F90 \
$(srcdir)/generated/_sqrt_c10.F90 \
$(srcdir)/generated/_sqrt_c16.F90 \
$(srcdir)/generated/_asin_r4.F90 \
$(srcdir)/generated/_asin_r8.F90 \
$(srcdir)/generated/_asin_r10.F90 \
$(srcdir)/generated/_asin_r16.F90 \
$(srcdir)/generated/_asinh_r4.F90 \
$(srcdir)/generated/_asinh_r8.F90 \
$(srcdir)/generated/_asinh_r10.F90 \
$(srcdir)/generated/_asinh_r16.F90 \
$(srcdir)/generated/_acos_r4.F90 \
$(srcdir)/generated/_acos_r8.F90 \
$(srcdir)/generated/_acos_r10.F90 \
$(srcdir)/generated/_acos_r16.F90 \
$(srcdir)/generated/_acosh_r4.F90 \
$(srcdir)/generated/_acosh_r8.F90 \
$(srcdir)/generated/_acosh_r10.F90 \
$(srcdir)/generated/_acosh_r16.F90 \
$(srcdir)/generated/_atan_r4.F90 \
$(srcdir)/generated/_atan_r8.F90 \
$(srcdir)/generated/_atan_r10.F90 \
$(srcdir)/generated/_atan_r16.F90 \
$(srcdir)/generated/_atanh_r4.F90 \
$(srcdir)/generated/_atanh_r8.F90 \
$(srcdir)/generated/_atanh_r10.F90 \
$(srcdir)/generated/_atanh_r16.F90 \
$(srcdir)/generated/_sin_r4.F90 \
$(srcdir)/generated/_sin_r8.F90 \
$(srcdir)/generated/_sin_r10.F90 \
$(srcdir)/generated/_sin_r16.F90 \
$(srcdir)/generated/_sin_c4.F90 \
$(srcdir)/generated/_sin_c8.F90 \
$(srcdir)/generated/_sin_c10.F90 \
$(srcdir)/generated/_sin_c16.F90 \
$(srcdir)/generated/_cos_r4.F90 \
$(srcdir)/generated/_cos_r8.F90 \
$(srcdir)/generated/_cos_r10.F90 \
$(srcdir)/generated/_cos_r16.F90 \
$(srcdir)/generated/_cos_c4.F90 \
$(srcdir)/generated/_cos_c8.F90 \
$(srcdir)/generated/_cos_c10.F90 \
$(srcdir)/generated/_cos_c16.F90 \
$(srcdir)/generated/_tan_r4.F90 \
$(srcdir)/generated/_tan_r8.F90 \
$(srcdir)/generated/_tan_r10.F90 \
$(srcdir)/generated/_tan_r16.F90 \
$(srcdir)/generated/_sinh_r4.F90 \
$(srcdir)/generated/_sinh_r8.F90 \
$(srcdir)/generated/_sinh_r10.F90 \
$(srcdir)/generated/_sinh_r16.F90 \
$(srcdir)/generated/_cosh_r4.F90 \
$(srcdir)/generated/_cosh_r8.F90 \
$(srcdir)/generated/_cosh_r10.F90 \
$(srcdir)/generated/_cosh_r16.F90 \
$(srcdir)/generated/_tanh_r4.F90 \
$(srcdir)/generated/_tanh_r8.F90 \
$(srcdir)/generated/_tanh_r10.F90 \
$(srcdir)/generated/_tanh_r16.F90 \
$(srcdir)/generated/_conjg_c4.F90 \
$(srcdir)/generated/_conjg_c8.F90 \
$(srcdir)/generated/_conjg_c10.F90 \
$(srcdir)/generated/_conjg_c16.F90 \
$(srcdir)/generated/_aint_r4.F90 \
$(srcdir)/generated/_aint_r8.F90 \
$(srcdir)/generated/_aint_r10.F90 \
$(srcdir)/generated/_aint_r16.F90 \
$(srcdir)/generated/_anint_r4.F90 \
$(srcdir)/generated/_anint_r8.F90 \
$(srcdir)/generated/_anint_r10.F90 \
$(srcdir)/generated/_anint_r16.F90

gfor_built_specific2_src= \
$(srcdir)/generated/_sign_i4.F90 \
$(srcdir)/generated/_sign_i8.F90 \
$(srcdir)/generated/_sign_i16.F90 \
$(srcdir)/generated/_sign_r4.F90 \
$(srcdir)/generated/_sign_r8.F90 \
$(srcdir)/generated/_sign_r10.F90 \
$(srcdir)/generated/_sign_r16.F90 \
$(srcdir)/generated/_dim_i4.F90 \
$(srcdir)/generated/_dim_i8.F90 \
$(srcdir)/generated/_dim_i16.F90 \
$(srcdir)/generated/_dim_r4.F90 \
$(srcdir)/generated/_dim_r8.F90 \
$(srcdir)/generated/_dim_r10.F90 \
$(srcdir)/generated/_dim_r16.F90 \
$(srcdir)/generated/_atan2_r4.F90 \
$(srcdir)/generated/_atan2_r8.F90 \
$(srcdir)/generated/_atan2_r10.F90 \
$(srcdir)/generated/_atan2_r16.F90 \
$(srcdir)/generated/_mod_i4.F90 \
$(srcdir)/generated/_mod_i8.F90 \
$(srcdir)/generated/_mod_i16.F90 \
$(srcdir)/generated/_mod_r4.F90 \
$(srcdir)/generated/_mod_r8.F90 \
$(srcdir)/generated/_mod_r10.F90 \
$(srcdir)/generated/_mod_r16.F90

gfor_misc_specifics = $(srcdir)/generated/misc_specifics.F90

gfor_specific_src= \
$(gfor_built_specific_src) \
$(gfor_built_specific2_src) \
$(gfor_misc_specifics) \
intrinsics/dprod_r8.f90 \
intrinsics/f2c_specifics.F90 \
intrinsics/random_init.f90

# Turn on vectorization and loop unrolling for matmul.
$(patsubst %.c,%.lo,$(notdir $(i_matmul_c))): AM_CFLAGS += -ffast-math -ftree-vectorize -funroll-loops --param max-unroll-times=4

if HAVE_AVX128
# Turn on AVX128 for AMD-specific matmul, but only if the compiler understands -mprefer-avx128
$(patsubst %.c,%.lo,$(notdir $(i_matmulavx128_c))): AM_CFLAGS += -ffast-math -ftree-vectorize -funroll-loops --param max-unroll-times=4 -mprefer-avx128
endif
# Logical matmul doesn't vectorize.
$(patsubst %.c,%.lo,$(notdir $(i_matmull_c))): AM_CFLAGS += -funroll-loops

# Add the -fallow-leading-underscore option when needed
$(patsubst %.F90,%.lo,$(patsubst %.f90,%.lo,$(notdir $(gfor_specific_src)))): AM_FCFLAGS += -fallow-leading-underscore
selected_real_kind.lo selected_int_kind.lo: AM_FCFLAGS += -fallow-leading-underscore

if IEEE_SUPPORT
# Add flags for IEEE modules
$(patsubst %.F90,%.lo,$(notdir $(gfor_ieee_src))): AM_FCFLAGS += -Wno-unused-dummy-argument -Wno-c-binding-type -ffree-line-length-0 -fallow-leading-underscore
endif

# Dependencies between IEEE_ARITHMETIC and IEEE_EXCEPTIONS
ieee_arithmetic.lo: ieee/ieee_arithmetic.F90 ieee_exceptions.lo
	$(LTPPFCCOMPILE) -c -o $@ $<

ieee_features.mod: ieee_features.lo
	:
ieee_exceptions.mod: ieee_exceptions.lo
	:
ieee_arithmetic.mod: ieee_arithmetic.lo
	:

BUILT_SOURCES=$(gfor_built_src) $(gfor_built_specific_src) \
    $(gfor_built_specific2_src) $(gfor_misc_specifics)

prereq_SRC = $(gfor_src) $(gfor_built_src) $(gfor_io_src) \
    $(gfor_helper_src) $(gfor_ieee_src) $(gfor_io_headers) $(gfor_specific_src)

if onestep
# dummy sources for libtool
BUILT_SOURCES+=libgfortran_c.c libgfortran_f.f90
libgfortran_c.c libgfortran_f.f90 libgfortran_F.F90:
	echo > $@
# overrides for libtool perusing the dummy sources
libgfortran_c.o: $(filter %.c,$(prereq_SRC))
	$(COMPILE) -c $^ -o $@ -combine

libgfortran_c.lo: $(filter %.c,$(prereq_SRC))
	$(LTCOMPILE) -c -o $@ $^ -combine

#libgfortran_f.o: $(filter %.f %.f90,$(prereq_SRC))
#	$(FCCOMPILE) -c $^ -o $@ -combine

#libgfortran_f.lo: $(filter %.f %.f90,$(prereq_SRC))
#	$(LTFCCOMPILE) -c -o $@ $^ -combine
# not currently used:
#libgfortran_F.o: $(filter %.F %.F90,$(prereq_SRC))
#	$(PPFCCOMPILE) -c $^ -o $@ -combine
#
#libgfortran_F.lo: 
#	$(LTPPFCCOMPILE) -c -o $@ $^ -combine

libgfortran_la_SOURCES = libgfortran_c.c $(filter-out %.c,$(prereq_SRC))

else
libgfortran_la_SOURCES = $(prereq_SRC)

endif

I_M4_DEPS=m4/iparm.m4
I_M4_DEPS0=$(I_M4_DEPS) m4/iforeach.m4
I_M4_DEPS1=$(I_M4_DEPS) m4/ifunction.m4
I_M4_DEPS2=$(I_M4_DEPS) m4/ifunction_logical.m4
I_M4_DEPS3=$(I_M4_DEPS) m4/iforeach-s.m4
I_M4_DEPS4=$(I_M4_DEPS) m4/ifunction-s.m4
I_M4_DEPS5=$(I_M4_DEPS) m4/iforeach-s2.m4
I_M4_DEPS6=$(I_M4_DEPS) m4/ifunction-s2.m4

kinds.h: $(srcdir)/mk-kinds-h.sh
	$(SHELL) $(srcdir)/mk-kinds-h.sh '$(FCCOMPILE)' > $@ || rm $@

kinds.inc: kinds.h
	grep '^#' < kinds.h > $@

c99_protos.inc: $(srcdir)/c99_protos.h
	grep '^#' < $(srcdir)/c99_protos.h > $@

selected_int_kind.inc: $(srcdir)/mk-sik-inc.sh
	$(SHELL) $(srcdir)/mk-sik-inc.sh '$(FCCOMPILE)' > $@ || rm $@

selected_real_kind.inc: $(srcdir)/mk-srk-inc.sh
	$(SHELL) $(srcdir)/mk-srk-inc.sh '$(FCCOMPILE)' > $@ || rm $@

fpu-target.h: $(srcdir)/$(FPU_HOST_HEADER)
	cp $(srcdir)/$(FPU_HOST_HEADER) $@

fpu-target.inc: fpu-target.h $(srcdir)/libgfortran.h
	grep '^#define GFC_FPE_' < $(top_srcdir)/../gcc/fortran/libgfortran.h > $@ || true
	grep '^#define GFC_FPE_' < $(srcdir)/libgfortran.h >> $@ || true

## A 'normal' build shouldn't need to regenerate these
## so we only include them in maintainer mode

if MAINTAINER_MODE
$(i_all_c): m4/all.m4 $(I_M4_DEPS2)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 all.m4 > $@

$(i_bessel_c): m4/bessel.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 bessel.m4 > $@

$(i_any_c): m4/any.m4 $(I_M4_DEPS2)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 any.m4 > $@

$(i_count_c): m4/count.m4 $(I_M4_DEPS2)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 count.m4 > $@

$(i_iall_c): m4/iall.m4 $(I_M4_DEPS1)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 iall.m4 > $@

$(i_iany_c): m4/iany.m4 $(I_M4_DEPS1)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 iany.m4 > $@

$(i_iparity_c): m4/iparity.m4 $(I_M4_DEPS1)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 iparity.m4 > $@

$(i_maxloc0_c): m4/maxloc0.m4 $(I_M4_DEPS0)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 maxloc0.m4 > $@

$(i_maxloc0s_c) : m4/maxloc0s.m4 $(I_M4_DEPS3)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 maxloc0s.m4 > $@

$(i_maxloc1_c): m4/maxloc1.m4 $(I_M4_DEPS1)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 maxloc1.m4 > $@

$(i_maxloc1s_c): m4/maxloc1s.m4 $(I_M4_DEPS4)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 maxloc1s.m4 > $@

$(i_maxloc2s_c): m4/maxloc2s.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 maxloc2s.m4 > $@

$(i_maxval_c): m4/maxval.m4 $(I_M4_DEPS1)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 maxval.m4 > $@

$(i_maxval0s_c): m4/maxval0s.m4 $(I_M4_DEPS5)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 maxval0s.m4 > $@

$(i_maxval1s_c): m4/maxval1s.m4 $(I_M4_DEPS6)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 maxval1s.m4 > $@

$(i_minloc0_c): m4/minloc0.m4 $(I_M4_DEPS0)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 minloc0.m4 > $@

$(i_minloc0s_c) : m4/minloc0s.m4 $(I_M4_DEPS3)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 minloc0s.m4 > $@

$(i_minloc1_c): m4/minloc1.m4 $(I_M4_DEPS1)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 minloc1.m4 > $@

$(i_minloc1s_c): m4/minloc1s.m4 $(I_M4_DEPS4)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 minloc1s.m4 > $@

$(i_minloc2s_c): m4/minloc2s.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 minloc2s.m4 > $@

$(i_minval_c): m4/minval.m4 $(I_M4_DEPS1)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 minval.m4 > $@

$(i_minval0s_c): m4/minval0s.m4 $(I_M4_DEPS5)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 minval0s.m4 > $@

$(i_minval1s_c): m4/minval1s.m4 $(I_M4_DEPS6)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 minval1s.m4 > $@

$(i_product_c): m4/product.m4 $(I_M4_DEPS1)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 product.m4 > $@

$(i_sum_c): m4/sum.m4 $(I_M4_DEPS1)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 sum.m4 > $@

$(i_matmul_c): m4/matmul.m4 m4/matmul_internal.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 matmul.m4 > $@

$(i_matmulavx128_c): m4/matmulavx128.m4 m4/matmul_internal.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 matmulavx128.m4 > $@

$(i_matmull_c): m4/matmull.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 matmull.m4 > $@

$(i_norm2_c): m4/norm2.m4 $(I_M4_DEPS1)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 norm2.m4 > $@

$(i_parity_c): m4/parity.m4 $(I_M4_DEPS1)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 parity.m4 > $@

$(i_shape_c): m4/shape.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 shape.m4 > $@

$(i_reshape_c): m4/reshape.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 reshape.m4 > $@

$(i_eoshift1_c): m4/eoshift1.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 eoshift1.m4 > $@

$(i_eoshift3_c): m4/eoshift3.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 eoshift3.m4 > $@

$(i_cshift0_c): m4/cshift0.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 cshift0.m4 > $@

$(i_cshift1_c): m4/cshift1.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 cshift1.m4 > $@

$(i_cshift1a_c): m4/cshift1a.m4 $(I_M$_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 cshift1a.m4 > $@

$(in_pack_c): m4/in_pack.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 in_pack.m4 > $@

$(in_unpack_c): m4/in_unpack.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 in_unpack.m4 > $@

$(i_pow_c): m4/pow.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 pow.m4 > $@

$(i_pack_c): m4/pack.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 pack.m4 > $@

$(i_unpack_c): m4/unpack.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 unpack.m4 > $@

$(i_spread_c): m4/spread.m4 $(I_M4_DEPS)
	$(M4) -Dfile=$@ -I$(srcdir)/m4 spread.m4 > $@

$(gfor_built_specific_src): m4/specific.m4 m4/head.m4
	$(M4) -Dfile=$@ -I$(srcdir)/m4 specific.m4 > $@

$(gfor_built_specific2_src): m4/specific2.m4 m4/head.m4
	$(M4) -Dfile=$@ -I$(srcdir)/m4 specific2.m4 > $@

$(gfor_misc_specifics): m4/misc_specifics.m4 m4/head.m4
	$(M4) -Dfile=$@ -I$(srcdir)/m4 misc_specifics.m4 > $@
## end of maintainer mode only rules
endif

EXTRA_DIST = $(m4_files)
