.. _security_considerations:

############################
Consideraciones de seguridad
############################

Aunque en general es bastante fácil hacer software que funcione como esperamos,
es mucho más difícil de chequear que nadie lo pueda usar de alguna forma que **no**
fue anticipada.

En solidity, esto es aún más importante ya que se pueden usar los contratos
inteligentes para mover tokens, o posiblemente, cosas aún más valiosas. Además,
cada ejecución de un contrato inteligente es pública, y a ello se suma
que el código fuente muchas veces está disponible.

<<<<<<< HEAD
Por supuesto, siempre se tiene que considerar lo que está en juego:
Puedes comparar un contrato inteligente con un servicio web que está abierto
al público (y por lo tanto, también a gente malintencionada) y quizá
de código abierto.
Si sólo se guarda la lista de compras en ese servicio web, puede que no tengas
que tener mucho cuidado, pero si accedes a tu cuenta bancaria usando ese servicio,
deberíás tener más cuidado.

Esta sección nombrará algunos errores comunes y recomendaciones de seguridad
generales pero no puede, por supuesto, ser una lista completa. Además, recordar
que incluso si tu contrato inteligente está libre de errores, el compilador
o la plataforma puede que los tenga. Una lista de errores de seguridad públicamente
conocidos del compilador puede encontrarse en: :ref:`lista de errores conocidos<known_bugs>`,
la lista también es legible por máquina. Nótese que hay una recompensa
por encontrar errores (bug-bounty) que cubre el generador de código del compilador de Solidity.
=======
Of course you always have to consider how much is at stake:
You can compare a smart contract with a web service that is open to the
public (and thus, also to malicious actors) and perhaps even open source.
If you only store your grocery list on that web service, you might not have
to take too much care, but if you manage your bank account using that web service,
you should be more careful.

This section will list some pitfalls and general security recommendations but
can, of course, never be complete.  Also, keep in mind that even if your smart
contract code is bug-free, the compiler or the platform itself might have a
bug. A list of some publicly known security-relevant bugs of the compiler can
be found in the :ref:`list of known bugs<known_bugs>`, which is also
machine-readable. Note that there is a bug bounty program that covers the code
generator of the Solidity compiler.
>>>>>>> 84c64edfeea316f2d5bc6b4f0f392301a53061b6

Como siempre ocurre con la documentación de código abierto, por favor, ayúdanos a extender
esta sección, ¡especialmente con algunos ejemplos!

<<<<<<< HEAD
=======
NOTE: In addition to the list below, you can find more security recommendations and best practices
`in Guy Lando's knowledge list <https://github.com/guylando/KnowledgeLists/blob/master/EthereumSmartContracts.md>`_ and
`the Consensys GitHub repo <https://consensys.github.io/smart-contract-best-practices/>`_.

********
Pitfalls
********
>>>>>>> 84c64edfeea316f2d5bc6b4f0f392301a53061b6

***************
Errores Comunes
***************

Información privada y aleatoriedad
==================================

Todo lo que usas en un contrato inteligente es públicamente visible, incluso
variables locales y variables de estado marcadas como ``private``.

Es bastante complejo usar números aleatorios en contratos inteligentes si no
quieres que los mineros puedan hacer trampa.

Reentrada
=========

Cualquier interacción desde un contrato (A) con otro contrato (B) y cualquier
transferencia de Ether, le da el control a ese contrato (B). Esto hace posible
que B vuelva a llamar a A antes de terminar la interacción. Para dar un ejemplo,
el siguiente código contiene un error (esto es sólo un snippet y no un contrato
completo):

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.6.0 <0.9.0;

<<<<<<< HEAD
  // ESTE CONTRATO CONTIENE UN ERROR - NO USAR
  contract Fund {
      /// Mapping de distribución de ether del contrato.
      mapping(address => uint) shares;
      /// Retira tu parte.
      function withdraw() {
          if (msg.sender.send(shares[msg.sender]))
              shares[msg.sender] = 0;
      }
  }

El problema aquí no es tan grave por el límite de gas de la función
``send``, pero aun así, expone una debilidad: una transferencia de Ether
siempre incluye ejecución de código, así que el receptor puede ser un
contrato que vuelve a llamar a ``withdraw``. Esto le permitiría obtener
múltiples devoluciones, y por lo tanto, vaciar el Ether del contrato.
=======
    // THIS CONTRACT CONTAINS A BUG - DO NOT USE
    contract Fund {
        /// @dev Mapping of ether shares of the contract.
        mapping(address => uint) shares;
        /// Withdraw your share.
        function withdraw() public {
            if (payable(msg.sender).send(shares[msg.sender]))
                shares[msg.sender] = 0;
        }
    }

