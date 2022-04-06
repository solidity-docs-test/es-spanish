********************************************
Composición de un fichero fuente de Solidity
********************************************

<<<<<<< HEAD
Los ficheros fuente pueden contener un número arbitrario de definiciones de contrato, incluir directivas y directivas de pragma.
=======
Source files can contain an arbitrary number of
:ref:`contract definitions<contract_structure>`, import_ ,
:ref:`pragma<pragma>` and :ref:`using for<using-for>` directives and
:ref:`struct<structs>`, :ref:`enum<enums>`, :ref:`function<functions>`, :ref:`error<errors>`
and :ref:`constant variable<constants>` definitions.

.. index:: ! license, spdx

SPDX License Identifier
=======================

Trust in smart contracts can be better established if their source code
is available. Since making source code available always touches on legal problems
with regards to copyright, the Solidity compiler encourages the use
of machine-readable `SPDX license identifiers <https://spdx.org>`_.
Every source file should start with a comment indicating its license:

``// SPDX-License-Identifier: MIT``

The compiler does not validate that the license is part of the
`list allowed by SPDX <https://spdx.org/licenses/>`_, but
it does include the supplied string in the :ref:`bytecode metadata <metadata>`.

If you do not want to specify a license or if the source code is
not open-source, please use the special value ``UNLICENSED``.
Note that ``UNLICENSED`` (no usage allowed, not present in SPDX license list)
is different from ``UNLICENSE`` (grants all rights to everyone).
Solidity follows `the npm recommendation <https://docs.npmjs.com/cli/v7/configuring-npm/package-json#license>`_.

Supplying this comment of course does not free you from other
obligations related to licensing like having to mention
a specific license header in each source file or the
original copyright holder.

The comment is recognized by the compiler anywhere in the file at the
file level, but it is recommended to put it at the top of the file.

More information about how to use SPDX license identifiers
can be found at the `SPDX website <https://spdx.org/ids-how>`_.


.. index:: ! pragma

.. _pragma:

Pragmas
=======

The ``pragma`` keyword is used to enable certain compiler features
or checks. A pragma directive is always local to a source file, so
you have to add the pragma to all your files if you want to enable it
in your whole project. If you :ref:`import<import>` another file, the pragma
from that file does *not* automatically apply to the importing file.
>>>>>>> 31b548577994397f7f849e8dd005d9d7aaa48c76

.. index:: ! pragma, version

.. _version_pragma:

<<<<<<< HEAD
Versión de Pragma
=================

Es altamente recomendable anotar los ficheros fuente con la versión de pragma para impedir que se compilen con una version posterior del compilador que podría introducir cambios incompatibles. Intentamos mantener este tipo de cambios al mínimo y que los cambios que introduzcamos y modifiquen la semántica requieran también un cambio en la sintáxis, pero esto no siempre es posible. Por ese motivo, siempre es buena idea repasar el registro de cambios por lo menos para las nuevas versiones que contienen cambios de ruptura. Estas nuevas versiones siempre tendrán un número de versión del tipo ``0.x.0`` o ``x.0.0``.

La versión de pragma se usa de la siguiente manera:
=======
Version Pragma
--------------

Source files can (and should) be annotated with a version pragma to reject
compilation with future compiler versions that might introduce incompatible
changes. We try to keep these to an absolute minimum and
introduce them in a way that changes in semantics also require changes
in the syntax, but this is not always possible. Because of this, it is always
a good idea to read through the changelog at least for releases that contain
breaking changes. These releases always have versions of the form
``0.x.0`` or ``x.0.0``.

The version pragma is used as follows: ``pragma solidity ^0.5.2;``
>>>>>>> 31b548577994397f7f849e8dd005d9d7aaa48c76

A source file with the line above does not compile with a compiler earlier than version 0.5.2,
and it also does not work on a compiler starting from version 0.6.0 (this
second condition is added by using ``^``). Because
there will be no breaking changes until version ``0.6.0``, you can
be sure that your code compiles the way you intended. The exact version of the
compiler is not fixed, so that bugfix releases are still possible.

<<<<<<< HEAD
Un fichero como este no se compilará con un compilador con una versión anterior a ``0.4.0`` y tampoco funcionará con un compilador que tiene una versión posterior a ``0.5.0`` (se especifíca esta segunda condición usando el ``^``). La idea detrás de esto es que no va a haber un cambio de ruptura antes de la versión ``0.5.0``, así que podemos estar seguro de que nuestro código se compilará de la manera en que nosotros esperamos. No fijamos la versión exacta del compilador, de manera que podemos liberar nuevas versiones que corrigen bugs.

Se puede especificar reglas mucho más complejas para la versión del compilador, la expresión sigue las utilizadas por `npm <https://docs.npmjs.com/misc/semver>`_.
=======
It is possible to specify more complex rules for the compiler version,
these follow the same syntax used by `npm <https://docs.npmjs.com/cli/v6/using-npm/semver>`_.

