[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=19144038&assignment_repo_type=AssignmentRepo)
# Lab03: Visualización de Datos en Raspberry Pi Zero W

## Integrantes


## Documentación

Se explica cada proceso realizo en el codigo


Importaciones

El código importa matplotlib.pyplot para crear gráficos interactivos, time para manejar intervalos y marcas temporales, subprocess para ejecutar comandos del sistema como la lectura de temperatura, csv para guardar datos en formato tabular, y os para verificar y manipular archivos en el sistema. Estas librerías son esenciales para integrar las funcionalidades de monitoreo, almacenamiento y visualización.

Clase MonitorTemperaturaRPI

Esta clase encapsula toda la lógica del programa, inicializando parámetros como la duración máxima del monitoreo (60 segundos) y el intervalo entre lecturas (0.5 segundos). También configura el archivo CSV para almacenar datos y prepara el entorno gráfico interactivo. Su estructura permite un manejo ordenado de los datos y su visualización en tiempo real.

Método __init__

El constructor inicializa las variables clave, como las listas tiempos y temperaturas, y guarda el momento de inicio del monitoreo. Verifica si el archivo CSV existe; si no, lo crea con los encabezados adecuados. Además, configura el gráfico interactivo usando plt.ion() y prepara los ejes para la visualización dinámica de datos.

Método leer_temperatura

Este método ejecuta el comando vcgencmd measure_temp para obtener la temperatura actual de la CPU en formato string. Luego, procesa el texto para extraer el valor numérico, eliminando caracteres innecesarios, y lo convierte a float. Si ocurre un error durante la lectura, muestra un mensaje y retorna None para manejar fallos sin interrumpir el programa.

Método actualizar_datos

Calcula el tiempo transcurrido desde el inicio y obtiene una nueva lectura de temperatura. Si la lectura es válida, añade los datos a las listas correspondientes y los guarda en el CSV. Además, elimina registros antiguos que superen el límite de duracion_max, manteniendo solo los datos relevantes en memoria para optimizar el rendimiento.

Método graficar

Limpia el gráfico anterior y dibuja una nueva línea con los datos actualizados de tiempo y temperatura. Configura el título, etiquetas de los ejes y activa la cuadrícula para mejor legibilidad. Finalmente, actualiza la visualización con draw() y flush_events() para garantizar que los cambios se reflejen en tiempo real.

Método guardar_csv

Abre el archivo CSV en modo de añadido ('a') para preservar los datos previos. Escribe una nueva fila con el tiempo redondeado a dos decimales y la temperatura actual. Este enfoque asegura que los datos se almacenen de forma persistente, permitiendo su análisis posterior sin afectar el rendimiento en tiempo real.

Método ejecutar

Es el bucle principal del programa, que actualiza los datos y el gráfico en cada iteración, respetando el intervalo definido. Detecta interrupciones del usuario (como Ctrl+C) para finalizar el monitoreo de manera controlada. Al terminar, desactiva el modo interactivo de matplotlib y cierra la figura para liberar recursos correctamente.

Bloque principal

Verifica si el script se ejecuta directamente (no como módulo) y, en ese caso, crea una instancia de MonitorTemperaturaRPI e inicia el monitoreo llamando a ejecutar(). Este bloque garantiza que el código solo se ejecute cuando es invocado directamente, facilitando su integración en otros proyectos si fuera necesario.

presentamos el diagrama de flujo


![Diagrama de flujo](tempe.png)

## Preguntas
1. ¿Qué función cumple plt.fignum_exists(self.fig.number) en el ciclo principal?
Verifica si la ventana del gráfico sigue activa. Si el usuario la cierra, el bucle termina ordenadamente sin errores, evitando intentar actualizar una figura inexistente.

2. ¿Por qué se usa time.sleep(self.intervalo) y qué pasa si se quita?
Pausa el bucle para controlar la frecuencia de lecturas (0.5s por defecto). Sin esto, el código consumiría el 100% de la CPU al ejecutarse en un bucle infinito sin esperas.

3. ¿Qué ventaja tiene usar __init__ para inicializar listas y variables?
Centraliza la configuración inicial, haciendo el código más organizado y reusable. Permite crear múltiples instancias con distintos parámetros fácilmente.

4. ¿Qué se está midiendo con self.inicio = time.time()?
Registra el momento exacto en que comienza el monitoreo. Luego se usa para calcular el tiempo transcurrido (eje X del gráfico) restando este valor a cada nueva lectura.

5. ¿Qué hace exactamente subprocess.check_output(...)?
Ejecuta el comando de sistema vcgencmd measure_temp para medir la temperatura de la CPU en la Raspberry Pi, y captura su salida como texto para procesarla.

6. ¿Por qué se almacena ahora = time.time() - self.inicio en lugar del tiempo absoluto?
Usar tiempo relativo (desde el inicio) simplifica el gráfico y análisis, mostrando siempre 0s como punto de partida en el eje X para mejor visualización.

7. ¿Por qué se usa self.ax.clear() antes de graficar?
Limpia el gráfico anterior para evitar superponer múltiples líneas. Sin esto, cada actualización dibujaría una nueva curva sobre las anteriores, haciendo ilegible la gráfica.

8. ¿Qué captura el bloque try...except dentro de leer_temperatura()?
Detecta errores al ejecutar vcgencmd (como fallos de hardware o permisos), evitando que el programa se cierre y permitiendo notificar el problema al usuario.

9. ¿Cómo podría modificar el script para guardar las temperaturas en un archivo .csv?
El código ya lo hace en temperaturas.csv. Para modificarlo, cambia el nombre en __init__ o edita guardar_csv() para añadir más columnas (como fecha exacta o métricas adicionales).

