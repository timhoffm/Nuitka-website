.. post:: 2022/08/16 13:45
   :tags: compiler, Python, Nuitka
   :author: Kay Hayen

####################
 Nuitka Release 1.0
####################

This is to inform you about the new stable release of `Nuitka
<https://nuitka.net>`_. It is the extremely compatible Python compiler,
`"download now" </doc/download.html>`_.

This release contains a large amount of new features, while
consolidating what we have with many bug fixes. Scalability should be
dramatically better, as well as new optimization that will accelerate
some code quite a bit. See the summary, how this release is paving the
way forward.

***********
 Bug Fixes
***********

-  Python3: Fix, ``bytes.decode`` with only ``errors`` argument given
   was not working. Fixed in 0.9.1 already.

-  MSYS2: Fix, the accelerate mode ``.cmd`` file was not working
   correctly. Fixed in 0.9.1 already.

-  Onefile: Fix, the bootstrap when waiting for the child, didn't
   protect against signals that interrupt this call. This only affected
   users of the non-public ``--onefile-tempdir`` option on Linux, but
   with that becoming the default in 1.0, this was discovered. Fixed in
   0.9.1 already.

-  Fix, ``pkg_resources`` compile time generated ``Distribution`` values
   could cause issues with code that put it into calls, or in tried
   blocks. Fixed in 0.9.1 already.

-  Standalone: Added implicit dependencies of ``Xlib`` package. Fixed in
   0.9.1 already.

-  macOS: Fix, the package configuration for ``wx`` had become invalid
   when restructuring the Yaml with code and schema disagreeing on
   allowed values. Fixed in 0.9.1 already.

-  Fix: The ``str.format`` with a single positional argument didn't
   generate proper code and failed to compile on the C level. Fixed in
   0.9.1 already.

-  Fix, the type shape of ``str.count`` result was wrong. Fixed in 0.9.1
   already.

-  UI: Fix, the warning about collision of just compiled package and
   original package in the same folder hiding the compiled package
   should not apply to packages without an ``__init__.py`` file, as
   those do **not** take precedence. Fixed in 0.9.2 already.

-  Debugging: Fix, the fallback to ``lldb`` from ``gdb`` when using the
   option ``--debugger`` was broken on anything but Windows. Fixed in
   0.9.2 already.

-  Python3.8: The module ``importlib.metadata`` was not recognized
   before 3.9, but actually 3.8 already has it, causing the compile time
   resolution of package versions to not work there. Fixed in 0.9.3
   already.

-  Standalone: Fix, at least on macOS we should also scan from parent
   folders of DLLs, since they may contain sub-directories in their
   names. This is mostly the case, when using frameworks. Fixed in 0.9.2
   already.

-  Standalone: Added package configuration for ``PyQt5`` to require
   onefile bundle mode on macOS, and recommend to disable console for
   PyQt6. This is same as we already do for ``PySide2`` and ``PySide6``.
   Fixed in 0.9.2 already.

-  Standalone: Removed stray macOS onefile bundle package configuration
   for ``pickle`` module which must have been added in error. Fixed in
   0.9.2 already.

-  UI: Catch user error of attempting to compile the ``__init__.py``
   rather than the package directory. Fixed in 0.9.2 already.

-  Fix, hard name import nodes failed to clone, causing issues in
   optimization phase. Fixed in 0.9.2 already.

-  Fix, avoid warnings given with gcc 11. Fixed in 0.9.2 already.

-  Fix, dictionary nodes where the operation itself has no effect, e.g.
   ``dict.copy`` were not properly annotating that their dictionary
   argument could still cause a raise and have side effects, triggering
   an assertion violation in Nuitka. Fixed in 0.9.2 already.

-  Standalone: Added ``pynput`` implicit dependencies on Linux. Fixed in
   0.9.2 already.

-  Fix, boolean condition checks on variables converted immutable
   constant value assignments to boolean values, leading to incorrect
   code execution. Fixed in 0.9.2 already.

-  Python3.9: Fix, could crash on generic aliases with non-hashable
   values. Fixed in 0.9.3 already.

   .. code:: python

      dict[str:any]

-  Python3: Fix, an iteration over ``sys.version_info`` was falsely
   optimized into a tuple, which is not always compatible. Fixed in
   0.9.3 already.

-  Standalone: Added support for ``xgboost`` package. Fixed in 0.9.3
   already.

-  Standalone: Added data file for ``text_unidecode`` package. Fixed in
   0.9.4 already.

-  Standalone: Added data files for ``swagger_ui_bundle`` package. Fixed
   in 0.9.4 already.

