# Preparando nuestra API para producción 

### Glosario:

# Production (Producción)

"Poner software en producción" o "subir a producción", son términos que se utilizan para referirnos a poner una API o app de manera disponible para los usuarios, lista para usarse.

# Environments / ambientes 

Hasta ahora hemos desarrollado en un ambiente de 'development' (evidentemente), pero las configuraciones de una aplicación son diferentes según sea el ambiente donde se esté ejecutando.

En cualquier organización con productos de software se tienen dos ambientes; development y production. También es común encontrar tres; development, producción y staging ó testing. Donde el tercero se refiere a un ambiente intermedio donde distintos desarrolladores pueden hacer pruebas de integración con otros componentes y partes del sistema. El ambiente de staging es muy útil para trabajar en equipo y probar nuevas funcionalidades antes de hacerlas disponibles para todos los usuarios.

# Deployment

En pocas palabras, el término deployment se refiere a todos los conceptos y actividades que se relacionan con el lanzamiento de un proyecto de software disponible para los usuarios.

Hacer deploy es de cierta forma publicar nuestra aplicación, o en nuestro caso, nuestra API Rest en algunos servicios cloud (nube). Pero el deployment puede abarcar mucho más que únicamente poner disponible a todo el mundo una aplicación, ya que dependiendo los requerimientos de un proyecto, es posible que sea necesario hacer un lanzamiento en una red privada o con ciertas restricciones.

El deployment implica preparar todos los sistemas necesarios para poder estar seguros de que nuestro sistema será estable cuando sea utilizado por nuestros usuarios.

# Ambientes de desarrollo

El manejo de ambientes de desarrollo nos permite establecer diferentes configuraciones cuando estamos trabajando con nuestro código. Por ejemplo, para 'development' podríamos establecer una base de datos de pruebas, ya sea de manera local o en un servidor. Ésta podría con recursos más limitados y la consistencia de sus datos no tendría que ser tan relevante como en la de base de datos de producción, la cual debe estar preparada para recibir más carga de trabajo y tener alta disponibilidad.

También es común que tengamos variables que van a cambiar según las circunstancias y la plataforma donde nuestro código sea ejecutado.

Si queremos compartir nuestro código por medio de un repositorio público, es importante tener cuidado con los datos sensibles a los cuales no deseamos que otras personas tengan acceso.

Una de las maneras más simples para almacenar información sin tenerla escrita directamente en el código es por medio de variables de entorno ó variables de ambiente.

Para crear una variable de entorno podemos utilizar la siguiente sintaxis directamente en la terminal de nuestro sistema `*-NIX` (linux – unix):

```bash
export NOMBRE_VARIABLE=valor
```

Una vez guardada podrás utilizarla así

```bash
echo $NOMBRE_VARIABLE
```

y para acceder a ellas con node.js utilizamos el siguiente código:

```node
process.env.NOMBRE_VARIABLE
```

# Variables de entorno para hacer el deploy

En primer lugar configuramos una variable para colocar la cadena de conexión a la BD llamada `MONGODB_URI`. Así que debemos modificar el archivo `app.js`.

```node
mongoose.connect(process.env.MONGODB_URI)
```

Debemos también definir otra variable de entorno para saber si el API está en producción o no. Le llamaremos `NODE_ENV`. Y la utilizamos así:

```node
var isProduction = process.env.NODE_ENV === 'production';
```

Tomando en cuenta el valor de la variable `isProduction`, se habilitará o no mensajes debug en la consola de nuestra API:

```node
 mongoose.set("debug", true);
```

Finalmente declaramos la variable `PORT` para que al llamar al método app.listen se utilice:

```node
var server = app.listen(process.env.PORT || 3000, function () {
console.log('App corriendo en el puerto ' + server.address().port); });
```

# Deploy de un API a Heroku

Heroku es una plataforma que nos permite saltarnos muchos pasos de configuración de arquitectura y lanzar una aplicación en sencillos pasos.

Aunque no es lo óptimo para proyectos grandes y con necesidades muy específicas, nos permite probar aplicaciones de manera rápida y sin dolores de cabeza, y también acercarnos a comprender muchos conceptos de deploy de manera rápida, aunque no estemos conscientes de todas las implicaciones que conlleva por detrás cada configuración.

Aunque no es lo óptimo para proyectos grandes y con necesidades muy específicas, nos permite probar aplicaciones de manera rápida y sin dolores de cabeza, y también acercarnos a comprender muchos conceptos de deploy de manera rápida, aunque no estemos conscientes de todas las implicaciones que conlleva por detrás cada configuración.

1. Entra al sitio oficial de Heroku: `https://www.heroku.com/`
2. Crea tu cuenta en Heroku eligiendo `Node.js` como tu `Primary development language`.
3. Una vez que hayas accedido verás una pantalla como la siguiente.

<img width="930" alt="Screen Shot 2022-09-11 at 12 55 06 a m" src="https://user-images.githubusercontent.com/24994818/189514530-e63f8b69-172f-4e32-9505-ffaee58c4606.png">

Da click sobre el botón "Create a new app".

4. Elige un nombre y crea tu app.

<img width="837" alt="Screen Shot 2022-09-11 at 12 56 28 a m" src="https://user-images.githubusercontent.com/24994818/189514561-15e659b4-3ab1-4979-936a-885d8fcd6815.png">

5. Ahora en la pestaña 'settings' configura las variables de entorno para producción. Da click en el botón "Reveal Config Vars"

<img width="911" alt="Screen Shot 2022-09-11 at 12 58 08 a m" src="https://user-images.githubusercontent.com/24994818/189514597-6ab844db-8d84-4796-8ba3-c4f42b8cd60c.png">

### Es importante que la variable PORT tenga el valor 8080.

<img width="914" alt="Screen Shot 2022-09-11 at 12 58 51 a m" src="https://user-images.githubusercontent.com/24994818/189514614-d0be5efb-3854-4c28-8adf-1f181b39e1a2.png">

En el método de deployment elige Github y conecta tu cuenta de Github.

<img width="900" alt="Screen Shot 2022-09-11 at 12 59 26 a m" src="https://user-images.githubusercontent.com/24994818/189514622-a1cad8f0-9934-47af-bb99-3eaa98ee8c93.png">

1. Busca tu repositorio y da click en conectar.

<img width="869" alt="Screen Shot 2022-09-11 at 1 00 10 a m" src="https://user-images.githubusercontent.com/24994818/189514636-66d7a726-c2f9-42c6-9546-e2cb1c30f809.png">

2. Una vez conectado ve a la sección "Manual Deploy" y presiona el botón "Deploy branch"

<img width="857" alt="Screen Shot 2022-09-11 at 1 01 19 a m" src="https://user-images.githubusercontent.com/24994818/189514670-fb8426e8-331d-4b1d-b1dc-1849e0548f66.png">

3. Una vez hecho lo anterior, presiona el botón "Open app" ubicado en el lado superior derecho de la interfaz de Heroku.

Esto abrirá la url de tu API, ahora verifica que esté funcionando dirigiendote al path /

Si todo está bien configurado, verás el mensaje de inicio de tu API.

# things to remember

`https://dashboard.heroku.com/apps/neptune-nodejs/deploy/github`
