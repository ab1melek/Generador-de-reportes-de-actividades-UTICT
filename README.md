# Generador de Reportes de Actividades

Herramienta para generar reportes de actividades en formato Word (.docx) a partir de Merge Requests de GitLab.

## Inicio Rápido (⚡ 3 pasos)

Si ya bajaste el repositorio, solo necesitas:

```bash
# Paso 1: Entra a la carpeta
cd reportes

# Paso 2: Activa el entorno virtual Python (ya está incluido)
source .venv/bin/activate

# Paso 3: ¡Ejecuta el script!
python3 reporte_mr.py
```

**¡Listo!** Los reportes `.docx` aparecerán en la carpeta actual.

> **⚠️ IMPORTANTE:** Debes configurar el archivo `.env` primero (ver sección "Configuración" más abajo)

## Requisitos previos

- **Python 3.12+** (revisa con `python3 --version`)
- **Token de acceso a GitLab**

## Instalación Completa

Si es la primera vez o necesitas reinstalar:

### Paso 1: Clonar o descargar el repositorio

```bash
# OPCIÓN A: Clonar con Git
git clone <url-del-repo>
cd reportes

# OPCIÓN B: Descargar como ZIP
# 1. Descarga el ZIP desde GitHub
# 2. Descomprime la carpeta
# 3. Abre terminal y entra en la carpeta: cd reportes
```

### Paso 2: Activar el entorno virtual (SIEMPRE es necesario)

El entorno virtual `.venv` está incluido en el repositorio. Solo necesitas **activarlo cada vez que uses el script**:

```bash
# En macOS o Linux:
source .venv/bin/activate

# En Windows:
.venv\Scripts\activate
```

Si ves `(.venv)` al inicio de tu terminal, ¡está activado! 

Ejemplo:
```
(.venv) usuario@PC reportes %
```

### Paso 3: Instalar dependencias (solo la primera vez)

Una vez activado el entorno virtual:

```bash
pip install -r requirements.txt
```

Esto instala:
- `requests` - para conectar con GitLab
- `python-docx` - para generar documentos Word
- `python-dotenv` - para leer variables de entorno

## Configuración

### 1️⃣ Configurar variables de entorno (.env)

**ANTES de ejecutar el script**, debes configurar tus datos:

#### Paso A: Copia el archivo de ejemplo

```bash
cp .env.example .env
```

#### Paso B: Edita `.env` con tus valores

Abre `.env` en tu editor de texto favorito y rellena:

```env
# Tu token de acceso personal de GitLab
GITLAB_TOKEN=glpat-tu_token_aqui

# Tu ID de usuario en GitLab (ej: 155)
GITLAB_AUTHOR_ID=155

# Año para el reporte
REPORT_YEAR=2026

# Mes inicial (1=enero, 12=diciembre)
REPORT_START_MONTH=1

# Mes final (si es igual al inicio, genera solo 1 mes)
REPORT_END_MONTH=3

# Tu nombre completo
USER_NAME=Tu Nombre Completo

# Descripción de tus actividades (puedes copiar del documento anterior)
ACTIVIDADES_CONTRATACION=DESCRIPCCIÓN DE LAS ACTIVIDADES...
```

#### Tabla de variables

| Variable | Obligatorio | Descripción | Ejemplo |
|----------|:---:|-------------|---------|
| `GITLAB_TOKEN` | ✅ | Token personal de GitLab | `glpat-12345...` |
| `GITLAB_AUTHOR_ID` | ✅ | Tu ID en GitLab | `155` |
| `REPORT_YEAR` | ✅ | Año del reporte | `2026` |
| `REPORT_START_MONTH` | ✅ | Mes inicial (1-12) | `1` |
| `REPORT_END_MONTH` | ✅ | Mes final (1-12) | `3` |
| `USER_NAME` | ✅ | Tu nombre completo | `Juan Pérez` |
| `ACTIVIDADES_CONTRATACION` | ✅ | Descripción de actividades | Texto libre |

### 2️⃣ Agregar tu firma (opcional)

Si quieres que aparezca tu firma digital en el reporte:

1. Coloca un archivo `firma.png` en la carpeta raíz
2. Debe estar sin fondo (fondo transparente)

### 3️⃣ Agregar el encabezado (opcional)

Si quieres un encabezado personalizado:

1. Coloca un archivo `header.png` en la carpeta raíz  
2. Ancho recomendado: 6.7 pulgadas

## Uso Diario

Cada vez que quieras generar nuevos reportes:

```bash
# 1. Abre terminal en la carpeta del proyecto
cd reportes

# 2. Activa el entorno virtual
source .venv/bin/activate

# 3. (OPCIONAL) Edita .env si cambió algo

# 4. ¡Ejecuta!
python3 reporte_mr.py
```

Los archivos `.docx` aparecerán en la **carpeta actual**:

```
reporte_enero.docx
reporte_febrero.docx
reporte_marzo.docx
```

Ejemplo: El script genera un reporte por cada mes en el rango que especificaste en `.env`.

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