-  Standalone: Added data files for ``connexion`` package. Fixed in
   0.9.4 already.

-  Standalone: Added implicit dependencies for ``sklearn.utils`` and
   ``rapidfuzz``. Fixed in 0.9.4 already.

-  Python3.10: Fix, the reformulation of ``match`` statements could
   create nodes that are used twice, causing code generation to assert.
   Fixed in 0.9.4 already.

-  Fix, module objects removed from ``sys.modules`` but still used could
   lack a reference to themselves, and therefore crash due to working on
   a released module variables dictionary. Fixed in 0.9.5 already.

-  Fix, the MSVC compiles code generated for SciPy 1.8 wrongly. Added a
   workaround for that code to avoid triggering it. Fixed in 0.9.6
   already.

-  Fix, calls to ``str.format`` where the result is not used, could
   crash the compiler during code generation. Fixed in 0.9.6 already.

-  Standalone: For DLLs on macOS and Anaconda, also consider the ``lib``
   directory of the root environment, as some DLLs are otherwise not
   found.

-  Fix, allow ``nonlocal`` and ``global`` for ``__class__`` to be used
   on the class level.

-  Fix, ``xrange`` with large values didn't work on all platforms. This
   affected at least Python2 on macOS, but potentially others as well.

-  Windows: When scanning for installed Pythons to e.g. run Scons or
   onefile compression, it was attempting to use installations that got
   deleted manually and could crash.

-  macOS: Fix, DLL conflicts are now resolved by checking the version
   information too, also all cases that previously errored out after a
   conflict was reported, will now work.

-  Fix, conditional expressions whose statically decided condition
   picking a branch will raise an exception could crash the compilation.

   .. code:: python

      # Would previously crash Nuitka during optimization.
      return 1/0 if os.name == "nt" else 1/0

-  Windows: Make sure we set C level standard file handles too

   At least newer subprocess was affected by this, being unable to
   provide working handles to child processes that pass their current
   handles through, and also this should help DLL code to use it as
   level.

-  Standalone: Added support for ``pyqtgraph`` data files.

-  Standalone: Added support for ``dipy`` by anti-bloat removal of its
   testing framework that wants to do unsupported stuff.

-  UI: Could still give warnings about modules not being followed, where
   that was not true.

-  Fix, ``--include-module`` was not working for non-automatic standard
   library paths.

**************
 New Features
**************

-  Onefile: Recognize a non-changing path from
   ``--onefile-tempdir-spec`` and then use cached mode. By default a
   temporary folder is used in the spec value, make it delete the files
   afterwards.

   The cached mode is not necessarily faster, but it is not going to
   change files already there, leaving the binaries there intact. In the
   future it may also become faster to execute, but right now checking
   the validity of the file takes about as long as re-creating it,
   therefore no gain yet. The main point, is to not change where it runs
   from.

-  Standalone: Added option to exclude DLLs. You can npw use
   ``--noinclude-dlls`` to exclude DLLs by filename patterns.

   The may e.g. come from Qt plugins, where you know, or experimented,
   that it is not going to be used in your specific application. Use
   with care, removing DLLs will lead to very hard to recognize errors.

-  Anaconda: Use ``CondaCC`` from environment variables for Linux and
   macOS, in case it is installed. This can be done with e.g. ``conda
   install gcc-linux-64`` on Linux or ``conda install clang_osx-64`` on
   macOS.

-  Added new option ``--nowarn-mnemonic`` to disable warnings that use
   mnemonics, there is currently not that many yet, but it's going to
   expand. You can use this to acknowledge the ones you accept, and not
   get that warning with the information pointer anymore.

-  Added method for resolving DLL conflicts on macOS too. This is using
   version information and picks the newer one where possible.

-  Added option ``--user-package-configuration-file`` for user provided
   Yaml files, which can be used to provide package configuration to
   Nuitka, to e.g. add DLLs, data files, do some anti-bloat work, or add
   missing dependencies locally. The documentation for this does not yet
   exist though, but Nuitka contains a Yaml schema in the
   ``misc/nuitka-package-config-schema.json`` file.

-  Added ``nuitka-project-else`` to avoid repeating conditions in Nuitka
   project configuration, this can e.g. be used like this:

   .. code:: python

      # nuitka-project-if: os.getenv("TEST_VARIANT", "pyside2") == "pyside2":
      #   nuitka-project: --enable-plugin=no-qt
      # nuitka-project-else:
      #   nuitka-project: --enable-plugin=no-qt
      #   nuitka-project: --noinclude-data-file=*.svg

   Previously, the inverted condition had to be used in another
   ``nuitka-project-if`` which is no big deal, but less readable.

