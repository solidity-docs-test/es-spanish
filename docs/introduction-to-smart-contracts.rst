#########################################
Introducción a los Contratos Inteligentes
#########################################

.. _simple-smart-contract:

******************************
Un contrato inteligente simple
******************************

<<<<<<< HEAD
Vamos a comenzar con el ejemplo más básico. No pasa nada si no entiendes nada ahora, entraremos más en detalle posteriormente.

Almacenamiento
==============
=======
Let us begin with a basic example that sets the value of a variable and exposes
it for other contracts to access. It is fine if you do not understand
everything right now, we will go into more details later.

Storage Example
===============
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.16 <0.9.0;

    contract SimpleStorage {
        uint storedData;

        function set(uint x) public {
            storedData = x;
        }

        function get() public view returns (uint) {
            return storedData;
        }
    }

<<<<<<< HEAD
La primera línea simplemente dice que el código fuente se ha escrito para la versión 0.4.0 de Solidity o en otra superior totalmente compatible (cualquiera anterior a la 0.5.0). Esto es para garantizar que el contrato no se comporte de una forma diferente con una versión más nueva del compilador. La palabra reservada ``pragma`` es llamada de esa manera porque, en general, los "pragmas" son instrucciones para el compilador que indican como este debe operar con el código fuente (p.ej.: `pragma once <https://en.wikipedia.org/wiki/Pragma_once>`_).  .

Un contrato para Solidity es una colección de código (sus *funciones*) y datos (su *estado*) que residen en una dirección específica en la blockchain de Ethereum. La línea ``uint storedData;`` declara una variable de estado llamada ``storedData`` del tipo ``uint`` (unsigned integer de 256 bits). Esta se puede entender como una parte única en una base de datos que puede ser consultada o modificada llamando a funciones del código que gestiona dicha base de datos. En el caso de Ethereum, este es siempre el contrato propietario. Y en este caso, las funciones ``set`` y ``get`` se pueden usar para modificar o consultar el valor de la variable.

Para acceder a una variable de estado, no es necesario el uso del prefijo ``this.`` como es habitual en otros lenguajes.

Este contrato no hace mucho todavía (debido a la infraestructura construída por Ethereum), simplemente permite a cualquiera almacenar un número accesible para todos sin un (factible) modo de prevenir la posibilidad de publicar este número. Por supuesto, cualquiera podría simplemente hacer una llamada ``set`` de nuevo con un valor diferente y sobrescribir el número inicial, pero este número siempre permanecería almacenado en la historía de la blockchain. Más adelante, veremos como imponer restricciones de acceso de tal manera que sólo tú puedas cambiar el número.

.. index:: ! submoneda
=======
The first line tells you that the source code is licensed under the
GPL version 3.0. Machine-readable license specifiers are important
in a setting where publishing the source code is the default.

The next line specifies that the source code is written for
Solidity version 0.4.16, or a newer version of the language up to, but not including version 0.9.0.
This is to ensure that the contract is not compilable with a new (breaking) compiler version, where it could behave differently.
:ref:`Pragmas<pragma>` are common instructions for compilers about how to treat the
source code (e.g. `pragma once <https://en.wikipedia.org/wiki/Pragma_once>`_).

A contract in the sense of Solidity is a collection of code (its *functions*) and
data (its *state*) that resides at a specific address on the Ethereum
blockchain. The line ``uint storedData;`` declares a state variable called ``storedData`` of
type ``uint`` (*u*\nsigned *int*\eger of *256* bits). You can think of it as a single slot
in a database that you can query and alter by calling functions of the
code that manages the database. In this example, the contract defines the
functions ``set`` and ``get`` that can be used to modify
or retrieve the value of the variable.

To access a member (like a state variable) of the current contract, you do not typically add the ``this.`` prefix,
you just access it directly via its name.
Unlike in some other languages, omitting it is not just a matter of style,
it results in a completely different way to access the member, but more on this later.

This contract does not do much yet apart from (due to the infrastructure
built by Ethereum) allowing anyone to store a single number that is accessible by
anyone in the world without a (feasible) way to prevent you from publishing
this number. Anyone could call ``set`` again with a different value
and overwrite your number, but the number is still stored in the history
of the blockchain. Later, you will see how you can impose access restrictions
so that only you can alter the number.

.. warning::
    Be careful with using Unicode text, as similar looking (or even identical) characters can
    have different code points and as such are encoded as a different byte array.

.. note::
    All identifiers (contract names, function names and variable names) are restricted to
    the ASCII character set. It is possible to store UTF-8 encoded data in string variables.

.. index:: ! subcurrency
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

Ejemplo de Submoneda
====================

