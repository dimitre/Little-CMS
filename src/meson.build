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

# Determine which library function to use based on default_library option
# This fixes cross-compilation naming issues with MSVC where library()
# incorrectly generates .a files instead of .lib for static libraries
if get_option('default_library') == 'static'
  lib_func = static_library
elif get_option('default_library') == 'shared'
  lib_func = shared_library
else
  lib_func = both_libraries
endif

if get_option('versionedlibs')
  liblcms2_lib = lib_func(
    'lcms2',
    lcms2_srcs,
    include_directories: inc_dirs,
    gnu_symbol_visibility: 'hidden',
    dependencies: deps,
    c_args: cargs,
    version: library_version,
    # vs_module_defs: 'lcms2.def',
    install: true,
  )
else
  liblcms2_lib = lib_func(
    'lcms2',
    lcms2_srcs,
    include_directories: inc_dirs,
    gnu_symbol_visibility: 'hidden',
    dependencies: deps,
    c_args: cargs,
    install: true,
  )
endif

# Handle dependency declaration for both_libraries case
if get_option('default_library') == 'both'
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
