## 2. FUNCIONES

Las **funciones** son una estructura fundamental en el código de cualquier aplicación. Son los ladrillos básicos que permiten armar y organizar la arquitectura del software. 

Aplicar en ellas prácticas de **Código Limpio** garantiza la legibilidad, la modularidad y la eficiencia del programa que estemos creando.

A continuación veremos, con ejemplos en JavaScript, algunas de las principales medidas que podemos tomar para escribir y refactorizar funciones como auténticos **desarrolladores seniors**.

### 2.1. El tamaño importa

La primera regla fundamental es: **cuanto más pequeñas sean las funciones mejor.**

Escribir un ladrillo enorme con 200 líneas de código no va a ayudar a nadie a la hora de entender qué hace tu función. No quieres que tu cerebro explote intentando recordar las 50 variables que contiene. Si necesitas hacer scroll para ver el final de tu función, es la señal de que toca refactorizar.

Una función debe ser tan pequeña que su propósito sea obvio de un vistazo. Las funciones más cortas se leen como pequeñas historias que se relacionan entre sí mediante llamadas.

#### ❌ La chapuza (código espagueti)
```js
// Este código intenta validar un formulario, calcular cosas y manipular el DOM todo junto.
function validarYEnviarFormulario() {
    let nombre = document.getElementById("nombre").value;
    let edad = document.getElementById("edad").value;
    let error = document.getElementById("error-msg");

    if (nombre === "") {
        error.innerHTML = "¡Pon el nombre!";
        error.style.display = "block";
    } else {
        if (edad < 18) {
            error.innerHTML = "Eres menor, fuera de aquí.";
            error.style.display = "block";
        } else {
            // Aquí simula que enviamos datos...
            console.log("Enviando...");
            // Y encima manipula el DOM otra vez
            document.getElementById("formulario").style.display = "none";
            document.getElementById("exito").style.display = "block";
            alert("¡Todo guay!");
        }
    }
}
```

#### ✅ La solución (Clean Code)
```js
// Dividimos y venceremos. Fíjate que ahora parece que estás leyendo instrucciones en inglés (o español).
function procesarRegistro() {
    if (datosSonInvalidos()) {
        mostrarError();
        return;
    }

    enviarDatosAlServidor();
    mostrarMensajeExito();
}

function datosSonInvalidos() {
    let nombre = document.getElementById("nombre").value;
    let edad = document.getElementById("edad").value;
    return nombre === "" || edad < 18;
}

function mostrarError() {
    let error = document.getElementById("error-msg");
    error.innerHTML = "Revisa los datos, anda.";
    error.style.display = "block";
}

function enviarDatosAlServidor() {
    console.log("Enviando datos...");
}

function mostrarMensajeExito() {
    document.getElementById("formulario").style.display = "none";
    document.getElementById("exito").style.display = "block";
}
```

### 2.2. El síndrome de la función orquesta (y cómo curarlo)

El mantra sagrado a recordar es: **cada función solo debe hacer una tarea, debe hacerla bien y que sea lo único que haga.** Queremos que toquen su instrumento, no que dirijan toda la orquesta.

Si una función valida el usuario, busca en Google Maps y además pinta un div de color rojo, está mal. ¿Por qué? Porque si mañana cambia la API de Google Maps, se te rompe la validación del usuario. Si cambia el color del div, puedes romper la búsqueda.

Al principio de los años 2000 el informático y escritor Robert C. Martin concibió una serie de principios fundamentales que sentaban las bases del diseño de un código mantenible y ampliable: los principios **SOLID**.

Esta pauta de crear funciones que solo cumplan una tarea responde al objetivo del primer principio SOLID y uno de los más importantes: el **Principio de Responsabilidad Única (SRP)**. Este dice que una función, clase o módulo solo puede tener un motivo para cambiar.

#### Trucos para saber si hace más de una cosa:

