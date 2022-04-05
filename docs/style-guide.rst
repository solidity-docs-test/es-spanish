.. index:: style, coding style

##############
Guía de estilo
##############

************
Introducción
************

<<<<<<< HEAD
Esta guía pretende proporcionar convenciones de codificación para escribir código con Solidity.
Esta guía debe ser entendida como un documento en evolución que cambiará con el tiempo según aparecen nuevas convenciones útiles y antiguas convenciones se vuelven obsoletas.
=======
This guide is intended to provide coding conventions for writing Solidity code.
This guide should be thought of as an evolving document that will change over
time as useful conventions are found and old conventions are rendered obsolete.
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

Muchos proyectos implementarán sus propias guías de estilo. En el caso de conflictos, las guías de estilo específicas del proyecto tendrán prioridad.

La estructura y muchas de las recomendaciones de esta guía de estilo fueron tomadas de Python: `guía de estilo pep8 <https://www.python.org/dev/peps/pep-0008/>`_.

<<<<<<< HEAD
El objetivo de esta guía *no* es ser la forma correcta o la mejor manera de escribir código con Solidity. El objetivo de esta guía es la *consistencia*. Una cita de python `pep8 <https://www.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds>`_ capta bien este concepto.

    Una guía de estilo es sobre consistencia. La consistencia con esta guía de estilo es importante. La consistencia dentro de un proyecto es más importante. La consistencia dentro de un módulo o función es lo más importante.
    Pero sobre todo: saber cuándo ser inconsistente - a veces la guía de estilo simplemente no se aplica. En caso de duda, use su mejor juicio. Mire otros ejemplos y decida qué parece mejor. ¡Y no dude en preguntar!
=======
The goal of this guide is *not* to be the right way or the best way to write
Solidity code.  The goal of this guide is *consistency*.  A quote from python's
`pep8 <https://www.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds>`_
captures this concept well.

.. note::

    A style guide is about consistency. Consistency with this style guide is important. Consistency within a project is more important. Consistency within one module or function is most important.

    But most importantly: **know when to be inconsistent** -- sometimes the style guide just doesn't apply. When in doubt, use your best judgment. Look at other examples and decide what looks best. And don't hesitate to ask!
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9


*****************
Diseño del código
*****************


Sangría
=======

Utilice 4 espacios por nivel de sangría.

Tabulador o espacios
====================

Los espacios son el método de indentación preferido.

Se deben evitar la mezcla del tabulador y los espacios.

Líneas en blanco
================

<<<<<<< HEAD
Envuelva las declaraciones de nivel superior en el código de Solidity con dos líneas en blanco.

Sí::
=======
Surround top level declarations in Solidity source with two blank lines.

Yes:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.0 <0.9.0;
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    contract A {
        // ...
    }


    contract B {
        // ...
    }


    contract C {
        // ...
    }

No:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.0 <0.9.0;

    contract A {
        // ...
    }
    contract B {
        // ...
    }

    contract C {
        // ...
    }

Dentro de un contrato, rodee las declaraciones de una función con una sola línea en blanco.

Las líneas en blanco se pueden omitir entre grupos de una frase relacionada (tales como las funciones stub en un contrato abstracto)

<<<<<<< HEAD
Sí::
=======
Yes:
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.6.0 <0.9.0;

    abstract contract A {
        function spam() public virtual pure;
        function ham() public virtual pure;
    }


    contract B is A {
        function spam() public pure override {
            // ...
        }

        function ham() public pure override {
            // ...
        }
    }

No:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.6.0 <0.9.0;

    abstract contract A {
        function spam() virtual pure public;
        function ham() public virtual pure;
    }


    contract B is A {
        function spam() public pure override {
            // ...
        }
        function ham() public pure override {
            // ...
        }
    }

<<<<<<< HEAD
Codificación de archivos de origen
==================================
=======
.. _maximum_line_length:

Maximum Line Length
===================

