# CS_CreacionDeAPIweb.NET-E1
Creación de un proyecto de API web


En este módulo se usa el SDK de .NET 6.0. Asegúrese de que tiene instalado .NET 6.0 mediante la ejecución del siguiente comando en el terminal que prefiera:

```bash
dotnet --list-sdks
```

Verá un resultado similar al siguiente:

Consola

```
3.1.100 [C:\program files\dotnet\sdk]
5.0.100 [C:\program files\dotnet\sdk]
6.0.100 [C:\program files\dotnet\sdk]
```

## **Creación y exploración de un proyecto de API web**

Para configurar un proyecto de .NET para que funcione con API web se va a usar Visual Studio Code. Visual Studio Code incluye un terminal integrado que facilita la creación de un proyecto. Si no quiere usar un editor de código, puede ejecutar los comandos de este módulo en un terminal.

1. En Visual Studio Code, seleccione **Archivo**>**Abrir carpeta**.
2. Cree una carpeta con el nombre **ContosoPizza** en la ubicación que prefiera y haga clic en **Seleccionar carpeta**.
3. Abra el terminal integrado desde Visual Studio Code; para ello, seleccione **Ver**>**Terminal** en el menú principal.
4. En la ventana del terminal, copie y pegue el siguiente comando:

```bash
dotnet new webapi -f net6.0
```

Este comando crea los archivos para un proyecto de API web básico que usa controladores, junto con un archivo de proyecto de C# llamado *ContosoPizza.csproj*
, que va a devolver una lista de previsiones meteorológicas. Si se produce un error, asegúrese de que tiene instalado el [SDK de .NET 6](https://dotnet.microsoft.com/download)
.

```bash
-| Controllers
-| obj
-| Properties
-| appsettings.Development.json
-| appsettings.json
-| ContosoPizza.csproj
-| Program.cs
-| WeatherForecast.cs
```

Examine los archivos y directorios siguientes:

| Nombre | Descripción |
| --- | --- |
| Controllers/ | Contiene clases con métodos públicos expuestos como puntos de conexión HTTP. |
| Program.cs | Configura los servicios y la canalización de solicitudes HTTP de la aplicación, y contiene el punto de entrada administrado de la aplicación. |
| ContosoPizza.csproj | Contiene los metadatos de configuración del proyecto. |

## **Compilación y prueba de la API web**

1. Ejecute el comando siguiente de la CLI de .NET Core en el shell de comandos:

```bash
dotnet run
```

El comando anterior:

- Busca el archivo de proyecto en el directorio actual.
- Recupera e instala las dependencias de proyecto necesarias para este proyecto.
- Compila el código del proyecto.
- Hospeda la API web con el servidor web de Kestrel de ASP.NET Core en un punto de conexión HTTP y HTTPS.

Se seleccionará un puerto de 5000 a 5300 para HTTP, y de 7000 a 7300 para HTTPS, en el momento de crear el proyecto. Los puertos usados durante el desarrollo se pueden cambiar fácilmente editando el archivo *launchSettings.json* del proyecto. En este módulo se usa la dirección URL `localhost` segura que comienza por `https`.

Se muestra una variación de la salida siguiente para indicar que la aplicación está en ejecución:

Consola

```
Building...
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: https://localhost:7294
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://localhost:5118
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

Si ejecuta esta aplicación en su propio equipo, puede dirigir un explorador al vínculo HTTPS que se muestra en la salida (en el caso anterior, `https://localhost:7294`
) para ver la página resultante. Recuerde este puerto, ya que lo usará en todo el módulo donde se usa `{PORT}`
.

Abra un explorador web y vaya a:

```bash
https://localhost:{PORT}/weatherforecast
```

La siguiente salida representa un extracto del código JSON que se devuelve:

```json
[
    {
    "date": "2021-11-09T20:36:01.4678814+00:00",
    "temperatureC": 33,
    "temperatureF": 91,
    "summary": "Scorching"
    },
    {
    "date": "2021-11-09T20:36:01.4682337+00:00",
    "temperatureC": -8,
    "temperatureF": 18,
    "summary": "Cool"
    },
    // ...
]
```

Abra un nuevo terminal integrado desde Visual Studio Code. Para ello, seleccione **Terminal**
>**Nuevo terminal**
 en el menú principal. Luego, ejecute el siguiente comando:

```bash
dotnet tool install -g Microsoft.dotnet-httprepl
```

1. El comando anterior instala la herramienta de línea de comandos .NET HTTP REPL que se va a usar para realizar solicitudes HTTP a la API web.
2. Conéctese a la API web mediante el comando siguiente:

```bash
httprepl https://localhost:{PORT}
```

Como alternativa, ejecute el siguiente comando en cualquier momento mientras `HttpRepl`
 se ejecuta:

```bash
(Disconnected)> connect https://localhost:{PORT}
```

Explore los puntos de conexión disponibles ejecutando el siguiente comando:

```bash
ls
```

El comando anterior detecta todas las API disponibles en el punto de conexión conectado. Debería mostrar lo siguiente:

```bash
https://localhost:{PORT}/> ls
.                 []
WeatherForecast   [GET]
```

Ejecute el comando siguiente para ir al punto de conexión `WeatherForecast`
:

```bash
cd WeatherForecast
```

El comando anterior muestra una salida de las API disponibles para el punto de conexión `WeatherForecast`
:

```bash
https://localhost:{PORT}/> cd WeatherForecast
/WeatherForecast    [GET]
```

Realice una solicitud `GET`
 en `HttpRepl`
 usando el comando siguiente:

```bash
get
```

El comando anterior realiza una solicitud `GET`
 similar a ir al punto de conexión en el explorador:

```bash
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Fri, 02 Apr 2021 17:31:43 GMT
Server: Kestrel
Transfer-Encoding: chunked
[
    {
    "date": 4/3/2021 10:31:44 AM,
    "temperatureC": 13,
    "temperatureF": 55,
    "summary": "Sweltering"
    },
    {
    "date": 4/4/2021 10:31:44 AM,
    "temperatureC": -13,
    "temperatureF": 9,
    "summary": "Warm"
    },
    // ..
]
```

Cierre la sesión de `HttpRepl`
 actual con el siguiente comando:

```bash
exit
```

1. Vuelva al terminal `dotnet` en la lista desplegable de Visual Studio Code. Para apagar la API web, seleccione CTRL+C en el teclado.

Ahora que ha creado la API web, la modificará para satisfacer las necesidades de la API web de pizza.

[![contoso-E1-A.png](https://i.postimg.cc/rFgmW12g/contoso-E1-A.png)](https://postimg.cc/pphRMnTn)

[![contoso-E1-B.png](https://i.postimg.cc/VN0L32rH/contoso-E1-B.png)](https://postimg.cc/TyG6b7cr)

[![contoso-E1-C.png](https://i.postimg.cc/T343NTwD/contoso-E1-C.png)](https://postimg.cc/5X8J6d84)

[![contoso-E1-D.png](https://i.postimg.cc/2S65x1zY/contoso-E1-D.png)](https://postimg.cc/rKBcVF0Z)

[![contoso-E1-E.png](https://i.postimg.cc/C1yLYJbM/contoso-E1-E.png)](https://postimg.cc/R3RzL7P2)

[![contoso-E1-F.png](https://i.postimg.cc/4dz43t16/contoso-E1-F.png)](https://postimg.cc/34xQSkVN)

[![contoso-E1-G.png](https://i.postimg.cc/6qc9rXgT/contoso-E1-G.png)](https://postimg.cc/wRt8zS6d)
