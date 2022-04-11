<<<<<<< HEAD
#######################
Ensamblador de Solidity
#######################

.. index:: ! assembly, ! asm, ! evmasm

Solidity define un lenguaje ensamblador que también puede ser usado sin Solidity. Este lenguaje ensamblador también se puede usar como "ensamblador inline" dentro del código fuente de Solidity. Vamos a comenzar explicando cómo usar el ensamblador inline y sus diferencias con el ensamblador independiente, y luego especificaremos el ensamblador propiamente dicho.

PENDIENTE DE HACER: Escribir sobre de qué manera el ámbito del ensamblador inline es un poco diferente y las complicaciones que aparecen cuando, por ejemplo, se usan funciones internas o librerías. Además, escribir sobre los símbolos definidos por el compilador.

Ensamblador inline
==================

Para tener un control más fino, especialmente para mejorar el lenguaje escribiendo librerías, es posible intercalar las declaraciones hechas en Solidity con el ensamblador inline en un lenguaje cercano al lenguaje de la máquina virtual. Debido a que el EVM (la máquina virtual de Ethereum) es un máquina de tipo pila (stack machine), suele ser difícil dirigirse a la posición correcta de la pila y proporcinar los argumentos a los opcodes en el punto correcto en la pila. El ensamblador inline de Solidity intenta facilitar esto, y otras complicaciones que ocurren cuando se escribe el ensamblador de forma manual, con las siguientes funcinalidades:

* opcodes de estilo funcional: ``mul(1, add(2, 3))`` en lugar de ``push1 3 push1 2 add push1 1 mul``
* variables de ensamblador local: ``let x := add(2, 3)  let y := mload(0x40)  x := add(x, y)``
* acceso a variables externos: ``function f(uint x) public { assembly { x := sub(x, 1) } }``
* etiquetas: ``let x := 10  repeat: x := sub(x, 1) jumpi(repeat, eq(x, 0))``
* bucles: ``for { let i := 0 } lt(i, x) { i := add(i, 1) } { y := mul(2, y) }``
* bloques if: ``if slt(x, 0) { x := sub(0, x) }``
* declaraciones de intercambio: ``switch x case 0 { y := mul(x, 2) } default { y := 0 }``
* llamadas a funciones: ``function f(x) -> y { switch x case 0 { y := 1 } default { y := mul(x, f(sub(x, 1))) }   }``

Ahora queremos decribir el lenguaje del ensamblador inline en detalles.

.. warning::
    El ensamblador inline es una forma de acceder a bajo nivel a la Máquina Virtual de Ethereum. Esto ignora varios elementos de seguridad de Solidity.
=======
.. _inline-assembly:

###############
Inline Assembly
###############

.. index:: ! assembly, ! asm, ! evmasm


You can interleave Solidity statements with inline assembly in a language close
to the one of the Ethereum virtual machine. This gives you more fine-grained control,
which is especially useful when you are enhancing the language by writing libraries.

The language used for inline assembly in Solidity is called :ref:`Yul <yul>`
and it is documented in its own section. This section will only cover
how the inline assembly code can interface with the surrounding Solidity code.


.. warning::
    Inline assembly is a way to access the Ethereum Virtual Machine
    at a low level. This bypasses several important safety
    features and checks of Solidity. You should only use it for
    tasks that need it, and only if you are confident with using it.


An inline assembly block is marked by ``assembly { ... }``, where the code inside
the curly braces is code in the :ref:`Yul <yul>` language.

The inline assembly code can access local Solidity variables as explained below.

Different inline assembly blocks share no namespace, i.e. it is not possible
to call a Yul function or access a Yul variable defined in a different inline assembly block.
>>>>>>> 9e92c7a466cb2139e555a225fd61b2357085a4a0

.. note::
    TODO: Escribir cómo las reglas del ámbito del ensamblador inline son un poco diferentes,
    y las complicaciones que surgen cuando, por ejemplo, se usan funciones internas o librerías.
    Escribir también sobre los símbolos definidos por el compilador.

Ejemplo
-------