.. note::
  Using the version pragma *does not* change the version of the compiler.
  It also *does not* enable or disable features of the compiler. It just
  instructs the compiler to check whether its version matches the one
  required by the pragma. If it does not match, the compiler issues
  an error.
>>>>>>> 31b548577994397f7f849e8dd005d9d7aaa48c76

ABI Coder Pragma
----------------

By using ``pragma abicoder v1`` or ``pragma abicoder v2`` you can
select between the two implementations of the ABI encoder and decoder.

The new ABI coder (v2) is able to encode and decode arbitrarily nested
arrays and structs. It might produce less optimal code and has not
received as much testing as the old encoder, but is considered
non-experimental as of Solidity 0.6.0. You still have to explicitly
activate it using ``pragma abicoder v2;``. Since it will be
activated by default starting from Solidity 0.8.0, there is the option to select
the old coder using ``pragma abicoder v1;``.

The set of types supported by the new encoder is a strict superset of
the ones supported by the old one. Contracts that use it can interact with ones
that do not without limitations. The reverse is possible only as long as the
non-``abicoder v2`` contract does not try to make calls that would require
decoding types only supported by the new encoder. The compiler can detect this
and will issue an error. Simply enabling ``abicoder v2`` for your contract is
enough to make the error go away.

.. note::
  This pragma applies to all the code defined in the file where it is activated,
  regardless of where that code ends up eventually. This means that a contract
  whose source file is selected to compile with ABI coder v1
  can still contain code that uses the new encoder
  by inheriting it from another contract. This is allowed if the new types are only
  used internally and not in external function signatures.

.. note::
  Up to Solidity 0.7.4, it was possible to select the ABI coder v2
  by using ``pragma experimental ABIEncoderV2``, but it was not possible
  to explicitly select coder v1 because it was the default.

.. index:: ! pragma, experimental

.. _experimental_pragma:

Experimental Pragma
-------------------

The second pragma is the experimental pragma. It can be used to enable
features of the compiler or language that are not yet enabled by default.
The following experimental pragmas are currently supported:


ABIEncoderV2
~~~~~~~~~~~~

Because the ABI coder v2 is not considered experimental anymore,
it can be selected via ``pragma abicoder v2`` (please see above)
since Solidity 0.7.4.

.. _smt_checker:

SMTChecker
~~~~~~~~~~

This component has to be enabled when the Solidity compiler is built
and therefore it is not available in all Solidity binaries.
The :ref:`build instructions<smt_solvers_build>` explain how to activate this option.
It is activated for the Ubuntu PPA releases in most versions,
but not for the Docker images, Windows binaries or the
statically-built Linux binaries. It can be activated for solc-js via the
`smtCallback <https://github.com/ethereum/solc-js#example-usage-with-smtsolver-callback>`_ if you have an SMT solver
installed locally and run solc-js via node (not via the browser).

If you use ``pragma experimental SMTChecker;``, then you get additional
:ref:`safety warnings<formal_verification>` which are obtained by querying an
SMT solver.
The component does not yet support all features of the Solidity language and
likely outputs many warnings. In case it reports unsupported features, the
analysis may not be fully sound.

.. index:: source file, ! import, module, source unit

.. _import:

Importar otros ficheros fuente
==============================

Sintáxis y semántica
--------------------

<<<<<<< HEAD
Solidity soporta la importación de declaraciones, que son muy similares a las que se hacen en JavaScript (a partir de ES6), aunque Solidity no conoce el concepto de "default export".
=======
Solidity supports import statements to help modularise your code that
are similar to those available in JavaScript
(from ES6 on). However, Solidity does not support the concept of
a `default export <https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export#Description>`_.
>>>>>>> 31b548577994397f7f849e8dd005d9d7aaa48c76

A nivel global, se puede usar la importación de declaraciones de la siguiente manera:

.. code-block:: solidity

    import "filename";

<<<<<<< HEAD
Esta declaración importa todos los símbolos globales de "filename" (junto con los símbolos importados desde allí) en el alcance global actual (diferente que en ES6 pero compatible con versiones anteriores de Solidity).
=======
The ``filename`` part is called an *import path*.
This statement imports all global symbols from "filename" (and symbols imported there) into the
current global scope (different than in ES6 but backwards-compatible for Solidity).
This form is not recommended for use, because it unpredictably pollutes the namespace.
If you add new top-level items inside "filename", they automatically
appear in all files that import like this from "filename". It is better to import specific
symbols explicitly.
>>>>>>> 31b548577994397f7f849e8dd005d9d7aaa48c76

The following example creates a new global symbol ``symbolName`` whose members are all
the global symbols from ``"filename"``:

