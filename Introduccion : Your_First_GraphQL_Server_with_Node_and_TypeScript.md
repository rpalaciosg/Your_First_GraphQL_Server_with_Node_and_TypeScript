# Introduccion: Tu primer servidor graphql con node y typescript

## Requisitos

- VSCode y estensiones com prettier, typescript suport
- Terminal o consola de tu preferencia
- Nodejs
- Navegador en este caso Google Chrome

## Modulo 1: Iniciando con nuestro servidor en Node

Vamos a crear nuestro servidor Node.js desde cerro con ayuda del framework Express y ademas introduciremos los beneficios de Typescript en la aplicacion de servidor de Node que crearemos.

Ademas usaremos [Nodemon](<[https://nodemon.io/](https://nodemon.io/)>) para recargar automaticamente el servidor cuando detecte algun cambio.

Veremos como compilar nuestro codigo Typescript a javascript valido.

Instalaremos y usaremos [ESLint](<[https://eslint.org/](https://eslint.org/)>) como nuestra herramienta linter.

Finalmente, use datos simulados para crear una ruta GET y POST dentro de nuestro servidor para imitar cómo se comportan normalmente las API RESTful.

### Que es Node?

Node es un entorno de ejecucion de javascript en el servidor y que es multiplataforma.

**Entrada y salida no bloqueante**

En este ejemplo comparamos codigo asincorno y sincrono al leer un archivo de nuestra PC usando Node File System (fs module)

_Codigo de bloqueo sincrono_

```jsx
const fs = require("fs");
const data = fs.readFileSync("/file.md");
moreWork();
```

- El meto readFilesync() es un metodo sincrono, es decir que mientras no termine de leer el archivo [file.md](http://file.md) no se podra ejecutar el metod moreWork() es decir este se bloqueara

_Codigo de bloque asincrono_

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

### Ejecutando js con node
