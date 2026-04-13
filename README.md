# Generador de Reportes de Actividades

Herramienta para generar reportes de actividades en formato Word (.docx) a partir de Merge Requests de GitLab.

## Requisitos previos

- Python 3.12+
- pip o gestor de paquetes Python
- Token de acceso a GitLab

## Instalación

1. Clona o descarga este repositorio
2. Instala las dependencias:

```bash
pip install -r requirements.txt
```

## Configuración

### 1. Configurar variables de entorno

Copia el archivo `.env.example` a `.env` y rellena los valores:

```bash
cp .env.example .env
```

Edita `.env` con tus valores:

```env
GITLAB_TOKEN=tu_token_aqui
GITLAB_AUTHOR_ID=tu_id_de_autor
REPORT_YEAR=2026
REPORT_START_MONTH=1
REPORT_END_MONTH=3
USER_NAME=Tu Nombre Completo
ACTIVIDADES_CONTRATACION=Descripción de tus actividades de contratación
```

### Variables disponibles

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `GITLAB_TOKEN` | Token de acceso personal a GitLab | `glpat-xxxxx...` |
| `GITLAB_AUTHOR_ID` | ID del autor de los MR | `123` |
| `REPORT_YEAR` | Año del reporte | `2026` |
| `REPORT_START_MONTH` | Mes inicial (1-12) | `1` |
| `REPORT_END_MONTH` | Mes final (1-12) | `3` |
| `USER_NAME` | Tu nombre completo | 
| `ACTIVIDADES_CONTRATACION` | Descripción de actividades | Texto libre |

### 2. Agregar archivos de imagen (opcional)

Coloca los siguientes archivos en la carpeta raíz si deseas incluirlos:

- **`header.png`**: Imagen del encabezado del documento (ancho recomendado: 6.7 pulgadas)
- **`firma.png`**: Tu firma digital (sin fondo - usa `make_signature_transparent.py` para remover el fondo)

### 3. Preparar tu firma (si tienes)

Si tu archivo `firma.png` tiene fondo blanco que deseas remover:

1. Coloca `firma.png` en la carpeta raíz
2. Ejecuta (Python 3.9+):
   ```bash
   python3 -c "from PIL import Image; img = Image.open('firma.png').convert('RGBA'); pixels = [(255, 255, 255, 0) if r > 240 and g > 240 and b > 240 else (r, g, b, a) for r, g, b, a in img.getdata()]; img.putdata(pixels); img.save('firma.png')"
   ```

## Uso

Ejecuta el script:

```bash
python3 reporte_mr.py
```

El script generará un archivo `.docx` por cada mes en el rango especificado. Los archivos se guardarán en tu carpeta `Downloads`:

```
~/Downloads/reporte_enero.docx
~/Downloads/reporte_febrero.docx
~/Downloads/reporte_marzo.docx
```

## Estructura del documento generado

El documento incluye:

1. **Encabezado**: Imagen (si existe `header.png`)
2. **Datos generales**: Nombre, período y actividades de contratación
3. **Tabla de actividades**: Listado de Merge Requests del período
   - No. (número secuencial)
   - PRODUCTO (proyecto de GitLab)
   - ACTIVIDADES PRINCIPALES (título del MR + fechas)
   - ESTATUS (Concluido o En proceso)
4. **Sección de firmas**:
   - Tu firma (con imagen si existe `firma.png`)
   - Lic. Arturo Martínez Alvarado (Supervisor)
   - Mtro. Luis Arturo López Caballero (Director)

## Notas importantes

- El archivo `.env` **no se commitea** (está en `.gitignore` por seguridad)
- Use `.env.example` como referencia para nuevas instalaciones
- Los Merge Requests se filtran por fecha de creación dentro del rango especificado
- El documento usa fuente "Times New Roman" en todo el contenido

## Solución de problemas

### No aparecen Merge Requests
- Verifica que `GITLAB_AUTHOR_ID` sea correcto
- Comprueba que el `GITLAB_TOKEN` tiene permisos de lectura
- Asegúrate de que existen MR en el período especificado

### La firma no aparece
- Verifica que `firma.png` esté en la carpeta raíz
- Intenta visualizar la firma con Excel o Word para confirmar que es un PNG válido
- Asegúrate de que la imagen no está corrupta

### Error de encoding en Windows
- Si obtienes errores de encoding, intenta agregar al inicio del script:
  ```python
  import sys
  sys.stdout.reconfigure(encoding='utf-8')
  ```

## Licencia

Este proyecto es de uso personal.