Keeping lines under the `PEP 8 recommendation <https://www.python.org/dev/peps/pep-0008/#maximum-line-length>`_ to a maximum of 79 (or 99)
characters helps readers easily parse the code.

Wrapped lines should conform to the following guidelines.

1. The first argument should not be attached to the opening parenthesis.
2. One, and only one, indent should be used.
3. Each argument should fall on its own line.
4. The terminating element, :code:`);`, should be placed on the final line by itself.

Function Calls

Yes:

.. code-block:: solidity

    thisFunctionCallIsReallyLong(
        longArgument1,
        longArgument2,
        longArgument3
    );

No:

.. code-block:: solidity

    thisFunctionCallIsReallyLong(longArgument1,
                                  longArgument2,
                                  longArgument3
    );

    thisFunctionCallIsReallyLong(longArgument1,
        longArgument2,
        longArgument3
    );

    thisFunctionCallIsReallyLong(
        longArgument1, longArgument2,
        longArgument3
    );

    thisFunctionCallIsReallyLong(
    longArgument1,
    longArgument2,
    longArgument3
    );

    thisFunctionCallIsReallyLong(
        longArgument1,
        longArgument2,
        longArgument3);

Assignment Statements

Yes:

.. code-block:: solidity

    thisIsALongNestedMapping[being][set][toSomeValue] = someFunction(
        argument1,
        argument2,
        argument3,
        argument4
    );

No:

.. code-block:: solidity

    thisIsALongNestedMapping[being][set][toSomeValue] = someFunction(argument1,
                                                                       argument2,
                                                                       argument3,
                                                                       argument4);

Event Definitions and Event Emitters

Yes:

.. code-block:: solidity

    event LongAndLotsOfArgs(
        address sender,
        address recipient,
        uint256 publicKey,
        uint256 amount,
        bytes32[] options
    );

    LongAndLotsOfArgs(
        sender,
        recipient,
        publicKey,
        amount,
        options
    );

No:

.. code-block:: solidity

    event LongAndLotsOfArgs(address sender,
                            address recipient,
                            uint256 publicKey,
                            uint256 amount,
                            bytes32[] options);

    LongAndLotsOfArgs(sender,
                      recipient,
                      publicKey,
                      amount,
                      options);

Source File Encoding
====================
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

Se prefiere la codificación del texto en UTF-8 o ASCII.

Importación
===========

Las declaraciones de importación siempre deben colocarse en la parte superior del archivo.

<<<<<<< HEAD
Sí::
=======
Yes:
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.0 <0.9.0;

    import "./Owned.sol";

    contract A {
        // ...
    }


    contract B is Owned {
        // ...
    }

No:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.0 <0.9.0;

    contract A {
        // ...
    }


    import "./Owned.sol";


    contract B is Owned {
        // ...
    }

Orden de funciones
==================

La ordenación ayuda a que los lectores puedan identificar las funciones que pueden invocar y encontrar las definiciones de constructor y de retorno más fácilmente.

Las funciones deben agruparse de acuerdo con su visibilidad y ser ordenadas de acuerdo a:

- constructor
<<<<<<< HEAD
- función fallback (Si existe)
=======
- receive function (if exists)
- fallback function (if exists)
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9
- external
- public
- internal
- private

<<<<<<< HEAD
Dentro de un grupo, coloque las funciones ``constant`` al final.

Sí::
=======
Within a grouping, place the ``view`` and ``pure`` functions last.

Yes:
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.7.0 <0.9.0;
    contract A {
        constructor() {
            // ...
        }

<<<<<<< HEAD
        function() {
            ...
        }

        // Funciones external
        // ...

        // Funciones external que son constantes
        // ...

        // Funciones public
        // ...

        // Funciones internal
        // ...

        // Funciones private
=======
        receive() external payable {
            // ...
        }

        fallback() external {
            // ...
        }

        // External functions
        // ...

        // External functions that are view
        // ...

        // External functions that are pure
        // ...

        // Public functions
        // ...

        // Internal functions
        // ...

        // Private functions
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9
        // ...
    }

No:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.7.0 <0.9.0;
    contract A {

<<<<<<< HEAD
        // Funciones external
        // ...

        // Funciones private
=======
        // External functions
        // ...

        fallback() external {
            // ...
        }
        receive() external payable {
            // ...
        }

        // Private functions
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9
        // ...

        // Funciones public
        // ...

<<<<<<< HEAD
        function A() {
            ...
        }

        function() {
            ...
        }

        // Funciones internal
        // ...       
=======
        constructor() {
            // ...
        }

        // Internal functions
        // ...
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9
    }

Espacios en blanco en expresiones
=================================

Evite los espacios en blanco superfluos en las siguientes situaciones:

<<<<<<< HEAD
Inmediatamente entre paréntesis, llaves o corchetes, con la excepción de declaraciones de una función en una sola línea.

Sí::
=======
Immediately inside parenthesis, brackets or braces, with the exception of single line function declarations.

Yes:

.. code-block:: solidity
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    spam(ham[1], Coin({name: "ham"}));

No:

.. code-block:: solidity

    spam( ham[ 1 ], Coin( { name: "ham" } ) );

<<<<<<< HEAD
Excepción::
=======
Exception:
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

.. code-block:: solidity

    function singleLine() public { spam(); }

Inmediatamente después de una coma, punto y coma:

<<<<<<< HEAD
Sí::
=======
Yes:
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

.. code-block:: solidity

    function spam(uint i, Coin coin) public;

No:

<<<<<<< HEAD
Más de un espacio alrededor de una asignación u otro operador para alinearlo con otro:

Sí::
=======
.. code-block:: solidity

    function spam(uint i , Coin coin) public ;

More than one space around an assignment or other operator to align with another:

Yes:

.. code-block:: solidity
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    x = 1;
    y = 2;
    longVariable = 3;

No:

.. code-block:: solidity

<<<<<<< HEAD
No incluya un espacio en blanco en la función fallback:

Sí::
=======
    x            = 1;
    y            = 2;
    longVariable = 3;

Don't include a whitespace in the receive and fallback functions:
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

Yes:

.. code-block:: solidity

    receive() external payable {
        ...
    }

<<<<<<< HEAD
No::

    function () {
        ...
    }

Estructuras de control
======================
=======
    fallback() external {
        ...
    }

No:

.. code-block:: solidity

    receive () external payable {
        ...
    }

    fallback () external {
        ...
    }


Control Structures
==================
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

Las llaves que denotan el cuerpo de un contrato, biblioteca, funciones y estructuras deberán:

<<<<<<< HEAD
* Abrir en la misma línea que la declaración
* Cerrar en su propia línea en el mismo nivel de sangría que la declaración.
* La llave de apertura debe ser procedida por un solo espacio.

Sí::
=======
* open on the same line as the declaration
* close on their own line at the same indentation level as the beginning of the
  declaration.
* The opening brace should be preceded by a single space.

Yes:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.0 <0.9.0;
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    contract Coin {
        struct Bank {
            address owner;
            uint balance;
        }
    }

No:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.0 <0.9.0;

    contract Coin
    {
        struct Bank {
            address owner;
            uint balance;
        }
    }

Las mismas recomendaciones se aplican a las estructuras de control ``if``, ``else``, ``while`` y ``for``.

Además, debería existir un único espacio entre las estructuras de control ``if``, ``while``, y ``for``, y el bloque entre paréntesis que representa el condicional, así como un único espacio entre el bloque del paréntesis condicional y la llave de apertura.

<<<<<<< HEAD
Sí::
=======
Yes:

.. code-block:: solidity
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    if (...) {
        ...
    }

    for (...) {
        ...
    }

No:

.. code-block:: solidity

    if (...)
    {
        ...
    }

    while(...){
    }

    for (...) {
        ...;}

