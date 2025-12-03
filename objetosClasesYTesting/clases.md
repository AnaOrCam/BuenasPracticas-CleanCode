# Clases #
<p>Escribir código que sea fácil de entender y mantener es igual de importante que saber usar las herramientas del lenguaje.</p> 
<p>Las clases pueden ser útiles, pero también pueden complicar innecesariamente el código si se usan mal.</p>

#### Buena estructuracion del codigo ####

<p>Para que el codigo sea facil de mantener es necesario que este lleve un orden en la definicion de sus metodos y variables, junto con un buen uso de tabuladores</p>

**Mal ejemplo 🔴**

```
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
```
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

```
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
```
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
```
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
```
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
```
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
```
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
}```
