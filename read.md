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


