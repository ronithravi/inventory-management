# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Factory Inventory Management System Demo - Full-stack application with Vue 3 frontend, Python FastAPI backend, and in-memory mock data (no database).

## Critical Tool Usage Rules

### Subagents
- **vue-expert**: **MANDATORY** for creating or significantly modifying any `.vue` file
- **code-reviewer**: Use after writing significant code
- **Explore**: Use for codebase exploration and pattern searches
- **general-purpose**: Use for complex multi-step tasks

### Skills
- **backend-api-test**: Use when writing or modifying tests in `tests/backend/`

### MCP Tools
- **ALWAYS use GitHub MCP tools** (`mcp__github__*`) for ALL GitHub operations
  - Exception: Local branches — use `git checkout -b` instead of `mcp__github__create_branch`
- **ALWAYS use Playwright MCP tools** (`mcp__playwright__*`) for browser testing
  - Frontend: `http://localhost:3000` | API: `http://localhost:8001`

## Stack
- **Frontend**: Vue 3 + Composition API + Vite (port 3000)
- **Backend**: Python FastAPI (port 8001)
- **Data**: JSON files in `server/data/` loaded at startup via `server/mock_data.py` (all in-memory)

## Commands

```bash
# Backend (uv preferred; falls back to system Python if uv not available)
cd server && uv run python main.py
# or: cd server && python main.py  (requires: pip install fastapi uvicorn pydantic)

# Frontend
cd client && npm install && npm run dev

# Backend tests (run from repo root)
cd tests && python -m pytest backend/ -v
# Single test file:
cd tests && python -m pytest backend/test_inventory.py -v
# Single test:
cd tests && python -m pytest backend/test_dashboard.py::TestDashboardEndpoints::test_summary_default -v
```

## Architecture

### Data Flow
Vue filter state (`useFilters.js`) → `client/src/api.js` (axios) → FastAPI query params → `server/mock_data.py` in-memory filtering → Pydantic validation → computed properties in Vue components

### Filter System
4 global filters: **Time Period, Warehouse, Category, Order Status** — managed in `useFilters.js` composable, applied to all API calls via query params. Inventory endpoint does not support month filtering (no time dimension on inventory data).

### Frontend Structure
- **Views** (`client/src/views/`): Dashboard, Inventory, Orders, Demand, Spending, Reports, Backlog — one per route
- **Components** (`client/src/components/`): FilterBar + 8 detail modals (BacklogDetail, CostDetail, InventoryDetail, ProductDetail, ProfileDetails, ProfileMenu, TasksModal, LanguageSwitcher)
- **Composables** (`client/src/composables/`): `useFilters.js` (global filter state), `useAuth.js` (profile/auth state), `useI18n.js` (language switching)
- **Localization**: `client/src/locales/en.js` and `ja.js` — English and Japanese UI strings via `useI18n` composable
- **Routes**: `/` Dashboard, `/inventory`, `/orders`, `/demand`, `/spending`, `/reports`

### Backend Structure
- `server/main.py`: All FastAPI routes, Pydantic models, and filtering logic in one file
- `server/mock_data.py`: Loads all JSON files once at import time; all filtering is done in `main.py` against these in-memory lists
- `server/generate_data.py`: Script to regenerate the JSON data files in `server/data/`

### API Endpoints
- `GET /api/inventory` — filters: warehouse, category
- `GET /api/orders` — filters: warehouse, category, status, month
- `GET /api/dashboard/summary` — all filters
- `GET /api/demand`, `/api/backlog` — no filters
- `GET /api/spending/summary|monthly|categories|transactions` — no filters
- `GET /api/reports/quarterly|monthly-trends`

### Test Structure
`tests/backend/` uses pytest + FastAPI `TestClient`. `conftest.py` provides the `client` fixture. Tests are organized by endpoint group (test_dashboard, test_inventory, test_misc_endpoints). `tests/pytest.ini` sets the root.

## Key Patterns & Common Issues

1. **v-for keys**: Always use unique field (`sku`, `id`, `month`) — never array index
2. **Date validation**: Always validate before `.getMonth()` — `new Date(x)` can return `Invalid Date`
3. **Pydantic sync**: Update models in `server/main.py` whenever JSON data structure changes
4. **Revenue goals**: $800K/month (single month filter), $9.6M YTD (all months)
5. **Reactivity**: Store raw API data in `ref()`, derive display data via `computed()`
6. **Currency utility**: Use `client/src/utils/currency.js` for formatting — don't inline `toLocaleString` calls

## Design System
- Colors: Slate/gray (`#0f172a`, `#64748b`, `#e2e8f0`)
- Status colors: green / blue / yellow / red
- Charts: Custom SVG only — no chart libraries
- Layout: CSS Grid for page structure, Flexbox for components
- No emojis in UI