The problem is not too serious here because of the limited gas as part
of ``send``, but it still exposes a weakness: Ether transfer can always
include code execution, so the recipient could be a contract that calls
back into ``withdraw``. This would let it get multiple refunds and
basically retrieve all the Ether in the contract. In particular, the
following contract will allow an attacker to refund multiple times
as it uses ``call`` which forwards all remaining gas by default:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.6.2 <0.9.0;

    // THIS CONTRACT CONTAINS A BUG - DO NOT USE
    contract Fund {
        /// @dev Mapping of ether shares of the contract.
        mapping(address => uint) shares;
        /// Withdraw your share.
        function withdraw() public {
            (bool success,) = msg.sender.call{value: shares[msg.sender]}("");
            if (success)
                shares[msg.sender] = 0;
        }
    }
>>>>>>> 84c64edfeea316f2d5bc6b4f0f392301a53061b6

Para evitar reentradas, puedes usar el orden Comprobaciones-Efectos-Interacciones
como detallamos aquí:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.6.0 <0.9.0;

<<<<<<< HEAD
  contract Fund {
      /// Mapping de distribución de ether del contrato.
      mapping(address => uint) shares;
      /// Retira tu parte.
      function withdraw() {
          var share = shares[msg.sender];
          shares[msg.sender] = 0;
          msg.sender.transfer(share);
      }
  }
=======
    contract Fund {
        /// @dev Mapping of ether shares of the contract.
        mapping(address => uint) shares;
        /// Withdraw your share.
        function withdraw() public {
            uint share = shares[msg.sender];
            shares[msg.sender] = 0;
            payable(msg.sender).transfer(share);
        }
    }
>>>>>>> 84c64edfeea316f2d5bc6b4f0f392301a53061b6

Nótese que las reentradas no sólo son un riesgo al transferir Ether, sino
de cualquier ejecución de una función de otro contrato. Además, también tienes que
considerar situaciones de multi-contrato. Un contrato ejecutado podría modificar el
estado de otro contrato del cual dependes.

Límite de gas y bucles
======================

<<<<<<< HEAD
Los bucles que no tienen un número fijo de iteraciones, por ejemplo, bucles que dependen de valores de almacenamiento, tienen que
usarse con cuidado:
Dado el límite de gas del bloque, las transacciones sólo pueden consumir una cierta cantidad de gas. Ya sea explícitamente,
o por una operación normal, el número de iteraciones en un bucle puede crecer más allá del límite de gas, lo que puede causar que el
contrato se detenga por completo en un cierto punto. Esto no se aplica a funciones ``constant`` que sólo se llaman para
leer información de la blockchain. Pero aun así, estas funciones pueden ser llamadas por otros contratos como parte de operaciones
on-chain y detenerlos a ellos. Por favor, sé explícito con estos casos en la documentación de tus contratos.
=======
Loops that do not have a fixed number of iterations, for example, loops that depend on storage values, have to be used carefully:
Due to the block gas limit, transactions can only consume a certain amount of gas. Either explicitly or just due to
normal operation, the number of iterations in a loop can grow beyond the block gas limit which can cause the complete
contract to be stalled at a certain point. This may not apply to ``view`` functions that are only executed
to read data from the blockchain. Still, such functions may be called by other contracts as part of on-chain operations
and stall those. Please be explicit about such cases in the documentation of your contracts.
>>>>>>> 84c64edfeea316f2d5bc6b4f0f392301a53061b6

Envío y recibo de Ether
=======================

<<<<<<< HEAD
- Ni los contratos ni las "cuentas externas", son actualmente capaces de prevenir que alguien
  les envíe Ether. Los contratos pueden reaccionar y rechazar una transferencia normal, pero hay maneras de mover
  Ether sin provocar la ejecución de código. Una manera es simplemente "minando" a la
  cuenta del contrato y la otra es usando ``selfdestruct(x)``.

