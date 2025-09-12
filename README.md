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

**sudo apt update**