# XSD — XML Schema Definition: Guía Completa

## ¿Qué es XSD?

XSD (XML Schema Definition) es un lenguaje de esquemas utilizado para **describir la estructura y validar el contenido de documentos XML**. Fue desarrollado por el W3C (World Wide Web Consortium) y publicado como recomendación oficial en el año 2001.

Un archivo XSD define:
- Qué elementos y atributos puede contener un XML.
- En qué orden deben aparecer.
- Cuántas veces pueden repetirse.
- Qué tipo de datos deben contener.
- Qué valores están permitidos.

En esencia, un XSD actúa como un **contrato o plantilla** que todo documento XML asociado debe cumplir para considerarse válido.

---

## ¿Por qué se usa XSD?

XSD resuelve una necesidad fundamental cuando se trabaja con XML: garantizar que los datos tienen la forma esperada antes de procesarlos. Sus principales ventajas son:

- **Validación automática**: permite comprobar que un XML es estructuralmente correcto sin necesidad de escribir código de validación manual.
- **Interoperabilidad**: cuando dos sistemas intercambian datos XML, el XSD actúa como contrato común que ambos comprenden.
- **Documentación implícita**: el propio esquema describe la estructura del XML, sirviendo de documentación técnica.
- **Tipos de datos ricos**: a diferencia del antiguo DTD, XSD soporta tipos como enteros, fechas, expresiones regulares, etc.
- **Soporte en herramientas**: la mayoría de IDEs, editores XML y frameworks de desarrollo leen XSD para ofrecer autocompletado y validación en tiempo real.

---

## ¿Cómo de extendido está XSD?

XSD es el estándar más utilizado para la definición de esquemas XML. Su presencia en el mundo real es muy amplia:

- **Servicios web SOAP**: todos los contratos WSDL usan XSD para definir los tipos de datos de los mensajes.
- **Administración pública**: la Agencia Tributaria española, la Seguridad Social y multitud de organismos oficiales publican sus formatos de intercambio de datos en XSD.
- **Industria editorial**: el estándar ONIX (información de libros) y otros formatos sectoriales están definidos con XSD.
- **Configuración de aplicaciones**: tecnologías como Spring Framework (Java), Maven o `.csproj` de .NET usan XML validado con XSD.
- **Bases de datos**: Oracle, SQL Server y otros motores relacionales admiten la importación/exportación de datos en XML validado por XSD.

Prácticamente cualquier sistema empresarial o institucional que intercambie datos en XML utiliza XSD como mecanismo de validación.

---

## Cómo vincular un XSD a un documento XML

Existen dos formas principales de asociar un XSD a un XML:

### Sin espacio de nombres (no namespace)

Se usa el atributo `xsi:noNamespaceSchemaLocation`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<agenda xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="agenda.xsd">
    ...
</agenda>
```

### Con espacio de nombres (namespace)

Se usa el atributo `xsi:schemaLocation`, indicando primero el namespace y luego la ruta al XSD:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<agenda xmlns="http://www.ejemplo.com/agenda"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.ejemplo.com/agenda agenda.xsd">
    ...
</agenda>
```

En ambos casos, `xmlns:xsi` declara el espacio de nombres estándar de XML Schema Instance, que es quien define los atributos de vinculación.

---

## Estructura general de un archivo XSD

Todo archivo XSD comienza con el elemento raíz `xs:schema`, que declara el espacio de nombres estándar de XML Schema:

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
    ...
</xs:schema>
```

El prefijo `xs:` (o `xsd:` indistintamente) se antepone a todos los elementos propios del lenguaje de esquemas para distinguirlos de los elementos del XML que se está definiendo.

---

## Conceptos fundamentales

### 1. `xs:element` — Declaración de elementos

Define un elemento que puede aparecer en el XML. Tiene dos formas principales:

**Con tipo simple directo:**
```xml
<xs:element name="nombre" type="xs:string"/>
```

**Con tipo complejo anónimo** (el tipo se define dentro del propio elemento):
```xml
<xs:element name="agenda">
    <xs:complexType>
        ...
    </xs:complexType>
