# Triggers
GitHub Actions nos ofrece una amplia lista de 'triggers' que pueden lanzar nuestros 'workflows'. Algunos de los más comunes incluyen 'push', 'pull request', 'issue', 'issue comment', 'workflow dispatch', y 'schedule'.
* ¿Cómo funciona el 'trigger' de 'push'?
Cuando haces 'push' de un nuevo 'commit' en alguna rama que tú definas, puedes activar un 'workflow'.
Esta acción también se puede configurar para todas las ramas y puede especificar a qué ramas afectará mediante la opción 'branches'. Y con la opción 'paths' puedes especificar qué archivos deben modificarse para que el 'workflow' se active.

```
on: #Acaparará los triggers de este workflow
	push: #El tipo de trigger: push
		branches: #Las ramas en las que se activará el workflow
			- 'main' #Se activará si se hace push en la rama main
			- 'releases/**' #O en la rama releases y derivados
		paths: #Serán las rutas que se tendrán en cuenta para activar el workflow
			- '**.js' #Todos los archivos .js en el repositorio
```

* ¿Cómo se utiliza el 'trigger' de 'pull request'?
 El 'trigger' 'pull request' ofrece características muy similares al de 'push'. Esto incluye 'branches' y 'paths', y también incluye 'types', que te permite especificar sobre qué acciones sobre 'pull request' quieres que se active.


```
on: #Acaparará los triggers de este workflow
	pull_request: #El tipo de trigger: pr
		types: #Los estados que activarán el trigger
			- [opened, labeled] #Tomará los estados del PR
		branches: #Las ramas en las que se activará el workflow
			- 'releases/**' #En la rama releases y derivados
		paths: #Serán las rutas que se tendrán en cuenta para activar el workflow
			- '**.js' #Todos los archivos .js en el repositorio
```

 
* ¿Cómo manejar la opción 'issues' y 'issue comment'?

El 'trigger' 'issues' funciona de manera similar al 'pull request', y tiene los mismos 'types'. Sin embargo, 'issue comment' funciona cuando se hacen comentarios nuevos sobre un 'issue' o 'pull request', y también puedes especificar si la acción se ejecuta solo en los comentarios de un 'pull request'.
```
on: 
 issues:
  types: [opened,closed,edited]
```
```
on: 
 issue_comment:
  types: [opened,edited]
on:
 issue_comment:
 jobs:
  pr_commeted:
    name: PR comment
    if: $ {{ github.event.issue.pull_request}}
```
* ¿Cómo lanzar 'workflows' de forma manual con 'workflow dispatch'?

El 'workflow dispatch' te permite lanzar un 'workflow' de forma manual y agregar los parámetros que desees. Puedes crear 'inputs', y estos pueden ser de diferentes tipos, como una elección, un boolean, o un string.
```
on: 
 workflow_dispatch:
   inputs:
    message:
      description: 'nivel'
      required: true
      default: 'hola'
      type: choice
      options: ['hola', 'adios']
    tags:
      description: 'opcional'
      required: false
      ****type: boolean
    enviromen:
      description:' objectivo'
      required: true
      type: string
```

* ¿Cómo programar eventos con el 'schedule'?

Finalmente, el 'trigger' 'schedule' te permite programar eventos que ocurran a intervalos regulares. Puedes especificar los minutos, la hora, el día del mes, el mes, y el día de la semana.
```
on: 
 schedule:
 - cron: "0 23 * * *"
```
<img src="https://airplane.ghost.io/content/images/size/w1000/2022/01/crontab.jpeg" atl="imagen de cron" >

* ¿Cómo puedes practicar la creación de 'workflows' basados en 'triggers'?

Para practicar la creación de 'workflows', te recomiendo que crees un nuevo archivo 'workflow' que use al menos tres de los 'triggers' que acabamos de explicar.

## Expresiones
Puedes utilizar expresiones para establecer variables de entorno de forma programática en archivos de flujo de trabajo y acceder a contextos. Una expresión puede ser cualquier combinación de valores literales, referencias a un contexto o funciones. Puedes combinar literales, referencias a contextos y funciones utilizando operadores.
Las expresiones se utilizan comúnmente con la palabra clave condicional "if" en un archivo de flujo de trabajo para determinar si un paso debe ejecutarse. Cuando una condición "if" es verdadera, el paso se ejecutará.

