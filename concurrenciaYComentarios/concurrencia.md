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
function getPosts(idUser) {
  return new Promise((resolve, reject) => {
    const peticion = new XMLHttpRequest();
    peticion.open('GET', SERVER + '/posts?userId=' + idUser);
    peticion.send();
    peticion.addEventListener('load', () => {
      if (peticion.status === 200) {
        resolve(JSON.parse(peticion.responseText));
      } else {
        reject("Error " + peticion.status + " (" + peticion.statusText + ") en la petición");
      }
    })
    peticion.addEventListener('error', () => reject('Error en la petición HTTP'));
  })
}
```

```javascript
let idUser = document.getElementById('id-usuario').value;
    if (isNaN(idUser) || idUser == '') {
      alert('Debes introducir un número');
    } else {
      const posts = await getPosts(idUser);
    }
```

