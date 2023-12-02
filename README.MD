# GITHUB ACTIONS
GitHub Actions permite a los desarrolladores automatizar, personalizar y ejecutar sus flujos de trabajo de software directamente en GitHub.

Uno de los elementos claves para comenzar es la creación de un workflow file, también conocido como yml file. Por ejemplo, un archivo llamado triggers.yml puede hacer que GitHub Actions reconozca que ha completado un desafío específico.
¿Cómo se crea un workflow file?

Primero, debe asignar un nombre descriptivo a su workflow, por ejemplo, triggers. A esto, a veces se le llama run name. Este término se refiere a lo que aparecerá en la consola o en la sección de acciones de GitHub.

Es importante porque a menudo hay más de un workflow en un repositorio, por lo que un run name claro puede ayudar a diferenciarlos.
¿Cómo se seleccionan los triggers correctos en un workflow file?

Se pueden usar una amplia variedad de triggers para iniciar un workflow. Estos pueden incluir:

    Push, que ejecuta el workflow cada vez que se realiza un 'push' a la rama maestra.
    Pull Request, que se activa cada vez que se abre un 'pull request'.
    Workflow Dispatch, que permite a los usuarios ejecutar workflows manualmente.
    Schedule, que ejecuta workflows en un horario específico.

¿Cómo se utilizan las expresiones en GitHub Actions?

Las expresiones son elementos indispensables en GitHub Actions. Permiten acceder a valores literales o de variables, comparar diferentes expresiones y utilizar varias funciones.

Existen muchas funciones comunes, como if, contains, toJson y fromJson, que desempeñan roles esenciales en el control de los workflows. Por ejemplo, la función if se puede usar para determinar si se ejecuta un trabajo, mientras que contains puede restringir la ejecución a situaciones específicas.

Por ahora, te animo a jugar con lo que has aprendido y te desafío a crear un archivo workflow llamado 'expresiones.yml' que utilice al menos tres de las expresiones que hemos discutido. Para una guía completa de todas las funciones disponibles de GitHub Actions