.. code-block:: solidity

<<<<<<< HEAD
...crea un nuevo símbolo global ``symbolName`` cuyos miembros son todos los símbolos globales de ``"filename"``.
=======
    import * as symbolName from "filename";
>>>>>>> 31b548577994397f7f849e8dd005d9d7aaa48c76

which results in all global symbols being available in the format ``symbolName.symbol``.

A variant of this syntax that is not part of ES6, but possibly useful is:

<<<<<<< HEAD
...crea un nuevo símbolo global ``alias`` y ``symbol2`` que referencian respectivamente ``symbol1`` y ``symbol2`` desde ``"filename"``.

Esta otra sintaxis no forma parte de ES6 pero es probablemente conveniente:

::
=======
.. code-block:: solidity
>>>>>>> 31b548577994397f7f849e8dd005d9d7aaa48c76

  import "filename" as symbolName;

lo que es equivalente a ``import * as symbolName from "filename";``.

<<<<<<< HEAD
Ruta
----

En lo que hemos visto más arriba, ``filename`` siempre se trata como una ruta con el ``/`` como separador de directorio, ``.`` como el directorio actual y ``..`` como el directorio padre. Cuando ``.`` o ``..`` es seguido por un carácter excepto ``/``, no se considera como el directorio actual o directorio padre. Se tratan todos los nombres de ruta como rutas absolutas, a no ser que empiecen por ``.`` o el directorio padre ``..``.

Para importar un fichero ``x`` desde el mismo directorio que el fichero actual, se usa `import "./x" as x;``. Si en lugar de esa expresión se usa ``import "x" as x;``, podría ser que se referencie un fichero distinto (en un "include directory" global).

Cómo se resuelve la ruta depende del compilador (ver más abajo). En general, la jerarquía de directorios no necesita mapear estrictamente su sistema local de ficheros, también puede mapear recursos que se descubren con por ejemplo ipfs, http or git.

Uso en compiladores actuales
----------------------------

Cuando se invoca el compilador, no sólo se puede especificar la manera en que se descubre el primer elemento de la ruta, también es posible especificar un prefijo de ruta de remapeo, de tal manera que por ejemplo ``github.com/ethereum/dapp-bin/library`` se remapee por ``/usr/local/dapp-bin/library`` y el compilador lea el fichero desde allí. Si bien se pueden hacer múltiples remapeos, se intentará primero con el remapeo con la clave más larga. Esto permite "fallback-remapping" con, por ejemplo, ``""`` mapea a ``/usr/local/include/solidity``. Además, estos remapeos pueden depender del contexto, lo que permite configurar paquetes para importar por ejemplo diferentes versiones de una librería con el mismo nombre.

**solc**:

Para solc (el compilador de línea de comando), los remapeos se proporcionan como argumentos ``context:prefix=target``, donde tanto la parte ``context:`` como la parte ``=target`` son opcionales (en este caso target toma por defecto el valor de prefix). Todos los valores que remapean que son ficheros estándares son compilados (incluyendo sus dependencias). Este mecanismo es completamente compatible con versiones anteriores (siempre y cuando ningún nombre de fichero contenga = o :) y por lo tanto no es un cambio de ruptura. Todas las importaciones en los ficheros en el directorio ``context`` (o debajo de él) que importa un fichero que empieza con un ``prefix`` están redireccionados reemplazando ``prefix`` por ``target``.

Como ejemplo, si clonas ``github.com/ethereum/dapp-bin/`` en tu local a ``/usr/local/dapp-bin``, puedes usar lo siguiente en tu código fuente:

::

  import "github.com/ethereum/dapp-bin/library/iterable_mapping.sol" as it_mapping;

y luego se corre el compilador de esa manera:

.. code-block:: bash

  solc github.com/ethereum/dapp-bin/=/usr/local/dapp-bin/ source.sol

Como ejemplo un poco más complejo, imaginemos que nos basamos en algún módulo que utiliza una versión muy antigua de dapp-bin. Esta versión anterior de dapp-bin se comprueba en ``/usr/local/dapp-bin_old``, y luego se puede usar:

.. code-block:: bash

  solc module1:github.com/ethereum/dapp-bin/=/usr/local/dapp-bin/ \
       module2:github.com/ethereum/dapp-bin/=/usr/local/dapp-bin_old/ \
       source.sol

así, todas las importaciones en ``module2`` apuntan a la versión anterior pero las importaciones en ``module1`` consiguen la nueva versión.

Nótese que solc sólo permite incluir ficheros desde algunos directorios. Tienen que estar en el diretorio (o subdirectorio) de uno de los ficheros fuente explícitamente especificado o en el directorio (o subdirectorio) de un target de remapeo. Si se desea permitir inclusiones directas absolutas, sólo hace falta añadir el ``=/`` de remapeo.

Si hay múltiples remapeos que conducen a un fichero válido, se elige el remapeo con el prefijo común más largo.

**Remix**:

`Remix <https://remix.ethereum.org/>`_ proporciona un remapeo automático para github y también recupera automáticamente el fichero desde la red: se puede importar el mapeo iterable con por ejemplo ``import "github.com/ethereum/dapp-bin/library/iterable_mapping.sol" as it_mapping;``.

