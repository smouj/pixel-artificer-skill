description: Esculpe obras maestras retro-pixel a partir de bocetos conceptuales
tags: diseño, píxeles, arte, retro, creatividad
version: 1.2.3
author: Equipo OpenClaw
license: MIT
dependencies:
  - imagemagick: Requerido para procesamiento y conversión de imágenes
  - python-pillow: Para manipulación avanzada de píxeles
  - pixel-art-ai: Biblioteca personalizada para generación asistida por IA de píxeles
environment_variables:
  - PIXEL_ART_STYLE: Estilo por defecto (ej. "nes", "gba", "arcade")
  - PIXEL_ART_SIZE: Tamaño de lienzo por defecto (ej. "16x16", "32x32")
  - PIXEL_ART_COLOR_PALETTE: Ruta del archivo de paleta de colores por defecto
requires_confirmation: false
rollback_available: true
---

# Pixel Artificer Skill

## Propósito

Pixel Artificer es una habilidad especializada diseñada para transformar ideas conceptuales, bocetos o descripciones de texto en obras maestras de arte pixel retro de alta calidad. Se destaca en la creación de sprites, tilesets y animaciones pixel-perfect para juegos, elementos de UI y arte digital. La habilidad conecta conceptos creativos con precisión técnica, permitiendo prototipado rápido de activos de arte pixel a partir de prompts simples o bocetos subidos.

### Casos de Uso Reales

- **Desarrollo de Juegos**: Generar sprites de personajes (ej. un caballero pixel con espada) para juegos indie en estilo NES o GBA.
- **Diseño de UI**: Crear iconos y botones con tema retro para apps o sitios web, como un icono de disquete en estilo 8-bit.
- **Prototipado de Animaciones**: Producir animaciones frame-by-frame, como un ciclo de caminata para un personaje pixel.
- **Creación de Activos**: Construir tilesets para plataformas, incluyendo hierba, ladrillos y nubes en paletas de colores específicas.
- **Visualización de Conceptos**: Convertir bocetos aproximados en arte pixel pulido para revisiones de arte conceptual.

## Alcance

Pixel Artificer maneja exclusivamente tareas de creación de arte pixel. No realiza edición general de imágenes, modelado 3D o tareas no relacionadas con arte pixel.

### Comandos Específicos

- `/pixel generate <prompt> [--style <style>] [--size <width>x<height>] [--palette <file>] [--animate <frames>] [--output <file>]`: Generar arte pixel a partir de prompt de texto.
- `/pixel sketch <image_path> [--style <style>] [--refine] [--output <file>]`: Convertir boceto subido a arte pixel.
- `/pixel tileset <theme> [--count <n>] [--size <width>x<height>] [--output <directory>]`: Crear un tileset de activos pixel relacionados.
- `/pixel palette <image_path> [--extract] [--apply-to <existing_art>] [--output <file>]`: Extraer o aplicar paletas de colores.
- `/pixel animate <frames> [--fps <rate>] [--loop] [--output <gif>]`: Combinar frames generados en animación.
- `/pixel verify <file> [--check-size] [--check-palette] [--preview]`: Validar calidad y especificaciones del arte pixel.

### Banderas y Opciones

- `--style`: Estilos soportados incluyen "nes" (paleta NES, máximo 16x16), "gba" (colores GBA, hasta 32x32), "arcade" (colores brillantes, tamaño variable), "custom" (paleta definida por usuario).
- `--size`: Dimensiones en píxeles (ej. "16x16", "64x32"). Deben ser múltiplos de 8 para autenticidad retro.
- `--palette`: Ruta a un archivo JSON que define colores (ej. `{"background": "#000000", "primary": "#FF0000"}`).
- `--animate`: Número de frames para animación (máximo 16 por rendimiento).
- `--fps`: Velocidad de animación en frames por segundo (por defecto 12).
- `--refine`: Habilitar refinamiento por IA en bocetos para mejor detección de bordes.
- `--extract`: Extraer colores dominantes de una imagen en un archivo de paleta.
- `--check-size`: Verificar que las dimensiones coincidan con especificaciones.
- `--check-palette`: Asegurar que los colores cumplan con restricciones retro (máximo 16 colores).
- `--preview`: Mostrar vista previa ASCII en terminal.

## Proceso de Trabajo

Pixel Artificer sigue un flujo de trabajo estructurado para asegurar una salida consistente y de alta calidad. El proceso se adapta según el tipo de entrada (prompt de texto vs. boceto).

### Pasos Detallados para Generación Basada en Texto

1. **Análisis de Prompt**: Analizar el prompt de entrada para elementos clave (sujeto, estilo, colores, acciones). Ej. "dragón rojo respirando fuego" extrae "dragón", "rojo", "respirando fuego".
2. **Aplicación de Estilo**: Aplicar las restricciones del estilo especificado (paleta de colores, densidad de píxeles).
3. **Generación por IA**: Usar modelos de IA integrados para esbozar el diseño inicial de píxeles.
4. **Bucle de Refinamiento**: Refinar iterativamente los píxeles para nitidez y autenticidad retro (hasta 3 pasadas).
5. **Cuantización de Color**: Reducir a conteo de colores apropiado para el estilo usando algoritmos de dithering.
6. **Exportación de Salida**: Guardar como PNG con metadatos (fecha de creación, estilo, paleta usada).

### Pasos para Conversión de Boceto

