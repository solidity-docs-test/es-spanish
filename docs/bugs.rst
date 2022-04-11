.. index:: Bugs

.. _known_bugs:

##########################
Lista de errores conocidos
##########################

Abajo encontrarás una lista en formato JSON de algunos de los errores de seguridad
en el compilador Solidity. El archivo en sí está alojado en el `repositorio de Github
<https://github.com/ethereum/solidity/blob/develop/docs/bugs.json>`_.
La lista comienza con la versión 0.3.0, y sólo los errores conocidos antes de esa versíon no
están listados.

Hay otro archivo llamado `bugs_by_version.json
<https://github.com/ethereum/solidity/blob/develop/docs/bugs_by_version.json>`_,
que puede usarse para revisar qué errores afectan a una versión específica del compilador.

Herramientas de verificación de código fuente y otras herramientas para contratos
deben consultar esta lista con los siguientes criterios:

<<<<<<< HEAD
 - Es relativamente sospechoso que un contrato se haya compilado con una versión
   nightly y no con una release. Esta lista no mantiene un registro de las versiones
   nightly.
 - También es sospechoso cuando un contrato fue compilado con una versión que no era
   la más reciente en el momento que el contrato fue creado. Para contratos
   creados de otros contratos, tienes que seguir la cadena de creación de
   vuelta a una transacción y revisar la fecha de esa transacción.
 - Es muy sospechoso que un contrato haya sido compilado con un compilador que contiene
   un error conocido, y que se cree cuando una versión del compilador con una corrección
   ya haya sido publicada.
=======
- It is mildly suspicious if a contract was compiled with a nightly
  compiler version instead of a released version. This list does not keep
  track of unreleased or nightly versions.
- It is also mildly suspicious if a contract was compiled with a version that was
  not the most recent at the time the contract was created. For contracts
  created from other contracts, you have to follow the creation chain
  back to a transaction and use the date of that transaction as creation date.
- It is highly suspicious if a contract was compiled with a compiler that
  contains a known bug and the contract was created at a time where a newer
  compiler version containing a fix was already released.
>>>>>>> 140e59d1908be63964ae51efd23ceb83cdf2f7b8

El archivo JSON de errores conocidos es una lista de objetos, uno por cada error,
con la siguiente información:

<<<<<<< HEAD
nombre (name)
    Nombre único asignado al error
resumen (summary)
    Pequeña descripción del error
descripción (description)
    Descripción detallada del error
enlace (link)
    URL de un sitio web con más información, opcional
introducido (introduced)
    La primera versión del compilador que contiene ese error, opcional
corregido (fixed)
    La primera versión del compilador que ya no contiene el error
publicado (publish)
    La fecha en la cual el error se hizo públicamente conocido, opcional
gravedad (severity)
    Gravedad del error: baja, media, alta. Considera la facilidad
    de ser detectado en tests de contratos, probabilidad de ocurrir
    y potencial daño.
condiciones (conditions)
    Condiciones que tienen que cumplirse para iniciar el error. Actualmente
    es un objeto que puede contener un valor booleano ``optimizer``, que
    significa que el optimizador tiene que ser activado para reproducir el error.
    Si no se da ninguna condición, se da por hecho que el error está presente.
=======
uid
    Unique identifier given to the bug in the form of ``SOL-<year>-<number>``.
    It is possible that multiple entries exists with the same uid. This means
    multiple version ranges are affected by the same bug.
name
    Unique name given to the bug
summary
    Short description of the bug
description
    Detailed description of the bug
link
    URL of a website with more detailed information, optional
introduced
    The first published compiler version that contained the bug, optional
fixed
    The first published compiler version that did not contain the bug anymore
publish
    The date at which the bug became known publicly, optional
severity
    Severity of the bug: very low, low, medium, high. Takes into account
    discoverability in contract tests, likelihood of occurrence and
    potential damage by exploits.
conditions
    Conditions that have to be met to trigger the bug. The following
    keys can be used:
    ``optimizer``, Boolean value which
    means that the optimizer has to be switched on to enable the bug.
    ``evmVersion``, a string that indicates which EVM version compiler
    settings trigger the bug. The string can contain comparison
    operators. For example, ``">=constantinople"`` means that the bug
    is present when the EVM version is set to ``constantinople`` or
    later.
    If no conditions are given, assume that the bug is present.
check
    This field contains different checks that report whether the smart contract
    contains the bug or not. The first type of check are Javascript regular
    expressions that are to be matched against the source code ("source-regex")
    if the bug is present.  If there is no match, then the bug is very likely
    not present. If there is a match, the bug might be present.  For improved
    accuracy, the checks should be applied to the source code after stripping
    comments.
    The second type of check are patterns to be checked on the compact AST of
    the Solidity program ("ast-compact-json-path"). The specified search query
    is a `JsonPath <https://github.com/json-path/JsonPath>`_ expression.
    If at least one path of the Solidity AST matches the query, the bug is
    likely present.
>>>>>>> 140e59d1908be63964ae51efd23ceb83cdf2f7b8

.. literalinclude:: bugs.json
   :language: js
