.. meta::
   :description: Protect your IP with Nuitka and Themida combined with VM technology
   :keywords: python,protection,reverse engineering,vm,Themida,WinLicense

################
 Nuitka Bundles
################

Nuitka can be purchased together as a bundle with other software.

Right now, the offer available is Themida and WinLicense. Both of these
are Oreans products, for which Nuitka Services acts as both a reseller,
and as a integrator, i.e. there is a Nuitka plugin that allows
additional features in Python code.

.. note::

   WinLicense combines the same protection-level as Themida with the
   power of advanced license control, offering the most powerful and
   flexible technology that allows developers to securely distribute
   trial and registered versions of their applications.

.. note::

   The limitations right now are.

   -  Only works in Windows, for other platforms we are preparing
      alternative bundles.
   -  Requires Visual Studio 2015 installation.
   -  Only works in Nuitka commercial, in fact Nuitka Themida is a
      superset of Nuitka commercial.

##################
 What is Themida?
##################

Themida is an SDK, where you modify the C code with macros, that when
compiled, end up as DLL usages for a special DLL. In a next step, it
then modifies the created binary and applies VM protection according to
settings and those macros, can be using multiple VMs for different code,
checks for running debugger, etc.

Nuitka Themida adds the ability, to do this directly in the Python code.
Is it not practical to do this manually with Nuitka, as it doesn't
respect things in protection.

##########
 Features
##########

****************
 File Embedding
****************

While Nuitka commercial allows embedding of data files, with Nuitka
Themida it is also possible to embed the CPython DLL, extension modules,
and DLLs used by it into one single binary. This on its own making it
much harder to attack with file replacements, editing.

With Nuitka Themida your binary is one file, without Nuitka
``--onefile`` mode which does not protect these files. But Nuitka
Themida makes it impossible to access these files.

************************
 Enhanced anti-debugger
************************

Nuitka commercial has its own ``anti-debugger`` plugin, currently not
listed as an official feature. But Themida has a more advanced
protection at this time.

*********************************
 Macros contained in Python code
*********************************

.. code:: python

    def sensitiveCode():

        something_that_prepares_but

        _inject_c_code = "VM_START;

        _


#########
 Pricing
#########

Oreans charges differently for single develop and team licenses. Also
with WinLicense, you get to use their C API to check license status at a
higher price. When bundled with Nuitka commercial, we can give you both
at a discount.

+----------------------+----------------+----------------+----------+--------+
|       Product        | Original Price | Nuitka Themida | Combined | Bundle |
+======================+================+================+==========+========+
| Themida Developer    | 199            | 401            | 600      | 550    |
+----------------------+----------------+----------------+----------+--------+
| Themida Company      | 399            | 401            | 800      | 700    |
+----------------------+----------------+----------------+----------+--------+
| WinLicense Developer | 399            | 401            | 800      | 700    |
+----------------------+----------------+----------------+----------+--------+
| WinLicense Company   | 799            | 401            | 1200     | 1100   |
+----------------------+----------------+----------------+----------+--------+

.. note::

   At the price, Nuitka Services cannot handle trial versions.
