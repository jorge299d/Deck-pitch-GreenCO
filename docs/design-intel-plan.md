# Plan — Design Intel Engine: extraer, aprender y superar

## Contexto

Existe un grupo de diseñadores y estudios cuyo nivel de ejecución — UI/UX, logos,
brand kits, tipografía propia, motion, sistemas visuales — es el estándar al que el
usuario quiere llegar y luego superar.

**La prioridad, en palabras del usuario:** que yo (el agente) sea capaz de *crear al
mismo nivel o mejor* — UI, UX, logos, brand kits, creatividad, creación de fonts,
estilos únicos, patrones. El mecanismo es la extracción **semanal y automatizada** de
su trabajo, convertida en capacidad instalada que mejora en cada pasada. El análisis
de redes, copy, venta y PR viene después.

### Nota honesta sobre "entrenarte a ti mismo"

No puedo modificar mis pesos. Lo que sí funciona y es lo que construye este plan:
convertir la extracción en **artefactos que yo cargo y ejecuto** — un Skill de diseño
con reglas duras, una biblioteca de patrones con números concretos (escalas, ratios,
curvas de easing, grids), tokens listos para usar, y un **loop de autoevaluación**
donde produzco una pieza, la mido contra las referencias con métricas objetivas,
detecto la brecha y corrijo el Skill. Eso es mejora real y medible pasada tras pasada.

### Sobre la copia

Todo asset de terceros se guarda como **referencia con atribución de fuente** en
`store/raw/`, que nunca se publica ni se commitea. Los entregables se generan desde
`library/SYSTEM.md` — principios y parámetros destilados — no desde los archivos
originales. Un logo ajeno es dato de estudio, nunca output. La métrica de originalidad
en `eval/rubric.md` penaliza explícitamente parecerse demasiado a una referencia.

---

## ⚠️ Restricción de red descubierta (define dónde corre el sistema)

Verificado en esta sesión: el proxy del entorno remoto rechaza la conexión a los
dominios objetivo.

```
403 CONNECT (policy denial) → matesha.studio, nor.ma, gola.supply,
                              studio.morflax.com, www.jckhlry.com, yahyavision.com
```

