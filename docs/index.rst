Solidity
========

.. image:: logo.svg
    :width: 120px
    :alt: Solidity logo
    :align: center

Solidity es un lenguaje orientado a objetos, de alto nivel para la implementación de contratos inteligentes. Los contratos inteligentes son programas que gobiernan el comportamiento de las cuentas dentro de Ethereum.

Solidity fue influenciado por C++, Python, JavaScript y diseñado para ser utilizado por la Máquina Virtual de Ethereum (EVM).

Solidity está tipado de manera estática y acepta, entre otras cosas, herencia, bibliotecas y tipos complejos definidos por el usuario.

Con Solidity es posible crear contratos para votar, para el crowdfunding, para subastas a ciegas y para monederos multi firmas.

Al desplegar contratos, deberias usar la última versión liberada de Solidity. Fuera de casos excepcionales, sólo la última versión recibe `Actualizaciones de Seguridad <https://github.com/ethereum/solidity/security/policy#supported-versions>`_.
Además, tanto cambios importantes como nuevas características son introducidas regularmente. Actualmente utilizamos un sistema de numeración de versiones 0.y.z `para indicar este ritmo rápido de cambio <https://semver.org/#spec-item-4>`_.

.. note::
    La mejor manera de probar Solidity ahora mismo es usando `Remix <https://remix.ethereum.org/>`_ (puede tardar un rato en cargarse, por favor sea paciente).

Enlaces útiles
--------------

* `Ethereum <https://ethereum.org>`_

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

Contenidos
==========

:ref:`Índice de palabras clave <genindex>`, :ref:`Página de búsqueda <search>`

.. toctree::
   :maxdepth: 2

   introduction-to-smart-contracts.rst
   installing-solidity.rst
   solidity-by-example.rst
   solidity-in-depth.rst
   security-considerations.rst
   using-the-compiler.rst
   abi-spec.rst
   style-guide.rst
   common-patterns.rst
   bugs.rst
   contributing.rst
   frequently-asked-questions.rst