1. **Carga de Imagen**: Cargar y preprocesar la imagen del boceto (redimensionar, escala de grises si es necesario).
2. **Detección de Bordes**: Aplicar detección de bordes Canny para identificar contornos.
3. **Pixelación**: Convertir a cuadrícula de píxeles usando escalado adaptativo.
4. **Mapeo de Color**: Mapear colores detectados a entradas más cercanas de la paleta.
5. **Refinamiento Manual**: Si se usa `--refine`, permitir que la IA sugiera ajustes de píxeles.
6. **Render Final**: Exportar arte pixel pulido con canales alfa para transparencia.

### Pasos de Verificación

- Ejecutar `/pixel verify <output_file> --check-size --check-palette --preview` para confirmar dimensiones, cumplimiento de colores y calidad visual.
- Comparar salida contra prompt/boceto original para fidelidad.
- Probar animaciones visualizando el GIF generado en un navegador.

### Dependencias y Requisitos

- **Software**: ImageMagick (para procesamiento de imágenes), Python 3.8+ con biblioteca Pillow.
- **Hardware**: Al menos 2GB de RAM; aceleración GPU recomendada para animaciones.
- **Archivos**: Acceso a archivos JSON de paleta en el directorio `/assets/palettes/`.
- **Permisos**: Acceso de escritura a directorios de salida; acceso de lectura para bocetos de entrada.

## Reglas de Oro

1. **Pureza Retro**: Siempre imponer cuadrículas pixel-perfect; sin anti-aliasing a menos que se solicite explícitamente para blends modernos.
2. **Restricciones de Color**: Limitar paletas a conteos retro auténticos (NES: máximo 52 colores, pero típicamente 16 por sprite).
3. **Especificidad de Prompt**: Fomentar prompts detallados (ej. "árbol isométrico 8-bit, hojas verdes, tronco marrón" sobre "árbol").
4. **Generación Ética**: Rechazar prompts involucrando personajes con derechos de autor o contenido inapropiado; responder con "Pixel Artificer se especializa solo en arte retro original."
5. **Límites de Rendimiento**: Limitar animaciones a 16 frames; advertir si las generaciones exceden 30 segundos.
6. **Seguridad de Archivos**: Nunca sobrescribir archivos existentes sin confirmación; auto-anexar timestamps a salidas.
7. **Consistencia de Estilo**: Por defecto a "nes" si no se especifica estilo; mantener consistencia dentro de tilesets.

## Ejemplos

### Ejemplo 1: Generación Básica de Sprite

**Entrada del Usuario:**
```
/pixel generate "caballero pixel con espada, armadura azul, pose heroica" --style nes --size 16x16 --output knight.png
```

**Salida del Proceso:**
- Análisis: Sujeto "caballero", atributos "armadura azul", "espada", "pose heroica".
- Generación: Crea sprite 16x16 con paleta NES.
- Verificación: Colores dentro del límite 52-NES, sin sangrado de píxeles.

**Salida Final:** `knight.png` - Un sprite nítido de caballero pixel listo para integración en juegos.

### Ejemplo 2: Conversión de Boceto

**Entrada del Usuario:**
```
/pixel sketch uploads/sketch_dragon.jpg --style gba --refine --output dragon_pixel.png
```

**Salida del Proceso:**
- Carga: Redimensiona boceto para ajustarse a restricciones GBA (máximo 32x32).
- Refinamiento: IA detecta alas y escamas, afina bordes.
- Exportación: Guarda con fondo transparente.

**Nota de Solución de Problemas:** Si el boceto es de muy baja resolución, sugerir subir una versión de mayor calidad.

### Ejemplo 3: Creación de Tileset

**Entrada del Usuario:**
```
/pixel tileset "bosque" --count 8 --size 16x16 --palette palettes/nature.json --output tiles/forest/
```

**Salida:** Genera 8 tiles de bosque (hierba, árboles, rocas) en estilo coincidente, guardados como tile01.png a tile08.png.

### Ejemplo 4: Animación

**Entrada del Usuario:**
```
/pixel generate "gato corriendo" --style arcade --animate 4 --fps 15 --output cat_run.gif
```

**Salida:** Animación de 4 frames en bucle de gato corriendo.

## Comandos de Rollback

- `/pixel rollback <file>`: Elimina el archivo generado especificado y lo remueve del historial.
- `/pixel undo --last`: Revierte la generación más reciente, restaurando cualquier archivo sobrescrito.
- `/pixel purge --all`: Limpia todos los activos generados en el directorio de salida (requiere confirmación).
- `/pixel revert <version>`: Si el versionado está habilitado, restaurar una versión previa de un archivo de arte pixel.

### Verificación de Rollback

Después del rollback, ejecutar `/pixel verify --list` para confirmar eliminación de archivo y verificar archivos temporales huérfanos.

## Solución de Problemas

- **Falla la Generación**: Verificar dependencias (`convert --version` para ImageMagick); asegurar que el archivo de paleta exista.
- **Colores Parecen Incorrectos**: Verificar sintaxis JSON de paleta; usar `--check-palette` en entrada.
- **Animaciones Lentas**: Reducir conteo de frames o FPS; verificar uso de RAM.
- **Bocetos No Se Convierten**: Asegurar imagen en JPG/PNG; intentar `--refine` para bocetos complejos.
- **Salida Demasiado Grande**: Redimensionar entrada o especificar `--size` menor; comprimir con ImageMagick post-generación.