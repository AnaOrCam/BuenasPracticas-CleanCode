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