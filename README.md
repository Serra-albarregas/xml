# 📘 Resumen Teórico de DTD con Ejemplos

## 📌 ¿Qué es un DTD?
Un **DTD (Document Type Definition)** define la estructura y las reglas de un documento XML. Establece qué elementos pueden aparecer, en qué orden, cuántas veces, y qué atributos pueden tener.

---

## 🧱 Componentes utilizados en el DTD

---

### 1. ELEMENTOS (`<!ELEMENT>`)

Define un elemento y su contenido:

#### 🔹 Sintaxis básica:
```dtd
<!ELEMENT nombre-del-elemento tipo-de-contenido>
```

#### 📘 Tipos usados:

| Tipo           | Significado                         | Ejemplo                              |
|----------------|-------------------------------------|--------------------------------------|
| `(#PCDATA)`    | Texto libre                         | `<!ELEMENT nombre (#PCDATA)>`        |
| `EMPTY`        | Elemento vacío                      | `<!ELEMENT pareja EMPTY>`            |
| `(A, B, C)`    | Secuencia obligatoria               | `<!ELEMENT contacto (nombre, apellidos, ...)` |
| `A*`           | 0 o más repeticiones                | `<!ELEMENT contacto (..., movil*, ...)>` |
| `A?`           | Opcional (0 o 1 vez)                | `<!ELEMENT contacto (..., email?, ...)>`       |
| `A \| B`        | Alternancia (uno u otro)            | `<!ELEMENT genero (hombre \| mujer)>` |

---

### 2. ATRIBUTOS (`<!ATTLIST>`)

Define los atributos que puede tener un elemento.

#### 🔹 Sintaxis básica:
```dtd
<!ATTLIST elemento nombre-tipo tipo-de-dato restricción>
```

#### 📘 Tipos usados:

| Tipo           | Significado                                 | Ejemplo                                           |
|----------------|---------------------------------------------|---------------------------------------------------|
| `ID`           | Identificador único                         | `<!ATTLIST contacto codigo ID #REQUIRED>`         |
| `IDREFS`       | Referencia a uno o más ID existentes        | `<!ATTLIST pareja codigo IDREFS #REQUIRED>`       |
| `CDATA`        | Texto libre                                 | `<!ATTLIST movil empresa CDATA #IMPLIED>`         |
| `(valores...)` | Enumeración de valores válidos              | `<!ATTLIST email tipo (empresa \| personal) #REQUIRED>` |

#### 📘 Restricciones usadas:

- `#REQUIRED`: El atributo es obligatorio.
- `#IMPLIED`: El atributo es opcional.

---

### 3. OCURRENCIA Y REPETICIÓN

Controlan cuántas veces puede aparecer un elemento:

| Símbolo | Significado         | Ejemplo                          |
|---------|---------------------|----------------------------------|
| `*`     | Cero o más veces    | `movil*`, `fijo*`                |
| `+`     | Una o más veces     | `<!ELEMENT agenda (contacto+)>`  |
| `?`     | Cero o una vez      | `email?`, `genero?`              |
| nada    | Exactamente una vez | `nombre`, `apellidos`            |

---

### 4. RELACIONES ENTRE ELEMENTOS

#### 🔹 Elementos anidados
Elementos que contienen otros elementos:

```dtd
<!ELEMENT contacto (nombre, apellidos, movil*, fijo*, email?, genero?, pareja?, comentario)>
```

#### 🔹 Referencias entre elementos
Se usan `ID` y `IDREFS` para crear vínculos:

- `contacto` usa `codigo` como identificador (`ID`)
- `pareja` usa `codigo` como referencia (`IDREFS`)

---

### 5. EJEMPLO COMPLETO DE DECLARACIÓN

```dtd
<!ELEMENT contacto (nombre, apellidos, movil*, fijo*, email?, genero?, pareja?, comentario)>
<!ATTLIST contacto codigo ID #REQUIRED>

<!ELEMENT nombre (#PCDATA)>
<!ELEMENT apellidos (#PCDATA)>
<!ELEMENT movil (#PCDATA)>
<!ATTLIST movil empresa CDATA #IMPLIED>

<!ELEMENT fijo (#PCDATA)>
<!ELEMENT email (#PCDATA)>
<!ATTLIST email tipo (empresa | personal) #REQUIRED>

<!ELEMENT genero (hombre | mujer)>
<!ELEMENT hombre EMPTY>
<!ELEMENT mujer EMPTY>

<!ELEMENT pareja EMPTY>
<!ATTLIST pareja codigo IDREFS #REQUIRED>

<!ELEMENT comentario (#PCDATA)>
```

---
