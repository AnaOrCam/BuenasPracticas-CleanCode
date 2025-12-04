# Concurrencia
Realizado por _Jose Manuel Vilchez Arenas_

## ¿Qué es la concurrencia?
La concurrencia es una estrategia que nos ayuda a separar lo que se hace
en nuestro código del cuando se hace. En aplicaciones de un solo hilo, el qué y cuando están
juntos.

En definitiva, se busca ejecutar varias tareas en paralelo entre sí para aprovechar mejor los 
recursos computacionales. Los encargados de proporcionar concurrencia son las  llamadas 
peticiones asíncronas, que permiten separar la ejecución de nuestro programa en varios hilos.

### Ventajas
* Mejora del rendimiento de nuestra aplicación
* Mejora la estructura de todo el programa

### Desventajas
* Difícil diseño e implementación
* Mayor complejidad en el código

---

## Callbacks
Son funciones que se pasan como parámetros a otras funciones para ser ejecutadas una vez esta última termina, 
es decir, la función principal solo se completará una vez que se cumpla la función que es pasada como parámetro.

**Mala implementación ❌**
> Tarea síncrona
```javascript
const tarea= () => console.log('Tarea completada');
tarea()
````

> Tarea asíncrona
```javascript
const tarea = (callback) =>{
    console.log('Iniciando tarea...');
    //El callback se ejecutará tras 2 segundos
    setTimeout(() => callback(), 2000);
}

tarea()
````

## Promesas
Una buena práctica en nuestro código es la implementación de promesas en lugar de los callbacks que hemos
visto anteriormente. Una promesa es un objeto que representa la finalización (o el fracaso) de una operación asíncrona 
y su valor resultante. Trabajamos con ella mostrando unos resultados u otros en función del retorno.

> Creamos la promesa 🧐
```javascript
const hacerTarea= () => {
    return new Promise((resolve, reject)=>{
        const num = 1 + Math.floor(Math.random() * 6)
        if (num == 6){
            resolve(num);
        } 
        reject(num);
    })
}
```

> Consumimos la promesa 🧐
```javascript
doTask()
        .then((num) => console.log('Promesa cumplida: ', num))
        .catch((num) => console.log('No se ha cumplido la promesa: ', num))
```

## Async/await
Ya hemos visto en el ejemplo anterior como conseguir concurrencia en nuestro código mediante promesas.
Podemos mejorar aún mas nuestro código mediante la implementación de _async/await_

Definimos funciones asíncronas mediante _async_, estas siempre van a devolver una promesa. Es una función 
que maneja una serie de operaciones que tardan un tiempo en resolverse.
Dentro de estas funciones, usamos _await_, que pausa la ejecución de nuestro programa hasta que se cumpla
dicha promesa y tengamos el valor de retorno _resolve_


**Buena implementación ✔**

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

procesarSolicitud()
```

