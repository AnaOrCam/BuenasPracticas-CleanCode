
## Objetos y estructuras de datos

- Utilizar getters y setters: 

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

- Utilizar atributos/métodos privados en los objetos:

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