A futuro se podrían añadir otros proveedores de código fuente.
=======
If there is a naming collision, you can rename symbols while importing. For example,
the code below creates new global symbols ``alias`` and ``symbol2`` which reference
``symbol1`` and ``symbol2`` from inside ``"filename"``, respectively.

.. code-block:: solidity

    import {symbol1 as alias, symbol2} from "filename";

.. index:: virtual filesystem, source unit name, import; path, filesystem path, import callback, Remix IDE

Import Paths
------------

In order to be able to support reproducible builds on all platforms, the Solidity compiler has to
abstract away the details of the filesystem where source files are stored.
For this reason import paths do not refer directly to files in the host filesystem.
Instead the compiler maintains an internal database (*virtual filesystem* or *VFS* for short) where
each source unit is assigned a unique *source unit name* which is an opaque and unstructured identifier.
The import path specified in an import statement is translated into a source unit name and used to
find the corresponding source unit in this database.

Using the :ref:`Standard JSON <compiler-api>` API it is possible to directly provide the names and
content of all the source files as a part of the compiler input.
In this case source unit names are truly arbitrary.
If, however, you want the compiler to automatically find and load source code into the VFS, your
source unit names need to be structured in a way that makes it possible for an :ref:`import callback
<import-callback>` to locate them.
When using the command-line compiler the default import callback supports only loading source code
from the host filesystem, which means that your source unit names must be paths.
Some environments provide custom callbacks that are more versatile.
For example the `Remix IDE <https://remix.ethereum.org/>`_ provides one that
lets you `import files from HTTP, IPFS and Swarm URLs or refer directly to packages in NPM registry
<https://remix-ide.readthedocs.io/en/latest/import.html>`_.
>>>>>>> 31b548577994397f7f849e8dd005d9d7aaa48c76

For a complete description of the virtual filesystem and the path resolution logic used by the
compiler see :ref:`Path Resolution <path-resolution>`.

.. index:: ! comment, natspec

Comentarios
===========

Se aceptan los comentarios de línea simple (``//``) y los comentarios de múltiples líneas (``/*...*/``).

.. code-block:: solidity

<<<<<<< HEAD
  // Esto es un comentario de línea simple.

  /*
  Esto es un comentario
  de múltiples líneas.
  */
=======
    // This is a single-line comment.

    /*
    This is a
    multi-line comment.
    */
>>>>>>> 31b548577994397f7f849e8dd005d9d7aaa48c76

.. note::
  A single-line comment is terminated by any unicode line terminator
  (LF, VF, FF, CR, NEL, LS or PS) in UTF-8 encoding. The terminator is still part of
  the source code after the comment, so if it is not an ASCII symbol
  (these are NEL, LS and PS), it will lead to a parser error.

<<<<<<< HEAD
Adicionalmente, existe otro tipo de comentario llamado natspec, pero la documentación todavía no está escrita. Estos comentarios se escriben con tres barras ***oblícuas (``///``) o como un bloque de comentarios con doble asterísco (``/** ... */``) y se deben usar justo arriba de las declaraciones de función o instrucciones. Para documentar funciones, anotar condiciones para la verificación formal y para proporcionar un **texto de confirmación** al usuario cuando intenta invocar una función, se puede usar etiquetas del tipo `Doxygen <https://en.wikipedia.org/wiki/Doxygen>`_ dentro de estos comentarios.

En el siguiente ejemplo, documentamos el título del contrato, la explicación para los dos parametros de entrada y para los dos valores de retorno.

::

    pragma solidity ^0.4.0;

 /** @title Shape calculator.*/
 contract shapeCalculator{
     /**@dev Calculates a rectangle's surface and perimeter.
      * @param w Width of the rectangle.
      * @param h Height of the rectangle.
      * @return s The calculated surface.
      * @return p The calculated perimeter.
      */
     function rectangle(uint w, uint h) returns (uint s, uint p) {
         s = w * h;
         p = 2 * (w + h);
     }
 }
=======
Additionally, there is another type of comment called a NatSpec comment,
which is detailed in the :ref:`style guide<style_guide_natspec>`. They are written with a
triple slash (``///``) or a double asterisk block (``/** ... */``) and
they should be used directly above function declarations or statements.
>>>>>>> 31b548577994397f7f849e8dd005d9d7aaa48c76
