from hashlib import sha256;
from os.path import basename

bson_config_macos = '''
#ifndef BSON_CONFIG_H
#define BSON_CONFIG_H

/*
 * Define to 1234 for Little Endian, 4321 for Big Endian.
 */
#define BSON_BYTE_ORDER 1234


/*
 * Define to 1 if you have stdbool.h
 */
#define BSON_HAVE_STDBOOL_H 1
#if BSON_HAVE_STDBOOL_H != 1
# undef BSON_HAVE_STDBOOL_H
#endif


/*
 * Define to 1 for POSIX-like systems, 2 for Windows.
 */
#define BSON_OS 1


/*
 * Define to 1 if we have access to GCC 32-bit atomic builtins.
 * While this requires GCC 4.1+ in most cases, it is also architecture
 * dependent. For example, some PPC or ARM systems may not have it even
 * if it is a recent GCC version.
 */
#define BSON_HAVE_ATOMIC_32_ADD_AND_FETCH 1
#if BSON_HAVE_ATOMIC_32_ADD_AND_FETCH != 1
# undef BSON_HAVE_ATOMIC_32_ADD_AND_FETCH
#endif

/*
 * Similarly, define to 1 if we have access to GCC 64-bit atomic builtins.
 */
#define BSON_HAVE_ATOMIC_64_ADD_AND_FETCH 1
#if BSON_HAVE_ATOMIC_64_ADD_AND_FETCH != 1
# undef BSON_HAVE_ATOMIC_64_ADD_AND_FETCH
#endif


/*
 * Define to 1 if your system requires {} around PTHREAD_ONCE_INIT.
 * This is typically just Solaris 8-10.
 */
#define BSON_PTHREAD_ONCE_INIT_NEEDS_BRACES 0
#if BSON_PTHREAD_ONCE_INIT_NEEDS_BRACES != 1
# undef BSON_PTHREAD_ONCE_INIT_NEEDS_BRACES
#endif


/*
 * Define to 1 if you have clock_gettime() available.
 */
#define BSON_HAVE_CLOCK_GETTIME 1
#if BSON_HAVE_CLOCK_GETTIME != 1
# undef BSON_HAVE_CLOCK_GETTIME
#endif


/*
 * Define to 1 if you have strnlen available on your platform.
 */
#define BSON_HAVE_STRNLEN 1
#if BSON_HAVE_STRNLEN != 1
# undef BSON_HAVE_STRNLEN
#endif


/*
 * Define to 1 if you have snprintf available on your platform.
 */
#define BSON_HAVE_SNPRINTF 1
#if BSON_HAVE_SNPRINTF != 1
# undef BSON_HAVE_SNPRINTF
#endif


/*
 * Define to 1 if you have gmtime_r available on your platform.
 */
#define BSON_HAVE_GMTIME_R 1
#if BSON_HAVE_GMTIME_R != 1
# undef BSON_HAVE_GMTIME_R
#endif


/*
 * Define to 1 if you have reallocf available on your platform.
 */
#define BSON_HAVE_REALLOCF 1
#if BSON_HAVE_REALLOCF != 1
# undef BSON_HAVE_REALLOCF
#endif


/*
 * Define to 1 if you have struct timespec available on your platform.
 */
#define BSON_HAVE_TIMESPEC 1
#if BSON_HAVE_TIMESPEC != 1
# undef BSON_HAVE_TIMESPEC
#endif


/*
 * Define to 1 if you want extra aligned types in libbson
 */
#define BSON_EXTRA_ALIGN 1
#if BSON_EXTRA_ALIGN != 1
# undef BSON_EXTRA_ALIGN
#endif


/*
 * Define to 1 if you have SYS_gettid syscall
 */
#define BSON_HAVE_SYSCALL_TID 0
#if BSON_HAVE_SYSCALL_TID != 1
# undef BSON_HAVE_SYSCALL_TID
#endif


#endif /* BSON_CONFIG_H */
'''

bson_stdint_macos = '''
#ifndef _SRC_BSON_BSON_STDINT_H
#define _SRC_BSON_BSON_STDINT_H 1
#ifndef _GENERATED_STDINT_H
#define _GENERATED_STDINT_H \\" \\"
/* generated using a gnu compiler version Apple LLVM version 8.1.0 (clang-802.0.42) Target: x86_64-apple-darwin16.7.0 Thread model: posix InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin */

#include <stdint.h>


/* system headers have good uint64_t */
#ifndef _HAVE_UINT64_T
#define _HAVE_UINT64_T
#endif

  /* once */
#endif
#endif
'''

bson_version = '''
#if !defined (BSON_INSIDE) && !defined (BSON_COMPILATION)
#error \\"Only <bson.h> can be included directly.\\"
#endif

#ifndef BSON_VERSION_H
#define BSON_VERSION_H

#define BSON_MAJOR_VERSION (1)
#define BSON_MINOR_VERSION (7)
#define BSON_MICRO_VERSION (0)

#define BSON_PRERELEASE_VERSION ()

#define BSON_VERSION (1.7.0)

#define BSON_VERSION_S \\"1.7.0\\"

#define BSON_VERSION_HEX (BSON_MAJOR_VERSION << 24 | \\
                          BSON_MINOR_VERSION << 16 | \\
                          BSON_MICRO_VERSION << 8)


/**
 * BSON_CHECK_VERSION:
 * @major: required major version
 * @minor: required minor version
 * @micro: required micro version
 *
 * Compile-time version checking. Evaluates to \%TRUE if the version
 * of BSON is greater than the required one.
 */
#define BSON_CHECK_VERSION(major,minor,micro)   \\
        (BSON_MAJOR_VERSION > (major) || \\
         (BSON_MAJOR_VERSION == (major) && BSON_MINOR_VERSION > (minor)) || \\
         (BSON_MAJOR_VERSION == (major) && BSON_MINOR_VERSION == (minor) && \\
          BSON_MICRO_VERSION >= (micro)))

#endif /* BSON_VERSION_H */
'''

def generate_file(name, content):
  target = basename(name) + '-' + sha256('generate_file' + name + content).hexdigest()[0:12]
  genrule(
    name = target,
    out = name,
    cmd = 'echo "' + content + '" > $OUT',
  )
  return ':' + target

bson_version_target = generate_file('bson-version.h', bson_version)

macos_exported_headers = {
  'bson-config.h': generate_file('bson-config.h', bson_config_macos),
  'bson-stdint.h': generate_file('bson-stdint.h', bson_stdint_macos),
  'bson-version.h': bson_version_target,
}

prebuilt_cxx_library(
  name = 'config',
  header_only = True,
  header_namespace = '',
  exported_platform_headers = [
    ('default', macos_exported_headers),
    ('^macos.*', macos_exported_headers),
  ],
)

cxx_library(
  name = 'bson',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('src/bson', '**/*.h'),
    ('src', 'jsonsl/**/*.h'),
  ]),
  srcs = glob([
    'src/**/*.c',
  ]),
  preprocessor_flags = [
    '-DBSON_COMPILATION=1',
  ],
  deps = [
    ':config',
  ],
  visibility = [
    'PUBLIC',
  ],
)