No es bloqueo de los sitios: es la **política de red del entorno de Claude Code on the
web** (ver https://code.claude.com/docs/en/claude-code-on-the-web). Consecuencia:

- **El código se escribe aquí** (esto funciona sin problema) y se pushea al repo.
- **La ingesta se ejecuta donde haya red abierta**: tu máquina local con Claude Code
  CLI, o un entorno remoto nuevo creado con política de red sin restricciones.
- El pipeline se diseña **portable y sin estado de sesión en el código** para que
  correrlo local sea `pnpm ingest` y nada más.

---

## Decisiones tomadas

| Tema | Decisión |
|---|---|
| Acceso a X | Playwright + Chromium con `storageState` autenticado del usuario |
| Ubicación | **Repo nuevo dedicado**: `design-intel-engine` |
| Cadencia | Extracción **semanal** |
| Prioridad | Capacidad de diseño primero; copy/venta/PR en F7 |
| Ejecución | Código aquí; ingesta en entorno con red abierta (ver arriba) |

`Deck-pitch-GreenCO` solo tiene `README.md` + un `index.html` monolítico del pitch deck.
No aporta nada reutilizable; el sistema se construye desde cero.

---

## Targets (la data entregada)

### Tier A — perfil de X + presencia propia → pipeline completo

| X | Sitio / portafolio | Notas |
|---|---|---|
| `@thetimgabe` | — | solo X |
| `@designbyzipzap` | `strategy.zipzap.design` | el sitio es de *estrategia* → estudiar cómo empaquetan y venden |
| `@yahyavision` | `yahyavision.com` + `cal.com/yahyavision/30min` | funnel completo y visible: perfil → sitio → booking |
| `@ktnr23` | `dribbble.com/ktnr` | portafolio en Dribbble |
| `@gola99` | `gola.supply` | |
| `@driceroland` | `nor.ma` (verificar autoría) | |
| `@kyleanthony` | — | solo X |
| `@davidmokos_` | `steercode.com` | producto/dev-tool más que estudio → referencia de UI de producto |

**Seed posts** que marcaste explícitamente (entran como ejemplos anclados de "esto sí"):
- `x.com/yahyavision/status/2080987176305926611`
- `x.com/ktnr23/status/2068241431504798152`

### Tier B — estudios/sitios sin X asociado → pipeline web + visual

- `matesha.studio`
- `studio.morflax.com` — motion/3D, alto valor para `extract/motion`
- `jckhlry.com` (entregado como `/contact` → también sirve para analizar el cierre de venta)
- `nor.ma`

### Tier C — fuentes de patrón y de modelo de negocio, no de emulación directa

- `the-brandidentity.com` — publicación editorial de branding. **La mina más rica del
  lote** para corpus de identidad: cientos de casos con rationale escrito.
- `dribbble.com/KateHets` — portafolio.
- `framer.com/community/marketplace/templates/kanso` — template comercial: estudiar
  cómo se estructura y se vende un producto de diseño empaquetado.
- `amirmushich.gumroad.com` — productos digitales: modelo de monetización.
- `moda.app/s/Opm4dJfiGiW-JRJ6AeQk3A` — link compartido; se clasifica al abrirlo.

**Piloto propuesto: `@yahyavision`** — es el único que tiene la cadena entera visible
(X + sitio propio + booking + post destacado), así que valida las tres capas del
pipeline de una sola vez.

---

## Arquitectura

```
design-intel-engine/
  targets.yaml              # los targets de arriba, con tier, prioridad y tags
  .env.example              # ruta al storageState de Playwright (nunca se commitea)

  ingest/
    x_profile.ts            # posts, media full-res, timestamps, métricas, replies
    website.ts              # HTML + CSS computado + webfonts + screenshots
                            #   (desktop/mobile, estados hover/scroll, frames de animación)
    portfolio.ts            # Dribbble / Behance / Gumroad / the-brandidentity
    manifest.ts             # cada asset con url origen, autor, fecha, hash

  extract/
    palette.ts              # colores dominantes + ratios de uso + contraste + OKLCH
    typography.ts           # familias, escala modular, tracking, leading, optical sizing
    layout.ts               # grid, columnas, spacing scale, radios, sombras, densidad
    motion.ts               # duraciones, easings reales del CSS/video, stagger
    logo.ts                 # construcción de marca, geometría base, grosores, terminales
    typeface.ts             # métricas de fuentes propias: x-height, contraste, anchos

  library/                  # ← EL PRODUCTO
    <handle>/DOSSIER.md     # ficha por estudio, con números
    PATTERNS.md             # patrones transversales con evidencia y frecuencia
    SYSTEM.md               # nuestro sistema derivado: tokens, reglas, do/don't
    tokens.json             # paleta, escala, spacing, easings — importables
    CHANGELOG.md            # qué aprendió el sistema en cada pasada semanal

  skill/design-master/      # Claude Code Skill autogenerado desde library/
    SKILL.md                # reglas duras que cargo al diseñar
    references/

  eval/
    briefs/                 # landing, identidad, poster, UI kit, especimen tipográfico
    run.ts                  # produzco → se mide → se compara
    rubric.md               # métricas objetivas + originalidad
    results/                # histórico semanal de scores

  ops/
    weekly.ts               # orquestador de la pasada semanal
    diff.ts                 # qué cambió desde la última pasada
```

---

## Fases

**F0 — Bootstrap.** Crear `design-intel-engine`: scaffolding TypeScript + Playwright,
`targets.yaml` poblado con la tabla de arriba, `.gitignore` excluyendo `store/raw/`,
`.env` y `storageState.json`. Script `pnpm doctor` que verifica red y sesión de X antes
de intentar cualquier ingesta.

**F1 — Ingesta piloto sobre `@yahyavision`.** Correr `x_profile` + `website` sobre un
solo target y validar media full-res, screenshots y manifest antes de escalar. Rate
limiting y backoff desde el día uno.

**F2 — Extracción visual del piloto.** Salida: `DOSSIER.md` con números, no adjetivos.
"Escala 1.25, base 16px, tracking −0.02em en display, easing `cubic-bezier(.2,.8,.2,1)`
a 420ms" sirve; "estilo minimalista limpio" no.

**F3 — Escalar a Tier A y B**, más barrido de `the-brandidentity.com` como corpus de
identidad. Generar `PATTERNS.md`: qué comparten, qué los diferencia, qué es tendencia
y qué es firma personal.

**F4 — Síntesis.** `SYSTEM.md` + `tokens.json` + compilar `skill/design-master/SKILL.md`
para que yo lo cargue automáticamente al diseñar.

**F5 — Loop de autoevaluación (el corazón).** Contra cada brief produzco una pieza con
el Skill. Se mide: consistencia de tokens, contraste AA/AAA, ritmo de la escala
tipográfica, coherencia de motion, densidad de layout y **originalidad** (distancia a
las referencias — parecerse demasiado es falla, no éxito). El gap se escribe de vuelta
al Skill.

**F6 — Automatización semanal.** Routine que corre `ops/weekly.ts`: re-ingesta → diff →
actualiza library → re-corre evals → `CHANGELOG.md`. Avisa si el score baja o aparece
un patrón nuevo.

**F7 — Fase 2 (después).** Copy, cadencia, replies, tiempos de respuesta, funnel de
venta y PR. `cal.com/yahyavision`, `jckhlry.com/contact`, la Gumroad de Amir Mushich y
el template de Framer son el material base. No se construye hasta que F5 dé resultados.

---

## Verificación

- **F1:** `store/raw/yahyavision/` con ≥50 piezas de media a resolución original +
  screenshots, todas con entrada en el manifest.
- **F2:** el `DOSSIER.md` es lo bastante específico para reconstruir una pieza suya sin
  mirarla. Prueba concreta: reconstruyo un layout desde el dossier y se compara lado a lado.
- **F4:** `tokens.json` se aplica a un HTML de prueba sin edición manual.
- **F5:** la primera corrida fija la línea base. Éxito = el score sube entre semanas y
  la métrica de originalidad se mantiene alta (aprender ≠ clonar).
- **F6:** una corrida manual de `weekly.ts` completa el ciclo y deja entrada nueva en
  `CHANGELOG.md`.

---

## Lo que necesito de ti

1. **Dónde corre la ingesta** — tu máquina local, o creas un entorno remoto con red
   abierta. (Ver la restricción de red arriba.)
2. **Sesión de X** — el `storageState.json` de Playwright. Te doy el comando para
   generarlo desde tu navegador sin que me pases contraseña.
3. **El repo** — si creas `design-intel-engine` o me autorizas a crearlo.
