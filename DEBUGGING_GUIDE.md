# 🔍 Guía de Debugging - Directorio ZRCPT

## Problema: No muestra puntos al abrir

### Paso 1: Abre la consola del navegador
- **Chrome/Edge**: `F12` o `Ctrl+Shift+I`
- **Firefox**: `F12`
- **Safari**: `Cmd+Option+I`

Haz clic en la pestaña **"Console"**

### Paso 2: Busca estos mensajes en la consola

**Si ves esto ✅ BIEN**:
```
🚀 Iniciando aplicación...
🗺️ Mapa inicializado
🔍 Cargando datos desde: https://script.google.com/macros/s/...
📊 Datos recibidos: [número] registros
📝 Ejemplo de datos: {objeto con propiedades}
✅ Sitios válidos: [número]
🎯 Mostrando todos los puntos: [número]
✅ Marcadores actualizados: [número]
```

**Si ves esto ❌ PROBLEMA**:
```
❌ Error al cargar datos: [error message]
⚠️ No hay datos en la hoja
```

### Paso 3: Verifica el tipo de error

| Error | Solución |
|-------|----------|
| `Error HTTP: 404` | El URL_SHEETS no existe. Verifica que sea válido |
| `Error HTTP: 403` | No tienes permisos. Verifica que el Google Apps Script sea público |
| `TypeError: Cannot read property` | El Google Sheets no tiene las columnas esperadas |
| `No hay datos en la hoja` | El Sheets está vacío o no tiene filas de datos |

### Paso 4: Verifica el contenido de los datos

En la consola, escribe:
```javascript
sitios
```

Presiona Enter. Deberías ver un array con objetos como:
```javascript
[
  {
    id: "sitio-0",
    ruta: "Nombre del sitio",
    comunidad: "Nombre comunidad",
    lat: 19.123,
    lng: -88.456,
    tipo_experiencia: "Tipo",
    imagen: "https://drive.google.com/file/d/ID/view",
    ...
  }
]
```

### Paso 5: Verifica las imágenes

En la consola, escribe:
```javascript
sitios[0].imagen
```

Deberías ver una URL como:
```
https://drive.google.com/file/d/1YPK8DzqGFXhZvuvkiJ_ilVknLBYxjUL4/view
```

Si es `null` o vacío, el campo "ENLACE FOTO" en tu Sheets no tiene datos.

---

## Problema: Los filtros no funcionan

En la consola, cuando hagas clic en un checkbox, deberías ver:
```
✅ Filtro cambiado, aplicando...
🔍 Filtros aplicados: { comunidades: 1, tipos: 1, resultado: 3 }
🔄 Actualizando marcadores...
✅ Marcadores actualizados: 3
```

---

## Problema: Las imágenes no abren

### Verifica en la consola:
```javascript
sitios.map(s => ({ ruta: s.ruta, imagen: s.imagen }))
```

Esto te mostrará todos los sitios con sus URLs de imagen. Verifica que sean URLs válidas de Google Drive.

### Prueba una URL manualmente:
Copia una URL de la consola y abrela en el navegador. Si funciona en el navegador, funcionará en el popup.

---

## Checklist de validación

- [ ] La consola no muestra errores
- [ ] El mensaje "Mostrando todos los puntos: X" aparece
- [ ] Ves puntos/marcadores en el mapa
- [ ] Los filtros cambian la cantidad de puntos
- [ ] Al clickear "Ver imagen en Drive" abre la imagen

Si algo falla, **copia todos los mensajes de error de la consola** y comparte.

---

## Tips útiles

1. **Busca en la consola**: Usa `Ctrl+F` dentro de la consola para buscar errores

2. **Recarga sin caché**: `Ctrl+Shift+R` (Windows) o `Cmd+Shift+R` (Mac)

3. **Copia todo**: Click derecho en la consola → "Save as..." para guardar los logs

4. **Prueba en Chrome**: A veces funciona mejor en Chrome que en otros navegadores