Es necesario emplear una sintaxis específica para indicarle a GitHub que evalúe una expresión en lugar de tratarla como una cadena." ${{ <expresión> }} "
```
name: expresiones
run-name: mi primera workflow de expresion
name: contexto
on:
 push:
  branches:
    - main
jobs:
	check-main:
		if: ${{ github.ref == 'refs/heads/main' }}
		runs-on: ubuntu-latest
		steps:
			- run : echo "Desplegando en la rama $GITHUB_REF"


```
### Contextos
Ahora que dominamos las expresiones, es tiempo de enfocarnos en los contextos. Un contexto en GitHub Actions es una forma de acceder a información relevante sobre ejecuciones de workflows, variables, entorno de runners, jobs y steps.

Exploraremos los diferentes tipos de contextos que GitHub Actions nos ofrece y lo que podemos obtener de cada uno de ellos.
* ¿Cuáles son los principales contextos que nos ofrece GitHub Actions?

GitHub Actions ofrece una variedad de contextos, los más destacados son
GitHub Context: Detalles ejecución workflow. Ejemplo: github.actor obtiene nombre usuario actual
ENV Context: Acceso variables en flujo, job o pasos
Contexto Vars: Acceso vars guardadas a nivel de ambiente, repo u org
Contexto Job: Info job actual. Acceso por nombre o identificador
Contexto Steps: Info steps actuales
Contexto Runner: Acceso info runner job
Contexto Secrets: Acceso a nombres y valores secretos en repo u org
Contexto Inputs: Usar entradas info en ciertos triggers.

* ¿Cómo se usa un contexto en un workflow file?

Un ejemplo de uso de contexto es un workflow file llamado "contexto" que se ejecuta en cada push a cualquier rama. Dentro de este file, hay un job llamado "checkMain" que verifica si estamos en la rama main usando el contexto GitHub y el atributo github.ref. Si estamos en la rama main, este job imprimirá "Desplegando en la rama main" en la terminal.

Los contextos nos permiten acceder a información relevante de GitHub Actions, abriendo puertas para una configuración más innovadora y eficiente de nuestros jobs. 

* ¿Qué son los Secretos en GitHub y cómo configurarlos?

En la sección de Configuración de nuestro repositorio, además de las variables, encontramos una opción para configurar los secretos. Los secretos son similares a las variables, pero suelen contener datos más sensibles, como las contraseñas de las cuentas que necesitamos para configurar una acción, o un valor de configuración que si se filtra podría poner en peligro la seguridad de nuestra aplicación o usuarios.

