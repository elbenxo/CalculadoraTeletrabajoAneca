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
- **Teletrabajo obligatorio** (teal): semanas de **Navidad y Semana Santa**; son teletrabajo por norma de ANECA
  y **no computan** (no suben el mínimo presencial ni pueden marcarse como presenciales).
- **Festivo Madrid** (violeta) y **fin de semana**: no computan como jornada.

Además:

- **Plantilla semanal**: eliges los días presenciales de una semana tipo (p. ej. lunes y martes) y se
  prerrellena todo el año, conservando ausencias y semanas obligatorias.
- **Fecha de inicio** (opcional): si te incorporas a mitad de año, los días anteriores quedan fuera y se
  recomputan los periodos del resto del año. Si se deja vacía, se asume el año completo.

El **resumen anual** muestra un histograma de teletrabajo frente a presencial por periodo, con la línea
del mínimo exigido, y marca ✓/✗ si cumples en cada periodo (los periodos anteriores al alta aparecen como «—»).

**Informe PDF**: el botón *Descargar informe (PDF)* abre una vista previa del informe (resumen, tabla por
periodo, rangos de vacaciones/ausencias y detalle mensual día a día) y usa *Guardar como PDF* del navegador,
sin librerías externas. Funciona en escritorio y móvil.

## Regla que aplica

- Deben ser presenciales **al menos el 40 % de las jornadas efectivas** de cada periodo.
- **Jornadas efectivas** = días laborables (L–V) de **Madrid capital**, menos festivos, menos tus ausencias.
- **Mínimo presencial = ⌊0,40 × jornadas efectivas⌋** → el redondeo favorece al teletrabajo. Cumples si tus
  días presenciales ≥ ese mínimo.
- **Ventanas de cómputo:** cada mes de octubre a mayo por separado; **junio, julio, agosto y septiembre se computan
  juntos** como un único bloque de verano de 4 meses.

## Configuración (`configuracion.html`)

- **Festivos de Madrid capital**: precargados los calendarios oficiales del Ayuntamiento de Madrid para
  **2025 y 2026**; editables y ampliables a otros años.
- **Semanas de teletrabajo obligatorio**: Semana Santa se calcula automáticamente (a partir de la Pascua) y
  Navidad viene con rangos por defecto; ambos son ajustables por año.

Todo se guarda en el navegador (localStorage), de modo que añadir años futuros o corregir cualquier fecha
no requiere tocar el código.

## Publicar en GitHub Pages

1. En el repositorio: **Settings → Pages**.
2. En *Build and deployment → Source*, elige **Deploy from a branch**.
3. Selecciona la rama y la carpeta raíz (`/`). Guarda.
4. La página quedará disponible en `https://<usuario>.github.io/<repositorio>/`.

> Herramienta orientativa. Ante cualquier duda, prevalece la normativa e instrucciones oficiales de ANECA.