- Si un contrato recibe Ether (sin que se llame a ninguna función), se ejecuta la función fallback.
  Si no tiene una función fallback, el Ether será rechazado (lanzando una excepción).
  Durante la ejecución de la función fallback, el contrato sólo puede depender del
  "estipendio de gas" (2300 gas) que tiene disponible en ese momento. Este estipendio no es suficiente para acceder
  al almacenamiento de ninguna forma. Para asegurarte de que tu contrato pueda recibir Ether de ese modo, verifica los
  requerimientos de gas de la función fallback (por ejemplo, en la sección de "details" de Remix).

- Hay una manera de enviar más gas al contrato receptor usando ``addr.call.value(x)()``.
  Esto es esencialmente lo mismo que ``addr.transfer(x)``, solo que envía todo el gas restante
  y permite la posibilidad al receptor de realizar acciones más caras (y sólo devuelve un código
  de error y no propaga automáticamente el error). Dentro de estas acciones se incluyen llamar de nuevo al contrato
  emisor u otros cambios de estado que no fueron previstos. Permite más flexibilidad para
  usuarios honestos pero también para los usuarios maliciosos.

- Si quieres enviar Ether usando ``address.transfer``, hay ciertos detalles que hay que saber:

  1. Si el receptor es un contrato, se ejecutará la función fallback, lo cual puede llamar de vuelta al
  contrato que envía Ether.
  2. El envío de Ether puede fallar debido a la profundidad de la llamada subiendo por encima 1024. Puesto que el que
  llama tiene el control total de la profundidad de la llamada, pueden forzar la transferencia para que falle;
  tened en consideración esta posibilidad o utilizad siempre ``send`` y aseguraos siempre de revisar el valor
  de retorno. O mejor aún, escribid el contrato siguiendo un patrón de modo que sea el receptor el que tenga que sacar el Ether.
  3. El envío de Ether también puede fallar porque la ejecución del contrato receptor necesita más gas que
  la cantidad asignada (OOG, por sus siglas en inglés "Out of Gas"). Esto ocurre porque
  explícitamente se usó ``require``, ``assert``, ``revert``, ``throw``, o simplemente porque la operación es
  demasiado cara. Si usas ``transfer`` o ``send`` comprobando el valor de retorno, esto puede hacer que el receptor bloquee
  el progreso en el contrato emisor.Pero volviendo a insistir, aquí lo mejor es usar un
  :ref:`patrón de "retirada" en vez de un "orden de envío" <withdrawal_pattern>`.

Profundidad de la pila de llamadas (Callstack)
==============================================

Las llamadas externas a funciones pueden fallar en cualquier momento al
exceder la profundidad máxima de la pila de llamadas de 1024. En tales situaciones,
Solidity lanza una excepción. Los usuarios maliciosos podrían forzar la profundidad de 
la pila a un valor alto antes de interactuar con el contrato.

Ten en cuenta que ``.send()`` **no** lanza una excepción si la pila está vacía, sino
que retorna ``false`` en ese caso. Las funciones de bajo nivel como ``.call()``,
``.callcode()``  ``.delegatecall()`` se comportan de la misma manera.
=======
- Neither contracts nor "external accounts" are currently able to prevent that someone sends them Ether.
  Contracts can react on and reject a regular transfer, but there are ways
  to move Ether without creating a message call. One way is to simply "mine to"
  the contract address and the second way is using ``selfdestruct(x)``.

- If a contract receives Ether (without a function being called),
  either the :ref:`receive Ether <receive-ether-function>`
  or the :ref:`fallback <fallback-function>` function is executed.
  If it does not have a receive nor a fallback function, the Ether will be
  rejected (by throwing an exception). During the execution of one of these
  functions, the contract can only rely on the "gas stipend" it is passed (2300
  gas) being available to it at that time. This stipend is not enough to modify
  storage (do not take this for granted though, the stipend might change with
  future hard forks). To be sure that your contract can receive Ether in that
  way, check the gas requirements of the receive and fallback functions
  (for example in the "details" section in Remix).

- There is a way to forward more gas to the receiving contract using
  ``addr.call{value: x}("")``. This is essentially the same as ``addr.transfer(x)``,
  only that it forwards all remaining gas and opens up the ability for the
  recipient to perform more expensive actions (and it returns a failure code
  instead of automatically propagating the error). This might include calling back
  into the sending contract or other state changes you might not have thought of.
  So it allows for great flexibility for honest users but also for malicious actors.

- Use the most precise units to represent the wei amount as possible, as you lose
  any that is rounded due to a lack of precision.

