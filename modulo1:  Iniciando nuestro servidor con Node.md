# modulo1: Iniciando nuestro servidor con Node

Vamos a crear nuestro servidor Node.js desde cerro con ayuda del framework Express y ademas introduciremos los beneficios de Typescript en la aplicacion de servidor de Node que crearemos.

Ademas usaremos [Nodemon](<[https://nodemon.io/](https://nodemon.io/)>) para recargar automaticamente el servidor cuando detecte algun cambio.

Veremos como compilar nuestro codigo Typescript a javascript valido.

Instalaremos y usaremos [ESLint](<[https://eslint.org/](https://eslint.org/)>) como nuestra herramienta linter.

Finalmente, use datos simulados para crear una ruta GET y POST dentro de nuestro servidor para imitar cómo se comportan normalmente las API RESTful.

## Que es Node?

Node es un entorno de ejecucion de javascript en el servidor y que es multiplataforma.

### Entrada y salida no bloqueante

En este ejemplo comparamos codigo asincorno y sincrono al leer un archivo de nuestra PC usando Node File System (fs module)

**Codigo de bloqueo sincrono**

```jsx
const fs = require("fs");
const data = fs.readFileSync("/file.md");
moreWork();
```

- El meto readFilesync() es un metodo sincrono, es decir que mientras no termine de leer el archivo [file.md](http://file.md) no se podra ejecutar el metod moreWork() es decir este se bloqueara

**Codigo de bloque asincrono**

Node permite que E/S sean asincronas y sin bloqueos y proporciona metodos asincronos para las operaciones con el sistema de archivos. En este ejemplo se intentara leer el archivo [file.md](http://file.md) y ejecutar el metodo moreWork() de forma asincrona.

```jsx
const fs = require("fs");
let data;
fs.readFile("/file.md", (err, res) => {
  if (err) throw err;
  data = res;
});
moreWork();
```

## Ejecutando js con node

Para esto creamos una carpeta vacia `tinyhouse_v1/` ue es donde crearemos nuestra aplicación.
Luego creamos una subcarpeta `server` que tendra la parte del servidor de nuestra app.
Ademas crearemos un archivo `index.js` en nuestra subcarpetra _server_

```js
console.log("Hello world!");
const one = 1;
const two = 2;

console.log(`1 + 2 = ${one + two}`);
```

Para ejecutar el `index.js` ejecutamos la siguiente linea en nuestro terminal.

```shell
node server/index.js
```

## Creando un servidor Node con Express

Hasta ahora hemos ejecutado un archivo simple usando node, pero ahora crearemos un servidor web con Node.
Un servidor es software o incluso hardware que tiene como objetivo proporcionar funcionalidad para las solicitudes de los clientes.

Node tiene un `modulo HTTP` que nos da la capacidad de crear un servidor wed.

### package.json

Primero crearemos un archivo `tinyhouse_v1/server/package.json` dentro de server. Este archivo debe contener una propiedad name y version.

```json
{
  "name": "tinyhouse-v1-server",
  "version": "0.1.0"
}
```

En este caso para ayudarnos a crear el servidor instalaremos [Express](https://expressjs.com/) que es un framework para Node muy popular que permite crear servidores y API's.
Para instalarlo usaremos npm como se ve en la siguiente linea:

```shell
cd server
npm install express
```

Cuando esté completo, npm obtendrá Express de su repositorio y colocará el módulo express en una carpeta llamada node_modules en el directorio de nuestro servidor.
También notaremos que el `package.json` ha cambiado agregnado `express` como dependencia. Esto ayuda a que cualquier persona que descargue nuestro repositorio solo debe ejecutar `npm install` para instalar las dependencias en el package.json.

```json
{
  "name": "tinyhouse-v1-server",
  "version": "0.1.0",
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

### package-lock.json

Podemos notar que también se creo automáticamente un archivo ` package-lock.json` que almacena un árbol de dependencias que destaca las dependencias instaladas desde el package.json en un momento determinado, algo parecido a un log o registry.

Este archivo sirve para garantizar que el equipo y procesos de implementacion garanticen instalar siempre las mismas dependencias ademas de mostrar los cambios que realizan en las dependencias de la app.

El `package-lock.json` siempre debe commitearse o enviarse en las versiones del repositorio juntos con los demas archivos del codigo fuente, pero nunca tocaremos este archivo directamente.

Si usáramos yarn(otra herramienta de administración de dependencias) en lugar de npm, se generaría automáticamente un archivo `yarn.lock`.

### Express

Ahora vamos a usar `Express` para crear una instanacia de un servidor Node.
Primero movemos `index.js` a una carpeta `src/` donde escribiremos el código fuente de nuestro servidor.

En el archivo `index.js` borramos lo anterior y empezaremos a escribir lo siguiente:

Importamos o requerimos express:

```js
const express = require("express");
```

Luego creamos una instancia del sevidor ejecutando `express()` y asignadolo a una constante `app`.

```js
const app = express();
```

Creamos una constante para asignar un numero del puertoque usara el servidor:

```js
const port = 9000;
```

Al ser `app` la instancia del servidor podemos usar enrutamiento rapido y usar `app.get()` para asociar una ruta con un endpoint. Entonces cuando recibamos una solicitud HTTP GET a la ruta indice indicada por `/`, el servidor respondera `hello world!`. Esta funcion del endpoint tiene 2 objetos como parametros `req` y `res`. El objeto `req` contiene información sobre la solicitud http. Con el objeto `res` es como enviamos la respuesta deseada.

```js
app.get("/", (req, res) => res.send("hello world"));
```

Finalmente creamos el servidor node en el puerto determinado usando la funcion listen de express.

```js
app.listen(port);
```

Para mayor comodidad, colocaremos un mensaje al final de nuestro archivo usando un `console.log` para informar que nuestra aplicación se está ejecutando correctamente en el puerto apropiado. Con todos los cambios anteriores:

```js
const express = require("express");
const app = express();
const port = 9000;

app.get("/", (req, res) => res.send("hello world!"));
app.listen(port);

console.log(`[app] : http://localhost:${port}`);
```

Luego podemos ejecutar esto escribiendo lo siguiente en nuestro terminal:

```shell
node src/
```

Nos mostrara en la consola el mensaje `[app] : http://localhost:9000` y si ingresamos a esa url desde el navegador veremos el mensaje `hello world!`.

## Recarga automática usando Nodemon

[Nodemon](https://nodemon.io/) es una herramienta monitoreará cualquier cambio en nuestro código fuente y reiniciará automáticamente nuestro servidor Node.

Para instalarlo usaremos npm pero con la bandera `-D` que es una abreviacion de `--save-dev`, esto indica que se instalara como dependencia de desarrollo, es decir solo se necesita para desarrollo local:

```shell
cd server
npm install -D nodemon
```

### Iniciar script

Para usar nodemon crearemos en nuestro `package.json` un script `start`

```json
{
  "name": "tinyhouse-v1-server",
  "version": "0.1.0",
  "dependencies": {
    "express": "^4.17.1"
  },
  "scripts": {
    "start": "nodemon src/"
  },
  "devDependencies": {
    "nodemon": "^2.0.7"
  }
}
```

Ahora para ejecutar nuestro servidor usamos:

```shell
npm run start
```

y obtendremos esta salida porque nodemon ejecutara la linea `node /src` que esta en el script `start` que creamos:

```shell
> tinyhouse-v1-server@0.1.0 start
> nodemon src/

[nodemon] 2.0.7
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node src/`
[app] : http://localhost:9000
```