Para las estructuras de control cuyo cuerpo sólo contiene declaraciones únicas, se pueden omitir los corchetes *si* la declaración cabe en una sola línea.

<<<<<<< HEAD
Sí::
=======
Yes:

.. code-block:: solidity
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    if (x < 10)
        x += 1;

No:

.. code-block:: solidity

    if (x < 10)
        someArray.push(Coin({
            name: 'spam',
            value: 42
        }));

Para los bloques ``if`` que contienen una condición ``else`` o ``else if``, el ``else`` debe estar en la misma línea que el corchete de cierre del ``if``. Esto es una excepción en comparación con las reglas de otras estructuras de tipo bloque.

<<<<<<< HEAD
Sí::
=======
Yes:

.. code-block:: solidity
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    if (x < 3) {
        x += 1;
    } else if (x > 7) {
        x -= 1;
    } else {
        x = 5;
    }


    if (x < 3)
        x += 1;
    else
        x -= 1;

No:

.. code-block:: solidity

    if (x < 3) {
        x += 1;
    }
    else {
        x -= 1;
    }

Declaración de funciones
========================

Para declaraciones de función cortas, se recomienda dejar el corchete de apertura del cuerpo de la función en la misma línea que la declaración de la función.

El corchete de cierre debe estar al mismo nivel de sangría que la declaración de la función.

<<<<<<< HEAD
El corchete de apertura debe estar precedido por un solo espacio.

Sí::
=======
The opening brace should be preceded by a single space.

Yes:
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

.. code-block:: solidity

    function increment(uint x) public pure returns (uint) {
        return x + 1;
    }

    function increment(uint x) public pure onlyOwner returns (uint) {
        return x + 1;
    }

No:

.. code-block:: solidity

    function increment(uint x) public pure returns (uint)
    {
        return x + 1;
    }

    function increment(uint x) public pure returns (uint){
        return x + 1;
    }

    function increment(uint x) public pure returns (uint) {
        return x + 1;
        }

    function increment(uint x) public pure returns (uint) {
        return x + 1;}

<<<<<<< HEAD
Se debe especificar la visibilidad de los modificadores para una función antes de cualquier modificador personalizado.

Sí::
=======
The modifier order for a function should be:

1. Visibility
2. Mutability
3. Virtual
4. Override
5. Custom modifiers
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

Yes:

.. code-block:: solidity

    function balance(uint from) public view override returns (uint)  {
        return balanceOf[from];
    }

    function shutdown() public onlyOwner {
        selfdestruct(owner);
    }

No:

.. code-block:: solidity

    function balance(uint from) public override view returns (uint)  {
        return balanceOf[from];
    }

    function shutdown() onlyOwner public {
        selfdestruct(owner);
    }

<<<<<<< HEAD
Para las declaraciones de función largas, se recomienda dejar a cada argumento su propia línea al mismo nivel de sangría que el cuerpo de la función. El paréntesis de cierre y el corchete de apertura deben de estar en su propia línea también y con el mismo nivel de sangría que la declaración de la función.

Sí::
=======
For long function declarations, it is recommended to drop each argument onto
its own line at the same indentation level as the function body.  The closing
parenthesis and opening bracket should be placed on their own line as well at
the same indentation level as the function declaration.

Yes:

.. code-block:: solidity
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    function thisFunctionHasLotsOfArguments(
        address a,
        address b,
        address c,
        address d,
        address e,
        address f
    )
        public
    {
        doSomething();
    }

No:

.. code-block:: solidity

    function thisFunctionHasLotsOfArguments(address a, address b, address c,
        address d, address e, address f) public {
        doSomething();
    }

    function thisFunctionHasLotsOfArguments(address a,
                                            address b,
                                            address c,
                                            address d,
                                            address e,
                                            address f) public {
        doSomething();
    }

    function thisFunctionHasLotsOfArguments(
        address a,
        address b,
        address c,
        address d,
        address e,
        address f) public {
        doSomething();
    }

