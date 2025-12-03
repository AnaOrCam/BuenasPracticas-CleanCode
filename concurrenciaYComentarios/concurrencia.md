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

## Concurrencia asíncrona
El propósito de async/await es permitirnos escribir código asíncrono, es decir, que no bloquea
el hilo principal de ejecución de nuestro programa.

En javascript definimos funciones como asíncronas mediante "async", funciones que siempre
van a devolver una promesa. Es una función que maneja unas operaciones que toman tiempo.

Dentro de estas funciones, usamos "await", que pausa la ejecución hasta que se cumpla
dicha promesa y tengamos el valor devuelto.

Usamos async/await en aquellas capas de nuestro programa destinadas al acceso a la base de datos.

**Implementación**

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

