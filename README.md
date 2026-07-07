# Calculadora / Planificador de Teletrabajo · ANECA

Web estática (sin dependencias) para **planificar el año en un calendario** y comprobar el cumplimiento
del teletrabajo en ANECA.

- `index.html` — planificador anual.
- `configuracion.html` — edición de los festivos de Madrid capital.

## Cómo se usa

En el calendario anual marcas, con un pincel y **clic o arrastre** (ratón o dedo, incluso cruzando de un mes a otro):

- **Presencial** (naranja): días en los que vas a la oficina (no teletrabajas).
- **Ausencia** (gris): vacaciones, bajas, permisos, asuntos propios…
- **Teletrabajo** (verde): el resto de días laborables, mostrado automáticamente.
- **Festivo Madrid** (violeta) y **fin de semana**: no computan como jornada.

El **resumen anual** muestra un histograma de teletrabajo frente a presencial por periodo, con la línea
del mínimo exigido, y marca ✓/✗ si cumples en cada periodo.

## Regla que aplica

- Deben ser presenciales **al menos el 40 % de las jornadas efectivas** de cada periodo.
- **Jornadas efectivas** = días laborables (L–V) de **Madrid capital**, menos festivos, menos tus ausencias.
- **Mínimo presencial = ⌊0,40 × jornadas efectivas⌋** → el redondeo favorece al teletrabajo. Cumples si tus
  días presenciales ≥ ese mínimo.
- **Ventanas de cómputo:** cada mes de octubre a mayo por separado; **junio, julio, agosto y septiembre se computan
  juntos** como un único bloque de verano de 4 meses.

## Festivos

Precargados los calendarios oficiales del Ayuntamiento de Madrid para **2025 y 2026**.
Se editan en **`configuracion.html`** (se guardan en el navegador), de modo que añadir años futuros
o corregir cualquier fecha no requiere tocar el código.

## Publicar en GitHub Pages

1. En el repositorio: **Settings → Pages**.
2. En *Build and deployment → Source*, elige **Deploy from a branch**.
3. Selecciona la rama y la carpeta raíz (`/`). Guarda.
4. La página quedará disponible en `https://<usuario>.github.io/<repositorio>/`.

> Herramienta orientativa. Ante cualquier duda, prevalece la normativa e instrucciones oficiales de ANECA.
