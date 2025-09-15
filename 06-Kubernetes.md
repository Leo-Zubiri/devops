# Kubernetes

Sistema que ayuda a manejar aplicaciones que se ejecutan en muchos servidores al mismo tiempo. 

- Si tu app se divide en partes (microservicios), Kubernetes se asegura de que cada parte esté en el servidor correcto.
- Si un servidor falla, Kubernetes mueve las partes a otro servidor automáticamente.
- Te permite aumentar o reducir la cantidad de copias de tu app según la demanda, sin que tengas que hacerlo manualmente.
- También se encarga de cosas como redes entre servicios, almacenamiento y seguridad, de forma automática.

En resumen: Kubernetes hace que desplegar, escalar y mantener aplicaciones en múltiples servidores sea mucho más fácil y confiable.

> Kubernetes toma tus contenedores y los organiza en servidores (nodos) automáticamente. Decide cuántos contenedores necesitas, los mueve si un servidor falla, y los escala según la demanda. Orquesta los contenedores para que tu aplicación funcione siempre y de manera eficiente.

## POD

Un Pod es la unidad más pequeña que Kubernetes maneja.

Piensa en un Pod como una “caja” que puede contener uno o varios contenedores que necesitan estar juntos y compartir recursos como red y almacenamiento.

Todos los contenedores dentro de un Pod comparten la misma IP y almacenamiento, y Kubernetes los maneja como una unidad.

--- 

# Minikube

Minikube es básicamente una forma fácil de probar Kubernetes en tu propia computadora.

Kubernetes normalmente corre en muchos servidores en la nube o en un cluster grande. Minikube te permite levantar un “mini cluster” de Kubernetes en tu laptop o PC, como si tu computadora fuera ese cluster completo.

Con Minikube puedes practicar, probar apps y aprender Kubernetes sin necesidad de tener varios servidores o gastar en la nube. Soporta contenedores Docker, así que puedes probar cómo Kubernetes maneja tus contenedores localmente.

> Es necesario tener la virtualizacion activada

## Installation

**Kubectl**

Es recomendable instalar Kubectl primero.

kubectl es la herramienta de línea de comandos para interactuar con Kubernetes.

Te permite decirle a Kubernetes qué hacer: crear aplicaciones, ver el estado de los Pods, escalar servicios, actualizar configuraciones, etc.

Es como el control remoto de tu cluster de Kubernetes.

## Helm

Helm es como un “gestor de paquetes” para Kubernetes.

Te permite instalar, actualizar y eliminar aplicaciones completas en Kubernetes de forma sencilla.

Las aplicaciones se empaquetan en charts, que incluyen todos los Pods, servicios y configuraciones necesarias.

```ps
winget install Helm.Helm
```

