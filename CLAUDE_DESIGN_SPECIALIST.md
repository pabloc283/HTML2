# Claude Design — Guía Operativa Personal

> Guía real, sin marketing. Basada en lo que ya tenés instalado (skills del Cowork, memoria con template Antigraviti Neon) y en lo que Claude Code soporta de verdad en abril 2026.

---

## 0. Lo primero que hay que entender

**No sos "desarrollador de sintaxis", sos Ingeniero de Intención.** Tu trabajo es describir *qué* querés y *por qué*, con precisión suficiente para que el modelo no invente. Claude Opus 4.7 (lo que corre esta sesión) tiene 1M de contexto y Adaptive Thinking — aprovechálo dándole restricciones, no pidiéndole "que quede lindo".

**Regla de costo:** prototipá con Sonnet 4.6 (`/model sonnet`), consolidá el resultado final con Opus 4.7. Opus cuesta ~1.6× más input y ~1.6× más output que Sonnet — no lo malgastes en iteraciones triviales.

---

## 1. Tu stack real (lo que YA tenés)

### Skills del Cowork disponibles (sin instalar nada)

| Para esto | Usá este skill |
|---|---|
| Infografías / dictámenes verticales | **`anthropic-skills:visualizador-lectura`** — tu workflow por default |
| PDFs de estudio hyper-visuales | **`anthropic-skills:lossless-study-pdf`** |
| Artifacts HTML elaborados (React/Tailwind/shadcn) | **`anthropic-skills:web-artifacts-builder`** |
| Color/tipografía Anthropic | **`anthropic-skills:brand-guidelines`** |
| Temas pre-hechos para artefactos | **`anthropic-skills:theme-factory`** |
| Crear un skill propio reutilizable | **`anthropic-skills:skill-creator`** |
| Arte generativo p5.js | **`anthropic-skills:algorithmic-art`** |
| UX copy / microcopy | **`design:ux-copy`** |
| Crítica de diseño estructurada | **`design:design-critique`** |
| Audit WCAG 2.1 AA | **`design:accessibility-review`** |
| Handoff a devs | **`design:design-handoff`** |
| Sistemas de diseño (tokens, componentes) | **`design:design-system`** |
| Testing de código | **`engineering:testing-strategy`** |
| Review de seguridad | **`engineering:code-review`** |
| Documentación técnica | **`engineering:documentation`** |
| Organizar memoria persistente | **`anthropic-skills:consolidate-memory`** |

Para invocar cualquiera: escribí `/nombre-del-skill` o decí "usá el skill X". No hay que instalar nada — ya están cargados.

### MCPs conectados (los que veo en sesión)

- **Claude Preview** (`preview_*`) — sandbox para probar HTML en vivo, con screenshots y console logs
- **Claude in Chrome** (`mcp__Claude_in_Chrome__*`) — automatización de navegador real
- **computer-use** — control del escritorio (Mac), screenshots, clicks
- **Canva** (`mcp__b1182322*`) — crear/editar diseños, exportar
- **Gmail / Calendar / Drive** — workflow de entrega
- **Common Room / Apollo / Salesforce** — si hacés sales/research
- **bio-research** — si tocás ciencia (biorxiv, chembl, clinical trials)

### Memoria persistente ya configurada

```
feedback_presentation_style.md → Antigraviti Neon como default
reference_antigraviti_template.md → ruta del template base
```

---

## 2. Workflows reales (no inventados)

### Workflow A — Dictamen / infografía vertical (tu default)

1. Pegame el contenido fuente (texto, PDF, transcripción).
2. Decime `/anthropic-skills:visualizador-lectura` o "armá un dictamen Antigraviti Neon sobre esto".
3. El skill local `antigraviti-neon` (abajo) entra automáticamente: duplica el template, reemplaza bloques, mantiene paleta.
4. Export a PDF: el template ya tiene print CSS con fondo oscuro preservado.

**Comando base:**
```
/antigraviti-neon [título corto] — fuente: [texto o ruta]
```

### Workflow B — Componente HTML/CSS interactivo (tu aprendizaje actual)

1. Describí el componente con restricciones: "botón mobile-first, touch target 44px, solo HTML+CSS, sin JS".
2. Pedí que use **`Claude Preview`** para renderizarlo: "creá el archivo y abrilo en preview".
3. Usá `preview_screenshot` + `preview_console_logs` para validar antes de dar por cerrado.

### Workflow C — Artifact React/Tailwind serio

1. Skill: **`anthropic-skills:web-artifacts-builder`**.
2. Tokens: pedíle que consuma tu tema Antigraviti como design tokens (colores/tipografía del template).
3. Entrega: un solo HTML autocontenido, o bundle `tokens.json + component.tsx + PROMPT.md` si después vas a `/code`.

### Workflow D — Review de seguridad (patrón Human-in-the-loop)

Nunca dejes que un agente aplique cambios criptográficos o de auth automáticamente.

