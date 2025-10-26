lcms2_srcs = files(
  'cmsalpha.c',
  'cmscam02.c',
  'cmscgats.c',
  'cmscnvrt.c',
  'cmserr.c',
  'cmsgamma.c',
  'cmsgmt.c',
  'cmshalf.c',
  'cmsintrp.c',
  'cmsio0.c',
  'cmsio1.c',
  'cmslut.c',
  'cmsmd5.c',
  'cmsmtrx.c',
  'cmsnamed.c',
  'cmsopt.c',
  'cmspack.c',
  'cmspcs.c',
  'cmsplugin.c',
  'cmsps2.c',
  'cmssamp.c',
  'cmssm.c',
  'cmstypes.c',
  'cmsvirt.c',
  'cmswtpnt.c',
  'cmsxform.c',
)

if host_machine.system() == 'windows'
    if get_option('default_library') == 'shared'

    lcms2_rc = configure_file(
        input: 'lcms2.rc.in',
        configuration: version_cfg,
        output: 'lcms2.rc',
        )

        lcms2_srcs += win.compile_resources(lcms2_rc)
        cargs += '-DCMS_DLL_BUILD=1'
    endif
endif

# Build library arguments - common to all library types
lib_common_args = {
  'include_directories': inc_dirs,
  'gnu_symbol_visibility': 'hidden',
  'dependencies': deps,
  'c_args': cargs,
  'install': true,
}

# Determine which library type to build based on default_library option
# This fixes cross-compilation naming issues with MSVC where library()
# incorrectly generates .a files instead of .lib for static libraries
default_lib_type = get_option('default_library')

if default_lib_type == 'static'
  # Static libraries don't use version numbers
  liblcms2_lib = static_library('lcms2', lcms2_srcs, kwargs: lib_common_args)
elif default_lib_type == 'shared'
  # Add version for shared library if requested
  if get_option('versionedlibs')
    lib_common_args += {'version': library_version}
  endif
  liblcms2_lib = shared_library('lcms2', lcms2_srcs, kwargs: lib_common_args)
else
  # 'both' option - add version if requested
  if get_option('versionedlibs')
    lib_common_args += {'version': library_version}
  endif
  liblcms2_lib = both_libraries('lcms2', lcms2_srcs, kwargs: lib_common_args)
endif

# Handle dependency declaration for both_libraries case
if default_lib_type == 'both'
  liblcms2_dep = declare_dependency(
    link_with: liblcms2_lib.get_static_lib(),
    include_directories: [inc_dirs, include_directories('.')],
  )
else
  liblcms2_dep = declare_dependency(
    link_with: liblcms2_lib,
    include_directories: [inc_dirs, include_directories('.')],
  )
endif
