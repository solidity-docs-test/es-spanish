####################################
Expresiones y estructuras de control
####################################

.. index:: ! parameter, parameter;input, parameter;output, function parameter, parameter;function, return variable, variable;return, return

<<<<<<< HEAD
Parámetros de entrada y de salida
=================================

Al igual que en Javascript, las funciones obtienen parámetros como entrada. Por otro lado, al contrario que en Javascript y C, estas también deberían devolver un número aleatorio de parámetros como salida.

Parámetros de entrada
---------------------

Los parámetros de entrada se declaran de la misma forma que las variables. Como una excepción, los parámetros no usados pueden omitir el nombre de la variable. Por ejemplo, si quisiéramos que nuestro contrato acepte un tipo de llamadas externas con dos enteros, el código quedaría similar a este::

    contract Simple {
        function taker(uint _a, uint _b) {
            // hace algo con _a y _b.
        }
    }

Parámetros de salida
--------------------

Los parámetros de salida se pueden declarar con la misma sintaxis después de la palabra reservada ``returns``. Por ejemplo, supongamos que deseamos devolver dos resultados: la suma y el producto de dos valores dados. Entonces, escribiríamos un código como este::

    contract Simple {
        function arithmetics(uint _a, uint _b) returns (uint o_sum, uint o_product) {
            o_sum = _a + _b;
            o_product = _a * _b;
        }
    }

Los nombres de los parámetros de salida se pueden omitir.
Los valores de salida se pueden especificar también usando declaraciones ``return``.
Las declaraciones ``return`` también son capaces de devolver múltiples valores, ver :ref:`multi-return`.
Los parámetros de retorno se inicializan a cero; si no se especifica explícitamente su valor, permanecen con dicho valor cero.

Los parámetros de entrada y salida se pueden usar como expresiones en el cuerpo de la función. En este caso, también pueden ir en el lado izquierdo de una asignación.
=======
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

.. index:: if, else, while, do/while, for, break, continue, return, switch, goto

Estructuras de control
======================

<<<<<<< HEAD
La mayoría de las estructuras de control disponibles en JavaScript, también lo están en Solidity exceptuando ``switch`` y ``goto``. Esto significa que tenemos: ``if``, ``else``, ``while``, ``do``, ``for``, ``break``, ``continue``, ``return``, ``? :``, con la semántica habitual conocida de C o JavaScript.

Los paréntesis no se pueden omitir para condicionales, pero sí las llaves alrededor de los cuerpos de las declaraciones sencillas.
=======
Most of the control structures known from curly-braces languages are available in Solidity:

There is: ``if``, ``else``, ``while``, ``do``, ``for``, ``break``, ``continue``, ``return``, with
the usual semantics known from C or JavaScript.

Solidity also supports exception handling in the form of ``try``/``catch``-statements,
but only for :ref:`external function calls <external-function-calls>` and
contract creation calls. Errors can be created using the :ref:`revert statement <revert-statement>`.

Parentheses can *not* be omitted for conditionals, but curly braces can be omitted
around single-statement bodies.
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

Hay que tener en cuenta que no hay conversión de tipos desde non-boolean a boolean como hay en C y JavaScript, por lo que ``if (1) { ... }`` *no* es válido en Solidity.

<<<<<<< HEAD
.. _multi-return:

Devolver múltiples valores
--------------------------

Cuando una función tiene múltiples parámetros de salida, ``return (v0, v1, ...,
vn)`` puede devolver múltiples valores. El número de componentes debe ser el mismo que el de parámetros de salida.

=======
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d
.. index:: ! function;call, function;internal, function;external

.. _function-calls:

Llamadas a funciones
====================

<<<<<<< HEAD
Llamadas a funciones internas
-----------------------------

Las funciones del contrato actual pueden ser llamadas directamente ("internamente") y, también, recursivamente como se puede ver en este ejemplo sin sentido funcional::
=======
.. _internal-function-calls:

Internal Function Calls
-----------------------

Functions of the current contract can be called directly ("internally"), also recursively, as seen in
this nonsensical example:
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.22 <0.9.0;

    // This will report a warning
    contract C {
        function g(uint a) public pure returns (uint ret) { return a + f(); }
        function f() internal pure returns (uint ret) { return g(7) + f(); }
    }

<<<<<<< HEAD
Estas llamadas a funciones son traducidas en simples saltos dentro de la máquina virtual de Ethereum (EVM). Esto tiene como consecuencia que la memoria actual no se limpia, así que pasar referencias de memoria a las funciones llamadas internamente es muy eficiente. Sólo las funciones del mismo contrato pueden ser llamadas internamente.
=======
These function calls are translated into simple jumps inside the EVM. This has
the effect that the current memory is not cleared, i.e. passing memory references
to internally-called functions is very efficient. Only functions of the same
contract instance can be called internally.

You should still avoid excessive recursion, as every internal function call
uses up at least one stack slot and there are only 1024 slots available.

.. _external-function-calls:
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

Llamadas a funciones externas
-----------------------------

<<<<<<< HEAD
Las expresiones ``this.g(8);`` and ``c.g(2);`` (donde ``c`` es la instancia de un contrato) son también llamadas válidas, pero en esta ocasión, la función se llamará "externamente" mediante un message call y no directamente por saltos.
Por favor, es importante tener en cuenta que las llamadas a funciones en ``this`` no pueden ser usadas en el constructor, ya que el contrato en cuestión no se ha creado todavía.
=======
Functions can also be called using the ``this.g(8);`` and ``c.g(2);`` notation, where
``c`` is a contract instance and ``g`` is a function belonging to ``c``.
Calling the function ``g`` via either way results in it being called "externally", using a
message call and not directly via jumps.
Please note that function calls on ``this`` cannot be used in the constructor,
as the actual contract has not been created yet.
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

