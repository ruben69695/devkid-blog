---
title: Microsoft Azure for Dummies
date: 2019-12-16 17:59:22 +0200
categories: [Seminary, Azure]
tags: [getting started, azure]     # TAG names should always be lowercase
pin: true
---

Seminario de introducción a la plataforma cloud de Microsoft Azure

![Microsoft Azure](https://ktconnections.com/images/easyblog_articles/658/microsoft-azure-500x500.png)

# Para empezar
Azure es un conjunto completo y en expansión constante de servicios de informática en la nube que nos ayuda a afrontar proyectos y desafíos. Con Azure podemos crear, administrar e implementar aplicaciones en una red mundial enorme utilizando nuestras herramientas favoritas.

## Características
- Productivo
- Hibrido
- Inteligente
- De confianza

Azure, es global. Actualmente es la plataforma en la nube que más está presente en distintas partes del mundo permitiéndonos reducir costes y tiempo.

![Azure es global](https://www.thomasmaurer.ch/wp-content/uploads/2019/08/Microsoft-Azure-Regions-Map-with-Swiss-Azure-Regions-Switzerland.jpg)

## Servicios
Azure tiene una gran cantidad de servicios a usar en el que destacan las siguientes categorías:

- Aplicaciones web y móvil
- Redes
- Administración y seguridad
- Computación
- Datos y almacenaje
- IOT


# App Service
Azure App Service nos permite crear y hospedar aplicaciones web, back-ends móviles y APIs RESTful en el lenguaje de programación que queramos sin tener que administrar la infraestructura.

<img src="https://slideplayer.com/slide/12341982/73/images/2/Azure+App+Service+Family.jpg" alt="azureAppService" width="530" height="300"></img>

## Características
- Escalado automático
- Alta disponibilidad
- Compatible con Windows y Linux
- Implementaciones automatizadas desde repositorios Git.

## Plataformas compatibles
- .NET
- Python (solo en Linux)
- Node.js
- PHP
- Java
- HTML
- Contenedor Docker

## Implementaciones compatibles
- ZIP / WAR
- FTP
- Git Local
- Azure DevOps
- Jenkins (mediante plugin)
- GitHub
- Bitbucket
- Dropbox

## Implementar usando VSCode
Lo que vamos a hacer ahora, es una pequeña demo de como subir una aplicación web SPA desarrollada con Angular 8 y ASP.NET Core 2.2 en un nuevo AppService. Para ello vamos a usar Visual Studio Code y la extensión [Azure App Service](vscode:extension/ms-azuretools.vscode-azureappservice).

1. Abrimos el proyecto con vscode e iniciamos sesión en nuestra cuenta de Microsoft dentro del IDE.

<!-- ![Inicio de sesión](https://i.ibb.co/h9zLx0g/1.png)
![Redirección al navegador](https://i.ibb.co/J5P6QdM/2.png) -->

2. Ahora, en el explorado de Azure App Service, le damos a la flecha azul para indicar una subida. Y luego le damos a "Create new web app".

<!-- ![Upload](https://i.ibb.co/1Xk5dwC/4.png)
![Crear nuevo app service](https://i.ibb.co/rQwmk1b/3-5.png) -->

3. Le ponemos un nombre a nuestro nuevo app service

<!-- ![App service name](https://i.ibb.co/z7cSKYR/5.png) -->

4. Creamos un nuevo recurso si no tenemos uno previo creado

<!-- ![Group resource](https://i.ibb.co/c6trq4X/6.png) -->

5. Seleccionamos el sistema operativo para el App Service, en mi caso quiero que corra en Linux.

<!-- ![Seleccion sistema operativo](https://i.ibb.co/w0YLhPY/7.png) -->

6. Seleccionamos el ejecutable con el que se ejecutará nuestra aplicación, en mi caso .NET Core 2.2

<!-- ![Escoger runtime](https://i.ibb.co/cvJJRSf/8.png) -->

7. Si no tenemos un plan de app service creamos uno nuevo y le damos un nombre, luego seleccionamos el plan de app service, podemos usar el F1 que es gratis o el B1 que es el básico.

<!-- ![Crear plan servicio](https://i.ibb.co/nP9DFg5/9.png)
![Plan básico](https://i.ibb.co/D7tw7pm/10.png) -->

8. Nos preguntará si queremos crear un recurso Insights, omitimos esto.

<!-- ![Ignorar insights](https://i.ibb.co/w0pTDm1/11.png) -->

9. Por último seleccionamos la región donde queremos implementar nuestro app service.

<!-- ![Seleccionar región de despliegue](https://i.ibb.co/vc9DZWq/12.png) -->

10. Esperamos a que acabe la implementación, es un proceso automático, relájate y espera unos minutos.

11. Listo, verás que el app service está en el explorador de app service, podrás navegar a la web para visualizarla.

## Implementar usando Azure y GitHub

1. Depositamos nuestro código en un repositorio de GitHub

2. Ingresamos en el portal de Azure y seleccionamos el servicio AppService

3. Agregamos un nuevo App Service

4. Rellenamos los campos, si nos faltan opciones las creamos como pueden ser el grupo de recursos, o el plan de pago para elegir el tipo de servicio App Service a usar

5. Una vez hemos rellenado, le damos a revisar y crear y cuando acabe a crear para que empiece a realizar las operaciones.

6. Esto nos creará un App Service vacío sin ninguna aplicación implementada.

7. Ahora tenemos que proceder a darle un origen de donde sacará el código y realizara las implementaciones en la instancia.

8. Nos dirigimos a nuestra nueva instancia app service en el portal de azure.

9. En el menú lateral dentro del app service, tenemos que dirigirnos al apartado implementación y seleccionar la opción del menú "Centro de implementación".

10. Aquí nos saldrán distintos puntos de partida desde donde poder implementar. Elegimos la opción que nos interese a nosotros. Pero en este ejemplo trabajaremos contra github.com

11. Tendremos que ingresar nuestra cuenta de github

12. Ahora hay que seleccionar el proveedor de compilaciones. Seleccionaremos que lo haga el app service, en este ejemplo no entraremos a usar los pipelines de azure dev ops.

13. Configuramos el repositorio y la rama que se va a compilar, para que cuando haya cambios automáticamente genere la build y nos publique el artefacto para depositarlo en el app service. 

14. Finalizamos, y esperamos a que cree el webhook con el job. Una vez lo cree automáticamente empezará a ejecutar el job que nos hará una build y publicará el artefacto en el app service.

## Planes de pago
- Gatis y compartido
- Básico
- Estándar
- Premium
- Windows Premium
- Aislado

# Azure Functions
Azure Functions es un servicio de proceso sin servidor que nos permite ejecutar código a petición sin necesidad de aprovisionar ni administrar explicitamente una infraestructura. Normalmente se usa para ejecutar scripts o fragmentos de código en respuesta a diversos eventos.

## Eventos compatibles
- HTTP
- Temporizadores
- Azure Cosmos DB
- Blob
- Cola

## Lenguajes compatibles
- C#
- Javscript
- Python
- PHP
- Typescript
- Java
- F#
- Powershell
- Bash
![Lenguajes compatibles](https://azurecomcdn.azureedge.net/cvt-4120495975bc244e8ea56b04e7a2fdbde0933c79fc3ac498390557fe50547148/images/page/services/functions/value-prop-5.svg)

## Planes de pago
- Plan de consumo
- Plan premium
- Plan de Azure App Service

## Implementar usando VSCode
Necesitamos descargar la siguiente [extensión](vscode:extension/ms-azuretools.vscode-azurefunctions) en nuestro vscode.

1. Una vez descargada nos situamos en el explorador de Azure Functions y pulsamos en la flecha azul para realizar la subida.

2. Creamos de forma avanzada una nueva función en azure, introducimos un nombre

3. Seleccionamos el sistema operativo dónde se ejecutará la función

4. Seleccionamos el plan, usaremos el plan de consumo

5. Seleccionamos el lenguaje de la función

6. Creamos un nuevo grupo de recursos para la función

7. Seleccionamos una cuenta de almacenaje, si no tenemos la creamos y le ponemos un nombre

8. Nos saltamos el añadir un recurso de Insights.

9. Seleccionamos la región

10. Empezará a crear la función y esperamos a que se complete

11. Una vez completado, es hora de agregar el código que ejecutará la función. Le volvemos a dar a la flecha azul y seleccionamos el azure function creado previamente. Se cargará el código y listo. Ya podemos realizar la petición.

> En Python solo está dispnible el desarrollo de Azure Functions usando VSCode, pero para otras plataformas como Node o .NET Core se puede crear la función directamente desde el editor online de Azure.