<<<<<<< HEAD
Si una declaración de función larga tiene modificadores, cada uno de ellos debe de estar en su propia línea.

Sí::
=======
If a long function declaration has modifiers, then each modifier should be
dropped to its own line.

Yes:

.. code-block:: solidity
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    function thisFunctionNameIsReallyLong(address x, address y, address z)
        public
        onlyOwner
        priced
        returns (address)
    {
        doSomething();
    }

    function thisFunctionNameIsReallyLong(
        address x,
        address y,
        address z
    )
        public
        onlyOwner
        priced
        returns (address)
    {
        doSomething();
    }

No:

.. code-block:: solidity

    function thisFunctionNameIsReallyLong(address x, address y, address z)
                                          public
                                          onlyOwner
                                          priced
                                          returns (address) {
        doSomething();
    }

    function thisFunctionNameIsReallyLong(address x, address y, address z)
        public onlyOwner priced returns (address)
    {
        doSomething();
    }

    function thisFunctionNameIsReallyLong(address x, address y, address z)
        public
        onlyOwner
        priced
        returns (address) {
        doSomething();
    }

<<<<<<< HEAD
Para las funciones de tipo constructor en contratos heredados que requieren argumentos, si la declaración de la función es larga o difícil de leer, se recomienda poner cada constructor base en su propia línea de la misma manera que con los modificadores.

Sí::
=======
Multiline output parameters and return statements should follow the same style recommended for wrapping long lines found in the :ref:`Maximum Line Length <maximum_line_length>` section.

Yes:

.. code-block:: solidity

    function thisFunctionNameIsReallyLong(
        address a,
        address b,
        address c
    )
        public
        returns (
            address someAddressName,
            uint256 LongArgument,
            uint256 Argument
        )
    {
        doSomething()

        return (
            veryLongReturnArg1,
            veryLongReturnArg2,
            veryLongReturnArg3
        );
    }

No:

.. code-block:: solidity

    function thisFunctionNameIsReallyLong(
        address a,
        address b,
        address c
    )
        public
        returns (address someAddressName,
                 uint256 LongArgument,
                 uint256 Argument)
    {
        doSomething()

        return (veryLongReturnArg1,
                veryLongReturnArg1,
                veryLongReturnArg1);
    }

For constructor functions on inherited contracts whose bases require arguments,
it is recommended to drop the base constructors onto new lines in the same
manner as modifiers if the function declaration is long or hard to read.

Yes:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.7.0 <0.9.0;
    // Base contracts just to make this compile
    contract B {
        constructor(uint) {
        }
    }


    contract C {
        constructor(uint, uint) {
        }
    }


    contract D {
        constructor(uint) {
        }
    }

>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    contract A is B, C, D {
        uint x;

        constructor(uint param1, uint param2, uint param3, uint param4, uint param5)
            B(param1)
            C(param2, param3)
            D(param4)
        {
            // do something with param5
            x = param5;
        }
    }

No:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.7.0 <0.9.0;

    // Base contracts just to make this compile
    contract B {
        constructor(uint) {
        }
    }


    contract C {
        constructor(uint, uint) {
        }
    }


    contract D {
        constructor(uint) {
        }
    }


    contract A is B, C, D {
        uint x;

        constructor(uint param1, uint param2, uint param3, uint param4, uint param5)
        B(param1)
        C(param2, param3)
        D(param4) {
            x = param5;
        }
    }


    contract X is B, C, D {
        uint x;

        constructor(uint param1, uint param2, uint param3, uint param4, uint param5)
            B(param1)
            C(param2, param3)
            D(param4) {
                x = param5;
            }
    }

<<<<<<< HEAD
Cuando se declara funciones cortas con una sola declaración, está permitido hacerlo en una solo línea.

Permisible::
=======

When declaring short functions with a single statement, it is permissible to do it on a single line.

Permissible:
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

.. code-block:: solidity

    function shortFunction() public { doSomething(); }