Las funciones de otros contratos se tienen que llamar de forma externa. Para una llamada externa,
todos los argumentos de la función tienen que ser copiados en memoria.

<<<<<<< HEAD
Cuando se llama a funciones de otros contratos, la cantidad de Wei enviada con la llamada y el gas pueden especificarse con las opciones especiales ``.value()`` y ``.gas()``, respectivamente::
=======
.. note::
    A function call from one contract to another does not create its own transaction,
    it is a message call as part of the overall transaction.

When calling functions of other contracts, you can specify the amount of Wei or
gas sent with the call with the special options ``{value: 10, gas: 10000}``.
Note that it is discouraged to specify gas values explicitly, since the gas costs
of opcodes can change in the future. Any Wei you send to the contract is added
to the total balance of that contract:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.6.2 <0.9.0;
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

    contract InfoFeed {
        function info() public payable returns (uint ret) { return 42; }
    }

    contract Consumer {
        InfoFeed feed;
        function setFeed(InfoFeed addr) public { feed = addr; }
        function callFeed() public { feed.info{value: 10, gas: 800}(); }
    }

<<<<<<< HEAD
El modificador ``payable`` se tiene que usar para ``info``, porque de otra manera la opción `.value()` no estaría disponible.

Destacar que la expresión ``InfoFeed(addr)`` realiza una conversión de tipo explícita afirmando que "sabemos que el tipo de contrato en la dirección dada es ``InfoFeed``", sin ejecutar un constructor. Las conversiones de tipo explícitas tienen que ser gestionadas con extrema precaución. Nunca se debe llamar a una función en un contrato donde no se sabe con seguridad cuál es su tipo.

También se podría usar ``function setFeed(InfoFeed _feed) { feed = _feed; }`` directamente.
Hay que tener cuidado con el hecho de que ``feed.info.value(10).gas(800)`` sólo (localmente) establece el valor y la cantidad de gas enviada con la llamada a la función. Sólo tras el último paréntesis se realiza realmente la llamada.

Las llamadas a funciones provocan excepciones si el contrato invocado no existe (en el sentido de que la cuenta no contiene código) o si el contrato invocado por sí mismo dispara una excepción o se queda sin gas.

.. warning::
    Cualquier interacción con otro contrato supone un daño potencial, especialmente si el código fuente del contrato no se conoce de antemano. El contrato actual pasa el control al contrato invocado y eso potencialmente podría suponer que haga cualquier cosa. Incluso si el contrato invocado hereda de un contrato padre conocido, el contrato del que hereda sólo requiere tener una interfaz correcta. La implementación del contrato, sin embargo, puede ser totalmente aleatoria y, por ello, crear un perjuicio. Además, hay que estar preparado en caso de que llame dentro de otros contratos del sistema o, incluso, volver al contrato que lo llama antes de que la primera llamada retorne. Esto significa que el contrato invocado puede cambiar variables de estado del contrato que le llama mediante sus funciones. Escribir tus funciones de manera que realicen, por ejemplo, llamadas a funciones externas ocurridas después de cualquier cambio en variables de estado en tu contrato, hace que este contrato no sea vulnerable a un ataque de reentrada.
    

Llamadas con nombre y parámetros de funciones anónimas
------------------------------------------------------

Los argumentos de una llamada a una función pueden venir dados por el nombre, en cualquier orden, si están entre ``{ }`` como se puede ver en el siguiente ejemplo. La lista de argumentos tiene que coincidir por el nombre con la lista de parámetros de la declaración de la función, pero pueden estar en orden aleatorio.
=======
You need to use the modifier ``payable`` with the ``info`` function because
otherwise, the ``value`` option would not be available.

.. warning::
  Be careful that ``feed.info{value: 10, gas: 800}`` only locally sets the
  ``value`` and amount of ``gas`` sent with the function call, and the
  parentheses at the end perform the actual call. So
  ``feed.info{value: 10, gas: 800}`` does not call the function and
  the ``value`` and ``gas`` settings are lost, only
  ``feed.info{value: 10, gas: 800}()`` performs the function call.

Due to the fact that the EVM considers a call to a non-existing contract to
always succeed, Solidity uses the ``extcodesize`` opcode to check that
the contract that is about to be called actually exists (it contains code)
and causes an exception if it does not. This check is skipped if the return
data will be decoded after the call and thus the ABI decoder will catch the
case of a non-existing contract.

Note that this check is not performed in case of :ref:`low-level calls <address_related>` which
operate on addresses rather than contract instances.

.. note::
    Be careful when using high-level calls to
    :ref:`precompiled contracts <precompiledContracts>`,
    since the compiler considers them non-existing according to the
    above logic even though they execute code and can return data.

Function calls also cause exceptions if the called contract itself
throws an exception or goes out of gas.

.. warning::
    Any interaction with another contract imposes a potential danger, especially
    if the source code of the contract is not known in advance. The
    current contract hands over control to the called contract and that may potentially
    do just about anything. Even if the called contract inherits from a known parent contract,
    the inheriting contract is only required to have a correct interface. The
    implementation of the contract, however, can be completely arbitrary and thus,
    pose a danger. In addition, be prepared in case it calls into other contracts of
    your system or even back into the calling contract before the first
    call returns. This means
    that the called contract can change state variables of the calling contract
    via its functions. Write your functions in a way that, for example, calls to
    external functions happen after any changes to state variables in your contract
    so your contract is not vulnerable to a reentrancy exploit.

