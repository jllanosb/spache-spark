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

