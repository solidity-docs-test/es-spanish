################
Patrones comunes
################

.. index:: withdrawal

.. _withdrawal_pattern:

************************
Retirada desde contratos
************************

<<<<<<< HEAD
El método recomendado de envío de fondos después de una acción,
es usando el patrón de retirada (withdrawal pattern). Aunque el método
más intuitivo para enviar Ether tras una acción es
llamar directamente a ``send``, esto no es recomendable ya que
introduce un potencial riesgo de seguridad. Puedes leer más
sobre esto en la página :ref:`security_considerations`.

Este es un ejemplo del patrón de retirada en un
contrato donde el objetivo es enviar la mayor cantidad de Ether
al contrato a fin de convertirse en el más "adinerado", inspirado por
`King of the Ether <https://www.kingoftheether.com/>`_.

En el siguiente contrato, si dejas de ser el más adinerado,
recibes los fondos de la persona que te destronó.
=======
The recommended method of sending funds after an effect
is using the withdrawal pattern. Although the most intuitive
method of sending Ether, as a result of an effect, is a
direct ``transfer`` call, this is not recommended as it
introduces a potential security risk. You may read
more about this on the :ref:`security_considerations` page.

The following is an example of the withdrawal pattern in practice in
a contract where the goal is to send the most money to the
contract in order to become the "richest", inspired by
`King of the Ether <https://www.kingoftheether.com/>`_.