.. note::
    Before Solidity 0.6.2, the recommended way to specify the value and gas was to
    use ``f.value(x).gas(g)()``. This was deprecated in Solidity 0.6.2 and is no
    longer possible since Solidity 0.7.0.

Named Calls and Anonymous Function Parameters
---------------------------------------------

Function call arguments can be given by name, in any order,
if they are enclosed in ``{ }`` as can be seen in the following
example. The argument list has to coincide by name with the list of
parameters from the function declaration, but can be in arbitrary order.
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.0 <0.9.0;

    contract C {
        mapping(uint => uint) data;

<<<<<<< HEAD
        function g() {
            // argumentos con nombre
            f({value: 2, key: 3});
=======
        function f() public {
            set({value: 2, key: 3});
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d
        }

        function set(uint key, uint value) public {
            data[key] = value;
        }

    }

Nombres de parámetros de función omitidos
-----------------------------------------

<<<<<<< HEAD
Los nombres de parámetros no usados (especialmente los de retorno) se pueden omitir.
Esos nombres estarán presentes en la pila, pero serán inaccesibles.
=======
The names of unused parameters (especially return parameters) can be omitted.
Those parameters will still be present on the stack, but they are inaccessible.
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.22 <0.9.0;

    contract C {
<<<<<<< HEAD
        // Se omite el nombre para el parámetro
        function func(uint k, uint) returns(uint) {
=======
        // omitted name for parameter
        function func(uint k, uint) public pure returns(uint) {
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d
            return k;
        }
    }


.. index:: ! new, contracts;creation

.. _creating-contracts:

Creando contratos mediante ``new``
==================================

<<<<<<< HEAD
Un contrato puede crear un nuevo contrato usando la palabra reservada ``new``. El código completo del contrato que se está creando tiene que ser conocido de antemano, por lo que no son posibles las dependencias de creación recursivas.
=======
A contract can create other contracts using the ``new`` keyword. The full
code of the contract being created has to be known when the creating contract
is compiled so recursive creation-dependencies are not possible.
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.7.0 <0.9.0;
    contract D {
        uint public x;
        constructor(uint a) payable {
            x = a;
        }
    }

    contract C {
        D d = new D(4); // Se ejecutará como parte del constructor de C

        function createD(uint arg) public {
            D newD = new D(arg);
            newD.x();
        }

<<<<<<< HEAD
        function createAndEndowD(uint arg, uint amount) {
            // Envía Ether junto con la creación
            D newD = (new D).value(amount)(arg);
        }
    }

Como se ve en el ejemplo, es posible traspasar Ether a la creación usando la opción ``.value()``,
pero no es posible limitar la cantidad de gas. Si la creación falla
(debido al desbordamiento de la pila, falta de balance o cualquier otro problema), se dispara una excepción.
=======
        function createAndEndowD(uint arg, uint amount) public payable {
            // Send ether along with the creation
            D newD = new D{value: amount}(arg);
            newD.x();
        }
    }

As seen in the example, it is possible to send Ether while creating
an instance of ``D`` using the ``value`` option, but it is not possible
to limit the amount of gas.
If the creation fails (due to out-of-stack, not enough balance or other problems),
an exception is thrown.

Salted contract creations / create2
-----------------------------------

When creating a contract, the address of the contract is computed from
the address of the creating contract and a counter that is increased with
each contract creation.

If you specify the option ``salt`` (a bytes32 value), then contract creation will
use a different mechanism to come up with the address of the new contract:

It will compute the address from the address of the creating contract,
the given salt value, the (creation) bytecode of the created contract and the constructor
arguments.

In particular, the counter ("nonce") is not used. This allows for more flexibility
in creating contracts: You are able to derive the address of the
new contract before it is created. Furthermore, you can rely on this address
also in case the creating
contracts creates other contracts in the meantime.

The main use-case here is contracts that act as judges for off-chain interactions,
which only need to be created if there is a dispute.

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.7.0 <0.9.0;
    contract D {
        uint public x;
        constructor(uint a) {
            x = a;
        }
    }

    contract C {
        function createDSalted(bytes32 salt, uint arg) public {
            // This complicated expression just tells you how the address
            // can be pre-computed. It is just there for illustration.
            // You actually only need ``new D{salt: salt}(arg)``.
            address predictedAddress = address(uint160(uint(keccak256(abi.encodePacked(
                bytes1(0xff),
                address(this),
                salt,
                keccak256(abi.encodePacked(
                    type(D).creationCode,
                    abi.encode(arg)
                ))
            )))));

            D d = new D{salt: salt}(arg);
            require(address(d) == predictedAddress);
        }
    }

.. warning::
    There are some peculiarities in relation to salted creation. A contract can be
    re-created at the same address after having been destroyed. Yet, it is possible
    for that newly created contract to have a different deployed bytecode even
    though the creation bytecode has been the same (which is a requirement because
    otherwise the address would change). This is due to the fact that the constructor
    can query external state that might have changed between the two creations
    and incorporate that into the deployed bytecode before it is stored.

>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

Orden de la evaluación de expresiones
=====================================

<<<<<<< HEAD
El orden de evaluación de expresiones no se especifica (más formalmente, el orden en el que los hijos de un nodo en el árbol de la expresión son evaluados no es especificado. Eso sí, son evaluados antes que el propio nodo). Sólo se garantiza que las sentencias se ejecutan en orden y que se hace un cortocircuito para las expresiones booleanas. Ver :ref:`order` para más información.
=======
The evaluation order of expressions is not specified (more formally, the order
in which the children of one node in the expression tree are evaluated is not
specified, but they are of course evaluated before the node itself). It is only
guaranteed that statements are executed in order and short-circuiting for
boolean expressions is done.
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

.. index:: ! assignment

Asignación
==========

.. index:: ! assignment;destructuring

Asignaciones para desestructurar y retornar múltiples valores
-------------------------------------------------------------

<<<<<<< HEAD
Solidity internamente permite tipos tupla, p.ej.: una lista de objetos de, potencialmente, diferentes tipos cuyo tamaño es constante en tiempo de compilación. Esas tuplas pueden ser usadas para retornar múltiples valores al mismo tiempo y, también, asignarlos a múltiples variables (o lista de valores en general) también al mismo tiempo::
=======
Solidity internally allows tuple types, i.e. a list of objects
of potentially different types whose number is a constant at
compile-time. Those tuples can be used to return multiple values at the same time.
These can then either be assigned to newly declared variables
or to pre-existing variables (or LValues in general).

Tuples are not proper types in Solidity, they can only be used to form syntactic
groupings of expressions.

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.5.0 <0.9.0;
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

    contract C {
        uint index;

        function f() public pure returns (uint, bool, uint) {
            return (7, true, 2);
        }

<<<<<<< HEAD
        function g() {
            // Declara y asigna variables. No es posible especificar el tipo de forma explícita.
            var (x, b, y) = f();
            // Asigna a una variable pre-existente.
            (x, y) = (2, 7);
            // Truco común para intercambiar valores -- no funciona con tipos de almacenamiento sin valor.
            (x, y) = (y, x);
            // Los componentes se pueden dejar fuera (también en declaraciones de variables).
            // Si la tupla acaba en un componente vacío,
            // el resto de los valores se descartan.
            (data.length,) = f(); // Establece la longitud a 7
            // Lo mismo se puede hacer en el lado izquierdo.
            (,data[3]) = f(); // Sets data[3] to 2
            // Los componentes sólo se pueden dejar en el lado izquierdo de las asignaciones, con
            // una excepción:
            (x,) = (1,);
            // (1,) es la única forma de especificar una tupla de un componente, porque (1) 
            // equivale a 1.
        }
    }

Complicaciones en Arrays y Structs
----------------------------------

La sintaxis de asignación es algo más complicada para tipos sin valor como arrays y structs.
Las asignaciones *a* variables de estado siempre crean una copia independiente. Por otro lado, asignar una variable local crea una copia independiente sólo para tipos elementales, como tipos estáticos que encajan en 32 bytes. Si los structs o arrays (incluyendo ``bytes`` y ``string``) son asignados desde una variable de estado a una local, la variable local se queda una referencia a la variable de estado original. Una segunda asignación a la variable local no modifica el estado, sólo cambia la referencia. Las asignaciones a miembros (o elementos) de la variable local *hacen* cambiar el estado.
=======
        function g() public {
            // Variables declared with type and assigned from the returned tuple,
            // not all elements have to be specified (but the number must match).
            (uint x, , uint y) = f();
            // Common trick to swap values -- does not work for non-value storage types.
            (x, y) = (y, x);
            // Components can be left out (also for variable declarations).
            (index, , ) = f(); // Sets the index to 7
        }
    }

