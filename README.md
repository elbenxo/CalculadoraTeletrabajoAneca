# Calculadora de Teletrabajo · ANECA

Página web (una sola `index.html`, sin dependencias) que calcula, para un periodo dado,
los **días presenciales obligatorios** y los **días de teletrabajo disponibles** en ANECA.

## Regla que aplica

- Deben ser presenciales **el 40 % de las jornadas efectivas** del periodo.
- **Jornadas efectivas** = días laborables (L–V) de **Madrid capital**, menos festivos, menos tus ausencias
  (vacaciones, bajas, permisos, asuntos propios…). Se computa sobre días realmente trabajados, no sobre el calendario.
- **Presencial obligatorio = ⌊0,40 × jornadas efectivas⌋** → el redondeo siempre favorece al teletrabajo.
- **Ventanas de cómputo:** cada mes de octubre a mayo por separado; **junio, julio, agosto y septiembre se computan
  juntos** como un único bloque de verano de 4 meses.

## Festivos

Precargados los calendarios oficiales del Ayuntamiento de Madrid para **2025 y 2026**.
Son **editables desde la propia página** (se guardan en el navegador), de modo que añadir años futuros
o corregir cualquier fecha no requiere tocar el código.

## Publicar en GitHub Pages

1. En el repositorio: **Settings → Pages**.
2. En *Build and deployment → Source*, elige **Deploy from a branch**.
3. Selecciona la rama y la carpeta raíz (`/`). Guarda.
4. La página quedará disponible en `https://<usuario>.github.io/<repositorio>/`.

> Herramienta orientativa. Ante cualquier duda, prevalece la normativa e instrucciones oficiales de ANECA.
