---
title: Testing like ninjas in C#
date: 2019-05-21 17:59:22 +0200
categories: [Seminary, Dotnet]
tags: [seminary, csharp]     # TAG names should always be lowercase
pin: true
---

Este seminario, está orientado para el desarrollo de pruebas funcionales utilizando el lenguaje C# para la plataforma .NET Core de Microsoft, pero la teoría es común para cualquier otro lenguaje, como Python, Java, Javascript... Y un largo etc.

The content of this seminary it's on [my github](https://github.com/ruben69695/testing-like-ninjas)

## Objetivos

1. Aprender la importancia de las pruebas de software
2. Indagar en los diferentes tipos de pruebas
3. Conocer las librerías más comunes para pruebas unitarias en .NET
4. Aprender a controlar el poder del Mocking y la Inyección de Dependencia
5. Convertirte en un ninja haciendo pruebas unitarias usando .NET Core
6. Quiero que te sientas mejor desarrollador después de la chapa

## Pruebas de Software
- Actividades que forman parte del proceso de desarrollo de software
- Aseguran que la aplicación funcione correctamente
- Proporcionan información sobre la calidad del producto

## Tipos de pruebas
- Funcionales
   - Comprueban que el producto funcione acorde a los requisitos funcionales
   - Detectan posibles errores durante la fase de desarrollo
   - La gran mayoría son automatizadas
- No funcionales
  - Ayudan a conocer que riesgos puede correr el producto en un futuro
  - Nos permiten saber si el producto tiene un mal desempeño o un bajo rendimiento en entornos de producción

### Tipos de pruebas funcionales
- Pruebas unitarias
- Pruebas de regresión
- Pruebas de integración

## Pruebas unitarias
- Centradas en probar únicamente funcionalidades concretas del programa cómo métodos, funciones o clases.
- Nos aseguran que el código principal está funcionando como esperábamos