It is not possible to mix variable declarations and non-declaration assignments,
i.e. the following is not valid: ``(x, uint y) = (1, 2);``

.. note::
    Prior to version 0.5.0 it was possible to assign to tuples of smaller size, either
    filling up on the left or on the right side (which ever was empty). This is
    now disallowed, so both sides have to have the same number of components.

.. warning::
    Be careful when assigning to multiple variables at the same time when
    reference types are involved, because it could lead to unexpected
    copying behaviour.

Complications for Arrays and Structs
------------------------------------

The semantics of assignments are more complicated for non-value types like arrays and structs,
including ``bytes`` and ``string``, see :ref:`Data location and assignment behaviour <data-location-assignment>` for details.

In the example below the call to ``g(x)`` has no effect on ``x`` because it creates
an independent copy of the storage value in memory. However, ``h(x)`` successfully modifies ``x``
because only a reference and not a copy is passed.

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.22 <0.9.0;

    contract C {
        uint[20] x;

        function f() public {
            g(x);
            h(x);
        }

        function g(uint[20] memory y) internal pure {
            y[2] = 3;
        }

        function h(uint[20] storage y) internal {
            y[3] = 4;
        }
    }
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

.. index:: ! scoping, declarations, default value

.. _default-value:

Scoping y declaraciones
=========================

<<<<<<< HEAD
Una variable cuando se declara tendrá un valor inicial por defecto que, representado en bytes, será todo ceros.
Los valores por defecto de las variables son los típicos "estado-cero" cualquiera que sea el tipo. Por ejemplo, el valor por defecto para un ``bool`` es ``false``. El valor por defecto para un ``uint`` o ``int`` es ``0``. Para arrays de tamaño estático y ``bytes1`` hasta ``bytes32``, cada elemento individual será inicializado a un valor por defecto según sea su tipo. Finalmente, para arrays de tamaño dinámico, ``bytes``y ``string``, el valor por defecto es un array o string vacío.

