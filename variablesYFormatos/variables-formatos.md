# Variables y Formato - Clean Code
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