![UNIT TEST EXAMPLE](https://i.ibb.co/6ybZfhb/logger-library-2222.png[)

### Requisitos
- Automatizable
- Repetibles
- Completas
- Independientes
- Profesionales
- Rápidas de crear

### Ventajas
- Fomentan la refactorización
- Simplifican las pruebas de integración
- Documentan el código
- Nos permiten programar dependiendo de abstracciones y no de implementaciones
- Los errores son más fáciles de localizar

### Bad Practice
La clase StorageData depende completamente de DbStorage (Una clase de acceso a base de datos), por lo tanto StorageData no se puede probar de forma unitaria.
![Bad Practice](https://i.ibb.co/wcNXMkJ/storage-library-unit-tests-bad-2222.png)

```csharp
public class StorageData
{
    private readonly DbStorage _storage;

    public StorageData()
    {
        _storage = new DbStorage();
    }
}

public class DbStorage
{
    public bool Save(object item) { /* Access to database */ return true; }
}   
```

### Good Practice
Extrayendo los métodos de DbStorage a una interfaz y substituyendo la dependencia de la clase StorageData por la abstracción (interfaz), podremos simular el funcionamiento de su dependencia en las pruebas unitarias. Ahora StorageData es fácil de probar.

![Good Practice](https://i.ibb.co/Vpzht7m/storage-library-unit-tests-good-2222.png)
```csharp
public class StorageData
{
    private readonly IStorage _storage;
    public StorageData(IStorage storage)
    {
        _storage = storage;
    }
}

public interface IStorage
{
    bool Save(object item);
}

public class DbStorage : IStorage
{
    public bool Save(object item) { return true; }
}
```


### Frameworks
Para .NET en general tenemos disponibles una gran variedad de Frameworks que nos ayudarán a poder crear proyectos de pruebas unitarias para nuestro producto, cómo:
- MSTest
- **NUnit**
- xUnit

### Ejemplo de pruebas con NUnit
```csharp
[Test]
public void LogError_CallMethod_ShouldSetLastErrorProperty()
{
    _loggerHelper.LogError("error2");
    
    Assert.That(_loggerHelper.LastError, Is.EqualTo("error2"));
}

[Test]
public void LogError_CallMethod_ShouldRaiseErrorLoggedEvent()
{
    bool eventRaised = false;

    _loggerHelper.ErrorLogged += (sender, guid) => eventRaised = true;
    _loggerHelper.LogError("error3");
    
    Assert.That(eventRaised, Is.True);            
}

[Test]
public void LogError_PassEmptyError_ShouldThrowArgumentNullException()
{
    Assert.That(() => _loggerHelper.LogError(string.Empty), 
        Throws.ArgumentNullException);
}
```

## Inyección de dependencia
La inyección de dependencia es un patrón de diseño que nos permite poder inyectar una dependencia en aquellas clases que las necesiten. Hay distintas formas de inyección:
- Por constructor
- Por propiedad
- Por método

### Ventajas de inyectar abstracciones
- Nuestro código estará menos acoplado
- Para las pruebas unitarias nos permitirá poder simular el funcionamiento de estos objetos (dependencias), creando nuestras propias implementaciones e inyectándolas cuando estemos probando.
- Seguiríamos el principio de inversión de la dependencia (SOLI**D**)

### Ejemplo por constructor
```csharp
public class StorageData
{
    private readonly IStorage _storage;
    public StorageData(IStorage storage)
    {
        _storage = storage;
    }
}
```

### Ejemplo por método
```csharp
public class Calculator
{
    public int MakeOperation(IOperation operation)
    {
        if(operation == null)
            throw new ArgumentNullException(nameof(operation));

        return operation.Calculate();
    }
}
```

### Ejemplo por propiedad
```csharp
public class FilterList : SuperList
{
    public IFilter filter;

    public void MakeFilter()
    {
        if(filter == null)
            throw new InvalidOperationException("Invalid, I don't have any filter assigned yet, assign one first");
        
        ViewCollection = filter.MakeFilter(SourceCollection);
    }
}

struct Program
{
    static void Main(string[] args)
    {
        var customList = new FilterList(new int[] {1, 2, 3, 4, 5});
        customList.Filter = new PrimeFilter(); // Class that implements IFilter
        customList.MakeFilter();
    }
}

```


## Mocking
Nos permite poder simular el funcionamiento de las dependencias (objetos complejos) en la clase que estamos probando. 
Algunos ejemplos de dependencias en nuestras clases:
- Acceso a recursos
- Acceso a base de datos
- Acceso a red o Internet
- Lógica de negocio
- Interacción con el sistema

### Herramientas de mocking
En .NET tenemos disponibles un conjunto de frameworks para realizar mocks (Fake object implementations), son los siguientes:
- **NSubstitute**
- Moq4
- Rhino Mocks

## NUnit
- Framework open source para el desarrollo de pruebas unitarias
- Nos sirve también para pruebas de integración
- Fork del conocido framework JUnit de Java
- Diseñado para TDD (Test Driven Development)

### Plataformas soportadas
- .NET Framework 2.0+
- .NET Standard 1.4+
- .NET Core

### Recursos
- [Repositorio de GitHub](https://github.com/nunit/nunit)
- [Paquete Nuget](https://www.nuget.org/packages/NUnit)
- [Documentación](https://github.com/nunit/docs/wiki/NUnit-Documentation)

### Código
```csharp
/* -- Atributo que marca la clase como colección de tests -- */
[TestFixture] 
public class MyStackTest
{
    private MyStack<string> _stack;
    
    /* -- Atributo que marca este método para que se ejecute siempre antes de cada tests  -- */
    [SetUp] 
    public void Setup()
    {
        _stack = new MyStack<string>();
    }

    /* Un simple tests */
    [Test]
    public void Push_PushAnItem_ReturnStackWithThePushedItem()
    {
        var stack = _stack.Push("foo");

        /* Verificación de los resultados del test, lo que identifica si el tests pasa o no */
        Assert.That(stack.Count, Is.EqualTo(1)); 
    }

    [Test]
    public void Push_PushAnItem_SourceStackIsImmutableDueOperation()
    {
        _stack.Push("foo");
        Assert.That(_stack.Count, Is.Zero);
    }
}
```
### Resultado de ejecución
```
Iniciando la ejecución de pruebas, espere...

Total de pruebas: 27. Correctas: 27. Con error: 0. Omitidas: 0.
La serie de pruebas se ejecutó correctamente.
Tiempo de ejecución de las pruebas: 1,5129 Segundos
```

## Creando el proyecto en .NET Core con NUnit
Abrimos terminal, creamos un directorio para nuestro proyecto, nos situamos en él, e introducimos el siguiente comando para crear la solución
```bash
dotnet new sln 
```

Creamos un directorio con nombre "utils" al mismo nivel que la solución
```bash
.
├── utils
└── testing-example.sln
1 directory, 1 file
```

Nos situamos en el directorio "utils" y creamos un proyecto de tipo librería
```bash
dotnet new classlib
```

El comando nos creará la siguiente estructura
```bash
.
├── Class1.cs
├── utils.csproj
└── obj
    ├── utils.csproj.nuget.cache
    ├── utils.csproj.nuget.g.props
    ├── utils.csproj.nuget.g.targets
    └── project.assets.json
1 directory, 6 files
```

Renombramos la clase Class1.cs a JsonSerializer.cs y creamos la función Serialize sin implementación:
```csharp
using System;
using Newtonsoft.Json;

namespace utils
{
    public class JsonSerializer
    {
        public string Serialize<T>(T item) where T : class
        {
            throw new NotImplementedException("Not implemented yet...");
        }
    }
}
```

Como para la futura implementación usaremos una librería de terceros en el proyecto utils para transformar todo a formato json, vamos a añadir el paquete nuget para satisfacer las dependencias:
```bash
dotnet add utils/utils.csproj package Newtonsoft.Json
```

Ahora vamos a crear otra clase en utils, para representar la información básica de un usuario, llamaremos a la clase User.cs
```csharp
namespace utils
{
    public class User
    {
        public string Name { get; set; }
        public string LastName { get; set; }
        public int Age { get; set; }
        public string Email { get; set; }
    }
}
```

Volvemos al directorio dónde se encuentra la solución y agregamos el proyecto utils.csproj a la solución
```bash 
dotnet sln add utils/utils.csproj
```

### Creando el proyecto de pruebas unitarias
Primero de todo creamos el directorio utils.unittests. La estructura quedará de la siguiente forma
```bash
.
├── testing-example.sln
├── utils
│   ├── JsonSerializer.cs
│   ├── User.cs
│   ├── obj
│   │   ├── project.assets.json
│   │   ├── utils.csproj.nuget.cache
│   │   ├── utils.csproj.nuget.g.props
│   │   └── utils.csproj.nuget.g.targets
│   └── utils.csproj
└── utils.unittests
3 directories, 8 files
```

Hacemos del directorio utils.unittests nuestro directorio actual y creamos el nuevo proyecto de tests
```bash 
dotnet new nunit
```

El comando nos creara un proyecto de tests usando NUnit como librería de pruebas.
```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>

    <IsPackable>false</IsPackable>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="nunit" Version="3.11.0" />
    <PackageReference Include="NUnit3TestAdapter" Version="3.11.0" />
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.9.0" />
  </ItemGroup>

</Project>
```
El proyecto de pruebas requiere de otros paquetes para poder correr las pruebas unitarias. "dotnet new" en el paso anterior nos añade SDK de Microsoft, el framework de pruebas NUnit y el NUnit test adapter. Ahora tenemos que añadir una referencia unidireccional entre el proyecto de pruebas y el proyecto de librería utils, usando el comando "dotnet add reference":
```bash 
dotnet add reference ../utils/utils.csproj
```

La estructura de la solución queda de la siguiente forma:
```bash
.
├── testing-example.sln
├── utils
│   ├── JsonSerializer.cs
│   ├── User.cs
│   ├── obj
│   └── utils.csproj
└── utils.tunitests
    ├── JsonSerializerTests.cs
    ├── obj
    └── utils.unittests.csproj

4 directories, 14 files
```

Ahora agreguemos a la solución el proyecto de pruebas unitarias que acabamos de crear volviendo al directorio dónde se encuentra la solución
```bash 
dotnet sln add utils.unittests/utils.unittests.csproj 
```

### Creando nuestro primer test
Vayamos a realizar nuestra primera prueba unitaria, para ello vayamos a nuestro proyecto de tests y cambiemos el nombre de la clase UnitTest1 a JsonSerializerTests.
Ahora escribamos nuestra clase de pruebas para probar la clase JsonSerializer. Mas adelante explicaremos cada cosa con más detalle:
```csharp
using NUnit.Framework;
using utils;

namespace Tests
{
    [TestFixture]
    public class JsonSerializerTests
    {
        private JsonSerializer _jsonSerializer;

        [SetUp]
        public void Setup()
        {
            _jsonSerializer = new JsonSerializer();
        }

        [Test]
        public void Serialize_SerializeAnObject_ShouldSerializeItToJsonFormatString()
        {
            string expectedResult = "{\"Name\":\"Jack\",\"LastName\":\"Stilson\",\"Age\":28,\"Email\":\"jack23@test.com\"}";
            var user = new User { 
                Name = "Jack", 
                LastName = "Stilson", 
                Age = 28, 
                Email = "jack23@test.com" 
            };

            string result = _jsonSerializer.Serialize(user);

            Assert.That(result, Is.EqualTo(expectedResult));
        }
    }
}
```
Una vez guardado vayamos a ejecutar las pruebas para ello utilizaremos el comando **dotnet test** que nos compilara todos los proyectos de la solución y además arrancara las pruebas buscando aquellos proyectos que sean de pruebas y ejecutará las pruebas.
```bash
dotnet test
````

Como podemos observar, los test fallan, esto es debido a que no tenemos implementación en la clase JsonSerializer. Hagamos que los tests pasen aplicando la correcta implementación en el método Serialize:
```csharp
public class JsonSerializer
{
    public string Serialize<T>(T item) where T : class
    {
        if(item == null)
            throw new ArgumentNullException(nameof(item));

        return JsonConvert.SerializeObject(item);
    }
}
```

Volvamos a pasar las pruebas:
```bash
dotnet test
```

Ahora cómo veis las pruebas ya pasan correctamente:
```bash
Iniciando la ejecución de pruebas, espere...

Total de pruebas: 1. Correctas: 1. Con error: 0. Omitidas: 0.
La serie de pruebas se ejecutó correctamente.
Tiempo de ejecución de las pruebas: 1,2999 Segundos
```

Ahora que tenemos nuestra primera prueba, pasando correctamente y probando de forma unitaria una clase, vamos a pasar hacer otra clase pero esta vez no serializaremos un objeto a json
lo haremos a XML. Para ello vamos al proyecto utils y vamos a agregar otra clase más, esta se llamará XmlSerializer y por ahora tampoco tendrá una implementación:
```csharp
using System;
using System.IO;
using System.Xml;

namespace utils
{
    public class XmlSerializer
    {
        public string Serialize<T>(T item) where T : class
        {
            throw new NotImplementedException("Not implemented yet...");
        }
    }
}
```

Como para la futura implementación usaremos una librería de terceros en el proyecto utils para transformar todo a formato xml, vamos a añadir el paquete nuget para satisfacer las dependencias:
```bash
dotnet add utils/utils.csproj package Microsoft.XmlSerializer.Generator --version 2.1.0-preview3.19128.7
```

Además tenemos que añadir una herramienta de .NET Core CLI al proyecto utils. Para ello editamos el fichero de proyecto utils.csproj y añadimos lo siguiente después de la etiqueta ItemGroup que ya existe y que contiene las referencias a los paquetes Nuget de los que depende nuestro proyecto.
```xml
<ItemGroup>
    <DotNetCliToolReference Include="Microsoft.XmlSerializer.Generator" Version="2.1.0-preview3.19128.7" />
</ItemGroup>
```

Ahora vamos a crear una clase más de pruebas unitarias, para ello vamos al proyecto de pruebas, y agregamos una nueva clase de pruebas llamada XmlSerializerTests, tal cual así:
```csharp
using System;
using NUnit.Framework;
using utils;

namespace Tests
{
    [TestFixture]
    public class XmlSerializerTests
    {
        private XmlSerializer _xmlSerializer;

        [SetUp]
        public void Setup()
        {
            _xmlSerializer = new XmlSerializer();
        }

        [Test]
        public void Serialize_SerializeAnObject_ShouldSerializeItToXmlFormatString()
        {
            string expectedResult = "<?xml version=\"1.0\" encoding=\"utf-16\"?>" +
                "<User xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns:xsd=\"http://www.w3.org/2001/XMLSchema\">" +
                    "<Name>Jack</Name>" +
                    "<LastName>Stilson</LastName>" +
                    "<Age>28</Age>" +
                    "<Email>jack23@test.com</Email>" +
                "</User>";
                
            var user = new User { 
                Name = "Jack", 
                LastName = "Stilson", 
                Age = 28, 
                Email = "jack23@test.com" 
            };

            string result = _xmlSerializer.Serialize(user);

            Assert.AreEqual(result, expectedResult);
        }
    }
}
```

Ahora ejecutamos los tests
```bash
dotnet test
```

Como podemos ver la última prueba que hemos realizado ha fallado, debido a que la función de la clase XmlSerializer que estamos testeando no tiene implementación, vamos a implementar lo que tiene que hacer la función:
```csharp
public string Serialize<T>(T item) where T : class
{
    if(item == null)
        throw new ArgumentNullException(nameof(item));

    var serializer = new System.Xml.Serialization.XmlSerializer(typeof(T));
    var stringWriter = new StringWriter();

    var xmlWriter = new XmlTextWriter(stringWriter);
    serializer.Serialize(xmlWriter, item);

    return stringWriter.ToString();
}
```

Ahora una vez más, arranquemos los test de nuevo y como podréis ver la prueba que era antes incorrecta, ahora es correcta!
```bash
dotnet test

Iniciando la ejecución de pruebas, espere...

Total de pruebas: 2. Correctas: 2. Con error: 0. Omitidas: 0.
La serie de pruebas se ejecutó correctamente.
Tiempo de ejecución de las pruebas: 1,1206 Segundos
```

Como habéis podido observar hemos realizado las pruebas antes que las implementaciones de las clases que queríamos probar. El proceso que hemos realizado ha sido el siguiente:
1. Hemos creado las clases sin implementación alguna.
2. Hemos diseñado las pruebas.
3. Hemos ejecutado las pruebas sabiendo que iban a fallar.
4. Al ver que fallaban, hemos añadido la correcta implementación en los métodos que estábamos probando.
5. Hemos vuelto a ejecutar las pruebas hasta que han pasado.

Tal vez no os habréis dado cuenta pero hemos seguido el patrón RED - GREEN - REFACTOR. Este patrón se utiliza en TDD ([Test Driven Development](https://es.wikipedia.org/wiki/Desarrollo_guiado_por_pruebas)) una práctica que consiste en diseñar primero las pruebas y a partir de las pruebas desarrollar el código final haciendo refactorización.

### Atributos
Atributos de NUnit usados en los ejemplos y otros también se suelen usar:

| Atributo        | Tipo | Descripción
| --------------- | ---- | -------------
| TestFixture     | Clase  | Indica que la clase contiene pruebas unitarias 
| SetUp           | Método | Ejecuta ese método justo antes de la realización de cada uno de los métodos de pruebas
| Test            | Método | Nos indica que el método es un método de prueba
| OneTimeSetUp    | Método | Ejecuta el método una única vez antes de iniciar las pruebas del espacio de nombres
| OneTimeTearDown | Método | Ejecuta un método una única vez al acabar las pruebas del espacio de nombres
| SetUpFixture    | Clase  | Marca la clase que contiene los métodos OneTimeSetUp o OneTimeTearDown para todas las pruebas del espacio de nombres
| TestCase        | Método | Marca un método como prueba a la que se le pueden pasar parámetros, se pueden definir múltiples testcases por método

Obviamente hay muchos más atributos e información, para ello [visita la documentación de NUnit](https://github.com/nunit/docs/wiki/Attributes) sobre todos los atributos.

### Afirmaciones
Las afirmaciones o assertions son la base de nuestras pruebas, en NUnit hemos utilizado algunos de los métodos estáticos de la clase Assert, para afirmar si las pruebas pasan o no. Hay dos tipos de modelos de afirmación:
- Modelo de restricción (Constraint Model)
- Modelo Clásico (Classic Model)

Aquí solo explicaré el modelo de restricción ya que es el que se usa mayormente en NUnit.
#### Modelo de restricción
Se le llama constraint model ya que toma como argumentos constraint objects. Para más información sobre las diferentes restricciones que hay, visitar el [siguiente enlace](https://github.com/nunit/docs/wiki/Constraints).

Algunos ejemplos de este modelo:
##### Excepciones
```csharp
// Verificar si salta una excepción al llamar a un método o función
Assert.That(() => {
    WebClient.DownloadFile("https://myserver.com/resource/file.py");
}, Throws.Exception);

Assert.That(() => {
    WebClient.DownloadFile("https://myserver.com/resource/file.py");
}, Throws.Exception.TypeOf<WebException>());
```

##### Operaciones con numeros
```csharp
// Operaciones con numeros
int result = 4 - 4;
Assert.That(result, Is.Zero);

int userListCount = 0;
Assert.That(count, Is.Zero);

int suma = 10 + 10;
Assert.That(suma, Is.EqualTo(20));
```

##### Colecciones que implementen IEnumerable
```csharp
var list = System.Collections.Generic.List<int>(new int[] { 4, 1, 6, 8});
Assert.That(list, Is.GreaterThan(0))
Assert.That(list, Is.Empty);
Assert.That(list, Contains.Item(4))
Assert.That(list, Has.Member(4))
Assert.That(list, Has.No.Member(10));
Assert.That(list, Does.Contain(4));

string[] sarray = new string[] { "c", "b", "a" };
Assert.That(sarray, Is.Ordered.Descending);

int[] iarray = new int[] { 1, 2, 3 };
Assert.That(iarray, Is.Ordered);
```

##### Boleanos
```csharp
bool result = true;
Assert.That(result, Is.True);

bool result = false;
Assert.That(result, Is.False);
```
##### Cadenas
```csharp
Assert.That(string.Empty, Is.Empty);
Assert.That("Hello World!", Is.Not.Empty);
Assert.That("{'Name':'Paco', 'Age':56}", Does.EndWith("}"))
Assert.That("{'Name':'Paco', 'Age':56}", Does.StartWith("{"))
Assert.That("{'Name':'Paco', 'Age':56}", Does.Not.StartWith("["))
Assert.That("{'Name':'Paco', 'Age':56}", Does.StartWith("{'Name':"))
```

Obviamente la información es mucha más extensa y hay muchos más ejemplos. Para más información sobre las afirmaciones (assertions) en NUnit os invito a que visiteis la [documentación oficial](https://github.com/nunit/docs/wiki/Assertions)

## NSubstitute
Librería para poder crear mocks, esto nos permite poder simular el funcionamiento de nuestras dependencias en las pruebas unitarias.

### Características
- Librería de mocking open source
- API amigable y con pocas lambdas
- Perfecta para los que se están iniciando en el mundo de las pruebas unitarias

### Requisitos
- Únicamente se podrán hacer mocks de Interfaces o de los miembros virtuales de las clases

### Ejemplo crear mock de una interfaz

Tenemos nuestra interfaz:
```csharp
public interface IOperation
{
    int X { get; set; }
    int Y { get; set; }
    int? Result { get; }
    
    int MakeOperation(int x, int y);
}
```

Ahora creamos un mock de esta
```csharp
var operacion = Substitute.For<IOperation>();
```

Podemos decirle a este mock con NSubstitute que nos retorne el resultado que queramos, para así simular su funcionamiento, por ejemplo vamos a simular que hacemos una operación de sumar
```csharp
int result = operation.MakeOperation(50, 8).Returns(58);
Assert.That(result, Is.EqualTo(58));
```

También podríamos saber si se ha llamado al método con unos parámetros específicos, y del mismo modo si no se ha recibido. Actua como una afirmación (assertion).
```csharp
operation.Received().MakeOperation(50, 8);
operation.DidNotReceived().MakeOperation(45, 1);
```

Si lo que queremos es saber si se ha llamado al método con cualquier parámetro, podemos hacer lo siguiente:
```csharp
operation.Received().MakeOperation(Arg.Any<int>(), Arg.Any<int>());
operation.Received().MakeOperation(Arg.Any<int>(), 60);
```

Del mismo modo podemos hacer que si nos pasan cualquier parámetro, nosotros siempre retornemos el mismo valor, por ejemplo si se llama al método Save para guardar un usuario, con cualquier tipo de parámetro que sea de tipo usuario, siempre devolverá verdadero hasta que nosotros le especifiquemos lo contrario
```csharp
persistance.Save(Arg.Any<User>()).Returns(true);
```

Ahora un ejemplo parecido al anterior, pero nos devolverá verdadero únicamente cuando el usuario que se le pase por parámetro tenga una edad superior o igual a 18 años
```csharp
persistance.Save(Arg.Is<User>(u => u.Age >= 18)).Returns(true);
```

## Implementando NSubstitute en el proyecto .NET Core
Ahora que tenemos claro lo que es el mocking y hemos visto unos cuantos ejemplos de como crear mocks utilizando la librería NSubstitute. Vamos a proceder a implementarlo en el proyecto de .NET Core y a ponerlo en práctica con un ejemplo real.

### Instalando NSubstitute
Volvamos al anterior proyecto que hemos usado de NUnit e instalemos la librería NSubstitute con el gestor de paquetes Nuget:
```bash
dotnet add utils.unittests/utils.unittests.csproj package NSubstitute
```

### Creando nuestro primer Mock
Ahora que tenemos la librería instalada, vamos a crear una cuantas interfaces en archivos .cs separados en el proyecto utils.csproj:

ISerializer
```csharp
namespace utils 
{
    public interface ISerializer
    {
        string Serialize<T>(T item) where T : class;
    }
}
```

IWritable
```csharp
namespace utils
{
    public interface IWritable
    {
        bool Write(string data);
    }
}
```

IDataExportable
```csharp
namespace utils 
{
    public interface IDataExportable
    {
        ISerializer Serializer { get; }
        bool Export<TData> (TData item, IWritable destination) where TData : class;
    }
}
```

Ahora que tenemos la interfaz ISerializer, vamos hacer lo que pertoca con ella, implementarla en las clases: JsonSerializer y XmlSerializer:
```csharp
public class JsonSerializer : ISerializer { ... }

public class XmlSerializer : ISerializer { ... }
```

Ahora que tenemos nuestras interfaces nuevas y hemos implementado ISerializer en las dos clases que serializan, vamos a crear la clase DataExporter, que va a ser una clase que se ocupará de exportar datos en el formato que nosotros queramos, podrá ser JSON o XML gracias a las clases que habíamos creado con anterioridad. La vamos a crear sin implementación y además implementaremos la interfaz IDataExportable:
```csharp
using System;
using System.IO;

namespace utils
{
    public class DataExporter : IDataExportable
    {
        private readonly ISerializer _serializer;

        public DataExporter(ISerializer serializer)
        { 
            _serializer = serializer;
        }

        public ISerializer Serializer => _serializer;

        public bool Export<TData> (TData item, IWritable destination) where TData : class
        {
            throw new NotImplementedException();
        }
    }
}
```

### Volviendo a crear pruebas
Sencillo, para que más. Ahora que tenemos la estructura básica de la clase y no tenemos implementación, vamos a crear otra prueba en el proyecto utils.tests que habíamos creado, la nueva clase de pruebas se llamará DataExporterTests:

DataExporterTests
```csharp
using NUnit.Framework;
using NSubstitute;
using utils;

namespace Tests
{
    [TestFixture]
    public class DataExporterTests
    {
        private DataExporter _dataExporter;
        private ISerializer _serializer;
        private IWritable _writableDestination;

        [SetUp]
        public void Setup()
        {
            _serializer = Substitute.For<ISerializer>();
            _writableDestination = Substitute.For<IWritable>();
            _dataExporter = new DataExporter(_serializer);
        }

        [Test]
        public void Export_MethodCall_ShouldReturnTrue()
        {
            // Simulamos la serialización de los datos
            string serializerDataSimulation = "Estos son los datos serializados, me da igual el formato que sea";
            _serializer.Serialize(Arg.Any<User>()).Returns(serializerDataSimulation);

            // Simulamos la escritura de los datos en un destino
            _writableDestination.Write(serializerDataSimulation).Returns(true);

            // Actuamos sobre el método que queremos probar
            var result = _dataExporter.Export(new User(), _writableDestination);

            // Afirmamos que la prueba pase
            Assert.That(result, Is.True);
        }
    }
}
```

Ahora ejecutemos las pruebas, cómo ya sabéis... fallarán, no tenemos implementación en la función Export:
```bash
IIniciando la ejecución de pruebas, espere...
Con error   Export_MethodCall_ShouldReturnTrue
Mensaje de error:
 System.NotImplementedException : The method or operation is not implemented.
Seguimiento de la pila:
   at utils.DataExporter.Export[TData](TData item, IWritable destination) in /Users/rubenarrebola/Develop/testing-like-ninjas/Seminary/utils/DataExporter.cs:line 55
   at Tests.DataExporterTests.Export_MethodCall_ShouldReturnTrue() in /Users/rubenarrebola/Develop/testing-like-ninjas/Seminary/utils.unittests/DataExporterTests.cs:line 30

Total de pruebas: 3. Correctas: 2. Con error: 1. Omitidas: 0.
No se pudo ejecutar la serie de pruebas.
Tiempo de ejecución de las pruebas: 1,4639 Segundos
```

Pues no hay tiempo que perder, vamos a añadir a la función export su correcta implementación
```csharp
public bool Export<TData> (TData item, IWritable destination) where TData : class
{
    if(item == null)
        throw new ArgumentNullException(nameof(item));
    if(destination == null)
        throw new ArgumentNullException(nameof(destination));

    bool exportResult = false;

    string serializedData = _serializer.Serialize(item);

    exportResult = destination.Write(serializedData);

    return exportResult;
}
```

Volvamos a ejecutar las pruebas de nuevo:
```bash
Iniciando la ejecución de pruebas, espere...

Total de pruebas: 3. Correctas: 3. Con error: 0. Omitidas: 0.
La serie de pruebas se ejecutó correctamente.
Tiempo de ejecución de las pruebas: 1,3040 Segundos
```

Perfecto ya tenemos nuestra última prueba y además hemos realizado mocking de una dependencia. El objetivo del mocking es sencillo, simular el funcionamiento de dependencias el cual no estamos probando en ese momento.
Es muy bueno acostumbrarse a separar conceptos y a programar de forma desacoplada, además, las pruebas unitarias se hacen más sencillas, más fáciles de leer y también mas fáciles de mantener. Como resultado final te hace ser mejor desarrollador en todos los aspectos.

## Pruebas de integración
- Verifican el correcto funcionamiento del conjunto de elementos que componen el producto
- Se realizan después de las pruebas unitarias
- Nos permiten detectar defectos en las interfaces y en la interacción entre los diferentes componentes integrados

![PruebaIntegracion](https://i.ibb.co/sW10Gg4/logger-library-integration-test.png)

### Requisitos
- Automatizable
- Completas
- Independientes
- Profesionales
- Fáciles de mantener

## Pruebas de integración en .NET Core
Ahora que sabemos que son las pruebas de integración, va siendo hora de pasar a una parte mas práctica, vamos a implementar nuestra prueba de integración en .NET Core. Para ello utilizaremos el proyecto que ya tenemos.
Así que no perdamos mas tiempo, vamos a ello!

Primero de todo, vamos a crear una nueva clase, la cual se encargará principalmente de crear un fichero y escribir información en él. Llamaremos a la clase FileWriterHelper e implementará la interfaz IWritable. Debido a que esta clase toca el sistema de ficheros, no vamos hacer una prueba unitara de ella, la cubrirá nuestra futura prueba de integración:
```csharp
using System;
using System.IO;

namespace utils
{
    public class FileWriterHelper : IWritable
    {
        public string DestinationFile { get; private set; }

        public FileWriterHelper(string destinationFile)
        {
            DestinationFile = destinationFile;
        }

        public bool Write(string data)
        {
            bool operationResult = false;

            using (StreamWriter outputFile = new StreamWriter(DestinationFile))
            {
                outputFile.WriteLine(data);
                operationResult = true;
            }

            return operationResult;
        }
    }
}
```

Ahora creamos un nuevo directorio en la raíz del proyecto, utils.integrationtests, nos situamos en él y creamos el proyecto nuevo:
```bash
dotnet new nunit

.
├── testing-example.sln
├── utils
│   ├── DataExporter.cs
│   ├── FileWriterHelper.cs
│   ├── IDataExportable.cs
│   ├── ISerializer.cs
│   ├── IWritable.cs
│   ├── JsonSerializer.cs
│   ├── User.cs
│   ├── XmlSerializer.cs
│   ├── bin
│   ├── obj
│   └── utils.csproj
├── utils.integrationtests
│   ├── UnitTest1.cs
│   ├── obj
│   │   ├── project.assets.json
│   └── utils.integrationtests.csproj
└── utils.unittests
    ├── DataExporterTests.cs
    ├── JsonSerializerTests.cs
    ├── XmlSerializerTests.cs
    ├── bin
    ├── obj
    └── utils.unittests.csproj

16 directories, 65 files
```

Agregamos nuestro nuevo proyecto a la solución:
```bash
dotnet sln ../testing-example.sln add utils.integrationtests.csproj
```

Ahora una referenica unidireccional del proyecto nuevo al proyecto utils
```bash
dotnet add reference ../utils/utils.csproj 
```

Ahora que tenemos el proyecto nuevo, vamos a renombrar la clase de test que nos crea de forma automática, cambiamos el nombre del archivo y el de la clase por JsonDataExportationTests e implementamos la siguiente prueba:
```csharp
using NUnit.Framework;
using System.IO;
using utils;

namespace Tests
{
    public class JsonDataExportationTests
    {
        private const string FILEPATH = "json_data_export_test.json";

        private DataExporter _dataExporter;
        private ISerializer _serializer;
        private IWritable _writableDestination;

        [SetUp]
        public void Setup()
        {
            _writableDestination = new FileWriterHelper(FILEPATH);
            _serializer = new JsonSerializer();
            _dataExporter = new DataExporter(_serializer);
        }

        [Test]
        public void ExportUserDataIntoJsonFormatFile()
        {
            string expectedDataResult = @"{""Name"":""Jack"",""LastName"":""Stilson"",""Age"":28,""Email"":""jack23@test.com""}" + "\n";
            var user = new User { 
                Name = "Jack", 
                LastName = "Stilson", 
                Age = 28, 
                Email = "jack23@test.com" 
            };

            _dataExporter.Export(user, _writableDestination);

            string dataFromFile = File.ReadAllText(FILEPATH);

            Assert.That(dataFromFile, Is.EqualTo(expectedDataResult));
        }
    }
}
```

Pasamos las pruebas. Si todo esta correcto, nos exportará los datos del usuario a un fichero y luego comprobará internamente que son los mismos datos que acabamos de serializar en formato JSON:
```bash
dotnet test

Iniciando la ejecución de pruebas, espere...

Total de pruebas: 1. Correctas: 1. Con error: 0. Omitidas: 0.
La serie de pruebas se ejecutó correctamente.
Tiempo de ejecución de las pruebas: 1,0825 Segundos
```

Vamos hacer una prueba mas, ahora vamos a añadir una prueba en la que exportaremos una colección de marcadores web, para ello vamos a crear la siguiente clase en utils:
```csharp
using System;

namespace utils
{
    public class Bookmark
    {
        public Guid Id { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }
    }
}
```

Ahora vamos a crear la nueva prueba en la que le pasaremos al método de exportación una colección de objetos de tipo Bookmark y afirmaremos que se haya serializado como esperamos. Añadimos la siguiente prueba seguida de una propiedad:
```csharp
[Test]
public void ExportBookmarkCollectionIntoJsonFormatFile()
{
    string expectedDataResult = 
        @"[{""Id"":1,""Name"":""My GitHub"",""Url"":""https://github.com/ruben69695""}," +
        @"{""Id"":2,""Name"":""StackOverflow"",""Url"":""https://stackoverflow.com/""}," + 
        @"{""Id"":3,""Name"":""LinkedIn"",""Url"":""https://www.linkedin.com""}]" + "\n";

    _dataExporter.Export(TestBookmarks, _writableDestination);

    string dataFromFile = File.ReadAllText(FILEPATH);

    Assert.That(dataFromFile, Is.EqualTo(expectedDataResult));
}

private IEnumerable<Bookmark> TestBookmarks 
{
    get 
    {
        return new List<Bookmark>(new [] {
            new Bookmark { Id = 1, Name = "My GitHub", Url = "https://github.com/ruben69695" },
            new Bookmark { Id = 2, Name = "StackOverflow", Url = "https://stackoverflow.com/" },
            new Bookmark { Id = 3, Name = "LinkedIn", Url = "https://www.linkedin.com" },
        });
    }

}
```

Pasamos las pruebas de nuevo y como esperabamos, las pruebas pasan correctamente
```bash
dotnet test

Iniciando la ejecución de pruebas, espere...

Total de pruebas: 2. Correctas: 2. Con error: 0. Omitidas: 0.
La serie de pruebas se ejecutó correctamente.
Tiempo de ejecución de las pruebas: 1,8326 Segundos
```

## Ejercicios
En el repositorio encontraréis ejercicios que podéis realizar, se divide en 2 directorios, uno con las soluciones y el otro vuestro entorno, dónde váis hacer las pruebas y a refactorizar clases:

Directorio Seminary.Exercices.Solved, este directorio contiene la solución a los ejercicios, se trata de los siguientes proyectos:

- MagicDarkLibraries:
Este proyecto contiene las clases que se tienen que probar ya refactorizadas

- UnitTests:
Este proyecto contiene todas las pruebas unitarias que se piden en los ejercicios de las clases del proyecto MagicDarkLibraries.

Por otra parte tenemos Seminary.Exercices, este directorio es dónde tendréis que hacer los ejercicios, consta de lo siguiente:

- TrollLibraries:
Es un proyecto parecido a las MagicDarkLibraries, pero las clases no están refactorizadas, por lo tanto no se pueden probar de forma unitaria. Es aquí dónde os toca trabajar, intentar hacer pruebas unitarias, identificar las dependencias y refactorizar las clases para que sean fáciles de probar sustituyendo las dependencias por abstracciones.

- TrollLibraries.UnitTests
En este proyecto es dónde vais a hacer las pruebas unitarias de las distintas clases que os pido en los ejercicios.

### Ejercicio CodeKatasStack
Este ejercicio consiste en realizar las pertinentes pruebas unitarias a la clase MyStack. ¿Parece fácil verdad? Bueno esta clase es algo especial...

### Ejercicio Installer
Este ejercicio consiste en realizar las pruebas unitarias de la clase InstallerHelper, el problema de esta clase es que necesita de una refactorización, ya que es una clase que no se puede probar de forma unitaria ya que tiene una dependencia que accede a Internet, se escapa del ámbito unitario. Identificad la dependencia, abstraerla y realizar las pertinentes pruebas unitarias.

### Ejercicio Logger
Nos encontramos con uno de mis ejercicios favoritos. Esta clase de log, se encarga de hacer log de error en 3 dependencias externas: un fichero, una base de datos y un servicio web. El objetivo es identificar estas dependencias y refactorizar la clase para que sea fácil de probar pero además que sea extensible a hacer log en cualquier otro sitio sin tocar internamente el funcionamiento de la clase una vez refactorizada.

### Ejercicio ViewModel
Nos encontramos con el ejercicio más grande. Este ejercicio consiste como en los demás, identificar las dependencias que tiene nuestro view model y refactorizar la clase y sus dependencias para poder probar de forma unitaria el view model. En este ejercicio os doy la libertad de hacerlo como vosotros creáis conveniente y luego comprobaremos el resultado final.