<<<<<<< HEAD
El siguiente ejemplo proporciona el código de librería que permite acceder al código de otro contrato y cargarlo en una variable ``bytes``. Esto no es para nada factible con "Solidity puro". La idea es que se usen librerías de ensamblador para aumentar las capacidades del lenguaje en ese sentido.
=======
The following example provides library code to access the code of another contract and
load it into a ``bytes`` variable. This is possible with "plain Solidity" too, by using
``<address>.code``. But the point here is that reusable assembly libraries can enhance the
Solidity language without a compiler change.
>>>>>>> 9e92c7a466cb2139e555a225fd61b2357085a4a0

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.16 <0.9.0;

    library GetCode {
<<<<<<< HEAD
        function at(address _addr) public view returns (bytes o_code) {
            assembly {
                // recupera el tamaño del código - esto necesita ensamblador
                let size := extcodesize(_addr)
                // asigna (output byte array) - esto se podría hacer también sin ensamblador
                // usando o_code = new bytes(size)
                o_code := mload(0x40)
                // nuevo "fin de memoria" incluyendo el relleno (padding)
                mstore(0x40, add(o_code, and(add(add(size, 0x20), 0x1f), not(0x1f))))
                // almacenar el tamaño en memoria
                mstore(o_code, size)
                // recuperar de verdad el código - esto necesita ensamblador
                extcodecopy(_addr, add(o_code, 0x20), 0, size)
=======
        function at(address addr) public view returns (bytes memory code) {
            assembly {
                // retrieve the size of the code, this needs assembly
                let size := extcodesize(addr)
                // allocate output byte array - this could also be done without assembly
                // by using code = new bytes(size)
                code := mload(0x40)
                // new "memory end" including padding
                mstore(0x40, add(code, and(add(add(size, 0x20), 0x1f), not(0x1f))))
                // store length in memory
                mstore(code, size)
                // actually retrieve the code, this needs assembly
                extcodecopy(addr, add(code, 0x20), 0, size)
>>>>>>> 9e92c7a466cb2139e555a225fd61b2357085a4a0
            }
        }
    }

<<<<<<< HEAD
El ensamblador inline también es útil es los casos en los que el optimizador falla en producir un código eficiente. Tenga en cuenta que es mucho más difíciil escribir el ensamblador porque el compilador no realiza controles, por lo tanto use ensamblador solamente para cosas complejas y solo si sabe lo que está haciendo.
=======
Inline assembly is also beneficial in cases where the optimizer fails to produce
efficient code, for example:
>>>>>>> 9e92c7a466cb2139e555a225fd61b2357085a4a0

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.16 <0.9.0;

<<<<<<< HEAD
    pragma solidity ^0.4.16;

    library VectorSum {
        // Esta función es menos eficiente porque el optimizador falla en quitar los controles de límite en el acceso al array.
        function sumSolidity(uint[] _data) public view returns (uint o_sum) {
            for (uint i = 0; i < _data.length; ++i)
                o_sum += _data[i];
        }

        // Sabemos que solamente accedemos al array dentro de los límites, así que podemos evitar los controles.
        // Se tiene que añadir 0x20 a un array porque la primera posición contiene el tamaño del array.
        function sumAsm(uint[] _data) public view returns (uint o_sum) {
            for (uint i = 0; i < _data.length; ++i) {
                assembly {
                    o_sum := add(o_sum, mload(add(add(_data, 0x20), mul(i, 0x20))))
=======

    library VectorSum {
        // This function is less efficient because the optimizer currently fails to
        // remove the bounds checks in array access.
        function sumSolidity(uint[] memory data) public pure returns (uint sum) {
            for (uint i = 0; i < data.length; ++i)
                sum += data[i];
        }

        // We know that we only access the array in bounds, so we can avoid the check.
        // 0x20 needs to be added to an array because the first slot contains the
        // array length.
        function sumAsm(uint[] memory data) public pure returns (uint sum) {
            for (uint i = 0; i < data.length; ++i) {
                assembly {
                    sum := add(sum, mload(add(add(data, 0x20), mul(i, 0x20))))
                }
            }
        }

        // Same as above, but accomplish the entire code within inline assembly.
        function sumPureAsm(uint[] memory data) public pure returns (uint sum) {
            assembly {
                // Load the length (first 32 bytes)
                let len := mload(data)

                // Skip over the length field.
                //
                // Keep temporary variable so it can be incremented in place.
                //
                // NOTE: incrementing data would result in an unusable
                //       data variable after this assembly block
                let dataElementLocation := add(data, 0x20)

                // Iterate until the bound is not met.
                for
                    { let end := add(dataElementLocation, mul(len, 0x20)) }
                    lt(dataElementLocation, end)
                    { data := add(dataElementLocation, 0x20) }
                {
                    sum := add(sum, mload(dataElementLocation))
>>>>>>> 9e92c7a466cb2139e555a225fd61b2357085a4a0
                }
            }
        }

        // Igual que arriba, pero todo mediante ensamblador inline.
        function sumPureAsm(uint[] _data) public view returns (uint o_sum) {
            assembly {
               // Carga la longitud (primeros 32 bytes)
               let len := mload(_data)

               // Saltar el campo de longitud
               //
               // Mantener la variable temporal para poder ser incrementada.
               //
               // NOTE: imcrementar _data resultaría en una variable 
               //       _data inusable tras este bloque de ensamblador
               let data := add(_data, 0x20)

               // Iterar hasta que se alcance el límite.
               for
                   { let end := add(data, len) }
                   lt(data, end)
                   { data := add(data, 0x20) }
               {
                   o_sum := add(o_sum, mload(data))
               }
            }
        }
    }



<<<<<<< HEAD
Síntaxis
--------

El ensamblador analiza comentarios, literales e identificadores de igual manera que en Solidity, así que se puede usar los comentarios habituales: ``//`` y ``/* */``. El ensamblador inline está señalado por ``assembly { ... }`` y dentro de estos corchetes se pueden usar los siguentes elementos (véase las secciones más abajo para más detalles):

 - literales, es decir ``0x123``, ``42`` o ``"abc"`` (strings de hasta 32 carácteres)
 - opcodes (en "estilo instruccional"), p.ej. ``mload sload dup1 sstore``, véase más abajo para tener una lista
 - opcodes en estilo fucional, e.g. ``add(1, mlod(0))``
 - etiquetas, p.ej. ``name:``
 - declaraciones de variable, p.ej. ``let x := 7``, ``let x := add(y, 3)`` o ``let x`` (se asigna el valor inicial de vacío (0))
 - identificadores (etiquetas o variables de ensamblador local y externos si se usa como ensamblador inline), p.ej. ``jump(name)``, ``3 x add``
 - tareas (en "estilo instruccional"), e.g. ``3 =: x``
 - tareas en estilo fucional, p.ej. ``x := add(y, 3)``
 - bloques donde las variables locales están incluidas dentro, p.ej. ``{ let x := 3 { let y := add(x, 1) } }``
=======
Access to External Variables, Functions and Libraries
-----------------------------------------------------

You can access Solidity variables and other identifiers by using their name.
>>>>>>> 9e92c7a466cb2139e555a225fd61b2357085a4a0

Local variables of value type are directly usable in inline assembly.
They can both be read and assigned to.

<<<<<<< HEAD
Este documento no pretende ser una descripción exhaustiva de la máquina virtual de Ethereum, pero la lista siguiente puede servir de referencia para sus opcodes.

Si un opcode recibe un argumento (siempre desde lo más alto de la pila), se ponen entre paréntesis.
Note que el orden de los argumentos puede verse invertido en estilo no funcional (se explica más abajo).
Opcodes marcados con un ``-`` no empuja un elemento encima de la pila, los marcados con ``*`` son especiales, y todos los demás empujan exactamente un elemento encima de la pila.

En el ejemplo ``mem[a...b)``, se indica los bytes de memoria empezando en la posición ``a`` hasta la posición (excluida) ``b`` y en el ejemplo ``storage[p]``, se indica los índices de almacenamiento en la posicón ``p``.

Los opcodes ``pushi`` y ``jumpdest`` no se pueden usar directamente.

En la gramática, los opcodes se representan como identificadores predefinidos.

+-------------------------+------+-----------------------------------------------------------------+
| stop                    + `-`  | parar ejecución, idéntico a return(0,0)                         |
+-------------------------+------+-----------------------------------------------------------------+
| add(x, y)               |      | x + y                                                           |
+-------------------------+------+-----------------------------------------------------------------+
| sub(x, y)               |      | x - y                                                           |
+-------------------------+------+-----------------------------------------------------------------+
| mul(x, y)               |      | x * y                                                           |
+-------------------------+------+-----------------------------------------------------------------+
| div(x, y)               |      | x / y                                                           |
+-------------------------+------+-----------------------------------------------------------------+
| sdiv(x, y)              |      | x / y, para números con signo en complemento de dos             |
+-------------------------+------+-----------------------------------------------------------------+
| mod(x, y)               |      | x % y                                                           |
+-------------------------+------+-----------------------------------------------------------------+
| smod(x, y)              |      | x % y, para números con signo en complemento de dos             |
+-------------------------+------+-----------------------------------------------------------------+
| exp(x, y)               |      | x elevado a y                                                   |
+-------------------------+------+-----------------------------------------------------------------+
| not(x)                  |      | ~x, cada bit de x está negado                                   |
+-------------------------+------+-----------------------------------------------------------------+
| lt(x, y)                |      | 1 si x < y, 0 de lo contrario                                   |
+-------------------------+------+-----------------------------------------------------------------+
| gt(x, y)                |      | 1 si x > y, 0 de lo contrario                                   |
+-------------------------+------+-----------------------------------------------------------------+
| slt(x, y)               |      | 1 si x < y, 0 de lo contrario, para números con signo en        |
|                         |      | complemento de dos                                              |
+-------------------------+------+-----------------------------------------------------------------+
| sgt(x, y)               |      | 1 si x > y, 0 de lo contrario, para números con signo en        |
|                         |      | complemento de dos                                              |
+-------------------------+------+-----------------------------------------------------------------+
| eq(x, y)                |      | 1 si x == y, 0 de lo contrario                                  |
+-------------------------+------+-----------------------------------------------------------------+
| iszero(x)               |      | 1 si x == 0, 0 de lo contrario                                  |
+-------------------------+------+-----------------------------------------------------------------+
| and(x, y)               |      | bitwise and de x e y                                            |
+-------------------------+------+-----------------------------------------------------------------+
| or(x, y)                |      | bitwise or of x and y                                           |
+-------------------------+------+-----------------------------------------------------------------+
| xor(x, y)               |      | bitwise xor of x and y                                          |
+-------------------------+------+-----------------------------------------------------------------+
| byte(n, x)              |      | n byte de x, donde el más significante byte es el byte 0        |
+-------------------------+------+-----------------------------------------------------------------+
| addmod(x, y, m)         |      | (x + y) % m con una precisión aritmética arbitraria             |
+-------------------------+------+-----------------------------------------------------------------+
| mulmod(x, y, m)         |      | (x * y) % m con una precisión aritmética arbitraria             |
+-------------------------+------+-----------------------------------------------------------------+
| signextend(i, x)        |      | el signo se extiende desde el bit (i*8+7) contando desde el     |
|                         |      | menos significante                                              |
+-------------------------+------+-----------------------------------------------------------------+
| keccak256(p, n)         |      | keccak(mem[p...(p+n)))                                          |
+-------------------------+------+-----------------------------------------------------------------+
| sha3(p, n)              |      | keccak(mem[p...(p+n)))                                          |
+-------------------------+------+-----------------------------------------------------------------+
| jump(label)             | `-`  | saltar a la posición de label / código                          |
+-------------------------+------+-----------------------------------------------------------------+
| jumpi(label, cond)      | `-`  | saltar a label si cond no es cero                               |
+-------------------------+------+-----------------------------------------------------------------+
| pc                      |      | posición actual en el código                                    |
+-------------------------+------+-----------------------------------------------------------------+
| pop(x)                  | `-`  | quitar el elemento empujado por x                               |
+-------------------------+------+-----------------------------------------------------------------+
| dup1 ... dup16          |      | copiar posición i de la pila en la posición de arriba           |
|                         |      | (contando desde arriba)                                         |
+-------------------------+------+-----------------------------------------------------------------+
| swap1 ... swap16        | `*`  | intercambiar la posición la más arriba con la posición i de     |
|                         |      | la pila justo debajo                                            |
+-------------------------+------+-----------------------------------------------------------------+
| mload(p)                |      | mem[p..(p+32))                                                  |
+-------------------------+------+-----------------------------------------------------------------+
| mstore(p, v)            | `-`  | mem[p..(p+32)) := v                                             |
+-------------------------+------+-----------------------------------------------------------------+
| mstore8(p, v)           | `-`  | mem[p] := v & 0xff    - sólo modifica un único byte             |
+-------------------------+------+-----------------------------------------------------------------+
| sload(p)                |      | storage[p]                                                      |
+-------------------------+------+-----------------------------------------------------------------+
| sstore(p, v)            | `-`  | storage[p] := v                                                 |
+-------------------------+------+-----------------------------------------------------------------+
| msize                   |      | tamaño de la memoria , es decir el índice más grande de         |
|                         |      | la memoria que ha sido accedida                                 |
+-------------------------+------+-----------------------------------------------------------------+
| gas                     |      | el gas todavía disponible para ejecución                        |
+-------------------------+------+-----------------------------------------------------------------+
| address                 |      | dirección del contrato actual / contexto de ejecución           |
+-------------------------+------+-----------------------------------------------------------------+
| balance(a)              |      | balance en wei de la dirección a                                |
+-------------------------+------+-----------------------------------------------------------------+
| caller                  |      | llamar el remitente (excluyendo delegatecall)                   |
+-------------------------+------+-----------------------------------------------------------------+
| callvalue               |      | wei que se enviaron junto con la llamada actual                 |
+-------------------------+------+-----------------------------------------------------------------+
| calldataload(p)         |      | llamar datos empezando por la posición p (32 bytes)             |
+-------------------------+------+-----------------------------------------------------------------+
| calldatasize            |      | tamaño de la llamada a datos en bytes                           |
+-------------------------+------+-----------------------------------------------------------------+
| calldatacopy(t, f, s)   | `-`  | copiar s bytes de la llamada a datos en la posición f a         |
|                         |      | la memoria en la posición t                                     |
+-------------------------+------+-----------------------------------------------------------------+
| codesize                |      | tamaño del código de contrato actual / contexto de ejecución    |
+-------------------------+------+-----------------------------------------------------------------+
| codecopy(t, f, s)       | `-`  | copiar s bytes del código en la posición f a la memoria         |
|                         |      | en la posición t                                                |
+-------------------------+------+-----------------------------------------------------------------+
| extcodesize(a)          |      | tamaño del código en la dirección a                             |
+-------------------------+------+-----------------------------------------------------------------+
| extcodecopy(a, t, f, s) | `-`  | igual que codecopy(t, f, s) pero tomando el código de           |
|                         |      | la dirección a                                                  |
+-------------------------+------+-----------------------------------------------------------------+
| returndatasize          |      | tamaño del último returndata                                    |
+-------------------------+------+-----------------------------------------------------------------+
| returndatacopy(t, f, s) | `-`  | copiar s bytes de returndata de la posición f a la memoria      |
|                         |      | en la posición t                                                |
+-------------------------+------+-----------------------------------------------------------------+
| create(v, p, s)         |      | crear un nuevo contrato con el código mem[p..(p+s))             |
|                         |      | y mandar v wei y devolver la nueva dirección                    |
+-------------------------+------+-----------------------------------------------------------------+
| create2(v, n, p, s)     |      | crear un nuevo contrato con el código mem[p..(p+s)) en la       |
|                         |      | dirección keccak256(<address> . n . keccak256(mem[p..(p+s)))    |
|                         |      | y mandar v wei y devolver la nueva dirección                    |
+-------------------------+------+-----------------------------------------------------------------+
| call(g, a, v, in,       |      | llamar el contrato a la dirección a con la entrada              |
| insize, out, outsize)   |      | mem[in..(in+insize)) proporcionando g gas y v wei y el campo    |
|                         |      | de salida mem[out..(out+outsize)) devolviendo 0 si hay un error |
|                         |      | (por ejemplo si se queda sin gas) y 1 si es un éxito            |
+-------------------------+------+-----------------------------------------------------------------+
| callcode(g, a, v, in,   |      | indéntico a `call` pero usando solo el código de a y si no,     |
| insize, out, outsize)   |      | quedarse en el contexto del contrato actual                     |
+-------------------------+------+-----------------------------------------------------------------+
| delegatecall(g, a, in,  |      | indéntico a `callcode` pero mantener también ``caller``         |
| insize, out, outsize)   |      | y ``callvalue``                                                 |
+-------------------------+------+-----------------------------------------------------------------+
| staticcall(g, a, in,    |      | idéntico a `call(g, a, 0, in, insize, out, outsize)` pero       |
| insize, out, outsize)   |      | no admite modificaciones de estado                              |
+-------------------------+------+-----------------------------------------------------------------+
| return(p, s)            | `-`  | termina la ejecución, devuelve los datos de mem[p..(p+s))       |
+-------------------------+------+-----------------------------------------------------------------+
| revert(p, s)            | `-`  | termina la ejecución, revierte los cambios de estado, devuelve  |
|                         |      | los datos de mem[p..(p+s))                                      |
+-------------------------+------+-----------------------------------------------------------------+
| selfdestruct(a)         | `-`  | termina la ejecución, destruye el contrato actual y manda los   |
|                         |      | fondos a a                                                      |
+-------------------------+------+-----------------------------------------------------------------+
| invalid                 | `-`  | termina la ejecución con una instrucción no válida              |
+-------------------------+------+-----------------------------------------------------------------+
| log0(p, s)              | `-`  | log sin tópicos y datos mem[p..(p+s))                           |
+-------------------------+------+-----------------------------------------------------------------+
| log1(p, s, t1)          | `-`  | log sin tópicos t1 y datos mem[p..(p+s))                        |
+-------------------------+------+-----------------------------------------------------------------+
| log2(p, s, t1, t2)      | `-`  | log sin tópicos t1, t2 y datos mem[p..(p+s))                    |
+-------------------------+------+-----------------------------------------------------------------+
| log3(p, s, t1, t2, t3)  | `-`  | log sin tópicos t1, t2, t3 y datos mem[p..(p+s))                |
+-------------------------+------+-----------------------------------------------------------------+
| log4(p, s, t1, t2, t3,  | `-`  | log sin tópicos t1, t2, t3, t4 y datos mem[p..(p+s))            |
| t4)                     |      |                                                                 |
+-------------------------+------+-----------------------------------------------------------------+
| origin                  |      | remitente de la transacción                                     |
+-------------------------+------+-----------------------------------------------------------------+
| gasprice                |      | precio del gas price de la transacción                          |
+-------------------------+------+-----------------------------------------------------------------+
| blockhash(b)            |      | hash del bloque número b - solamente para los últimos           |
|                         |      | 256 bloques, exluyendo al bloque actual                         |
+-------------------------+------+-----------------------------------------------------------------+
| coinbase                |      | el beneficiario actual del minado                               |
+-------------------------+------+-----------------------------------------------------------------+
| timestamp               |      | timestamp en segundos del bloque actual desde epoch             |
+-------------------------+------+-----------------------------------------------------------------+
| number                  |      | número del bloque actual                                        |
+-------------------------+------+-----------------------------------------------------------------+
| difficulty              |      | dificultad del bloque actual                                    |
+-------------------------+------+-----------------------------------------------------------------+
| gaslimit                |      | límite de gas del bloque para el bloque actual                  |
+-------------------------+------+-----------------------------------------------------------------+

Literales
---------

Se pueden usar constantes íntegras usando la notación decimal o hexadecimal y con eso, una instrucción apropiada ``PUSHi`` sereá automáticamente generada. El siguiente código suma 2 con 3, lo que resulta en 5 y luego computa el bitwise con el string "abc". Los strings están almacenados alineados a la izquierda y no pueden ser más largos que 32 bytes.

.. code::

    assembly { 2 3 add "abc" and }

Estilo funcional
----------------

Se puede entrar opcodes uno trás el otro de la misma manera que van aparecer en el bytecode. Por ejemplo sumar ''3'' al contenido de la memoria en la posición ``0x80`` sería:

.. code::

    3 0x80 mload add 0x80 mstore

Como suele ser complicado ver cuales son los argumentos actuales para algunos de los opcodes, el ensamblador inline de Solidity proporciona también una notación de "estilo fucinonal" donde el mismo código se escribiría de la siguiente manera:

.. code::

    mstore(0x80, add(mload(0x80), 3))

Expresiones en estilo funcional no pueden hacer uso internamente del estilo instruccional, es decir que ``1 2 mstore(0x80, add)`` no es ensamblador válido, debería escribirse como ``mstore(0x80, add(2, 1))``. Para los opcodes que no toman argumentos, las parentésis pueden obviarse.

Nótese que el orden de los argumentos está invertido en estilo funcional con respecto al estilo instruccional. Si hace uso del estilo funcional, el primer argumento aparecerá arriba de la pila.


Acceso a variables y funciones internas
---------------------------------------

Se accede a las variables y otros identificadores de Solidity simplemente por su nombre. Para las variables de memoria, esto tiene como consecuencia que es la dirección y no el nombre que se empuja en la pila. Con las variables de almacenamiento es diferente: los valores en almacenamiento podrían no ocupar una posición completa en la pila, de tal manera que su dirección estaría compuesta por una posición y un decalage en bytes dentro de esta posición. Para recuperar la posición a la que apunta la variable ``x``, se usa ``x_slot`` y para recuperar el decalage en bytes se usa ``x_offset``.

En las asignaciones (ver abajo), hasta se pueden usar las variables locales de Solidity y asignarlas.

También se puede acceder a las funciones externas al ensamblador inline: el ensamblador empujará su etiqueta de entrada (aplicando la resolución de funciones virtuales). Las semánticas de llamada en Solidity son las siguientes:

 - el que llama empuja return etiqueta, arg1, arg2, ..., argn
 - la llamada devuelve ret1, ret2, ..., retm

Esta funcionalidad está todavía un poco dificultosa de usar, esensialmente porque el decalage de pila cambia durante la llamada, y por lo tanto las referencias a las variables locales estarán mal.

.. code::

    pragma solidity ^0.4.11;

    contract C {
        uint b;
        function f(uint x) public returns (uint r) {
            assembly {
                r := mul(x, sload(b_slot)) // ignorar el decalage, sabemos que es cero
=======
Local variables that refer to memory evaluate to the address of the variable in memory not the value itself.
Such variables can also be assigned to, but note that an assignment will only change the pointer and not the data
and that it is your responsibility to respect Solidity's memory management.
See :ref:`Conventions in Solidity <conventions-in-solidity>`.

Similarly, local variables that refer to statically-sized calldata arrays or calldata structs
evaluate to the address of the variable in calldata, not the value itself.
The variable can also be assigned a new offset, but note that no validation to ensure that
the variable will not point beyond ``calldatasize()`` is performed.

For external function pointers the address and the function selector can be
accessed using ``x.address`` and ``x.selector``.
The selector consists of four right-aligned bytes.
Both values can be assigned to. For example:

.. code-block:: solidity
    :force:

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.8.10 <0.9.0;

    contract C {
        // Assigns a new selector and address to the return variable @fun
        function combineToFunctionPointer(address newAddress, uint newSelector) public pure returns (function() external fun) {
            assembly {
                fun.selector := newSelector
                fun.address  := newAddress
            }
        }
    }

For dynamic calldata arrays, you can access
their calldata offset (in bytes) and length (number of elements) using ``x.offset`` and ``x.length``.
Both expressions can also be assigned to, but as for the static case, no validation will be performed
to ensure that the resulting data area is within the bounds of ``calldatasize()``.

For local storage variables or state variables, a single Yul identifier
is not sufficient, since they do not necessarily occupy a single full storage slot.
Therefore, their "address" is composed of a slot and a byte-offset
inside that slot. To retrieve the slot pointed to by the variable ``x``, you
use ``x.slot``, and to retrieve the byte-offset you use ``x.offset``.
Using ``x`` itself will result in an error.

You can also assign to the ``.slot`` part of a local storage variable pointer.
For these (structs, arrays or mappings), the ``.offset`` part is always zero.
It is not possible to assign to the ``.slot`` or ``.offset`` part of a state variable,
though.

Local Solidity variables are available for assignments, for example:

.. code-block:: solidity
    :force:

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.7.0 <0.9.0;

    contract C {
        uint b;
        function f(uint x) public view returns (uint r) {
            assembly {
                // We ignore the storage slot offset, we know it is zero
                // in this special case.
                r := mul(x, sload(b.slot))
>>>>>>> 9e92c7a466cb2139e555a225fd61b2357085a4a0
            }
        }
    }

<<<<<<< HEAD
Etiquetas
---------

Otro de los problemas en el ensamblador del EVM es que ``jump`` y ``jumpi`` hacen uso de direcciones absolutas que pueden facilmente cambiar. El ensamblador inline de Solidity proporciona etiquetas para hacer el uso de saltos más cómodo. Nótese que las etiquetas son una funcionalidad de bajo nivel y que es perfectamente posible escribir un ensamblador eficicente sin etiquetas, usando solo funciones de ensamblador, bucles e instrucciones de intercambio (ver abajo). El siguiente código computa un elemento en una serie de Fibonacci.

.. code::

    {
        let n := calldataload(4)
        let a := 1
        let b := a
    loop:
        jumpi(loopend, eq(n, 0))
        a add swap1
        n := sub(n, 1)
        jump(loop)
    loopend:
        mstore(0, a)
        return(0, 0x20)
    }

Tenga en cuenta que acceder automáticamente a variables de pila sólo funciona si el ensamblador conoce la altura de la pila actual. Esto falla si el inicio y el destino del salto tienen alturas de pila distintas. Aún así se pueden usar estos saltos, pero no debería entonces acceder a ninguna variables de pila (incluso variables de ensamblador)

Además, el analizador de la altura de la pila lee el código opcode por opcode (y no acorde al control de flujo), por lo tanto y según indica al ejemplo que figura abajo, el ensamblador tendría una falsa idea de la altura de la pila al llegar a la etiqueta ``two``:

.. code::

    {
        let x := 8
        jump(two)
        one:
            // Aquí la altura de la pila es de 2 (porque empujamos x y 7), pero el ensamblador cree que la altura es de 1 porque lee desde arriba abajo.
            // Acceder a la variable de pila x aquí nos llevaría a un error.
            x := 9
            jump(three)
        two:
            7 // empujar algo arriba de la pila
            jump(one)
        three:
    }

Declarando variables de ensamblador local
-----------------------------------------

Puede usar la palabra clave ``let`` para declarar variables que están visibles solamente en ensamblador inline y en realidad solamente en el bloque actual ``{...}``. Lo que pasa es que la instrucción ``let`` crea una nueva posición en la pila que está reservada para la variable. Esta posición se quitará automaticamente cuando se llegue al final del bloque. Es necesario proporcionar un valor inicial para la variable, que puede ser simplemente ``0``, pero también puede ser una expresión compleja en el estilo funcional.

.. code::

    pragma solidity ^0.4.16;

    contract C {
        function f(uint x) public view returns (uint b) {
            assembly {
                let v := add(x, 1)
                mstore(0x80, v)
                {
                    let y := add(sload(v), 1)
                    b := y
                } // aquí se "desasigna" y
                b := add(b, v)
            } // aquí se "desasigna" v
        }
    }


Asignaciones
------------

Las asignaciones son posibles para las variables de ensamblador local y para las variables de función local. Tenga cuidado que cuando usted asigna a variables que apuntan a la memoria o al almacenamiento, usted sólo cambiará lo que apunta pero no los datos.

Existen asignaciones de dos tipos: las de estilo funcional y las de estilo instruccional. Para las asignaciones de estilo funcional, (``variable := value``), se requiere proporcionarun valor dentro de una expresión de estilo funcional que resulta en exactamente un valor de pila. Para las asignaciones de estilo instruccional (``=: variable``), el valor simplemente se toma desde arriba de la pila. Para ambas maneras, la coma apunta al nombre de la variable. La asignación se ejecuta remplazando el valor de la variable en la pila por el valor nuevo.

.. code::

    {
        let v := 0 // asignación de estilo funcional como parte de la declaración de variable
        let g := add(v, 2)
        sload(10)
        =: v // asignación de estilo instruccional, pone el resultado de sload(10) en v
    }

If
--

La declaración if se puede usar para ejecutar código de forma condicionada.
No hay parte "else", considera usar "switch" (ver abajo) si
necesitas múltiples alternativas.

.. code::

    {
        if eq(value, 0) { revert(0, 0) }
    }

The curly braces for the body are required.


Switch
------

Se puede usar una declaración de switch como una versión muy básica de un "if/else". Toma el valor de una expresión y lo compara con distintas constantes. Se elige la rama correspondiente a la constante que combina. A contrario de algunos de los lenguages de programación que son propensos a errores de comportamiento, el flujo de control, después de un caso, no pasa al siguiente. Puede haber un fallback o un caso por defecto llamado ``default``.

.. code::

    {
        let x := 0
        switch calldataload(4)
        case 0 {
            x := calldataload(0x24)
        }
        default {
            x := calldataload(0x44)
        }
        sstore(0, div(x, 2))
    }

Una lista de casos no necesita llaves, pero el cuerpo de un caso si.

Bucles
------

El ensamblador soporta un bucle simple de tipo for. Los bucles de tipo for contienen un encabezado con la inicialización, una condición y una parte post interación. La condición debe ser una expresión de estilo funcional, mientras que las otras dos partes son bloques. Si se declaran variables en la parte de inicialización, el alcance de estas variables se extenderá hasta el cuerpo (incluyendo la condición y la parte post iteración).

El ejemplo siguiente computa la suma de un área en la memoria.

.. code::

    {
        let x := 0
        for { let i := 0 } lt(i, 0x100) { i := add(i, 0x20) } {
            x := add(x, mload(i))
        }
    }

Los bucles for también se pueden escribir de modo que se comporten como bucles while:
Simplemente deja vacías las partes de inicialización y post-iteración.

.. code::

    {
        let x := 0
        let i := 0
        for { } lt(i, 0x100) { } {     // while(i < 0x100)
            x := add(x, mload(i))
            i := add(i, 0x20)
        }
    }


Funciones
---------

El ensamblador permite la definición de funciones de bajo nivel. Éstas toman sus argumentos (y un return PC) desde la pila y también ponen los resultados arriba de la pila. Llamar una función se parece a la ejecución de un opcode de estilo funcional.

Las funciones pueden definirse en cualquier lugar y son visibles en el bloque en el que se han declarado. Dentro de una función, no se permite acceder a variables locales definidas fuera de esta función. No existe una declaración ``return`` explícita.

Si se llama una función que devuelve múltiples valores, es obligatorio asignarlos a un tuple usando ``a, b := f(x)`` o ``let a, b := f(x)``.

El siguiente ejemplo implementa la función de potencia con cuadrados y multiplicaciones.

.. code::

    {
        function power(base, exponent) -> result {
            switch exponent
            case 0 { result := 1 }
            case 1 { result := base }
            default {
                result := power(mul(base, base), div(exponent, 2))
                switch mod(exponent, 2)
                    case 1 { result := mul(base, result) }
            }
        }
    }
=======
.. warning::
    If you access variables of a type that spans less than 256 bits
    (for example ``uint64``, ``address``, or ``bytes16``),
    you cannot make any assumptions about bits not part of the
    encoding of the type. Especially, do not assume them to be zero.
    To be safe, always clear the data properly before you use it
    in a context where this is important:
    ``uint32 x = f(); assembly { x := and(x, 0xffffffff) /* now use x */ }``
    To clean signed types, you can use the ``signextend`` opcode:
    ``assembly { signextend(<num_bytes_of_x_minus_one>, x) }``


Since Solidity 0.6.0 the name of a inline assembly variable may not
shadow any declaration visible in the scope of the inline assembly block
(including variable, contract and function declarations).

Since Solidity 0.7.0, variables and functions declared inside the
inline assembly block may not contain ``.``, but using ``.`` is
valid to access Solidity variables from outside the inline assembly block.
>>>>>>> 9e92c7a466cb2139e555a225fd61b2357085a4a0

Cosas a evitar
--------------

<<<<<<< HEAD
Aunque el ensamblador inline puede dar la sensación de tener un aspecto de alto nivel, es en realidad de nivel extremadamente bajo. Se convierten las llamadas a funciones, los bucles, los ifs y los switches con simples reglas de reescritura y luego, lo único que el ensamblador hace para el usuario es reorganizar los opcodes en estilo funcional, manejando etiquetas de salto, contando la altura de la pila para el acceso a variables y quitando posiciones en la pila para variables locales del ensamblador cuando se alcanza el final de su bloque. Especialmente en estos dos últimos casos, es importante saber que el ensamblador solo cuenta la altura de la pila desde arriba abajo, y no necesariamente siguiendo el flujo de control. Además, las operaciones como los intercambios solo van a intercambiar los contenidos de la pila pero no la ubicación de las variables.
=======
Inline assembly might have a quite high-level look, but it actually is extremely
low-level. Function calls, loops, ifs and switches are converted by simple
rewriting rules and after that, the only thing the assembler does for you is re-arranging
functional-style opcodes, counting stack height for
variable access and removing stack slots for assembly-local variables when the end
of their block is reached.

.. _conventions-in-solidity:
>>>>>>> 9e92c7a466cb2139e555a225fd61b2357085a4a0

Convenciones en Solidity
------------------------

<<<<<<< HEAD
A cambio del ensamblador EVM, Solidity conoce tipos que son más estrechos que 256 bits, por ejemplo ``uint24``. Para hacerlos más eficientes, la mayoría de las operaciones aritméticas los tratan como números de 256 bits y los bits de mayor orden sólo se limpian cuando es necesario, es decir sólo un poco antes de almacenarse en memoria o antes de realizar comparaciones. Lo que significa que si se accede dicha variable desde dentro del ensamblador inline, puede que se tenga primero que limpiar los bits de mayor orden.

Solidity maneja la memoria de una manera muy simple: hay un "cursor de memoria disponible" en la posición ``0x40`` de la memoria. Si se desea asignar memoria, tan solo se tiene que usar la memoria a partir de este punto y actualizar el cursor en consecuencia.

En Solidity, los elementos en arrays de memoria siempre ocupan múltiples de 32 bytes (y si, esto también es cierto para ``byte[]``, pero no para ``bytes`` y ``string``). Arrays multidimensionales son cursores de arrays de memoria. La longitud de un array dinámico se almacena en la primera posición del array y luego sólo le siguen elementos del array.

.. warning::
    Arrays de memoria estáticos en tamaño no tienen un campo para la longitud, pero se añadirá pronto para permitir una mejor convertibilidad entre arrays de tamaño estático y dinñamico, pero es importante no contar con esto por ahora.
=======
.. _assembly-typed-variables:

Values of Typed Variables
=========================

In contrast to EVM assembly, Solidity has types which are narrower than 256 bits,
e.g. ``uint24``. For efficiency, most arithmetic operations ignore the fact that
types can be shorter than 256
bits, and the higher-order bits are cleaned when necessary,
i.e., shortly before they are written to memory or before comparisons are performed.
This means that if you access such a variable
from within inline assembly, you might have to manually clean the higher-order bits
first.

.. _assembly-memory-management:

Memory Management
=================

Solidity manages memory in the following way. There is a "free memory pointer"
at position ``0x40`` in memory. If you want to allocate memory, use the memory
starting from where this pointer points at and update it.
There is no guarantee that the memory has not been used before and thus
you cannot assume that its contents are zero bytes.
There is no built-in mechanism to release or free allocated memory.
Here is an assembly snippet you can use for allocating memory that follows the process outlined above

.. code-block:: yul

    function allocate(length) -> pos {
      pos := mload(0x40)
      mstore(0x40, add(pos, length))
    }

The first 64 bytes of memory can be used as "scratch space" for short-term
allocation. The 32 bytes after the free memory pointer (i.e., starting at ``0x60``)
are meant to be zero permanently and is used as the initial value for
empty dynamic memory arrays.
This means that the allocatable memory starts at ``0x80``, which is the initial value
of the free memory pointer.

Elements in memory arrays in Solidity always occupy multiples of 32 bytes (this is
even true for ``bytes1[]``, but not for ``bytes`` and ``string``). Multi-dimensional memory
arrays are pointers to memory arrays. The length of a dynamic array is stored at the
first slot of the array and followed by the array elements.

.. warning::
    Statically-sized memory arrays do not have a length field, but it might be added later
    to allow better convertibility between statically- and dynamically-sized arrays, so
    do not rely on this.
>>>>>>> 9e92c7a466cb2139e555a225fd61b2357085a4a0

Memory Safety
=============

<<<<<<< HEAD
Ensamblador independiente
=========================

El lenguage ensamblador que hemos descrito más arriba como ensamblador inline también puede usarse de forma independiente. De hecho, el plan es de usarlo como un lenguage intermedio para el compilador de Solidity. De esta forma, intenta cumplir con varios objetivos:

1. Los programas escritos en el lenguage ensamblador deben de ser legibles, aunque el código sea generado por un compilador de Solidity.
2. La traducción del lenguage ensamblador al bytecode deben de contener el número de sorpresas el más reducido posible.
3. El control de flujo debe de ser fácil de detectar para ayudar a la verificación formal y a la optimización.

Para cumplir con el primero y el útlimo de los objetivos, el ensamblador proporciona constructs de alto nivel como bucles ``for``, ``if``, declaraciones ``switch`` y llamadas a funciones. Debería de ser posible de escribir programas de ensamblador que no hacen uso de declaraciones explícitas de tipo ``SWAP``, ``DUP``, ``JUMP`` y ``JUMPI``, porque las dos primeras declaraciones ofuscan el flujo de datos y las dos últimas ofuscan el control de flujo. Además, hay que privilegiar declaraciones funcionales del tipo ``mul(add(x, y), 7)`` a las declaraciones de opcodes puras como ``7 y x add mul`` porque en la primera, es mucho más fácil ver qué operando se usa para qué opcode.

El segundo objetivo se cumple introduciendo una fase de desazucarización que sólo quita los constructs de más alto nivel de una forma muy regular pero permitiendo todavía la inspección el código ensamblador de bajo nivel generado. La única operación no local realizada por el ensamblador es la búsqueda de nombre de identificadores (funciones, variables, ...) definidos por el usuario, lo que se hace siguiendo reglas con un alcance muy simple y regular y con un proceso de limpieza de variables locales desde la pila.

Alcance: Un identificador que está declarado (etiqueta, variable, función, ensamblador) sólo es visible en el bloque donde ha sido declarado (incluyendo bloques anidados dentro del bloque actual). No es legal acceder variables locales cruzando los límites de la función, aunque estas variables estuvieran dentro del alcance. Ocultar no está permitido. No se puede acceder variables locales antes de que estén declaradas, pero está permitido acceder etiquetas, funciones y ensambladores sin que lo estén. Ensambladores son bloques especiales que se usan para, por ejemplo, devolver el tiempo de ejecución del código o crear contratos. Identificadores externos a un ensamblador no son visibles en un sub ensamblador.

Si el flujo de control va más allá del final de un bloque, se insertan instrucciones pop que corresponden al número de variables locales declaradas en este bloque. Cuando se referencia una variables local, el generador de código necesita saber su posición relativa actual en la pila y por lo tanto necesita hacer un seguimiento de la así llamada altura actual de la pila. Como se quitan todas las variables locales al final de un bloque, la altura de la pila antes y después de un bloque debería ser la misma. Si esto no fuera el caso, se emite un aviso.

¿Por qué usamos constructs de alto nivel como ``switch``, ``for`` y funciones:

Usando ``switch``, ``for`` y funciones, debería ser posible escribir códigos complejos sin usar ``jump`` o ``jumpi`` manualmente. Esto simplifica mucho el análisis del control de flujo, lo que permite hacer mejor la verificación formal y la optimización.

Además, si se permiten los saltos manuales, computar la altura de la pila se hace bastante complicado. Se necesita saber la posición de todas las variables locales de la pila, de lo contrario ni las referencias a las variables locales ni quitar automáticamente variables locales de la pila al final de un bloque funcionará correctamente. El mecanismo de desazucarado inserta operaciones correctamente en bloques inalcanzables que ajustan correctamente la altura de la pila en el caso de tener saltos que no tengan un control de flujo en marcha

Ejemplo:

Vamos a seguir un ejemplo de compilación de Solidity a ensamblador desazucarado.
Consideramos el tiempo de ejecución del bytecode del siguiente programa escrito en Solidity::

    pragma solidity ^0.4.16;
    
    contract C {
      function f(uint x) public pure returns (uint y) {
        y = 1;
        for (uint i = 0; i < x; i++)
          y = 2 * y;
      }
    }

Se va a generar el siguiente ensamblador::

    {
      mstore(0x40, 0x60) // almacenar el "puntero de memoria libre"
      // función dispatcher
      switch div(calldataload(0), exp(2, 226))
      case 0xb3de648b {
        let (r) = f(calldataload(4))
        let ret := $allocate(0x20)
        mstore(ret, r)
        return(ret, 0x20)
      }
      default { revert(0, 0) }
      // repartidor de memoria
      function $allocate(size) -> pos {
        pos := mload(0x40)
        mstore(0x40, add(pos, size))
      }
      // la función del contrato
      function f(x) -> y {
        y := 1
        for { let i := 0 } lt(i, x) { i := add(i, 1) } {
          y := mul(2, y)
        }
      }
    }

Después de la fase de desazucarado, se parece a lo siguiente::

    {
      mstore(0x40, 0x60)
      {
        let $0 := div(calldataload(0), exp(2, 226))
        jumpi($case1, eq($0, 0xb3de648b))
        jump($caseDefault)
        $case1:
        {
          // la llamada de función – ponemos la etiqueta return y los argumentos encima de la pila
          $ret1 calldataload(4) jump(f)
          // Esto es código inalcanzable. Se añaden opcodes que reproducen el efecto de la función sobre la altura de la pila: se quitan argumentos y se introducen valores devueltos.
          pop pop
          let r := 0
          $ret1: // el punto de retorno actual
          $ret2 0x20 jump($allocate)
          pop pop let ret := 0
          $ret2:
          mstore(ret, r)
          return(ret, 0x20)
          // aunque no sirva de nada, se inserta el salto automáticamente, ya que el proceso de desazucarado es una operación puramente sintáctica que no analiza el control de flujo.
          jump($endswitch)
        }
        $caseDefault:
        {
          revert(0, 0)
          jump($endswitch)
        }
        $endswitch:
      }
      jump($afterFunction)
      allocate:
      {
        // nos saltamos el código inalcanzable que introduce los argumentos de la función
        jump($start)
        let $retpos := 0 let size := 0
        $start:
        // las variables de salida están dentro del mismo alcance que los argumentos y ahora se reparten
        let pos := 0
        {
          pos := mload(0x40)
          mstore(0x40, add(pos, size))
        }
        // Este código remplaza los argumentos por los valores de retorno y los saltos hacía atrás.
        swap1 pop swap1 jump
        // Esto es, de nuevo, código inalcanzable que corrige la altura de la pila.
        0 0
      }
      f:
      {
        jump($start)
        let $retpos := 0 let x := 0
        $start:
        let y := 0
        {
          let i := 0
          $for_begin:
          jumpi($for_end, iszero(lt(i, x)))
          {
            y := mul(2, y)
          }
          $for_continue:
          { i := add(i, 1) }
          jump($for_begin)
          $for_end:
        } // Aquí, se inserta una instrucción pop para i
        swap1 pop swap1 jump
        0 0
      }
      $afterFunction:
      stop
=======
Without the use of inline assembly, the compiler can rely on memory to remain in a well-defined
state at all times. This is especially relevant for :ref:`the new code generation pipeline via Yul IR <ir-breaking-changes>`:
this code generation path can move local variables from stack to memory to avoid stack-too-deep errors and
perform additional memory optimizations, if it can rely on certain assumptions about memory use.

While we recommend to always respect Solidity's memory model, inline assembly allows you to use memory
in an incompatible way. Therefore, moving stack variables to memory and additional memory optimizations are,
by default, disabled in the presence of any inline assembly block that contains a memory operation or assigns
to solidity variables in memory.

However, you can specifically annotate an assembly block to indicate that it in fact respects Solidity's memory
model as follows:

.. code-block:: solidity

    assembly ("memory-safe") {
        ...
    }

In particular, a memory-safe assembly block may only access the following memory ranges:

- Memory allocated by yourself using a mechanism like the ``allocate`` function described above.
- Memory allocated by Solidity, e.g. memory within the bounds of a memory array you reference.
- The scratch space between memory offset 0 and 64 mentioned above.
- Temporary memory that is located *after* the value of the free memory pointer at the beginning of the assembly block,
  i.e. memory that is "allocated" at the free memory pointer without updating the free memory pointer.

Furthermore, if the assembly block assigns to Solidity variables in memory, you need to assure that accesses to
the Solidity variables only access these memory ranges.

Since this is mainly about the optimizer, these restrictions still need to be followed, even if the assembly block
reverts or terminates. As an example, the following assembly snippet is not memory safe, because the value of
``returndatasize()`` may exceed the 64 byte scratch space:

.. code-block:: solidity

    assembly {
      returndatacopy(0, 0, returndatasize())
      revert(0, returndatasize())
    }

On the other hand, the following code *is* memory safe, because memory beyond the location pointed to by the
free memory pointer can safely be used as temporary scratch space:

.. code-block:: solidity

    assembly ("memory-safe") {
      let p := mload(0x40)
      returndatacopy(p, 0, returndatasize())
      revert(p, returndatasize())
>>>>>>> 9e92c7a466cb2139e555a225fd61b2357085a4a0
    }

Note that you do not need to update the free memory pointer if there is no following allocation,
but you can only use memory starting from the current offset given by the free memory pointer.

<<<<<<< HEAD
El ensamblador sucede en cuatro etapas:

1. Análisis sintático (o parsing en inglés)
2. Desazucarización (quita switch, for y funciones)
3. Generación de la transmisión de opcodes
4. Generación del bytecode

Vamos a especificar las etapas uno a tres de una forma pseudo formal. Especificaciones más formales se darán luego.


Análisis sintático / Gramática
------------------------------

Éstas son las tareas del módulo de análisis (o del parser en inglés):

- Convertir la transmisión de byte en una transmisión de token, desechando comentarios de tipo C++ (existe un comentario especial para las referencias fuente, pero no lo vamos a explicar aquí).
- Convertir la transmisión de token en un AST según la gramática que figura más abajo.
- Registrar identificadores con el bloque en el que están definidos (anotación al nodo AST) y anotar sobre el punto a partir del cual se puede acceder a las variables.

El ensamblador lexer sigue el ensamblador definido por Solidity.

El espacio en blanco se usa para delimitar tokens y consiste en los caracteres Espacio, Tabular y Línea de alimentación. Los comentarios aceptados son comentarios típicos de JavaScript/C++ y se interpretan de la misma manera que el Espacio.

Gramática::

    AssemblyBlock = '{' AssemblyItem* '}'
    AssemblyItem =
        Identifier |
        AssemblyBlock |
        AssemblyExpression |
        AssemblyLocalDefinition |
        AssemblyAssignment |
        AssemblyStackAssignment |
        LabelDefinition |
        AssemblyIf |
        AssemblySwitch |
        AssemblyFunctionDefinition |
        AssemblyFor |
        'break' |
        'continue' |
        SubAssembly
    AssemblyExpression = AssemblyCall | Identifier | AssemblyLiteral
    AssemblyLiteral = NumberLiteral | StringLiteral | HexLiteral
    Identifier = [a-zA-Z_$] [a-zA-Z_0-9]*
    AssemblyCall = Identifier '(' ( AssemblyExpression ( ',' AssemblyExpression )* )? ')'
    AssemblyLocalDefinition = 'let' IdentifierOrList ( ':=' AssemblyExpression )?
    AssemblyAssignment = IdentifierOrList ':=' AssemblyExpression
    IdentifierOrList = Identifier | '(' IdentifierList ')'
    IdentifierList = Identifier ( ',' Identifier)*
    AssemblyStackAssignment = '=:' Identifier
    LabelDefinition = Identifier ':'
    AssemblyIf = 'if' AssemblyExpression AssemblyBlock
    AssemblySwitch = 'switch' AssemblyExpression AssemblyCase*
        ( 'default' AssemblyBlock )?
    AssemblyCase = 'case' AssemblyExpression AssemblyBlock
    AssemblyFunctionDefinition = 'function' Identifier '(' IdentifierList? ')'
        ( '->' '(' IdentifierList ')' )? AssemblyBlock
    AssemblyFor = 'for' ( AssemblyBlock | AssemblyExpression )
        AssemblyExpression ( AssemblyBlock | AssemblyExpression ) AssemblyBlock
    SubAssembly = 'assembly' Identifier AssemblyBlock
    NumberLiteral = HexNumber | DecimalNumber
    HexLiteral = 'hex' ('"' ([0-9a-fA-F]{2})* '"' | '\'' ([0-9a-fA-F]{2})* '\'')
    StringLiteral = '"' ([^"\r\n\\] | '\\' .)* '"'
    HexNumber = '0x' [0-9a-fA-F]+
    DecimalNumber = [0-9]+


Desazucarización
----------------

Una transformación AST quita los for, switch y funciones constructs. El resultados aún es pasible de ser analizado desde el punto de vista sintáctico por el mismo analizador, pero no usará algunos de los constructs. Si se añaden saltos a los que sólo se salta y desde los que luego no se continúa, se añade información sobre el contenido de la pila, a no ser que no se acceda a ninguna variable local de alcance externo o que la altura de la pila sea la misma que para la instrucción anterior.

Pseudo código::

    desugar item: AST -> AST =
    match item {
    AssemblyFunctionDefinition('function' name '(' arg1, ..., argn ')' '->' ( '(' ret1, ..., retm ')' body) ->
      <name>:
      {
        jump($<name>_start)
        let $retPC := 0 let argn := 0 ... let arg1 := 0
        $<name>_start:
        let ret1 := 0 ... let retm := 0
        { desugar(body) }
        swap and pop items so that only ret1, ... retm, $retPC are left on the stack
        jump
        0 (1 + n times) to compensate removal of arg1, ..., argn and $retPC
      }
    AssemblyFor('for' { init } condition post body) ->
      {
        init // no puede ser su propio bloque porque queremos que el alcance de la variable se extienda hasta el cuerpo
        // encuentra I de tal manera que no haya etiquetas $forI_*
        $forI_begin:
        jumpi($forI_end, iszero(condition))
        { body }
        $forI_continue:
        { post }
        jump($forI_begin)
        $forI_end:
      }
    'break' ->
      {
        // encuentra el alcance que encierra más cercano con la etiqueta $forI_end
        pop all local variables that are defined at the current point
        but not at $forI_end
        jump($forI_end)
        0 (as many as variables were removed above)
      }
    'continue' ->
      {
        // encuentra el alcance envolvente más cercano con la etiqueta $forI_continue
        pop all local variables that are defined at the current point
        but not at $forI_continue
        jump($forI_continue)
        0 (as many as variables were removed above)
      }
    AssemblySwitch(switch condition cases ( default: defaultBlock )? ) ->
      {
        // // encuentra I de tal manera que no haya etiqueta o variable $switchI*
        let $switchI_value := condition
        for each of cases match {
          case val: -> jumpi($switchI_caseJ, eq($switchI_value, val))
        }
        if default block present: ->
          { defaultBlock jump($switchI_end) }
        for each of cases match {
          case val: { body } -> $switchI_caseJ: { body jump($switchI_end) }
        }
        $switchI_end:
      }
    FunctionalAssemblyExpression( identifier(arg1, arg2, ..., argn) ) ->
      {
        if identifier is function <name> with n args and m ret values ->
          {
            // encuentra I de tal manera que no exista $funcallI_*
            $funcallI_return argn  ... arg2 arg1 jump(<name>)
            pop (n + 1 times)
            if the current context is `let (id1, ..., idm) := f(...)` ->
              let id1 := 0 ... let idm := 0
              $funcallI_return:
            else ->
              0 (m times)
              $funcallI_return:
              turn the functional expression that leads to the function call
              into a statement stream
          }
        else -> desugar(children of node)
      }
    default node ->
      desugar(children of node)
    }

Generación de la transmisión de opcodes
---------------------------------------

Durante la generación de la transmisión de opcodes, hacemos un seguimiento de la altura de la pila en un contador, de tal manera que se pueda acceder a la variables de la pila por su nombre. Se modifica la altura de la pila con cada opcode que modifica la pila y con cada etiqueta que se anota con un ajuste de la pila. Cada vez que se introduce una nueva variable local, se registra junto con la altua actual de la pila. Si se accede a una variable (bien para copiar su valor, bien para asignar algo), se selecciona la instrucción ``DUP`` o ``SWAP``, dependiendo de la diferencia entre la altura actual de la pila y la altura de la pila en el momento en que se introdujo esta variable.

Pseudo código::

    codegen item: AST -> opcode_stream =
    match item {
    AssemblyBlock({ items }) ->
      join(codegen(item) for item in items)
      if last generated opcode has continuing control flow:
        POP for all local variables registered at the block (including variables
        introduced by labels)
        warn if the stack height at this point is not the same as at the start of the block
    Identifier(id) ->
      lookup id in the syntactic stack of blocks
      match type of id
        Local Variable ->
          DUPi where i = 1 + stack_height - stack_height_of_identifier(id)
        Label ->
          // se tendrá que resolver esta referencia durante la generación del bytecode
          PUSH<bytecode position of label>
        SubAssembly ->
          PUSH<bytecode position of subassembly data>
    FunctionalAssemblyExpression(id ( arguments ) ) ->
      join(codegen(arg) for arg in arguments.reversed())
      id (which has to be an opcode, might be a function name later)
    AssemblyLocalDefinition(let (id1, ..., idn) := expr) ->
      register identifiers id1, ..., idn as locals in current block at current stack height
      codegen(expr) - assert that expr returns n items to the stack
    FunctionalAssemblyAssignment((id1, ..., idn) := expr) ->
      lookup id1, ..., idn in the syntactic stack of blocks, assert that they are variables
      codegen(expr)
      for j = n, ..., i:
      SWAPi where i = 1 + stack_height - stack_height_of_identifier(idj)
      POP
    AssemblyAssignment(=: id) ->
      look up id in the syntactic stack of blocks, assert that it is a variable
      SWAPi where i = 1 + stack_height - stack_height_of_identifier(id)
      POP
    LabelDefinition(name:) ->
      JUMPDEST
    NumberLiteral(num) ->
      PUSH<num interpreted as decimal and right-aligned>
    HexLiteral(lit) ->
      PUSH32<lit interpreted as hex and left-aligned>
    StringLiteral(lit) ->
      PUSH32<lit utf-8 encoded and left-aligned>
    SubAssembly(assembly <name> block) ->
      append codegen(block) at the end of the code
    dataSize(<name>) ->
      assert that <name> is a subassembly ->
      PUSH32<size of code generated from subassembly <name>>
    linkerSymbol(<lit>) ->
      PUSH32<zeros> and append position to linker table
=======
If the memory operations use a length of zero, it is also fine to just use any offset (not only if it falls into the scratch space):

.. code-block:: solidity

    assembly ("memory-safe") {
      revert(0, 0)
    }

Note that not only memory operations in inline assembly itself can be memory-unsafe, but also assignments to
solidity variables of reference type in memory. For example the following is not memory-safe:

.. code-block:: solidity

    bytes memory x;
    assembly {
      x := 0x40
>>>>>>> 9e92c7a466cb2139e555a225fd61b2357085a4a0
    }
    x[0x20] = 0x42;

Inline assembly that neither involves any operations that access memory nor assigns to any solidity variables
in memory is automatically considered memory-safe and does not need to be annotated.

.. warning::
    It is your responsibility to make sure that the assembly actually satisfies the memory model. If you annotate
    an assembly block as memory-safe, but violate one of the memory assumptions, this **will** lead to incorrect and
    undefined behaviour that cannot easily be discovered by testing.

In case you are developing a library that is meant to be compatible across multiple versions
of solidity, you can use a special comment to annotate an assembly block as memory-safe:

.. code-block:: solidity

    /// @solidity memory-safe-assembly
    assembly {
        ...
    }

Note that we will disallow the annotation via comment in a future breaking release, so if you are not concerned with
backwards-compatibility with older compiler versions, prefer using the dialect string.