Una variable declarada en cualquier punto de una función estará dentro del alcance de *toda la función*, independientemente de donde se haya declarado. Esto ocurre porque Solidity hereda sus reglas de scoping de JavaScript.
Esto difiere de muchos lenguajes donde las variables sólo están en el alcance de donde se declaran hasta que acaba el bloque semántico.
Como consecuencia de esto, el código siguiente es ilegal y hace que el compilador devuelva un error porque el identificador se ha declarado previamente, ``Identifier already declared``::
=======
A variable which is declared will have an initial default
value whose byte-representation is all zeros.
The "default values" of variables are the typical "zero-state"
of whatever the type is. For example, the default value for a ``bool``
is ``false``. The default value for the ``uint`` or ``int``
types is ``0``. For statically-sized arrays and ``bytes1`` to
``bytes32``, each individual
element will be initialized to the default value corresponding
to its type. For dynamically-sized arrays, ``bytes``
and ``string``, the default value is an empty array or string.
For the ``enum`` type, the default value is its first member.

Scoping in Solidity follows the widespread scoping rules of C99
(and many other languages): Variables are visible from the point right after their declaration
until the end of the smallest ``{ }``-block that contains the declaration.
As an exception to this rule, variables declared in the
initialization part of a for-loop are only visible until the end of the for-loop.
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

Variables that are parameter-like (function parameters, modifier parameters,
catch parameters, ...) are visible inside the code block that follows -
the body of the function/modifier for a function and modifier parameter and the catch block
for a catch parameter.

Variables and other items declared outside of a code block, for example functions, contracts,
user-defined types, etc., are visible even before they were declared. This means you can
use state variables before they are declared and call functions recursively.

As a consequence, the following examples will compile without warnings, since
the two variables have the same name but disjoint scopes.

<<<<<<< HEAD
            while (i++ < 2) {
                uint same1 = 0; // Ilegal, segunda declaración para same1
            }
        }
=======
.. code-block:: solidity
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.5.0 <0.9.0;
    contract C {
        function minimalScoping() pure public {
            {
                uint same;
                same = 1;
            }

            {
<<<<<<< HEAD
                uint same2 = 0; // Ilegal, segunda declaración para same2
            }
        }

        function forLoopScoping() {
            for (uint same3 = 0; same3 < 1; same3++) {
            }

            for (uint same3 = 0; same3 < 1; same3++) { // Ilegal, segunda declaración para same3
=======
                uint same;
                same = 3;
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d
            }
        }
    }