```
/engineering:code-review
→ auditá contra OWASP Top 10
→ devolveme parches propuestos en diff, NO los apliques
→ yo valido uno por uno
```

---

## 3. Prompts de alta precisión (copiar/pegar)

### HTML/CSS mobile-first

```
Creá [componente] usando solo HTML + CSS moderno (:has, container queries, clamp()).
Restricciones:
- Mobile-first, touch targets 44px mínimo (WCAG 2.5.5)
- Sin frameworks JS, sin Tailwind
- Tokens: leé ./styles/tokens.css (si existe) antes de inventar colores
- Un solo archivo autocontenido para preview rápido
Al terminar: abrí en preview y pasame screenshot + console logs.
```

### React 19 + Tailwind (cuando estés listo)

```
Implementá [componente] con patrones React 19:
- use() para recursos/context async (NO useEffect para fetch)
- Server Components por defecto; 'use client' solo si hay estado/evento
- Compound Components en vez de prop drilling
- forwardRef NO (ref es prop normal en React 19)
- Accesibilidad: roles ARIA, focus visible, keyboard nav
Tokens: JSON que te paso. Tipado estricto TypeScript.
```

### Dictamen Antigraviti

```
/antigraviti-neon "[Título]"
Fuente: [pegá texto o ruta]
Capítulos sugeridos: [opcional]
Reglas:
- Máx 6–9 tarjetas por capítulo
- Máx 12 resaltados por capítulo
- Paleta neon fija (no agregar colores)
- Anclar CADA dato a cita textual de la fuente
- Print CSS que preserve fondo oscuro al exportar PDF
```

---

## 4. Anti-patterns que debés evitar

| No hagas esto | Hacé esto |
|---|---|
| "Haceme algo lindo" | "Componente X, con restricciones Y, audiencia Z" |
| Aceptar código sin preview | Abrir en Claude Preview + screenshot antes de cerrar |
| Dejar que aplique fixes de seguridad solo | Pedir diff, validar, aplicar vos |
| Usar Opus para todo | Sonnet para iterar, Opus para el entregable final |
| Generar 15 variantes | Describir 1 restricción más y pedir 3 variantes con justificación |
| Inventar tokens cada vez | Referenciar `template-antigraviti-neon.html` como fuente única |

---

## 5. Los "skills de la guía viral" — verdad vs mito

La guía que recibiste mezcla cosas reales con comandos inventados. Acá la traducción honesta:

| Prometido | Realidad |
|---|---|
| `npx skills add https://github.com/...` | **No existe.** Skills se distribuyen vía marketplaces de `/plugin` o archivos locales en `~/.claude/skills/` |
| "Superpowers de obra" con `/write-plan` `/execute-plan` | El repo `obra/superpowers` existe en GitHub, pero se instala copiando archivos a `~/.claude/`, no con un CLI mágico |
| "Frontend Design de Anthropic" vía `skills.sh` | El dominio `skills.sh` con ese formato no existe. Anthropic publica skills en `github.com/anthropics/skills` — lo instalás clonando |
| "Firecrawl CLI con `init --all --browser`" | Firecrawl es un servicio SaaS con API/MCP. Ese comando es inventado. Si querés web scraping, usá `WebFetch` o Chrome MCP (ya tenés) |
| "Vercel Labs agent-skills" | El repo `vercel-labs/agent-skills` existe pero se instala manual, no con `npx skills add` |

**Conclusión:** no te falta nada. Tu stack Cowork cubre 95% de esos casos con skills reales.

---

## 6. Mejoras del sistema (cuando tengas tiempo)

1. **Node 20 LTS** — tu Node 16 impide tooling moderno (Vite 6, React 19, muchos MCP servers).
   ```bash
   brew install node@20
   brew link --overwrite node@20
   ```
2. **Figma MCP** — si usás Figma, conectalo con `/mcp` → agregar servidor oficial de Figma. Permite que Claude lea frames y extraiga tokens.
3. **Skill Creator** — cuando repitas un workflow 3+ veces, convertilo en skill local con `/anthropic-skills:skill-creator`.

---

## 7. Checklist del especialista

Antes de cerrar una entrega, pasá este check:

- [ ] ¿Preview visto en pantalla (no solo "el código compila")?
- [ ] ¿Accesibilidad básica (contraste, touch targets, focus visible)?
- [ ] ¿Tokens desde una sola fuente (no hardcoded)?
- [ ] ¿Print CSS si va a PDF?
- [ ] ¿Comentarios solo donde el *por qué* no es obvio?
- [ ] ¿Memoria actualizada si aprendí algo nuevo del usuario/proyecto?

---

## 8. Comando de inicio rápido

Cuando arranques una sesión nueva y quieras el contexto completo:

```
Leé CLAUDE_DESIGN_SPECIALIST.md y MEMORY.md global. Resumime en 3 bullets qué tengo cargado y preguntame qué querés hacer hoy.
```
