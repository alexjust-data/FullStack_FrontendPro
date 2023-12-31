
---
> [!NOTE]
> Paso a paso de todo el temario `Frontend PRO`. 
> Todo el proceso se desarrolla en [nodeapp](https://github.com/alexjust-data/FullStack_FrontendPro/tree/main/nodeapp) de este repositorio.
> 
> Instructor :  Javier  
> ciudad : Madrid  
> web : https://javiermiguel.com   
> contacto : jamg44@gmail.com   
> twitter : @JavierMiguelG  
> Guía https://github.com/KeepCodingWeb15/backend-nodejs-mongodb/commits/main/  
>
> Este es una continuación más avanzada de [Fundamentos-Backend-Node.js-y-MongoDB](https://github.com/alexjust-data/FullStack_Node_mongoDB)
>
---

# Arrancamos

Partimos de que ya se hizo una pequeña aplicacion (https://github.com/alexjust-data/App-Nodepop-backend) y que ya teníamos en https://github.com/alexjust-data/FullStack_Node_mongoDB la teoría y pática de la primera parte, ahora hemos iniciado el primer commit



```sh
➜  nodeapp git:(main) git tag 

1_Initial_Commit
```

**Arrancamos MongoDB**

Ya lo tienes instalado (si quieres repasar veta aquí https://github.com/alexjust-data/FullStack_Node_mongoDB)

```sh
$ cd /Applications/mongodb-macos-x86_64-7.0.1

# arranca la base de datos
$ ./bin/mongod --dbpath ./data/db
```

Con esto ya tiene mongoDb en marcha

**Arrancamos Aplicación**

Me he instalado `npm install --save-dev cross-env` que es una herramienta utilizada en proyectos Node.js para establecer variables de entorno de manera que funcione en múltiples sistemas operativos, como Unix y Windows.

```sh
cd FullStack_FrontendPro/nodeapp
npm run dev

> nodeapp@0.0.0 dev
> cross-env DEBUG=nodeapp:* nodemon ./bin/www

[nodemon] 3.0.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,cjs,json
[nodemon] starting `node ./bin/www`
  nodeapp:server Listening on port 3000 +0ms
Conectado a MongoDB en cursonode
```

Con esto ya debería estar funcionando tu aplicación


## HTTPS en mi entorno local

Para desarrollar sólo en nuestra máquina local. Cuando sea real utilizarás un certificado oficial con herraminentas de despliegue.

* manual : https://www.freecodecamp.org/news/how-to-get-https-working-on-your-local-development-environment-in-5-minutes-7af615770eec/
* shell : https://github.com/dakshshah96/local-cert-generator
* mkcert : https://github.com/FiloSottile/mkcert

**También podemos usar:**  
https://ngrok.com/

Cuando arrancas la app en el puerto 3000 por ejemplo, crea un proxy en sus servidores y todas las peticiones que haces a engrock la redirigje a tu puerto 3000 y el servidor que despliga en los servidores de ngroik ya está usando http. Es muy sencillo.

```sh
#https://dashboard.ngrok.com/ Authentication / Your Authtoken
ngrok authtoken <token>
ngrok http 3000
ngrok http -auth="username:password" 3000
http://localhost:4040/status
```

**Incluso otra más sencilla que yo uso:**  
https://github.com/localtunnel/localtunnel

npx localtunnel -p 3000 
npx localtunnel -p 3000 --subdomain jamg44 

```sh
# arranca la base de datos
./bin/mongodb --dppath ./data
# ejecutaslocaltunnel
npx localtunnel --port 3000
# esta es tu url
your url is hhtps://easy-melons....
```

**Comparados**

- ngrok: http/https/tcp tunnels  --> para qué sirven? Un tunel es para si quieres hacer un tunnel a un servicio que no utilice el protocolo http, por ejemplo bbsss mongoDb, postgrest, mysql.
- 
- Localtunnel: solo http/https, pero tiene custom domains  


## Debugging

**Consola**

```js
//mensaje con marcador para un string, salida en stdout stream
console.log("Hello %s", "World");
//mensaje con marcador para un int, salida en stdout stream
console.log("Number of items: %d", 5);
//mensaje con salida en stdout stream
console.info("Hello Info");
//mensaje con salida en stderr stream
console.error("Hello on Stdout");
//mensaje con salida en stderr stream
console.warn("Hello Warn");
```

```js
//empieza a contar tiempo
console.time("100mil_elementos");

for(var i=0;i<100000000;i++) 
{
    let a = 1;
    a = a * i; 
}

//para de contar
console.timeEnd("100mil_elementos"); // 100mil_elementos: 462ms
```


```js
//Escribe a stderr 'Trace :', seguido de nuestro mensaje
console.trace("Traza");

Trace: Traza
    at test (/Users/javi/traza.js:21:13)
    at Object.<anonymous> (/Users/javi/traza.js:25:1)
    at Module._compile (module.js:434:26)
    at Object.Module._extensions..js (module.js:452:10)
    at Module.load (module.js:355:32)
    at Function.Module._load (module.js:310:12)
    at Function.Module.runMain (module.js:475:10)
    at startup (node.js:117:18)
    at node.js:951:3
```

Si ponemos la instrucción debugger; en una linea podremos parar la ejecución ahí. Arrancamos con argumento debug:

```sh
$ node debug prueba.js

    < Debugger listening on port 5858 debug> . ok
    break in prueba.js:1
    >   1 "use strict";
        2
        3 let neo = { name: 'Thomas', age: 33, surname: 'Andreson'}; debug>
```

Algunos comandos en debugger en consola:

```sh
cont, c - Continue execution next, n - Step next
step, s - Step in
out, o - Step out
pause - Pause running code
repl - Entra en modo evaluación (Ctrl+c para salir) help - Muestra comandos
restart - Re-inicia aplicación
kill - Mata la aplicación
scripts - Muestra scripts cargados
```

## --inspect y Chrome

Browser y en nodeJs arrancar la aplicacion con este comando

```sh
# chrome://inspect
➜  nodeapp git:(main) ✗ node --inspect-brk ./bin/www

Debugger listening on ws://127.0.0.1:9229/d54a4555-92cd-408a-9f59-5f7f83741f43
For help, see: https://nodejs.org/en/docs/inspector
```

`index.js ` esto es el fichero que quieres que ejecute, su tu aplicación arranca en `./bin/wwww` pues pon esto y arrancaras el debugger y verás que te muestra una guía.

Vete al browser y pone esto `chrome://inspect` verás que te permite hacer un link en `inspect` a las `DevTools` de tu codigo y está en la primera linea para esperando ¿porqué? porque en el arranque `node --inspect-brk index.js ` le pusiste `-brk`.  

![](nodeapp/public/assets/img/1readme.png)

Escojes la carpeta de la app

![](nodeapp/public/assets/img/2readme.png)

Y a apartir de ahí te vas a los archivos que quieras y colocas los `breakpoints`. Recargas la página `localhost:3000` y se irá parando en los puntos

En **VSC**

Con VSC tienes los mismo en el `play con bicho` que te pide crear un archivo `launch.json` para `node.js` que lo crea en la raiz y le añades `"cwd": "${workspaceFolder}/nodeapp",` la carpeta de trabajo y el programa a ejecutar ` "program": "./bin/www"`

```json
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "cwd": "${workspaceFolder}/nodeapp",
            "skipFiles": [
                "<node_internals>/**"
            ],
            "program": "./bin/www"
        }
    ]
}
```

Teniendo apagado el programa, le das al `Launch Progran` para arrancar desde terminal verás que conecta con mongoDB y se para en el breakpoint que le pusiste.


## Cluster

Cuando desarrolles no hace falta, te irá bien en diferentes escenario cuando despliegues la aplicación.

Poner tu aplicación en modo cluster se refiere a la técnica de iniciar múltiples instancias de tu aplicación Node.js para que corran en paralelo. Esto se hace normalmente para aprovechar al máximo los sistemas de múltiples núcleos/CPUs, ya que Node.js es un entorno de ejecución de un solo hilo por defecto. Al usar el modo cluster, puedes crear un proceso principal (master) que lanza varios procesos hijos (workers), cada uno corriendo su propia instancia de tu aplicación.

* Uso Eficiente de Recursos : permite que tu aplicación cree un proceso por núcleo
* Mejora del Rendimiento y Confiabilidad : Con varios procesos corriendo en paralelo, las tareas se pueden distribuir entre ellos. 
* Tolerancia a Fallos : En un cluster, si un worker se cae (debido a errores no capturados, por ejemplo), puede ser reemplazado por un nuevo worker sin afectar la disponibilidad de la aplicación. 
* Esclalabilidad : El modo cluster facilita la escalabilidad de tu aplicación. A medida que la carga en tu aplicación crece, puedes aumentar el número de workers en el cluster para manejar la carga adicional, suponiendo que la infraestructura subyacente tenga los recursos necesarios disponibles.

---
> [!IMPORTANT]
> seguimos con el código
---

Me hago una copia del fichero `./bin/www` y le llamo `./bin/cluster` y le añado estas lineas

https://nodejs.org/api/cluster.html


```js
/**
 * Module dependencies.
 */

// var app = require('../app');
// var debug = require('debug')('nodeapp:server');
// var http = require('http');
const cluster = require('node:cluster');
const os = require('node:os');

if (cluster.isPrimary) {
  // arrancar los workers
  console.log('Arrancando el primario')

  const numCores = os.cpus().length

  for (let i = 0; i < numCores; i++) {
    cluster.fork();
  }

  cluster.on('listening', (worker, address) => {
    console.log(`Worker ${worker.id} arrancado con PID ${worker.process.pid} `)
  });

  cluster.on('exit', (worker, code, signal) => {
    console.log(`Worker ${worker.id} con PID ${worker.process.pid} se ha parado con codigo ${code} y signal ${signal}`)
    console.log('Arranco otro worker');
    cluster.fork();
  })

  // si soy primary no hago nada más
} else {
  // soy un worker, por tanto me pongo a atender peticiones

```

El proceso principal es el proceso original que inicia la aplicación y es responsable de iniciar los procesos trabajadores (workers).

```sh
# arranco el fichero cluster
➜  nodeapp git:(main) ✗ node ./bin/cluster

Arrancando el primario
Worker 4 arrancado con PID 12379 
Worker 3 arrancado con PID 12378 
Worker 1 arrancado con PID 12376 
Worker 2 arrancado con PID 12377 
Worker 8 arrancado con PID 12383 
Worker 6 arrancado con PID 12381 
Worker 7 arrancado con PID 12382 
Worker 5 arrancado con PID 12380 
```

si abres `monitor de actividad` verás 


Los eventos que emite los escucharemos así:

```js
  // al arrancar un worker...
cluster.on('online', function(worker) { console.log('Worker ' + worker.id +
      ' is online with pid ' + worker.process.pid);
  });


// al terminar un worker...
cluster.on('exit', function(worker, code, signal) { console.log('worker ' + worker.process.pid + ' died');
});
```

Cuando un worker muere emite el evento exit. En este momento podemos reaccionar y crear otro que le sustituya:

```js
  // al terminar un worker...
cluster.on('exit', function(worker, code, signal) { console.log('Worker ' + worker.process.pid +
    ' died with code: ' + code +
    ', and signal: ' + signal);
  console.log('Starting a new worker');
  cluster.fork();
});
```

El proceso master puede mandar mensajes a los workers.

```js
// master
// cluster.workers nos da una lista de los que tenemos
worker.send('hello from the master');
// worker
process.on('message', function(message) { console.log(message);
});
```

Los workers pueden mandar mensajes al master.

```js
// worker
process.send('hello from worker with id: ' + process.pid);
// master
worker.on('message', function(message) { console.log(message);
});
```

Con cluster aumentaremos notablemente el rendimiento de nuestro servidor, usando más recursos del sistema.

Cuando vayas hacer pruebas de carga de tu app, tendrás que pensar cuántas peticiones por segundo vas a recibir, entonces has de hacer un Bechmark una prubea de rendimiento de la aplicacion para sacar la tabla de cuantas workers necesitas para los `single Proces` que vas a cargar

Concurrente, cada vez en el tiempo atiendo una peticion

Es recomendable leer la documentación! https://nodejs.org/api/cluster.html



## Internacionalización y localización

Se hace normalmente en frontend no en el backend. Depende si tu web es una singlepageaplication () o si es una multipageaplication. Normalmente lo `contenidos`de la web no se traduce de esta forma, orque está en la base de datos y es variable. Eso lo encargas a quien crea el contenido, es dinémico y es el usuario quien tiene o se le deja la responsabilidad de traducir si quiere. CMS´s gestores de contenido que guardan las traducciones en la base de datos, esta es la internacionalizacion de contenidos. Pero eso no es lo que veremos ahora.

**Nuestra web, en varios idiomas**

El objetivo de la internacionalización y la localización es permitir a un único sitio web ofrecer sus contenidos en diferentes idiomas y formatos adaptados a su audiencia.

No sólo vale con traducir, también hay que formatear los contenidos en función de las preferencias del usuario.

* Formato de fecha España: dd/mm/yyyy 
* Formato de fecha USA: mm/dd/yyyy

**Definiciones**

* Internacionalización: preparar el software para que sea localizable (trabajo de desarrolladores).
* Localización: escribir la traducción y los formatos locales (trabajo de traductores)

**Cómo traducir en Node.js**

```sh
#Instalar i18n-node - 
npm install i18n

#Inicializar i18n con 
i18n.configure({ ... })

#Crear archivos de mensajes en carpeta locales 

#En nuestro código, usar la función i18n._ _()
```

https://github.com/mashpie/i18n-node


**Configuración**

```js
const i18n = require("i18n");

i18n.configure({
    directory: path.join(__dirname,'../locales'), defaultLocale: 'en',
    queryParameter: 'lang',
    autoReload: true,
    updateFiles: false,
    objectNotation: true,
    register: global
});
```
---
> [!IMPORTANT]
> seguimos con el código
---

**Manos a la obra con nodeapp (vamos a internacionalizarla)**

```sh
npm install i18n --save
```

puedes ver todas las configuraciones que tiene en https://github.com/mashpie/i18n-node

vamos a carpeta y cremaos fichero `lib/i18nConfigure.js` aquí cagamos la librerios

```js
// cargar librería i18n
const i18n = require('i18n');
const path = require('node:path');

// configurar mi instancia de i18n
i18n.configure({
  locales: ['en', 'es'],
  directory: path.join(__dirname, '..', 'locales'), // creo carpeta locales
  defaultLocale: 'en', // idioma por defecto
  autoReload: true, // watch for changes in JSON files to reload locale on updates 
  syncFiles: true, // sync locale information across all files
  cookie: 'nodeapp-locale'
});

// para utilizar en scripts
i18n.setLocale('en');

// exportar
module.exports = i18n
```

Fíjate que tiene que ir creando la carpeta `lib/locales`.  Ahora cargamos este modulo en `app.js` y vamos a crear un midelwork que utilice el midelwork que nos da `i18n`

`app.js`
```js
const i18n = require('./lib/i18nConfigure');

...

/**
 * Rutas del website
 */
app.use(i18n.init);
app.use('/',      require('./routes/index'));
app.use('/users', require('./routes/users'));
```

esto lo que hace es leer la cabecera antes de `/` y de `/users` y de ahí va a saber qué idioma tiene que utilizar para cada petición.

Vamos a utilizarlo ahora. Comenzamos por `views/index.ejs` ahí tenemos un 
`<p>Welcome to<%= title %></p>` que lo renderiza `route/index.js` con el 

```js
/* GET home page. */
router.get('/', function(req, res, next) {
    ...
      res.render('index'); //<--aquí renderizamos Wellcome>
});
```

Ahora este literal que yo quiero internacionalizar lo hago así `<p><%= __('Welcome to') %> <%= title %></p>` fíjate que la función con dos guiones bajos `__` está disponible porque hemos metido en `app` este init `app.use(i18n.init);` y lo que hace es buscar esto 'Wellcome to' en los ficheros de idiomas y lo creará cuando ejecutes. Si te vas a la carpeta `locales` verás que ha creado dos ficheros de idioma de ingles y español `en.json` , cuando recargas el browser le pedirá el idioma que tenga el usuario.

En cualquier vista lo podrías traducir, por ejemplo `views/cabecera` siempre con el mismo `<%=__(...)%>`


**Plurales**

Esta llibrería nos ayuda con los plurales `i18n.__n() `.

```js
<h2><%= __('i18n example for plurals') %></h2>
<p>Hay 0 <%= __n('Mouse', 0) %></p>
<p>Hay 1 <%= __n('Mouse', 1) %></p>
<p>Hay 2 <%= __n('Mouse', 2) %></p>
<p>Hay 3 <%= __n('Mouse', 3) %></p>
```

**lo usamos en los controladores**

POr ejemplo el HOLA del controlador `index.js` le añadimos el objeto de respuesta `res.__(....)`

```js
/* GET home page. */
router.get('/', function(req, res, next) {

  res.locals.texto = res.__('Hola');
//   res.locals.nombre = '<script>alert("inyección de código")</script>'

//   const ahora = new Date();
//   res.locals.esPar = (ahora.getSeconds() % 2) === 0;
//   res.locals.segundoActual = ahora.getSeconds();

//   res.locals.usuarios = [
//     { nombre: 'Smith', edad: 32 },
//     { nombre: 'Jones', edad: 27 }
//   ]

  res.render('index');
});
```


### Selector de idioma

Vamos a utilizar una **PLANTILLA** y así practicamos como se hace. Hay cientos de sitios con plantillas gratis y de pago.

* https://startbootstrap.com/themes
* me descargo esta plantilla : https://startbootstrap.com/theme/new-age

pongo el .zip dentro de la carpeta de la app. A la carpeta que se ha descomprimido le llamo plublic, si recargas la palicacion desde el browser ya puedes ver el template en la app.

Ya aparece porque en `app.js` va a mirar a ver si lo puede encontrar ahí en `plublic/index.html`

```js
// middlewares
// app.use(logger('dev'));
// app.use(express.json());
// app.use(express.urlencoded({ extended: false })); // parea el body en formato urlencoded
// app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));
```

Voy a utilizar trocitos de la plantilla y lo voy a llamar `plublic/index_from_template.html`. 
* Voy a copiar una parte de la cabecera y lo voy a pegar en `views/cabecera.ejs`
* Voy a copiar el Footer y lo copio en una vista nueva que le voy a llmar `pie.ejs` y lo pego
* En mi vista index le meto el include `<% include cabecera.ejs %>` y el `<% include pie.ejs %>` y areglo cosas para que se vea bien con el template


Me creo el archivo `routes/api/features.js`

```js
const express = require('express');
const router = express.Router();

router.get('/', (req, res, next) => {
  res.render('features'); // quiero que renderice esta vista que crearé ahora
});

module.exports = router;
```

Creo vista `views/features.ejs` y copio parte del template

```html
<% include cabecera.ejs %>
  <!-- Basic features section-->
  <section class="bg-light">
    <div class="container px-5">
        <div class="row gx-5 align-items-center justify-content-center justify-content-lg-between">
            <div class="col-12 col-lg-5">
                <h2 class="display-4 lh-1 mb-4"><%= __('Enter a new age of web design') %></h2>
                <p class="lead fw-normal text-muted mb-5 mb-lg-0">This section is perfect for featuring some information about your application, why it was built, the problem it solves, or anything else! There's plenty of space for text here, so don't worry about writing too much.</p>
            </div>
            <div class="col-sm-8 col-md-6">
                <div class="px-5 px-sm-0"><img class="img-fluid rounded-circle" src="https://source.unsplash.com/u8Jn2rzYIps/900x900" alt="..." /></div>
            </div>
        </div>
    </div>
</section>
<% include pie.ejs %>
```
Fíjate que puse `<h2 class="display-4 lh-1 mb-4"><%= __('Enter a new age of web design') %></h2>` para ver si genera los ficheros de idioma y que funcione.
Ahora lo meto en `app.js` para que utilice este controlador para cuendo haga una petición a '`/features`' cargue `./routes/users`

```js
/**
 * Rutas del website
 */
app.use(i18n.init);
app.use('/',      require('./routes/index'));
app.use('/users', require('./routes/users'));
app.use('/features', require('./routes/features'));
```

para probarlo, en la cabecera voy a poner un link `href="/features" y href="/download"` en la linea y cambio el multiidioma `<%= __('Features') %>`

```html
<ul class="navbar-nav ms-auto me-4 my-3 my-lg-0">
    <li class="nav-item"><a class="nav-link me-lg-3" href="/features"><%= __('Features') %></a></li>
    <li class="nav-item"><a class="nav-link me-lg-3" href="/download"><%= __('Download') %></a></li>
</ul>
```

**Incluyendo `en` `es` de forma dinamica en cabecera --  i18n.getLocales()**

```js
// https://github.com/mashpie/i18n-node
// Returns a list with all configured locales.
i18n.getLocales() // --> ['en', 'de', 'en-GB']
```

le añado en la `cabecera.ejs` 

```js
<a class="navbar-brand fw-bold" href="/"><%= title %></a>

// añado el bucle para que devuelva los lenguajes de que hay en i18nConfigure.js
// si algún día añades otro lo hará de forma dinámica

<% getLocales().forEach(locale => { %>
    <a href=""><%= locale %></a>&nbsp;
<% }) %>
```


Vemos que tiene una prpiedad que trabaja con cookies

```js
  // sets a custom cookie name to parse locale settings from - defaults to NULL
  cookie: 'yourcookiename',
```

que si le decimos el nombre de una cookie el busca en esa cookie que idioma tiene que utilizar. Voy a configurar ``

```js
i18n.configure({
//   locales: ['en', 'es'],
//   directory: path.join(__dirname, '..', 'locales'),
//   defaultLocale: 'en',
//   autoReload: true,
//   syncFiles: true,
  cookie: 'nodeapp-locale'
});
```

así mirará si hay una cookie para ver que idioma utilizará y si no hay utilizarñá la cabecera normal. Aunque el browser diga que lo quiere en español, el usuario a puesto una cookie que lo quiere en ingles y persitirá. Y crearemos un método nuevo `/change-locale/<%= locale %>`

```js
<% getLocales().forEach(locale => { %>
    <a href="/change-locale/<%= locale %>"><%= locale %></a>&nbsp;
<% }) %>
```

ahora si te vas a `http://localhost:3000/change-locale/en` verás error porque no hay el metodo creado. En tonces creo `routes/change-locale.js`

```js
const express = require('express');
const router = express.Router();

// GET /change-locale/[locale]
router.get('/:locale', (req, res, next) => {

  const locale = req.params.locale;

  // poner una cookie con el nuevo idioma
  res.cookie('nodeapp-locale', locale, {
    maxAge: 1000 * 60 * 60 * 24 * 30 // 30 días OPCION AÑADIDA, si el user no entra en 30 días se pierde la cokkie
  })

  // responder con una redirección a la misma página de la que venía
  res.redirect(req.get('referer'));
});

module.exports = router;
```

Podemos saber de donde venía porque si haces una inspección al browser, en `Headers, Referer: https:// localhost/3000` te lo indica.

Vamos a `app` 

```js
/**
 * Rutas del website
 */
// app.use(i18n.init);
// app.use('/',      require('./routes/index'));
// app.use('/users', require('./routes/users'));
// app.use('/features', require('./routes/features'));
app.use('/change-locale', require('./routes/change-locale'));
```



## Controladores - Test unitario llamando solo a una funcion

Por ejmplo quiero testear esta función del `features.js`

```js
router.get('/', (req, res, next) => {
  res.render('features');
});
```

Creo carpeta y fichero `controllers/FeaturesController.js`

```js
class FeaturesController {
  index(req, res, next) {     // metodo index
    res.render('features');
  }
}

module.exports = FeaturesController;
```

Lo llamas en la aplicacion `app.js`

```js
// const basicAuthMiddleware = require('./lib/basicAuthMiddleware');
// const swaggerMiddleware = require('./lib/swaggerMiddleware');
// const i18n = require('./lib/i18nConfigure');
const FeaturesController = require('./controllers/FeaturesController');


/**
 * Rutas del website
 */
const featuresController = new FeaturesController(); // crea instancia de esa clase

// app.use(i18n.init);
// app.use('/',      require('./routes/index'));
// app.use('/users', require('./routes/users'));
// app.use('/features', require('./routes/features'));
app.get('/features', featuresController.index);
// app.get('/change-locale/:locale', langController.changeLocale);
```

Me creo OTRO test, Creo carpeta y fichero `controllers/LangController.js`

```js
class LangController {
  changeLocale(req, res, next) {
    const locale = req.params.locale;

    // poner una cookie con el nuevo idioma
    res.cookie('nodeapp-locale', locale, {
      maxAge: 1000 * 60 * 60 * 24 * 30 // 30 días
    })

    // responder con una redirección a la misma página de la que venía
    res.redirect(req.get('referer'));
  }
}

module.exports = LangController;
```

`app.js`

```js
// const i18n = require('./lib/i18nConfigure');
// const FeaturesController = require('./controllers/FeaturesController');
const LangController = require('./controllers/LangController');

...

/**
 * Rutas del website
 */
const featuresController = new FeaturesController();
const langController = new LangController();

// app.use(i18n.init);
// app.use('/',      require('./routes/index'));
// app.use('/users', require('./routes/users'));
// // app.use('/features', require('./routes/features'));
app.get('/features', featuresController.index);
app.get('/change-locale/:locale', langController.changeLocale);
```

Ahora en el testing ahora podremos importar esta clase y probarla como queremos sin depender de express y sin tener que hacer una petición a express o ver dónde está en esa ruta express.



## Tipos de autenticación HTTP (Sesión, API Key, Tokens (JWT))

**Autenticación HTTP**

El protocolo HTTP es un protocolo sin estado: cada petición del navegador a un servidor es independiente de las anteriores. Sin embargo en las aplicaciones web a veces necesitamos mantener el estado para poder relacionar peticiones.

* Ejemplo: Petición 1: el usuario añade un producto al carrito de compra Petición 2: el usuario va a pagar en el proceso de checkout

**Sesión**

![](nodeapp/public/assets/img/3readme.png)

La autenticación por sesión se basa en el uso de cookies.
  
Las cookies son bloques de información enviados en las cabeceras HTTP en las respuestas de los servidores a los navegadores.
  
Los navegadores almacenan estos bloques de información y los envían automáticamente a los servidores en las siguientes peticiones.

![](nodeapp/public/assets/img/4readme.png)


**Anatomía de una cookie**

Las cookies están compuestas por los siguientes elementos:

* **domain** : dominio del servidor que ha puesto la cookie
* **path** : ruta opcional de la página que ha puesto la cookie. Si existe, sólo se enviará la cookie de vuelta a esa ruta.
* **expires** : fecha de expiración de la cookie
* **secure** : flag opcional para indicar que la cookie sólo se enviará por protocolo HTTPS
* **httpOnly** : flag opcional que indica que solo la puede leer el servidor value: separados por ; pueden tener nombre.

```sh
Set-Cookie: name=darth; role=admin;
    domain=deathstar.com; path=/home; secure;
    expires=Tue, 29 Jan 1985 01:02:03 GMT+2
```

**Hash de contraseñas**

* **PBKDF2**: https://keepcoding.io/blog/que-es-pbkdf2/
* **bcrypt**: https://codahale.com/how-to-safely-store-a-password/ scrypt: https://github.com/Tarsnap/scrypt
* **argon2**: https://medium.com/@mpreziuso/password-hashing- pbkdf2-scrypt-bcrypt-and-argon2-e25aaf41598e

**Como elegir?** (spoiler: PBKDF2 está antiguo, cualquiera de los otros tres es bueno)

* https://stytch.com/blog/argon2-vs-bcrypt-vs-scrypt/ 
* https://hashing.dev/about/ https://npmtrends.com/argon2-vs-bcrypt-vs-scrypt-js

---
> [!IMPORTANT]
> seguimos con el código
---


Creo el modelo de usuarios en `models/Usuario.js`


```js
const mongoose = require('mongoose');

// creamos esquema
const usuarioSchema = mongoose.Schema({
  email: String,
  password: String
});

// creamos el modelo
const Usuario = mongoose.model('Usuario', usuarioSchema);

// exportamos el modelo
module.exports = Usuario;
```

Voy hacer en `init-db.js` que haya algunos usuarios  ya inicialmente. Recuerda que tenías una funcion `await initAgentes();` que nos borraba algunso agente que habían y creaban algunos nuevos que los metía desde el json `init-db-data.json`

```js
async function main() {
  ...
  // inicializar la colección de agentes
  await initAgentes();
}

...

async function initAgentes() {
  // borrar todos los documentos de la colección de agentes
  const deleted = await Agente.deleteMany();
  console.log(`Eliminados ${deleted.deletedCount} agentes.`);

  // crear agentes iniciales
  const inserted = await Agente.insertMany(initData.agentes);
  console.log(`Creados ${inserted.length} agentes.`);
};
```

bueno pues ahora nos creamos la funcion de eliminar y crear usuarios

```js
// importo modelos desde "models index"
const { Agente, Usuario } = require('./models');

...

async function main() {
  ...
  await initAgentes();
  // inicializar la colección de usuarios
  await initUsuarios();
}

...

// definimos funcion
async function initUsuarios() {
  // eliminar usuarios
  const deleted = await Usuario.deleteMany();
  console.log(`Eliminados ${deleted.deletedCount} usuarios.`);

  // crear usuarios
  const inserted = await Usuario.insertMany([
    { email: 'admin@example.com', password: '1234'},
    { email: 'usuario1@example.com', password: '1234'},
  ]);
  console.log(`Creados ${inserted.length} usuarios.`)
}

```

con el `models/index.js` puedo exportar todo lo que sea "publico"

```js
module.exports = {
  Agente: require('./Agente'),
  Usuario: require('./Usuario'),
}
```

voy a provar que funcione

```sh
➜  nodeapp git:(main) ✗ npm run init-db

> nodeapp@0.0.0 init-db
> node init-db.js

Conectado a MongoDB en cursonode
Estas seguro de que quieres borrar la base de datos y cargar datos iniciales?si
Eliminados 3 agentes.
Creados 3 agentes.
Eliminados 0 usuarios.
Creados 2 usuarios.
```

**Ahora que tenemos usuarios nos hacemos una página de Login**

Me creo un nuevo controlador `controlador/LoginController.js`

```js
class LoginController {

  index(req, res, next) {
    res.render('login'); // render de una vista de una pag que se llama login
  }
}

module.exports = LoginController;
```

Vemos a la aplicación y cargamos 

```js
...
// const i18n = require('./lib/i18nConfigure');
// const FeaturesController = require('./controllers/FeaturesController');
// const LangController = require('./controllers/LangController');
const LoginController = require('./controllers/LoginController');

...


/**
 * Rutas del website
 */
// const featuresController = new FeaturesController();
// const langController = new LangController();
const loginController = new LoginController();

// app.use(i18n.init);
// app.use('/',      require('./routes/index'));
// app.use('/users', require('./routes/users'));
// // app.use('/features', require('./routes/features'));
// app.get('/features', featuresController.index);
// app.get('/change-locale/:locale', langController.changeLocale);
app.get('/login', loginController.index)
```

Creamos una vista de esta página. Dentro de `views/features.ejs` si arrancas la app y miras lo que hay, te puede interesar, entonces vamos hacer una copia y le llamaremos `login.ejs` copiando el contenido y modificando cosas, es decir vamos a meter un formulario de LOGUIN para que se loguee el usuario guardando el diseño o aprevechando el diseño del template features

```html
<% include cabecera.ejs %>
  <!-- Basic features section-->
  <section class="bg-light">
    <div class="container px-5">
        <div class="row gx-5 align-items-center justify-content-center justify-content-lg-between">
            <!-- Formulario de login -->

        </div>
    </div>
</section>
<% include pie.ejs %>
```

Si te fijas en `public/index_from_template.html` verás que cargará Boostrap. Y en la página de Boostrap https://getbootstrap.com/docs/5.3/forms/overview/ puedes encontrar modelos de formularios login para copiar y pegay el codigo directamente


```html
<% include cabecera.ejs %>
  <!-- Basic features section-->
  <section class="bg-light">
    <div class="container px-5">
        <div class="row gx-5 align-items-center justify-content-center justify-content-lg-between">


            <form>
                <div class="mb-3">
                  <label for="exampleInputEmail1" class="form-label">Email address</label>
                  <input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
                  <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
                </div>
                <div class="mb-3">
                  <label for="exampleInputPassword1" class="form-label">Password</label>
                  <input type="password" class="form-control" id="exampleInputPassword1">
                </div>
                <button type="submit" class="btn btn-primary">Submit</button>
            </form>


        </div>
    </div>
</section>
<% include pie.ejs %>
```

Queremos que el usuario pinche en el boton submit tengamos un metodo POST para enviarlo a la BBDD y los imputs han de tener el `name` si no no irán al servidor. Así hará un post a la ruta `/login`

```html
<form method="POST">
    <div class="mb-3">
      <label for="exampleInputEmail1" class="form-label">Email address</label>
      <input type="email" name="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
      <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
    </div>
    <div class="mb-3">
      <label for="exampleInputPassword1" class="form-label">Password</label>
      <input type="password" name="password" class="form-control" id="exampleInputPassword1">
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>

</form>
```

Vamos al `loginControler`

```js
const Usuario = require('../models/Usuario');

class LoginController {

  index(req, res, next) {
    res.locals.error = '';
    res.locals.email = '';
    res.render('login');
  }
  // esto solo para probar que están llegando los valores al servidor
  async post(req, res, next) {
    console.log(req.body)
    res.send(1)  
  }

}

module.exports = LoginController;
```

y ahora este método post que he añadido a la clase LoginControler, tendré que definir su ruta en `app`

```js
/**
 * Rutas del website
 */
// const featuresController = new FeaturesController();
// const langController = new LangController();
// const loginController = new LoginController();

// app.use(i18n.init);
// app.use('/',      require('./routes/index'));
// app.use('/users', require('./routes/users'));
// // app.use('/features', require('./routes/features'));
// app.get('/features', featuresController.index);
// app.get('/change-locale/:locale', langController.changeLocale);
app.get('/login', loginController.index);
app.post('/login', loginController.post);

```

Carga la página y comproeba que hay conexion. 

Aprovecha y vete a `es.json` y traduce `"Login": "Acceso",`

Vamos al `loginControler`

```js
const Usuario = require('../models/Usuario');

class LoginController {

  index(req, res, next) {
    res.locals.error = '';
    res.render('login');
  }

  async post(req, res, next) {
    try {
      const { email, password } = req.body;

      // buscar el usuario en la base de datos
      const usuario = await Usuario.findOne({ email: email });

      // si no lo encuentro o la contraseña no coincide --> error
      if (!usuario || usuario.password !== password) {
        res.locals.error = req.__('Invalid credentials');
        res.render('login');
        return;
      }
      // si existe y la contraseña coincide --> zona privada
      res.redirect('/privado');
      
    } catch (err) {
      next(err);
    }
  }
}

module.exports = LoginController;
```
Esto `res.locals.error = req.__('Invalid credentials');` error de vista quiero pintarla en la vista. Me voy a `login.ejs` y le pongo en la forma:

```html
<p><%= error %></p>
```

en el controlador, esto `res.locals.error = '';` hace que cuando se renderice por primera vez esté

```html
<form method="POST">
    <div class="mb-3">
      <label for="exampleInputEmail1" class="form-label">Email address</label>
      <input type="email" name="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
      <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
    </div>
    <div class="mb-3">
      <label for="exampleInputPassword1" class="form-label">Password</label>
      <input type="password" name="password" class="form-control" id="exampleInputPassword1">
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
    <p><%= error %></p>
</form>
```

esto hará que renderice la página pero que te diga, invalit credentials si en inválido. Esto le obliga al usuario escribir de nuevo todo. 

Vamos hacer que **si las credenciales no son correctas el email por lo menos se le mantenga** al usuario en la casilla. Para el voy a meter una variable en el email  `value="<%= email %>"` en ` <input type="email" name="email" value="<%= email %>" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">` esto se utiliza para insertar dinámicamente un valor en el campo de entrada del email cuando la página se carga, lo que permite al desarrollador prellenar el formulario con datos existentes o predeterminados.

Aprovecha y vete a `es.json` y traduce `"Invalid credentials": "Credenciale invalidas"`

`res.locals.email = '';` esto hará que cuando se renderice esté vacio la primera vez.
Y cuando pongan invalid credential, voy a volver a mendarle a la vista ese email `res.locals.email = email;`

```js
const Usuario = require('../models/Usuario');

class LoginController {

  index(req, res, next) {
    res.locals.error = '';
    res.locals.email = '';
    res.render('login');
  }

  async post(req, res, next) {
    try {
      const { email, password } = req.body;

      // buscar el usuario en la base de datos
      const usuario = await Usuario.findOne({ email: email });

      // si no lo encuentro o la contraseña no coincide --> error
      if (!usuario || usuario.password !== password) {
        res.locals.error = req.__('Invalid credentials');
        res.locals.email = email;
        res.render('login');
        return;
      }
      // si existe y la contraseña coincide --> zona privada
      res.redirect('/privado');
      
    } catch (err) {
      next(err);
    }
  }
}

module.exports = LoginController;
```


**USUARIO UNICOS**  

vamos hacer que no ubiera usuarios multiples con el mismo email: `unique: true` solo con esto moonguse ya no le dejará

```js
const mongoose = require('mongoose');

// creamos esquema
const usuarioSchema = mongoose.Schema({
  email: { type: String, unique: true},
  password: String
});

// creamos el modelo
const Usuario = mongoose.model('Usuario', usuarioSchema);

// exportamos el modelo
module.exports = Usuario;
```

**CREAMOS ZONA PRIVADA**

Si hace login lo redirigimos a una zona privada. 

CReo controlador  `controlers/PrivadoController.js`

```js
class PrivadoController {
  index(req, res, next) {
    res.render('privado'); // renderiza vista "privado"
  }
}

module.exports = PrivadoController;
```

Lo llamo en la `app.js`

```js
...
// const LoginController = require('./controllers/LoginController');
const PrivadoController = require('./controllers/PrivadoController');

...
/**
 * Rutas del website
 */
// const featuresController = new FeaturesController();
// const langController = new LangController();
// const loginController = new LoginController();
const privadoController = new PrivadoController();

// app.use(i18n.init);
// app.use('/',      require('./routes/index'));
// app.use('/users', require('./routes/users'));
// // app.use('/features', require('./routes/features'));
// app.get('/features', featuresController.index);
// app.get('/change-locale/:locale', langController.changeLocale);
// app.get('/login', loginController.index);
// app.post('/login', loginController.post);
app.get('/privado', privadoController.index);

```

Creo la vista (me copio features y lo trabajo) `views/privado`

```html
<% include cabecera.ejs %>
  <!-- Basic features section-->
  <section class="bg-light">
    <div class="container px-5">
        <div class="row gx-5 align-items-center justify-content-center justify-content-lg-between">
            <div class="col-12 col-lg-5">

                <h2 class="display-4 lh-1 mb-4"><%= __('Private zone') %></h2>

            </div>
        </div>
    </div>
</section>
<% include pie.ejs %>
```

**caja de sesion en la memoria del servidor**

Nos apoyaremos en https://github.com/expressjs/session  `npm repo expres-session`
Que se cree sesion y se guarden ahí àra cada usuario con identificador, etc Gestiona sesion y las cookies, etc dejando la sesion de cada usuario en una propiedad `req.session`

Se puede hacer sin esta librería pero es codigo ya escrito mil veces en mil sitios


`$ npm install express-session`

`app.js`

```js
...
const session = require('express-session');


...
/**
 * Rutas del website
 */
// const featuresController = new FeaturesController();
// const langController = new LangController();
// const loginController = new LoginController();
// const privadoController = new PrivadoController();

// app.use(i18n.init);
app.use(session({
  name: 'nodeapp-session', // nombre de la cookie
  secret: 'as98987asd98ashiujkasas768tasdgyy', // a mano o busca secure passport generator
  saveUninitialized: true, // Forces a session that is "uninitialized" to be saved to the store
  resave: false, // Forces the session to be saved back to the session store, even if the session was never modified during the request
  cookie: {
    maxAge: 1000 * 60 * 60 * 24 * 2 // 2d - expiración de la sesión por inactividad
  }
}));
// app.use('/',      require('./routes/index'));
// app.use('/users', require('./routes/users'));
// // app.use('/features', require('./routes/features'));
// app.get('/features', featuresController.index);
// app.get('/change-locale/:locale', langController.changeLocale);
// app.get('/login', loginController.index);
// app.post('/login', loginController.post);
// app.get('/privado', privadoController.index);
```

Cuando el usuario haga login en `LoginController.js` antes de enviarle a la página privada 

```js
      // si existe y la contraseña coincide --> zona privada
      // apuntar en la sesión del usuario, que está autenticado
      req.session.usuarioLogado = usuario._id;
```

¿como sabemos que la página privada sea privada realmente? miramos en la página que ha hecho el usuario esa petición y vamos a mirar si está logueado `PrivadoControlle.js`

Esta página queda protegida gracias al trocito de codigo `// si el cliente que hace la petición, ...`

```js
class PrivadoController {
  index(req, res, next) {

    // si el cliente que hace la petición, no tiene en su sesión la variable usuarioLogado
    // le mandamos al login porque no le conocemos
    if (!req.session.usuarioLogado) {
      res.redirect('/login');
      return;
    }

    res.render('privado');
  }
}

module.exports = PrivadoController;
```

Si quiero proteger más páginas a parte de esta página, tendrías que poner en cada una de ellas repetir el mismo codigo, incluso si la página privada tuviera otras peticiones como un PUT o un POST, pues tendrías que repetir este codigo en cada uno de ellos, para comprobar que todos los accesos a este controlador están identificados por un usuario con sesion y está logueado. ¿tendría que repetir el codigo en multiples sitios? pies vamos a ponerlo para reutilizarlo:

Lo pongo en un modulo y me creo un midelware

`lib/sessionAuthMiddleware.js`

```js
// modulo que exporta un middleware que controla si estamos logados o no

module.exports = (req, res, next) => {
  // si el cliente que hace la petición, no tiene en su sesión la variable usuarioLogado
  // le redirigimos al login porque no le conocemos
  if (!req.session.usuarioLogado) {
    res.redirect('/login');
    return;
  }
  next();
}
```

`app.js`

```js
// ...
// const basicAuthMiddleware = require('./lib/basicAuthMiddleware');
// const swaggerMiddleware = require('./lib/swaggerMiddleware');
const sessionAuthMiddleware = require('./lib/sessionAuthMiddleware');

...


/**
 * Rutas del website
 */
// const featuresController = new FeaturesController();
// const langController = new LangController();
// const loginController = new LoginController();
// const privadoController = new PrivadoController();

// app.use(i18n.init);
// app.use(session({
//   name: 'nodeapp-session', // nombre de la cookie
//   secret: 'as98987asd98ashiujkasas768tasdgyy',
//   saveUninitialized: true, // Forces a session that is "uninitialized" to be saved to the store
//   resave: false, // Forces the session to be saved back to the session store, even if the session was never modified during the request
//   cookie: {
//     maxAge: 1000 * 60 * 60 * 24 * 2 // 2d - expiración de la sesión por inactividad
//   }
// }));
// app.use('/',      require('./routes/index'));
// app.use('/users', require('./routes/users'));
// // app.use('/features', require('./routes/features'));
// app.get('/features', featuresController.index);
// app.get('/change-locale/:locale', langController.changeLocale);
// app.get('/login', loginController.index);
// app.post('/login', loginController.post);
app.get('/privado', sessionAuthMiddleware, privadoController.index);
```

cuando recibas una petición a `/privado`  activará este middelware `sessionAuthMiddleware` que comprueba si estoy logado y si no estás el mismo te mandará al login; pero si sí estás logado te dejará pasar a `privadoController.index`



**ENCRIPTAMOS LAS CONTRASEÑAS DEL USUARIOS**

https://codahale.com/how-to-safely-store-a-password/

`npm install bcrypt`

https://github.com/kelektiv/node.bcrypt.js/


En el modelo de usuario, aprovechando que en el modelo puedo crear metodos, me creo un metodo estático que haga un hash de una password.

`models/Usuario.js`

```js
// const mongoose = require('mongoose');
const bcrypt = require('bcrypt');

// // creamos esquema
// const usuarioSchema = mongoose.Schema({
//   email: { type: String, unique: true},
//   password: String
// });

// método estático que hace un hash de una password
usuarioSchema.statics.hashPassword = function(passwordEnClaro) {
  return bcrypt.hash(passwordEnClaro, 7);
}

// método de instancia que comprueba la password de un usuario
usuarioSchema.methods.comparePassword = function(passwordEnClaro) {
  return bcrypt.compare(passwordEnClaro, this.password)
}

// // creamos el modelo
// const Usuario = mongoose.model('Usuario', usuarioSchema);

// // exportamos el modelo
// module.exports = Usuario;
```


Vamos a `init-db` y cambiamos esto `{ email: 'admin@example.com', password: '1234'},`

```js
    { email: 'admin@example.com', password: await Usuario.hashPassword('1234')},
    { email: 'usuario1@example.com', password: await Usuario.hashPassword('1234')},
```

probemos `npm run init-db`

Ahora no funcionaría el login en el controlador del Login 


Creo un metodo en `models/Usuario.js` y lo uso en `loginController`

```js
// const mongoose = require('mongoose');
const bcrypt = require('bcrypt');

// // creamos esquema
// const usuarioSchema = mongoose.Schema({
//   email: { type: String, unique: true},
//   password: String
// });

// // método estático que hace un hash de una password
// usuarioSchema.statics.hashPassword = function(passwordEnClaro) {
//   return bcrypt.hash(passwordEnClaro, 7);
// }

// método de instancia que comprueba la password de un usuario
usuarioSchema.methods.comparePassword = function(passwordEnClaro) {
  return bcrypt.compare(passwordEnClaro, this.password)
}

// // creamos el modelo
// const Usuario = mongoose.model('Usuario', usuarioSchema);

// // exportamos el modelo
// module.exports = Usuario;
```

En en `loginController` en vez de comparar con 
` if (!usuario || usuario.password !== password) {` comparará con   
` if (!usuario || !(await usuario.comparePassword(password)) ) {`  
Que no coincida, 

```js
  // si no lo encuentro o la contraseña no coincide --> error
  if (!usuario || !(await usuario.comparePassword(password)) ) {
    res.locals.error = req.__('Invalid credentials');
    res.locals.email = email;
    res.render('login');
    return;
  }
```

cambio la cabecera y cambio el button por el `<a href="/login"`

```html
<a href="/login" class="btn btn-primary rounded-pill px-3 mb-2 mb-lg-0">
    <span class="d-flex align-items-center">
        <span class="small"><%= __('Login') %></span>
    </span>
</a>
```


**EN ZONA PRIVADA: quiero ver el email del user**

```html
<% include cabecera.ejs %>
  <!-- Basic features section-->
  <section class="bg-light">
    <div class="container px-5">
        <div class="row gx-5 align-items-center justify-content-center justify-content-lg-between">
            <div class="col-12 col-lg-5">

                <h2 class="display-4 lh-1 mb-4"><%= __('Private zone') %></h2>
                <p>User: <%= email %></p>

            </div>
        </div>
    </div>
</section>
<% include pie.ejs %>
```

Ahora en `PrivadoControle.js` le pasamos el emial del usuario 

```js
const Usuario = require('../models/Usuario');
const createError = require('http-errors');

// class PrivadoController {
//   async index(req, res, next) {

    try {
      // obtener el id del usuario de la sesión
      const usuarioId = req.session.usuarioLogado; // está en memoria

      // buscar el usuario en la BD
      const usuario = await Usuario.findById(usuarioId);

      if (!usuario) { // esto no debería ocurrir por lo tanto erro de servidor
        next(createError(500, 'usuario no encontrado'))
        return;
      }

      res.render('privado', { email: usuario.email });

    } catch (err) {
      next(err);
    }
  }
// }

// module.exports = PrivadoController;
```

**Hash de contraseñas**

Contacta con un profesional y que te haga el pestTesting

* PBKDF2: https://keepcoding.io/blog/que-es-pbkdf2/
* bcrypt: https://codahale.com/how-to-safely-store-a-password/ 
* scrypt: https://github.com/Tarsnap/scrypt
* argon2: https://medium.com/@mpreziuso/password-hashing-pbkdf2-scrypt-bcrypt-and-argon2-e25aaf41598e

Como elegir? (spoiler: PBKDF2 está antiguo, cualquiera de los otros tres es bueno)

* https://stytch.com/blog/argon2-vs-bcrypt-vs-scrypt/ 
* https://hashing.dev/about/ 
* https://npmtrends.com/argon2-vs-bcrypt-vs-scrypt-js


---
> [!IMPORTANT]
> commit "ponemos el email del usuario en la página privada"
---

**Continuamos dentro de zona privada**

Voy hacer que el botón login desapareca cuando el usuario esté logueado.

`views/cabecera.ejs`

```html
        <a href="/login" class="btn btn-primary rounded-pill px-3 mb-2 mb-lg-0">
            <span class="d-flex align-items-center">
                <span class="small"><%= __('Login') %></span>
            </span>
        </a>
```

para eso me gustaria acceder a la sesión para saber si la sesión tiene ese atributo que le pusimos en 

`loginControler.js`

```js
// cuando un usuario hace login lo guardamos en el atributo
req.session.usuarioLogado = usuario._id;
```

pues desde la vista quiero ver si el usuario está logado. Podría decirle que desde el controlador renderice esa página privada `PrivadoControler,js` podría decírselo aquí y sería tan sencillo como modificar esto:

```js
res.render('privado', { email: usuario.email, usuarioLogado: usuarioId });
```

y hacer esto en cada una de las vistas. Pero vamos hacer algo más práctico, y es permitir que todas las vistas puedan acceder directamente a la sesion para leer datos de la sesion porque si escalas la app, sin meter nada en el controlador leer algo de esa sesion.

Para que la vista pueda leer la sesion, en `app.js`

```js
// hacemos que el objeto session esté disponible al renderizar las vistas
app.use((req, res, next) => {
  res.locals.session = req.session;
  // todo middelware hace dos cosas o responder o next para que siga evaluando
  next();
});
```

Nos vamos a al cabecera y cambio esta linea `<li class="nav-item"><a class="nav-link me-lg-3" href="/download"><%= __('Download') %></a></li>` por esta otra

```html
<li class="nav-item"><a class="nav-link me-lg-3" href="/privado"><%= __('Private area') %></a></li>
```

Ahora tendremos en la cabecera un "Private area" que nos lleva al area privada `href="/privado`. Ahora le meto la condición de si está logado.

```html
<ul class="navbar-nav ms-auto me-4 my-3 my-lg-0">
    <li class="nav-item"><a class="nav-link me-lg-3" href="/features"><%= __('Features') %></a></li>
    <% if (session.usuarioLogado) { %>
        <li class="nav-item"><a class="nav-link me-lg-3" href="/privado"><%= __('Private area') %></a></li>
    <% } %>
</ul>
```

Y además lo pondré en el botón del login que aparezca el botón del `login` y aprobecho para else le meto un botoun de `logout`.

```html
  <% if (!session.usuarioLogado) { %>
      <a href="/login" class="btn btn-primary rounded-pill px-3 mb-2 mb-lg-0">
          <span class="d-flex align-items-center">
              <span class="small"><%= __('Login') %></span>
          </span>
      </a>
  <% } else  { %>
      <a href="/logout" class="btn btn-primary rounded-pill px-3 mb-2 mb-lg-0">
          <span class="d-flex align-items-center">
              <span class="small"><%= __('Logout') %></span>
          </span>
      </a>
  <% } %>
```

Vamos hacer la funcionalidad del `logout` en `LoginController` y le metemos el metodo logout.  
**caja de sesion en la memoria del servidor** Nos apoyaremos en https://github.com/expressjs/session  `npm repo expres-session`


```js
LoginController {
  
  ...

  logout(req, res, next) {
    req.session.regenerate(err => {
      if (err) {
        next(err);
        return;
      }
      res.redirect('/');
    })
  }
}
```

Añadimos en `app.js`

```js
app.get('/logout', loginController.logout);
```

En cuanto abrimos NoSqlBoosters para ver lo que pcurre en MongoDB podemos ver que tenemos una carpeta de sesion nueva con una sesion abierta y guardada ahí

![](nodeapp/public/assets/img/5readme.png)

---
> [!WARNING]
> De cara al proyecto final, todos pinchan en esto y puede que no te contraten por este error:
> 
>Todos sabemos que cuendo un usuario enre al correo electronico, lo que vemos ahí son nuestros emails, no los de otra persona, entonces el servidor de gmail cuando recibe una petición al backend y busca en la base de datos hace un filtro por los emials d e este usario. Ta,bién cuando el frontedn pones eliminar este email, hace una peticion al backend y le dice que elimine el email y el backend lo priermo que hace es verificar si ese email es mío; para que yo no pueda eliminar el email de tra persona.
>.
---


**Más formas de autentificacion***  
**VAMOS A PERMITIR CREAR AGENTES**  

Para que no esté dando la lata cada vez que reiniciemos perdamos la seseión vamos a guardar la sesión en base de datos y acontinuacion veremos como aparece una lista de agentes. Hasta ahora como guardabamos en la memoria la autentificación, cada vez que cierras la app se borra la mememoria y todos los usuario pierden su sesión.

Se puede guardar las sesiones en el disco, en una BBDD (ahora tenemos mongDb), en una BBDD en memoria (redis). Ahora con mongodb nos está bien, si algún día tu aplicación va a tener una concurrencia de decenas de miles de usuarios entonces puede que redis te irá más rápido.

**Metemos en MogoDb las s**

https://github.com/expressjs/session  propiedad `stores`, en esta propiedad se guardarán la información de las sesiones. Ahora este modulo lo guarda en un store `MomeryStore` lo que ahora estamos haciendo, pero vamos a guardarlo en un store diferente. 

Me apoyo en una librerio que se llama `connectMOngo` https://github.com/jdesboeufs/connect-mongo : 


le tengo que decir en qué base de datos quiero que la guarde, la cadena de conexion a la bbdd.

```sh
npm install connect-mongo
```

Cambio en  `app.js`


`store: MongoStore.create({ mongoUrl: 'mongodb://127.0.0.1/cursonode'})`


```js
const MongoStore = require('connect-mongo');

...



app.use(session({
  name: 'nodeapp-session', // nombre de la cookie
  secret: 'as98987asd98ashiujkasas768tasdgyy',
  saveUninitialized: true, // Forces a session that is "uninitialized" to be saved to the store
  resave: false, // Forces the session to be saved back to the session store, even if the session was never modified during the request
  cookie: {
    maxAge: 1000 * 60 * 60 * 24 * 2 // 2d - expiración de la sesión por inactividad
  },
  store: MongoStore.create({ mongoUrl: 'mongodb://127.0.0.1/cursonode'})
}));
```

Ahora entras en la aplicacion coomo usuario, luego apagas la app desde la terminar y la reinicas, podrás ver que ya no te echa como usuario, ya tiene tus credenciales guardadas en la base de datos.


Esta `'mongodb://127.0.0.1/cursonode'` es la cadena de conexion a la base de datos, nosotros también la tenemos en `connectMongoose.js` y ahora estará en dos sitios y será desagradable tener que buscar la en en lfuturo, entonces vamos a cambiar eso tambien. No quiero que en el futuro quien tanga que cambiar esto necesite entender el codigo, entonces voy a ponerselo facil y lo haremos en un fichero init donde venga esta configuracion. Existen librería que se llama `dotenv`

Voy hacer que mi aplicación vea la configuración de variables de entorno del sistema operativo, además con esta libreria haré que carge estas variables de entorno por si no estuvieran ahí.

`npm install dotenv`

https://github.com/motdotla/dotenv

`app.js` :  `store: MongoStore.create({ mongoUrl: process.env.MONGODB_URI})`

`lib/connectMongoose.js` : `mongoose.connect(process.env.MONGODB_URI)`

Sólo con esto si arrancas la aplicacion da error, si quieres que funcione has de pasarle las variables de entorno. Me creo fichero `.env` para que las cargue desde ahí. 
Después, en el inicio más temprano de la aplicación, como dice la doc, le añado la linea requerida :

`bin/www/`: `require('dotenv').config();`  
`bin/cluster`: `require('dotenv').config();` 

y para cuando ejecutamos la semilla init-db  
`init-db`: `require('dotenv').config();`  

lo hemos cargado en cada punto de entrada a la aplicacion.

Ahora ya carga.

---
> ![WARNING]
> el fichero `.env` no se mete en el repositirio git. Si ahora haces un commit la has cagado. El fichero .env es tuyo, del desarrollador, de nadie más, de tus cuentas de desarrollador, ni de tus compañeros de trabajo ni de nadie más.
---

Para que un compañero sepa que ha de poner, vamos a facilitar la vida a mis compñaeros, vamos hacer una copia del fichero y lo llamaremos `.env.example` y este si que va al repo aninimizado. ¿cómo sabe que ha de utilizar esto? cómo podemos hacer para que lo sepa- LO APUNTAMOS EN EL `README.md`


Copy .env.example to to your custom .env.
```sh
cp .env.example .env
```
And setup your configuration.



## Listado de agentes 

Ahora lo que quiero es que cuando pinte los agente, pinte sólo los agente de admin o de usuario1, del que esté logado.

modelo de agente `models/Agente.js` Quiero que cada agente tenga un propietario. Pero voy a aprvechar para haceruna relación de MOngoDb `owner: { ref: 'Usuario', type: mongoose.Schema.Types.ObjectId },`

```js
// definir el esquema de los agentes
const agenteSchema = mongoose.Schema({
  name: { type: String, index: true },
  age: { type: Number, index: true, min: 18, max: 120 },
  owner: { ref: 'Usuario', type: mongoose.Schema.Types.ObjectId },
}, {
  // collection: 'agentes' // para forzar un nombre concreto de colección
});
```

ahora cada agente tiene un atributo diciendo quien es el propietario de este agente.  
Voy a `init-db` donde creo los agentes les pongo una propiedad `await initAgentes();`

```js
async function main() {
  // espero a que se conecte a la base de datos
  await new Promise(resolve => connection.once('open', resolve))
  const borrar = await pregunta(
    'Estas seguro de que quieres borrar la base de datos y cargar datos iniciales?'
  )
  if (!borrar) {
    process.exit();
  }
  await initUsuarios(); //1 se inicializan los usuarios
  await initAgentes(); // los agnetes
  connection.close();
}
```
y en la inicializacion de los agentes, hasta ahora traíamos los agentes del archivo `init-db-data.json` pero ahora cojo los agente y los inicializo directamente en `init-db`

```js
  // crear agentes iniciales
  const inserted = await Agente.insertMany([
    { "name": "Smith", "age": 33, owner: adminUser._id }, // Smith es propiedad de adminUser._id
    { "name": "Jones", "age": 23, owner: adminUser._id },
    { "name": "Brown", "age": 46, owner: usuario1User._id }
  ]);
```

A cada agente que inserto lo pongo un owner y quiero poner un email en cada uno. Voy hacer una funcion para que lo llame:

```js
  const [ adminUser, usuario1User ] = await Promise.all([
    Usuario.findOne({ email: 'admin@example.com'}),
    Usuario.findOne({ email: 'usuario1@example.com' })
  ])
```

Puedes comprobar que se han creado `npm run init-db` verás en NoSQLBooster como se han creado. Esto inicializará la base de datos eliminando y creando


Quiero que en la zona privada aparezcan los agentes del usuario logueado. Voy a `PrivadoControññer.js`

```js
const { Agente, Usuario } = require('../models');

// class PrivadoController {
//   async index(req, res, next) {

//     try {
//       // obtener el id del usuario de la sesión
//       const usuarioId = req.session.usuarioLogado;

//       // buscar el usuario en la BD
//       const usuario = await Usuario.findById(usuarioId);

//       if (!usuario) {
//         next(createError(500, 'usuario no encontrado'))
//         return;
//       }

      // cuando el usuario existe
      // cargar lista de agentes que pertenecen al usuario
      const agentes = await Agente.find({ owner: usuarioId }); 
      // buscaremos aquellos documentos cuya propiedad owner sea el mismo usuario id 

      res.render('privado', {
        email: usuario.email,
        agentes // le doy una lista de agentes que enviaré a la vista
      });

//     } catch (err) {
//       next(err);
//     }
//   }
// }

// module.exports = PrivadoController;
```

Me voy a la vista `Privado` y me creo una vista chula para ver los agentes.


**Creamos un nuevo agente**

Añado linea en `Privado.ejs` : `<th scope="col"><a href="/agentes-new">New agent</a></th>`  
Me creo un `controllers/AgenteController.js` y le meto la petición de crear y borrar un agente.

En la `app`

```js
const AgentesController = require('./controllers/AgentesController');

/**
 * Rutas del website
 */
const agentesController = new AgentesController();

// defino ruta
app.get('/agentes-new', sessionAuthMiddleware, agentesController.new);
```

En `AgentesController` 

```js
const Agente = require('../models/Agente');

class AgentesController {
  new(req, res, next) {
    res.render('agentes-new');
  }

  async postNewAgent(req, res, next) {
    try {
      const usuarioId = req.session.usuarioLogado;
      const { name, age } = req.body;
      
      // le paso un objeto
      const agente = new Agente({ 
        name,
        age,
        owner: usuarioId // el que crea el agente
      });
      await agente.save(); // lo guardamos

      res.redirect('/privado'); // lo mandamos a la lista de agentes

    } catch (error) {
      next(error);
    }
  }
}

module.exports = AgentesController;
```

Creamos la vista del agente new. `view/agentes-new.ejs` y me creo una vista bonita para crear un nuevo agente, me creo un formulario de boostrap.

`app.js` añadimos

```js
app.post('/agentes-new', sessionAuthMiddleware, agentesController.postNewAgent);
```

**Eliminar agentes del usuarios**


En `view/Private.ejs` todo lo voy cogiendo de Boostrad plantillas e iconos `<i class="bi bi-trash"></i>` esto es una papelera. Esto `confirmDeleteAgent` es un afuncion para que confirme antes de borrar.

```html
<td><a
    onclick="confirmDeleteAgent('<%= agente.name %>', '<%= agente._id %>')"
    href="javascript:void(0);">
        <i class="bi bi-trash"></i>
</a></td>
```

le paso una función dentro del la vista para que pregunte si seguro quiere elimnar 

```html
<script>
    function confirmDeleteAgent(name, agenteId) {
        if (confirm(`Estas seguro que quieres eliminar el agente ${name}?`)) {
            window.location.href = `/agentes-delete/${agenteId}`;
        }
    }
</script>
```

Voy `AgentesController.js` y le añado la función para eliminar el agente

```js
async deleteAgent(req, res, next) {
    try {
      const usuarioId = req.session.usuarioLogado;
      const agenteId = req.params.agenteId;

      // validar que el agente que queremos borrar es propiedad del usuario!!!!
      const agente = await Agente.findOne({ _id: agenteId });

      // verificar que existe
      if (!agente) {
        console.warn(`WARNING - el usuario ${usuarioId} intentó eliminar un agente inexistente (${agenteId})`);
        next(createError(404, 'Not found'));
        return;
      }

      // agente.owner viene de la base de datos y es un ObjectId
      if (agente.owner.toString() !== usuarioId) {
        console.warn(`WARNING - el usuario ${usuarioId} intentó eliminar un agente (${agenteId}) propiedad de otro usuario (${agente.owner})`);
        next(createError(401, 'No autorizado'));
        return;
      }

      await Agente.deleteOne({ _id: agenteId });

      res.redirect('/privado');

    } catch (error) {
      next(error);
    }
```

`app.js` acuerdate siempre que la zona privada está protegida con `sessionAuthMiddleware` recuerda esto para el proyecto final porque esto lomiramos bien. Las rutas se tiene que proteger.

```js
// Zona privada del usuario
// app.get('/privado', sessionAuthMiddleware, privadoController.index);
// app.get('/agentes-new', sessionAuthMiddleware, agentesController.new);
// app.post('/agentes-new', sessionAuthMiddleware, agentesController.postNewAgent);
app.get('/agentes-delete/:agenteId', sessionAuthMiddleware, agentesController.deleteAgent)
```

### Autentificacion por API key 


* La autenticación con API Key se basa en enviar en todas las peticiones un API Key (como cabecera o parámetro GET)
* Éste API Key suele ser suministrada por el servicio tras el registro de usuario.
* El API Key es único e identifica al usuario de manera unívoca.
* El API Key siempre es la misma, no cambia ni hay que renovarla.

![](nodeapp/public/assets/img/6readme.png)


### Tokens (y JWT)

![](nodeapp/public/assets/img/7readme.png)


* JWT es un estándar abierto (RFC 7519)
* Define una forma segura y compacta de transmitir información entre diferentes partes con un objeto JSON
* La información puede ser verificada porque va firmada digitalmente
* Se pueden firmar los tokens con HMAC (algoritmo secreto) o con un par de claves RSA (pública/privada)
* el mejor sitio para guardar un token en el Browser es en una cookie como las que hemos visto

![](nodeapp/public/assets/img/8readme.png)

Un JWT está formado por tres partes:

* Cabecera: JSON en formato base64 que indica el tipo de token y el algoritmo usado para firmarlo
* Payload: JSON de datos en formato base64. Contiene los claims (claves del diccionario de datos).
* Firma: firma del token resultante de aplicar la siguiente fórmula: 
> HMACRSA256( base64(header) + base64(payload) + <secret> )

**¿Por qué usar JWT?**

* Evitamos guardar la sesión del usuario en la base de datos.
* Es el usuario quien nos envía su información en todo momento.
* Es seguro, porque podemos validar que el token no ha sido alterado desde que nosotros lo enviamos.


Autenticación por JWT

![](nodeapp/public/assets/img/9readme.png)

---
> [!IMPORTANT]
> Segimos con el codigo: implantamos token JWT
---

vamos a `controlers/LoginControllers.js` y genero ` postJWT(req, res, next)`

(merece la pena pagar con esta auth const `jsonwebtoken`)

`npm install jsonwebtoken`


```js
const jwt = require('jsonwebtoken');
// const Usuario = require('../models/Usuario');

// class LoginController {

//   index(req, res, next) {}

//   async post(req, res, next) {}

//   logout(req, res, next) {}

  async postJWT(req, res, next) {
    try {
      const { email, password } = req.body;

      // buscar el usuario en la base de datos
      const usuario = await Usuario.findOne({ email: email });

      // si no lo encuentro o la contraseña no coincide --> error
      if (!usuario || !(await usuario.comparePassword(password)) ) {
        res.json({ error: 'Invalid credentials' });
        return;
      }

      // si existe y la contraseña coincide --> devuelvo un JWT
      const tokenJWT = await jwt.sign({ _id: usuario._id }, 's876ads87dasuytasduytasd', {
        expiresIn: '2h'
      });
      res.json({ jwt: tokenJWT });

    } catch (err) {
      next(err);
    }
  }
}
module.exports = LoginController;
```

en `app.js`

```js
// esto lo utilizaré para los dos, api y website
const loginController = new LoginController();

/**
 * Rutas del API
 */
// app.use('/api-doc', swaggerMiddleware);
app.post('/api/login', loginController.postJWT);
// app.use('/api/agentes', require('./routes/api/agentes'));
```

Abro `POSTMAN` y hago un post

![](nodeapp/public/assets/img/14readme.png)

y vemos la respuesta del servidor, nos ha devuelvo un JToken. Si lo coias y lo pegas en http://jwt.io verás el contenido.


Ahora mismo si pones en Browser `http://localhost:3000/api/agentes` saldrá por pantalla la lista de agentes sin haber pedido ninguna autentificación

```sh
{"results":[{"_id":"657c862970776a3eba495f55","name":"Smith","age":33,"owner":"657c862970776a3eba495f4f","__v":0},{"_id":"657c862970776a3eba495f57","name":"Brown","age":46,"owner":"657c862970776a3eba495f50","__v":0},{"_id":"657c9cbd5c692b557a6a26ca","name":"AlexJust","age":18,"owner":"657c862970776a3eba495f4f","__v":0}]}
```
Entonces lo que queremos es que cuando este middelware reciba una petición, que haga la validación de que tiene el JWToken correcto.

```js
/**
 * Rutas del API
 */
// app.use('/api-doc', swaggerMiddleware);
// app.post('/api/login', loginController.postJWT);
app.use('/api/agentes', require('./routes/api/agentes'));
```
Para esto nos creamos un middelware para esto `lib/jwtAuthMiddleware.js` y creo un modulo que esporta un middleware. 

```js
var createError = require('http-errors');
const jwt = require('jsonwebtoken');

// modulo que exporta un middleware
module.exports = async (req, res, next) => {
  try {

    // recoger el jwtToken de la cabecera, o del body, o de la query string
    const jwtToken = req.get('Authorization') || req.body.jwt || req.query.jwt;

    // comprobar que mandado un jwtToken
    if (!jwtToken) {
      next(createError(401, 'no token provided'));
      return;
    }

    // comprobaremos que el token en válido
    jwt.verify(jwtToken, process.env.JWT_SECRET, (err, payload) => {
      if (err) {
        next(createError(401, 'invalid token'));
        return;
      }
      // apuntamos el usuario logado en la request
      req.usuarioLogadoAPI = payload._id;
      // dejamos pasar al siguiente middleware
      next();
    })


  } catch (error) {
    next(error);
  }
}
```

Acuérdate que el `LoginController.js` teníamos 

```js
      // si existe y la contraseña coincide --> devuelvo un JWT
      const tokenJWT = await jwt.sign({ _id: usuario._id }, 's876ads87dasuytasduytasd', {
```

vamos a ponerlo en `.env`

```sh
JWT_SECRET='s876ads87dasuytasduytasd'
```
y en `.env.example` JWT_SECRET=secret  
es por esto que hemos puesto esta lina así `jwt.verify(jwtToken, process.env.JWT_SECRET,` en `module.exports`

voy a `app.js`

```js
// const swaggerMiddleware = require('./lib/swaggerMiddleware');
// const basicAuthMiddleware = require('./lib/basicAuthMiddleware');
// const sessionAuthMiddleware = require('./lib/sessionAuthMiddleware');
const jwtAuthMiddleware = require('./lib/jwtAuthMiddleware');

...

/**
 * Rutas del API
 */
// app.use('/api-doc', swaggerMiddleware);
// app.post('/api/login', loginController.postJWT);
app.use('/api/agentes', jwtAuthMiddleware, require('./routes/api/agentes'));
```

en `routes/api/agentes.js`

```js
router.get('/', async (req, res, next) => {
  try {

    // escribimos en el log el id del usuario logado en el API con JWT
    console.log('Usuario API :', req.usuarioLogadoAPI): 
```

te imprimirá el Id del usuario que podriamos utilizar para si quisiéramos hacer alguna petición a la base de datos y sacar solamente sus agentes en vez de sacar todos pues podríamos sacar los suyos.

Si te fijas cada agente tiene un owner, (lo puedes ver en la base de datos NoSqlBosster) pues podrías filtar con owner con filtros : 

```js
router.get('/', async (req, res, next) => {
  try {

    // escribimos en el log el id del usuario logado en el API con JWT
    const usuarioIdLogado = req.usuarioLogadoAPI;

    // // filtros
    // // http://127.0.0.1:3000/api/agentes?name=Jones
    // const filterByName = req.query.name;
    // const filtreByAge = req.query.age;
    // // paginación
    // // http://127.0.0.1:3000/api/agentes?skip=2&limit=2
    // const skip = req.query.skip;
    // const limit = req.query.limit;
    // // ordenación
    // // http://127.0.0.1:3000/api/agentes?sort=-age%20name
    // const sort = req.query.sort;
    // // field selection
    // // http://localhost:3000/api/agentes?fields=name%20-_id%20address
    // const fields = req.query.fields;

    // const filtro = {};

    // if (filterByName) {
    //   filtro.name = filterByName;
    // }

    // if (filtreByAge) {
    //   filtro.age = filtreByAge;
    // }

    filtro.owner = usuarioIdLogado;
```

Con esto cuando hagas una petición te devolverá sólo los agentes que son de tu propiedad. Tanto si quiere eliminar algo como soliciatr, siempre con el filtro del usuario propietario del espacio.


## Tareas en segundo plano

Cuando tenemos una app en el movil, le damos alguna cosa y tarda, esto es pésimo para el producto. La percepción del usuario a ver que es lento, el usuario no quiere ese servicio. Si realmente sabemos que va a tardar la respuesta, hemos de resolver las espectativas del usuario. Por ejemplo si pide la codificacion de un video que tarda 1h, pues has de decirle con una progreso como avanza, en front. 

**Tareas lentas o pesadas**

* Las respuestas a las peticiones HTTP deben ser casi inmediatas 
* Un backend lento, es un backend mal hecho
* Pero a veces hay que hacer cosas que son lentas como enviar correo, redimensionar una imagen, leer archivos gigantes...
* Todas esas tareas lentas, deberemos hacerlas de manera diferida


**VAMOS A ENVAR CORREOS DESDE LA APLICACION**

Es sencillo

https://github.com/nodemailer/nodemailer

https://nodemailer.com/

```sh
npm install nodemailer
```

Hay palataformas para desarrolladores como 
* https://sendgrid.com/en-us/free y es gratis;   
* https://www.brevo.com/
* https://www.mailgun.com/

El spam es un problema, como la suplantación, gracias a esto ya no es tan sencillo enviar un email, cierto volumne de correo los servidores se protejen mucho por culpa del spam. Los servidores tiene como mision tratar de reducir el spam todo lo que sea posible, si nuestra app va a enviar volumne de correos directamente iremos a la carpate de espam.

Hay dos tipos de emial, los 
* `transaccionales`(avisas al usario en base a algo que el usuario a hecho p.e envío email con el link) el usuario tiene un contexto del porque se envía este emial
* `marketing` (p.e novedades del producto, etc).

https://nodemailer.com/ permite conetcarse a las pataformas de estos servidores de emial casi sin cmabiar nada. En el enlace tienes los ejmplos de como aplicarlo. Además tiene un servicio que ayuda a los desarrolladores https://ethereal.email/ para que no se envíe realmente y que solo sea para ver si funciona bien.

Vamos hacer que en nuestro frontend cuando el usuario haga login vamos a enviarle un email antes de mandarle a otro sitio

`loginController.js`

```js
  async post(req, res, next) {
    try {
      // const { email, password } = req.body;

      // // buscar el usuario en la base de datos
      // const usuario = await Usuario.findOne({ email: email });

      // // si no lo encuentro o la contraseña no coincide --> error
      // if (!usuario || !(await usuario.comparePassword(password)) ) {
      //   res.locals.error = req.__('Invalid credentials');
      //   res.locals.email = email;
      //   res.render('login');
      //   return;
      // }

      // // si existe y la contraseña coincide --> zona privada
      // // apuntar en la sesión del usuario, que está autenticado
      // req.session.usuarioLogado = usuario._id;

      // enviar email al usuario
      const emailResult = await usuario.sendEmail('Bienvenido', 'Bienvenido a NodeApp');
      console.log('Email enviado', emailResult); // luego ya lo quitamos

  //     res.redirect('/privado');

  //   } catch (err) {
  //     next(err);
  //   }

  // }
```

vamos h hacernos el método `sendEmail` en el `models/usuario` para enviar emails ¿sería un metodo estático o de instancia? utilizaremos una instancia porque necesitamos el emial del usuario entonces usaremos `methods`

```js
const nodemailer = require('nodemailer');

...


// método para enviar emails al usuario
usuarioSchema.methods.sendEmail = async function(asunto, cuerpo) {

  // crear un transport [lo he creado en lib/emailTransportConfigure.js]
  const transport = await emailTransportConfigure();

  // enviar email
  ...
}
``` 

`lib/emailTransportConfigure.js`

```js
const nodemailer = require('nodemailer');

module.exports = async function() {

  // entorno desarrollo
  const testAccount = await nodemailer.createTestAccount();

  const developmetTransport = {
    host: testAccount.smtp.host, //'smtp.ethereal.email',
    port: testAccount.smtp.port,
    secure: testAccount.smtp.secure,
    auth: {
        user: testAccount.user,
        pass: testAccount.pass
    }
  }

  const transport = nodemailer.createTransport(developmetTransport);

  return transport;
}
```

continúo... `models/usuario`

```js
// método para enviar emails al usuario
usuarioSchema.methods.sendEmail = async function(asunto, cuerpo) {
  // crear un transport
  const transport = await emailTransportConfigure();

  // enviar email
  const result = await transport.sendMail({
    from: process.env.EMAIL_SERVICE_FROM, // esto en .env EMAIL_SERVICE_FROM=admin@example.com
    to: this.email,
    subject: asunto,
    // text: --> para emails con texto plano
    html: cuerpo
  });
  console.log(`URL de previsualización: ${nodemailer.getTestMessageUrl(result)}`);
  return result;
}
```

Ahora por consola si recargas el usaurio en el browser verás como 

```sh
POST /login - - ms - -
URL de previsualización: https://ethereal.email/message/ZYHhhESjrQqJ-uwEZYHhwX9x6n-bKr2VAAAAAYiccvnrtvhgkd4MUGtaHio
Email enviado {
  accepted: [ 'admin@example.com' ],
  rejected: [],
  ehlo: [ 'PIPELINING', '8BITMIME', 'SMTPUTF8', 'AUTH LOGIN PLAIN' ],
  envelopeTime: 185,
  messageTime: 176,
  messageSize: 291,
  response: '250 Accepted [STATUS=new MSGID=ZYHhhESjrQqJ-uwEZYHhwX9x6n-bKr2VAAAAAYiccvnrtvhgkd4MUGtaHio]',
  envelope: { from: 'admin@example.com', to: [ 'admin@example.com' ] },
  messageId: '<e0033287-2fd0-0563-f1d9-b037dc148261@example.com>'
}
```

si te vas a `https://ethereal.email/message/ZYHhhESjrQqJ-uwEZYHhwX9x6n-bKr2VAAAAAYiccvnrtvhgkd4MUGtaHio` verás la estructura de tu email que se ha enviado de verdad con ese contenido


![](nodeapp/public/assets/img/15readme.png)


Ahora vamos hacer para que quede preparado para producción.


`lib/emailTransportConfigure.js`

```js
// const nodemailer = require('nodemailer');

// module.exports = async function() {

//   // entorno desarrollo
//   const testAccount = await nodemailer.createTestAccount();

//   const developmetTransport = {
//     host: testAccount.smtp.host, //'smtp.ethereal.email',
//     port: testAccount.smtp.port,
//     secure: testAccount.smtp.secure,
//     auth: {
//         user: testAccount.user,
//         pass: testAccount.pass
//     }
//   }

  const productionTransport = {
    // que sea configurable
    service: process.env.EMAIL_SERVICE_NAME, 
    auth: {
      user: process.env.EMAIL_SERVICE_USER,
      pass: process.env.EMAIL_SERVICE_PASS,
    }
  }

  console.log('process.env.NODE_ENV', process.env.NODE_ENV); // haz login y lo verás

  // uso la configuración del entorno en el que me encuentro
  const activeTransport = process.env.NODE_ENV === 'development' ?
    developmetTransport :
    productionTransport;

  const transport = nodemailer.createTransport(activeTransport);

  // return transport;
}
```

`package.json`

```sh
  "scripts": {
    "start": "cross-env node NODE_ENV=production ./bin/www",
    "dev": "cross-env NODE_ENV=development DEBUG=nodeapp:* nodemon ./bin/www",
```
esto nos dice si estamos en modo desarrollador o en produccion, entonces aquí podríamos utilizar esto 

```js
  // uso la configuración del entorno en el que me encuentro
  const activeTransport = process.env.NODE_ENV === 'development' ?
    developmetTransport :
    productionTransport;
``` 

ahora cuando nos logueamos podemos ver en terminal 

```sh
npm run dev

...


POST /login - - ms - -
process.env.NODE_ENV development
URL de previsualización: https://ethereal.email/message/ZYHmVkSjrQqJ.EtHZYHmk34VFVvG.PRsAAAAAfD0.da9H60FawqCIKrsrRI
```

se hubiera enviado el emial al usuario logueado. NO tiene más historia.
Lo que si hemos notado es que cuando hacemos lgoin tarda mucho a cargar la página y esperaríamos que tardase nada, instanteneo. Eso es muy malo ¿qué podemos hacer?

Para este caso concreto podemos hacer una cosa sencilla  `loginController.js` le quito el awaut que significa que antes que pueda entrar le envío el emial, entonces para esta caso me puedo permitir quitar esa espera de envío de emial antes de que pueda redirigirlo a la area privada.

```js
const emailResult = usuario.sendEmail('Bienvenido', 'Bienvenido a NodeApp');
```

en otro caso, si que tendría que utilizar ua tecnica quemepermita enviar ese emial y que el usuario no tenga que esperar a que se envíe.

**Recuerda que : Tareas lentas o pesadas**

* Las respuestas a las peticiones HTTP deben ser casi inmediatas 
* Un backend lento, es un backend mal hecho
* Pero a veces hay que hacer cosas que son lentas como enviar correo, redimensionar una imagen, leer archivos gigantes...
* Todas esas tareas lentas, deberemos hacerlas de manera diferida

**Flujo básico de una petición HTTP**

![](nodeapp/public/assets/img/10readme.png)

**Envío de un e-mail desde un back-end**

![](nodeapp/public/assets/img/11readme.png)

**Envío con tarea programada**

![](nodeapp/public/assets/img/12readme.png)

**Comandos personalizados en NPM**

NPM nos permite crear nuestro propios comandos personalizados

Esto es útil para ejecutarlos desde la consola o a través de tareas programadas (con cron)

Tan sólo hay que crear una entrada en package.json, apartado scripts

**VEAMOS OTRA FORMA DE HACER MUCHO MEJOR:**

**Envío de un e-mail con tareas en background**

![](nodeapp/public/assets/img/13readme.png)


**Tareas en background con RabbitMQ**

Es un software de envio de mensajes entre aplicativos. No tiene nada que ver con el emial o whatsapp, son mensajes entre aplicativos para que se cominquen a través de este software- RabbitMQ es un software de envío de mensajes que implementa el protocolo AMQP

https://rabbitmq.com/#getstarted

Podemos usarlo desde:

* Instalación Win/linux/Mac https://www.rabbitmq.com/download.html
* Docker https://hub.docker.com/_/rabbitmq/
* CloudAMQP https://www.cloudamqp.com/

El servicio que usaremos nosotros es CloudAMQP https://www.cloudamqp.com/  y me creo una instancia

![](nodeapp/public/assets/img/16readme.png)

Y usarlo desde amazon, cloud foundry, etc...

Las credenciales para conectar estám en OVERVIEW

Creo carpeta `ejemplo-rabbitmq` y los archivos `consumer.js` y `publisher.js` que se comunicarán entre ellos. Ahora me voy a 

```sh
cd ejemplo-rabbitmq
npm init -y
```
esto creará un packeage.json y voy a utilizar una librería `AMQP` advence message q protocol

```sh
npm repo amqplib
npm install amqplib
```

`publisher.js`

utilizamos estas técnicas https://amqp-node.github.io/amqplib/channel_api.html

```js
'use strict';

require('dotenv').config();

const amqplib = require('amqplib');
const EXCHANGE = 'task-request';

main().catch(err => console.log('Hubo un error', err));

// const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

async function main() {
  // conectar al broker de RabbitMQ
  // la URL la sacamos de AMQP details https://api.cloudamqp.com/ (está en .env)
  const connection = await amqplib.connect('amqps://ltdqkxwg:nLJMO8DacQSTA4mUtMXLzR707hamH0Gl@whale.rmq.cloudamqp.com/ltdqkxwg'); 

  // crear un canal
  const canal = await connection.createChannel();

  // asegurar que existe un exchange (si no lo crea)
  // https://amqp-node.github.io/amqplib/channel_api.html#channel_assertExchange
  await canal.assertExchange(EXCHANGE, 'direct', {
    durable: true // the exchange will survive broker restarts
  })
  
  const mensaje = {
    tarea : 'enviar un email'
  };
  console.log('enviado mensaje', mensaje);

  canal.publish(EXCHANGE, '*', Buffer.from(JSON.stringify(mensaje)), {
    persistent: true, // the message will survive broker restarts
  });

  console.log('enviado mensaje', mensaje);
  // await sleep(1);
}
```

ya ùedes empezar a comprovar si tienes conexion

```sh
cd ejemplo-rabbitmq
npx nodemon publisher.js
```
Si te vas a `RabbitMQ Manager` en https://customer.cloudamqp.com/instance haces click y te vas a `Eschanges` y en Exchange: task-request verás el gráfico con la señal.

> [!NOTE] 
> Quien publica, publica un Exchange y quien recibe recibe una Coda. La Coda está asociada a quien recibe el mensaje normalmente.
> Puedes ver lo patrones que hai en :
> https://www.rabbitmq.com/getstarted.html
>
> La X en los gráficos es un Exchange y ese X fíjate que lo envía a una o varias colas. 
> * `Publish/Subscribe` lo enviará a todas las colas, lo enviará a todo el suscrito
> * `Work Queues` irá uno detrás de otro
> ¿a qué codas se lo enviaremos nosotros? `keepSending = canal.publish(EXCHANGE, '*', Buffer.from(JSON.stringify(mensaje)), {` a todas, poreso hemos puesto el `*`.
> * `Routing` dependiendo del tipo de `log` el exchange lo enviará a una cola u otra. Si es por ejemplo un log de error pues le llega a un consumidor, por ejempli que elerte al admin del sistema. El log de warning lo enviaría a un fichero de log, o lo que sea.
> * `Topics` podríamos definirle comodines 
> * `RPC` de petición respuesta, hace petición y espera una respuesta.

Nosotros vamos hacer funcionar `Work Queues`

Voy a meterlo en un bucle infonito para que esté publicando mensajes constantemente.
Vpu a instalar `npm install dotenv` para que me cargue las variable de entorno. Esto es porque la clave la vamos a meter en `.env` en `cd ejemplo-rabbitmq`

```js
'use strict';

require('dotenv').config();

const amqplib = require('amqplib');
const EXCHANGE = 'task-request';

main().catch(err => console.log('Hubo un error', err));

const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

async function main() {
  // conectar al broker de RabbitMQ
  const connection = await amqplib.connect(process.env.RABBITMQ_BROKER_URL);

  // crear un canal
  const canal = await connection.createChannel();

  // asegurar que existe un exchange
  await canal.assertExchange(EXCHANGE, 'direct', {
    durable: true // the exchange will survive broker restarts
  })

  let keepSending = true;

  while (true) {
    // publicar mensajes
    const mensaje = {
      tarea: 'enviar un email' + Date.now()
    };

    // verificar si puedo enviar más o tengo que darle un respiro
    if (!keepSending) {
      // esperar a que se drene (vacie) el buffer de escritura del broker
      console.log('Buffer lleno, espero a que se vacie');
      await new Promise(resolve => canal.on('drain', resolve))
    }

    keepSending = canal.publish(EXCHANGE, '*', Buffer.from(JSON.stringify(mensaje)), {
      persistent: true, // the message will survive broker restarts
    });

    console.log('enviado mensaje', mensaje);
    // await sleep(1);
  }

}
```

Creamos el publicador ahora vamos a crear el consumidor `consumidor.js`

```js
'use strict';

require('dotenv').config();

const amqplib = require('amqplib');

const QUEUE = 'tasks';

main().catch(err => console.log('Hubo un error', err));

const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

async function main() {
  // conectar al broker de RabbitMQ
  const connection = await amqplib.connect(process.env.RABBITMQ_BROKER_URL);

  // crear un canal
  const canal = await connection.createChannel();

  // asegurar que existe la cola para recibir mensajes
  // es responsabilidad del consumir que exista esta cola
  await canal.assertQueue(QUEUE, {
    durable: true, // the queue will survive broker restarts
    // si prebalece la velocidad que una poca pérdida de mensajes pondrías a FALSE
    // y lo gestionaría todo en memoria
  });

  // le dice al publicador "dame 1 mensaje y hasta que no acabae no me des más"
  canal.prefetch(1); // pending ack's

  canal.consume(QUEUE, async mensaje => {
    // sacamos el contenido del Buffet (esto puede ser cualquier tipo de fichero ahora es toString()
    const payload = mensaje.content.toString();
    console.log(payload);
    await sleep(150);
    canal.ack(mensaje);
  });

}
```

Tengo arrancado y enviando mesajes ` npx nodemon publisher.js` 

```sh
enviado mensaje { tarea: 'enviar un email1703079460016' }
enviado mensaje { tarea: 'enviar un email1703079462018' }
enviado mensaje { tarea: 'enviar un email1703079464018' }
enviado mensaje { tarea: 'enviar un email1703079466020' }
enviado mensaje { tarea: 'enviar un email1703079468022' }
enviado mensaje { tarea: 'enviar un email1703079470024' }
enviado mensaje { tarea: 'enviar un email1703079472025' }
```

Ahora arranco `npx nodemon consumer.js` y verás que no parece que nos llegue ningún mensaje, esto es porque no lo hemos conectado:

A mano (se podría hacer en codigo pero yo prefiero a mano) conectamos la cola del consumidor https://whale.rmq.cloudamqp.com/#/exchanges/ltdqkxwg/task-request:

![](nodeapp/public/assets/img/17readme.png)


Ahora si que nos aparecen los mensajes en consola

```sh
➜  ejemplo-rabbitmq git:(main) ✗ npx nodemon consumer.js
[nodemon] 3.0.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,cjs,json
[nodemon] starting `node consumer.js`
{"tarea":"enviar un email1703079645001"}
{"tarea":"enviar un email1703079647004"}
{"tarea":"enviar un email1703079649007"}
{"tarea":"enviar un email1703079651009"}
```

Puedes ver que llegan y vemos que son string, podrían ser ficheros o lo que sea que se envia y consume.

![](nodeapp/public/assets/img/18readme.png)

Ves que hay una tarea abierta, entra en task

![](nodeapp/public/assets/img/19readme.png)

Puedes ver que están llegando los mensajes en `Message rates last ten minutes ` y en `Queued messages last ten minutes ` nos dice la cola que tiene, puedes ver que es baja o que no hay porque le estamos diciendo `canal.ack(mensaje);` en 

![](nodeapp/public/assets/img/20readme.png)


Ahora voy hacer que el que ublica lo haga más rápido  `await sleep(100);`  
Podríamos hacer que el consumidor tarde más, y que cada vez que consuma lo escriba por la consola , cómo configurar al consumidor para que le diga a la cola que se vaya esperando porque estoy trabajando: con `canal.prefetch(1); // pending ack's`

Si duplicas el terminal y abres otro consumidor `npx nodemon consumer.js` también consumirá, puedes abrir tantos consumidores como creas y consumirán lo que publique el publicador. Escalas tu apicación escalando el proceso consumidor.

Este metodo nos permite tener varios elementos que publican y varios que consuman.

**Podrías instaar RabbitMQ en LOCAL**

Local descargando o con Docker es una buena alternativa (en la documentacion tienes como hacerlo)

* Instalación Win/linux/Mac https://www.rabbitmq.com/download.html
* Docker https://hub.docker.com/_/rabbitmq/

```sh
# latest RabbitMQ 3.12
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.12-management
```
tienes la explicacion en el video 5.mpp minuto 3:57


**Docker**

```sh
docker run
  -d --hostname=mq --name mq -p 8080:15672 -p 5672:5672
  rabbitmq:3-management
```

```sh
npm i amqplib
```

Uso con Node.js http://www.squaremobius.net/amqp.node/

Ejemplos estratégias https://github.com/squaremo/amqp.node/blob/master/examples/tutorials/README.md


¿Que tamaño pueden tener los mensajes?  

Teoricamente: 2^64 bytes  

En la práctica depende de las máquinas que usemos y la conexión.  

Si los mensajes son grandes mejor enviar una ruta a un servidor de ficheros o similar.


Usando MongoDB: 
* https://github.com/chilts/mongodb-queue
* https://github.com/scttnlsn/monq 
* https://github.com/agenda/agenda

Usando Redis: 
* https://github.com/bee-queue/bee-queue
* https://github.com/OptimalBits/bull
* https://github.com/Automattic/kue


---

> [!IMPORTANT]
> Seguimos 




Ahora predendemos que en el `amodels/usuario` cuando nos toca enviar un email `usuarioSchema.methods.sendEmail = async function(asunto, cuerpo) {` en cevz de enviarlo, que lo envía a ... para encargárselo a un microservicio; es decir al `consumer` que nos vamos hacer.

Lo primero es traerme a mi `.env` de la aplicacion la dirección del servidor que hemos abierto antes con `RabittQM` : `RABBITMQ_BROKER_URL=amqps://suuberqm:5RRPrm2BvZyYHXV8gxh4KmVvzAyexTwd@whale.rmq.cloudamqp.com/suuberqm`

Instalo en la app la librería `npm i amqplib` (antes la habías instaldo en el ejemplo)

```sh
npm i amqplib
npm run dev
```

verás qe sigue teniedo la sesion abierta porque la tenemos guardada en mongodb, en los controladores cuando hacía `loginControler` teníamos que llamaba a 


```js
// enviar email al usuario
      const emailResult = usuario.sendEmail('Bienvenido', 'Bienvenido a NodeApp');
      console.log('Email enviado', emailResult);
```

ahora en el `amodels/usuario`  vamos hacernos un método para pedir otro serbicio que envía un email.

```js
// método para pedir a otro servicio que envie un email (RabbitMQ)
usuarioSchema.methods.sendEmailRabbitMQ = async function(asunto, cuerpo) {
  // cargar rabbitMQLib y enviamos un mensaje
  // AHORA NOS CREAMOS UN lib/rabbitMQLib.js
  const canal = await canalPromise;

  ... continuará

}
```

`lib/rabbitMQLib.js` ya que es habitual que más tarde en nuestras partes de la app queramos utilizar el envío de mensajes con otros microservicios. 

```js
const amqplib = require('amqplib');

// conectar al broker de RabbitMQ
const canalPromise = amqplib.connect(process.env.RABBITMQ_BROKER_URL)
  .then(connection => {
    // crear un canal
    return connection.createChannel();
  })

module.exports = canalPromise;
```

En `usuario`

```js
// cargamos la promesa de un canal
const canalPromise = require('../lib/rabbitMQLib');

...


// método para pedir a otro servicio que envie un email (RabbitMQ)
usuarioSchema.methods.sendEmailRabbitMQ = async function(asunto, cuerpo) {
  // cargar rabbitMQLib y enviamos un mensaje
  // AHORA NOS CREAMOS UN lib/rabbitMQLib.js
  const canal = await canalPromise;
  // asegurar que existe el exchange
  const exchange = 'email-request'
  await canal.assertExchange(exchange, 'direct', {
    durable: true // the exchange will survive broker restarts
  });

  const mensaje = {
    asunto,
    to: this.email,
    cuerpo
  };

  canal.publish(exchange, '*', Buffer.from(JSON.stringify(mensaje)), {
    persistent: true, // the message will survive broker restarts
  });
}
```

Vamos a `loginController` y cambiamos el método

```js
// enviar email al usuario
// usuario.sendEmail('Bienvenido', 'Bienvenido a NodeApp');
usuario.sendEmailRabbitMQ('Bienvenido', 'Bienvenido a NodeApp');
console.log('Email enviado', emailResult);
```

Ahora puedes ver que se ha creado un Exchange `email-request` ya teníamos el Exchange de `tasks-request` de antes.

![](nodeapp/public/assets/img/21readme.png)

---

Ahor anos gustaría tener un microservicio que cuando conectáramos el Exchange a una cola de un microServicio ese microservicio es una aplicacion aparte y normalmente no lo tendremos en la misma carpeta de la aplicación.

> [!NOTE]
> En este caso me crearé una carpeta de `nodeapp/micro-services` y aquí voy a colocar los consumidores de los microservicios; en un proyecto real normalmente se mantiene en un repositorio de código separado, porque al fin y al cabo es otra palicación. Conceptualmente yo ya comenzaría a llamar a la app que estamos creando **plataforma** porque tenemos dos aplicaciones una nuestra app y por otra los microservicios de os emials. Con lo ucal podríamos tener dos repos con cada aplicación. Para este caso vamos a hacerlo así `nodeapp/micro-services` pero siendo conscientes que son aplicaciones distintas.
> NodeApp no está llamando al microservicio, NOdeApp está enviando un mensaje y el microservicio recibe el mensaje y hace lo qu etenga que hacer. Entre ellos no se comunican, se comunican a través de RabbitMQ.

Creo archivo `nodeapp/micro-services/emailConsumer.js` 

```js
'use strict';

require('dotenv').config();

const amqplib = require('amqplib');
const nodemailer = require('nodemailer');

const QUEUE = 'email-sender';

main().catch(err => console.log('Hubo un error', err));

const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

async function main() {
  // conectar al broker de RabbitMQ
  const connection = await amqplib.connect(process.env.RABBITMQ_BROKER_URL);
  const transport = await createTransport();

  // crear un canal
  const canal = await connection.createChannel();

  // asegurar que existe la cola para recibir mensajes
  await canal.assertQueue(QUEUE, {
    durable: true, // the queue will survive broker restarts
  });

  canal.prefetch(1); // pending ack's [consejo: siempre arranca con 1]
                     // mide rendimiento y si es estable comienza augmentar
                     // el prefetch(10) para que envíe más 

  canal.consume(QUEUE, async mensaje => {
    const payload = JSON.parse(mensaje.content.toString());

    const result = await transport.sendMail({
      from: process.env.EMAIL_SERVICE_FROM,
      to: payload.to,
      subject: payload.asunto,
      html: payload.cuerpo // text: --> para emails con texto plano
    });
    console.log(`URL de previsualización: ${nodemailer.getTestMessageUrl(result)}`);

    canal.ack(mensaje);
  });

}

async function createTransport() {
  // entorno desarrollo
  const testAccount = await nodemailer.createTestAccount();

  const developmetTransport = {
    host: testAccount.smtp.host, //'smtp.ethereal.email',
    port: testAccount.smtp.port,
    secure: testAccount.smtp.secure,
    auth: {
        user: testAccount.user,
        pass: testAccount.pass
    }
  }

  return nodemailer.createTransport(developmetTransport);
}
```

en `package.json`

```json
  "scripts": {
    "start": "cross-env node NODE_ENV=production ./bin/www",
    "dev": "cross-env NODE_ENV=development DEBUG=nodeapp:* nodemon ./bin/www",
    "init-db": "node init-db.js",
    "email-sender-service": "node ./micro-services/emailSender.js"
  },
```
Y ya que lo apunto en `package.json` lo apunto en el `README.md`

```sh
## Start

In production:

### Start email sender service

npm run email-sender-service

```

voy a verificar si funciona arrancando el microservicio 


```sh
➜  nodeapp git:(main) ✗ npm run email-sender-service

> nodeapp@0.0.0 email-sender-service
> node ./micro-services/emailSender.js
```

y nos vamos a ver si ha creado la cola que debería haber creado.

![](nodeapp/public/assets/img/22readme.png)


Ahora haces click en el **suuberqm	email-sender** y creamos un buildin para que desde Exchange `email-request` envíe todos `*`

![](nodeapp/public/assets/img/23readme.png)

Ahora con el Building hecho puedo probar si le están enviando mensajes.

---

> [!NOTE]
> VOY A CORRER RabbitMQ en local porque va lento.  
> `docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.12-management`  
> tengo que cambiar la cadena de conexión  `.env`  
> `RABBITMQ_BROKER_URL=amqp://guest:guest@localhost:5672`  
> Y ahora rranco los dos :   
> `npm run email-sender-service`  
> `npm run dev`  
> Una vez arracado ya te puedes ir a `[localhost:5672](http://localhost:15672/)` user: guets password:guest   
> verás que tienes la cola creada y el Exchange, si no lo ves loguéate.  
> Ahora falta conectado creando el Building de `Queues and Streams/Queue email-sender`  le colocas el `email-request` y el `*`   
> Te logueas y verás en terminal que funciona porque le hemos dicho en `emailSender` que en víe la URL `URL de previsualización: https://ethereal.email/message/ZYM1q0SjrQqJKClyZYM3C34VFVvGAETBAAAAAsjS0uxl2iH8Pv76sIZhLRc`  


Arranco con `npx nodemon ./micro-services/emailSender.js` porque así no vamos a estar trabajando parándolo y arranconadolo:

recuerda que esto es una aplicacion separada de nodeapp

```sh
➜  nodeapp git:(main) ✗ npx nodemon ./micro-services/emailSender.js
[nodemon] 3.0.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,cjs,json
[nodemon] starting `node ./micro-services/emailSender.js`
```
---

Ahora no voy a utilizar ni codigo ni librerías de mi app (nodeapp) porque no quiero que dependa de nodeapp porque asumo que está en un repositorio de c odigo distinto.

Voy a cojer el codigo de `lib/emailTransportConfigure.js` y me lo llevo a `./micro-services/emailSender.js` y lo vamos ajustar ahí.

```js
'use strict';

require('dotenv').config();

const amqplib = require('amqplib');
const nodemailer = require('nodemailer');

const QUEUE = 'email-sender';

main().catch(err => console.log('Hubo un error', err));

const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

async function main() {
  // conectar al broker de RabbitMQ
  const connection = await amqplib.connect(process.env.RABBITMQ_BROKER_URL);
  const transport = await createTransport();

  // crear un canal
  const canal = await connection.createChannel();

  // asegurar que existe la cola para recibir mensajes
  await canal.assertQueue(QUEUE, {
    durable: true, // the queue will survive broker restarts
  });

  canal.prefetch(1); // pending ack's

  canal.consume(QUEUE, async mensaje => {
    const payload = JSON.parse(mensaje.content.toString());

    const result = await transport.sendMail({
      from: process.env.EMAIL_SERVICE_FROM,
      to: payload.to,
      subject: payload.asunto,
      html: payload.cuerpo // text: --> para emails con texto plano
    });
    console.log(`URL de previsualización: ${nodemailer.getTestMessageUrl(result)}`);

    canal.ack(mensaje);
  });

}

async function createTransport() {
  // entorno desarrollo
  const testAccount = await nodemailer.createTestAccount();

  const developmetTransport = {
    host: testAccount.smtp.host, //'smtp.ethereal.email',
    port: testAccount.smtp.port,
    secure: testAccount.smtp.secure,
    auth: {
        user: testAccount.user,
        pass: testAccount.pass
    }
  }

  return nodemailer.createTransport(developmetTransport);
}

```

> [!NOTE]   
> Me fuí a `models/usuario` y en `// método para pedir a otro servicio que envie un email (RabbitMQ)` le añadí a quien debe enviarle el email con `const mensaje = {` esta linea para que se viera el emial `to: this.email,`


```js
// método para pedir a otro servicio que envie un email (RabbitMQ)
usuarioSchema.methods.sendEmailRabbitMQ = async function(asunto, cuerpo) {
  // cargar rabbitMQLib y enviamos un mensaje
  const canal = await canalPromise;
  // asegurar que existe el exchange
  const exchange = 'email-request'
  await canal.assertExchange(exchange, 'direct', {
    durable: true // the exchange will survive broker restarts
  });

  const mensaje = {
    asunto,
    to: this.email,
    cuerpo
  };

  canal.publish(exchange, '*', Buffer.from(JSON.stringify(mensaje)), {
    persistent: true, // the message will survive broker restarts
  });

}
```

y en `./micro-services/emailSender.js`  hemos de tener el destinatario `to: payload.to,`

A partir de ahora verás los picos en `http://localhost:15672/#/queues/%2F/email-sender` de los envío, en terminal verás la `URL de previsualización: https://ethereal.email/message/ZYM7mESjrQqJKxm5ZYM8lY1irCAB4kX0AAAAArXAZfmwnlM5OVSI4vCGz9Y` y todo funciona bien.



**Agenda** https://github.com/agenda/agenda ya es una solucion sólo para colas de tareas.

## WebSockets

Nos permite comunicar de una forma más versatil, por ejemplo en frondtend con el backend.

> [!NOTE]
> "WebSockets" es una tecnología de comunicación bidireccional en tiempo real que se utiliza en el desarrollo web y permite la transferencia de datos entre un servidor y un cliente de manera eficiente y en tiempo real. A diferencia de la comunicación HTTP tradicional, que es principalmente un protocolo de solicitud-respuesta, WebSockets establece una conexión persistente entre el cliente y el servidor, lo que permite la comunicación en tiempo real en ambas direcciones.
>

WebSockets es una tecnología avanzada que hace posible abrir una sesión de comunicación interactiva entre el navegador del usuario y un servidor

* Fuente: MDN https://developer.mozilla.org/es/docs/WebSockets-840092-dup

Características: 

* El protocolo es un estándar (RFC 6455)
* Está diseñada para ser implementada en navegadores y servidores web, pero puede utilizarse por cualquier aplicación cliente/servidor
* Usa los puertos habituales HTTP (80,443) Atraviesa firewalls y proxies

Casos prácticos

* Juegos online multijugador
* Aplicaciones de chat
* Rotativos de información deportiva
* Actualizaciones en tiempo real de las actividades de tus amigos


> [!IMPORTANT]
> Continúo con codigo

Creo carpeta `ejemplo-websockets/`

CReo `ejemplo-websockets/httpServer.js` y me crearé un servidor express mínimo, luego le haré un commit y después le añadimos websoker para que en un commit separado tengamos como se hace

```sh
cd ejemplo-websockets/
npm init -y
```

```sh
npm install express
```

`httpServer.js`

```js
'use strict';

const http = require('node:http');
const path = require('node:path');
const express = require('express');

const app = express();

app.use('/', (req, res, next) => {
  res.sendFile(path.join(__dirname, 'index.html'));
});

const server = http.createServer(app);

server.listen(3000, () => {
  console.log('Servidor HTTP arrancado en http://localhost:3000');
});
```

```sh
➜  ejemplo-websockets git:(main) ✗ npx nodemon httpServer.js
[nodemon] 3.0.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,cjs,json
[nodemon] starting `node httpServer.js`
Servidor HTTP arrancado en http://localhost:3000
```

nos creamos un archivo `ejemplo-websockets/index.html` y recargamos `commit  origin/main) 18: websockets, creamos un servidor http`


Usaremos la librería `npm repo socket.io`

```sh
npm i socket.io
```

creamos fichero `webSocketsServer.js`

```js
const socketio = require('socket.io');

// exportar una función que configura un servidor HTTP
module.exports = (server) => {

}
```

`httpServer.js`


```js
'use strict';

const http = require('node:http');
const path = require('node:path');
const express = require('express');
const webSocketsServer = require('./webSocketsServer');

const app = express();

app.use('/', (req, res, next) => {
  res.sendFile(path.join(__dirname, 'index.html'));
});

const server = http.createServer(app);

server.listen(3000, () => {
  console.log('Servidor HTTP arrancado en http://localhost:3000');
});

webSocketsServer(server);
```


`webSocketsServer.js`

```js
const socketio = require('socket.io');

// exportar una función que configura un servidor HTTP
module.exports = (server) => {
  const io = socketio(server); // por convencion le llamo io

  // ante cada conexión de un cliente (socket)
  io.on('connection', socket => {
    console.log('Nueva conexión de un cliente, con el id', socket.id);
  });
}
```

arracamos el servidor

```sh
➜  ejemplo-websockets git:(main) ✗ npx nodemon httpServer.js
[nodemon] 3.0.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,cjs,json
[nodemon] starting `node httpServer.js`
Servidor HTTP arrancado en http://localhost:3000
```

en el cliente, en el `index.html` haremos que se conecte con el servidor 

```html
    <script>
      $(function () {
        const socket = io();
      });
    </script>
  </body>
</html>
```

sólo con esto ya tienen un hilo de comunicacion entre servidor y cliente (html) para decirse cosas uno al otro. Si queremos una especie de chat

Ahora vamos hacer que el cliente envíe mensajes al servidor

```html
    <script>
      $(function () {
        const socket = io();

        // enviar mensajes al servidor
        $('form').submit(() => {
          const texto = $('#m').val();

          socket.emit('nuevo-mensaje', texto); // envía mensaje
          $('#m').val(''); // limpio el input
          return false;
        });
      });
    </script>
```


Vamos al servidor y le diremos dentro de cada conexión que nos llegua se va a invocar esta funcion socket, y a ese spcket le vamos a configurar los eventos:

```js
// const socketio = require('socket.io');

// // exportar una función que configura un servidor HTTP
// module.exports = (server) => {
//   const io = socketio(server); // por convencion le llamo io

  // ante cada conexión de un cliente (socket)
  io.on('connection', socket => {
    console.log('Nueva conexión de un cliente, con el id', socket.id);

    socket.on('nuevo-mensaje', texto => {
      // Verás por la terminal que si envías un mensaje te lo indicará
      console.log('mensaje recibido de un cliente', texto); // vamos a configurar los eventos
      // reenviar el mensaje a todos los sockets conectados
      io.emit('mensaje-desde-el-servidor', texto);
    })
//   });
// }
```

Vete a browser y envia un "Hola" y verás por la terminal que si envías un mensaje te lo indicará

```sh
[nodemon] starting `node httpServer.js`
Servidor HTTP arrancado en http://localhost:3000
Nueva conexión de un cliente, con el id K4jTi3rOX1q6tosNAAAB
mensaje recibido de un cliente 
mensaje recibido de un cliente 
mensaje recibido de un cliente 
mensaje recibido de un cliente rr
mensaje recibido de un cliente 
mensaje recibido de un cliente hola
```


Vamos a cliente `index.html` y pides recibir mensajes en el servidor `// recibir mensajes desde el servidor`

```html
    <script>
      $(function () {
        const socket = io();

        // enviar mensajes al servidor
        $('form').submit(() => {
          const texto = $('#m').val();

          socket.emit('nuevo-mensaje', texto);
          $('#m').val(''); // limpio el input
          return false;
        });

        // recibir mensajes desde el servidor
        socket.on('mensaje-desde-el-servidor', texto => {
          // añadir el texto a la lista
          const li = $('<li>').text(texto);
          $('#messages').append(li);
        })

      });
    </script>
```

Ahora si escribes en el Borwser verás como lo pinta en la pantalla del cliente. Si quieres habre ota pantalla en incognito y repite el chat. Pon las dos pantallas y verás como tienes a cada cliente cuando hay un mensaje de alguien a cada cliente le está enviando la informacion.


![](nodeapp/public/assets/img/24readme.png)


**El servidor envia mensajes cada X tiempo a los clientes**

`webSockeetsServer.js`

```js
const socketio = require('socket.io');

// exportar una función que configura un servidor HTTP
module.exports = (server) => {
  const io = socketio(server);

  // ante cada conexión de un cliente (socket)
  io.on('connection', socket => {
    console.log('Nueva conexión de un cliente, con el id', socket.id);

    socket.on('nuevo-mensaje', texto => {
      console.log('mensaje recibido de un cliente', texto);
      // reenviar el mensaje a todos los sockets conectados
      io.emit('mensaje-desde-el-servidor', texto);
    });

    setInterval(() => {
      socket.emit('noticia', 'noticia numero' + Date.now())
    }, 2000);

  });
}
```

Y en el cliente tambie´n voy hacer que escuche este tipo de mensjaes `// cuando llegue noticas que lo añada`

```html
    <script>
      // $(function () {
      //   const socket = io();

      //   // enviar mensajes al servidor
      //   $('form').submit(() => {
      //     const texto = $('#m').val();

      //     socket.emit('nuevo-mensaje', texto);
      //     $('#m').val(''); // limpio el input
      //     return false;
      //   });

      //   // recibir mensajes desde el servidor
      //   socket.on('mensaje-desde-el-servidor', texto => {
      //     // añadir el texto a la lista
      //     const li = $('<li>').text(texto);
      //     $('#messages').append(li);
      //   });

        // cuando llegue noticas que lo añada
        socket.on('noticia', texto => {
          const textoCompleto = 'Nueva noticia!: ' + texto
          const li = $('<li>').text(textoCompleto);
          $('#messages').append(li);
        })

      });
    </script>
```

Ahora puedes ver en el browser como van apareciendo noticias.

Con socket.io:

```js
// sending to sender-client only
socket.emit('message', "this is a test");

// sending to all clients, include sender
io.emit('message', "this is a test");

// sending to all clients except sender
socket.broadcast.emit('message', "this is a test");

// sending to all clients in 'game' room(channel) except sender
socket.broadcast.to('game').emit('message', 'nice game');

// sending to all clients in 'game' room(channel), include sender
io.in('game').emit('message', 'cool game');

// sending to sender client, only if they are in 'game' room(channel)
socket.to('game').emit('message', 'enjoy the game');

// sending to all clients in namespace 'myNamespace', include sender
io.of('myNamespace').emit('message', 'gg');

// sending to individual socketid
socket.broadcast.to(socketid).emit('message', 'for your eyes only');
```

---

> [!IMPORTANT]
> En video 5.mp4 minuto 1:50" explica como hacer una sala de chat por usuario.

---

A parte de Sockets exite muchas tras como `Server-sent events` muy parecido conceptualmente a sockets 

https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events


## Microservicios

La arquitectura de microservicios nace como respuesta a la problemática del mantenimiento y evolución de grandes aplicaciones monolíticas.


https://martinfowler.com/articles/microservices.html

Hasta ahora hemos creado una aplicación monolítica con `nodeapp`

**Problemática de las aplicaciones monolíticas**
* Si algo falla en la aplicación, todo el sistema se cae 
* Son difícil de escalar a nivel de infraestructura
* Es difícil de escalar a nivel de equipo de desarrollo (hasta que todo los desarrolladores saben cómo funciona todo el monolito)
* Es muy difícil mantener la interdependencia de relaciones y un código limpio (spaguetti code, merge conflics, etc...)
* Es la manera natural de desarrollar (todo software tiende a monolito)


**Ventajas de los microservicios** 5.mp4 21:50 muy interesante  
* Si falla un microservicio, no todo el sistema falla 
* Su infraestructura es fácilmente escalable
* Fácil de escalar los equipos de desarrollo (cada equipo sólo debe mantener un par de microservicios)
* Código más mantenible y reutilizable (menos interdependencias) 
* Permiten utilizar la mejor tecnología para cada problema
* Facilitan externalización a equipos de desarrollo de otras empresas


**Problemática de los microservicios**
* Orquestación: es más difícil hacer una buena coordinación de todos
* Entorno de desarrollo: arrancar un entorno de desarrollo con 300 microservicios te puede llevar un rato...solución: Docker/Vagrant
* Negociación y compromiso: cada micro servicio debe mantener sus contratos con otros micro servicios que los usan (no podemos cambiar cosas a la ligera).
* Gestión de logs. Están distribuidos, hay que unificarlos para facilitar la búsqueda 
*  Unión de datos: los JOIN debemos hacerlos a mano
* ¿Quién es el responsable de controlar la autenticación y autorización?


**API Gateway**
* Permite ser el único punto de entrada al sistema
* Comprueba la autenticación y autorización de los usuarios
* Los servicios tras él se despreocupan de la autenticación y autorización
* Puede actuar como proxy/router: enrutando la petición al servicio y devolviendo la misma respuesta que el servicio
* O puede actuar como gestor: haciendo varias peticiones y mezclando el resultado

**Características de un microservicio**
* Resolver un sólo problema y hacerlo muy bien
* Utilizar la mejor tecnología posible para resolver ese problema 
* Exponer un API con endpoints para interactuar con él
* Base de datos propia (a ser posible, se aceptan BD compartidas) 
* Conexión al bus de mensajes/cola de eventos (si es necesario) Librería cliente del API (a ser posible)

---
> [!IMPORTANT]
> **Monolith First**
> Comenzar una aplicación con una arquitectura de microservicios es arriesgado (tardarás mucho en acabar v.1)
> Un monolito te permite explorar complejidad de sistemas e ir midiendo las necesidades de cada parte, por ejemplo la tienda online necesita escalar, pues mantienes tu monolito y esa parte de la tienda online la rompes fuera a un microservicio y haces solo con eso microservicio. Y así hasta llegar a una arquitectura de microservicios.
>
> Los microsevicios hay que mantenerlos, etc etc
---


**Vamos a crearnos un microservicios**

Hay multiples librerías para crear microservicios (aunque no hace falta ninguna librerías para crearte microservicios por ejemplo los que hemos hecho nosotros hasta ahora)

https://github.com/dashersw/cote

```sh
npm repo cote
```


Me creo una carpeta en ejemplos `ejemplo-microservicios`
Me creo un pequeño script que simula una aplicacion que va hacer un cambio de moneda, y después nos haremos un microservicio que se sedique hacer cambios de moneda; además veremos como tiene propio almacenamiento el microservicio.

```sh
cd ejemplo/ejemplo-microservicios
npm init -y 
npm install cote
```


```js
'use strict';

// esta app necesita un microservicio para hacer cambios de moneda

const { Requester } = require('cote');

const requester = new Requester({ name: 'app' });

const evento = {
  type: 'convertir-moneda',
  cantidad: 100,
  desde: 'USD',
  hacia: 'EUR',
};

setInterval(() => {
  requester.send(evento, resultado => {
    console.log(Date.now(), 'app obtiene resultado:', resultado);
  });
}, 1000);
```

```sh
➜  ejemplos git:(main) ✗ cd ejemplo-microservicios 
➜  ejemplo-microservicios git:(main) ✗ npx nodemon app.js
[nodemon] 3.0.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,cjs,json
[nodemon] starting `node app.js`

Hello! I'm app#37a339a6-5228-4ed7-ac6f-d3068398af01 
========================

app > service.online servicio de mnoneda#0c52a340-403f-459c-ac5f-b5cd0b858a47 on 8000
========================
```

Creo fichero `conversionService.js`

```js
'use strict';

// microservicio de conversión de moneda

const { Responder } = require('cote');

// almacén de datos
const tasas = {
  USD_EUR: 0.94,
  EUR_USD: 1.06,
};

// lógica del servicio

const responder = new Responder({ name: 'servicio de mnoneda' });
// este responder se encarga del type: 'convertir-moneda'
responder.on('convertir-moneda', (req, done) => {
  // miramos que hay por aquí
  console.log(req);
})
```

```sh
➜  ejemplo-microservicios git:(main) ✗ npx nodemon conversionService.js

[nodemon] 3.0.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,cjs,json
[nodemon] starting `node conversionService.js`

Hello! I'm servicio de mnoneda#0c52a340-403f-459c-ac5f-b5cd0b858a47 on 8000 
========================

{ type: 'convertir-moneda', cantidad: 100, desde: 'USD', hacia: 'EUR' }
servicio de mnoneda > service.online app#37a339a6-5228-4ed7-ac6f-d3068398af01
```

Fíjate que el mensaje es el mismo `servicio de mnoneda > service.online app#37a339a6-5228-4ed7-ac6f-d3068398af01`. 
Fíjate que ha imprimido req=`{ type: 'convertir-moneda', cantidad: 100, desde: 'USD', hacia: 'EUR' }`

Vamos hacer la conversión:

```js
'use strict';

// microservicio de conversión de moneda

const { Responder } = require('cote');

// almacén de datos
const tasas = {
  USD_EUR: 0.94,
  EUR_USD: 1.06,
};

// lógica del servicio

const responder = new Responder({ name: 'servicio de mnoneda' });

// app.js : type: 'convertir-moneda',
responder.on('convertir-moneda', (req, done) => {
  const { cantidad, desde, hacia } = req;

  console.log(Date.now(), 'servicio:', cantidad, desde, hacia);

  // calcular la tasa de cambio
  const tasa = tasas[`${desde}_${hacia}`];
  const resultado = cantidad * tasa;

  done(resultado);
})
```

Verás que se están comunicando entre ellos sin haber dicho nada. Tenemos en una terminal arracada ` npx nodemon conversionService.js` y en la otra `✗ npx mon app.js`

Nota que han hecho la conversión de 100 en la derecha y ha respondido a la izquierda que tiene como resultado 94. Le ha dicho, cambiamo 100$ a €, pues son 94€

![](nodeapp/public/assets/img/25readme.png)


Y se comunican si decirle cada uno donde está el otro [siempre que estén en la misma red local, no remota] no te has de preocupar de qué puerto ni nada, lo pones y funciona. Vamos hacer que esté enviando cambios de moneda cada segundo.

```js
// 'use strict';

// // esta app necesita un microservicio para hacer cambios de moneda

// const { Requester } = require('cote');

// const requester = new Requester({ name: 'app' });

// const evento = {
//   type: 'convertir-moneda',
//   cantidad: 100,
//   desde: 'USD',
//   hacia: 'EUR',
// };

setInterval(() => {
  requester.send(evento, resultado => {
    console.log(Date.now(), 'app obtiene resultado:', resultado);
  });
}, 1000); // cada segundo
```

Puedes adaptar con `setInterval` la velocidad de solicitudes de cambio de moneda. Si lo pines aun milisegundo, se puede porque básicamente hace una multiplicacion.

![](nodeapp/public/assets/img/26readme.png)

Si tu arrancas otro Service, las peticiones de la app se repartiran entre servidores. Además si los sevidores están parados y app está pidiendo, se acumulan en cola y cuando arranque alguno se responderían todos los pendientes por sistema de colas.

---
> [!IMPORTANT]  
> * El de la izquierda, la `app` : Fíjate que ahora la aplicación `app` está lanzando cada segundo una petición de cambio de moneda. Imagínate que lo esté lanzando a una nube de servicios y dice "oye quien sepa resolver un evento del tipo : `type: 'convertir-moneda',` que lo haga". No sabe quien lo va hacer, ni donde está, ni qué servidor está, ni si quiera si va a responder.
>  
> * El de la derecha que es un responder : que se ha suscrito a a ese tipo de evento `responder.on('convertir-moneda', (req, done) => {` hace porque sabe hacer la conversión de la moneda y devuelve el resultado y la palicación recibe el resultado del servicio.
---


Cote está muy bien para empezar con microservicios, pero fíjate que no ves cuántos mensajes hay en cola, no sabes que está pasando por detrás. Si estuvieras con 1000 peticiones por segundo y los responders no estuvieran dando abasto, tendrías que pensar un mecanismo para solucionar el tema y con `RabnitNQL` lo ves de una forma muy sencilla qué está pasando y donde has de mejorar.


En este momento en `nodeapp/models/Usuario.js` tenemos un método  que envía emails directamente con `NodeMeiler -> emailTransportConfigure();`

```js
// método para enviar emails al usuario
usuarioSchema.methods.sendEmail = async function(asunto, cuerpo) {
  // crear un transport
  const transport = await emailTransportConfigure();
```

hicmimos otro método donde implmementábamos eso con un microservicio comunicaondomos don RabbitMQ

```js
// método para pedir a otro servicio que envie un email (RabbitMQ)
usuarioSchema.methods.sendEmailRabbitMQ = async function(asunto, cuerpo) {
  ...
}
```

y ahora vamos acrear otro método con `cote`

```js
usuarioSchema.methods.sendEmailCote = async function(asunto, cuerpo) {
  ...
}
```


```sh
# me instalo `cote`
npm install cote
```

arranco la app y me da error porque de la conexión con RabbitMQ no lo tengo arrancado

```sh
# arranco la app
➜  nodeapp git:(main) npm run dev                 

> nodeapp@0.0.0 dev
> cross-env NODE_ENV=development DEBUG=nodeapp:* nodemon ./bin/www

[nodemon] 3.0.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,cjs,json
[nodemon] starting `node ./bin/www`
  nodeapp:server Listening on port 3000 +0ms
Conectado a MongoDB en cursonode
node:internal/process/promises:289
            triggerUncaughtException(err, true /* fromPromise */);
            ^

AggregateError
    at internalConnectMultiple (node:net:1114:18)
    at afterConnectMultiple (node:net:1667:5) {
  code: 'ECONNREFUSED',
  [errors]: [
    Error: connect ECONNREFUSED ::1:5672
        at createConnectionError (node:net:1634:14)
        at afterConnectMultiple (node:net:1664:40) {
      errno: -61,
      code: 'ECONNREFUSED',
      syscall: 'connect',
      address: '::1',
      port: 5672
    },
    Error: connect ECONNREFUSED 127.0.0.1:5672
        at createConnectionError (node:net:1634:14)
        at afterConnectMultiple (node:net:1664:40) {
      errno: -61,
      code: 'ECONNREFUSED',
      syscall: 'connect',
      address: '127.0.0.1',
      port: 5672
    }
  ]
}

Node.js v20.6.1
[nodemon] app crashed - waiting for file changes before starting...
```

arranco la app y me da error porque de la conexión con RabbitMQ no lo tengo arrancado.  
Voy a `.env`  teníamos esto 

```sh
# RABBITMQ_BROKER_URL=amqps://suuberqm:5RRPrm2BvZyYHXV8gxh4KmVvzAyexTwd@whale.rmq.cloudamqp.com/suuberqm
RABBITMQ_BROKER_URL=amqp://guest:guest@localhost:5672
```

y lo cambiamos a esto, anulamos RabbitMQ en localhost:5672 y lo añadimos en la nube cloudamqp que habíamos creado.

```sh
RABBITMQ_BROKER_URL=amqps://suuberqm:5RRPrm2BvZyYHXV8gxh4KmVvzAyexTwd@whale.rmq.cloudamqp.com/suuberqm
# RABBITMQ_BROKER_URL=amqp://guest:guest@localhost:5672
```

y arrancamos

```sh
➜  nodeapp git:(main) npm run dev

> nodeapp@0.0.0 dev
> cross-env NODE_ENV=development DEBUG=nodeapp:* nodemon ./bin/www

[nodemon] 3.0.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,cjs,json
[nodemon] starting `node ./bin/www`
  nodeapp:server Listening on port 3000 +0ms
Conectado a MongoDB en cursonode
```

Añadimos en `nodeapp/models/Usuario.js`  :

```js
const { Requester } = require('cote');

// instanciamos el Requester 
// esto hace que queda arrancado en la aplicacion
const requester = new Requester({ name: 'nodeapp-email' });


...


usuarioSchema.methods.sendEmailCote = async function(asunto, cuerpo) {
  // creo un evento
  const evento = {
    type: 'enviar-email',
    from: process.env.EMAIL_SERVICE_FROM,
    to: this.email,
    subject: asunto,
    html: cuerpo
  }
  // lanzamos la funcion que cote llamará cuando obtenga respuesta de la comunicacion
  return new Promise(resolve => requester.send(evento, resolve));
}
```

Nos vamos al controlador ``controllers/LoginController.js` 

```js
      // usuario.sendEmailRabbitMQ('Bienvenido', 'Bienvenido a NodeApp');
      const result = await usuario.sendEmailCote('Bienvenido', 'Bienvenido a NodeApp');
      console.log(result);
```

Pues la parte de nodeapp de pedir que alguien mande ese emial ya está lista. Ahora en la carpeta de `microervicios/emailSender` le cambiamos el nombre `microervicios/emailSenderRabbitMQ.js` y creamos el otro archivo `microervicios/emailSenderCote.js`   

actualizo en `package.json`

```json
  "scripts": {
    "start": "cross-env node NODE_ENV=production ./bin/www",
    "dev": "cross-env NODE_ENV=development DEBUG=nodeapp:* nodemon ./bin/www",
    "init-db": "node init-db.js",
    "email-sender-rabbitmq": "node ./micro-services/emailSenderRabbitMQ.js",
    "email-sender-cote": "node ./micro-services/emailSenderCote.js"
```
y actualizamos en `README.md`

```sh
## Start

#In production:

#Start the application
npm start


#Start email sender service
npm run email-sender-rabbitmq
# or
npm run email-sender-cote


#In development:
npm run dev

```

vamos a implementar el `microervicios/emailSenderCote.js`

```js
'use strict';


const { Responder } = require('cote');
const nodemailer = require('nodemailer');

main().catch(err => console.log('Hubo un error', err));

async function main() {
  try {

    const transport = await createTransport();
    const responder = new Responder({ name: 'servicio de email' });
    
    // 'enviar-email' lo tienes en `Usuario.js/sendEmailCote : type: 'enviar-email',`
    // cuando ocura un evento llamado 'enviar-email'
    responder.on('enviar-email', async (req, done) => {
      try {
        // lo tienes en `Usuario.js/sendEmailCote
        const { from, to, subject, html } = req;
        // lo mandamos con el transport
        const result = await transport.sendMail({ from, to, subject, html });
        console.log(`Email enviado. URL: ${nodemailer.getTestMessageUrl(result)}`);
        // le pasamos lo que queremos que responda
        done(result);
      } catch (error) {
        console.log(error)
        done({ message: err.message });
      }
    })

  } catch (error) {
    console.log('error')
    done({ message: err.message });
  }

}

// la tenías en `microervicios/emailSenderRabbitMQ.js`
async function createTransport() {
  // entorno desarrollo
  const testAccount = await nodemailer.createTestAccount();

  const developmetTransport = {
    host: testAccount.smtp.host, //'smtp.ethereal.email',
    port: testAccount.smtp.port,
    secure: testAccount.smtp.secure,
    auth: {
        user: testAccount.user,
        pass: testAccount.pass
    }
  }

  return nodemailer.createTransport(developmetTransport);
}
```

```sh
npx nodemon ./micro-services/emailSenderCote.js
```

![](nodeapp/public/assets/img/27readme.png)

Ya comienzan a comunicarse.
Nodeapp ve que es un servicio email

```sh
nodeapp-email > service.online servicio de email#8f8f312e-ac79-4e45-8dd3-8ce0a94bc9dc on 8000
```

y el microservicio ve que está en la aplicacion 

```sh
servicio de email > service.online nodeapp-email#a14b1525-ec21-47c9-9369-736635a42536
```

Vamos a probarla abriendo la sesion desde login y deberías ver como 


![](nodeapp/public/assets/img/28readme.png)


## PM2

PM2 es esencialmente un gestor de procesos robusto y una herramienta de monitoreo que ayuda a mantener las aplicaciones en línea y monitorear su rendimiento, siendo especialmente valioso en entornos de producción. Su uso no se limita a Node.js; puede ser adaptado para gestionar cualquier tipo de proceso, lo que lo hace versátil para diferentes tecnologías y escenarios. 24/7

https://pm2.keymetrics.io/docs/usage/quick-start/

```sh
# modo local
npm install pm2@latest -g

# sin necesidad de instalarlo
npx pm2 -v

# que arranque unestra aplicacion 
npx pm2 start app.js

# arranca microservicio
npx pm2 start ./micro-services/emailSenderCote.js

# para ver los procesos que están corriendo
npx pm2 monit
```

mira el slides o la página web porque hay muchos servicios.  
Incluso puedes crearte un fichero de configuración de arranque:  
https://pm2.keymetrics.io/docs/usage/application-declaration/

```sh
npx pm2 init
```

nos crea un firchero `ecosystem.config.js`

Definimos tres procesos que quiero que arranque 
* le digo por dende tiene que arrancar `script: './bin/www',`
* que arranque miroservicios : ` script: './micro-services/emailSenderCote.js',`
* que vigile si ocurre algo: `watch: ['./micro-services/emailSenderCote.js'],`
* las variables de entorno para cuando arranque en produccion: `NODE_ENV: 'production`,
* `pm2 start process.json --env production`
* las variables de entorno para cuando arranque en desarrollo: `NODE_ENV: 'development`,
* `pm2 restart process.json --env development`
* Que te arraqnue cinco veces para que tega mayor copacidad: `instances: 5` (aunque cierres la terminal coorren en 2 plano)


> [!IMPORTAN] 
> Estudia esto video 6.mp4 1:30"
> Tienes que dominar esto.

`deploy : {`es uno de los mejores inventos
* servidor de la aplicacion : `host : 'nodeapp.com',`
* la rama que se publica normalmente : ref  : 'origin/main',
* en que repositorio está mi aplicacion : `repo : 'https://github.com/KeepCodingWeb15/backend-nodejs-mongodb',`
* la ruta del servidor donde quiero que se despliqgue : `path : '/home/nodeapp/app',`
* `pm2 deploy production setup` cuando arrancas por primera vez
* `pm2 deploy production` hace ssh al servidor coje la version anterior que tu tuviers publicada y la deja en una carpeta aparte par que puedas hacer un rollback muy rápido y despues hace un gitpull cojiendo la nueva version del repositorio del git y reinicia tu aplicacion. Todo del forma automatica.

```json
module.exports = {
  apps : [{
    name: 'nodeapp',
    script: './bin/www',
    watch: '.',
    env_production: {
      NODE_ENV: 'production',
    },
    env_development: {
      NODE_ENV: 'development'
    },
    log_date_format: 'YYYY-MM-DD HH:mm'
  }, {
    script: './micro-services/emailSenderCote.js',
    watch: ['./micro-services/emailSenderCote.js'],
    instances: 5
  }, {
    script: './micro-services/emailSenderRabbitMQ.js',
    watch: ['./micro-services/emailSenderRabbitMQ.js']
  }
],

  deploy : {
    production : {
      user : 'javi',
      host : 'nodeapp.com',
      ref  : 'origin/main',
      repo : 'https://github.com/KeepCodingWeb15/backend-nodejs-mongodb',
      path : '/home/nodeapp/app',
      'pre-deploy-local': 'npm run build',
      'post-deploy' : 'npm install && pm2 reload ecosystem.config.js --env production',
      'pre-setup': ''
    }
  }
};
```

Arranco :

```sh
npx pm2 start ecosystem.config.js
```

> [!NOTE]
>
> PM2 tiene https://pm2.io/ que para una aplicación es gratuita. Te mide las métricas de rendimiento de tu app está genial y recomendada al 100% te monitores los errores, el tiempo hasta que se recupera, etc
> EN PRODUCCION SE UTILIZA MUCHO PM2 para verlo todo más fácil, PM" es la antesala de Docker. Si no quieres meterte en Doker pero tener una buena herramienta de control de procesos PM2 es lo ideal.


## ejemplo práctica

Vamos hacer el upload de las imagenes de la aplicacion inicial. Recuerda que

> [!NOTE]  
> 
> Partimos de que ya se hizo una pequeña aplicacion (https://github.com/alexjust-data/App-Nodepop-backend) y que ya teníamos en https://github.com/alexjust-data/FullStack_Node_mongoDB la teoría y pática de la primera parte, ahora hemos iniciado el primer commit

Ahora en `models/Agente.js` Haremos que cuando se crea un Agente le podamos poner el avatar `avatar: String,`.

```js
// definir el esquema de los agentes
const agenteSchema = mongoose.Schema({
  name: { type: String, index: true },
  age: { type: Number, index: true, min: 18, max: 120 },
  owner: { ref: 'Usuario', type: mongoose.Schema.Types.ObjectId },
  avatar: String, // en la práctica sería la imagen del anuncio
}, {
  // collection: 'agentes' // para forzar un nombre concreto de colección
});
```

Quiero que cuando me hagan la petición a la API `ROUTES/api/agentes.js` quiero que me puedan subor un fichero y yo guarde en la base de datos la referencia de ese fichero y ese fichero tenga que ir a una carpeta de uploads que es la imagen del avatar de ese usuario.

```js
// POST /api/agentes
// Crea un agente
```

Para recibir un `loads` lo primero que tenemos que vigilar es en `app.js` que podamos recibirlos. Ahora mismo estamos usando

```js
// middlewares
// app.use(logger('dev'));
// app.use(express.json());
app.use(express.urlencoded({ extended: false })); // parea el body en formato urlencoded
// app.use(cookieParser());
// app.use(express.static(path.join(__dirname, 'public')));
```

y no lo cambiaremos, pero los uploads utilizan otra codificacion, `multipart/form-data`

---

Si ahora desde `Postman` te creas un agente, si te da error es porque no tienes el token


![](nodeapp/public/assets/img/29readme.png)


Create un token haciedo un post de acceso a la api. Luego copia y pega la cabecera este token


![](nodeapp/public/assets/img/30readme.png)


Cuando hayas puesto autorizacion : token, envía el post y te creará un agente nuevo


![](nodeapp/public/assets/img/31readme.png)


Puedes ver desde NoSQLBooster que se ha creado el agente nuevo_1


![](nodeapp/public/assets/img/32readme.png)


Ahora vamos hacer un upload del avatar. Voy al body de POstman del mismo POST que hacemos y selecciono formdata y creo agente2 con su file de avatar. Ahora si envías SEND creará un agente pero lo creará sin datos porque has enviado el body en otro formato `form-data` que no espera la app por lo cual no hay nada.


![](nodeapp/public/assets/img/33readme.png)


Entonces, vamos aprender a gestionar el body´s con formato `form-data`. Para ello usaremos una librería que se llama `multer`


> [!IMPORTANTE]
> PARA escojer una librería u otra :
> 1: miro cuantas estrellas tiene en github
> 2: miro en `npm  multer` cuantas descargas semanales tiene, porque puede que las estrellas sean por antaño.
> 3: miro las fechas de los commits para ver si tiene mantenimiento o actividad.
> El numero de `isues` de git para ver si el autor corrije los bugs



```sh
# 
npm repo multer

# instalar
npm install multer
```

Me hago un modulo de configuracion para los uploats por si quiero configurar en varios sitios tenerlo accesible. `lib/uploadConfigure.js`

```js
const multer = require('multer');

// declaro una configuración de upload
const upload = multer({ dest: 'uploads/' });

module.exports = upload;
```

Ahora vamos a nuestro `routes/api/agentes.js` añado ` upload.single('avatar') `

teníamos

```js
// POST /api/agentes
// Crea un agente
router.post('/', async (req, res, next) => {
  try {
    const agenteData = req.body;

    // creamos una instancia de agente en memoria
    const agente = new Agente(agenteData);

    // la persistimos en la BD
    const agenteGuardado = await agente.save();

    res.json({ result: agenteGuardado });

  } catch (err) {
    next(err);
  }
});
```

ahora 


```js
// const express = require('express');
// const router = express.Router();
// const Agente = require('../../models/Agente');
const upload = require('../../lib/uploadConfigure');

...
// POST /api/agentes
// Crea un agente
router.post('/', upload.single('avatar') , async (req, res, next) => {
  // try {
  //   const agenteData = req.body;
    console.log(req.file);

  //   // creamos una instancia de agente en memoria
  //   const agente = new Agente(agenteData);

  //   // la persistimos en la BD
  //   const agenteGuardado = await agente.save();

  //   res.json({ result: agenteGuardado });

  // } catch (err) {
  //   next(err);
  // }
});

```

La carpeta `nodeapp/upload` la tienes que crear tu a mano. Pero en este caso ya la teníamos creada

```sh
npm run dev
```

Hasta ahora podemos ver que en `agentes.js` este log:

```js
// POST /api/agentes
// Crea un agente
router.post('/', upload.single('avatar') , async (req, res, next) => {
  try {
    const agenteData = req.body;

    console.log(req.file);
```

por consola nos imprime este objeto

```sh
POST /api/agentes 200 45.098 ms - 78
{
  fieldname: 'avatar',
  originalname: 'Esquema_SQL.jpeg',
  encoding: '7bit',
  mimetype: 'image/jpeg',
  destination: 'uploads/',
  filename: '6b27e20fccbfd8a1297f7a44a417bbcd',
  path: 'uploads/6b27e20fccbfd8a1297f7a44a417bbcd',
  size: 208753
}
POST /api/agentes 200 59.919 ms - 78
```

Además hemos creado una propiedad `avatar` en el modelo `agente.js` ; pued lo que haremos será que despues de crear un nuevo agente 

```js
// POST /api/agentes
// Crea un agente
// router.post('/', upload.single('avatar') , async (req, res, next) => {
//   try {
//     const agenteData = req.body;
//     const usuarioIdLogado = req.usuarioLogadoAPI;

//     console.log(req.file);

    // creamos una instancia de agente en memoria
    const agente = new Agente(agenteData);
    agente.avatar = req.file.filename; 
    agente.owner = usuarioIdLogado; // le dices quién lo ha creado, qué usuario

//     // la persistimos en la BD
//     const agenteGuardado = await agente.save();

//     res.json({ result: agenteGuardado });

//   } catch (err) {
//     next(err);
//   }
// });
```

En el Midelwere que nos da este `upload.single('avatar')` la configuracion de este upload que está en `lib/uploadConfigure.js` en vez de poner directamente el destino (lo tienes en el apartado de Storage de https://github.com/expressjs/multer "The disk storage engine gives you full control on storing files to disk." )


```js
const multer = require('multer');
const path = require('node:path');

// declaro una configuración de almacenamiento
const storage = multer.diskStorage({

  // objeto de opciones
  destination: function(req, file, callback) {
    // ¿donde quiero que deje las fotos de los avatares?
    // me creo una carpeta "avatares" manualmente y le digo que lo guade allí
    const ruta = path.join(__dirname, '..', 'public', 'avatares');
    callback(null, ruta);
  },

  // objeto de opciones
  filename: function(req, file, callback) {
    // fieldname: 'avatar',
    // Date.now() --> para diferenciar si te suben el mismo archivo varias veces
    // originalname: 'Esquema_SQL.jpeg',
    const filename = `${file.fieldname}-${Date.now()}-${file.originalname}`;
    callback(null, filename);
  }
})

// declaro una configuración de upload
// antes lo teníasmos en ({ dest: 'uploads/' });
const upload = multer({ storage });

module.exports = upload;
```

Fíjate que `file` será el mismo objeto que nos aparece por la terminal 

```sh
{
  fieldname: 'avatar',
  originalname: 'Esquema_SQL.jpeg',
  encoding: '7bit',
  mimetype: 'image/jpeg',
  destination: 'uploads/',
  filename: '6b27e20fccbfd8a1297f7a44a417bbcd',
  path: 'uploads/6b27e20fccbfd8a1297f7a44a417bbcd',
  size: 208753
}
```

me creo una carpeta "avatares" manualmente en `public/avatares`.
Veta a Postman y crea el agente "NUevo_3". Ya no te sirve la carpeta `uploads` puedes elimnarla, ahora se crearán en `public/avatares` tendrás le fichero ``${file.fieldname}-${Date.now()}-${file.originalname}`;`


![](nodeapp/public/assets/img/35readme.png)


Si otro uasuario crea el mismo archivo no perderás la imgane anterior porque hemos puesto `Date.now()` enla creación del archivo. Si te vas a POstman y haces un GET solicitando los agentes te pide el token lo creas o le das el anterior, pero verás que el que hemos creado tiene avatar.


```sh
{
    "results": [
        {
            "_id": "657c862970776a3eba495f55",
            "name": "Smith",
            "age": 33,
            "owner": "657c862970776a3eba495f4f",
            "__v": 0
        },
        {
            "_id": "657c9cbd5c692b557a6a26ca",
            "name": "AlexJust",
            "age": 18,
            "owner": "657c862970776a3eba495f4f",
            "__v": 0
        },
        {
            "_id": "65914c4a93f85840e418512c",
            "name": "nuevo3",
            "age": 24,
            "avatar": "avatar-1704021066315-Esquema_SQL.jpeg",
            "owner": "657c862970776a3eba495f4f",
            "__v": 0
        }
    ]
}
```


> [!NOTE]
> No queremos publicar en git la carpeta de Avatares
> .gitignore : nodeapp/public/avatares


## astro js

https://docs.astro.build/en/install/auto/

Astro a diferencia con Express 

> EXPRESS
> Express es un framework web rápido, poco dogmático y minimalista para Node.js. Proporciona herramientas y características esenciales para construir aplicaciones web y APIs de manera eficiente. Express simplifica el desarrollo de aplicaciones en el lado del servidor con Node.js al manejar las tareas comunes relacionadas con las solicitudes y respuestas HTTP, y ofrece una amplia gama de middleware para extender su funcionalidad.


> ASTRO (NO TANTO PARA APIS, para webs con contenido)
> Astro es un moderno framework para desarrollo web diseñado para la creación de sitios web y aplicaciones impulsadas por contenido. Se caracteriza por su capacidad para generar sitios web extremadamente rápidos, combinando la eficiencia de la generación de sitios estáticos con la flexibilidad de los frameworks de JavaScript modernos. Astro permite a los desarrolladores escribir menos código de JavaScript cliente-side, resultando en páginas más ligeras y rápidas. También es versátil, pudiendo utilizarse para una amplia gama de aplicaciones web, desde sitios web estáticos hasta aplicaciones web del lado del cliente y puntos de acceso API dinámicos. Su enfoque está en la entrega de un rendimiento óptimo y una excelente experiencia de usuario.
> * Cuando decimos para web con contenido, por ejemplo un blog, o curriculum, o tienda onine con mucho productos o iimgenes, el website de la empresa,
> * ¿cuales no gestionan contenido? un api, un dashboard (mejor express)

PrerequisitesSection titled Prerequisites https://docs.astro.build/en/install/auto/
* Node.js - v18.14.1 or higher.
* Text editor - We recommend VS Code with our Official Astro extension.
* Terminal - Astro is accessed through its command-line interface (CLI).


```sh
➜  FullStack_FrontendPro git:(main) pwd
/Users/alex/Desktop/KEEPKODING/08_BackendAvanzado/FullStack_FrontendPro


➜  FullStack_FrontendPro git:(main) npm create astro@latest
Need to install the following packages:
create-astro@4.6.0
Ok to proceed? (y) y

 astro   Launch sequence initiated.

   dir   Where should we create your new project?
         ./adorable-aurora
➜  FullStack_FrontendPro git:(main) cd ejemplos 
➜  ejemplos git:(main) pwd
/Users/alex/Desktop/KEEPKODING/08_BackendAvanzado/FullStack_FrontendPro/ejemplos
➜  ejemplos git:(main) npm create astro@latest

 astro   Launch sequence initiated.

   dir   Where should we create your new project?
         ./ejemplo-astro

  tmpl   How would you like to start your new project?
         Include sample files
 ██████  Template copying...

  deps   Install dependencies?
         Yes
 ██████  Installing dependencies with npm...

    ts   Do you plan to write TypeScript?
         Yes

   use   How strict should TypeScript be?
         Relaxed
 ██████  TypeScript customizing...

   git   Initialize a new git repository?
         No
      ◼  Sounds good!
         You can always run git init manually.

  next   Liftoff confirmed. Explore your project!

 Enter your project directory using cd ./ejemplo-astro 
 Run npm run dev to start the dev server. CTRL+C to stop.
 Add frameworks like react or tailwind using astro add.

 Stuck? Join us at https://astro.build/chat

╭──🎁─╮  Houston:
│ ◠ ◡ ◠  Good luck out there, astronaut! 🚀
╰─────╯
➜  ejemplos git:(main) ✗ 
```

```sh

➜  ejemplos git:(main) ✗ cd ejemplo-astro 
➜  ejemplo-astro git:(main) ✗ npm run dev     

> ejemplo-astro@0.0.1 dev
> astro dev

▶ Astro collects anonymous usage data.
  This information helps us improve Astro.
  Run "astro telemetry disable" to opt-out.
  https://astro.build/telemetry


 astro  v4.0.8 ready in 201 ms

┃ Local    http://localhost:4321/
┃ Network  use --host to expose

13:08:46 watching for file changes...
```

Puedes ver que ha creado los json y demás

nos ha creado la app en http://localhost:4321/

Si no le decimos nada esta web está funcionando con SSG (Static Site Generator https://jamstack.org/generators/ ) Lo que hace es que por defecto genera todas las páginas del website, todas las páginas del website en una carpeta y publica autimáticmaente, no son dinamicas, genera todos los html de todo el wepsite, y los pone en la carpeta publica . Esto es lo que hace que este framework en velocidad de saque un 30% más que otras como Next, Gatsby, worpress etc lo puedes ver en la portada https://astro.build/

Al no ser dinámico, si queremos añadirle contenidos de la base de datos y la BBDD tendremos que regenerar todo el web site y volver a publicar. Si quisieramos añadirle BBDD tendríasmo que modificarle este SSG.

Crea en `src/pages/pagina2.astro`

```html
---
import Layout from '../layouts/Layout.astro';
---
<Layout title="Pagina 2">
<p>Esta es la página 2</p>
<a href="/">Volver a la home</a>
</Layout>

<style>
  html {
    background-color: white;;
  }
</style>
```

http://localhost:4321/pagina2 ya está funcionando

Si te fijas estos guinoes de la parte de arriba, es el controladro, la parte que se ejecuta en el servidor

```js
---
import Layout from '../layouts/Layout.astro';
import Card from '../components/Card.astro';
---
```

en Express como nodeapp sería similar a `routes/index` lo que pone aquí /* GET home page. */ sería similar a los que hay dentro de los guiones.

```js
/* GET home page. */
```

## tipos de renderizado : Web Rendering: SSR, CSR, SSG, e ISR


https://fajarwz.com/blog/web-rendering-what-is-ssr-csr-ssg-and-isr/


**SSR (Server-Side Rendering) : EXPRESS**

- **Descripción:** El contenido de la página se genera en el servidor en cada solicitud.
- **Ventajas:** SEO mejorado, carga inicial rápida.
- **Desventajas:** Mayor carga en el servidor, tiempos de espera en cada solicitud.
- **Uso:** `res.render('index', { title: 'My Page' });`

**CSR (Client-Side Rendering) : REACT**

- **Descripción:** El navegador carga un shell HTML y el contenido se genera mediante JavaScript.
- **Ventajas:** Interactividad del usuario, menos carga del servidor.
- **Desventajas:** SEO pobre, carga inicial lenta.
- **Uso:** Aplicaciones SPA como React (`ReactDOM.render(<App />, document.getElementById('root'));`).

**SSG (Static Site Generation) : ASTRO**

- **Descripción:** Las páginas se generan en tiempo de compilación y se sirven como archivos estáticos.
- **Ventajas:** Rápido, seguro, SEO mejorado.
- **Desventajas:** No es ideal para contenido dinámico.
- **Uso:** Generadores de sitios estáticos como Jekyll, Next.js (`next export`).

**ISR (Incremental Static Regeneration)**

- **Descripción:** Genera páginas estáticas de forma incremental a petición del usuario.
- **Ventajas:** Combina ventajas de SSG con contenido siempre actualizado.
- **Desventajas:** Complejidad en la implementación.
- **Uso:** Disponible en Next.js (`revalidate` en `getStaticProps`).


SI yo quiero cambiar todo el proyecto de ASTRO a `SSR (Server-Side Rendering) : EXPRESS` en la wep de astro tiene como hacerlo:

https://docs.astro.build/en/guides/server-side-rendering/

como voy a instalarlo donde haya un servidor nodo : https://docs.astro.build/en/guides/integrations-guide/node/

```sh
npx astro add node
```

```sh
➜  ejemplo-astro git:(main) ✗ npx astro add node
✔ Resolving packages...
15:52:33 
  Astro will run the following command:
  If you skip this step, you can always run it yourself later

 ╭───────────────────────────────────╮
 │ npm install @astrojs/node@^7.0.3  │
 ╰───────────────────────────────────╯

✔ Continue? … yes
⠼ Installing dependencies...
✔ Installing dependencies...
15:52:37 
  Astro will make the following changes to your config file:

 ╭ astro.config.mjs ─────────────────────────────╮
 │ import { defineConfig } from 'astro/config';  │
 │ import tailwind from "@astrojs/tailwind";     │
 │                                               │
 │ import node from "@astrojs/node";             │
 │                                               │
 │ // https://astro.build/config                 │
 │ export default defineConfig({                 │
 │   integrations: [tailwind()],                 │
 │   output: "server",                           │
 │   adapter: node({                             │
 │     mode: "standalone"                        │
 │   })                                          │
 │ });                                           │
 ╰───────────────────────────────────────────────╯

15:52:37   For complete deployment options, visit
  https://docs.astro.build/en/guides/deploy/

✔ Continue? … yes
15:52:38   
   success  Added the following integration to your project:
  - @astrojs/node
```


Ará exactamente lo mismo pero renderiza de otra forma


**Añadir integrations : frameworks de fromdEnd**

https://docs.astro.build/en/guides/integrations-guide/


vamos añadir taiwind https://docs.astro.build/en/guides/integrations-guide/tailwind/

```sh
➜  ejemplo-astro git:(main) npx astro add tailwind
✔ Resolving packages...
15:48:55 
  Astro will run the following command:
  If you skip this step, you can always run it yourself later

 ╭──────────────────────────────────────────────────────────╮
 │ npm install @astrojs/tailwind@^5.1.0 tailwindcss@^3.4.0  │
 ╰──────────────────────────────────────────────────────────╯

✔ Continue? … yes
⠙ Installing dependencies...
✔ Installing dependencies...
15:49:03 
  Astro will generate a minimal ./tailwind.config.mjs file.

✔ Continue? … yes
15:49:08 
  Astro will make the following changes to your config file:

 ╭ astro.config.mjs ─────────────────────────────╮
 │ import { defineConfig } from 'astro/config';  │
 │                                               │
 │ import tailwind from "@astrojs/tailwind";     │
 │                                               │
 │ // https://astro.build/config                 │
 │ export default defineConfig({                 │
 │   integrations: [tailwind()]                  │
 │ });                                           │
 ╰───────────────────────────────────────────────╯

✔ Continue? … yes
15:49:12   
   success  Added the following integration to your project:
  - @astrojs/tailwind
```

```sh
npm run dev
```

A cambiado un poco el fronten, puedes trabajarlo desde la página 

```html
<style>
  html {
    background-color: white;;
  }
  a {
    color: lightblue;
    text-decoration: underline;;
  }
</style>
```


**UI Frameworks**

https://docs.astro.build/en/guides/integrations-guide/

con REACT https://docs.astro.build/en/guides/integrations-guide/react/


```sh
➜  ejemplo-astro git:(main) ✗ npx astro add react
✔ Resolving packages...
15:54:39 
  Astro will run the following command:
  If you skip this step, you can always run it yourself later

 ╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
 │ npm install @astrojs/react@^3.0.9 @types/react@^18.2.46 @types/react-dom@^18.2.18 react@^18.2.0 react-dom@^18.2.0  │
 ╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯

✔ Continue? … yes
✔ Installing dependencies...
15:54:45 
  Astro will make the following changes to your config file:

 ╭ astro.config.mjs ─────────────────────────────╮
 │ import { defineConfig } from 'astro/config';  │
 │ import tailwind from "@astrojs/tailwind";     │
 │ import node from "@astrojs/node";             │
 │                                               │
 │ import react from "@astrojs/react";           │
 │                                               │
 │ // https://astro.build/config                 │
 │ export default defineConfig({                 │
 │   integrations: [tailwind(), react()],        │
 │   output: "server",                           │
 │   adapter: node({                             │
 │     mode: "standalone"                        │
 │   })                                          │
 │ });                                           │
 ╰───────────────────────────────────────────────╯

✔ Continue? … yes
15:54:46   
   success  Added the following integration to your project:
  - @astrojs/react
15:54:46 
  Astro will make the following changes to your tsconfig.json:

 ╭ tsconfig.json ────────────────────────╮
 │ {                                     │
 │   "extends": "astro/tsconfigs/base",  │
 │   "compilerOptions": {                │
 │     "jsx": "react-jsx",               │
 │     "jsxImportSource": "react"        │
 │   }                                   │
 │ }                                     │
 ╰───────────────────────────────────────╯

✔ Continue? … yes
15:54:48   
   success  Successfully updated TypeScript settings
```

Astro tiene componente, pero son de servidor (no confundir con los compnenete tipo react que son de cliente),  
fíjate en el archivo `src/componentes/Card.astro` pues en esta carpeta me creo un nuevo fichero `src/componentes/Counter.tsx`


NOs cremos un contador

```tsx
import { useState } from 'react'

export default function App() {
  const [counter, setCounter] = useState(0);

  //increase counter
  const increase = () => {
    setCounter(count => count + 1);
  };

  //decrease counter
  const decrease = () => {
    setCounter(count => count - 1);
  };

  //reset counter
  const reset = () =>{
    setCounter(0)
  }

  return (
    <div className="counter">
      <h1>React Counter</h1>
      <span className="counter__output">{counter}</span>
      <div className="btn__container">
        <button className="control__btn" onClick={increase}>+</button>
        <button className="control__btn" onClick={decrease}>-</button>
        <button className="reset" onClick={reset}>Reset</button>
      </div>
    </div>
  );
}
```

En archivo `src/pages/pagina2.astro` añadimos `import Counter from '../components/Counter';`

```html
---
import Layout from '../layouts/Layout.astro';
import Counter from '../components/Counter';
---
<Layout title="Pagina 2">
<p>Esta es la página 2</p>

<Counter client:visible></Counter>
```

**Podemos crear un api**


https://docs.astro.build/en/core-concepts/endpoints/#_top

En `src/pages/api/agentes/index.ts`

copia y pega en contenido de dco de astro 

```ts
import type { APIRoute } from 'astro';

export const GET: APIRoute = ({ params, request }) => {
    return new Response(JSON.stringify({
        message: "This was a GET!"
      })
    )
  }
```

sólo xon esto ya tienes un api:  `http://localhost:4321/api/agentes` : `{"message":"This was a GET!"}`




