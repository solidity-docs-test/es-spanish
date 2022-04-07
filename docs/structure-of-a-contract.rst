.. index:: contract, state variable, function, event, struct, enum, function;modifier

.. _contract_structure:

*************************
Estructura de un contrato
*************************

<<<<<<< HEAD
Los Contratos en Solidity son similares a las clases de los lenguajes orientados a objetos.
Cualquier contrato puede contener declaraciones del tipo :ref:`variables de estado <structure-state-variables>`, :ref:`funciones <structure-functions>`, :ref:`modificadores de función <structure-function-modifiers>`, :ref:`eventos <structure-events>`, :ref:`structs <structure-struct-types>` y :ref:`enums <structure-enum-types>`.
Además, los contratos pueden heredar de otros contratos.
=======
Contracts in Solidity are similar to classes in object-oriented languages.
Each contract can contain declarations of :ref:`structure-state-variables`, :ref:`structure-functions`,
:ref:`structure-function-modifiers`, :ref:`structure-events`, :ref:`structure-errors`, :ref:`structure-struct-types` and :ref:`structure-enum-types`.
Furthermore, contracts can inherit from other contracts.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

There are also special kinds of contracts called :ref:`libraries<libraries>` and :ref:`interfaces<interfaces>`.

The section about :ref:`contracts<contracts>` contains more details than this section,
which serves to provide a quick overview.

.. _structure-state-variables:

Variables de estado
===================

<<<<<<< HEAD
Las variables de estado son valores que están permanentemente almacenados en una parte del contrato conocida como storage del contrato.
=======
State variables are variables whose values are permanently stored in contract
storage.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.0 <0.9.0;

<<<<<<< HEAD
  contract SimpleStorage {
      uint storedData; // Variable de estado
      // ...
  }
=======
    contract SimpleStorage {
        uint storedData; // State variable
        // ...
    }
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

Véase la sección :ref:`tipos <types>` para conocer los diferentes tipos válidos de variables de estado y :ref:`visibilidad-y-getters <visibility-and-getters>` para conocer las distintas posibilidades de visibilidad que pueden tener las variables de estado.

.. _structure-functions:

Funciones
=========

<<<<<<< HEAD
Las funciones son las unidades ejecutables del código dentro de un contrato.
=======
Functions are the executable units of code. Functions are usually
defined inside a contract, but they can also be defined outside of
contracts.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.7.1 <0.9.0;

<<<<<<< HEAD
  contract SimpleAuction {
      function bid() public payable { // Función
          // ...
      }
  }

Las :ref:`llamadas a una función <function-calls>` pueden ocurrir dentro o fuera de la misma. Una función puede tener varios niveles de :ref:`visibilidad <visibility-and-getters>` con respecto a otros contratos.
=======
    contract SimpleAuction {
        function bid() public payable { // Function
            // ...
        }
    }

    // Helper function defined outside of a contract
    function helper(uint x) pure returns (uint) {
        return x * 2;
    }

:ref:`function-calls` can happen internally or externally
and have different levels of :ref:`visibility<visibility-and-getters>`
towards other contracts. :ref:`Functions<functions>` accept :ref:`parameters and return variables<function-parameters-return-variables>` to pass parameters
and values between them.
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. _structure-function-modifiers:

Modificadores de función
========================

<<<<<<< HEAD
Los modificadores de función se usan para enmendar de un modo declarativo la semántica de las funciones (véase :ref:`modificadores <modifiers>` en la sección sobre contratos).
=======
Function modifiers can be used to amend the semantics of functions in a declarative way
(see :ref:`modifiers` in the contracts section).
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

Overloading, that is, having the same modifier name with different parameters,
is not possible.

Like functions, modifiers can be :ref:`overridden <modifier-overriding>`.

.. code-block:: solidity

<<<<<<< HEAD
      modifier onlySeller() { // Modificador
          require(msg.sender == seller);
          _;
      }

      function abort() public onlySeller { // Uso de modificador
          // ...
      }
  }
=======
    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.22 <0.9.0;

    contract Purchase {
        address public seller;

        modifier onlySeller() { // Modifier
            require(
                msg.sender == seller,
                "Only seller can call this."
            );
            _;
        }

        function abort() public view onlySeller { // Modifier usage
            // ...
        }
    }
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. _structure-events:

Eventos
=======

Los eventos son interfaces de conveniencia con los servicios de registro de la EVM (Máquina Virtual de Ethereum).

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.21 <0.9.0;

<<<<<<< HEAD
  contract SimpleAuction {
      event HighestBidIncreased(address bidder, uint amount); // Evento

      function bid() public payable {
          // ...
          HighestBidIncreased(msg.sender, msg.value); // Lanzamiento del evento
      }
  }
=======
    contract SimpleAuction {
        event HighestBidIncreased(address bidder, uint amount); // Event

        function bid() public payable {
            // ...
            emit HighestBidIncreased(msg.sender, msg.value); // Triggering event
        }
    }
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

Véase :ref:`eventos <events>` en la sección sobre contratos para tener más información sobre cómo se declaran los eventos y cómo se pueden usar dentro de una dapp.

<<<<<<< HEAD
.. _structure-struct-types:

Tipos de structs
================
=======
.. _structure-errors:

Errors
======

Errors allow you to define descriptive names and data for failure situations.
Errors can be used in :ref:`revert statements <revert-statement>`.
In comparison to string descriptions, errors are much cheaper and allow you
to encode additional data. You can use NatSpec to describe the error to
the user.

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity ^0.8.4;

    /// Not enough funds for transfer. Requested `requested`,
    /// but only `available` available.
    error NotEnoughFunds(uint requested, uint available);

    contract Token {
        mapping(address => uint) balances;
        function transfer(address to, uint amount) public {
            uint balance = balances[msg.sender];
            if (balance < amount)
                revert NotEnoughFunds(amount, balance);
            balances[msg.sender] -= amount;
            balances[to] += amount;
            // ...
        }
    }

See :ref:`errors` in the contracts section for more information.

.. _structure-struct-types:

Struct Types
=============
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

Las estructuras de datos (Structs) son tipos definidos por el propio usuario y pueden agrupar mútiples variables (véase :ref:`structs <structs>` en la sección sobre tipos).

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.0 <0.9.0;

<<<<<<< HEAD
  contract Ballot {
      struct Voter { // Structs
          uint weight;
          bool voted;
          address delegate;
          uint vote;
      }
  }
=======
    contract Ballot {
        struct Voter { // Struct
            uint weight;
            bool voted;
            address delegate;
            uint vote;
        }
    }
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. _structure-enum-types:

Tipos de enum
=============

<<<<<<< HEAD
Los enumerados (Enums) se usan para crear tipos con un conjunto de valores finitos y están definidos por el propio usuario (véase :ref:`enums <enums>` en la sección sobre tipos).
=======
Enums can be used to create custom types with a finite set of 'constant values' (see
:ref:`enums` in types section).
>>>>>>> 15c2a33eb32fe6f0db825f90f83f24c1e45a6574

.. code-block:: solidity

    // SPDX-License-Identifier: GPL-3.0
    pragma solidity >=0.4.0 <0.9.0;

    contract Purchase {
        enum State { Created, Locked, Inactive } // Enum
    }
