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

    SetupTeardownIncluder.render(pageData) // representa los datos en el objeto pageData

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

    // Tienes que entender la relación entre 'stream' y 'name' y recordar el orden.
    function writeField(outputStream, name) {
        outputStream.write(name);
    }

✅ Bien:

    // Pasamos el argumento complejo en el constructor de la clase.
    const writer = new FieldWriter(outputStream);
    writer.write(name);

### Triadas
Las funciones con 3 argumentos nos generan más problemas cuando tengamos que ordenar, ignorar o pararse a entenderlos.

❌ Mal:

    / Obliga a pausar la lectura para ignorar el primer parámetro.
    assertEquals("Error de conexión", "Conectado", estadoActual);

✅ Bien:

    // Comparar números decimales correctamente y es fácil de comprender.
    assertFloatEquals(1.0, amount, 0.001);  

### Objeto de argumento
Cuando una función parece necesitar dos o más argumentos, es posible que alguno de ellos se tenga una clase propia.

❌ Mal:

    Circle makeCircle (double x, double y, double radius);

✅ Bien:

    Circle makeCircle(Point center, double radius);

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

    // Es difícil recordar si 'expected' va primero o segundo.
    assertEqual(expected, actual);

✅ Bien:

    // El nombre de la función dicta el orden. Elimina la ambigüedad.
    assertExpectedEqualsActual(expected, actual);

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

    // Parece una entrada, pero en realidad modifica 's'.
    agregarPie(s);

✅ Bien:

    // Se ve claro que 'informe' es el objeto que se modifica a sí mismo.
    informe.agregarPie();

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