In the following contract, if you are no longer the richest,
you receive the funds of the person who is now the richest.
>>>>>>> 5f4c91cc6d43031e2d785a588d5964765cabb67f

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity ^0.8.4;

    contract WithdrawalContract {
        address public richest;
        uint public mostSent;

        mapping (address => uint) pendingWithdrawals;

<<<<<<< HEAD
        function WithdrawalContract() public payable {
=======
        /// The amount of Ether sent was not higher than
        /// the currently highest amount.
        error NotEnoughEther();

        constructor() payable {
>>>>>>> 5f4c91cc6d43031e2d785a588d5964765cabb67f
            richest = msg.sender;
            mostSent = msg.value;
        }

<<<<<<< HEAD
        function becomeRichest() public payable returns (bool) {
            if (msg.value > mostSent) {
                pendingWithdrawals[richest] += msg.value;
                richest = msg.sender;
                mostSent = msg.value;
                return true;
            } else {
                return false;
            }
=======
        function becomeRichest() public payable {
            if (msg.value <= mostSent) revert NotEnoughEther();
            pendingWithdrawals[richest] += msg.value;
            richest = msg.sender;
            mostSent = msg.value;
>>>>>>> 5f4c91cc6d43031e2d785a588d5964765cabb67f
        }

        function withdraw() public {
            uint amount = pendingWithdrawals[msg.sender];
            // Acuérdate de poner a cero la cantidad a reembolsar antes
            // de enviarlo para evitar re-entrancy attacks
            pendingWithdrawals[msg.sender] = 0;
            payable(msg.sender).transfer(amount);
        }
    }

Esto en lugar del patrón más intuitivo de envío:

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity ^0.8.4;

    contract SendContract {
        address payable public richest;
        uint public mostSent;

<<<<<<< HEAD
        function SendContract() public payable {
            richest = msg.sender;
            mostSent = msg.value;
        }

        function becomeRichest() public payable returns (bool) {
            if (msg.value > mostSent) {
                // Esta línea puede causar problemas (explicado abajo).
                richest.transfer(msg.value);
                richest = msg.sender;
                mostSent = msg.value;
                return true;
            } else {
                return false;
            }
        }
    }

Nótese que, en este ejemplo, un atacante puede bloquear
el contrato en un estado inútil haciendo que ``richest``
sea la dirección de un contrato que tiene una función fallback
que falla (ej. usando ``revert()`` o simplemente consumiendo más de
2300 de gas). De esa forma, cuando se llama a ``transfer``
para enviar fondos al contrato "envenenado", fallará
y también fallará la función ``becomeRichest``, bloqueando el
contrario para siempre.
=======
        /// The amount of Ether sent was not higher than
        /// the currently highest amount.
        error NotEnoughEther();

        constructor() payable {
            richest = payable(msg.sender);
            mostSent = msg.value;
        }

        function becomeRichest() public payable {
            if (msg.value <= mostSent) revert NotEnoughEther();
            // This line can cause problems (explained below).
            richest.transfer(msg.value);
            richest = payable(msg.sender);
            mostSent = msg.value;
        }
    }

Notice that, in this example, an attacker could trap the
contract into an unusable state by causing ``richest`` to be
the address of a contract that has a receive or fallback function
which fails (e.g. by using ``revert()`` or by just
consuming more than the 2300 gas stipend transferred to them). That way,
whenever ``transfer`` is called to deliver funds to the
"poisoned" contract, it will fail and thus also ``becomeRichest``
will fail, with the contract being stuck forever.
>>>>>>> 5f4c91cc6d43031e2d785a588d5964765cabb67f

Por el contrario, si usas el patrón "withdrawl" del primer ejemplo,
el atacante sólo puede causar que su propio withdrawl falle y no
el resto del contrato.

.. index:: access;restricting

***********************
Restringiendo el acceso
***********************

Restringiendo el acceso (Restricting access) es un patrón común para contratos.
Nótese que nunca se puede evitar que un humano o un ordenador
lean el contenido de una transacción o el estado de un
contrato. Lo puedes hacer un poco más difícil de leer usando
criptografía, pero si tu contrato debe leer los datos, todos
podrán leerlo.

<<<<<<< HEAD
Puedes restringir el acceso de lectura al estado de tu contrato
por **otros contratos**. Esto ocurre por defecto
salvo que declares tus variables como ``public``.

Además, puedes restringir quién puede hacer modificaciones
al estado de tu contrato o quién puede llamar a las funciones.
De eso se trata esta sección.
=======
You can restrict read access to your contract's state
by **other contracts**. That is actually the default
unless you declare your state variables ``public``.

Furthermore, you can restrict who can make modifications
to your contract's state or call your contract's
functions and this is what this section is about.
>>>>>>> 5f4c91cc6d43031e2d785a588d5964765cabb67f

.. index:: function;modifier

El uso de **modificadores de funciones**
hace estas restricciones altamente visibles.

.. code-block:: solidity
    :force:

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity ^0.8.4;

    contract AccessRestriction {
        // Estas serán asignadas en la fase de
        // compilación, donde `msg.sender` es
        // la cuenta que crea este contrato.
        address public owner = msg.sender;
        uint public creationTime = block.timestamp;

        // Now follows a list of errors that
        // this contract can generate together
        // with a textual explanation in special
        // comments.

        /// Sender not authorized for this
        /// operation.
        error Unauthorized();

        /// Function called too early.
        error TooEarly();

        /// Not enough Ether sent with function call.
        error NotEnoughEther();

<<<<<<< HEAD
        // Los modificadores pueden usarse para
        // cambiar el cuerpo de una función.
        // Si se usa este modificador, agregará
        // un chequeo que sólo pasa si la
        // función se llama desde una cierta
        // dirección.
        modifier onlyBy(address _account)
        {
            require(msg.sender == _account);
            // ¡No olvides el "_;"!
            // Esto se reemplazará por el cuerpo
            // de la función cuando se use
            // el modificador
            _;
        }

        /// Hacer que `_newOwner` sea el nuevo owner de
        /// este contrato.
        function changeOwner(address _newOwner)
=======
        // Modifiers can be used to change
        // the body of a function.
        // If this modifier is used, it will
        // prepend a check that only passes
        // if the function is called from
        // a certain address.
        modifier onlyBy(address account)
        {
            if (msg.sender != account)
                revert Unauthorized();
            // Do not forget the "_;"! It will
            // be replaced by the actual function
            // body when the modifier is used.
            _;
        }

        /// Make `newOwner` the new owner of this
        /// contract.
        function changeOwner(address newOwner)
>>>>>>> 5f4c91cc6d43031e2d785a588d5964765cabb67f
            public
            onlyBy(owner)
        {
            owner = newOwner;
        }

        modifier onlyAfter(uint time) {
            if (block.timestamp < time)
                revert TooEarly();
            _;
        }

        /// Borra la información del dueño.
        /// Sólo puede llamarse 6 semanas
        /// después de que el contrato haya sido
        /// creado.
        function disown()
            public
            onlyBy(owner)
            onlyAfter(creationTime + 6 weeks)
        {
            delete owner;
        }

<<<<<<< HEAD
        // Este modificador requiere del pago de
        // una comisión asociada a la llamada
        // de una función.
        // Si el llamador envió demasiado, será
        // reembolsado, pero sólo después del cuerpo
        // de la función.
        // Esto era peligroso antes de la versión
        // 0.4.0 de Solidity, donde era posible
        // saltarse la parte después de `_;`.
        modifier costs(uint _amount) {
            require(msg.value >= _amount);
=======
        // This modifier requires a certain
        // fee being associated with a function call.
        // If the caller sent too much, he or she is
        // refunded, but only after the function body.
        // This was dangerous before Solidity version 0.4.0,
        // where it was possible to skip the part after `_;`.
        modifier costs(uint amount) {
            if (msg.value < amount)
                revert NotEnoughEther();

>>>>>>> 5f4c91cc6d43031e2d785a588d5964765cabb67f
            _;
            if (msg.value > amount)
                payable(msg.sender).transfer(msg.value - amount);
        }

<<<<<<< HEAD
        function forceOwnerChange(address _newOwner)
            public
            costs(200 ether)
        {
            owner = _newOwner;
            // sólo una condición de ejemplo
            if (uint(owner) & 0 == 1)
                // Esto no se hacía antes de Solidity
                // 0.4.0
=======
        function forceOwnerChange(address newOwner)
            public
            payable
            costs(200 ether)
        {
            owner = newOwner;
            // just some example condition
            if (uint160(owner) & 0 == 1)
                // This did not refund for Solidity
                // before version 0.4.0.
>>>>>>> 5f4c91cc6d43031e2d785a588d5964765cabb67f
                return;
            // reembolsar los fees excesivos
        }
    }

Una manera más especializada de la forma en la que se puede
reestringir el acceso a la llamada de funciones se verá en el
próximo ejemplo.

.. index:: state machine

******************
Máquina de estados
******************

Los contratos a menudo actúan como una máquina de estados,
lo que significa que tienen ciertas **etapas** en donde se
comportan de manera diferente o en donde distintas funciones
pueden ser llamadas. Una llamada de función a menudo
termina una etapa y pasa el contrato a la siguiente
etapa (especialmente si el contrato modela la **interacción**).
También es común que algunas etapas se alcancen
automáticamente en cierto punto en el **tiempo**.

Un ejemplo de esto es el contrato de subastas a ciegas
que comienza en la etapa "aceptando pujas a ciegas", luego
pasa a "revelando pujas" que es finalizado por
"determinar resultado de la subasta".

.. index:: function;modifier

Los modificadores de funciones se pueden usar en esta
situación para modelar los estados y evitar
el uso incorrecto del contrato.

Ejemplo
=======

En el siguiente ejemplo,
el modificador ``atStage`` asegura que la función
sólo pueda ser llamada en una cierta etapa.

<<<<<<< HEAD
El modificador ``timeTransitions`` gestiona las
transiciones de etapas de forma automática en función
del tiempo. Debe ser usado en todas las funciones.
=======
Automatic timed transitions
are handled by the modifier ``timedTransitions``, which
should be used for all functions.
>>>>>>> 5f4c91cc6d43031e2d785a588d5964765cabb67f

.. note::
    **El orden de los modificadores importa**.
    Si atStage se combina
    con timesTransitions, asegúrate de que puedas
    mencionarlo después de éste, para que la nueva
    etapa sea tomada en cuenta.

Finalmente, el modificador ``transitionNext`` puede
ser usado para ir automáticamente a la próxima etapa
cuando la función termine.

.. note::
    **El modificador puede ser omitido**.
    Esto sólo se aplica a Solidity antes de la versión
    0.4.0:
    Puesto que los modificadores se aplican simplemente
    reemplazando código y no realizando una llamada a una función,
    el código del modificador transitionNext se puede omitir
    si la propia función usa return. Si es lo que quieres hacer, asegúrate
    de llamar manualmente a nextStage desde esas funciones.
    A partir de la versión 0.4.0, el código de los modificadores
    se ejecutará incluso en el caso de que la función
    ejecute explícitamente un return.

.. code-block:: solidity
    :force:

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity ^0.8.4;

    contract StateMachine {
        enum Stages {
            AcceptingBlindedBids,
            RevealBids,
            AnotherStage,
            AreWeDoneYet,
            Finished
        }
        /// Function cannot be called at this time.
        error FunctionInvalidAtThisStage();

        // Ésta es la etapa actual.
        Stages public stage = Stages.AcceptingBlindedBids;

        uint public creationTime = block.timestamp;

        modifier atStage(Stages stage_) {
            if (stage != stage_)
                revert FunctionInvalidAtThisStage();
            _;
        }

        function nextStage() internal {
            stage = Stages(uint(stage) + 1);
        }

        // Hace transiciones temporizadas. Asegúrate de
        // mencionar este modificador primero, si no,
        // no se tendrá en cuenta la nueva etapa.
        modifier timedTransitions() {
            if (stage == Stages.AcceptingBlindedBids &&
                        block.timestamp >= creationTime + 10 days)
                nextStage();
            if (stage == Stages.RevealBids &&
                    block.timestamp >= creationTime + 12 days)
                nextStage();
            // La transición del resto de etapas se produce por transacciones
            _;
        }

        // ¡El orden de los modificadores importa aquí!
        function bid()
<<<<<<< HEAD
            public 
=======
            public
>>>>>>> 5f4c91cc6d43031e2d785a588d5964765cabb67f
            payable
            timedTransitions
            atStage(Stages.AcceptingBlindedBids)
        {
            // No implementaremos esto aquí
        }

        function reveal()
            public
            timedTransitions
            atStage(Stages.RevealBids)
        {
        }

        // Este modificador pasa a la próxima etapa
        // una vez terminada la función.
        modifier transitionNext()
        {
            _;
            nextStage();
        }

        function g()
            public
            timedTransitions
            atStage(Stages.AnotherStage)
            transitionNext
        {
        }

        function h()
            public
            timedTransitions
            atStage(Stages.AreWeDoneYet)
            transitionNext
        {
        }

        function i()
            public
            timedTransitions
            atStage(Stages.Finished)
        {
        }
    }
