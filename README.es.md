# Ourway – Camino de Santiago · Servidor MCP

[🇬🇧 English version](README.md)

**Planifica, cotiza y reserva el Camino de Santiago desde cualquier asistente de IA.** Este es el
servidor oficial [Model Context Protocol](https://modelcontextprotocol.io) de
[Ourway](https://ourway.travel), agencia de viajes española especializada en Caminos de Santiago
organizados.

| | |
|---|---|
| **Endpoint** | `https://mcp.ourway.travel/mcp` (Streamable HTTP, sin auth, CORS abierto) |
| **Nombre en el registro** | `travel.ourway/camino-de-santiago` ([registro oficial MCP](https://registry.modelcontextprotocol.io)) |
| **Server card** | https://mcp.ourway.travel/.well-known/mcp/server-card.json |
| **Docs / homepage** | https://ourway.travel/ai (también en · fr · de · it · pt) |
| **Tools** | 12 · **Prompts** 3 · **Resources** 3 |
| **Contacto** | contacto@ourway.travel |

> El servidor es un servicio alojado y propietario. Este repositorio contiene su documentación
> pública, los esquemas de las tools y el `server.json` para que directorios, registros y
> asistentes puedan descubrirlo.

## Qué hace

- **Responde gratis a dudas del Camino**: credencial, Compostela, regla de los 100 km, mejor
  época, qué ruta elegir (Francés, Portugués Central/Costa, Norte, Primitivo, Inglés, Vía de la
  Plata, Finisterre-Muxía), cuántos días desde Sarria, Tui, Oporto, Roncesvalles u Oviedo,
  transporte de mochila, qué llevar, niños, bici, mascotas, presupuesto.
- **Construye itinerarios reales** sobre las etapas oficiales según el ritmo del peregrino, con mapa.
- **Cotiza todo el viaje como UN solo precio de paquete** con disponibilidad real de hoteles y
  filtros de calidad Ourway (régimen verificado, cerca del centro, valoración).
- **Permite ajustar hoteles** noche a noche (más barato, más categoría, más céntrico, con desayuno).
- **Crea la propuesta de reserva** y devuelve un enlace personal (válido 7 días) para revisar,
  confirmar y pagar.

## Conectar

**Claude (web / escritorio / móvil)** — Ajustes → Conectores → Añadir conector personalizado →
pega la URL del endpoint. Sin autenticación.

**ChatGPT** — Settings → Connectors → Advanced → Developer Mode → añade un conector MCP con la URL.

**Claude Code**
```bash
claude mcp add --transport http ourway https://mcp.ourway.travel/mcp
```

**Gemini CLI** (`~/.gemini/settings.json`)
```json
{ "mcpServers": { "ourway": { "httpUrl": "https://mcp.ourway.travel/mcp" } } }
```

**Cursor / VS Code** (`.cursor/mcp.json`)
```json
{ "mcpServers": { "ourway": { "url": "https://mcp.ourway.travel/mcp" } } }
```

## Tools

| Tool | Para qué |
|---|---|
| `camino_de_santiago_info` | FAQ gratuita e instantánea del Camino (cualquier idioma) |
| `list_camino_routes` | Caminos reservables con km totales |
| `camino_route_towns` | Poblaciones de una ruta en orden, km acumulados, coordenadas |
| `plan_camino_itinerary` | Itinerario día a día sobre las etapas oficiales + mapa PNG |
| `quote_camino_package` | Un precio de paquete con disponibilidad real (1–3 min) |
| `camino_hotel_alternatives` | Hasta 3 hoteles alternativos para una noche |
| `adjust_camino_package` | Más barato / más categoría / más céntrico / con desayuno |
| `swap_camino_hotel` | Aplica una alternativa y devuelve una nueva oferta firmada |
| `camino_extras_catalog` | Extras oficiales del peregrino (credencial, seguro, mochila…) |
| `camino_trip_intake_form` | Campos a recoger antes de cotizar |
| `create_camino_booking_proposal` | Crea la propuesta y devuelve el enlace del cliente |
| `camino_proposal_status` | Estado de una propuesta (código PET) |

Esquemas JSON completos: [`docs/tools.json`](docs/tools.json) (exportado de `tools/list`).

## Prompts de ejemplo

Ver [`examples/prompts.md`](examples/prompts.md).

## Principios de diseño

1. **Un solo precio.** Las tools nunca devuelven precios por hotel, noche o extra: solo el total del paquete.
2. **Ofertas firmadas.** Cada cotización es un token opaco firmado; la propuesta se crea a partir de él, nunca de precios aportados por el modelo.
3. **Sin itinerarios inventados.** Las etapas salen siempre de `plan_camino_itinerary` (etapas oficiales de cada camino).
4. **Privacidad.** Los datos personales solo se envían cuando el usuario aprueba expresamente crear la propuesta. Límites por IP.

## Privacidad y condiciones

https://ourway.travel/politica-privacidad · https://ourway.travel/aviso-legal