jobs: Saludo_secretos: runs-on: ubuntu-latest steps: - name: Echo de saludo con secreto run: echo "${{ secrets.SALUDO }} ${{ secrets.NOMBRE }}, tu contraseña secreta es ${{ secrets.CONTRASEÑA }}" ```
¿Cómo se utilizan las Variables y Secretos en Producto?

Las variables y secretos varían en sus usos. A diferencia de los secretos, las variables son utilizadas para guardar valores que no son sensibles. Mientras tanto, los secretos resguardan información altamente sensible, como contraseñas o credenciales.

¿Qué es un flujo de despliegue continuo y cómo se realiza?

El flujo de despliegue continuo utiliza la imagen subida en el paso anterior, y la lanza para desplegarla utilizando los servicios proporcionados por los proveedores cloud. Entre estos servicios se incluyen:

    AppRunner de AWS
    CloudRun de GCP
    Azure Container Instances de Azure

Elegir entre estas tres opciones está sujeto a tus necesidades específicas. No es necesario que en tu proyecto personal debas realizarlo en estos tres proveedores o siquiera tengas que subirlo en estos servicios.

* ¿Cómo se configura el flujo de despliegue continuo en AWS?

Para AWS, inicialmente configuramos las credenciales de AWS dentro de nuestro runner utilizando el action oficial de AWS. Creamos un archivo de configuración para AppRunner, especificando la imagen del ECR que creamos en el paso anterior. Cada vez que se detecta un cambio en el proyecto, este cambio se prueba, se compila, y podemos de forma manual indicarle a GitHub que lo despliegue.

Integrar imágenes de Docker en distintos servicios en la nube puede parecer un desafío, pero GitHub y sus workflows facilitan el proceso. Aquí te mostramos cómo hacerlo con ejemplos en AWS, GCP y Azure.
* ¿Cómo comenzar en GitHub?

Al abordar la segunda etapa de la integración continua, regresar a GitHub es crucial. Aquí se enfoca en la compilación, que se activa con dos tipos de eventos: mediante un workflow dispatch o un comentario en un issue. La selección de AWS, GCP o Azure se hace mediante un input de opción.
¿Cuál es el propósito del job 'docker-aws'?

El job 'docker-aws' es el trabajo inicial y se ejecuta para compilar la imagen y subirla al registro de AWS. Esto ocurre solo cuando se cumple una condición: el evento que activó el flujo fue un pull request y el comentario del pull request contiene 'build-aws'.

* ¿Cómo se configuran QEMU y Docker BuildX?

Para generar los archivos requeridos para la imagen Docker, se configura QEMU, una opción especial de Docker, usando la acción oficial de Docker llamada setupQemuAction. Además, se emplea otra acción para configurar Docker BuildX, que controla la construcción de las imágenes y es recomendado para entornos de producción.

* ¿Cómo subir la imagen a AWS?

Para cargar la imagen en AWS, se requiere configurar credenciales de un usuario IAM de AWS que contengan accessKeyId y secretAccessKey. Luego, se realiza la autenticación en el Elastic Container Registry (ECR) de AWS, el servicio para cargar y mantener imágenes Docker, usando la acción AmazonECRlogin.

* ¿Cómo añadir la imagen en otros proveedores cloud?

Además de AWS, la imagen se puede añadir en GCP y Azure. En GCP, el registro se denomina 'Artifact Registry'; en Azure, es 'Azure Container Registry'. Para cada opción, busca acciones en el Marketplace y consulta en comunidades cómo otras personas han subido sus imágenes de Docker.

En un mundo donde la automatización es sinónimo de mayor eficiencia y productividad, GitHub Actions se ha revelado como un aliado de oro. Sin embargo, a medida que íbamos implementando GitHub Actions en un proyecto open source, surgió un conjunto de buenas prácticas que decidimos hacer explícitas para optimizar el flujo de trabajo en cualquier proyecto de desarrollo.

A continuación, se detalla una serie de recomendaciones transformadas del discurso a la escritura para maximizar tu rendimiento con GitHub Actions.
* ¿Por qué es recomendable usar workflows pequeños en GitHub Actions?

Usar workflows pequeños en GitHub Actions es una práctica sugerida. Estos permiten mayor modularización y facilitan la detección y corrección de errores en caso de fallos. La idea detrás de esta práctica es tener workflows definidos para cada proceso en lugar de tener un solo workflow que maneje todo el flujo.
* ¿Cuál es la importancia de usar timeouts en workflows GitHub Actions?

Los timeouts son parámetros que puedes establecer en los jobs de tus workflows. Estos son cruciales para evitar que un proceso corra indefinidamente debido a un error, lo que podría aumentar el tiempo de ejecución de los runners y, en consecuencia, incrementar los costos - en el caso de los repositorios privados, GitHub cobra por la cantidad de minutos que los runners están en funcionamiento.
* ¿Qué precauciones se deben tener al utilizar actions de terceros en GitHub Actions?

Con más de 17,000 actions disponibles en el Marketplace de GitHub, la tentación de utilizar actions de terceros puede ser grande. Pero ten en cuenta que al agregar actions de terceros a tus workflows es similar a agregar una nueva dependencia a tu proyecto.

Asegúrate de que las actions sean de un autor confiable, tengan actividad reciente en su repositorio y no presenten amenazas de seguridad para tu proyecto.
* ¿Por qué es importante especificar la versión de los actions en GitHub Actions?

Especificar la versión de los actions impide que tu trabajo dependa de actualizaciones imprevistas que podrían agregar errores a tu trabajo. Considéralo una forma de garantizar la estabilidad de tu proyecto. También puedes usar el SHA de GitHub, el hash que se genera para un commit específico cuando se requiere alta sensibilidad y fiabilidad en el trabajo.
* ¿Cómo puede limitarse los permisos del token en GitHub Actions para aumentar la seguridad?

Una de las claves para una buena seguridad en GitHub Actions es limitar los permisos del token. El token predeterminado que crea GitHub puede ser configurado para tener sólo permisos de lectura, agregando permisos de escritura sólo cuando sea absolutamente necesario. Esta es una forma eficaz de reducir vulnerabilidades y mantener a salvo tu proyecto en caso de un ataque a un repositorio de terceros.

