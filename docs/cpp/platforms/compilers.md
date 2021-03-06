---
title: Compiler Macros Guide
layout: docs
sidenav: side-nav-cpp.html
type: markdown
---

In low-level libraries like Abseil, macros are used to conditionally compile code for various purposes
like portability, performance, functionality, etc. Here are the categories of macros that are used to
write those preprocessor conditionals:

 * Pre-defined compiler macros
 * Build configuration macros
 * Feature check macros

## Pre-defined Compiler Macros

C and C++ compilers automatically define some macros that can be used to:

 * Identify platforms (architecture, endianness, OS, compiler, C++ standard, library)
 * Detect compiler configurations (debug mode, exception handling, etc.)

## Canonical Macros

### Architectures

References:

* [Pre-defined Compiler Macros](https://sourceforge.net/p/predef/wiki/Architectures/) 
* [How to detect the processor type using compiler predefined macros](http://nadeausoftware.com/articles/2012/02/c_c_tip_how_detect_processor_type_using_compiler_predefined_macros)

|Foo|Bar|Baz|
|AMD64|`__x86_64__`|Defined by GNU C and Sun Studio|
||`_M_X64`|Defined by Visual Studio|
|ARM|`__arm__`|Defined by GNU C|
||`_M_ARM`|Defined by Visual Studio|
|ARM64|`__aarch64__`|Defined by GNU C|
|`__i386__`|Defined by GNU C|
||`_M_IX86`|Only defined for 32-bits architectures|


Defined by Visual Studio, Intel C/C++, Digital Mars, and Watcom C/C++
   </td>
  </tr>
  <tr>
   <td>Intel Itanium (IA-64)
   </td>
   <td>`__ia64__`
   </td>
   <td>Defined by GNU C
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>`_M_IA64`
   </td>
   <td>Defined by Visual Studio
   </td>
  </tr>
  <tr>
   <td>PowerPC
   </td>
   <td>`__ppc__ || __PPC__ || __ppc64__ || __PPC64__`
   </td>
   <td>Defined by GNU C
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>`_M_PPC`
   </td>
   <td>Defined by Visual Studio
   </td>
  </tr>
  <tr>
   <td>Myriad2
   </td>
   <td>`__myriad2__`
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>MIPS
   </td>
   <td>`__mips__`
   </td>
   <td>Defined by GNUC
   </td>
  </tr>
</table>



### Endianness

References:

* https://sourceforge.net/p/predef/wiki/Endianness/

There is no standard portable pre-defined macros that can be used to determine endianness.

Instead, Abseil defines `ABSL_IS_LITTLE_ENDIAN` and `ABSL_IS_BIG_ENDIAN` in `"third_party/absl/base/port.h"`.

Also, as suggested by this
[paper](https://www.google.com/url?q=http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0463r0.html&sa=D&ust=1492041914805000&usg=AFQjCNG6NAKbx2jfEp_QQwpdedq_D2Jk9g),
we should also add a `enum class endian` to `"third_party/absl/meta/type_traits.h"`.

### OS

References:

* https://sourceforge.net/p/predef/wiki/OperatingSystems/


<table>
  <tr>
   <td>Android
   </td>
   <td>`__ANDROID__`
   </td>
  </tr>
  <tr>
   <td>MacOS, iOS
   </td>
   <td>`__APPLE__ && __MACH__`
   </td>
  </tr>
  <tr>
   <td>Windows
   </td>
   <td>`_WIN32`
   </td>
  </tr>
  <tr>
   <td>Linux
   </td>
   <td>`__linux__`
   </td>
  </tr>
  <tr>
   <td>Akaros
   </td>
   <td>`__ros__`
   </td>
  </tr>
  <tr>
   <td>Fuchsia
   </td>
   <td>`__Fuchsia__`
   </td>
  </tr>
</table>

### Compilers

References:

* https://sourceforge.net/p/predef/wiki/Compilers/

<table>
  <tr>
   <td>
   </td>
   <td>Identification
   </td>
   <td>Version
   </td>
   <td>Note
   </td>
  </tr>
  <tr>
   <td>GCC
   </td>
   <td>`__GNUC__`
   </td>
   <td>`__GNUC__`, `__GNUC_MINOR__`, `__GNUC_PATCHLEVEL__`
   </td>
   <td>`__GNUC__` is also defined by Clang. If you want to check "GCC only", write `defined(__GNUC__) && !defined(__clang__)`
   </td>
  </tr>
  <tr>
   <td>Clang
   </td>
   <td>`__clang__`
   </td>
   <td>`__clang_major__`, `__clang_minor__`, `__clang_patchlevel__`
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>MSVC
   </td>
   <td>`_MSC_VER`
   </td>
   <td>`_MSC_VER`, `_MSC_FULL_VER`
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td><a href="http://kripken.github.io/emscripten-site/">Emscripten</a>
   </td>
   <td>`__EMSCRIPTEN__`
   </td>
   <td>`__EMSCRIPTEN_major__`, `__EMSCRIPTEN_minor__`, `__EMSCRIPTEN_tiny__`
   </td>
   <td><a href="http://kripken.github.io/emscripten-site/docs/compiling/Building-Projects.html#building-projects">http://kripken.github.io/emscripten-site/docs/compiling/Building-Projects.html#building-projects</a>
   </td>
  </tr>
  <tr>
   <td><a href="http://asmjs.org/">asm.js</a>
   </td>
   <td>`__asmjs__`
   </td>
   <td>
   </td>
   <td>When targeting asm.js, the preprocessor defines `__asmjs` and `__asmjs__` are present.
   </td>
  </tr>
  <tr>
   <td>NVCC
   </td>
   <td>`__NVCC__`
   </td>
   <td>
   </td>
   <td>http://docs.nvidia.com/cuda/cuda-compiler-driver-nvcc/#axzz4hSssNIE2
   </td>
  </tr>
</table>

### C++ Standard

Usually you want to check certain C++ language features:

* Prefer to use C++ feature testing macros if supported by the compiler:
  http://en.cppreference.com/w/cpp/experimental/feature_test.
	* GCC 5.0+ https://gcc.gnu.org/gcc-5/changes.html
	* Clang
    * Not supported by MSVC
* Otherwise check compiler version for the feature support: http://en.cppreference.com/w/cpp/compiler_support


### Libraries

Library headers need to be included before checking the macros. For example, `base/config.h` includes
`<cstddef>` for `__GLIBCXX__`, `_LIBCPP_VERSION`.

<table>
  <tr>
   <td>libstdc++
   </td>
   <td>`__GLIBCXX__`
   </td>
  </tr>
  <tr>
   <td>libc++
   </td>
   <td>`_LIBCPP_VERSION`
   </td>
  </tr>
  <tr>
   <td>MSVC
   </td>
   <td>`_MSC_VER`
   </td>
  </tr>
  <tr>
   <td>GNU glibc
   </td>
   <td>`__GLIBC__`
   </td>
  </tr>
  <tr>
   <td>Bionic libc
   </td>
   <td>`__BIONIC__`
   </td>
  </tr>
</table>


## Build Configuration Macros

**NOTE: This list is not complete yet.**

Abseil expose a few macros for configuration of some modules.

<table>
<tr>
   <td>


Macro

   </td>
   <td>File

   </td>
   <td>Purpose

   </td>
   </tr>
   <tr>
   <td>





`ABSL_STRIP_LOG`


   </td>
   <td>


`base/logging.h`(logging)


   </td>
   <td>Log messages will be compiled away when `ABSL_STRIP_LOG` is non-zero, for

security reasons or to reduce binary size.

   </td>
   </tr>
   <tr>
   <td>


`ABSL_MIN_LOG_LEVEL`


   </td>
   <td>

`base/log_severity.h`


   </td>
   <td>
   </td>
   </tr>
   <tr>
   <td>

`SHRINK_WRAP_FRAME_POINTER`


   </td>
   <td>

`base/stacktrace_x86-inl.inc`


   </td>
   <td>http://g3doc/devtools/crosstool/g3doc/shrinkwrapping.md

This macros should be defined when compiled with `-fshrink-wrap-frame-pointer`.

**This seems to be an internal optimization, need to confirm.**

   </td>
   </tr>
   <tr>
   <td>

`NO_FRAME_POINTER`


   </td>
   <td>

`base/stacktrace_config.h`


   </td>
   <td>This macro should be defined when compiled with `-fomit-frame-pointer`.

We need this because there is no good way to check whether `-fomit-frame-pointer` is turned on.

   </td>
   </tr>
   <tr>
   <td>

`ABSL_FORCE_PTS_MODE`


   </td>
   <td>

`base/per-thread-sem.cc`


   </td>
   <td>`ABSL_PTS_MODE` is usually chosen based on OS.

This macro sets `ABSL_PTS_MODE` manually. 

`ABSL_PTS_MODE` branches the implementation for the `Waiter` class.

   </td>
   </tr>
   <tr>
   <td>
   `ADDRESS_SANITIZE`
   </td>
   <td>
   </td>
   <td>
   This macro should be defined when compiled with `-fsanitize=address`.
   </td>
   </tr>
   </table>

## Feature Check Macros

At compile time, we sometimes need to conditionally compile our code based on whether a feature
is provided by the underlying platform (architecture, OS, compiler, C++ standard, library) for
various purposes (portability, performance, sanity check, etc.). In such case, you should
define a feature check macro to check whether the feature is available or missing. Introducing
a feature check macro is better than writing a complex conditional directly because it explicitly
states which feature it is checking and makes the code more readable and maintainable.

### Definition

In Abseil, feature check macros are named with `ABSL_` prefix and defined like the following:

```cpp
#ifdef ABSL_HAVE_FEATURE_FOO
#error "ABSL_HAVE_FEATURE_FOO cannot be overridden."
#elif <complex preprocessor conditional>
#define ABSL_HAVE_FEATURE_FOO 1
#endif
```
Feature check macros are not allowed to be overriden (`-D`) on the compiler command line.
They should only be derived from pre-defined macros, other feature check macros or build
configuration macros.

In terms of writing the preprocessor conditional, you can either white-list all the platforms
where the feature is available, or black-list all the platforms where the feature is missing.
They both have their own pros and cons:

<table>
 <tr>
 <td></td>
 <td>white-list</td>
 <td>black-list</td>
 </tr>
 <tr>
 <td>pros</td>
 <td>It's conservative and safe, and more likely to build on a new platform.</td>
 <td>The compilation fails if a new platform doesn’t support the feature.</td>
 </tr>
 <tr>
 <td>cons</td>
 <td>The list needs to be extended for new platforms and can turn into a very long list.</td>
 <td>The compilation might succeed but runtime behavior might be unexpected, if the interface exists but implementation sucks.</td>
 </tr>
</table>

It is really a case-by-case decision on which approach to use. The guideline is to think about
what is likely to happen when a new platform is added and which approach is easier to maintain
the invariant of your module. Some examples are:

1. If you are working around a compiler bug or a missing C++ feature in the standard library
implementation, you probably want to black-list the combinations where the feature is missing.
This way, when a new platform is added, you assume it is standard-compliant, and your unit test
will fail if it is not.

2. If you are relying on a low-level OS feature for better functionality (more logging, better
debugging context), you probably want to white-list the OSes that claim to support this feature.
This way, when a new OS is added, you assume the feature is missing, and your module will
continue to work with the minimum functionality. If desired, the users can decide to opt-in by
extending the white-list.

### Usage

Feature check macros are used to conditionally compile your code like the following:

```cpp
#ifdef ABSL_HAVE_FEATURE_FOO  // or #if ABSL_HAVE_FEATURE_FOO
// Handle the case where feature foo is available.
#else
// Handle the case where feature foo is missing.
#endif
```

Make sure you `#include` the header where the feature check macro is defined.

### Feature Check Macros in Third-party OSS

Abseil has dependency on some third-party OSS libraries like
[Google Test](https://github.com/google/googletest), which also defines some feature check
macros to help writing tests. For example: `GTEST_HAS_DEATH_TEST`.
For these macros, you should just follow the convention of the third-party repo.