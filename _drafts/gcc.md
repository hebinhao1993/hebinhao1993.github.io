---
title: gcc
date: 2019-06-21 14:49:39
tags:
---

记录一下gcc是如何编译c语言的。

<!-- more -->

假设我们有一个`helloworld.c`文件。其内容为

```c
#include <stdio.h>

int main()
{
    printf("hello world\n");
    return 0;
}
```

gcc 对该文件进行预处理

```sh
gcc -E helloworld.c -o helloworld.i
```

gcc 将`.c`文件认为是需要预处理的c源代码，将`.i`文件认为是不需要预处理的c源代码。

gcc在预处理中会进行头文件包含，注释删除等工作。

对于头文件包含而言，gcc是如何寻找头文件的呢？参考[Where Does GCC Look to Find its Header Files?](https://commandlinefanatic.com/cgi-bin/showarticle.cgi?article=art026)，对于用双引号包含的头文件，首先在c文件所在目录查找，然后在`-iqoute dir1 -iqoute dir2 ...`指令中指定的目录查找，然后在`-IPath1 -IPath2 ...`指令中指定的目录查找，然后在


CPATH -I
C_INCLUDE_PATH -isystem
CPLUS_INCLUDE_PATH -isystem
OBJC_INCLUDE_PATH -isystem


Options for Directory Search
    These options specify directories to search for header files, for libraries and for parts of the compiler:

    -I dir
    -iquote dir
    -isystem dir
    -idirafter dir
        Add the directory dir to the list of directories to be searched for header files during preprocessing.  If dir begins with =, then the = is replaced by the sysroot prefix; see --sysroot
        and -isysroot.

        Directories specified with -iquote apply only to the quote form of the directive, "#include "file"".  Directories specified with -I, -isystem, or -idirafter apply to lookup for both the
        "#include "file"" and "#include <file>" directives.

        You can specify any number or combination of these options on the command line to search for header files in several directories.  The lookup order is as follows:

        1.  For the quote form of the include directive, the directory of the current file is searched first.

        2.  For the quote form of the include directive, the directories specified by -iquote options are searched in left-to-right order, as they appear on the command line.

        3.  Directories specified with -I options are scanned in left-to-right order.

        4.  Directories specified with -isystem options are scanned in left-to-right order.

        5.  Standard system directories are scanned.

        6.  Directories specified with -idirafter options are scanned in left-to-right order.

        You can use -I to override a system header file, substituting your own version, since these directories are searched before the standard system header file directories.  However, you
        should not use this option to add directories that contain vendor-supplied system header files; use -isystem for that.

        The -isystem and -idirafter options also mark the directory as a system directory, so that it gets the same special treatment that is applied to the standard system directories.

        If a standard system include directory, or a directory specified with -isystem, is also specified with -I, the -I option is ignored.  The directory is still searched but as a system
        directory at its normal position in the system include chain.  This is to ensure that GCC's procedure to fix buggy system headers and the ordering for the "#include_next" directive are
        not inadvertently changed.  If you really need to change the search order for system directories, use the -nostdinc and/or -isystem options.

    -I- Split the include path.  This option has been deprecated.  Please use -iquote instead for -I directories before the -I- and remove the -I- option.

        Any directories specified with -I options before -I- are searched only for headers requested with "#include "file""; they are not searched for "#include <file>".  If additional
        directories are specified with -I options after the -I-, those directories are searched for all #include directives.

        In addition, -I- inhibits the use of the directory of the current file directory as the first search directory for "#include "file"".  There is no way to override this effect of -I-.

    -iprefix prefix
        Specify prefix as the prefix for subsequent -iwithprefix options.  If the prefix represents a directory, you should include the final /.

    -iwithprefix dir
    -iwithprefixbefore dir
        Append dir to the prefix specified previously with -iprefix, and add the resulting directory to the include search path.  -iwithprefixbefore puts it in the same place -I would;
        -iwithprefix puts it where -idirafter would.

    -isysroot dir
        This option is like the --sysroot option, but applies only to header files (except for Darwin targets, where it applies to both header files and libraries).  See the --sysroot option for
        more information.

    -imultilib dir
        Use dir as a subdirectory of the directory containing target-specific C++ headers.

    -nostdinc
        Do not search the standard system directories for header files.  Only the directories explicitly specified with -I, -iquote, -isystem, and/or -idirafter options (and the directory of the
        current file, if appropriate) are searched.

    -nostdinc++
        Do not search for header files in the C++-specific standard directories, but do still search the other standard directories.  (This option is used when building the C++ library.)

    -iplugindir=dir
        Set the directory to search for plugins that are passed by -fplugin=name instead of -fplugin=path/name.so.  This option is not meant to be used by the user, but only passed by the driver.

    -Ldir
        Add directory dir to the list of directories to be searched for -l.

    -Bprefix
        This option specifies where to find the executables, libraries, include files, and data files of the compiler itself.

        The compiler driver program runs one or more of the subprograms cpp, cc1, as and ld.  It tries prefix as a prefix for each program it tries to run, both with and without machine/version/
        for the corresponding target machine and compiler version.

        For each subprogram to be run, the compiler driver first tries the -B prefix, if any.  If that name is not found, or if -B is not specified, the driver tries two standard prefixes,
        /usr/lib/gcc/ and /usr/local/lib/gcc/.  If neither of those results in a file name that is found, the unmodified program name is searched for using the directories specified in your PATH
        environment variable.

        The compiler checks to see if the path provided by -B refers to a directory, and if necessary it adds a directory separator character at the end of the path.

        -B prefixes that effectively specify directory names also apply to libraries in the linker, because the compiler translates these options into -L options for the linker.  They also apply
        to include files in the preprocessor, because the compiler translates these options into -isystem options for the preprocessor.  In this case, the compiler appends include to the prefix.

        The runtime support file libgcc.a can also be searched for using the -B prefix, if needed.  If it is not found there, the two standard prefixes above are tried, and that is all.  The file
        is left out of the link if it is not found by those means.

        Another way to specify a prefix much like the -B prefix is to use the environment variable GCC_EXEC_PREFIX.

        As a special kludge, if the path provided by -B is [dir/]stageN/, where N is a number in the range 0 to 9, then it is replaced by [dir/]include.  This is to help with boot-strapping the
        compiler.

    -no-canonical-prefixes
        Do not expand any symbolic links, resolve references to /../ or /./, or make the path absolute when generating a relative prefix.

    --sysroot=dir
        Use dir as the logical root directory for headers and libraries.  For example, if the compiler normally searches for headers in /usr/include and libraries in /usr/lib, it instead searches
        dir/usr/include and dir/usr/lib.

        If you use both this option and the -isysroot option, then the --sysroot option applies to libraries, but the -isysroot option applies to header files.

        The GNU linker (beginning with version 2.16) has the necessary support for this option.  If your linker does not support this option, the header file aspect of --sysroot still works, but
        the library aspect does not.

    --no-sysroot-suffix
        For some targets, a suffix is added to the root directory specified with --sysroot, depending on the other options used, so that headers may for example be found in dir/suffix/usr/include
        instead of dir/usr/include.  This option disables the addition of such a suffix.