</xs:element>
```

**Con referencia a otro elemento** ya declarado globalmente:
```xml
<xs:element ref="contacto" maxOccurs="unbounded"/>
```

El atributo `ref` permite reutilizar la definición de un elemento declarado en otro lugar del esquema.

---

### 2. `xs:complexType` — Tipo complejo

Se usa cuando un elemento **contiene otros elementos y/o atributos**. Es el tipo más común en esquemas reales.

```xml
<xs:element name="contacto">
    <xs:complexType>
        <xs:sequence>
            ...
        </xs:sequence>
        <xs:attribute ref="codigo" use="required"/>
    </xs:complexType>
</xs:element>
```

Un `xs:complexType` puede contener un compositor (`xs:sequence`, `xs:choice`, `xs:all`) y/o atributos.

---

### 3. `xs:sequence` — Secuencia ordenada

Indica que los elementos hijos deben aparecer **en el orden exacto** en que están declarados:

```xml
<xs:sequence>
    <xs:element ref="nombre"/>
    <xs:element ref="apellidos"/>
    <xs:element ref="movil" minOccurs="0" maxOccurs="unbounded"/>
</xs:sequence>
```

Es el compositor más habitual. El orden importa: un XML que ponga `apellidos` antes que `nombre` no sería válido.

---

### 4. `xs:choice` — Elección exclusiva

Indica que debe aparecer **exactamente uno** de los elementos hijos, pero no varios:

```xml
<xs:element name="genero">
    <xs:complexType>
        <xs:choice>
            <xs:element ref="hombre"/>
            <xs:element ref="mujer"/>
        </xs:choice>
    </xs:complexType>
</xs:element>
```

En este ejemplo, `genero` puede contener `hombre` o `mujer`, pero nunca ambos.

---

### 5. `minOccurs` y `maxOccurs` — Cardinalidad

Controlan cuántas veces puede aparecer un elemento:

| Combinación | Significado |
|---|---|
| `minOccurs="0" maxOccurs="1"` | Opcional, como máximo una vez |
| `minOccurs="0" maxOccurs="unbounded"` | Opcional, puede repetirse sin límite |
| `minOccurs="1" maxOccurs="1"` | Obligatorio exactamente una vez (valor por defecto) |
| `minOccurs="1" maxOccurs="unbounded"` | Al menos una vez, puede repetirse |

`unbounded` significa sin límite superior. Ejemplo del esquema:

```xml
<xs:element ref="contacto" maxOccurs="unbounded"/>
<!-- Al menos un contacto, sin límite máximo -->

<xs:element ref="movil" minOccurs="0" maxOccurs="unbounded"/>
<!-- Cero o más móviles -->

<xs:element ref="email" minOccurs="0" maxOccurs="1"/>
<!-- Email opcional -->
```

---

### 6. `xs:attribute` — Atributos

Define un atributo que puede o debe tener un elemento. Se declaran dentro de `xs:complexType`, **después** de los compositores:

```xml
<xs:attribute ref="codigo" use="required"/>
```

El atributo `use` puede tomar tres valores:
- `required` — obligatorio.
- `optional` — opcional (valor por defecto).
- `prohibited` — no se permite.

Los atributos también pueden declararse globalmente con nombre y tipo:

```xml
<xs:attribute name="empresa" type="xs:string"/>
```

---

### 7. `xs:simpleType` — Tipo simple personalizado

Permite **crear tipos de datos propios** basados en tipos existentes, añadiendo restricciones. A diferencia de `xs:complexType`, un `xs:simpleType` solo contiene texto, nunca elementos hijos ni atributos.

```xml
<xs:simpleType name="movil-type">
    <xs:restriction base="xs:string">
        <xs:pattern value="\d{9}"/>
    </xs:restriction>
</xs:simpleType>
```

Una vez definido, se puede reutilizar en varios elementos:

```xml
<xs:element name="fijo" type="movil-type"/>
```

---

### 8. `xs:restriction` — Restricciones

Deriva un nuevo tipo a partir de uno existente, **restringiendo** los valores permitidos. Siempre se define dentro de un `xs:simpleType`:

```xml
<xs:restriction base="xs:string">
    ...
</xs:restriction>
```

El atributo `base` indica el tipo del que se parte. Las restricciones más comunes son:

#### `xs:pattern` — Expresión regular

Valida que el valor siga un patrón concreto:

```xml
<!-- Exactamente 9 dígitos -->
<xs:pattern value="\d{9}"/>

