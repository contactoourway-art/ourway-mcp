# Ourway – Camino de Santiago · MCP server

[![mcptry](https://mcptry.com/badge/travel.ourway/camino-de-santiago)](https://mcptry.com/servers/travel.ourway/camino-de-santiago)

[🇪🇸 Versión en español](README.es.md)

**Plan, quote and book the Camino de Santiago from any AI assistant.** This is the official
[Model Context Protocol](https://modelcontextprotocol.io) server of [Ourway](https://ourway.travel),
a Spanish travel agency specialized in organized Camino de Santiago trips.

| | |
|---|---|
| **Endpoint** | `https://mcp.ourway.travel/mcp` (Streamable HTTP, no auth, CORS open) |
| **Registry name** | `travel.ourway/camino-de-santiago` ([official MCP registry](https://registry.modelcontextprotocol.io)) |
| **Server card** | https://mcp.ourway.travel/.well-known/mcp/server-card.json |
| **Docs / homepage** | https://ourway.travel/en/ai (also es · fr · de · it · pt) |
| **Tools** | 12 · **Prompts** 3 · **Resources** 3 |
| **Contact** | contacto@ourway.travel |

> The server itself is a hosted, proprietary service. This repository holds its public
> documentation, tool schemas and `server.json` so that directories, registries and
> assistants can discover it.

## What it does

- **Answers Camino questions for free** — credential, Compostela, the 100 km rule, best
  season, which route (Francés, Portugués Central/Coastal, Norte, Primitivo, Inglés, Vía de la
  Plata, Finisterre-Muxía), how many days from Sarria, Tui, Porto, Roncesvalles or Oviedo,
  luggage transfer, packing, kids, cycling, pets, budget.
- **Builds real itineraries** on the official stages for the pilgrim's pace, with a route map.
- **Quotes the whole trip as ONE package price** with live hotel availability and Ourway
  quality filters (verified meal plan, close to the centre, guest rating).
- **Lets the user tweak hotels** night by night (cheaper, upgrade, more central, breakfast).
- **Creates the booking proposal** and returns a personal link (valid 7 days) to review,
  confirm and pay.

## Connect

**Claude (web / desktop / mobile)** — Settings → Connectors → Add custom connector → paste the
endpoint URL. No authentication.

**ChatGPT** — Settings → Connectors → Advanced → Developer Mode → add MCP connector with the
endpoint URL.

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

| Tool | Purpose |
|---|---|
| `camino_de_santiago_info` | Free, instant FAQ about the Camino (any language) |
| `list_camino_routes` | Bookable routes with total km |
| `camino_route_towns` | Towns along a route in trail order, cumulative km, coordinates |
| `plan_camino_itinerary` | Day-by-day itinerary on the official stages + PNG route map |
| `quote_camino_package` | One package price with live availability (1–3 min) |
| `camino_hotel_alternatives` | Up to 3 alternative hotels for one night |
| `adjust_camino_package` | Cheaper / upgrade / more central / with breakfast |
| `swap_camino_hotel` | Apply an alternative and get a new signed offer |
| `camino_extras_catalog` | Official pilgrim extras (credential, insurance, luggage transfer…) |
| `camino_trip_intake_form` | Fields to collect before quoting |
| `create_camino_booking_proposal` | Create the proposal and return the customer link |
| `camino_proposal_status` | Status of a proposal (PET code) |

Full JSON schemas: [`docs/tools.json`](docs/tools.json) (exported from `tools/list`).

## Example prompts

See [`examples/prompts.md`](examples/prompts.md). A few:

- *"I want to walk the Camino de Santiago from Sarria the first week of September, 2 people. Organize it for me."*
- *"Plan a 7-night Portuguese Camino from Tui for a family with two kids, breakfast included."*
- *"Do I need a pilgrim credential? How does the Compostela work?"*
- *"Make the package cheaper without changing the itinerary."*

## Design principles

1. **One price.** Tools never return per-hotel, per-night or per-extra prices — only the package total.
2. **Signed offers.** Every quote is an opaque signed token; the proposal is created from it, never from prices supplied by the model.
3. **No hallucinated itineraries.** Stages always come from `plan_camino_itinerary` (official stages of each route).
4. **Privacy.** Personal data is sent only when the user explicitly approves creating the proposal. Per-IP rate limits.

## Privacy & terms

https://ourway.travel/en/privacy-policy · https://ourway.travel/en/legal-notice
