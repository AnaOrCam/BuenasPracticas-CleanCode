
# Testing

El testing es una parte muy importante del desarrollo. No hay un número predefinido de test necesarios para garantizar que el programa funciona ya que puedes tener muchos pero que no sean de gran ayuda. En este caso tienes dos opciones:

- Definir con el equipo de desarrollo un porcentaje de éxito u error.
- Asegurar el 100% de covertura de test para lo que se necesitará una buena herramienta para testear y para calcular correctamente el porcentaje cubierto.

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