<!-- Formato email básico -->
<xs:pattern value="[^@]+@[^@]+\.[^@]+"/>

<!-- Empieza por 'c' seguido de uno o más dígitos -->
<xs:pattern value="c[0-9]+"/>
```

#### `xs:enumeration` — Lista de valores permitidos

Restringe el valor a un conjunto cerrado de opciones:

```xml
<xs:restriction base="xs:string">
    <xs:enumeration value="personal"/>
    <xs:enumeration value="empresa"/>
</xs:restriction>
```

Solo se aceptarán los valores `personal` o `empresa`, cualquier otro producirá un error de validación.

---

### 9. `xs:simpleContent` y `xs:extension` — Contenido simple con atributos

Se usa cuando un elemento tiene **contenido de texto y además atributos**. Es la solución cuando `xs:complexType` necesita contener texto (no elementos hijos) y atributos al mismo tiempo:

```xml
<xs:element name="movil">
    <xs:complexType>
        <xs:simpleContent>
            <xs:extension base="movil-type">
                <xs:attribute ref="empresa"/>
            </xs:extension>
        </xs:simpleContent>
    </xs:complexType>
</xs:element>
```

El resultado es un elemento que contiene un número de móvil (validado como `movil-type`) y opcionalmente un atributo `empresa`. En el XML quedaría así:

```xml
<movil empresa="Movistar">666123456</movil>
```

`xs:extension` extiende el tipo base (`movil-type`) añadiéndole atributos.

---

### 10. Tipos de datos primitivos de XSD

XSD ofrece tipos de datos predefinidos que se usan directamente con `type="xs:..."`:

| Tipo | Descripción | Ejemplo de valor |
|---|---|---|
| `xs:string` | Cadena de texto | `"Hola mundo"` |
| `xs:integer` | Número entero | `42` |
| `xs:decimal` | Número decimal | `3.14` |
| `xs:boolean` | Verdadero o falso | `true` / `false` |
| `xs:date` | Fecha | `2024-01-15` |
| `xs:dateTime` | Fecha y hora | `2024-01-15T10:30:00` |
| `xs:ID` | Identificador único en el documento | `c001` |
| `xs:IDREF` | Referencia a un `xs:ID` existente | `c001` |

#### `xs:ID` e `xs:IDREF` — Identidad y referencias

Son tipos especiales que permiten establecer **relaciones entre elementos** del mismo documento XML:

```xml
<!-- El atributo 'codigo' es un ID único -->
<xs:attribute name="codigo">
    <xs:simpleType>
        <xs:restriction base="xs:ID">
            <xs:pattern value="c[0-9]+"/>
        </xs:restriction>
    </xs:simpleType>
</xs:attribute>

<!-- El atributo 'codigo' de pareja referencia un ID existente -->
<xs:element name="pareja">
    <xs:complexType>
        <xs:attribute name="codigo" type="xs:IDREF"/>
    </xs:complexType>