<<<<<<< HEAD
Esta guía sobre la declaración de funciones está pensada para mejorar la legibilidad. Sin embargo, los autores deberían utilizar su mejor juicio, ya que esta guía tampoco intenta cubrir todas las posibles permutaciones para las declaraciones de función.
=======
These guidelines for function declarations are intended to improve readability.
Authors should use their best judgment as this guide does not try to cover all
possible permutations for function declarations.
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

Mapeo
=====

<<<<<<< HEAD
Pendiente de hacer
=======
In variable declarations, do not separate the keyword ``mapping`` from its
type by a space. Do not separate any nested ``mapping`` keyword from its type by
whitespace.

Yes:

.. code-block:: solidity

    mapping(uint => uint) map;
    mapping(address => bool) registeredAddresses;
    mapping(uint => mapping(bool => Data[])) public data;
    mapping(uint => mapping(uint => s)) data;

No:

.. code-block:: solidity

    mapping (uint => uint) map;
    mapping( address => bool ) registeredAddresses;
    mapping (uint => mapping (bool => Data[])) public data;
    mapping(uint => mapping (uint => s)) data;
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

Declaración de variables
========================

La declaración de variables tipo array no deben incluir un espacio entre el tipo y el corchete.

<<<<<<< HEAD
Sí::
=======
Yes:

.. code-block:: solidity
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    uint[] x;

No:

.. code-block:: solidity

    uint [] x;


Otras recommendaciones
======================

* Los strings deben de citarse con comillas dobles en lugar de comillas simples.

<<<<<<< HEAD
Sí::
=======
Yes:

.. code-block:: solidity
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    str = "foo";
    str = "Hamlet says, 'To be or not to be...'";

No:

.. code-block:: solidity

    str = 'bar';
    str = '"Be yourself; everyone else is already taken." -Oscar Wilde';

* Se envuelve los operadores con un solo espacio de cada lado.

<<<<<<< HEAD
Sí::
=======
Yes:

.. code-block:: solidity
    :force:
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    x = 3;
    x = 100 / 10;
    x += 3 + 4;
    x |= y && z;

No:

.. code-block:: solidity
    :force:

    x=3;
    x = 100/10;
    x += 3+4;
    x |= y&&z;

<<<<<<< HEAD
* Para los operadores con una prioridad mayor que otros, se pueden omitir los espacios de cada lado del operador para marcar la precedencia. Esto se hace para mejorar la legibilidad de declaraciones complejas. Se debe usar siempre el mismo número de espacios a cada lado de un operador.

Sí::
=======
* Operators with a higher priority than others can exclude surrounding
  whitespace in order to denote precedence.  This is meant to allow for
  improved readability for complex statements. You should always use the same
  amount of whitespace on either side of an operator:

Yes:

.. code-block:: solidity
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

    x = 2**3 + 5;
    x = 2*y + 3*z;
    x = (a+b) * (a-b);

No:

.. code-block:: solidity

    x = 2** 3 + 5;
    x = y+z;
    x +=1;

***************
Order of Layout
***************

Layout contract elements in the following order:

1. Pragma statements
2. Import statements
3. Interfaces
4. Libraries
5. Contracts

Inside each contract, library or interface, use the following order:

1. Type declarations
2. State variables
3. Events
4. Modifiers
5. Functions

.. note::

    It might be clearer to declare types close to their use in events or state
    variables.

************************
Convención sobre nombres
************************

Las convenciones sobre nombres son extremadamente útiles siempre y cuando se usen de forma ámplia. El uso de diferentes convenciones puede transmitir *meta* información significativa a la que de otro modo no tendríamos acceso de manera inmediata.

Las recomendaciones de nombres que se dan aquí están pensadas para mejorar la legibilidad, y por lo tanto no se deben considerar como reglas. Son más bien una guía para intentar transmitir la mayor información posible a través del nombre de las cosas.

<<<<<<< HEAD
Finalmente, la consistencia dentro de un bloque de código siempre debe prevalecer sobre cualquier convención destacada en este documento.