<<<<<<< HEAD
Como añadido a esto, si la variable se declara, se inicializará al principio de la función con su valor por defecto.
Esto significa que el siguiente código es legal, aunque se haya escrito de manera un tanto pobre::

    function foo() returns (uint) {
        // baz se inicializa implícitamente a 0
        uint bar = 5;
        if (true) {
            bar += baz;
        } else {
            uint baz = 10;// Nunca se ejecuta
        }
        return bar;// devuelve 5
=======
As a special example of the C99 scoping rules, note that in the following,
the first assignment to ``x`` will actually assign the outer and not the inner variable.
In any case, you will get a warning about the outer variable being shadowed.

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.5.0 <0.9.0;
    // This will report a warning
    contract C {
        function f() pure public returns (uint) {
            uint x = 1;
            {
                x = 2; // this will assign to the outer variable
                uint x;
            }
            return x; // x has value 2
        }
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d
    }

.. warning::
    Before version 0.5.0 Solidity followed the same scoping rules as
    JavaScript, that is, a variable declared anywhere within a function would be in scope
    for the entire function, regardless where it was declared. The following example shows a code snippet that used
    to compile but leads to an error starting from version 0.5.0.

<<<<<<< HEAD
Excepciones
===========

Hay algunos casos en los que las excepciones se lanzan automáticamente (ver más adelante). Se puede usar la instrucción ``throw`` para lanzarlas manualmente. La consecuencia de una excepción es que la llamada que se está ejecutando en ese momento se para y se revierte (todos los cambios en los estados y balances se deshacen) y la excepción también se genera mediante llamadas de función de Solidity (las excepciones ``send`` y las funciones de bajo nivel ``call``, ``delegatecall`` y ``callcode``, todas ellas devuelven ``false`` en caso de una excepción).

Todavía no es posible capturar excepciones.

En el siguiente ejemplo, se enseña como ``throw`` se puede usar para revertir fácilmente una transferencia de Ether y, además, se enseña como comprobar el valor de retorno de ``send``::
=======
.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.5.0 <0.9.0;
    // This will not compile
    contract C {
        function f() pure public returns (uint) {
            x = 2;
            uint x;
            return x;
        }
    }


.. index:: ! safe math, safemath, checked, unchecked
.. _unchecked:
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d

Checked or Unchecked Arithmetic
===============================

An overflow or underflow is the situation where the resulting value of an arithmetic operation,
when executed on an unrestricted integer, falls outside the range of the result type.

Prior to Solidity 0.8.0, arithmetic operations would always wrap in case of
under- or overflow leading to widespread use of libraries that introduce
additional checks.

Since Solidity 0.8.0, all arithmetic operations revert on over- and underflow by default,
thus making the use of these libraries unnecessary.

To obtain the previous behaviour, an ``unchecked`` block can be used:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity ^0.8.0;
    contract C {
        function f(uint a, uint b) pure public returns (uint) {
            // This subtraction will wrap on underflow.
            unchecked { return a - b; }
        }
        function g(uint a, uint b) pure public returns (uint) {
            // This subtraction will revert on underflow.
            return a - b;
        }
    }

The call to ``f(2, 3)`` will return ``2**256-1``, while ``g(2, 3)`` will cause
a failing assertion.

The ``unchecked`` block can be used everywhere inside a block, but not as a replacement
for a block. It also cannot be nested.

The setting only affects the statements that are syntactically inside the block.
Functions called from within an ``unchecked`` block do not inherit the property.

.. note::
    To avoid ambiguity, you cannot use ``_;`` inside an ``unchecked`` block.

The following operators will cause a failing assertion on overflow or underflow
and will wrap without an error if used inside an unchecked block:

``++``, ``--``, ``+``, binary ``-``, unary ``-``, ``*``, ``/``, ``%``, ``**``

``+=``, ``-=``, ``*=``, ``/=``, ``%=``

.. warning::
    It is not possible to disable the check for division by zero
    or modulo by zero using the ``unchecked`` block.

.. note::
   Bitwise operators do not perform overflow or underflow checks.
   This is particularly visible when using bitwise shifts (``<<``, ``>>``, ``<<=``, ``>>=``) in
   place of integer division and multiplication by a power of 2.
   For example ``type(uint256).max << 3`` does not revert even though ``type(uint256).max * 8`` would.

.. note::
    The second statement in ``int x = type(int).min; -x;`` will result in an overflow
    because the negative range can hold one more value than the positive range.

Explicit type conversions will always truncate and never cause a failing assertion
with the exception of a conversion from an integer to an enum type.

.. index:: ! exception, ! throw, ! assert, ! require, ! revert, ! errors

.. _assert-and-require:

Error handling: Assert, Require, Revert and Exceptions
======================================================

Solidity uses state-reverting exceptions to handle errors.
Such an exception undoes all changes made to the
state in the current call (and all its sub-calls) and
flags an error to the caller.

When exceptions happen in a sub-call, they "bubble up" (i.e.,
exceptions are rethrown) automatically unless they are caught in
a ``try/catch`` statement. Exceptions to this rule are ``send``
and the low-level functions ``call``, ``delegatecall`` and
``staticcall``: they return ``false`` as their first return value in case
of an exception instead of "bubbling up".

.. warning::
    The low-level functions ``call``, ``delegatecall`` and
    ``staticcall`` return ``true`` as their first return value
    if the account called is non-existent, as part of the design
    of the EVM. Account existence must be checked prior to calling if needed.

Exceptions can contain error data that is passed back to the caller
in the form of :ref:`error instances <errors>`.
The built-in errors ``Error(string)`` and ``Panic(uint256)`` are
used by special functions, as explained below. ``Error`` is used for "regular" error conditions
while ``Panic`` is used for errors that should not be present in bug-free code.

Panic via ``assert`` and Error via ``require``
----------------------------------------------

The convenience functions ``assert`` and ``require`` can be used to check for conditions and throw an exception
if the condition is not met.

The ``assert`` function creates an error of type ``Panic(uint256)``.
The same error is created by the compiler in certain situations as listed below.

Assert should only be used to test for internal
errors, and to check invariants. Properly functioning code should
never create a Panic, not even on invalid external input.
If this happens, then there
is a bug in your contract which you should fix. Language analysis
tools can evaluate your contract to identify the conditions and
function calls which will cause a Panic.

A Panic exception is generated in the following situations.
The error code supplied with the error data indicates the kind of panic.

#. 0x00: Used for generic compiler inserted panics.
#. 0x01: If you call ``assert`` with an argument that evaluates to false.
#. 0x11: If an arithmetic operation results in underflow or overflow outside of an ``unchecked { ... }`` block.
#. 0x12; If you divide or modulo by zero (e.g. ``5 / 0`` or ``23 % 0``).
#. 0x21: If you convert a value that is too big or negative into an enum type.
#. 0x22: If you access a storage byte array that is incorrectly encoded.
#. 0x31: If you call ``.pop()`` on an empty array.
#. 0x32: If you access an array, ``bytesN`` or an array slice at an out-of-bounds or negative index (i.e. ``x[i]`` where ``i >= x.length`` or ``i < 0``).
#. 0x41: If you allocate too much memory or create an array that is too large.
#. 0x51: If you call a zero-initialized variable of internal function type.

The ``require`` function either creates an error without any data or
an error of type ``Error(string)``. It
should be used to ensure valid conditions
that cannot be detected until execution time.
This includes conditions on inputs
or return values from calls to external contracts.

.. note::

    It is currently not possible to use custom errors in combination
    with ``require``. Please use ``if (!condition) revert CustomError();`` instead.

An ``Error(string)`` exception (or an exception without data) is generated
by the compiler
in the following situations:

#. Calling ``require(x)`` where ``x`` evaluates to ``false``.
#. If you use ``revert()`` or ``revert("description")``.
#. If you perform an external function call targeting a contract that contains no code.
#. If your contract receives Ether via a public function without
   ``payable`` modifier (including the constructor and the fallback function).
#. If your contract receives Ether via a public getter function.

For the following cases, the error data from the external call
(if provided) is forwarded. This means that it can either cause
an `Error` or a `Panic` (or whatever else was given):

#. If a ``.transfer()`` fails.
#. If you call a function via a message call but it does not finish
   properly (i.e., it runs out of gas, has no matching function, or
   throws an exception itself), except when a low level operation
   ``call``, ``send``, ``delegatecall``, ``callcode`` or ``staticcall``
   is used. The low level operations never throw exceptions but
   indicate failures by returning ``false``.
#. If you create a contract using the ``new`` keyword but the contract
   creation :ref:`does not finish properly<creating-contracts>`.

You can optionally provide a message string for ``require``, but not for ``assert``.

.. note::
    If you do not provide a string argument to ``require``, it will revert
    with empty error data, not even including the error selector.


The following example shows how you can use ``require`` to check conditions on inputs
and ``assert`` for internal error checking.

.. code-block:: solidity
    :force:

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.5.0 <0.9.0;

    contract Sharer {
<<<<<<< HEAD
        function sendHalf(address addr) payable returns (uint balance) {
            if (!addr.send(msg.value / 2))
                throw; // También revierte la transferencia de Sharer
            return this.balance;
        }
    }

Actualmente, Solidity genera automáticamente una excepción en tiempo de ejecución en las siguientes situaciones:

#. Si se accede a un array en un índice demasiado largo o negativo (ejemplo: ``x[i]`` donde ``i >= x.length`` o ``i < 0``).
#. Si se accede a un ``bytesN`` de longitud fija en un índice demasiado largo o negativo.
#. Si se llama a una función con un message call, pero no finaliza adecuadamente (ejemplo: se queda sin gas, no tiene una función de matching, o dispara una excepción por sí mismo), exceptuando el caso en el que se use una operación de bajo nivel ``call``, ``send``, ``delegatecall`` o ``callcode``. Las operaciones de bajo nivel nunca disparan excepciones, pero indican fallos devolviendo ``false``.
#. Si se crea un contrato usando la palabra reservada ``new``, pero la creación del contrato no finaliza correctamente (ver más arriba la definición de "no finalizar correctamente").
#. Si se divide o se hace módulo por cero (ejemplos: ``5 / 0`` o ``23 % 0``).
#. Si se hace un movimiento por una cantidad negativa.
#. Si se convierte un valor muy grande o negativo en un tipo enum.
#. Si se realiza una llamada de función externa apuntando a un contrato que no contiene código.
#. Si un contrato recibe Ether mediante una función sin el modificador ``payable`` (incluyendo el constructor y la función de fallback).
#. Si un contrato recibe Ether mediante una función getter pública.
#. Si se llama a una variable inicializada a cero de un tipo de función interna.
#. Si un ``.transfer()`` falla.
#. Si se invoca con ``assert`` junto con un argumento que evalúa a falso.

Un usuario genera una excepción en las siguientes situaciones:

#. Llamando a ``throw``.
#. Llamando a ``require`` junto con un argumento que evalúa a ``false``.

Internamente, Solidity realiza una operación de revertir (revert, instrucción ``0xfd``) cuando una excepción provista por un usuario se lanza o la condición de la llamada ``require`` no se satisface. Por contra, realiza una operación inválida (instrucción ``0xfe``) si una excepción en tiempo de ejecución aparece o la condición de una llamada ``assert`` no se satisface. En ambos casos, esto ocasiona que la EVM revierta todos los cambios de estado acaecidos. El motivo de todo esto es que no existe un modo seguro de continuar con la ejecución debido a que no sucedió el efecto esperado. Como se quiere mantener la atomicidad de las transacciones, lo más seguro es revertir todos los cambios y hacer que la transacción no tenga ningún efecto en su totalidad o, como mínimo, en la llamada.

En el caso de que los contratos se escriban de tal manera que ``assert`` sólo sea usado para probar condiciones internas y ``require`` se use en caso de que haya una entrada malformada, una herramienta de análisis formal que verifique que el opcode inválido nunca pueda ser alcanzado, se podría usar para chequear la ausencia de errores asumiendo entradas válidas.
=======
        function sendHalf(address payable addr) public payable returns (uint balance) {
            require(msg.value % 2 == 0, "Even value required.");
            uint balanceBeforeTransfer = address(this).balance;
            addr.transfer(msg.value / 2);
            // Since transfer throws an exception on failure and
            // cannot call back here, there should be no way for us to
            // still have half of the money.
            assert(address(this).balance == balanceBeforeTransfer - msg.value / 2);
            return address(this).balance;
        }
    }