</xs:element>
```

El validador comprobará automáticamente que todo `IDREF` apunta a un `ID` que existe en el documento, y que no hay dos elementos con el mismo `ID`.

---

## Ejemplo completo comentado

El siguiente esquema define una agenda de contactos. Se muestran todos los conceptos anteriores integrados:

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

    <!-- ELEMENTOS COMPLEJOS -->

    <!-- Raíz: agenda contiene uno o más contactos -->
    <xs:element name="agenda">
        <xs:complexType>
            <xs:sequence>
                <xs:element ref="contacto" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>

    <!-- Contacto: secuencia de datos personales + atributo obligatorio -->
    <xs:element name="contacto">
        <xs:complexType>
            <xs:sequence>
                <xs:element ref="nombre"/>
                <xs:element ref="apellidos"/>
                <xs:element ref="movil"    minOccurs="0" maxOccurs="unbounded"/>
                <xs:element ref="fijo"     minOccurs="0" maxOccurs="unbounded"/>
                <xs:element ref="email"    minOccurs="0" maxOccurs="1"/>
                <xs:element ref="genero"   minOccurs="0" maxOccurs="1"/>
                <xs:element ref="pareja"   minOccurs="0" maxOccurs="1"/>
                <xs:element ref="comentario"/>
            </xs:sequence>
            <xs:attribute ref="codigo" use="required"/>
        </xs:complexType>
    </xs:element>

    <!-- Móvil: texto validado + atributo opcional -->
    <xs:element name="movil">
        <xs:complexType>
            <xs:simpleContent>
                <xs:extension base="movil-type">
                    <xs:attribute ref="empresa"/>
                </xs:extension>
            </xs:simpleContent>
        </xs:complexType>
    </xs:element>

    <!-- Email: texto validado + atributo con enumeración -->
    <xs:element name="email">
        <xs:complexType>
            <xs:simpleContent>
                <xs:extension base="email-type">
                    <xs:attribute ref="tipo"/>
                </xs:extension>
            </xs:simpleContent>
        </xs:complexType>
    </xs:element>

    <!-- Género: elección entre hombre o mujer -->
    <xs:element name="genero">
        <xs:complexType>
            <xs:choice>
                <xs:element ref="hombre"/>
                <xs:element ref="mujer"/>
            </xs:choice>
        </xs:complexType>
    </xs:element>

    <!-- Pareja: referencia a otro contacto por su ID -->
    <xs:element name="pareja">
        <xs:complexType>
            <xs:attribute name="codigo" type="xs:IDREF"/>
        </xs:complexType>
    </xs:element>

    <!-- Elementos vacíos (sin contenido ni atributos) -->
    <xs:element name="hombre"><xs:complexType/></xs:element>
    <xs:element name="mujer"><xs:complexType/></xs:element>

    <!-- ELEMENTOS SIMPLES -->
    <xs:element name="nombre"    type="xs:string"/>
    <xs:element name="apellidos" type="xs:string"/>
    <xs:element name="fijo"      type="movil-type"/>
    <xs:element name="comentario" type="xs:string"/>

    <!-- ATRIBUTOS GLOBALES -->

    <!-- codigo: ID único con patrón c + dígitos -->
    <xs:attribute name="codigo">
        <xs:simpleType>
            <xs:restriction base="xs:ID">
                <xs:pattern value="c[0-9]+"/>
            </xs:restriction>
        </xs:simpleType>
    </xs:attribute>

    <xs:attribute name="empresa" type="xs:string"/>

    <!-- tipo: solo acepta 'personal' o 'empresa' -->
    <xs:attribute name="tipo">
        <xs:simpleType>
            <xs:restriction base="xs:string">
                <xs:enumeration value="personal"/>
                <xs:enumeration value="empresa"/>
            </xs:restriction>
        </xs:simpleType>
    </xs:attribute>

    <!-- TIPOS SIMPLES PERSONALIZADOS -->

    <!-- movil-type: exactamente 9 dígitos -->
    <xs:simpleType name="movil-type">
        <xs:restriction base="xs:string">
            <xs:pattern value="\d{9}"/>
        </xs:restriction>
    </xs:simpleType>

    <!-- email-type: formato básico de email -->
    <xs:simpleType name="email-type">
        <xs:restriction base="xs:string">
            <xs:pattern value="[^@]+@[^@]+\.[^@]+"/>
        </xs:restriction>
    </xs:simpleType>

</xs:schema>
```

---

## Resumen de conceptos

| Concepto | Descripción |
|---|---|
| `xs:schema` | Elemento raíz del esquema |
| `xs:element` | Declara un elemento XML |
| `xs:attribute` | Declara un atributo |
| `xs:complexType` | Tipo con elementos hijos y/o atributos |
| `xs:simpleType` | Tipo solo con contenido de texto |
| `xs:sequence` | Hijos ordenados |
| `xs:choice` | Exactamente uno de los hijos |
| `xs:simpleContent` | Contenido textual + atributos |
| `xs:extension` | Extiende un tipo añadiendo atributos |
| `xs:restriction` | Restringe los valores de un tipo |
| `xs:pattern` | Valida con expresión regular |
| `xs:enumeration` | Lista de valores permitidos |
| `minOccurs` / `maxOccurs` | Cardinalidad mínima y máxima |
| `use="required"` | Atributo obligatorio |
| `ref` | Referencia a un elemento/atributo global |
| `xs:ID` | Identificador único en el documento |
| `xs:IDREF` | Referencia a un `xs:ID` existente |
| `xs:string`, `xs:integer`... | Tipos de datos primitivos |
