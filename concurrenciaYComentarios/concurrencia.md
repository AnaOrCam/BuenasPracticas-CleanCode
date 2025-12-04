# Concurrencia
> Jose Manuel Vilchez Arenas

## ¿Qué es la concurrencia?
La concurrencia es una estrategia de desacoplamiento que nos ayuda a separar lo que se hace
en nuestro código del cuando se hace. En aplicaciones de un solo hilo, el qué y cuando están
juntos.

En definitiva, se busca ejecutar varias tareas en paralelo entre sí para aprovechar mejor los 
recursos computacionales. Los encargados de proporcionar concurrencia son los hilos.

### Ventajas
* Mejora del rendimiento de nuestra aplicación
* Mejora la estructura de todo el programa
* Código mas limpio

### Desventajas
* Difícil diseño e implementación
* Hace que nuestro código sea mas complejo

## Llamadas asíncronas
En javascript, logramos concurrencia en nuestro código mediante la implementación de async/await, que
nos permite realizar peticiones asíncronas. Se trata de código que no bloquea el hilo principal de ejecución 
de nuestro programa.

Definimos funciones asíncronas mediante "async", que siempre van a devolver una promesa. Es una función que 
maneja una serie de operaciones que tardan un tiempo en resolverse.
Dentro de estas funciones, usamos "await", que pausa la ejecución de nuestro programa hasta que se cumpla
dicha promesa y tengamos el valor de retorno (resolve)

Async/await es importante en aquellas capas de nuestro programa destinadas al acceso a la base de datos.

**Petición síncrona ❌**

````javascript
function obtenerCodigoUsuario() {
  return "Codigo del usuario: 101";
}

function procesarSolicitud() {
  console.log("Iniciando la solicitud...");

  try {
    let codigo = obtenerCodigoUsuario();
    console.log("Solicitud completada. Codigo: " + codigo);
  } catch (error) {
    console.error("No se pudo completar la solicitud", error);
  }
````

**Petición asíncrona ✔**

```javascript
function obtenerCodigoUsuario() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve("Codigo del usuario: 101");
    }, 2000);
  });
}
```

```javascript
async function procesarSolicitud() {
  console.log("Iniciando la solicitud...");

  try {
    let codigo = await obtenerCodigoUsuario();
    console.log("Solicitud completada. Codigo:" + codigo);
  } catch (error) {
    console.error("No se pudo completar la solicitud", error);
  }
}
```

