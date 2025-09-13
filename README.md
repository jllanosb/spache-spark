# Instalacion de Apache Spark en Lunix (Ubuntu)

Apache Spark es un motor de análisis unificado para el procesamiento de datos a gran escala. Gracias a su alta velocidad de procesamiento en memoria, la plataforma es popular en entornos de computación distribuida.

Spark admite diversas fuentes y formatos de datos y puede ejecutarse en clústeres independientes o integrarse con Hadoop , Kubernetes y servicios en la nube . Al ser un framework de código abierto , admite diversos lenguajes de programación como Java , Scala, Python y R.

En este tutorial, aprenderá cómo instalar y configurar Spark en Ubuntu.

### Pre-Requisitos

- Un sistema Ubuntu.
- Acceso a una terminal o línea de comandos.
- Un usuario con permisos sudo o root.

# Instalación de Spark en Ubuntu

Los ejemplos de este tutorial se presentan utilizando Ubuntu 24.04 y Spark 4.0.1

## Lista de paquetes del sistema de actualización

Para garantizar que las dependencias de Ubuntu estén actualizadas, abra una ventana de terminal y actualice la lista de paquetes del repositorio en su sistema:

```markdown
sudo apt update
```

## Instalar dependencias de Spark (Git, Java, Scala)

Antes de descargar y configurar Spark, instale las dependencias necesarias. Esto incluye los siguientes paquetes:

- JDK (Kit de desarrollo de Java)
- Escala
- Git

Introduzca el siguiente comando para instalar los tres paquetes:

```markdown
sudo apt install default-jdk scala git -y
```

Utilice el siguiente comando para verificar las _*dependencias*_ instaladas:

```bash
java -version; javac -version; scala -version; git --version
```
La salida muestra las versiones de OpenJDK, Scala y Git.

## Descargar Apache Spark en Ubuntu

Puede descargar la última versión de Spark desde el *sitio web de Apache*. Para este tutorial, se usa Spark 4.0.1 con *Hadoop 3*, la versión más reciente al momento de escribir este repositorio.

Utilice el _*comando wget*_ y el enlace directo para descargar el archivo Spark:

```markdown
wget https://dlcdn.apache.org/spark/spark-4.0.1/spark-4.0.1-bin-hadoop3.tgz
```

Una vez completada la descarga, verás el mensaje guardado.

##### *Nota*: Si descarga una versión diferente de Apache Spark, reemplace el número de versión de Spark en los comandos posteriores.

Para verificar la integridad del archivo descargado, recupere la _*suma de comprobación*_ SHA-512 correspondiente :

```bash
wget https://downloads.apache.org/spark/spark-4.0.1/spark-4.0.1-bin-hadoop3.tgz.sha512
```

Compruebe si la suma de comprobación descargada coincide con la suma de comprobación del archivo tar de Spark:

```bash
shasum -a 512 -c spark-4.0.1-bin-hadoop3.tgz.sha512
```
Resultado: spark-4.0.1-bin-hadoop3.tgz : OK
El mensaje *OK* indica que el archivo es legítimo.

## Extraer el paquete Apache Spark

Extraiga el archivo descargado usando el *comando tar*:

```bash
tar xvf spark-*.tgz
```

La salida muestra los archivos que se están descomprimiendo del archivo comprimido. Use el _*comando "mv"*_ para mover el directorio descomprimido spark-4.0.1-bin-hadoop3 al directorio opt/spark:

```bash
sudo mv spark-4.0.1-bin-hadoop3 /opt/spark
```
Este comando coloca Spark en la ubicación correcta para el acceso a todo el sistema. La terminal no devuelve respuesta si el proceso se completa correctamente.

## Verificar la instalación de Spark

En este punto, Spark ya está instalado. Puede verificar la instalación comprobando la versión de Spark:

```bash
/opt/spark/bin/spark-shell --version
```
El comando muestra el número de versión de Spark y otra información general.

# Configurar Spark en Ubuntu

Esta sección explica cómo configurar Spark en Ubuntu e iniciar un *controlador* (maestro) y un *servidor de trabajo*.

## Establecer variables de entorno

Antes de iniciar el servidor maestro, debe configurar *las variables de entorno*. Use el *_comando echo_* para agregar las siguientes líneas al archivo _.bashrc_:

```bash
echo "export SPARK_HOME=/opt/spark" >> ~/.bashrc
echo "export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin" >> ~/.bashrc
echo "export PYSPARK_PYTHON=/usr/bin/python3" >> ~/.bashrc
```

Como alternativa, puede editar manualmente el archivo .bashrc con un _*editor de texto*_ como _Nano o Vim_. Por ejemplo, para abrir el archivo con Nano, escriba:

```bash
nano ~/.bashrc
```

Cuando se cargue el perfil, desplácese hasta la parte inferior y agregue estas tres líneas:

```bash
export SPARK_HOME=/opt/spark
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
export PYSPARK_PYTHON=/usr/bin/python3
```

Si usa Nano, presione *CTRL+X*, seguido de *Y*, y luego *Enter* para guardar los cambios y _*salir del archivo*_.

Cargue el perfil actualizado escribiendo:

