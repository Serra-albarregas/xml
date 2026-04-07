# Teoría sobre el esquema XSD de una agenda de contactos

Este documento describe los principales conceptos y estructuras del fichero XSD proporcionado, que define un esquema XML para una agenda de contactos. El objetivo es comprender los elementos, atributos, tipos complejos y simples utilizados.

---

## 1. Introducción a XSD

XSD (XML Schema Definition) es un lenguaje que permite definir la estructura y las reglas de validación de un documento XML. Con XSD se puede especificar:

- Qué elementos y atributos pueden aparecer.
- En qué orden deben ir.
- Qué tipo de datos contienen.
- Reglas de validación como longitudes, patrones, enumeraciones, etc.

---

## 2. Tipos de elementos

### 2.1 Elementos complejos (`xs:complexType`)

Los elementos complejos son aquellos que contienen otros elementos o/y atributos.

Dentro de un elemento complejo podemos encontrar:
- Secuencia de elementos.
- Contenido simple extendido.
- Una seleción de elementos.
- Un elemento vacío (con o sin atributos).

En el esquema se encuentran varios:

#### `agenda`

- Contiene una **secuencia** de elementos `contacto`.
- Puede tener múltiples contactos (`maxOccurs="unbounded"`).

#### `contacto`

- Contiene los datos de una persona: `nombre`, `apellidos`, `movil`, `fijo`, `email`, `genero`, `pareja`, `comentario`.
- Algunos elementos son opcionales (`minOccurs="0"`) y/o repetibles (`maxOccurs="unbounded"`).
- Tiene un atributo obligatorio `codigo`.

#### `movil` y `email`

- Son elementos complejos con **contenido simple extendido** (`xs:simpleContent` y `xs:extension`): contienen texto, pero además incluyen atributos (`empresa` para `movil`, `tipo` para `email`).

#### `genero`

- Utiliza una estructura de elección (`xs:choice`) entre dos posibles subelementos: `hombre` o `mujer`.

#### `pareja`

- Es un elemento vacío, pero con un atributo `codigo` que es una referencia (`xs:IDREF`) a otro contacto de la agenda.

#### `hombre` y `mujer`

- Son elementos definidos como vacíos (sin contenido ni atributos), usados como marcadores.

---

### 2.2 Elementos simples (`xs:element` con tipo simple)

- `nombre`, `apellidos`, `comentario`: texto simple (`xs:string`).
- `fijo`: usa el tipo `movil-type`, definido con validación de formato.
  
---

## 3. Tipos simples (`xs:simpleType`)

Los tipos simples definen restricciones sobre tipos de datos básicos.

### `movil-type`

- Tipo de dato basado en `xs:string`.
- Usa una expresión regular (`pattern`) para validar que el valor sea un número de 9 dígitos (`\d{9}`).

### `email-type`

- Tipo de dato basado en `xs:string`.
- Valida que el contenido tenga un formato básico de email: `[^@]+@[^@]+\.[^@]+`.

---

## 4. Atributos (`xs:attribute`)

Los atributos permiten añadir información adicional a los elementos.

### `codigo` (referenciado por `contacto` y `pareja`)

- Definido como tipo ID con patrón `c[0-9]+`, por ejemplo: `c001`.

### `empresa`

- Atributo sin restricciones específicas, usado en el elemento `movil`.

### `tipo`

- Define si un email es de tipo `personal` o `empresa`, mediante enumeración.

---

## 5. Estructuras destacadas

### 5.1 `xs:sequence`

- Se usa para establecer un orden fijo de los subelementos.

### 5.2 `xs:choice`

- Permite una selección entre varios subelementos (como en el género).

### 5.3 `xs:extension`

- Se usa dentro de `xs:simpleContent` para extender tipos simples con atributos.

---

## 6. Relaciones

- El atributo `codigo` es un identificador único (`xs:ID`).
- El elemento `pareja` referencia a otro `contacto` mediante un ID (`xs:IDREF`), estableciendo una relación entre contactos.

---

## 7. Validaciones

Las validaciones se realizan mediante:

- **Restricciones de ocurrencia** (`minOccurs`, `maxOccurs`).
- **Patrones de expresiones regulares** para tipos como móvil y email.
- **Enumeraciones** para restringir valores posibles (como el tipo de email).
- **Tipos de datos estándar** de XML Schema (`xs:string`, `xs:ID`, etc.).

---