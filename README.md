# ⚜ Código limpio - 2ºDAW ⚜
>Grupo 1: José Miguel e Izán (Variables y Formato)

>Grupo 2: José Luis, David Galán y Alexis (Funciones)

>Grupo 3: Ana y David Rosa (Objetos, Clases y Testing)

>Grupo 4: Andrés y José Manuel (Concurrencia y Comentarios)

---

## Tabla de contenidos
* [1. Variables y formato](#variables-y-formato)
* [2. Funciones (parte 1)](#2-funciones)
* [3. Funciones (parte 2)](#argumentos-de-funciones)
* [4. Funciones (parte 3)](#mejor-excepciones-que-devolver-códigos-de-error)
* [5. Objetos](#objetos-y-estructuras-de-datos)
* [6. Clases](#clases)
* [7. Testing](#testing)
* [8. Concurrencia](#concurrencia)
* [9. Comentarios](#comentarios)

# Variables y Formato
**Autores:** José Miguel e Izan (Grupo 1)

En esta sección exploramos cómo nombrar variables correctamente y cómo dar formato al código para que sea legible, basándonos en los principios de *Clean Code* adaptados a JavaScript.

---

## 1. Nombres con Intención Reveladora

El nombre de una variable debe responder: **¿Por qué existe? ¿Qué hace? ¿Cómo se usa?** Si necesitas un comentario para explicarlo, el nombre no es bueno.

**❌ Mal:**
```javascript
let d; // días transcurridos
let temp;
```

**✅ Bien:**
```javascript
const daysSinceCreation = 10;
const currentTemperature = 25;
```

**Principio universal:** Aplica a todos los lenguajes. Un nombre claro ahorra tiempo y reduce errores.

---

## 2. Nombres Pronunciables y Buscables

Usa palabras completas para poder leer el código en voz alta y encontrarlo con CTRL+F.

**❌ Mal:**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
const usrCnt = 42;
```

**✅ Bien:**
```javascript
const currentDate = moment().format('YYYY/MM/DD');
const userCount = 42;
```

---

## 3. Evita Números Mágicos

Sustituye valores numéricos sin contexto por constantes con nombres explicativos.

**❌ Mal:**
```javascript
// ¿Qué significa 86400000?
setTimeout(restart, 86400000);
const total = price * 1.21; // ¿Por qué 1.21?
```

**✅ Bien:**
```javascript
const MILLISECONDS_IN_A_DAY = 86400000;
setTimeout(restart, MILLISECONDS_IN_A_DAY);

const IVA_SPAIN = 0.21;
const total = price * (1 + IVA_SPAIN);
```

**Principio universal:** Las constantes con nombre son mejores que números literales en cualquier lenguaje.

---

## 4. Vocabulario Consistente

Usa el mismo término para el mismo concepto. No mezcles sinónimos.

**❌ Mal:**
```javascript
getUserInfo();
getClientData();
fetchCustomerRecord();
```

**✅ Bien:**
```javascript
getUser();
getProduct();
getOrder();
```

---

## 5. Evita Contexto Innecesario

No repitas información que ya proporciona el objeto o clase.

**❌ Mal:**
```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};
```

**✅ Bien:**
```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};
```

---

## 6. Declaración de Variables (JavaScript)

**Reglas modernas:**
- **`const`:** Por defecto. No permite reasignación.
- **`let`:** Solo si la variable cambiará.
- **`var`:** **NUNCA**. Tiene problemas de scope.

**❌ Mal:**
```javascript
var name = "José";
var name = "Izan"; // JS permite redeclarar (peligroso)
```

**✅ Bien:**
```javascript
const name = "José";
let age = 25;
age = 26; // Correcto con let
```

---

## 7. Formato Vertical

El código debe leerse de arriba a abajo como un periódico:
- Variables cerca de donde se usan
- Líneas en blanco entre bloques lógicos
- Funciones ordenadas por nivel de abstracción

**❌ Mal:**
```javascript
function processOrder(order){
const tax=order.total*0.21;const shipping=calculateShipping(order);const total=order.total+tax+shipping;return total;}
```

**✅ Bien:**
```javascript
function processOrder(order) {
  const tax = order.total * TAX_RATE;
  const shipping = calculateShipping(order);
  
  const total = order.total + tax + shipping;
  return total;
}
```

---

## 8. Capitalización Consistente

**Convenciones JS:**
- `camelCase` → variables y funciones: `userName`, `calculateTotal()`
- `PascalCase` → clases: `UserAccount`, `ShoppingCart`
- `UPPER_SNAKE_CASE` → constantes: `MAX_USERS`, `API_KEY`

**❌ Mal:**
```javascript
const DAYS_in_week = 7;
function Calculate_total() {}
class animal {}
```

**✅ Bien:**
```javascript
const DAYS_IN_WEEK = 7;
function calculateTotal() {}
class Animal {}
```

---

## 9. Funciones que llaman y funciones que son llamadas, deberían estar cerca

Mantén las funciones que llaman y las que son llamadas próximas en el código. Agrupa funciones relacionadas, evita dispersarlas en diferentes partes del archivo y organiza su orden de manera lógica para que la lectura sea natural y coherente. Usa módulos si el grupo de funciones crece demasiado.

**❌ Mal:**
```javascript
class Calculator {
    add(a, b) {
        return a + b;
    }

    divide(a, b) {
        if (b === 0) return 'Error';
        return a / b;
    }

    calculate(operation, a, b) {
        switch (operation) {
            case 'add':
                return this.add(a, b);
            case 'subtract':
                return this.subtract(a, b);
            case 'divide':
                return this.divide(a, b);
            default:
                return 'Operation not supported';
        }
    }

    subtract(a, b) {
        return a - b;
    }

    multiply(a, b) {
        return a * b;
    }
}

```

**✅ Bien:**
```javascript
class Calculator {
    add(a, b) {
        return a + b;
    }

    subtract(a, b) {
        return a - b;
    }

    multiply(a, b) {
        return a * b;
    }

    divide(a, b) {
        if (b === 0) return 'Error';
        return a / b;
    }

    calculate(operation, a, b) {
        switch (operation) {
            case 'add':
                return this.add(a, b);
            case 'subtract':
                return this.subtract(a, b);
            case 'multiply':
                return this.multiply(a, b);
            case 'divide':
                return this.divide(a, b);
            default:
                return 'Operation not supported';
        }
    }
}

```

---

## Resumen: Principios Universales

Estos conceptos aplican a **cualquier lenguaje**:

1. ✅ Nombres descriptivos > abreviaturas
2. ✅ Constantes con nombre > números mágicos
3. ✅ Consistencia en vocabulario y formato
4. ✅ Evita redundancia
5. ✅ Formato limpio y organizado

---

## Checklist Rápido

- [ ] ¿Variables con nombres descriptivos?
- [ ] ¿Números mágicos extraídos a constantes?
- [ ] ¿Usé `const` por defecto?
- [ ] ¿Evité `var`?
- [ ] ¿Formato consistente?

---

## Preguntas de Refuerzo

### 1. ¿Qué está mal en este código?
```javascript
let x = 86400000;
setTimeout(reset, x);
```
<details>
<summary>Ver respuesta</summary>

**Problema:** `86400000` es un número mágico y `x` no es descriptivo.

**Solución:**
```javascript
const MILLISECONDS_IN_A_DAY = 86400000;
setTimeout(reset, MILLISECONDS_IN_A_DAY);
```
</details>

---

### 2. ¿Por qué deberíamos evitar `var`?
<details>
<summary>Ver respuesta</summary>

- Tiene scope de función, no de bloque
- Permite redeclaraciones accidentales
- Causa bugs difíciles de detectar
- `const` y `let` son más seguros y modernos
</details>

---

### 3. Mejora este código:
```javascript
const u = {
  uName: "María",
  uAge: 25,
  uEmail: "maria@example.com"
};
```
<details>
<summary>Ver respuesta</summary>

**Problema:** Contexto innecesario (repetir `u` en cada propiedad).

**Solución:**
```javascript
const user = {
  name: "María",
  age: 25,
  email: "maria@example.com"
};
```
</details>

---

### 4. ¿Qué diferencia hay entre estos dos ejemplos?
```javascript
// Ejemplo A
let count = 0;
count = 5;

// Ejemplo B
const count = 0;
count = 5;
```
<details>
<summary>Ver respuesta</summary>

**Ejemplo A:** Funciona correctamente (`let` permite reasignación).

**Ejemplo B:** Da error (`const` no permite reasignación).

**Regla:** Usa `const` por defecto, `let` solo si necesitas cambiar el valor.
</details>

---

### 5. ¿Cuál nombre es mejor y por qué?
```javascript
// Opción 1
const d = new Date();

// Opción 2
const currentDate = new Date();
```
<details>
<summary>Ver respuesta</summary>

**Mejor:** Opción 2 (`currentDate`)

**Razones:**
- Pronunciable y buscable
- Auto-explicativo (no necesita comentarios)
- Más fácil de entender al leer el código
</details>

# 2. Funciones

Las **funciones** son una estructura fundamental en el código de cualquier aplicación. Son los ladrillos básicos que permiten armar y organizar la arquitectura del software. 

Aplicar en ellas prácticas de **Código Limpio** garantiza la legibilidad, la modularidad y la eficiencia del programa que estemos creando.

A continuación veremos, con ejemplos en JavaScript, algunas de las principales medidas que podemos tomar para escribir y refactorizar funciones como auténticos **desarrolladores seniors**.

## 2.1. El tamaño importa

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

## 2.2. El síndrome de la función orquesta (y cómo curarlo)

El mantra sagrado a recordar es: **cada función solo debe hacer una tarea, debe hacerla bien y que sea lo único que haga.** Queremos que toquen su instrumento, no que dirijan toda la orquesta.

Si una función valida el usuario, busca en Google Maps y además pinta un div de color rojo, está mal. ¿Por qué? Porque si mañana cambia la API de Google Maps, se te rompe la validación del usuario. Si cambia el color del div, puedes romper la búsqueda.

Al principio de los años 2000 el informático y escritor Robert C. Martin concibió una serie de principios fundamentales que sentaban las bases del diseño de un código mantenible y ampliable: los principios **SOLID**.

Esta pauta de crear funciones que solo cumplan una tarea responde al objetivo del primer principio SOLID y uno de los más importantes: el **Principio de Responsabilidad Única (SRP)**. Este dice que una función, clase o módulo solo puede tener un motivo para cambiar.

### Trucos para saber si hace más de una cosa:

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

## 2.3. El arquitecto no pone ladrillos

Una función puede encargarse de tareas que, por ejemplo, impliquen utilizar la lógica de negocio como *"calcular un cambio de divisa"* o *"finalizar una compra"*. Esto sería un **alto nivel de abstracción.**

Una función puede, de la misma manera, trabajar sobre detalles de implementación como *"manipular el DOM mediante* `document.getElementById`". Esto sería un **bajo nivel de abstracción.**

Lo que esta norma nos indica es que mezclar conceptos de alto nivel y de bajo nivel dentro de una misma función es una receta para el desastre. Es como si el arquitecto se pusiera a poner ladrillos mientras diseña los planos.

Tener sentencias que realizan tareas con diferentes niveles de abstracción supone un mareo continuo para quien lea tu código: o hablas de cómo funciona la empresa, o hablas de cómo funciona el navegador, pero no cambies de tema en cada línea.

### El código es un periódico: la regla descendente

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

## 2.4. La trampa mortal de los switch

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

## 2.5. El arte del bautizo

Bautizar correctamente a tus funciones es un arte en sí mismo. El nombre es la carta de presentación de una función. Si necesitas entrar a mirar el código para saber qué hace la función, su nombre ha fracasado.

Los nombres, ante todo, deben ser autoexplicativos. Es preferible **un nombre largo pero descriptivo que uno corto y misterioso**. Y muchísimo mejor que tener que recurrir a comentarios para describir la función. Olvídate de abreviaturas que solo tú conoces (`calc`, `proc`, `hndlr`). El código se lee muchas más veces de las que se escribe, hazlo accesible para humanos, no para máquinas.

Es aconsejable y, prácticamente una convención, que los nombres de las funciones sean **verbos**. Ante todo, es imprescindible mantener una nomenclatura consistente: si usas **get** para obtener datos, no escribas funciones con **retrieve** u **obtain** en otras parte del código.

#### ❌ Nombres crípticos

```js
// ¿"Tratar"? ¿Qué es esto, una consulta médica?
// ¿"lista"? ¿La de la compra? ¿La de los Reyes Magos?
function tratarLista(lista) {
    for (let i = 0; i < lista.length; i++) {
        lista[i].precio = lista[i].precio * 1.21; // ¿Qué es este número mágico? ¿Por qué 1.21?
    }
}

// ¿"noEstaDesactivado"? 
// O sea, si devuelve 'true', es que NO está desactivado, es decir... está activado.
// Y si hago un if (!noEstaDesactivado)... ¡Socorro!
function noEstaDesactivado(usuario) {
    return usuario.estado !== 'INACTIVO';
}

// Si mañana cambias el Array por una Base de Datos
// te toca cambiar el nombre de la función en 50 sitios. ¡Mal!
function buscarEnArrayPorIndice(id) {
    return inventario.find(item => item.id === id);
}
```

#### ✅ Nombres que cuentan historias

```js
// Ah, amigo. Ahora sé exactamente qué va a pasar.
function aplicarIvaAProductos(productos) {
    for (let producto of productos) {
        producto.precio = producto.precio * 1.21;
    }
}

// Mucho más fácil. ¿Está activo? Sí o no.
// Siempre intenta buscar la versión positiva de la condición.
function estaActivo(usuario) {
    return usuario.estado !== 'INACTIVO';
}

// Me da igual si buscas en un array, en un Excel o en una servilleta.
// Yo quiero recuperar un producto. Punto.
function recuperarProductoPorIdentificador(id) {
    return inventario.find(item => item.id === id);
}
```

## Argumentos de Funciones
Es importante tener 2 o menos argumentos por función, cuando sea posible, pueden existir de más pero no es lo ideal. Al leer las funciones siempre nos será más fácil cuantos menos argumentos tengamos que pensar para qué se utilizan.

- 0 argumentos (Niládico)
- 1 argumento (Monádico)
- 2 argumentos (Diádico)
- 3 argumentos (Triádico)
- Más de 3 argumentos (Poliádico)

Los argumentos en la parte de pruebas generan más problemas aun, porque tendriamos que probar todas las combinaciones de estas, ahora sino tenemos ninguno es más sencillo.

Hay dos tipos de argumentos:
* Argumentos de Entrada

Es más fácil de entender y es como todos lo conocemos, es información añadida a la función.
* Argumentos de Salida

Son más complejas de entender, ya que solemos entender la función que extrae un valor devuelto, no en forma de argumentos, esto hace que haya más comprobaciones.

    GeneradorDeFacturas.renderizar(datosDelPedido); // datosDelPedido argumento de entrada GeneradorDeFacturas argumento de salida

### Formas Monádicas habituales
Hay dos motivos principales para pasar un solo argumento a una función.

- Realizar una pregunta Se hace una consulta sobre el argumento y devuelve un boolean.
- Transformar un argumento Procesa el argumento, lo transforma en otra cosa y lo devuelve.

Una forma menos habitual es el Evento. Hay argumento de entrada pero no de salida (no hay return). Se usa para alterar el estado del sistema. Hay que tener cuidado con los nombres para que quede claro que es un evento.

Debemos evitar usar argumentos de salida para realizar transformaciones. Si una función va a transformar su entrada, el objeto transformado debe ser el valor devuelto.

❌ Mal:

    // Es raro, no devuelve nada y modifica el argumento 'out' internamente.
    function transform(out) {
        out.push("nuevo valor");
    }

✅ Bien:

    // Entra algo y sale algo nuevo
    function transform(input) {
        return input + " transformado";
    }

    //Evento (Altera estado, sin retorno)
    function passwordAttemptFailed(attempts) {
        if (attempts > 3) lockSystem();
    }

### Argumentos de indicador
Usar como argumento un valor Booleano es muy mala práctica, ya que quiere decir que vamos a hacer más de una cosa dentro. Si es ```true``` hace una cosa y si es ```false``` otra diferente.

### Funciones diádicas
Una función con dos argumentos es más difícil de entender que una con uno solo. Aunque ```writeField(outputStream, name)``` parece fácil, nos obliga a hacer una pausa mental para ignorar el primer parámetro, lo que nos puede crear errores y no verlos.
Hay excepciones, como ```Point(0,0)```, donde los argumentos son componentes de un mismo valor.
Las funciones diádicas las vamos a tener que usar, pero si podemos convertirlas en unitarias mejor.

❌ Mal:

    // Tienes que entender la relación entre 'dbConnection' y 'usuario' y recordar el orden.
    function guardarUsuario(dbConnection, usuario) {
    dbConnection.insert("users", usuario);
}

✅ Bien:

    // Pasamos el argumento complejo en el constructor de la clase.
    const repo = new RepositorioUsuarios(miConexionSQL);
    repo.guardar(usuario);
   

### Triadas
Las funciones con 3 argumentos nos generan más problemas cuando tengamos que ordenar, ignorar o pararse a entenderlos.

❌ Mal:

    / Obliga a pausar la lectura para pensar que hace el 3 argumento.
    assertEquals(5, usuario.getIntentos(), 0);

✅ Bien:

    // Se comprende leyendo.
    assertThat(usuario.getIntentos()).isEqualTo(5);  

### Objeto de argumento
Cuando una función parece necesitar dos o más argumentos, es posible que alguno de ellos se tenga una clase propia.

❌ Mal:

    buscarReservas(Date fechaInicio, Date fechaFin);

✅ Bien:

    buscarReservas(DateRange rango);

### Listas de argumentos
En algunos momentos hay que pasar una cantidad variable de argumentos. Si los argumentos se procesan de la misma forma es un tipo List. Entonces una función que reciba String y List es diádica.

✅ Bien: Monádica

    function logMessages(List) {
        // ...
    }

✅ Bien: Diádica

    function format(template, List) {
        // ...
    }

❌ Mal: Poliádica

    function createReport(title, date, author, List) {
        // ...
    }


### Verbos y palabras clave
Es importante escoger un buen nombre, que sea descriptivo de su función y orden de argumentos.

- Funciones Monádicas

Se puede crear con nombre de la funcion (verbo) y argumento (sustantivo).

✅ Bien:

    // Se lee natural: "write name" (Escribir nombre).
    write(name);

- Superior a un argumento

Podemos mejorar la claridad escribiendo el nombre del argumento dentro del nombre de la función.

❌ Mal:

    // Es difícil saber cual se copia sobre otro
    copiar(archivoA, archivoB);

✅ Bien:

    // El nombre de la función dicta el orden.
    copiarDesdeHacia(origen, destino);

## Sin efectos secundario
Los efectos secundarios son las consecuencias de mentiras, ya que en muchos casos promete una cosa, pero tambien hace otras cosas ocultas. Alguna vez, realiza cambios inesperados en variables de la clase.

❌ Mal:

    let usuarioLogueado = false;
    // El nombre dice "verificar", pero la función cambia lo cambia a true.
    function verificarContrasena(password) {
        if (password === "secreto123") {
            usuarioLogueado = true; // Efecto secundario
            return true;
        }
        return false;
    }

✅ Bien:

    let usuarioLogueado = false;
    // El nombre describe de lo que va a pasar
    function verificarEIniciarSesion(password) {
        if (password === "secreto123") {
            usuarioLogueado = true;
            return true;
        }
        return false;
    }

### Argumentos de salida
Usar argumentos de salida hace que al revisar el código sea más dificil su comprensión, siempre que se pueda evitar su uso será mejor. Esto se usaba antes de la creación de la programación orientada a objetos.

❌ Mal:

    // Puede generar duda de lo que hace
    aplicarDaño(jugador, 50);

✅ Bien:

    // Se ve claro que 'jugador' es el objeto que se modifica a sí mismo.
    jugador.recibirDaño(50);

## Separación de consultas de comando
La función debe centrarse en hacer o responder, no ambas. Cambiar el estado o devolver información, no ambas. Esto si se hace junto crea confusión.

Cuando una función devuelve un valor y a la vez modifica datos, no se entiende si pregunta o ejecuta.

❌ Mal:

    // Es confuso porque no sabemos si quiere cambiarlo o validarlo
    if (set("nombre", "Pepe")) {
    }

Si separamos el set del if se hace más fácil la comprensión.

✅ Bien:

    // Primero preguntamos 
    if (existeAtributo("nombre")) {
        // Luego ejecutamos
        setAtributo("nombre", "Pepe");

    }
### Mejor excepciones que devolver códigos de error

Devolver códigos de error en funciones de comando es un sutil incumplimiento de la separación de comandos y consultas. Esto hace que los comandos usados asciendan a expresiones en los predicados de las instrucciones `if`, provocando estructuras anidadas que oscurecen la lógica real del programa.

Al devolver un código de error se crea un problema: el invocador debe procesar el error de forma inmediata. Si se usan excepciones en lugar de códigos de error, el código de procesamiento del error se puede separar del código de ruta feliz y se simplifica enormemente la lectura.

#### ❌ La pirámide de la condena (Códigos de error)

```javascript
function borrarPagina(pagina) {
    if (borrarPaginaDeBaseDeDatos(pagina) === "OK") {
        if (borrarReferencias(pagina) === "OK") {
            if (borrarCache(pagina) === "OK") {
                console.log("Página borrada");
            } else {
                console.log("Error borrando caché");
            }
        } else {
            console.log("Error borrando referencias");
        }
    } else {
        console.log("Error borrando página");
    }
}
```

#### ✅ Limpio y directo (Excepciones)

```javascript
function borrarPagina(pagina) {
    try {
        borrarPaginaYReferencias(pagina);
    } catch (error) {
        console.log(error.message);
    }
}

function borrarPaginaYReferencias(pagina) {
    borrarPaginaDeBaseDeDatos(pagina);
    borrarReferencias(pagina);
    borrarCache(pagina);
}
```

### Extraer bloques Try/Catch

Los bloques `try/catch` no son atractivos por naturaleza. Confunden la estructura del código y mezclan el procesamiento de errores con el procesamiento normal.

Por ello, conviene extraer el cuerpo de los bloques `try` y `catch` en funciones individuales. De este modo, la separación facilita la comprensión y la modificación del código.

Recordemos que **las funciones solo deben hacer una cosa**, y el procesamiento de errores es una cosa en sí misma. Por tanto, una función que procese errores no debe hacer nada más. Esto implica que, si una función incluye la palabra clave `try`, esta debería ser la primera palabra de la función y no debería haber nada más después de los bloques `catch/finally`.

#### ❌ Mezclando responsabilidades

```javascript
function procesarPago(usuario) {
    let total = calcularTotal(usuario);
    
    // Aquí empieza la mezcla fea de lógica y errores
    try {
        servidorPagos.cobrar(total);
        registrarTransaccion(usuario, total);
    } catch (e) {
        console.error("Falló el pago: " + e);
    }
    
    enviarEmailConfirmacion(usuario);
}
```

#### ✅ Separación de inquietudes

```javascript
function procesarPago(usuario) {
    try {
        ejecutarCobro(usuario);
    } catch (error) {
        console.error("Falló el pago: " + error);
    }
}

function ejecutarCobro(usuario) {
    let total = calcularTotal(usuario);
    servidorPagos.cobrar(total);
    registrarTransaccion(usuario, total);
    enviarEmailConfirmacion(usuario);
}
```

### No repetirse (DRY)

La duplicación es un problema grave, ya que aumenta el tamaño del código innecesariamente y requerirá una modificación múltiple si alguna vez cambia el algoritmo. También multiplica el riesgo de errores.

La duplicación puede ser la raíz de todos los problemas del software. Existen numerosos principios y prácticas para controlarla o eliminarla. La programación orientada a objetos concentra el código en clases base que en otros casos serían redundantes. La programación estructurada y orientada a componentes son, en parte, estrategias para eliminar duplicados.

#### ❌ Código repetitivo (Duplicación)

```javascript
function crearAdmin(nombre) {
    // Lógica repetida
    let usuario = new Usuario();
    usuario.nombre = nombre;
    usuario.fechaCreacion = new Date();
    usuario.rol = "ADMIN";
    guardarEnBD(usuario);
}

function crearInvitado(nombre) {
    // Lógica repetida otra vez
    let usuario = new Usuario();
    usuario.nombre = nombre;
    usuario.fechaCreacion = new Date();
    usuario.rol = "GUEST";
    guardarEnBD(usuario);
}
```

#### ✅ Código reutilizable (DRY)

```javascript
function crearAdmin(nombre) {
    crearUsuario(nombre, "ADMIN");
}

function crearInvitado(nombre) {
    crearUsuario(nombre, "GUEST");
}

function crearUsuario(nombre, rol) {
    let usuario = new Usuario();
    usuario.nombre = nombre;
    usuario.fechaCreacion = new Date();
    usuario.rol = rol;
    guardarEnBD(usuario);
}
```

### Programación estructurada

Edsger Dijkstra propuso reglas de programación estructurada que afirman que todas las funciones y bloques deben tener una entrada y una salida. Estas reglas implican que solo debe haber una instrucción `return` en una función, que no debe haber instrucciones `break` o `continue` en un bucle y nunca debe haber instrucciones `goto`.

Sin embargo, si sus funciones son de tamaño reducido (como deben ser en Código Limpio), una instrucción `return`, `break` o `continue` múltiple no hará daño alguno. De hecho, en ocasiones puede resultar mucho más expresiva que la regla rígida de una sola entrada y una salida, evitando anidamientos innecesarios.

#### ❌ Rígido (Una sola salida forzada)

```javascript
function esUsuarioValido(usuario) {
    let esValido = false;
    if (usuario) {
        if (usuario.edad >= 18) {
            if (usuario.tieneCuentaActiva) {
                esValido = true;
            }
        }
    }
    return esValido; // Un solo return, pero mucho anidamiento
}
```

#### ✅ Expresivo (Múltiples retornos)

```javascript
function esUsuarioValido(usuario) {
    if (!usuario) return false;
    if (usuario.edad < 18) return false;
    if (!usuario.tieneCuentaActiva) return false;

    return true; // Más fácil de leer en funciones pequeñas
}
```

### Cómo crear este tipo de funciones

La creación de software es como cualquier otro proceso creativo (como escribir un libro). Al escribir, primero se plasman las ideas y después se refina el mensaje hasta que se lea bien. El primer borrador puede estar desorganizado, así que se retoca y mejora.

Cuando se crean funciones, al principio suelen ser extensas y complicadas, con sangrados y bucles anidados. Pero también se cuenta (o se debería contar) con **pruebas de unidad**. Posteriormente, se retoca el código: se dividen las funciones, se cambian los nombres y se eliminan los duplicados.

Al final, se consiguen funciones que cumplen las reglas. No se escriben perfectas al comenzar y se duda que nadie pueda hacerlo.

### Conclusión

Todo sistema se crea a partir de un lenguaje específico del dominio diseñado por los programadores. Las funciones son los verbos del lenguaje y las clases los sustantivos.

Los programadores experimentados piensan en los sistemas como en **historias que contar**, no como en programas que escribir. La jerarquía de funciones describe las acciones que se pueden realizar en el sistema.

El verdadero objetivo es contar la historia del sistema y que las funciones que escriba encajen en un lenguaje claro y preciso que le sirva para contar esa historia.

# Objetos y estructuras de datos

## *Utilizar getters y setters:* 

Es mejor usar getters y setter que simplemente acceder a esa propiedad del objeto por las siguientes razones:
        
1. A la hora de modificarlo no tienes que ir mirando si la propiedad existe o no, el setter se encarga de la lógica interna.
1. Encapsula, es decir, en el código externo está oculto como estan organizados los datos internamente.
1. Se centraliza el acceso a la propiedad en un solo lugar, eso hace mas fácil el uso de validaciones, errores, conversiones... etc.
1. Permite el uso de lazy load.

```js
❌

    let usuario1={
        nombre: 'Ana',
        apellidos: 'Montes',
        datos:{
            direccion: null
        }
    };

    if (usuario1.datos && usuario1.datos.direccion){
        let dir1=usuario1.datos.direccion;
    }else{
        console.error('Usuario no tiene una calle definida');
    }

✅

    class Usuario{
        #nombre;
        #apellidos;
        #direccion;

        constructor(nombre, apellidos, dir){
            this.#nombre=nombre;
            this.#apellidos=apellidos;
            this.#direccion=dir;
        }

        get getDireccion(){
            return this.#direccion;
        }

        set setDireccion(dirNueva){
            this.#direccion=dirNueva;
        }
    }

    let usuario=new Usuario('Ana', 'Montes','');

    let dirNueva='calle Nueva, 12';
    usuario.setDireccion=dirNueva;

```

## *Utilizar atributos/métodos privados en los objetos:*

Mediante atributos privados en clases o *clousures* conseguimos:
    
1. Encapsulación: la lógica interna del objeto queda oculta.
1. Evitar errores externos: no se accede directamente a la modificación de atributos.
1.  Control de acceso a los atributos mediante métodos públicos o getters y setters.

```js

❌

    let usuario1={
        nombre: 'Ana',
        apellidos: 'Montes',
    };

    usuario1.nombre='Maria';

✅

    function usuario(nombre){
        let nombreUsuario=nombre;

        this.getNombre=function(){
            return nombreUsuario;
        }
        this.setNombre=function(nuevoNombre){
            nombreUsuario=nuevoNombre;
        }
    }

    let usuario1=new Usuario('Ana');
    usuario1.getNombre();
    usuario1.setNombre('Maria');

```

# Clases #
<p>Escribir código que sea fácil de entender y mantener es igual de importante que saber usar las herramientas del lenguaje.</p> 
<p>Las clases pueden ser útiles, pero también pueden complicar innecesariamente el código si se usan mal.</p>

#### Buena estructuracion del codigo ####

<p>Para que el codigo sea facil de mantener es necesario que este lleve un orden en la definicion de sus metodos y variables, junto con un buen uso de tabuladores</p>

**Mal ejemplo 🔴**

```javascript
class Usuario {
    saludar() {
        console.log(`Hola, mi nombre es ${this.nombre}`);
    }

  constructor(nombre, correo, edad) {
        this.nombre = nombre;
    this.correo = correo;
    this.edad = edad;
  }

    esAdulto() {
    return this.edad >= 18;
  }

  setCorreo(correo) {
    this.correo = correo;
    return this;
    }

  getNombre() {
        return this.nombre;
  }

  setNombre(nombre) {
    this.nombre = nombre;
    return this;
        }

  getCorreo() {
    return this.correo;
  }
    }

```
**Buen ejemplo 🟢**
```javascript
class Usuario {
  // 1. Definición de propiedades
  constructor(nombre, correo, edad) {
    this.nombre = nombre;
    this.correo = correo;
    this.edad = edad;
  }

  // 2. Métodos de acceso (getters / setters)
  setNombre(nombre) {
    this.nombre = nombre;
    return this;
  }

  getNombre() {
    return this.nombre;
  }

  setCorreo(correo) {
    this.correo = correo;
    return this;
  }

  getCorreo() {
    return this.correo;
  }

  // 3. Métodos principales
  saludar() {
    console.log(`Hola, mi nombre es ${this.nombre}`);
  }

  esAdulto() {
    return this.edad >= 18;
  }
}

```
#### Formato adecuado de definición de clases y metodos ####
<p>Prioriza las clases ES2015/ES6 respecto a las funciones planas ES5 , ya que son más legibles</p>

**Mal ejemplo 🔴**

```javascript
// Crea un constructor de funcion Animal
// Le asigna una edad para cada instancia
// Añade un metodo mover a todas las instacias de animal

const Animal = function(edad) {
  if (!(this instanceof Animal)) {
    throw new Error("Inicializa Animal con `new`");
  }
  this.edad = edad;
};

Animal.prototype.mover = function mover() {};

const Mamifero = function(edad, furColor) {
  if (!(this instanceof Mamifero)) {
    throw new Error("Inicializa Mamifero con `new`");
  }
  Animal.call(this, edad);
  this.furColor = furColor;
};

Mamifero.prototype = Object.create(Animal.prototype);
Mamifero.prototype.constructor = Mamifero;

// Humano hereda de mamifero 
// Llama al constructor de mamifero para inicializar a humano
// Añade un metodo a todas las instacias de Humano
const Humano = function(edad, furColor, idioma) {
  if (!(this instanceof Humano)) {
    throw new Error("Inicializa Humano con `new`");
  }
  Mamifero.call(this, edad, furColor);
  this.idioma = idioma;
};

Humano.prototype = Object.create(Mamifero.prototype);
Humano.prototype.constructor = Humano;
Humano.prototype.hablar = function hablar() {};
```

**Buen Ejemplo 🟢**
```javascript
class Animal {
  constructor(edad) {
    this.edad = edad;
  }

  mover() {
    
  }
}

class Mamifero extends Animal {
  constructor(edad, furColor) {
    super(edad);
    this.furColor = furColor;
  }
}

class Humano extends Mamifero {
  constructor(edad, furColor, idioma) {
    super(edad, furColor);
    this.idioma = idioma;
  }

  hablar() {
    
  }
}
```

#### Anidación de funciones ####
<p> Es una técnica que mejora mantenibilidad del código.
Su principal ventaja es que cada método devuelve el objeto permitiendo encadenar varios metodos seguidos en una sola expresion , por lo que ademas de hacer el codigo más compacto , tambien lo hace más facil de seguir</p>

**Mal ejemplo🔴**
```javascript
class Coche {
  constructor(marca, modelo, color) {
    this.marca = marca;
    this.modelo = modelo;
    this.color = color;
  }

  introducirMarca(marca) {
    this.marca = marca;
  }

  introducirModelo(modelo) {
    this.modelo = modelo;
  }

  introducirColor(color) {
    this.color = color;
  }

  guardar() {
    console.log(this.marca, this.modelo, this.color);
  }
}

const coche = new Coche("Ford", "F-150", "rojo");
coche.introducirColor("rosa");
coche.guardar();
```

**Buen ejemplo 🟢**
```javascript
class Coche {
  constructor(marca, modelo, color) {
    this.marca = marca;
    this.modelo = modelo;
    this.color = color;
  }

  introducirMarca(marca) {
    this.marca = marca;
    //Devolvemos el objeto para poder anidar funciones
    return this;
  }

  introducirModelo(modelo) {
    this.modelo = modelo;
    return this;
  }

  introducirColor(color) {
    this.color = color;
    return this;
  }

  guardar() {
    console.log(this.marca, this.modelo, this.color);
    return this;
  }
}

const coche = new Coche("Ford", "F-150", "rojo")
  .introducirColor("rosa")
  .guardar();
  ```
#### Prioriza la composición en vez de la herecia ####

<p> Es necesario saber diferenciar cuando es necesario usar la herencia y la composicion , mientras que la herencia se basa en "es un", por ejemplo un humano es un mamifero , la composicion se basa en "tiene un", por ejemplo un empleado tiene una información de impuestos de empleado</p>

**Mal Ejemplo🔴**
```javascript
class Empleado {
  constructor(nombre, correoElectronico) {
    this.nombre = nombre;
    this.correoElectronico = correoElectronico;
  }

}

class InformacionImpuestosEmpleado extends Empleado {
  constructor(ssn, salario,nombre,correoElectronico) {
    super(nombre,correoElectronico);
    this.ssn = ssn;
    this.salario = salario;
  }

}
```

**Buen Ejemplo 🟢**
```javascript
class InformacionImpuestosEmpleado {
  constructor(ssn, salario) {
    this.ssn = ssn;
    this.salario = salario;
  }
}

class Empleado {
  constructor(nombre, correoElectronico) {
    this.nombre = nombre;
    this.correoElectronico = correoElectronico;
  }

  introducirInformacionImpuestos(ssn, salario) {
    this.informacionImpuestos = new InformacionImpuestosEmpleado(ssn, salario);
  }
}
```

# Testing

El testing es una parte muy importante del desarrollo. No hay un número predefinido de test necesarios para garantizar que el programa funciona ya que puedes tener muchos pero que no sean de gran ayuda. En este caso tienes dos opciones:

- Definir con el equipo de desarrollo un porcentaje de éxito u error.
- Asegurar el 100% de cobertura de test para lo que se necesitará una buena herramienta para testear y para calcular correctamente el porcentaje cubierto.

Una buena práctica es que, una vez definido el framework de desarrollo, por cada nueva funcionalidad añadida se escriban tests.

## *Solo un concepto por test*

Cada test debe comprobar una regla o comportamiento por las siguientes razones:
- Serán test más simples y fáciles de entender.
- Serán mas fáciles de mantener.
- Aclara en que punto está el fallo en caso de haberlo.


```js
❌

test("usuarioNombreYEdad", ()=> {
  let usuario = new Usuario("Ana", 30);

  expect(usuario.nombre).toBe("Ana");             
  expect(usuario.edad).toBe(30);                  

  usuario.setNombre("Maria");
  expect(usuario.nombre).toBe("Maria");           

  usuario.cumplirAnios();
  expect(usuario.edad).toBe(31);                  
});


✅

describe("Usuario",() => {
  test("nombreCorrecto",() => {
    let usuario = new Usuario("Ana", 30);
    expect(usuario.nombre).toBe("Ana");
  });

  test("cambioNombre", () =>{
    let usuario = new Usuario("Ana", 30);
    usuario.setNombre("Maria");
    expect(usuario.nombre).toBe("Maria");
  });

  test("cumpleanios",() =>{
    let usuario = new Usuario("Ana", 30);
    usuario.cumplirAnios();
    expect(usuario.edad).toBe(31);
  });
});

```
# Concurrencia
_Realizado por Jose Manuel Vilchez Arenas_

## ¿Qué es la concurrencia?
La concurrencia es una estrategia que nos ayuda a separar lo que se hace
en nuestro código del cuando se hace. En aplicaciones de un solo hilo, el qué y cuando están
juntos.

En definitiva, se busca ejecutar varias tareas en paralelo entre sí para aprovechar mejor los 
recursos computacionales. Los encargados de proporcionar concurrencia son las llamadas 
tareas asíncronas, que permiten separar la ejecución de nuestro programa en varios hilos.

### Ventajas
* Mejora del rendimiento de nuestra aplicación
* Mejora la estructura de todo el programa

### Desventajas
* Difícil diseño e implementación
* Mayor complejidad en el código

---

## NO USES CALLBACKS ❗
Los callbacks no son limpios en legibilidad ni en cuanto al formato de texto. Una buena práctica es la 
implementación de promesas, que representan la finalización o fracaso de una tarea asíncrona.

**Mala implementación ❌**
```javascript
const tarea = (callback) => {
  const num = 1 + Math.floor(Math.random() * 6)
    setTimeout(() => {
        if (num == 6){
            console.log('Tarea completada con éxito');
        }else{
            console.log('No se pudo completar la tarea')}, 2000);    
}
tarea()

````
**Buena implementación ✔**
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

hacerTarea()
        .then((num) => console.log('Tarea completada con éxito', num))
        .catch((num) => console.log('No se pudo completar la tarea'))
```

## Async/await ✅
Podemos mejorar aún mas nuestro código mediante la implementación de async/await.
Añadimos a funciones el prefijo _async_ para usar esta función de forma imperativa sin
emplear ningun _.then()_ o _.catch()_

```javascript
function completaTarea() {
  return new Promise(resolve => {
    setTimeout(() => {
        if (num == 6){
            console.log('Tarea completada con éxito');
        }else{
            console.log('No se pudo completar la tarea');}, 2000);    
  });
}

async function solicitud() {
  try {
    let resultado = await completaTarea();
    console.log(resultado);
  } catch (error) {
    console.error('No se pudo completar la tarea. ', error);
  }
}
solicitud()
```
> En definitiva, esta tarea se completará si el número ha sido un 6 y tras un breve retardo de 2 segundos.

# Comentarios

Cuando hablamos del desarrollo de software limpio y eficiente, no podemos pasar por alto el uso de los comentarios

Estos pueden resultar muy útiles cuando se emplean de manera adecuada, ya que facilitan la comprensión del código y mejoran su mantenibilidad

Sin embargo, también pueden convertirse en una mala práctica incluso innecesaria si se abusa de ellos o se redactan de forma incorrecta 

**Malas Practicas del uso de comentarios ❌**

```javascript
// Ordenar la lista de productos por precio de menor a mayor
const productosOrdenados = productos.sort((a, b) => a.precio - b.precio);
```
> El comentario repite lo que el código ya hace.

No aporta contexto ni explica la intención de negocio (por qué se ordenan los productos). 

Si la lógica cambia (por ejemplo, ordenar por nombre), el comentario quedaría obsoleto.

```javascript
// Guardar el nombre del usuario
const nombreUsuario = "Andres";
```

> El comentario solo repite la acción evidente

```javascript
// Ordenar productos por nombre
const productosOrdenados = productos.sort((a, b) => a.precio - b.precio);
```

> Comentario desactualizado, el comentario dice "por nombre", pero el código ordena por precio. Esto genera confusión.

**Buenas practicas utilizando comentarios ✔**

```javascript
function esUsuarioActivo(usuario) {
  return usuario.activo;
}
const usuariosActivos = usuarios.filter(esUsuarioActivo);
```

Quitar un comentario por una funcion informativa

```javascript
// Este método muta el array original. Usar con precaución si se necesita mantener la lista intacta.
function ordenarPorPrecio(productos) {
  return productos.sort((a, b) => a.precio - b.precio);
}
```
> El comentario informa sobre un efecto secundario que podría sorprender a otro desarrollador

```javascript
// Regla de negocio: los clientes VIP obtienen un 20% de descuento adicional en todas las compras
function calcularPrecioFinal(cliente, precioBase) {
  if (cliente.esVIP) {
    return precioBase * 0.8;
  }
  return precioBase;
}
```
> El comentario explica por qué existe esa condición, no lo que ya hace el código

## Conclucion final

Como desarrolladores, debemos evitar el uso excesivo de comentarios, ya que en muchos casos son una señal de falta de expresividad en nuestro código. Un código bien estructurado y con nombres claros en funciones, variables y constantes debería ser comprensible por sí mismo, sin necesidad de explicaciones adicionales

No obstante, existen situaciones en las que resulta imposible prescindir de los comentarios. En esos casos, es fundamental que sean contundentes e informativos, aportando valor real al lector. Un buen comentario debe explicar la intención, la regla de negocio o la limitación técnica detrás de una decisión, nunca repetir lo que ya es evidente en la sintaxis




