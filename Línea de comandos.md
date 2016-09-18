# Línea de comandos:
La ***línea de comandos*** es una interfaz de texto del sistema operativo del ordenador. Para acceder a la línea de comandos utilizamos *terminal*.
Los archivos del equipo están organizados en un sistema de directorios organizados en una estructura en árbol, que comienza en el directorio *raíz* o *root*. Cada directorio padre puede contener más directorios hijos y archivos.
Desde la línea de comandos se puede navegar por el sistema de directorios:

 * `pwd` muestra la ruta del directorio de trabajo actual.
 * `ls` revela todos los archivos y directorios del directorio de trabajo actual.
   Tiene las siguientes opciones:
    * `ls -a` revela todos los contenidos del directorio de trabajo, incluyendo archivos y carpetas ocultas.
    * `ls -l` revela todos los contenidos del directorio de trabajo incluyendo sus propiedades.
    * `ls -t` ordena los archivos y directorios del directorio de trabajo por su fecha de modificación.  
 Pueden usarse varias opciones al mismo tiempo: `ls -alt`.
 * `cd` te dirige al directorio que se especifica a continuación.
 * `mkdir` crea un nuevo directorio en el directorio de trabajo.
 * `touch` crea un nuevo archivo en el directorio de trabajo.

Desde la línea de comandos se pueden copiar, mover y eliminar archivos y directorios:
 * `cp` copia archivos.
 * `mv` mueve y renombra archivos.
 * `rm` elimina archivos.
 * `rm -r` elimina directorios completos.

El símbolo asterisco `*` se utiliza para seleccionar grupos de archivos o directorios.

Existe la posibilidad de redireccionar el *standard input*, el *standard output* y el *standard error* de la siguiente manera:

 * `>` redirecciona el *standard output* de un comando a un archivo sobreescribiendo el contenido.
 * `>>` redirecciona el *standard output* de un comando a un archivo añadiendo el nuevo contenido a continuación.
 * `<` redirecciona el *standard input* a un comando.
 * `|` redirecciona el *standard output* de un comando a otro comando.

Hay otros comandos muy útiles cuando se usan conjuntamente con las redirecciones:

 * `sort` ordena las líneas de un texto alfabéticamente.
 * `uniq` filtra las líneas consecutivas que están repetidas.
 * `grep` busca y muestra el texto que sigue un patrón indicado.
 * `sed` modifica y muestra el texto que sigue un patrón indicado.

El *entorno* o *environment* se refiere a las configuraciones y preferencias del usuario.  
El editor de texto de línea de texto `nano`, se emplea para configurar el entorno.  
`~/.bash_profile` es el lugar donde se almacenan las configuraciones del entorno. Puede ser editado empleando `nano`.  
Las *variables de entorno* o *environment variables* son variables que se pueden incluir en comandos y programas y contienen información sobre el entorno.

 * `export VARIABLE="Value"` crea una variable de entorno.
 * `USER` es el nombre del usuario actual.
 * `PS1` es el marcador de la línea de comandos.
 * `HOME` es el directorio principal. No debe ser modificado.
 * `PATH` revela una lista de las rutas de archivos separadas por comas. Se puede modificar para usos avanzados.
 * `env` revela una lista con las variables de entorno.
