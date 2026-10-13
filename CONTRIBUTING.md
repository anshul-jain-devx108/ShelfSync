# Contributing to ShelfSync

ShelfSync is an open-source, multimodal social-to-shelf and phygital commerce engine. We welcome contributions from the community.

Whether you are optimizing Gemini video reasoning pipelines, building intelligent in-store aisle routing algorithms, adding support for new store catalog APIs, or improving our Shopper PWA and Merchant Command Portal, every contribution helps bridge the gap between social inspiration and physical store reality.

---

## Table of Contents

- [How to Contribute](#how-to-contribute)
- [Pull Request Guidelines](#pull-request-guidelines)
  - [1. Title Format](#1-title-format)
  - [2. Link to an Issue](#2-link-to-an-issue)
  - [3. Use the PR Template](#3-use-the-pr-template)
  - [4. AI-Generated Contributions Policy](#4-ai-generated-contributions-policy)
  - [5. Branching Strategy](#5-branching-strategy)
- [Development Setup](#development-setup)
  - [Prerequisites](#prerequisites)
  - [Backend Setup (Python / FastAPI / Vertex AI)](#backend-setup-python--fastapi--vertex-ai)
  - [Frontend Setup (Next.js / PWA / TypeScript)](#frontend-setup-nextjs--pwa--typescript)
  - [Environment Variables](#environment-variables)
- [Code Formatting and Validation](#code-formatting-and-validation)
- [Local Testing](#local-testing)
- [Modular Extension Guides](#modular-extension-guides)
  - [Adding a New Multimodal Agent Tool](#1-adding-a-new-multimodal-agent-tool)
  - [Adding a New Store Inventory / Catalog Adapter](#2-adding-a-new-store-inventory--catalog-adapter)
  - [Adding a New Social Video / Media Ingestion Provider](#3-adding-a-new-social-video--media-ingestion-provider)
- [Community and Getting Help](#community-and-getting-help)
- [License](#license)

---

## How to Contribute

Please follow the standard [Fork and Pull Request](https://docs.github.com/en/get-started/quickstart/contributing-to-projects) workflow:

1. **Fork the Repository** on GitHub to your personal account.
2. **Clone your fork locally**:
   ```bash
   git clone https://github.com/<your-username>/ShelfSync.git
   cd ShelfSync
   ```
3. **Create a topic branch** from `develop`:
   ```bash
   git checkout -b feat/your-feature-name
   ```
4. **Set up your local environment** following the [Development Setup](#development-setup) guide below.
5. **Make your changes** with atomic, well-described commits.
6. **Format and validate your code** using the pre-commit linting and type-checking scripts.
7. **Ensure all tests pass** locally before submitting.
8. **Push to your fork** and submit a **Pull Request** against the `develop` branch.

---

## Pull Request Guidelines

To keep the codebase maintainable, secure, and well-documented, all PRs must adhere to the following rules:

### 1. Title Format
Your PR title must begin with a standardized type tag in square brackets, followed by a concise, imperative description:

- **Format:** `[<type>] <concise summary>`
- **Examples:**
  - `[feat] Add Gemini 2.0 Flash video keyframe analysis for YouTube Shorts`
  - `[fix] Correct aisle coordinate distance heuristic in Dijkstra router`
  - `[agent] Implement pantry photo subtraction tool`
  - `[catalog] Add Shopify Storefront inventory adapter`
  - `[ui] Implement real-time shelf audit camera feed in Merchant Portal`
  - `[docs] Update GCP Vertex AI setup guide in architecture docs`
  - `[test] Add integration tests for dynamic substitution engine`
  - `[chore] Upgrade uv and ruff dependencies`

**Valid Types:**
`[feat]`, `[fix]`, `[agent]`, `[vision]`, `[catalog]`, `[ui]`, `[test]`, `[refactor]`, `[docs]`, `[chore]`, `[revert]`

### 2. Link to an Issue
Every PR should reference the issue it addresses:
- Use keywords like `Fixes #<issue_number>`, `Closes #<issue_number>`, or `Resolves #<issue_number>`.
- For non-trivial features or architecture changes, open an issue first for discussion before writing code.
- If an issue is already assigned to someone, coordinate in the issue thread before submitting a duplicate implementation.

### 3. Use the PR Template
Please complete all sections in our [Pull Request Template](.github/pull_request_template.md):
- Context & Motivation
- Proposed Changes
- Screenshots / Screen Recordings (mandatory for UI / PWA changes)
- Testing & Verification Steps

### 4. AI-Generated Contributions Policy
We embrace modern AI developer tooling (Gemini Code Assist, Claude, Copilot, Cursor, etc.). However:
- **Author Responsibility:** You are 100% responsible for all submitted code. You must understand every line and verify its correctness, security, and performance.
- **Testing Standard:** AI-generated PRs must include relevant unit/integration tests and pass linting and type checks.
- **Zero Hallucination Tolerance:** PRs containing hallucinated API methods, non-existent GCP endpoints, or placeholder `TODO` stubs will be closed.
- **Transparency:** Disclose whether AI tooling was used in the PR description.

### 5. Branching Strategy
- `main`: Production-ready, stable releases.
- `develop`: Active integration branch. **All PRs should target `develop` by default.**
- `feat/*`, `fix/*`, `docs/*`: Feature and fix branches created by contributors.

---

## Development Setup

ShelfSync consists of a high-performance **Python backend** (FastAPI, Google GenAI SDK, Vertex AI) and a responsive **Web/PWA frontend** (Shopper App & Merchant Portal).

### Prerequisites
- **Python:** `3.11+` (recommended: `3.12`)
- **uv:** Ultra-fast Python package and project manager ([Install uv](https://docs.astral.sh/uv/getting-started/installation/))
- **Node.js:** `18.x` or `20.x+` and `npm` / `pnpm`
- **Google Cloud SDK (`gcloud`):** Optional for local development with Vertex AI, or use local mock credentials.

---

### Backend Setup (Python / FastAPI / Vertex AI)

1. Verify `uv` is installed:
   ```bash
   uv --version
   # If not installed: pip install uv
   ```

2. Initialize virtual environment and install dependencies:
   - **On Unix/macOS:**
     ```bash
     uv venv .venv
     source .venv/bin/activate
     uv pip install -e ".[dev]"
     ```
   - **On Windows (PowerShell):**
     ```powershell
     uv venv .venv
     .venv\Scripts\Activate.ps1
     uv pip install -e ".[dev]"
     ```

3. Run the backend development server:
   ```bash
   uvicorn api.main:app --reload --port 8000
   ```
   Interactive API documentation will be available at `http://localhost:8000/docs`.

---

### Frontend Setup (Next.js / PWA / TypeScript)

1. Navigate to the web frontend directory:
   ```bash
   cd web
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the local development server:
   ```bash
   npm run dev
   ```
   Open `http://localhost:3000` in your browser.

---

### Environment Variables

Copy the provided environment sample file:
```bash
cp .env.example .env
```

Configure your local credentials in `.env`:
```env
# Google Cloud & Vertex AI
GCP_PROJECT_ID=your-gcp-project-id
GCP_LOCATION=us-central1
GEMINI_API_KEY=your-gemini-api-key   # For local prototyping

# Database / Storage
DATABASE_URL=postgresql://postgres:password@localhost:5432/shelfsync
GCS_BUCKET_NAME=shelfsync-media-uploads

# Application Settings
APP_ENV=development
LOG_LEVEL=INFO
```

> **Tip for Offline/Mock Development:** When developing without active GCP credentials, set `MOCK_GEMINI_SERVICES=true` in your `.env` to use recorded multimodal responses and fixtures.

---

## Code Formatting and Validation

Before committing or opening a PR, ensure your code complies with the project style guides.

### Python (Backend)
We use [Ruff](https://github.com/astral-sh/ruff) for linting and formatting, and [mypy](https://github.com/python/mypy) for static type checking:

- **Format Code:**
  ```bash
  # Unix / macOS
  ./scripts/format.sh
  # Windows PowerShell
  .\scripts\format.ps1
  ```
  *(Or directly: `ruff format . && ruff check --fix .`)*

- **Validate & Type Check:**
  ```bash
  # Unix / macOS
  ./scripts/validate.sh
  # Windows PowerShell
  .\scripts\validate.ps1
  ```
  *(Or directly: `ruff check . && mypy .`)*

### TypeScript / Frontend
```bash
cd web
npm run lint
npm run type-check
```

---

## Local Testing

All tests must pass before a PR can be merged.

### Run All Backend Tests
```bash
pytest
```

### Run Specific Test Suites
```bash
# Test multimodal video extraction
pytest tests/unit/agents/test_video_parser.py

# Test in-store aisle routing logic
pytest tests/unit/routing/test_aisle_navigator.py

# Test shelf visual audit computer vision
pytest tests/unit/vision/test_shelf_audit.py
```

### Run with Coverage
```bash
pytest --cov=shelfsync --cov-report=term-missing
```

---

## Modular Extension Guides

ShelfSync is architected to be modular and extensible. Below are guides for common contribution patterns:

### 1. Adding a New Multimodal Agent Tool
ShelfSync agents use specialized tools for video parsing, pantry subtraction, and substitute reasoning.

1. Create a new tool file under `shelfsync/agents/tools/<your_tool_name>.py`.
2. Inherit from `BaseTool` (defined in `shelfsync/agents/tools/base.py`):
   ```python
   from shelfsync.agents.tools.base import BaseTool
   from pydantic import BaseModel, Field

   class PantryAnalysisInput(BaseModel):
       image_uri: str = Field(description="Cloud storage or local path to pantry image")

   class PantrySubtractionTool(BaseTool):
       name: str = "pantry_subtraction"
       description: str = "Detects pantry items from photo and subtracts them from the cart"
       args_schema = PantryAnalysisInput

       async def _run(self, image_uri: str) -> dict:
           # Implementation logic here
           return {"excluded_items": ["butter", "salt"]}
   ```
3. Register your tool in `shelfsync/agents/registry.py`.
4. Add comprehensive unit tests in `tests/unit/agents/test_<your_tool_name>.py`.
5. Run formatting and validation before opening your PR.

---

### 2. Adding a New Store Inventory / Catalog Adapter
ShelfSync synchronizes live store inventories and physical aisle coordinates across diverse retail systems.

1. Create your adapter under `shelfsync/catalog/adapters/<adapter_name>.py`.
2. Implement the `CatalogAdapter` interface from `shelfsync/catalog/base.py`:
   - `get_product_by_sku(sku: str) -> Product`
   - `search_products(query: str, filters: dict) -> list[Product]`
   - `get_shelf_location(sku: str, store_id: str) -> AisleLocation`
   - `find_substitutes(sku: str, store_id: str) -> list[Product]`
3. Add a mock dataset or sandbox connection in `tests/fixtures/catalogs/<adapter_name>.json`.
4. Register the adapter in `shelfsync/catalog/factory.py`.
5. Add unit and integration tests under `tests/unit/catalog/`.

---

### 3. Adding a New Social Video / Media Ingestion Provider
ShelfSync parses videos from social sources (YouTube, Instagram, TikTok) to create timestamped ingredient/product breakdowns.

1. Create a new provider under `shelfsync/ingestion/providers/<provider_name>.py`.
2. Implement `MediaIngestionProvider` from `shelfsync/ingestion/base.py`:
   - `validate_url(url: str) -> bool`
   - `extract_metadata(url: str) -> VideoMetadata`
   - `extract_audio_and_keyframes(url: str) -> MediaPayload`
3. Wire your provider into the ingestion router in `shelfsync/ingestion/router.py`.
4. Add tests with mock video URL inputs and sample transcripts in `tests/unit/ingestion/`.

---

## Community and Getting Help

- **Questions & Ideas:** Open a discussion on [GitHub Discussions](https://github.com/anshul-jain-devx108/ShelfSync/discussions) or create an issue.
- **Bug Reports:** Search existing [Issues](https://github.com/anshul-jain-devx108/ShelfSync/issues) before opening a new bug report. Provide reproducible steps, logs, and system details.
- **Feature Requests:** Open a [Feature Proposal Issue](https://github.com/anshul-jain-devx108/ShelfSync/issues/new?template=feature_request.md) describing the problem, use case, and proposed solution.

---

## License

By contributing to ShelfSync, you agree that your contributions will be licensed under the project's [MIT License](LICENSE).