Estilos para poner nombres
==========================

Para evitar confusiones, se usarán los siguiente nombres para referirse a diferentes estilos para poner nombres.

* ``b`` (letra minúscula única)
* ``B`` (letra mayúscula única)
* ``minuscula``
* ``minuscula_con_guiones_bajos``
* ``MAYUSCULA``
* ``MAYUSCULA_CON_GUIONES_BAJOS``
* ``PalabrasConLaInicialEnMayuscula`` (también llamado CapWords)
* ``mezclaEntreMinusculaYMayuscula`` (¡distinto a PalabrasConLaInicialEnMayuscula por el uso de una minúscula en la letra inicial!)
* ``Palabras_Con_La_Inicial_En_Mayuscula_Y_Guiones_Bajos``

.. note:: Cuando se usan abreviaciones en CapWords, usar mayúsculas para todas las letras de la abreviación. Es decir que HTTPServerError es mejor que HttpServerError


Nombres a evitar
================

* ``l`` - Letra minúscula ele
* ``O`` - Letra mayúscula o
* ``I`` - Letra mayúscula i

No usar jamás ninguna de estas letras únicas para nombrar una variable. Estas letras generalmente no se diferencian de los dígitos uno y cero.
=======
Lastly, consistency within a codebase should always supersede any conventions
outlined in this document.


Naming Styles
=============

To avoid confusion, the following names will be used to refer to different
naming styles.

* ``b`` (single lowercase letter)
* ``B`` (single uppercase letter)
* ``lowercase``
* ``UPPERCASE``
* ``UPPER_CASE_WITH_UNDERSCORES``
* ``CapitalizedWords`` (or CapWords)
* ``mixedCase`` (differs from CapitalizedWords by initial lowercase character!)

.. note:: When using initialisms in CapWords, capitalize all the letters of the initialisms. Thus HTTPServerError is better than HttpServerError. When using initialisms in mixedCase, capitalize all the letters of the initialisms, except keep the first one lower case if it is the beginning of the name. Thus xmlHTTPRequest is better than XMLHTTPRequest.


Names to Avoid
==============

* ``l`` - Lowercase letter el
* ``O`` - Uppercase letter oh
* ``I`` - Uppercase letter eye

Never use any of these for single letter variable names.  They are often
indistinguishable from the numerals one and zero.


Contract and Library Names
==========================

* Contracts and libraries should be named using the CapWords style. Examples: ``SimpleToken``, ``SmartBank``, ``CertificateHashRepository``, ``Player``, ``Congress``, ``Owned``.
* Contract and library names should also match their filenames.
* If a contract file includes multiple contracts and/or libraries, then the filename should match the *core contract*. This is not recommended however if it can be avoided.

As shown in the example below, if the contract name is ``Congress`` and the library name is ``Owned``, then their associated filenames should be ``Congress.sol`` and ``Owned.sol``.

Yes:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.7.0 <0.9.0;

    // Owned.sol
    contract Owned {
        address public owner;

        constructor() {
            owner = msg.sender;
        }

        modifier onlyOwner {
            require(msg.sender == owner);
            _;
        }

        function transferOwnership(address newOwner) public onlyOwner {
            owner = newOwner;
        }
    }

and in ``Congress.sol``:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.0 <0.9.0;

    import "./Owned.sol";


    contract Congress is Owned, TokenRecipient {
        //...
    }

No:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.7.0 <0.9.0;

    // owned.sol
    contract owned {
        address public owner;

        constructor() {
            owner = msg.sender;
        }

        modifier onlyOwner {
            require(msg.sender == owner);
            _;
        }

        function transferOwnership(address newOwner) public onlyOwner {
            owner = newOwner;
        }
    }

and in ``Congress.sol``:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity ^0.7.0;


    import "./owned.sol";


    contract Congress is owned, tokenRecipient {
        //...
    }

Struct Names
==========================

Structs should be named using the CapWords style. Examples: ``MyCoin``, ``Position``, ``PositionXY``.


