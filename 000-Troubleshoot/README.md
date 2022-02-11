# Troubleshoot





### Error codes

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