<<<<<<< HEAD
El siguiente contrato va a implementar la forma más sencilla de una criptomoneda. Se pueden generar monedas de la nada, pero sólo la persona que creó el contrato estará habilitada para hacerlo (es trivial implementar un esquema diferente de emisión). Es más, cualquiera puede enviar monedas a otros sin necesidad de registrarse con usuario y contraseña - sólo hace falta un par de claves de Ethereum.
=======
The following contract implements the simplest form of a
cryptocurrency. The contract allows only its creator to create new coins (different issuance schemes are possible).
Anyone can send coins to each other without a need for
registering with a username and password, all you need is an Ethereum keypair.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity ^0.8.4;

    contract Coin {
<<<<<<< HEAD
        // La palabra clave "public" hace que dichas variables
        // puedan ser leídas desde fuera.
        address public minter;
        mapping (address => uint) public balances;
        
        // Los eventos permiten a los clientes ligeros reaccionar
        // de forma eficiente a los cambios.
        event Sent(address from, address to, uint amount);

        // Este es el constructor cuyo código
        // sólo se ejecutará cuando se cree el contrato.
        function Coin() {
=======
        // The keyword "public" makes variables
        // accessible from other contracts
        address public minter;
        mapping (address => uint) public balances;

        // Events allow clients to react to specific
        // contract changes you declare
        event Sent(address from, address to, uint amount);

        // Constructor code is only run when the contract
        // is created
        constructor() {
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574
            minter = msg.sender;
        }

        // Sends an amount of newly created coins to an address
        // Can only be called by the contract creator
        function mint(address receiver, uint amount) public {
            require(msg.sender == minter);
            balances[receiver] += amount;
        }

        // Errors allow you to provide information about
        // why an operation failed. They are returned
        // to the caller of the function.
        error InsufficientBalance(uint requested, uint available);

        // Sends an amount of existing coins
        // from any caller to an address
        function send(address receiver, uint amount) public {
            if (amount > balances[msg.sender])
                revert InsufficientBalance({
                    requested: amount,
                    available: balances[msg.sender]
                });

            balances[msg.sender] -= amount;
            balances[receiver] += amount;
            emit Sent(msg.sender, receiver, amount);
        }
    }

Este contrato introduce algunos conceptos nuevos que vamos a detallar uno a uno.

<<<<<<< HEAD
La línea ``address public minter;`` declara una variable de estado de tipo address (dirección) que es públicamente accesible. El tipo ``address`` es un valor de 160 bits que no permite operaciones aritméticas. Es apropiado para almacenar direcciones de contratos o pares de claves pertenecientes a personas externas. La palabra reservada ``public`` genera automáticamente una función que permite el acceso al valor actual de la variable de estado. Sin esta palabra reservada, otros contratos no tienen manera de acceder a la variable.
Esta función sería algo como esto::
=======
The line ``address public minter;`` declares a state variable of type :ref:`address<address>`.
The ``address`` type is a 160-bit value that does not allow any arithmetic operations.
It is suitable for storing addresses of contracts, or a hash of the public half
of a keypair belonging to :ref:`external accounts<accounts>`.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

The keyword ``public`` automatically generates a function that allows you to access the current value of the state
variable from outside of the contract. Without this keyword, other contracts have no way to access the variable.
The code of the function generated by the compiler is equivalent
to the following (ignore ``external`` and ``view`` for now):

<<<<<<< HEAD
Por supuesto, añadir una función exactamente como esa no funcionará porque deberíamos tener una función y una variable de estado con el mismo nombre, pero afortunadamente, has cogido la idea - el compilador se lo imagina por tí.

.. index:: mapping

La siguiente línea, ``mapping (address => uint) public balances;`` también crea una variable de estado pública, pero se trata de un tipo de datos más complejo. El tipo mapea direcciones a enteros sin signo.
Los mapeos (Mappings) pueden ser vistos como tablas hash `hash tables <https://en.wikipedia.org/wiki/Hash_table>`_ que son virtualmente inicializadas de tal forma que cada clave candidata existe y es mapeada a un valor cuya representación en bytes es todo ceros. 
Esta anología no va mucho más allá, ya que no es posible obtener una lista de todas las claves de un mapeo, ni tampoco una lista de todos los valores. Por eso hay que tener en cuenta (o mejor, conservar una lista o usar un tipo de datos más avanzado) lo que se añade al mapping o usarlo en un contexto donde no es necesario, como este caso. La función getter creada mediante la palabra reservada ``public`` es un poco más compleja en este caso. De forma aproximada, es algo parecido a lo siguiente::
=======
.. code-block:: solidity

    function minter() external view returns (address) { return minter; }

You could add a function like the above yourself, but you would have a function and state variable with the same name.
You do not need to do this, the compiler figures it out for you.

.. index:: mapping

The next line, ``mapping (address => uint) public balances;`` also
creates a public state variable, but it is a more complex datatype.
The :ref:`mapping <mapping-types>` type maps addresses to :ref:`unsigned integers <integers>`.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

Mappings can be seen as `hash tables <https://en.wikipedia.org/wiki/Hash_table>`_ which are
virtually initialised such that every possible key exists from the start and is mapped to a
value whose byte-representation is all zeros. However, it is neither possible to obtain a list of all keys of
a mapping, nor a list of all values. Record what you
added to the mapping, or use it in a context where this is not needed. Or
even better, keep a list, or use a more suitable data type.

The :ref:`getter function<getter-functions>` created by the ``public`` keyword
is more complex in the case of a mapping. It looks like the
following:

.. code-block:: solidity

    function balances(address account) external view returns (uint) {
        return balances[account];
    }

<<<<<<< HEAD
Como se puede ver, se puede usar esta función para, de forma sencilla, consultar el balance de una única cuenta.

.. index:: event

La línea ``event Sent(address from, address to, uint amount);`` declara un evento que es disparado en la última línea de la ejecución de 
``send``. Las interfaces de usuario (como las de servidor, por supuesto) pueden escuchar esos eventos que están siendo disparados en la blockchain sin mucho coste. Tan pronto son disparados, el listener también recibirá los argumentos ``from``, ``to`` y ``amount``, que hacen más fácil trazar las transacciones. Con el fin de escuchar este evento, se podría usar ::
=======
You can use this function to query the balance of a single account.

.. index:: event

The line ``event Sent(address from, address to, uint amount);`` declares
an :ref:`"event" <events>`, which is emitted in the last line of the function
``send``. Ethereum clients such as web applications can
listen for these events emitted on the blockchain without much
cost. As soon as it is emitted, the listener receives the
arguments ``from``, ``to`` and ``amount``, which makes it possible to track
transactions.

To listen for this event, you could use the following
JavaScript code, which uses `web3.js <https://github.com/ethereum/web3.js/>`_ to create the ``Coin`` contract object,
and any user interface calls the automatically generated ``balances`` function from above::
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

    Coin.Sent().watch({}, '', function(error, result) {
        if (!error) {
            console.log("Coin transfer: " + result.args.amount +
                " coins were sent from " + result.args.from +
                " to " + result.args.to + ".");
            console.log("Balances now:\n" +
                "Sender: " + Coin.balances.call(result.args.from) +
                "Receiver: " + Coin.balances.call(result.args.to));
        }
    })

<<<<<<< HEAD
Es interesante como la función generada automáticamente ``balances`` es llamada desde la interfaz de usuario.

.. index:: coin

La función especial ``Coin`` es el constructor que se ejecuta durante la creación de un contrato y no puede ser llamada con posterioridad. Almacena permanentemente la dirección de la persona que crea el contrato: ``msg`` (junto con ``tx`` y ``block``) es una variable global mágica que contiene propiedades que permiten el acceso a la blockchain. ``msg.sender`` es siempre la dirección desde donde se origina la llamada a la función actual (externa).

Finalmente, las funciones que realmente habrá en el contrato y que podrán ser llamadas por usuarios y contratos como son ``mint`` y ``send``. Si se llama a ``mint`` desde una cuenta distinta a la del creador del contrato, no ocurrirá nada. Por otro lado, ``send`` puede ser usado por todos (los que ya tienen algunas de estas monedas) para enviar monedas a cualquier otro. Hay que tener en cuenta que si se usa este contrato para enviar monedas a una dirección, no se verá reflejado cuando se busque la dirección en un explorador de la blockchain por el hecho de enviar monedas, y que los balances sólo serán guardados en el almacenamiento de este contrato de moneda. Con el uso de eventos es relativamente sencillo crear un "explorador de la blockchain" que monitorice las transacciones y los balances de la nueva moneda.
=======
.. index:: coin

The :ref:`constructor<constructor>` is a special function that is executed during the creation of the contract and
cannot be called afterwards. In this case, it permanently stores the address of the person creating the
contract. The ``msg`` variable (together with ``tx`` and ``block``) is a
:ref:`special global variable <special-variables-functions>` that
contains properties which allow access to the blockchain. ``msg.sender`` is
always the address where the current (external) function call came from.

The functions that make up the contract, and that users and contracts can call are ``mint`` and ``send``.

The ``mint`` function sends an amount of newly created coins to another address. The :ref:`require
<assert-and-require>` function call defines conditions that reverts all changes if not met. In this
example, ``require(msg.sender == minter);`` ensures that only the creator of the contract can call
``mint``. In general, the creator can mint as many tokens as they like, but at some point, this will
lead to a phenomenon called "overflow". Note that because of the default :ref:`Checked arithmetic
<unchecked>`, the transaction would revert if the expression ``balances[receiver] += amount;``
overflows, i.e., when ``balances[receiver] + amount`` in arbitrary precision arithmetic is larger
than the maximum value of ``uint`` (``2**256 - 1``). This is also true for the statement
``balances[receiver] += amount;`` in the function ``send``.

:ref:`Errors <errors>` allow you to provide more information to the caller about
why a condition or operation failed. Errors are used together with the
:ref:`revert statement <revert-statement>`. The ``revert`` statement unconditionally
aborts and reverts all changes similar to the ``require`` function, but it also
allows you to provide the name of an error and additional data which will be supplied to the caller
(and eventually to the front-end application or block explorer) so that
a failure can more easily be debugged or reacted upon.

The ``send`` function can be used by anyone (who already
has some of these coins) to send coins to anyone else. If the sender does not have
enough coins to send, the ``if`` condition evaluates to true. As a result, the ``revert`` will cause the operation to fail
while providing the sender with error details using the ``InsufficientBalance`` error.

.. note::
    If you use
    this contract to send coins to an address, you will not see anything when you
    look at that address on a blockchain explorer, because the record that you sent
    coins and the changed balances are only stored in the data storage of this
    particular coin contract. By using events, you can create
    a "blockchain explorer" that tracks transactions and balances of your new coin,
    but you have to inspect the coin contract address and not the addresses of the
    coin owners.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. _blockchain-basics:

*************************
Fundamentos de Blockchain
*************************

<<<<<<< HEAD
Las blockchains son un concepto no muy difícil de entender para desarrolladores. La razón es que la mayoría de las complicaciones (minería, `hashes <https://es.wikipedia.org/wiki/Funci%C3%B3n_hash_criptogr%C3%A1fica>`_, `criptografa de curva elíptica <https://es.wikipedia.org/wiki/Criptograf%C3%ADa_de_curva_el%C3%ADptica>`_, `redes P2P <https://es.wikipedia.org/wiki/Peer-to-peer>`_, etc.) están justo ahí para proveer un conjunto de funcionalidades y espectativas. Una vez que aceptas estas funcionalidades tal cual vienen dadas, no tienes que preocuparte por la tecnología que lleva inmersa - o, ¿Tienes que saber realmente cómo funciona internamente Amazon AWS para poder usarlo?.
=======
Blockchains as a concept are not too hard to understand for programmers. The reason is that
most of the complications (mining, `hashing <https://en.wikipedia.org/wiki/Cryptographic_hash_function>`_,
`elliptic-curve cryptography <https://en.wikipedia.org/wiki/Elliptic_curve_cryptography>`_,
`peer-to-peer networks <https://en.wikipedia.org/wiki/Peer-to-peer>`_, etc.)
are just there to provide a certain set of features and promises for the platform. Once you accept these
features as given, you do not have to worry about the underlying technology - or do you have
to know how Amazon's AWS works internally in order to use it?
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. index:: transaction

Transacciones
=============

<<<<<<< HEAD
Una blockchain es una base de datos transaccional globalmente compartida. Esto quiere decir que todos pueden leer las entradas en la base de datos simplemente participando en la red. Si quieres cambiar algo en la base de datos, tienes que crear una transacción a tal efecto que tiene que ser aceptada por todos los demás.
La palabra transacción implica que el cambio que quieres hacer (asumiendo que quieres cambiar dos valores al mismo tiempo) o se aplica por completo, o no se realiza. Es más, mientras tu transacción es aplicada en la base de datos, ninguna otra transacción puede modificarla. 
=======
A blockchain is a globally shared, transactional database.
This means that everyone can read entries in the database just by participating in the network.
If you want to change something in the database, you have to create a so-called transaction
which has to be accepted by all others.
The word transaction implies that the change you want to make (assume you want to change
two values at the same time) is either not done at all or completely applied. Furthermore,
while your transaction is being applied to the database, no other transaction can alter it.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

Como ejemplo, imagine una tabla que lista los balances de todas las cuentas en una divisa electrónica. Si se solicita una transferencia de una cuenta a otra, la naturaleza transaccional de la base de datos garantiza que la cantidad que es sustraída de una cuenta, es añadida en la otra. Si por la razón que sea, no es posible añadir la cantidad a la cuenta de destino, la cuenta origen tampoco se modifica. 

Yendo más allá, una transacción es siempre firmada criptográficamente por el remitente (creador). Esto la hace más robusta para garantizar el acceso a modificaciones específicas de la base de datos. En el ejemplo de divisas electrónicas, un simple chequeo asegura que sólo la persona que posee las claves de la cuenta puede transferir dinero desde ella.

.. index:: ! block

Bloques
=======

<<<<<<< HEAD
Un obstáculo mayor que sobrepasar es el que, en términos de Bitcoin, se llama ataque de "doble gasto": ¿Qué ocurre si dos transacciones existentes en la red quieren borrar una cuenta?, ¿Un conflicto?.

La respuesta abstracta a esto es que no tienes de qué preocuparte. El orden de las transacciones se seleccionará por ti, las transacciones se aglutinarán en lo que es llamado "bloque" y entonces serán ejecutadas y distribuídas entre todos los nodos participantes. Si dos transacciones se contradicen, la que concluye en segundo lugar será rechazada y no formará parte del bloque.
=======
One major obstacle to overcome is what (in Bitcoin terms) is called a "double-spend attack":
What happens if two transactions exist in the network that both want to empty an account?
Only one of the transactions can be valid, typically the one that is accepted first.
The problem is that "first" is not an objective term in a peer-to-peer network.

The abstract answer to this is that you do not have to care. A globally accepted order of the transactions
will be selected for you, solving the conflict. The transactions will be bundled into what is called a "block"
and then they will be executed and distributed among all participating nodes.
If two transactions contradict each other, the one that ends up being second will
be rejected and not become part of the block.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

Estos bloques forman una secuencia lineal en el tiempo de la que viene la palabra cadena de bloques o "blockchain". Los bloques son añadidos a la cadena en intervalos regulares - para Ethereum esto viene a significar cada 17 segundos.

<<<<<<< HEAD
Como parte del "mecanismo de selección de orden" (que se conoce como minería), tiene que pasar que los bloques sean revertidos de cuando en cuando, pero sólo en el extremo o "tip" de la cadena. Cuantos más bloques se añaden encima, menos probable es. En ese caso, lo que ocurre es que tus transacciones son revertidas e incluso borradas de la blockchain, pero cuanto más esperes, menos probable será.
=======
As part of the "order selection mechanism" (which is called "mining") it may happen that
blocks are reverted from time to time, but only at the "tip" of the chain. The more
blocks are added on top of a particular block, the less likely this block will be reverted. So it might be that your transactions
are reverted and even removed from the blockchain, but the longer you wait, the less
likely it will be.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. note::
    Transactions are not guaranteed to be included in the next block or any specific future block,
    since it is not up to the submitter of a transaction, but up to the miners to determine in which block the transaction is included.

    If you want to schedule future calls of your contract, you can use
    a smart contract automation tool or an oracle service.

.. _the-ethereum-virtual-machine:

.. index:: !evm, ! ethereum virtual machine

***************************
Máquina Virtual de Ethereum
***************************

Introducción
============

La máquina virtual de Ethereum (EVM por sus siglas en inglés) es un entorno de ejecución de contratos inteligentes en Ethereum. Va más allá de una configuración tipo sandbox ya que se encuentra totalmente aislada, lo que significa que el código que se ejecuta en la EVM no tiene acceso a la red, ni al sistema de ficheros, ni a ningún otro proceso. Incluso los contratos inteligentes tienen acceso limitado a otros contratos inteligentes.

.. index:: ! account, address, storage, balance

<<<<<<< HEAD
Cuentas
=======
=======
.. _accounts:

Accounts
========
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

Hay dos tipos de cuentas en Ethereum que comparten el mismo espacio de dirección: **Cuentas externas** que están controladas por un par de claves pública-privada (p-ej.: humanos) y **Cuentas contrato** que están controladas por el código almacenado conjuntamente con la cuenta.

La dirección de una cuenta externa viene determinada por la clave pública mientras que la dirección de la cuenta contrato se define en el momento en que se crea dicho contrato (se deriva de la dirección del creador y del número de transacciones enviadas desde esa dirección, el llamado "nonce").

Independientemente de que la cuenta almacene código, los dos tipos se tratan de forma equitativa por la EVM.

Cada cuenta tiene un almacenamiento persistente clave-valor que mapea palabras de 256 bits a palabras de 256 bits llamado **almacenamiento**.

<<<<<<< HEAD
Además, cada cuenta tiene un **balance** en Ether (en "Wei" para ser exactos) que puede ser modificado enviando transacciones que incluyen Ether.
=======
Furthermore, every account has a **balance** in
Ether (in "Wei" to be exact, ``1 ether`` is ``10**18 wei``) which can be modified by sending transactions that
include Ether.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. index:: ! transaction

Transacciones
=============

<<<<<<< HEAD
Una transacción es un mensaje que se envía de una cuenta a otra (que debería ser la misma o la especial cuenta-cero, ver más adelante). Puede incluir datos binarios (payload) y Ether.
=======
A transaction is a message that is sent from one account to another
account (which might be the same or empty, see below).
It can include binary data (which is called "payload") and Ether.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

Si la cuenta destino contiene código, este es ejecutado y el payload se provee como dato de entrada.

<<<<<<< HEAD
Si la cuenta destino es la cuenta-cero (la cuenta con dirección ``0``), la transacción crea un **nuevo contrato**. Como se ha mencionado, la dirección del contrato no es la dirección cero, sino de una dirección derivada del emisor y su número de transacciones enviadas (el "nonce"). Los datos binarios de la transacción que crea el contrato son obtenidos como bytecode por la EVM y ejecutados. La salida de esta ejecución es permanentemente almacenada como el código del contrato. Esto significa que para crear un contrato, no se envía el código actual del contrato, realmente se envía código que nos devuelve ese código final.
=======
If the target account is not set (the transaction does not have
a recipient or the recipient is set to ``null``), the transaction
creates a **new contract**.
As already mentioned, the address of that contract is not
the zero address but an address derived from the sender and
its number of transactions sent (the "nonce"). The payload
of such a contract creation transaction is taken to be
EVM bytecode and executed. The output data of this execution is
permanently stored as the code of the contract.
This means that in order to create a contract, you do not
send the actual code of the contract, but in fact code that
returns that code when executed.

.. note::
  While a contract is being created, its code is still empty.
  Because of that, you should not call back into the
  contract under construction until its constructor has
  finished executing.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. index:: ! gas, ! gas price

Gas
===

<<<<<<< HEAD
En cuanto se crean, cada transacción se carga con una determinada cantidad de **gas**,
cuyo propósito es limitar la cantidad de trabajo que se necesita para ejecutar la transacción y pagar por esta ejecución. Mientras la EVM ejecuta la transacción, el gas se gasta gradualmente según unas reglas específicas.

El **precio del gas** (gas price) es un valor establecido por el creador de la transacción, quien tiene que pagar el ``gas_price * gas`` desde la cuenta de envío. Si queda algo de gas después de la ejecución, se le reembolsa.

Si se ha gastado todo el gas en un punto (p.ej.: es negativo),
se lanza una excepción de out-of-gas, que revierte todas las modificaciones hechas al estado en el contexto de la ejecución actual.
=======
Upon creation, each transaction is charged with a certain amount of **gas**
that has to be paid for by the originator of the transaction (``tx.origin``).
While the EVM executes the
transaction, the gas is gradually depleted according to specific rules.
If the gas is used up at any point (i.e. it would be negative),
an out-of-gas exception is triggered, which ends execution and reverts all modifications
made to the state in the current call frame.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

This mechanism incentivizes economical use of EVM execution time
and also compensates EVM executors (i.e. miners / stakers) for their work.
Since each block has a maximum amount of gas, it also limits the amount
of work needed to validate a block.

The **gas price** is a value set by the originator of the transaction, who
has to pay ``gas_price * gas`` up front to the EVM executor.
If some gas is left after execution, it is refunded to the transaction originator.
In case of an exception that reverts changes, already used up gas is not refunded.

Since EVM executors can choose to include a transaction or not,
transaction senders cannot abuse the system by setting a low gas price.

.. index:: ! storage, ! memory, ! stack

Almacenamiento, Memoria y la Pila
=================================

<<<<<<< HEAD
Cada cuenta tiene un área de memoria persistente que se llama **almacenamiento**.
El almacenamiento es un almacén clave-valor que mapea palabras de 256 bits con palabras de 256 bits.
No es posible enumerar el almacenamiento interno desde un contrato y es comparativamente costoso leer y, más todavía, modificar el almacenamiento. Un contrato no pueder leer ni escribir en otro almacenamiento que no sea el suyo.

La segunda área de memoria se conoce como **memoria**, de la que un contrato obtiene de forma ágil una instancia clara de cada message call (llamada de mensaje). La memoria es lineal y puede ser tratada a nivel de byte, pero las lecturas están limitadas a un ancho de 256 bits, mientras que las escrituras pueden ser tanto de 8 bits como de 256 bits de ancho. La memoria se expande por palabras (256 bits), cuando se accede (tanto para leer o escribir) a una palabra de memoria sin modificar previamente (p.ej.: cualquier offset de una palabra). En el momento de expansión, se debe pagar el coste en gas. La memoria es más costosa cuanto más crece (escala cuadráticamente).

La EVM no es una máquina de registro, es una máquina de pila por lo que todas las operaciones se hacen en un área llamada la **pila**. Tiene un espacio máximo de 1024 elementos y contiene palabras de 256 bits. El acceso a la pila está limitado a su cima de la siguiente manera:
Es posible copiar uno de los 16 elementos superiores a la cima de la pila o intercambiar el elemento superior justo después de uno de los 16 elementos superiores.
El resto de operaciones cogen los dos elementos más superiores (o uno, o más, dependiendo de la operación) de la pila y ponen el resultado en ella. Por supuesto, es posible mover elementos de la pila al almacenamiento o a la memoria, pero no es posible acceder simplemente a elementos arbitrarios más profundos dentro de la pila sin, primeramente, borrar los que ya están encima.
=======
The Ethereum Virtual Machine has three areas where it can store data:
storage, memory and the stack.

Each account has a data area called **storage**, which is persistent between function calls
and transactions.
Storage is a key-value store that maps 256-bit words to 256-bit words.
It is not possible to enumerate storage from within a contract, it is
comparatively costly to read, and even more to initialise and modify storage. Because of this cost,
you should minimize what you store in persistent storage to what the contract needs to run.
Store data like derived calculations, caching, and aggregates outside of the contract.
A contract can neither read nor write to any storage apart from its own.

The second data area is called **memory**, of which a contract obtains
a freshly cleared instance for each message call. Memory is linear and can be
addressed at byte level, but reads are limited to a width of 256 bits, while writes
can be either 8 bits or 256 bits wide. Memory is expanded by a word (256-bit), when
accessing (either reading or writing) a previously untouched memory word (i.e. any offset
within a word). At the time of expansion, the cost in gas must be paid. Memory is more
costly the larger it grows (it scales quadratically).

The EVM is not a register machine but a stack machine, so all
computations are performed on a data area called the **stack**. It has a maximum size of
1024 elements and contains words of 256 bits. Access to the stack is
limited to the top end in the following way:
It is possible to copy one of
the topmost 16 elements to the top of the stack or swap the
topmost element with one of the 16 elements below it.
All other operations take the topmost two (or one, or more, depending on
the operation) elements from the stack and push the result onto the stack.
Of course it is possible to move stack elements to storage or memory
in order to get deeper access to the stack,
but it is not possible to just access arbitrary elements deeper in the stack
without first removing the top of the stack.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. index:: ! instruction

Conjunto de instrucciones
=========================

<<<<<<< HEAD
El conjunto de instrucciones de la EVM se mantiene mínimo con el objetivo de evitar implementaciones incorrectas que podrían causar problemas de consenso. Todas las instrucciones operan con el tipo de datos básico, palabras de 256 bits.
Las operaciones de aritmética habitual, bit, lógica y de comparación están presentes.
Se permiten tanto los saltos condicionales como los no condicionales. Es más, los contratos pueden acceder a propiedades relevantes del bloque actual como su número y timestamp.
=======
The instruction set of the EVM is kept minimal in order to avoid
incorrect or inconsistent implementations which could cause consensus problems.
All instructions operate on the basic data type, 256-bit words or on slices of memory
(or other byte arrays).
The usual arithmetic, bit, logical and comparison operations are present.
Conditional and unconditional jumps are possible. Furthermore,
contracts can access relevant properties of the current block
like its number and timestamp.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

For a complete list, please see the :ref:`list of opcodes <opcodes>` as part of the inline
assembly documentation.

.. index:: ! message call, function;call

Message Calls
=============

Los contratos pueden llamar a otros contratos o enviar Ether a cuentas que no sean de contratos usando message calls. Los Message calls son similares a las transacciones, en el sentido de que tienen un origen, un destino, datos, Ether, gas y datos de retorno. De hecho, cada transacción consiste en un message call de alto nivel que de forma consecutiva puede crear message calls posteriores.

<<<<<<< HEAD
Un contrato puede decidir cuánto de su **gas** restante podría ser enviado con el message call interno y cuánto quiere retener. Si ocurre una excepción de out-of-gas durante la llamada interna (o cualquier otra excepción), se mostrará como un valor de error introducido dentro de la pila. En este caso, sólo se gasta el gas enviado junto con la llamada.
En Solidity, el contrato que hace la llamada causa una excepción manual por defecto en estas situaciones, por lo que esas excepciones ascienden en la pila de llamada. 

Como se ha mencionado, el contrato llamado (que podría ser el mismo que el que hace la llamada) recibirá una instancia de memoria vacía y tendrá acceso a los datos de la llamada - que serán provistos en un área separada que se llama **calldata**.
Después de finalizar su ejecución, puede devolver datos que serán almacenados en una localización en la memoria del que hace la llamada que éste ha reservado previamente.

Las llamadas están **limitadas** a la profundidad de 1024, lo que quiere decir que para operaciones más complejas, se debería preferir bucles sobre llamadas recursivas.
=======
A contract can decide how much of its remaining **gas** should be sent
with the inner message call and how much it wants to retain.
If an out-of-gas exception happens in the inner call (or any
other exception), this will be signaled by an error value put onto the stack.
In this case, only the gas sent together with the call is used up.
In Solidity, the calling contract causes a manual exception by default in
such situations, so that exceptions "bubble up" the call stack.

As already said, the called contract (which can be the same as the caller)
will receive a freshly cleared instance of memory and has access to the
call payload - which will be provided in a separate area called the **calldata**.
After it has finished execution, it can return data which will be stored at
a location in the caller's memory preallocated by the caller.
All such calls are fully synchronous.

Calls are **limited** to a depth of 1024, which means that for more complex
operations, loops should be preferred over recursive calls. Furthermore,
only 63/64th of the gas can be forwarded in a message call, which causes a
depth limit of a little less than 1000 in practice.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. index:: delegatecall, callcode, library

Delegatecall / Callcode y librerías
===================================

<<<<<<< HEAD
Existe una variante especial de message call llamada **delegatecall**
que es idéntica a un message call con la excepción de que el código en la dirección destino se ejecuta en el contexto del que hace la llamada y ``msg.sender`` y ``msg.value`` no cambian sus valores.
=======
There exists a special variant of a message call, named **delegatecall**
which is identical to a message call apart from the fact that
the code at the target address is executed in the context (i.e. at the address) of the calling
contract and ``msg.sender`` and ``msg.value`` do not change their values.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

Esto significa que un contrato puede cargar código dinámicamente desde una dirección diferente en tiempo de ejecución. El almacenamiento, la dirección actual y el balance siguen referenciando al contrato que realiza la llamada, sólo se coge el código desde la dirección llamada.

<<<<<<< HEAD
Esto hace posible implementar la funcionalidad de "librería" en Solidity:
Código de librería reusable que se puede aplicar a un almacenamiento de contrato, por ejemplo, con el fin de implementar una estructura de datos compleja.
=======
This makes it possible to implement the "library" feature in Solidity:
Reusable library code that can be applied to a contract's storage, e.g. in
order to implement a complex data structure.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. index:: log

Logs
====

<<<<<<< HEAD
Es posible almacenar datos en una estructura de datos indexada que mapea todo el recorrido hasta el nivel de bloque. Esta funcionalidad llamada **logs** se usa en Solidity para implementar **eventos**.
Los contratos no pueden acceder a los datos del log después de crearse, pero pueden ser accedidos desde fuera de la blockchain de forma eficiente. Como parte de los datos del log se guardan en  `bloom filters <https://es.wikipedia.org/wiki/Filtro_de_Bloom>`_ (Filtro de Bloom), es posible buscar estos datos eficientemente y criptográficamente de manera segura, por lo que los otros miembros de la red que no se han descargado la blockchain entera ("light clients") todavía pueden buscarlos.
=======
It is possible to store data in a specially indexed data structure
that maps all the way up to the block level. This feature called **logs**
is used by Solidity in order to implement :ref:`events <events>`.
Contracts cannot access log data after it has been created, but they
can be efficiently accessed from outside the blockchain.
Since some part of the log data is stored in `bloom filters <https://en.wikipedia.org/wiki/Bloom_filter>`_, it is
possible to search for this data in an efficient and cryptographically
secure way, so network peers that do not download the whole blockchain
(so-called "light clients") can still find these logs.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. index:: contract creation

Creación
========

<<<<<<< HEAD
Los contratos pueden incluso crear otros contratos usando un opcode especial (p.ej.: ellos no llaman simplemente a la dirección cero). La única diferencia entre estos **create calls** y los message calls normales es que los datos son ejecutados y el resultado almacenado como código y el caller / creador recibe la dirección del nuevo contrato en la pila.
=======
Contracts can even create other contracts using a special opcode (i.e.
they do not simply call the zero address as a transaction would). The only difference between
these **create calls** and normal message calls is that the payload data is
executed and the result stored as code and the caller / creator
receives the address of the new contract on the stack.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. index:: selfdestruct, self-destruct, deactivate

<<<<<<< HEAD
Auto-destrucción
================

La única posibilidad de borrar el código de la blockchain es cuando un contrato en esa dirección realiza una operación de ``selfdestruct``. Los Ether restantes almacenados en esa dirección son enviados al destinatario designado y, entonces, se borran el almacenamiento y el código del estado.

.. warning:: Aunque un contrato no contenga una llamada a ``selfdestruct``,
  todavía podría hacer esa operación mediante ``delegatecall`` o ``callcode``.

.. note:: La eliminación de contratos antiguos puede, o no, ser implementada en clientes de Ethereum. Adicionalmente, los nodos de archivo podrían elegir mantener el almacenamiento del contrato y el código de forma indefinida.

.. note:: Actualmente las **cuentas externas** no se pueden borrar del estado.
=======
Deactivate and Self-destruct
============================

The only way to remove code from the blockchain is when a contract at that
address performs the ``selfdestruct`` operation. The remaining Ether stored
at that address is sent to a designated target and then the storage and code
is removed from the state. Removing the contract in theory sounds like a good
idea, but it is potentially dangerous, as if someone sends Ether to removed
contracts, the Ether is forever lost.

.. warning::
    Even if a contract is removed by ``selfdestruct``, it is still part of the
    history of the blockchain and probably retained by most Ethereum nodes.
    So using ``selfdestruct`` is not the same as deleting data from a hard disk.

.. note::
    Even if a contract's code does not contain a call to ``selfdestruct``,
    it can still perform that operation using ``delegatecall`` or ``callcode``.

If you want to deactivate your contracts, you should instead **disable** them
by changing some internal state which causes all functions to revert. This
makes it impossible to use the contract, as it returns Ether immediately.


.. index:: ! precompiled contracts, ! precompiles, ! contract;precompiled

.. _precompiledContracts:

Precompiled Contracts
=====================

There is a small set of contract addresses that are special:
The address range between ``1`` and (including) ``8`` contains
"precompiled contracts" that can be called as any other contract
but their behaviour (and their gas consumption) is not defined
by EVM code stored at that address (they do not contain code)
but instead is implemented in the EVM execution environment itself.

Different EVM-compatible chains might use a different set of
precompiled contracts. It might also be possible that new
precompiled contracts are added to the Ethereum main chain in the future,
but you can reasonably expect them to always be in the range between
``1`` and ``0xffff`` (inclusive).
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574