Event Names
===========

Events should be named using the CapWords style. Examples: ``Deposit``, ``Transfer``, ``Approval``, ``BeforeTransfer``, ``AfterTransfer``.
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9


Contratos y librerías de nombres
================================

<<<<<<< HEAD
Contratos y librerías deben de nombrarse usando el estilo CapWord (PalabrasConLaInicialEnMayuscula).


Eventos
=======
=======
Functions should use mixedCase. Examples: ``getBalance``, ``transfer``, ``verifyOwner``, ``addMember``, ``changeOwner``.


Function Argument Names
=======================

Function arguments should use mixedCase. Examples: ``initialSupply``, ``account``, ``recipientAddress``, ``senderAddress``, ``newOwner``.
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9

Los eventos deben de nombrarse usando el estilo CapWord (PalabrasConLaInicialEnMayuscula).


<<<<<<< HEAD
Nombres para funciones
======================

Las funciones deben de nombrarse usando el estilo mezclaEntreMinusculaYMayuscula.


Argumentos de funciones
=======================

Cuando se escriben funciones de librerías que operan sobre un struct personalizado, el struct debe ser el primer argumento y debe nombrarse siempre ``self``.


Variables locales y de estado
=============================

Usar el estilo mezclaEntreMinusculaYMayuscula.


Constantes
==========

Las constantes deben de nombrarse con todas las letras mayúsculas y guiones bajos para separar las palabras (p.ej. ``MAX_BLOCKS``).


Modificadores
=============

Usar el estilo mezclaEntreMinusculaYMayuscula.


Evitar Colisiones
=================

* ``guion_bajo_unico_con_cola_``

Se recomienda usar esta convención cuando el nombre deseado colisiona con un nombre inherente al sistema o reservado.


Recommendaciones generales
==========================

Pendiente de hacer
=======
Local and State Variable Names
==============================

Use mixedCase. Examples: ``totalSupply``, ``remainingSupply``, ``balancesOf``, ``creatorAddress``, ``isPreSale``, ``tokenExchangeRate``.


Constants
=========

Constants should be named with all capital letters with underscores separating
words. Examples: ``MAX_BLOCKS``, ``TOKEN_NAME``, ``TOKEN_TICKER``, ``CONTRACT_VERSION``.


Modifier Names
==============

Use mixedCase. Examples: ``onlyBy``, ``onlyAfter``, ``onlyDuringThePreSale``.


Enums
=====

Enums, in the style of simple type declarations, should be named using the CapWords style. Examples: ``TokenGroup``, ``Frame``, ``HashStyle``, ``CharacterLocation``.


Avoiding Naming Collisions
==========================

* ``singleTrailingUnderscore_``

This convention is suggested when the desired name collides with that of a
built-in or otherwise reserved name.

.. _style_guide_natspec:

*******
NatSpec
*******

Solidity contracts can also contain NatSpec comments. They are written with a
triple slash (``///``) or a double asterisk block (``/** ... */``) and
they should be used directly above function declarations or statements.

For example, the contract from :ref:`a simple smart contract <simple-smart-contract>` with the comments
added looks like the one below:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.16 <0.9.0;

    /// @author The Solidity Team
    /// @title A simple storage example
    contract SimpleStorage {
        uint storedData;

        /// Store `x`.
        /// @param x the new value to store
        /// @dev stores the number in the state variable `storedData`
        function set(uint x) public {
            storedData = x;
        }

        /// Return the stored value.
        /// @dev retrieves the value of the state variable `storedData`
        /// @return the stored value
        function get() public view returns (uint) {
            return storedData;
        }
    }

It is recommended that Solidity contracts are fully annotated using :ref:`NatSpec <natspec>` for all public interfaces (everything in the ABI).

Please see the section about :ref:`NatSpec <natspec>` for a detailed explanation.
>>>>>>> d0bd365d2c7e43b84f685dcdddac944fb0e286d9