- Si el cuerpo de la función puede dividirse en secciones, hace más de una cosa.

    ```js
    function procesarPedidoCompleto(pedido) {
        // SECCIÓN 1: Validaciones
        if (pedido.items.length === 0) {
            console.log("Error: Carrito vacío");
            return;
        }
        if (!pedido.usuario.tieneSaldo) {
            console.log("Error: Sin saldo");
            return;
        }

        // SECCIÓN 2: Cálculos
        let total = 0;
        for (let item of pedido.items) {
            total += item.precio;
        }
        let impuestos = total * 0.21;
        let totalConImpuestos = total + impuestos;

        // SECCIÓN 3: Guardado y Notificación
        console.log("Guardando en base de datos...");
        console.log("Enviando email a: " + pedido.usuario.email);
    }
    ```
- Si se puede extraer otra función con un nombre que sea una **reformulación de la implementación** (se llame de forma equivalente a la función que la contiene), hace demasiadas cosas.

    ```js
    function gestionarSesionUsuario(usuario) {
        conectarConBaseDeDatos();
        verificarCredenciales(usuario);
        crearCookieDeSesion();
        redirigirAlHome();
    }
    ```
    ¿Podrías extraer el contenido de dentro en una nueva función? ¿Cómo la llamarías?
    
    ```js
    // Nombre redundante
    function hacerTodoLoDeLaSesion(usuario) { 
        conectarConBaseDeDatos();
        verificarCredenciales(usuario);
        // ...
    }
    ```

#### ❌ Mal hecho (Acoplamiento)

```js
function crearUsuario(nombre, email) {
    
    // Validar (Cosa 1)
    if (email.indexOf("@") === -1) {
        alert("Email falso");
        return;
    }
    
    // Crear objeto (Cosa 2)
    let usuario = { n: nombre, e: email };

    // Guardar en lista (Cosa 3)
    listaUsuarios.push(usuario);
}
```

#### ✅ Bien hecho (Separación de responsabilidades)

```js
// Cada función cumple un único cometido
function registrarUsuario(nombre, email) {
    if (esEmailInvalido(email)) {
        return; 
    }
    guardarUsuario(nombre, email);
}

function esEmailInvalido(email) {
    if (email.indexOf("@") === -1) {
        alert("Email falso");
        return true;
    }
    return false;
}

function guardarUsuario(nombre, email) {
    let usuario = { n: nombre, e: email };
    listaUsuarios.push(usuario);
}
```

### 2.3. El arquitecto no pone ladrillos

Una función puede encargarse de tareas que, por ejemplo, impliquen utilizar la lógica de negocio como *"calcular un cambio de divisa"* o *"finalizar una compra"*. Esto sería un **alto nivel de abstracción.**

Una función puede, de la misma manera, trabajar sobre detalles de implementación como *"manipular el DOM mediante* `document.getElementById`". Esto sería un **bajo nivel de abstracción.**

Lo que esta norma nos indica es que mezclar conceptos de alto nivel y de bajo nivel dentro de una misma función es una receta para el desastre. Es como si el arquitecto se pusiera a poner ladrillos mientras diseña los planos.

Tener sentencias que realizan tareas con diferentes niveles de abstracción supone un mareo continuo para quien lea tu código: o hablas de cómo funciona la empresa, o hablas de cómo funciona el navegador, pero no cambies de tema en cada línea.

#### El código es un periódico: la regla descendente

Para evitar este entuerto se recomienda aplicar la **Regla Descendente**. Queremos que el código se lea como un periódico: los titulares arriba y los detalles abajo. Así pues, las funciones deben declararse de arriba hacia abajo desde el mayor nivel de abstracción hasta el menor.

#### ❌ Mezclando niveles (HTML sucio y lógica)

```js
function mostrarListaProductos(productos) {
    let total = 0;

    // Detalle técnico de bajo nivel (HTML string) mezclado con lógica
    let html = "<ul>";
    
    for (let producto of productos) {
        // Lógica de negocio (Cálculo)
        total += producto.precio;
        // Más detalle técnico
        html += "<li>" + producto.nombre + "</li>";
    }
    
    html += "</ul>";

    // Manipulación directa del DOM
    document.body.innerHTML += html;
    document.body.innerHTML += "<p>Total: " + total + "</p>";
}
```

#### ✅ Niveles separados (regla descendente)