- If you want to send Ether using ``address.transfer``, there are certain details to be aware of:

  1. If the recipient is a contract, it causes its receive or fallback function
     to be executed which can, in turn, call back the sending contract.
  2. Sending Ether can fail due to the call depth going above 1024. Since the
     caller is in total control of the call depth, they can force the
     transfer to fail; take this possibility into account or use ``send`` and
     make sure to always check its return value. Better yet, write your
     contract using a pattern where the recipient can withdraw Ether instead.
  3. Sending Ether can also fail because the execution of the recipient
     contract requires more than the allotted amount of gas (explicitly by
     using :ref:`require <assert-and-require>`, :ref:`assert <assert-and-require>`,
     :ref:`revert <assert-and-require>` or because the
     operation is too expensive) - it "runs out of gas" (OOG).  If you
     use ``transfer`` or ``send`` with a return value check, this might
     provide a means for the recipient to block progress in the sending
     contract. Again, the best practice here is to use a :ref:`"withdraw"
     pattern instead of a "send" pattern <withdrawal_pattern>`.

Call Stack Depth
================

External function calls can fail any time because they exceed the maximum
call stack size limit of 1024. In such situations, Solidity throws an exception.
Malicious actors might be able to force the call stack to a high value
before they interact with your contract. Note that, since `Tangerine Whistle <https://eips.ethereum.org/EIPS/eip-608>`_ hardfork, the `63/64 rule <https://eips.ethereum.org/EIPS/eip-150>`_ makes call stack depth attack impractical. Also note that the call stack and the expression stack are unrelated, even though both have a size limit of 1024 stack slots.

Note that ``.send()`` does **not** throw an exception if the call stack is
depleted but rather returns ``false`` in that case. The low-level functions
``.call()``, ``.delegatecall()`` and ``.staticcall()`` behave in the same way.

Authorized Proxies
==================

If your contract can act as a proxy, i.e. if it can call arbitrary contracts
with user-supplied data, then the user can essentially assume the identity
of the proxy contract. Even if you have other protective measures in place,
it is best to build your contract system such that the proxy does not have
any permissions (not even for itself). If needed, you can accomplish that
using a second proxy:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity ^0.8.0;
    contract ProxyWithMoreFunctionality {
        PermissionlessProxy proxy;

        function callOther(address addr, bytes memory payload) public
                returns (bool, bytes memory) {
            return proxy.callOther(addr, payload);
        }
        // Other functions and other functionality
    }

    // This is the full contract, it has no other functionality and
    // requires no privileges to work.
    contract PermissionlessProxy {
        function callOther(address addr, bytes memory payload) public
                returns (bool, bytes memory) {
            return addr.call(payload);
        }
    }
>>>>>>> 84c64edfeea316f2d5bc6b4f0f392301a53061b6

tx.origin
=========

No uses nunca tx.origin para dar autorización. Digamos que tienes un contrato de cartera como este:

.. code-block:: solidity

<<<<<<< HEAD
    // ESTE CONTRATO CONTIENE UN ERROR - NO USAR
=======
    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.7.0 <0.9.0;
    // THIS CONTRACT CONTAINS A BUG - DO NOT USE
>>>>>>> 84c64edfeea316f2d5bc6b4f0f392301a53061b6
    contract TxUserWallet {
        address owner;

        constructor() {
            owner = msg.sender;
        }

        function transferTo(address payable dest, uint amount) public {
            // THE BUG IS RIGHT HERE, you must use msg.sender instead of tx.origin
            require(tx.origin == owner);
            dest.transfer(amount);
        }
    }

<<<<<<< HEAD
Ahora alguien te engaña para que le envíes Ether a esta cartera maliciosa:
=======
Now someone tricks you into sending Ether to the address of this attack wallet:
>>>>>>> 84c64edfeea316f2d5bc6b4f0f392301a53061b6

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.7.0 <0.9.0;
    interface TxUserWallet {
        function transferTo(address payable dest, uint amount) external;
    }

    contract TxAttackWallet {
        address payable owner;

        constructor() {
            owner = payable(msg.sender);
        }

        receive() external payable {
            TxUserWallet(msg.sender).transferTo(owner, msg.sender.balance);
        }
    }

Si tu cartera hubiera comprobado ``msg.sender`` para darle autorización, recibiría la cuenta de la cartera atacante, en vez de la cartera del 'owner'. Pero al chequear ``tx.origin``, recibe la cuenta original que envió la transacción, que es la cuenta owner. La cartera atacante inmediatamente vacía todos tus fondos.

