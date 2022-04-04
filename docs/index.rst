Solidity
========

Solidity is an object-oriented, high-level language for implementing smart
contracts. Smart contracts are programs which govern the behaviour of accounts
within the Ethereum state.

<<<<<<< HEAD
Solidity es un lenguaje orientado a objetos, de alto nivel para la implementación de contratos inteligentes. Los contratos inteligentes son programas que gobiernan el comportamiento de las cuentas dentro de Ethereum.
=======
Solidity is a `curly-bracket language <https://en.wikipedia.org/wiki/List_of_programming_languages_by_type#Curly-bracket_languages>`_ designed to target the Ethereum Virtual Machine (EVM).
It is influenced by C++, Python and JavaScript. You can find more details about which languages Solidity has been inspired by in the :doc:`language influences <language-influences>` section.
>>>>>>> c4909e99c1015233d6937d229058cde52615f246

Solidity fue influenciado por C++, Python, JavaScript y diseñado para ser utilizado por la Máquina Virtual de Ethereum (EVM).

<<<<<<< HEAD
Solidity está tipado de manera estática y acepta, entre otras cosas, herencia, bibliotecas y tipos complejos definidos por el usuario.

Con Solidity es posible crear contratos para votar, para el crowdfunding, para subastas a ciegas y para monederos multi firmas.

Al desplegar contratos, deberias usar la última versión liberada de Solidity. Fuera de casos excepcionales, sólo la última versión recibe `Actualizaciones de Seguridad <https://github.com/ethereum/solidity/security/policy#supported-versions>`_.
Además, tanto cambios importantes como nuevas características son introducidas regularmente. Actualmente utilizamos un sistema de numeración de versiones 0.y.z `para indicar este ritmo rápido de cambio <https://semver.org/#spec-item-4>`_.

.. note::
    La mejor manera de probar Solidity ahora mismo es usando `Remix <https://remix.ethereum.org/>`_ (puede tardar un rato en cargarse, por favor sea paciente).

Enlaces útiles
--------------
=======
With Solidity you can create contracts for uses such as voting, crowdfunding, blind auctions,
and multi-signature wallets.

When deploying contracts, you should use the latest released
version of Solidity. Apart from exceptional cases, only the latest version receives
`security fixes <https://github.com/ethereum/solidity/security/policy#supported-versions>`_.
Furthermore, breaking changes as well as
new features are introduced regularly. We currently use
a 0.y.z version number `to indicate this fast pace of change <https://semver.org/#spec-item-4>`_.

.. warning::

  Solidity recently released the 0.8.x version that introduced a lot of breaking
  changes. Make sure you read :doc:`the full list <080-breaking-changes>`.

Ideas for improving Solidity or this documentation are always welcome,
read our :doc:`contributors guide <contributing>` for more details.

.. Hint::

  You can download this documentation as PDF, HTML or Epub by clicking on the versions
  flyout menu in the bottom-left corner and selecting the preferred download format.


Getting Started
---------------

**1. Understand the Smart Contract Basics**

If you are new to the concept of smart contracts we recommend you to get started by digging
into the "Introduction to Smart Contracts" section, which covers:

* :ref:`A simple example smart contract <simple-smart-contract>` written in Solidity.
* :ref:`Blockchain Basics <blockchain-basics>`.
* :ref:`The Ethereum Virtual Machine <the-ethereum-virtual-machine>`.

**2. Get to Know Solidity**

Once you are accustomed to the basics, we recommend you read the :doc:`"Solidity by Example" <solidity-by-example>`
and “Language Description” sections to understand the core concepts of the language.

**3. Install the Solidity Compiler**

There are various ways to install the Solidity compiler,
simply choose your preferred option and follow the steps outlined on the :ref:`installation page <installing-solidity>`.

.. hint::
  You can try out code examples directly in your browser with the
  `Remix IDE <https://remix.ethereum.org>`_. Remix is a web browser based IDE
  that allows you to write, deploy and administer Solidity smart contracts, without
  the need to install Solidity locally.

.. warning::
    As humans write software, it can have bugs. You should follow established
    software development best-practices when writing your smart contracts. This
    includes code review, testing, audits, and correctness proofs. Smart contract
    users are sometimes more confident with code than their authors, and
    blockchains and smart contracts have their own unique issues to
    watch out for, so before working on production code, make sure you read the
    :ref:`security_considerations` section.

**4. Learn More**

If you want to learn more about building decentralized applications on Ethereum, the
`Ethereum Developer Resources <https://ethereum.org/en/developers/>`_
can help you with further general documentation around Ethereum, and a wide selection of tutorials,
tools and development frameworks.