```js
function mostrarCatalogo(productos) {
    let precioTotal = calcularTotal(productos);
    let listaHTML = generarHTMLLista(productos);
    
    pintarEnPantalla(listaHTML, precioTotal);
}

// Nivel alto: gestiona la lógica de negocio
function calcularTotal(productos) {
    let total = 0;
    for (let p of productos) {
        total += p.precio;
    }
    return total;
}

// Nivel medio: Nos preocupamos de las etiquetas HTML y el bucle
function generarHTMLLista(productos) {
    
    let htmlGenerado = "<ul>";
    
    for (let producto of productos) {
        htmlGenerado += "<li>" + producto.nombre + " - " + producto.precio + "€</li>";
    }
    
    htmlGenerado += "</ul>";
    return htmlGenerado;
}

// Nivel bajo: manipulamos el DOM
function pintarEnPantalla(lista, total) {
    let contenedor = document.getElementById("app");
    contenedor.innerHTML = lista + "<p>Total: " + total + "</p>";
}
```

### 2.4. La trampa mortal de los switch

Las sentencias `switch` son peligrosas, al igual que las cadenas de `if/else`. Son piezas de código inevitablemente largas y farragosas que acostumbran a pegarle una patada al Principio de Responsabilidad Única mediante sus múltiples `case`.

También violan otro principio SOLID fundamental con más facilidad de la esperada: el **principio de abierto/cerrado (OCP)**. Según este principio, una función o una clase bien diseñada, debe permitir su ampliación cuando se desee sin que se necesite modificar su contenido.

Para ponerle solución se recomienda limitar el uso de los `switch` al mínimo imprescindible y, si se puede, hacer uso del polimorfismo. 

En JavaScript, en lugar de usar clases complejas (polimorfismo clásico), podemos usar objetos como si fueran diccionarios. Es rápido y mucho más limpio.

#### ❌ El Switch eterno

```js
function obtenerSonidoAnimal(animal) {
    switch (animal) {
        case "perro":
            return "Guau";
        case "gato":
            return "Miau";
        case "pato":
            return "Cuack";
        // Si añadimos "vaca", hay que tocar este archivo y esta función. Peligro.
        default:
            return "Sonido desconocido";
    }
}
```
#### ✅ La alternativa JS (Diccionario / Mapa)

```js
// Configuración separada de la lógica
const sonidosAnimales = {
    "perro": "Guau",
    "gato": "Miau",
    "pato": "Cuack",
    "vaca": "Muuu" // Añadir uno nuevo es facilísimo
};

function obtenerSonido(animal) {
    // Buscamos en el objeto, si no existe devolvemos el default (||)
    return sonidosAnimales[animal] || "Sonido desconocido";
}
```

### 2.5. El arte del bautizo

Bautizar correctamente a tus funciones es un arte en sí mismo. El nombre es la carta de presentación de una función. Si necesitas entrar a mirar el código para saber qué hace la función, su nombre ha fracasado.

Los nombres, ante todo, deben ser autoexplicativos. Es preferible **un nombre largo pero descriptivo que uno corto y misterioso**. Y muchísimo mejor que tener que recurrir a comentarios para describir la función. Olvídate de abreviaturas que solo tú conoces (`calc`, `proc`, `hndlr`). El código se lee muchas más veces de las que se escribe, hazlo accesible para humanos, no para máquinas.

Es aconsejable y, prácticamente una convención, que los nombres de las funciones sean **verbos**. Ante todo, es imprescindible mantener una nomenclatura consistente: si usas **get** para obtener datos, no escribas funciones con **retrieve** u **obtain** en otras parte del código.

#### ❌ Nombres crípticos

```js
// ¿Qué es 'd'? ¿Días? ¿Dedos? ¿Datos?
// ¿Qué hace 'proc'? ¿Procesar? ¿Procrear?
function proc(d) { 
  return d * 86400; // ¿Qué es este número mágico?
}

function a(n, e) { // a... ¿de añadir?
    alert("Hola " + n);
}
```

#### ✅ Nombres que cuentan historias

```js
function convertirDiasASegundos(dias) {
  const SEGUNDOS_EN_UN_DIA = 86400;
  return dias * SEGUNDOS_EN_UN_DIA;
}

function saludarUsuario(nombre) { 
    alert("Hola " + nombre);
}
```