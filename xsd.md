# Conceptos de XSD (XML Schema Definition) Utilizados

Este documento resume todos los conceptos y construcciones utilizados en el esquema XSD del ejemplo de agenda XML.

---

## 1. **Elementos y Tipos**

- `xs:element`: Define un elemento XML.
- `xs:complexType`: Define un tipo complejo (elemento con subelementos o atributos).
- `xs:simpleType`: Define un tipo simple con restricciones.

---

## 2. **Restricciones de Valores**

- `xs:restriction`: Restringe un tipo base con ciertas condiciones.
- `xs:pattern`: Define una expresión regular que debe cumplirse.
  - Ej.: Teléfonos, emails, nombres.
- `xs:enumeration`: Lista de valores válidos predefinidos.
  - Ej.: Tipo de email: `empresa`, `personal`.
- `xs:minLength` / `xs:maxLength`: Define longitud mínima y máxima de una cadena.
- `xs:minInclusive` / `xs:maxInclusive`: Define un rango válido para valores numéricos (no usado explícitamente, pero común).

---

## 3. **Tipos de Datos**

- `xs:string`: Texto plano.
- `xs:ID`: Identificador único dentro del documento.
- `xs:IDREF`: Referencia a un `xs:ID`.
- `xs:IDREFS`: Lista de referencias a varios `xs:ID`s.
- `xs:anyType`: Acepta cualquier contenido. Usado para elementos vacíos (`<hombre />`).

---

## 4. **Estructura del Contenido**

- `xs:sequence`: Especifica un orden obligatorio de los subelementos.
- `xs:choice`: Permite elegir entre varios elementos posibles (uno solo).
  - Ej.: Género como `hombre` o `mujer`.

---

## 5. **Atributos**

- `xs:attribute`: Define un atributo para un elemento.
- `use="required"`: El atributo es obligatorio.
- `use="optional"`: El atributo es opcional.
- `default`: (No usado en el ejemplo) define un valor por defecto si no se proporciona.

---

## 6. **Ocurrencias**

- `minOccurs` / `maxOccurs`: Número mínimo/máximo de veces que puede aparecer un elemento.
  - Ej.: `<movil>` puede repetirse (varios móviles por contacto).

---

## 7. **Tipos Personalizados**

- Definición de nuevos tipos reutilizables usando `xs:simpleType` o `xs:complexType` con nombre:
  - `telefonoMovilType`
  - `telefonoFijoType`
  - `emailString`
  - `movilType`, `emailType`, `generoType`, `parejaType`

---

## 8. **Contenido con Atributos**

- `xs:simpleContent` + `xs:extension`: Permite que un elemento tenga contenido simple (texto) y atributos.
  - Ej.: `<movil empresa="...">654654...</movil>`

---

## 9. **Validación de Formatos Específicos (Patrones)**

- Teléfonos móviles: `6\d{8}|7\d{8}`
- Teléfonos fijos: `9\d{8}`
- Email: `[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}`
- Nombre propio: `[A-ZÁÉÍÓÚÑ][a-záéíóúñ]+`

---

## 10. **Relaciones entre Elementos**

- Uso de `xs:ID` y `xs:IDREFS` para vincular contactos (relación de pareja).

---