If you have any questions, you can try searching for answers or asking on the
`Ethereum StackExchange <https://ethereum.stackexchange.com/>`_, or
our `Gitter channel <https://gitter.im/ethereum/solidity/>`_.

.. _translations:

Translations
------------
>>>>>>> c4909e99c1015233d6937d229058cde52615f246

Community contributors help translate this documentation into several languages.
Note that they have varying degrees of completeness and up-to-dateness. The English
version stands as a reference.

<<<<<<< HEAD
* `Registro de Cambios <https://github.com/ethereum/solidity/blob/develop/Changelog.md>`_

* `Trabajos Pendientes (Story Backlog) <https://www.pivotaltracker.com/n/projects/1189488>`_

* `Código Fuente <https://github.com/ethereum/solidity/>`_

* `Ethereum Stackexchange <https://ethereum.stackexchange.com/>`_

* `Chat de Gitter <https://gitter.im/ethereum/solidity/>`_

Integraciones de Solidity disponibles
-------------------------------------

* `Remix <https://remix.ethereum.org/>`_
    Entorno integrado de desarrollo (IDE) basado en un navegador que integra un compilador y un entorno en tiempo de ejecución para Solidity sin los componentes orientados al servidor.
    
* `Ethereum Studio <https://live.ether.camp/>`_
    Entorno integrado de desarrollo (IDE) especializado que proporciona acceso a un entorno completo de Ethereum a través de un intérprete de comandos (shell).

* `Plugin IntelliJ IDEA <https://plugins.jetbrains.com/plugin/9475-intellij-solidity>`_
    Plugin de Solidity para IntelliJ IDEA (y el resto de IDEs de JetBrains).

* `Extensión de Visual Studio <https://visualstudiogallery.msdn.microsoft.com/96221853-33c4-4531-bdd5-d2ea5acc4799/>`_
    Plugin de Solidity para Microsoft Visual Studio que incluye un compilador de Solidity.

* `Paquete para SublimeText <https://packagecontrol.io/packages/Ethereum/>`_
    Paquete para resaltar la sintáxis de Solidity en el editor SublimeText.

* `Etheratom <https://github.com/0mkara/etheratom>`_
    Plugin para el editor Atom que ofrece: resaltar la sintaxis, un entorno de compilación y un entorno en tiempo de ejecución (compatible con un nodo en segundo plano y con una máquina virtual).

* `Linter de Solidity para Atom <https://atom.io/packages/linter-solidity>`_
    Plugin para el editor Atom que ofrece linting para Solidity.

* `Linter de Solium para Atom <https://atom.io/packages/linter-solium>`_
    Programa de linting de Solidity configurable para Atom que usa Solium como base.

* `Solium <https://github.com/duaraghav8/Solium/>`_
    Programa de linting de Solidity para la interfaz de línea de comandos que sigue estrictamente las reglas prescritas por la `Guía de Estilo de Solidity <http://solidity.readthedocs.io/en/latest/style-guide.html>`_.

* `Extensión para Visual Studio Code <http://juan.blanco.ws/solidity-contracts-in-visual-studio-code/>`_
    Plugin de Solidity para Microsoft Visual Studio que incluye resaltar la sintaxis y el compilador de Solidity.

* `Emacs Solidity <https://github.com/ethereum/emacs-solidity/>`_
    Plugin para el editor Emacs que incluye resaltar la sintaxis y el reporte de los errores de compilación.

* `Vim Solidity <https://github.com/tomlion/vim-solidity/>`_
    Plugin para el editor Vim que incluye resaltar la sintaxis.
    Plugin for the Vim editor providing syntax highlighting.

* `Vim Syntastic <https://github.com/scrooloose/syntastic>`_
    Plugin para el editor Vim que incluye la verificación de la compilación. 

Descontinuados:

* `Mix IDE <https://github.com/ethereum/mix/>`_
    Entorno integrado de desarrollo (IDE) basado en Qt para el diseño, debugging y testeo de contratos inteligentes en Solidity.


Herramientas para Solidity
--------------------------

* `Dapp <https://dapp.readthedocs.io>`_
    Herramienta de construcción, gestión de paquetes y asistente de despliegue para Solidity.

* `Solidity REPL <https://github.com/raineorshine/solidity-repl>`_
    Prueba Solidity al instante gracias a una consola de línea de comandos de Solidity.

* `solgraph <https://github.com/raineorshine/solgraph>`_
    Visualiza el flujo de control de Solidity y resalta potenciales vulnerabilidades de seguridad.