Internally, Solidity performs a revert operation (instruction
``0xfd``). This causes
the EVM to revert all changes made to the state. The reason for reverting
is that there is no safe way to continue execution, because an expected effect
did not occur. Because we want to keep the atomicity of transactions, the
safest action is to revert all changes and make the whole transaction
(or at least call) without effect.

In both cases, the caller can react on such failures using ``try``/``catch``, but
the changes in the callee will always be reverted.

.. note::

    Panic exceptions used to use the ``invalid`` opcode before Solidity 0.8.0,
    which consumed all gas available to the call.
    Exceptions that use ``require`` used to consume all gas until before the Metropolis release.

.. _revert-statement:

``revert``
----------

A direct revert can be triggered using the ``revert`` statement and the ``revert`` function.

The ``revert`` statement takes a custom error as direct argument without parentheses:

    revert CustomError(arg1, arg2);

For backwards-compatibility reasons, there is also the ``revert()`` function, which uses parentheses
and accepts a string:

    revert();
    revert("description");

The error data will be passed back to the caller and can be caught there.
Using ``revert()`` causes a revert without any error data while ``revert("description")``
will create an ``Error(string)`` error.

Using a custom error instance will usually be much cheaper than a string description,
because you can use the name of the error to describe it, which is encoded in only
four bytes. A longer description can be supplied via NatSpec which does not incur
any costs.

