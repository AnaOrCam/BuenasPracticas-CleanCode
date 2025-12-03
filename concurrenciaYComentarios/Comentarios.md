# Comentarios

### Cuando hablamos del desarrollo de software limpio y eficiente, no podemos pasar por alto el uso de los comentarios

### Estos pueden resultar muy útiles cuando se emplean de manera adecuada, ya que facilitan la comprensión del código y mejoran su mantenibilidad

### Sin embargo, también pueden convertirse en una mala práctica incluso innecesaria si se abusa de ellos o se redactan de forma incorrecta 

## Malas Practicas del uso de comentarios

```javascript
// Ordenar la lista de productos por precio de menor a mayor
const productosOrdenados = productos.sort((a, b) => a.precio - b.precio);
```
### **El comentario repite lo que el código ya hace**
### **No aporta contexto ni explica la intención de negocio (por qué se ordenan los productos).**
### **Si la lógica cambia (por ejemplo, ordenar por nombre), el comentario quedaría obsoleto**

```javascript
// Guardar el nombre del usuario
const nombreUsuario = "Andres";
```

### **El comentario solo repite la acción evidente**

```javascript
// Ordenar productos por nombre
const productosOrdenados = productos.sort((a, b) => a.precio - b.precio);
```

### **Comentario desactualizado El comentario dice "por nombre", pero el código ordena por precio. Esto genera confusión.**


# Buenas paracticas utilizando comentarios 

```javascript
function esUsuarioActivo(usuario) {
  return usuario.activo;
}
const usuariosActivos = usuarios.filter(esUsuarioActivo);
```

### **Quitar un comentario por una funcion informativa**

```javascript
// Este método muta el array original. Usar con precaución si se necesita mantener la lista intacta.
function ordenarPorPrecio(productos) {
  return productos.sort((a, b) => a.precio - b.precio);
}
```

## **El comentario informa sobre un efecto secundario que podría sorprender a otro desarrollador**

```javascript
// Regla de negocio: los clientes VIP obtienen un 20% de descuento adicional en todas las compras
function calcularPrecioFinal(cliente, precioBase) {
  if (cliente.esVIP) {
    return precioBase * 0.8;
  }
  return precioBase;
}
```

## **El comentario explica por qué existe esa condición, no lo que ya hace el código**

## Conclucion final

### Como desarrolladores, debemos evitar el uso excesivo de comentarios, ya que en muchos casos son una señal de falta de expresividad en nuestro código. Un código bien estructurado y con nombres claros en funciones, variables y constantes debería ser comprensible por sí mismo, sin necesidad de explicaciones adicionales

### No obstante, existen situaciones en las que resulta imposible prescindir de los comentarios. En esos casos, es fundamental que sean contundentes e informativos, aportando valor real al lector. Un buen comentario debe explicar la intención, la regla de negocio o la limitación técnica detrás de una decisión, nunca repetir lo que ya es evidente en la sintaxis