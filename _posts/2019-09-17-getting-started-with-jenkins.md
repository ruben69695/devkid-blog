---
title: Empezando con Jenkins
date: 2019-09-17 17:59:22 +0200
categories: [Blogging, Jenkins]
tags: [getting started, jenkins]     # TAG names should always be lowercase
pin: true
---

Introducción a la herramienta Jenkins para incrementar y asegurar la calidad de nuestros desarrollos en equipo usando integración continua.

### Integración Continua
Consiste en la automatización continua de tareas, como pueden ser: 

- Compilación
- Ejecución de pruebas automáticas
- Publicación de los resultados
- Análisis de la calidad del software

Todo esto con el objetivo de poder detectar fallos cuanto antes y solucionarlos.

### Entrega Continua
Es la tapa que viene después de la integración continua en la que tenemos la posibilidad de liberar versiones de nuestro software. Normalmente se implementa cuando hay entregas frecuentes, ya que se automatizan las entregas mediante scripts que se encargan de: 

- Creación de los artefactos
- Generación de scripts de base de datos
- Publicación de los paquetes en repositorios

### Despliegue Continuo
Esta etapa es justo después de la entrega continua, en la que tenemos la opción de implementar de forma automatizada:

- Ejecución de scripts de base de datos
- Implementación de los artefactos en entornos de pre y pro.

**Ejemplo 1**: implementación de los artefactos de una nueva versión de una aplicación web en el cloud de Azure.

**Ejemplo 2**: registro y publicación de un contenedor docker con nuestra app web en un entorno de pre.

## ¿Qué es Jenkins?
Jenkins es una herramienta programada en Java que nos permite implementar:

- Automatización de tareas
- Ciclos de Integración Continua (CI)
- Ciclos de Entrega o Despliegue Continuo (CD).

### Infraestructura
Jenkins se compone de la parte de servidor y de sus agentes esclavos, máquinas que pueden ser Windows, macOS o Linux y se encargan de realizar las compilaciones.

![Jenkins Infraestrctura](https://i.ibb.co/k6WbLGD/jenkins-infraestrctura.png)

### ¿Qué ventajas nos ofrece implementarlo?
- Configuración web sencilla
- Multitud de plugins
- Extensible
- Compilaciones distribuidas
- Open Source
- Comunidad con experiencia
- Automatización de tareas
    - Compilación
    - Tests
    - Publicaciones
    - Análisis
    - Despliegues
- Detección de errores de forma rápida
    - Código siempre compilable
    - Publicación de resultados de las pruebas
- Plus de calidad en nuestras entregas

### ¿Qué desventajas tiene?
- Se necesita tiempo para montarlo todo
- Mantenimiento
<!-- Hay que aplicarle tiempo para crear todas las automatizaciones y programar un poco si se quiere montar algo fiable y profesional -->

### Caso de uso de implementación en un proyecto real
- Debido a las necesidades del proyecto y del aumento de calidad que se le quiere asegurar al cliente con cada una de las entregas del proyecto, se pensó la idea de implementar un ciclo de integración continua.
A continuación tenemos una vista del worflow del proyecto con los pasos que sigue nuestro código desde que lo subimos al repositorio hasta que se generan los artefactos, pasando todo por el Jenkins que automatiza todos los procesos para facilitarnos la vida y asegurar la calidad de nuestro producto al mismo tiempo.

![Workflow View](https://i.ibb.co/qMNfxFP/integra-dev-simple-workflow-view.png)