* `evmdis <https://github.com/Arachnid/evmdis>`_
    Desensamblador de la Máquina Virtual de Ethereum (EVM) que realiza análisis estáticos sobre el bytecode y así proporciona un mayor nivel de abstracción que las operaciones brutas del EVM.

* `Doxity <https://github.com/DigixGlobal/doxity>`_
    Generador de documentación para Solidity.

Analizadores de sintaxis y de gramática alternativos para Solidity
------------------------------------------------------------------

* `solidity-parser <https://github.com/ConsenSys/solidity-parser>`_
    Analizador de sintaxis para JavaScript.

* `Solidity Grammar para ANTLR 4 <https://github.com/federicobond/solidity-antlr4>`_
    Analizador de gramática de Solidity para el generador de sintaxis ANTLR 4.

Documentación del lenguaje
--------------------------

A continuación, primero veremos un :ref:`contrato inteligente sencillo <simple-smart-contract>` escrito en Solidity, seguido de una introducción sobre :ref:`blockchains <blockchain-basics>` y sobre la :ref:`Máquina Virtual de Ethereum <the-ethereum-virtual-machine>`.

En la siguiente sección se explicarán distintas *características* de Solidity con varios :ref:`ejemplos de contratos <voting>`. Recuerde que siempre puede probar estos contratos `en su navegador <https://remix.ethereum.org>`_!.

La última sección (y también la más amplia) cubre, en profundidad, todos los aspectos de Solidity.

Si todavía tiene dudas o preguntas, puede buscar y preguntar en la web de `Ethereum Stackexchange <https://ethereum.stackexchange.com/>`_, o puede unirse a nuestro `canal de gitter <https://gitter.im/ethereum/solidity/>`_. ¡Ideas para mejorar Solidity o esta documentación siempre son bienvenidas!

También existe la `versión rusa (русский перевод) <https://github.com/ethereum/wiki/wiki/%D0%A0%D1%83%D0%BA%D0%BE%D0%B2%D0%BE%D0%B4%D1%81%D1%82%D0%B2%D0%BE-%D0%BF%D0%BE-Solidity>`_.
=======
You can switch between languages by clicking on the flyout menu in the bottom-left corner
and selecting the preferred language.

* `French <https://docs.soliditylang.org/fr/latest/>`_
* `Indonesian <https://github.com/solidity-docs/id-indonesian>`_
* `Persian <https://github.com/solidity-docs/fa-persian>`_
* `Japanese <https://github.com/solidity-docs/ja-japanese>`_
* `Korean <https://github.com/solidity-docs/ko-korean>`_
* `Chinese <https://github.com/solidity-docs/zh-cn-chinese/>`_

.. note::

   We recently set up a new GitHub organization and translation workflow to help streamline the
   community efforts. Please refer to the `translation guide <https://github.com/solidity-docs/translation-guide>`_
   for information on how to start a new language or contribute to the community translations.
>>>>>>> c4909e99c1015233d6937d229058cde52615f246

Contenidos
==========

:ref:`Índice de palabras clave <genindex>`, :ref:`Página de búsqueda <search>`

.. toctree::
   :maxdepth: 2
   :caption: Basics

   introduction-to-smart-contracts.rst
   installing-solidity.rst
   solidity-by-example.rst

.. toctree::
   :maxdepth: 2
   :caption: Language Description

   layout-of-source-files.rst
   structure-of-a-contract.rst
   types.rst
   units-and-global-variables.rst
   control-structures.rst
   contracts.rst
   assembly.rst
   cheatsheet.rst
   grammar.rst

.. toctree::
   :maxdepth: 2
   :caption: Compiler

   using-the-compiler.rst
   analysing-compilation-output.rst
   ir-breaking-changes.rst

.. toctree::
   :maxdepth: 2
   :caption: Internals

   internals/layout_in_storage.rst
   internals/layout_in_memory.rst
   internals/layout_in_calldata.rst
   internals/variable_cleanup.rst
   internals/source_mappings.rst
   internals/optimizer.rst
   metadata.rst
   abi-spec.rst

.. toctree::
   :maxdepth: 2
   :caption: Additional Material

   050-breaking-changes.rst
   060-breaking-changes.rst
   070-breaking-changes.rst
   080-breaking-changes.rst
   natspec-format.rst
   security-considerations.rst
   smtchecker.rst
   resources.rst
   path-resolution.rst
   yul.rst
   style-guide.rst
   common-patterns.rst
   bugs.rst
   contributing.rst
   brand-guide.rst
   language-influences.rst