<<<<<<< HEAD
Detalles Menores
================

- En ``for (var i = 0; i < arrayName.length; i++) { ... }``, el tipo de ``i`` será ``uint8``, porque este es el tipo más pequeño que es requerido para guardar el valor ``0``. Si el vector (array) tiene más de 255 elementos, el bucle no se terminará.
- La palabra reservada ``constant`` para funciones no es actualmente forzada por el compilador.
  Tampoco está forzada por la EVM, de modo que una función de un contrato que "pretenda" ser constante,
  todava podría hacer cambios al estado.
- Los tipos que no utilizan totalmente los 32 bytes pueden contener basura en los bits más significativos.
  Esto es especialmente importante si se accede a ``msg.data`` ya que supone un riesgo de maleabilidad:
  Puedes crear transacciones que llaman una función ``f(uint8 x)`` con un argumento de tipo byte
  de ``0xff000001`` y ``0x00000001``. Ambos se pasarán al contrato y ambos se verán como
  el número ``1``. Pero ``msg.data`` es diferente, así que si se usa ``keccak246(msg.data)`` para
  algo, tendrás resultados diferentes.
=======
.. _underflow-overflow:

Two's Complement / Underflows / Overflows
=========================================

As in many programming languages, Solidity's integer types are not actually integers.
They resemble integers when the values are small, but cannot represent arbitrarily large numbers.

The following code causes an overflow because the result of the addition is too large
to be stored in the type ``uint8``:

.. code-block:: solidity

  uint8 x = 255;
  uint8 y = 1;
  return x + y;

Solidity has two modes in which it deals with these overflows: Checked and Unchecked or "wrapping" mode.

The default checked mode will detect overflows and cause a failing assertion. You can disable this check
using ``unchecked { ... }``, causing the overflow to be silently ignored. The above code would return
``0`` if wrapped in ``unchecked { ... }``.

Even in checked mode, do not assume you are protected from overflow bugs.
In this mode, overflows will always revert. If it is not possible to avoid the
overflow, this can lead to a smart contract being stuck in a certain state.

In general, read about the limits of two's complement representation, which even has some
more special edge cases for signed numbers.

Try to use ``require`` to limit the size of inputs to a reasonable range and use the
:ref:`SMT checker<smt_checker>` to find potential overflows.

.. _clearing-mappings:

Clearing Mappings
=================

The Solidity type ``mapping`` (see :ref:`mapping-types`) is a storage-only
key-value data structure that does not keep track of the keys that were
assigned a non-zero value.  Because of that, cleaning a mapping without extra
information about the written keys is not possible.
If a ``mapping`` is used as the base type of a dynamic storage array, deleting
or popping the array will have no effect over the ``mapping`` elements.  The
same happens, for example, if a ``mapping`` is used as the type of a member
field of a ``struct`` that is the base type of a dynamic storage array.  The
``mapping`` is also ignored in assignments of structs or arrays containing a
``mapping``.

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.6.0 <0.9.0;

    contract Map {
        mapping (uint => uint)[] array;

        function allocate(uint newMaps) public {
            for (uint i = 0; i < newMaps; i++)
                array.push();
        }

        function writeMap(uint map, uint key, uint value) public {
            array[map][key] = value;
        }

        function readMap(uint map, uint key) public view returns (uint) {
            return array[map][key];
        }

        function eraseMaps() public {
            delete array;
        }
    }

Consider the example above and the following sequence of calls: ``allocate(10)``,
``writeMap(4, 128, 256)``.
At this point, calling ``readMap(4, 128)`` returns 256.
If we call ``eraseMaps``, the length of state variable ``array`` is zeroed, but
since its ``mapping`` elements cannot be zeroed, their information stays alive
in the contract's storage.
After deleting ``array``, calling ``allocate(5)`` allows us to access
``array[4]`` again, and calling ``readMap(4, 128)`` returns 256 even without
another call to ``writeMap``.

If your ``mapping`` information must be deleted, consider using a library similar to
`iterable mapping <https://github.com/ethereum/dapp-bin/blob/master/library/iterable_mapping.sol>`_,
allowing you to traverse the keys and delete their values in the appropriate ``mapping``.

Minor Details
=============

- Types that do not occupy the full 32 bytes might contain "dirty higher order bits".
  This is especially important if you access ``msg.data`` - it poses a malleability risk:
  You can craft transactions that call a function ``f(uint8 x)`` with a raw byte argument
  of ``0xff000001`` and with ``0x00000001``. Both are fed to the contract and both will
  look like the number ``1`` as far as ``x`` is concerned, but ``msg.data`` will
  be different, so if you use ``keccak256(msg.data)`` for anything, you will get different results.
