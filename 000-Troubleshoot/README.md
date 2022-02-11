# Troubleshoot

<br><br>

### Ejemplos reales.

<div align="center"> 
	<img src="img/error-137.png" /> 
	<br>
	<img src="img/oom-137.png" /> 
</div>
<br><br>


### Codigos de error de un pod 
|Exit code  | Tipo                                    | Descripcion |
|-          |-                                        |-            |
|0          |detenido a propósito                     | Utilizado para indicar que el contenedor se detuvo automáticamente  |
|1          |Error de la aplicación                   | El contenedor se detuvo debido a un error de la aplicación o una referencia a un archivo que no existe dentro del contenedor |
|125        |El contenedor no pudo ejecutar el error  | El comando que corre en el contenedor no se ejecutó correctamente  |
|126        |Error de invocación de comando           | No se pudo invocar un comando especificado en el contenedor |
|127        |Archivo o directorio no encontrado       | No se encontró el archivo o directorio especificado en la especificación en el contenedor |
|128        |Argumento no válido utilizado al salir   | La salida se activó con un código de salida no válido (los códigos válidos son números enteros entre 0 y 255) |
|134        |Terminación anormal (SIGABRT)            | El contenedor se abortó usando la función abort(). [Ref](https://en.wikipedia.org/wiki/Signal_(IPC)) |
|137        |Terminación inmediata (SIGKILL)          | El contenedor recibio la señal SIGKILL del sistema operativo host. Esta señal indica a un proceso que finalice inmediatamente, sin período de gracia.  [Ref](https://en.wikipedia.org/wiki/Signal_(IPC)) |
|139        |Fallo de segmentación (SIGSEGV)          | El contenedor intentó acceder a la memoria que no se le asignó y se canceló  [Ref](https://en.wikipedia.org/wiki/Signal_(IPC)) |
|143        |Terminación elegante (SIGTERM)           | El contenedor recibió una advertencia de que estaba a punto de cancelarse y luego terminó   [Ref](https://en.wikipedia.org/wiki/Signal_(IPC))  |
|255        |Estado de salida fuera de rango          | Se salió del contenedor y se devolvió un código de salida fuera del rango aceptable, lo que significa que se desconoce la causa del error |

<br><br>

### Comandos cotidianos
| comando | descripcion | 
| -       | -           | 
|kubectl -n foobar get pod MI-POD        |Muestra todos los pods en el namespace foobar  |
|kubectl -n foobar top pods              |Muestra el top (mem/cpu) de todos los pods del namespace foobar |
|kubectl -n foobar describe pod  MI-POD  |Muestra informacion detallada del pod (es como la radiografia del pod) |
|kubectl -n foobar logs MI-POD           |Muestra los logs del pod  (DATO: acá no van a ver logs de la aplicacion si esta no los expone en STDOUT)  |
|kubectl -n foobar get pod MI-POD -o yaml|Muestra en formato yaml como esta configurado ese pod (es como la receta para que ese pod funcione)  |
|kubectl -n foobar get events            |Muestra eventos de todo tipo registrados por el cluster en el namespace foobar (si usan "get events -A" van a ver eventos de todos los ns)  |
|kubectl get nodes 						 |Muestra los nodos que estan corriendo |
|kubectl top nodes 						 |Muestra el top (mem/cpu) de los nodos |
|kubectl describe node MI-NODO           |Muestra informacion SUPER util del nodo|

<br><br>

### Eventos:
|namespace| tiempo| razon  | tipo | origen | descripcion |
|-        |-      |-       |-     |-       |-            |
|default  | 51m   | Warning|SystemOOM| node/ip-172-25-51-209.ec2.internal        | System OOM encountered, victim process: telegraf, pid: 28886 |
|foobar  | 5m53s  | Normal |Pulling  | pod/foobar-telegraf-rds-785547578c-mbzk4 | Pulling image "foobar-foobar/sre/telegraf:latest" |

<br><br>

### Fases de inicio de un POD (cuando arranca la primera vez)
| Estado 	| Descripcion |
|-          |-       |
| Pending 	| El cluster recibio la accion de levantar el pod, pero el o los containers dentro del mismo aún no estan listos o aún no se esta ejecutando el servicio/programa. Includido el tiempo que pasa descargando imágenes de contenedores a través de la red. | 
| Running 	| El pod se asocio a un nodo y uno o mas contenedores dentro del pod estan corriendo. Chequear "READY" = 1/1, 2/2 o n/n. En algunos casos se muestra como 1/2, esto significa que el pod esta corriendo pero uno de los containers no esta listo aún.  (aunque puede figurar como Running)
Succeeded 	All containers in the Pod have terminated in success, and will not be restarted.| 
| Failed 	| Todos los contenedores en el Pod terminaron y por lo menos un contenedor terminó con alguna falla. El contenedor termino con un estado distinto de 0 o el sistema lo finalizó.  (ver los Exit code) | 
| Unknown 	| No es muy común, pero por alguna razón kubernetes pudo obtener el estado del Pod. Este estado generalmente ocurre debido a un error en la comunicación con el nodo donde se ejecuta el Pod.  (chequear los nodos, o chequear los eventos del cluster | 

<br><br>

### Estados de un POD (cuando esta corriendo, luego de pasar la Fase de inicio)
| Estado | descripcion | 
|-        |-            | 
| Waiting | Si un contenedor no está en el estado Running o Terminated, está "esperando". Un pod o contenedor en el estado de espera sigue ejecutando las operaciones básicas para completar el inicio: ej, pullear las imagenes del registry, aplicando configmaps de secretos, etc . Cuando se chequea un pod con un contenedor que está en espera, se puede ver un campo "Reason" para resumir por qué el contenedor está en ese estado. |
|Running | El estado Running indica que un contenedor se está ejecutando Ok. En este punto ya deberia haber pasado todos los test (readynes,livenes, etc, etc) y el contenedor (la aplicacion) esta funcionando con normalidad. |
| Terminated | Un pod o contenedor en el estado Terminated indica que comenzó a ejecutarse y luego se ejecutó hasta completarse o bien falló por alguna razón (ver codigos de error).|
| Completed | Un pod en este estado indica que comenzó, ejecuto la tarea que debia realizar y finalizo correctamente (como si fuera con exit 0). Este tipo de estados se puede visualizar en pods que corren como "job"  (tareas tipo crontab)

<br><br>