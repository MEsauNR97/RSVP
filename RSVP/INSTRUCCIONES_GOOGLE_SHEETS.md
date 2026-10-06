# Instrucciones: Conectar RSVP con Google Sheets

## Paso 1 — Crear la hoja de Google Sheets

1. Ve a [sheets.google.com](https://sheets.google.com) e inicia sesión.
2. Crea una hoja nueva y ponle el nombre que quieras (ej. `RSVP Boda 2025`).
3. En la primera fila escribe estos encabezados exactos (uno por columna):

   | A | B | C | D | E |
   |---|---|---|---|---|
   | codigo | familia | personas | ninos_asisten | fecha |

4. Copia la **URL** de la hoja (la necesitarás en el siguiente paso).

---

## Paso 2 — Crear el Apps Script

1. Dentro de tu Google Sheet ve al menú: **Extensiones → Apps Script**.
2. Borra todo el código que aparece por defecto.
3. Pega el siguiente código:

```javascript
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const data  = JSON.parse(e.postData.contents);

  sheet.appendRow([
    data.codigo,
    data.familia,
    data.personas,
    data.ninos_asisten,
    data.fecha,
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({ status: 'ok' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

4. Haz clic en **Guardar** (ícono de disco o Ctrl+S).
5. Ponle un nombre al proyecto (ej. `RSVP Boda`).

---

## Paso 3 — Desplegar como Web App

1. Haz clic en **Implementar → Nueva implementación**.
2. En "Tipo de implementación" selecciona **Aplicación web**.
3. Configura así:
   - **Descripción:** `RSVP Boda`
   - **Ejecutar como:** `Yo (tu correo)`
   - **Quién tiene acceso:** `Cualquier usuario`
4. Haz clic en **Implementar**.
5. Google te pedirá que autorices permisos — acepta todo.
6. Copia la **URL de la aplicación web** que aparece al final.

---

## Paso 4 — Pegar la URL en la página RSVP

1. Abre el archivo `index.html` con cualquier editor de texto.
2. Busca esta línea (cerca del final del archivo):

```javascript
const APPS_SCRIPT_URL = 'PEGAR_URL_AQUI';
```

3. Reemplaza `PEGAR_URL_AQUI` con la URL que copiaste. Ejemplo:

```javascript
const APPS_SCRIPT_URL = 'https://script.google.com/macros/s/AKfycb.../exec';
```

4. Guarda el archivo.

---

## Paso 5 — Probar

1. Abre `index.html` en tu navegador.
2. Sube el archivo `familias.xlsx`.
3. Ingresa un código (ej. `001`) y confirma asistencia.
4. Regresa a Google Sheets — deberías ver la fila nueva en segundos.

---

## Notas importantes

- Cada vez que alguien confirme, se agrega una fila nueva en tu Google Sheet.
- Puedes ver las respuestas en tiempo real desde cualquier dispositivo.
- Si necesitas **actualizar la lista de familias**, solo modifica `familias.xlsx` y vuelve a subirlo en la página.
- Si en el futuro cambias algo en el Apps Script, deberás crear una **nueva implementación** (no editar la existente) para que los cambios tomen efecto.
