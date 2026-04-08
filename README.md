```markdown
# Infografía Interactiva de Lista — Experiencias de Madres

Infografía web interactiva que presenta cinco hallazgos clave de la investigación de Daghustani y MacKenzie (2022) sobre las experiencias de madres con hijos adolescentes autistas en sociedades conservadoras.

## Descripción del Proyecto

Documento HTML único que transforma una lista de cinco puntos en una experiencia visual interactiva mediante dos efectos CSS principales:

- **Efecto de desplazamiento del ícono (#18):** Cada marcador muestra inicialmente un semicírculo de fondo. Al pasar el cursor sobre el ítem, el segundo semicírculo rota desde una posición oculta hasta completar un círculo completo detrás del emoji.
- **Línea de menú ondulada (#24):** Al activar el hover, se revela una línea ondulada animada debajo del título del ítem. La onda se desplaza horizontalmente de forma continua usando un SVG codificado en base64 como imagen de fondo.

## Estructura HTML

```
<body>
  <main class="contenedor-principal">
    <header class="encabezado">        ← Título y subtítulo
    <ol class="lista-infografia">       ← Lista semántica ordenada
      <li>                              ← Cada ítem con dos columnas
        <div class="icono-contenedor">  ← Columna izquierda: número + emoji
        <div class="texto-contenedor">  ← Columna derecha: título + descripción
      </li>
      × 5
    </ol>
    <footer>                            ← Fuente y autoría
  </main>
</body>
```

Se utilizó `<ol>` en lugar de `<ul>` por tratarse de una secuencia numerada con jerarquía temática. Los números visuales se implementan manualmente mediante `<span class="numero">` para mantener control total sobre su posicionamiento dentro del ícono circular, independientemente del estilo nativo del contador de la lista.

## Arquitectura CSS

### Variables personalizadas (`:root`)

Todas las decisiones de color, ritmo de animación y dimensiones se centralizan en variables CSS para facilitar la personalización:

| Variable | Valor por defecto | Función |
|---|---|---|
| `--color-primario` | `#1F3A5F` | Color oscuro para textos, números y fondo del ícono al hover |
| `--color-secundario` | `#4E8098` | Color medio para semicírculos iniciales, onda y acentos |
| `--color-fondo` | `#EAEFF2` | Fondo general del documento |
| `--velocidad-rotacion` | `0.5s` | Duración de la rotación del semicírculo móvil |
| `--velocidad-onda` | `1.5s` | Duración de un ciclo completo de desplazamiento de la onda |

### Efecto del ícono (semicírculos)

El fondo circular del ícono se construye sin elementos adicionales en el HTML, usando únicamente pseudoelementos sobre `.icono-contenedor`:

- `::before` — Semicírculo fijo (mitad izquierda), siempre visible con `--color-secundario`.
- `::after` — Semicírculo móvil (mitad derecha), inicialmente rotado `-90deg` con `opacity: 0`. Al hacer `:hover` sobre el `<li>` padre, se transforma a `rotate(0deg)` con `opacity: 1`.

Ambos cambian a `--color-primario` durante el hover para generar un contraste de color que refuerza visualmente el estado activo. El emoji escala a `1.15` simultáneamente.

### Efecto de la onda (SVG base64)

La línea ondulada es un `<path>` SVG sinusoidal de 4 ciclos codificado en base64, aplicado como `background-image` sobre `h2::after`. La animación `@keyframes onda-deslizante` desplaza `background-position` de `0` a `200px` (un ciclo completo del patrón) en bucle infinito, garantizando una transición sin saltos.

La revelación usa `transform: scaleX(0)` → `scaleX(1)` con `transform-origin: left center` para un efecto de "dibujo" desde la izquierda.

### Línea lateral animada

Cada `<li>` tiene un `::before` adicional (línea vertical de 4px a la izquierda) que escala de `scaleY(0)` a `scaleY(1)` al hover, cambiando su `transform-origin` de `bottom` a `top` para crear un efecto de "crecimiento hacia arriba".

## Estrategia Responsive (Mobile First)

El CSS se escribe desde el breakpoint más pequeño y se amplía progresivamente:

| Breakpoint | Dispositivo objetivo | Ajustes principales |
|---|---|---|
| Base (0px) | Móvil | Ícono de 64px, padding compacto, tipografía reducida |
| `≤ 400px` | Móvil pequeño | Ícono de 52px, padding mínimo, onda de 4px de altura |
| `≥ 600px` | Tablet | Ícono de 72px, más espaciado, tipografía amplificada |
| `≥ 900px` | Escritorio | Máximo espaciado, onda de 7px, footer expandido |

El área táctil de cada ítem supera los 48px de alto en todos los breakpoints, cumpliendo con las pautas de accesibilidad WCAG para objetivos táctiles.

## Personalización

### Cambiar la onda ondulada

1. Diseña tu SVG ondulado (ancho sugerido: 200px, altura: 6–7px).
2. Codifícalo a base64 usando cualquier herramienta online.
3. Reemplaza la cadena dentro de `url("data:image/svg+xml;base64,...")` en la regla `.texto-contenedor h2::after`.
4. Ajusta `background-size` para que coincida con las dimensiones de tu SVG.
5. Ajusta el valor final de `background-position` en `@keyframes onda-deslizante` para que coincida con el ancho de tu patrón.

### Cambiar colores

Modifica las tres variables de color en `:root`. Los colores se aplican de forma cascada a todos los elementos: fondo, textos, sombras, íconos, onda y pie de página.

### Cambiar velocidad de animaciones

- Rotación del ícono: modifica `--velocidad-rotacion` (valores entre `0.3s` y `0.8s` recomendados).
- Desplazamiento de la onda: modifica `--velocidad-onda` (valores entre `0.8s` y `3s` recomendados).

### Cambiar íconos o emojis

Sustituye el texto dentro de cada `<span class="emoji">` por cualquier otro emoji o caracter Unicode. No es necesario ajustar CSS.

## Contenido

Los cinco puntos representan hallazgos del artículo:

> Daghustani, W., y MacKenzie, A. (2022). Stigma, gendered care work and sex education: Mothers' experiences of raising autistic adolescents sons in socially conservative societies. *International Journal of Educational Research Open*, 3, 100215. https://doi.org/10.1016/j.ijedro.2022.100215

## Restricciones técnicas cumplidas

- HTML semántico sin atributos de presentación.
- CSS puro (Vanilla CSS) sin preprocesadores, frameworks ni JavaScript.
- Transiciones unificadas con `transition: ... 0.4s ease-in-out`.
- Respeta `prefers-reduced-motion: reduce` desactivando animaciones y transiciones.
- Documento HTML único, autocontenido, sin dependencias externas.

## Uso

Abrir el archivo `.html` directamente en cualquier navegador moderno. No requiere servidor, compilación ni conexión a internet.
```