-  Added support for deep copying uncompiled functions. There is now a
   section in the User Manual that explains how to clone compiled
   functions. This allows a workaround like this:

   .. code:: python

      def binder(func, name):
         try:
            result = func.clone()
         except AttributeError:
            result = types.FunctionType(func.__code__, func.__globals__, name=func.__name__, argdefs=func.__defaults__, closure=func.__closure__)
            result = functools.update_wrapper(result, func)
            result.__kwdefaults__ = func.__kwdefaults__

         result.__name__ = name
         return result

-  Plugins: Added explicit deprecation status of a plugin. We now have a
   few that do nothing, and are just there for compatibility with
   existing users, and this now informs the user properly rather than
   just saying it is not relevant.

-  Fix, some Python installations crash when attempting to import
   modules, such as ``os`` with a ``ModuleName`` object, because we
   limit string operations done, and e.g. refuse to do ``.startswith``
   which of course, other loaders that your installation has added,
   might still use.

-  Windows: In case of not found DLLs, we can still examine the run time
   of the currently compiling Python process of Nuitka, and locate them
   that way, which helps for some Python configurations to support
   standalone, esp. to find CPython DLL in unusual spots.

-  Debian: Workaround for ``lib2to3`` data files. These are from stdlib
   and therefore the patched code from Debian needs to be undone, to
   make these portable again.

**************
 Optimization
**************

-  Scalability: Avoid merge traces of initial variable versions, which
   came into play when merging a variable used in only one branch. These
   are useless and only made other optimization slower or impossible.

-  Scalability: Also avoid merge traces of merge traces, instead flatten
   merge traces and avoid the duplication doing so. There were
   pathological cases, where this reduced optimization time for
   functions from infinite to instant.

-  For comparison helpers, switch comparison where possible, such that
   there are only 3 variants, rather than 6. Instead the boolean result
   is inverted, e.g. changing ``>=`` into ``not <`` effectively. Of
   course this can only be done for types, where we know that nothing
   special, i.e. no method overloads of ``__gte__`` is going on.

-  For binary operations that are commutative with the selected types,
   in mixed type cases, swap the arguments during code generation, such
   that e.g. ``long_a + float_b`` is actually computed as ``float_b +
   long_a``. This again avoids many helpers. It also can be done for
   ``*`` with integers and container types.

-  In cases, where a comparison (or one of the few binary operation
   where we consider it useful), is used in a boolean context, but we
   know it is impossible to raise an exception, a C boolean result type
   is used rather than a ``nuitka_bool`` which is now only used when
   necessary, because it can indicate the exception result.

-  Anti-Bloat: More anti-bloat work was done for popular packages,
   covering also uses of ``setuptools_scm``, ``nose`` and ``nose2``
   package removals and warnings. There was also a focus on making
   ``mmvc``, ``tensorflow`` and ``tifffile`` compile well, removing e.g.
   the uses of the tensorflow testing framework.

-  Faster comparison of ``int`` values with constant values, this uses
   helpers that work with C ``long`` values that represent a single
   "digit" of a value, or ones that use the full value space of C
   ``long``.

-  Faster comparison of ``float`` values with constant values, this uses
   helpers that work with C ``float`` values, avoiding the useless
   Python level constant objects.