>>>>>>> 84c64edfeea316f2d5bc6b4f0f392301a53061b6

***************
Recomendaciones
***************

<<<<<<< HEAD
Restringir la cantidad de Ether
===============================
=======
Take Warnings Seriously
=======================

If the compiler warns you about something, you should change it.
Even if you do not think that this particular warning has security
implications, there might be another issue buried beneath it.
Any compiler warning we issue can be silenced by slight changes to the
code.

Always use the latest version of the compiler to be notified about all recently
introduced warnings.

Messages of type ``info`` issued by the compiler are not dangerous, and simply
represent extra suggestions and optional information that the compiler thinks
might be useful to the user.

Restrict the Amount of Ether
============================
>>>>>>> 84c64edfeea316f2d5bc6b4f0f392301a53061b6

Restringir la cantidad de Ether (u otros tokens) que puedan ser almacenados
en un contrato inteligente. Si el código fuente, el compilador o la plataforma
tiene un error, estos fondos podrían perderse. Si quieres limitar la pérdida,
limita la cantidad de Ether.

Pequeño y modular
=================

Mantén tus contratos pequeños y fáciles de entender. Separa las funcionalidades
no relacionadas en otros contratos o en librerías. Se pueden aplicar las recomendaciones
estándar de calidad de código: Limitar la cantidad de variables locales, limitar la longitud
de las funciones, etc. Documenta tus funciones para que otros puedan ver cuál era la
intención del código y para ver si hace algo diferente de lo que pretendía.

Usa el orden Comprobaciones-Efectos-Interacciones
=======================================================

La mayoría de las funciones primero ejecutan algunos chequeos (¿quién ha llamado a
la función?, ¿los argumentos están en el rango?, ¿mandaron suficiente Ether?
¿La cuenta tiene tokens?, etc.). Estos chequeos deben de hacerse primero.

Como segundo paso, si se pasaron todos los chequeos, se deben ejecutar los efectos a las
variables de estado del contrato actual. La interacción con otros contratos debe
hacerse como último paso en cualquier función.

Al principio, algunos contratos retrasaban la ejecución de los efectos y esperaban a que
una función externa devolviera un estado sin errores. Esto es un error grave ya que se puede
hacer un reingreso, como explicamos arriba.

También hay que tener en cuenta que las llamadas a contratos conocidos pueden a su vez causar llamadas
a otros contratos no conocidos, así que siempre es mejor aplicar este orden.

Incluir un modo a prueba de fallos
==================================

Aunque hacer que tu sistema sea completamente descentralizado eliminará cualquier intermediario,
puede que sea una buena idea, especialmente para nuevo código, incluir un sistema
a prueba de fallos:

Puedes agregar una función a tu contrato que realize algunas comprobaciones internas como
"¿se ha filtrado Ether?", "¿es igual la suma de los tokens al balance de la cuenta?"
o cosas similares. Recordad que no se puede usar mucho gas para eso, así que ayuda mediante
computaciones off-chain podrían ser necesarias.

Si los chequeos fallan, el contrato automáticamente cambia a modo a prueba de fallos, donde,
por ejemplo, se desactivan muchas funciones, da el control a una entidad tercera de confianza
o se convierte en un contrato "devuélveme mi dinero".

Ask for Peer Review
===================

<<<<<<< HEAD
*******************
Verificación formal
*******************

Usando verificación formal, es posible realizar pruebas matemáticas automatizadas de que el código
sigue una cierta especificación formal. La especificación aún es formal (como el código fuente),
pero normalmente mucho más simple. Hay un prototipo en Solidity que realiza verificación formal y
pronto se documentará mejor.

Ten en cuenta que la verificación formal en sí misma sólo puede ayudarte a entender la diferencia
entre lo que hiciste (la especificación) y cómo lo hiciste (la implementación real). Aún necesitas
chequear si la especificación es lo que querías y que no hayas olvidado efectos inesperados de ello.
=======
The more people examine a piece of code, the more issues are found.
Asking people to review your code also helps as a cross-check to find out whether your code
is easy to understand - a very important criterion for good smart contracts.
>>>>>>> 84c64edfeea316f2d5bc6b4f0f392301a53061b6