The following example shows how to use an error string and a custom error instance
together with ``revert`` and the equivalent ``require``:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity ^0.8.4;

    contract VendingMachine {
        address owner;
        error Unauthorized();
        function buy(uint amount) public payable {
            if (amount > msg.value / 2 ether)
                revert("Not enough Ether provided.");
            // Alternative way to do it:
            require(
                amount <= msg.value / 2 ether,
                "Not enough Ether provided."
            );
            // Perform the purchase.
        }
        function withdraw() public {
            if (msg.sender != owner)
                revert Unauthorized();

            payable(msg.sender).transfer(address(this).balance);
        }
    }

The two ways ``if (!condition) revert(...);`` and ``require(condition, ...);`` are
equivalent as long as the arguments to ``revert`` and ``require`` do not have side-effects,
for example if they are just strings.

.. note::
    The ``require`` function is evaluated just as any other function.
    This means that all arguments are evaluated before the function itself is executed.
    In particular, in ``require(condition, f())`` the function ``f`` is executed even if
    ``condition`` is true.

The provided string is :ref:`abi-encoded <ABI>` as if it were a call to a function ``Error(string)``.
In the above example, ``revert("Not enough Ether provided.");`` returns the following hexadecimal as error return data:

.. code::

    0x08c379a0                                                         // Function selector for Error(string)
    0x0000000000000000000000000000000000000000000000000000000000000020 // Data offset
    0x000000000000000000000000000000000000000000000000000000000000001a // String length
    0x4e6f7420656e6f7567682045746865722070726f76696465642e000000000000 // String data

The provided message can be retrieved by the caller using ``try``/``catch`` as shown below.

.. note::
    There used to be a keyword called ``throw`` with the same semantics as ``revert()`` which
    was deprecated in version 0.4.13 and removed in version 0.5.0.


.. _try-catch:

``try``/``catch``
-----------------

A failure in an external call can be caught using a try/catch statement, as follows:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.8.1;

    interface DataFeed { function getData(address token) external returns (uint value); }

    contract FeedConsumer {
        DataFeed feed;
        uint errorCount;
        function rate(address token) public returns (uint value, bool success) {
            // Permanently disable the mechanism if there are
            // more than 10 errors.
            require(errorCount < 10);
            try feed.getData(token) returns (uint v) {
                return (v, true);
            } catch Error(string memory /*reason*/) {
                // This is executed in case
                // revert was called inside getData
                // and a reason string was provided.
                errorCount++;
                return (0, false);
            } catch Panic(uint /*errorCode*/) {
                // This is executed in case of a panic,
                // i.e. a serious error like division by zero
                // or overflow. The error code can be used
                // to determine the kind of error.
                errorCount++;
                return (0, false);
            } catch (bytes memory /*lowLevelData*/) {
                // This is executed in case revert() was used.
                errorCount++;
                return (0, false);
            }
        }
    }

The ``try`` keyword has to be followed by an expression representing an external function call
or a contract creation (``new ContractName()``).
Errors inside the expression are not caught (for example if it is a complex expression
that also involves internal function calls), only a revert happening inside the external
call itself. The ``returns`` part (which is optional) that follows declares return variables
matching the types returned by the external call. In case there was no error,
these variables are assigned and the contract's execution continues inside the
first success block. If the end of the success block is reached, execution continues after the ``catch`` blocks.

Solidity supports different kinds of catch blocks depending on the
type of error:

- ``catch Error(string memory reason) { ... }``: This catch clause is executed if the error was caused by ``revert("reasonString")`` or
  ``require(false, "reasonString")`` (or an internal error that causes such an
  exception).

- ``catch Panic(uint errorCode) { ... }``: If the error was caused by a panic, i.e. by a failing ``assert``, division by zero,
  invalid array access, arithmetic overflow and others, this catch clause will be run.

- ``catch (bytes memory lowLevelData) { ... }``: This clause is executed if the error signature
  does not match any other clause, if there was an error while decoding the error
  message, or
  if no error data was provided with the exception.
  The declared variable provides access to the low-level error data in that case.

- ``catch { ... }``: If you are not interested in the error data, you can just use
  ``catch { ... }`` (even as the only catch clause) instead of the previous clause.


It is planned to support other types of error data in the future.
The strings ``Error`` and ``Panic`` are currently parsed as is and are not treated as identifiers.

In order to catch all error cases, you have to have at least the clause
``catch { ...}`` or the clause ``catch (bytes memory lowLevelData) { ... }``.

The variables declared in the ``returns`` and the ``catch`` clause are only
in scope in the block that follows.

.. note::

    If an error happens during the decoding of the return data
    inside a try/catch-statement, this causes an exception in the currently
    executing contract and because of that, it is not caught in the catch clause.
    If there is an error during decoding of ``catch Error(string memory reason)``
    and there is a low-level catch clause, this error is caught there.

.. note::

    If execution reaches a catch-block, then the state-changing effects of
    the external call have been reverted. If execution reaches
    the success block, the effects were not reverted.
    If the effects have been reverted, then execution either continues
    in a catch block or the execution of the try/catch statement itself
    reverts (for example due to decoding failures as noted above or
    due to not providing a low-level catch clause).

.. note::
    The reason behind a failed call can be manifold. Do not assume that
    the error message is coming directly from the called contract:
    The error might have happened deeper down in the call chain and the
    called contract just forwarded it. Also, it could be due to an
    out-of-gas situation and not a deliberate error condition:
    The caller always retains at least 1/64th of the gas in a call and thus
    even if the called contract goes out of gas, the caller still
    has some gas left.
>>>>>>> 1bc1f8bdbbbb4b0d884df7493b3dc4a0b66aad6d