```bash
source ~/.bashrc
```
El sistema no proporciona ninguna salida.

# Iniciar el servidor maestro independiente de Spark

Ingrese el siguiente comando para iniciar el servidor del controlador Spark (maestro):

```bash
start-master.sh
```
La URL del servidor maestro Spark es el nombre de su dispositivo en el puerto 8080. Para ver la interfaz de usuario web de Spark, abra un *navegador web* e ingrese el nombre de su dispositivo o la *dirección IP del host local* en el puerto 8080:

```markdown
http://127.0.0.1:8080/
```

La página muestra la URL de Spark, información del estado del trabajador, utilización de recursos de hardware, etc.

##### *Nota*: aprenda a automatizar la implementación de clústeres Spark en servidores Ubuntu leyendo nuestro artículo Implementación automatizada de clústeres Spark en la nube Bare Metal.

# Iniciar Spark Worker Server (Iniciar un proceso de trabajo)

Utilice el siguiente formato de comando para iniciar un servidor de trabajo en una configuración de servidor único:

```bash
start-worker.sh spark://master_server:port
```

El comando inicia un servidor de trabajo junto con el servidor maestro. master_serverPuede contener una IP o un nombre de host. En este caso, el nombre de host es *phoenixnap*:

```bash
start-worker.sh spark://phoenixnap:7077
```
Reinicie la interfaz web de Spark Master para ver al nuevo trabajador en la lista.

## Especificar la asignación de recursos para los trabajadores

De forma predeterminada, a un trabajador se le asignan todos los núcleos de CPU disponibles . Puede limitar la cantidad de núcleos asignados mediante el comando *start-worker* opción *-c*.

Por ejemplo, ingrese el siguiente comando para asignar un núcleo de CPU al trabajador:

```
start-worker.sh -c 1 spark://phoenixnap:7077
```
Vuelva a cargar la interfaz web de Spark para confirmar la configuración del trabajador.

También puedes especificar la cantidad de memoria asignada. Por defecto, Spark asigna toda la RAM disponible menos 1 GB.

Utilice la -mopción para iniciar un trabajador y asignarle una cantidad específica de memoria. Por ejemplo, para iniciar un trabajador con 512 MB de memoria, introduzca:

```
start-worker.sh -m 512M spark://phoenixnap:7077
```
##### Nota: utilice Gpara gigabytes y Mpara megabytes al especificar el tamaño de la memoria.

Vuelva a cargar la interfaz web para verificar el estado del trabajador y la asignación de memoria.

# Introducción a Spark en Ubuntu

Después de iniciar el controlador (maestro) y el servidor de trabajo, puede cargar el shell Spark y comenzar a emitir comandos.
## Prueba Spark Shell

El shell predeterminado de Spark usa Scala, su lenguaje nativo. Scala está fuertemente integrado con las bibliotecas de Java, otras herramientas de big data basadas en JVM y el ecosistema JVM en general.

Para cargar el shell de Scala, ingrese:

```bash
spark-shell
```

Este comando abre una interfaz de shell interactiva con notificaciones e información de Spark. La salida incluye detalles sobre la versión de Spark, la configuración y los recursos disponibles.

Escriba *:q* y presione *Enter* para salir de Scala.

## Prueba Python en Spark

Los desarrolladores que prefieren Python pueden usar PySpark, la API de Python para Spark, en lugar de Scala. Los flujos de trabajo de ciencia de datos que combinan ingeniería de datos y aprendizaje automático se benefician de la estrecha integración con herramientas de Python como pandas , NumPy y TensorFlow .

Ingrese el siguiente comando para iniciar el shell de PySpark:

```bash
pyspark
```
La salida muestra información general sobre Spark, la versión de Python y otros detalles.

Para salir del shell de PySpark, escriba *quit()* y presione *Enter*.

## Comandos básicos para iniciar y detener el servidor maestro y los trabajadores

En la siguiente tabla se enumeran los comandos básicos para iniciar y detener el servidor maestro y los trabajadores Apache Spark (controlador) en una configuración de una sola máquina.

```markdown
| Dominio                       | Descripción                                   |
|-------------------------------|-------------------------------------------------------                           |
| start-master.sh               | Inicie la instancia del servidor del controlador (maestro) en la máquina actual. |
| stop-master.sh                | Detener la instancia del servidor del controlador (maestro) en la máquina actual. |
| start-worker.sh spark://master_server:port                         | Inicie un proceso de trabajo y conéctelo al servidor maestro (use la IP o el nombre de host del maestro). |
| stop-worker.sh                | Detener un proceso de trabajo en ejecución. |
| start-all.sh                  | Inicie las instancias del controlador (maestro) y del trabajador. |
| stop-all.sh                   | Detener todas las instancias del controlador (maestro) y del trabajador. |
```

Los comandos *start-all.sh* and *stop-all.sh* funcionan en configuraciones de un solo nodo, pero en clústeres multinodo, debe configurar el *inicio de sesión SSH sin contraseña* en cada nodo. Esto permite que el servidor maestro controle los nodos de trabajo de forma remota.

##### Nota: intente ejecutar *PySpark en Jupyter Notebook* para obtener un procesamiento de datos más potente y una experiencia interactiva.