-  Python2: Comparison of ``int`` and ``long`` now has specialized
   helpers that avoids converting the ``int`` to a ``long`` through
   coercion. This takes advantage of code to compare C ``long`` values
   (which are at the core of Python2 ``int`` objects, with ``long``
   objects.

-  For binary operation on mixed types, e.g. ``int * bytes`` the slot of
   the first function was still considered, and called to give a
   ``Py_NotImplemented`` return value for no good reason. This also
   applies to mixed operations of ``int``, ``long``, and ``float``
   types, and for ``str`` and ``unicode`` values on Python2.

-  Added missing helper for ``**`` operation with floats, this had been
   overlooked so far.

-  Added dedicated nodes for ``ctypes.CDLL`` which aims to allow us to
   detect used DLLs at compile time in the future, and to move closer to
   support its bindings more efficiently.

-  Added specialized nodes for ``dict.popitem`` as well. With this, now
   all of the dictionary methods are specialized.

-  Added specialized nodes for ``str.expandtabs``, ``str.translate``,
   ``str.ljust``, ``str.rjust``, ``str.center``, ``str.zfill``, and
   ``str.splitlines``. While these are barely performance relevant, this
   completes all ``str`` methods, except ``removeprefix`` and
   ``removesuffix`` that are Python3.9 or higher.

-  Added type shape for result of ``str.index`` operation as well, this
   was missing so far.

-  Optimize ``str``, ``bytes`` and ``dict`` method calls through
   variables.

-  Optimize calls through variables containing e.g. mutable constant
   values, these will be rare, because they all become exceptions.

-  Optimize calls through variables containing built-in values,
   unlocking optimization of such calls, where it is assigned to a local
   variable.

-  For generated attribute nodes, avoid local doing import statements on
   the function level. While these were easier to generate, they can
   only be slow at runtime.

-  For the ``str`` built-in annotate its value as derived from ``str``,
   which unfortunately does not allow much optimization, since that can
   still change many things, but it was still a missing attribute.

-  For variable value release nodes, specialize them by value type as
   well, enhancing the scalability, because e.g. parameter variable
   specific tests, need not be considered for all other variable types
   as well.

****************
 Organisational
****************

-  Plugins: Major changes to the Yaml file content, cleaning up some of
   the DLL configuration to more easy to use.

   The DLL configuration has two flavors, one from code and one from
   filename matching, and these got separated into distinct items in the
   Yaml configuration. Also how source and dest paths get provided got
   simplified, with a relative path now being used consistently and with
   sane defaults, deriving the destination path from where the module
   lives. Also what we called patterns, are actually prefixes, as there
   is still the platform specific DLL file naming appended.

-  Plugins: Move mode checks to dedicated plugin called
   ``options-nanny`` that is always enabled, giving also much cleaner
   Yaml configuration with a new section added specifically for these.
   It controls advice on the optional or required use of
   ``--disable-console`` and the like. Some packages, e.g. ``wx`` are
   known to crash on macOS when the console is enabled, so this advice
   is now done with saner configuration.

-  Plugins: Also for all Yaml configuration sub-items where is now a
   consistent ``when`` field, that allows checking Python version, OS,
   Nuitka modes such as standalone, and only apply configuration when
   matching this criterion, with that the anti-bloat options to allow
   certain bloat, should now have proper effect as well.

-  The use of ``AppImage`` on Linux is no more. The performance for
   startup was always slower, while having lost the main benefit of
   avoiding IO at startup, due to new cached mode, so now we always use
   the same bootstrap binary as on macOS and Windows.

-  UI: Do not display implicit reports reported by plugins by default
   anymore. These have become far too many, esp. with the recent stdlib
   work, and often do not add any value. The compilation report will
   become where to turn to find out why a module in included.

-  UI: Ask the user to install the ordered set package that will
   actually work for the specific Python version, rather than making him
   try one of two, where sometimes only one can work, esp. with Python
   3.10 allowing only one.

-  GitHub: More clear wording in the issue template that ``python -m
   nuitka --version`` output is really required for support to given.

-  Attempt to use Anaconda ``ccache`` binary if installed on
   non-Windows. This is esp. handy on macOS, where it is harder to get
   it.

-  Windows: Avoid byte-compiling the inline copy of Scons that uses
   Python3 when installing for Python2.

-  Added experimental switches to disable certain optimization in order
   to try out their impact, e.g. on corruption bugs.

-  Reports: Added included DLLs for standalone mode to compilation
   report.

-  Reports: Added control tags influencing plugin decisions to the
   compilation report.

-  Plugins: Make the ``implicit-imports`` dependency section in the Yaml
   package configuration a list, for consistency with other blocks.

-  Plugins: Added checking of tags such from the package configuration,
   so that for things dependent on python version (e.g.
   ``python39_or_higher``, ``before_python39``), the usage of Anaconda
   (``anaconda``) or certain OS (e.g. ``macos``), or modes (e.g.
   ``standalone``), expressions in ``when`` can limit a configuration
   item.

-  Quality: Re-enabled string normalization from black, the issues with
   changes that are breaking to Python2 have been worked around.

-  User Manual: Describe using a minimal virtualenv as a possible help
   low memory situations as well.

-  Quality: The yaml auto-format now properly preserves comments, being
   based on ``ruamel.yaml``.

-  Nuitka-Python: Added support for the Linux build with Nuitka-Python
   for our own CPython fork as well, previously only Windows was
   working, amd macOS will follow later.

-  The commit hook when installed from git bash was working, but doing
   so from ``cmd.exe`` didn't find a proper path for shell from the
   ``git`` location.

-  Debugging: A lot of experimental toggles were added, that allow
   control over the use of certain optimization, e.g. use of dict, list,
   iterators, subscripts, etc. internals, to aid in debugging in
   situations where it's not clear, if these are causing the issue or
   not.

-  Added support for Fedora 36, which requires some specific linker
   options, also recognize Fedora based distributions as such.

-  Removed long deprecated option ``--noinclude-matplotlib`` from numpy
   plugin, as it hasn't had an effect for a long time now.

-  Visual Code: Added extension for editing Jinja2 templates. This one
   even detects that we are editing C or Python and properly highlights
   accordingly.

**********
 Cleanups
**********

-  Standalone: Major cleanup of the dependency analysis for standalone.
   There is no longer a distinction between entry points (main binary,
   extension modules) and DLLs that they depend on. The OS specific
   parts got broken out into dedicated modules as well and decisions are
   now taken immediately.

-  Plugins: Split the Yaml package configuration files into 3 files. One
   contains now Python2 only stdlib configuration, and another one
   general stdlib.

-  Plugins: Also cleanup the ``zmq`` plugin, which was one the last
   holdouts of now removed plugin method, moving parts to the Yaml
   configuration. We therefore no longer have ``considerExtraDlls``
   which used to work on the standalone folder, but instead only plugin
   code that provides included DLL or binary objects from
   ``getExtraDlls`` which gives Nuitka much needed control over DLL
   copying. This was a long lasting battle finally won, and will allow
   many new features to come.

-  UI: Avoid changing whitespace in warnings, where we have intended
   line breaks, e.g. in case of duplicate DLLs. Went over all warnings
   and made sure to either avoid new-lines or have them, depending on
   wanted output.

-  Iterator end check code now uses the same code as rich comparison
   expressions and can benefit from optimization being done there as
   well.

-  Solved TODO item about code generation time C types to specify if
   they have error checking or not, rather than hard coding it.

-  Production of binary helper function set was cleaned up massively,
   but still needs more work, comparison helper function set was also
   redesigned.

-  Changing the spelling of our container package to become more clear.

-  Used ``namedtuple`` objects for storing used DLL information for more
   clear code.

-  Added spellchecker ignores for all attribute and argument names of
   generated fixed attribute nodes.

-  In auto-format make sure the imports float to the top. That very much
   cleans up generated attribute nodes code, allowing also to combine
   the many ones it makes, but also cleans up some of our existing code.

-  The package configuration Yaml files are now sorted according to
   module names. This will help to avoid merge conflicts during hotfixes
   merge back to develop and automatically group related entries in a
   sane way.

-  Moved large amounts of code producing implicit imports to Yaml
   configuration files.

-  Changed the ``tensorflow`` plugin to Yaml based configuration, making
   it a deprecated do nothing plugin, that only remains there for a few
   releases, to not crash existing build scripts.

-  Lots of spelling cleanups, e.g. renaming ``nuitka.codegen`` to
   ``nuitka.code_generation`` for clarity.

*******
 Tests
*******

-  Added generated test to cover ``bytes`` method. This would have found
   the issue with ``decode`` potentially.

-  Enhanced standalone test for ``ctypes`` on Linux to actually have
   something to test.

*********
 Summary
*********

This release improves on many things at once. A lot of work has been put
into polishing the Yaml configuration that now only lacks documentation
and examples, such that the community as a whole should become capable
of adding missing dependencies, data files, DLLs, and even anti-bloat
patches.

Then a lot of new optimization has been done, to close the missing gaps
with ``dict`` and ``str`` methods, but before completing ``list`` which
is already a work in progress pull request, and ``bytes``, we want to
start and generate the node classes that form the link or basis of
dedicated nodes. This will be an area to work on more.

The many improvements to existing code helpers, and them being able to
pick target types for the arguments of comparisons and binary
operations, is a pre-cursor to universal optimization of this kind. What
is currently only done for constant values, will in the future be
interesting for picking specific C types for use. That will then be a
huge difference from what we are doing now, where most things still have
to use ``PyObject *`` based types.

Scalability has again seen very real improvements, memory usage of
Nuitka itself, as well as compile time inside Nuitka are down by a lot
for some cases, very noticeable. There is never enough of this, but it
appears, in many cases now, large compilations run much faster.

For macOS specifically, the new DLL dependency analysis, is much more
capable or resolving conflicts all by itself. Many of the more complex
packages with some variants of Python, specifically Anaconda will now be
working a lot better.

And then, of course there is the big improvement for Onefile, that
allows to use cached paths. This will make it more usable in the general
case, e.g. where the firewall of Windows hate binaries that change their
path each time they run.

Future directions will aim to make the compilation report more concise,
and given reasons and dependencies as they are known on the inside more
clearly, such that is can be a major tool for testing, bug reporting and
analysis of the compilation result.
