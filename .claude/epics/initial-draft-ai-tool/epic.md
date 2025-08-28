---
name: initial-draft-ai-tool
status: backlog
created: 2025-08-28T00:54:59Z
progress: 0%
prd: .claude/prds/initial-draft-ai-tool.md
github: [Will be updated when synced to GitHub]
---

# Epic: initial-draft-ai-tool

## Overview
Build a comprehensive fantasy football draft assistant that combines VORP calculations, Monte Carlo simulations, and LLM-powered insights. The system will leverage existing Python data processing libraries and Streamlit for rapid development, focusing on delivering actionable recommendations within tight time constraints during live drafts.

## Architecture Decisions

### Core Design Principles
- **Modular architecture** - Separate data, business logic, and UI layers
- **Cache-first approach** - Minimize API calls and expensive computations
- **Async operations** - Non-blocking data fetching and simulations
- **Progressive enhancement** - Core functionality works offline with cached data

### Technology Choices
- **Python 3.11+** - Leverage performance improvements and type hints
- **Streamlit** - Rapid UI development with built-in state management
- **SQLite** - Lightweight, file-based database for player data
- **Parquet files** - Efficient storage for simulation results
- **asyncio + aiohttp** - Concurrent data fetching
- **NumPy vectorization** - Fast Monte Carlo simulations
- **Google Gemini API** - Cost-effective LLM for player insights

### Design Patterns
- **Repository pattern** - Abstract data access layer
- **Strategy pattern** - Pluggable VORP calculation strategies
- **Observer pattern** - Real-time draft updates
- **Factory pattern** - Flexible simulation engine creation

## Technical Approach

### Frontend Components
- **Main Dashboard** (`app.py`) - Streamlit entry point with navigation
- **Draft Tracker** - Real-time pick input and board visualization
- **Player Recommender** - Monte Carlo results with confidence scores
- **Player Browser** - Sortable/filterable table with VORP rankings
- **Player Detail Modal** - LLM insights and multi-source rankings
- **Roster Manager** - Current team and positional needs matrix
- **Settings Panel** - League configuration and VORP parameters

### Backend Services

#### Data Layer (`src/data/`)
- `scraper.py` - Async scraping for FantasyPros, PFR
- `api_client.py` - Sleeper API integration
- `data_repository.py` - Unified data access interface
- `cache_manager.py` - TTL-based caching logic

#### Core Engine (`src/core/`)
- `vorp_calculator.py` - Configurable VORP engine with positional adjustments
- `monte_carlo.py` - Vectorized simulation engine using NumPy
- `llm_analyst.py` - Gemini API integration with prompt templates
- `draft_state.py` - Draft tracking and state management

#### Models (`src/models/`)
- `player.py` - Player data model with calculated fields
- `league_settings.py` - Configuration schemas
- `draft_pick.py` - Pick tracking model
- `simulation_result.py` - Monte Carlo output structure

### Infrastructure

#### Data Pipeline
- **Pre-draft batch job** - Scrape all data, generate LLM insights
- **SQLite database** - Store player data, rankings, projections
- **Parquet cache** - Store pre-computed simulation scenarios
- **Redis (optional)** - Real-time draft state for multi-user

#### Performance Optimizations
- **Vectorized calculations** - NumPy arrays for VORP and simulations
- **Parallel processing** - multiprocessing for batch simulations
- **Lazy loading** - Load player details on-demand
- **Incremental updates** - Only fetch changed data

## Implementation Strategy

### Phase 1: Foundation (Week 1)
- Set up project structure and dependencies
- Implement data scraping and storage layer
- Build basic VORP calculation engine
- Create minimal Streamlit UI

### Phase 2: Intelligence (Week 2)
- Integrate LLM for player insights
- Build Monte Carlo simulation engine
- Add caching and optimization layers
- Enhance UI with recommendations

### Phase 3: Polish (Week 3)
- Add draft tracking functionality
- Implement roster management
- Performance tuning and testing
- Documentation and deployment

### Risk Mitigation
- **Fallback data sources** - Local CSV if scraping fails
- **Simulation timeout** - Return best available if >20s
- **LLM rate limiting** - Batch requests, implement backoff
- **UI responsiveness** - Progressive loading, optimistic updates

### Testing Approach
- **Unit tests** - Core calculations and data processing
- **Integration tests** - API and scraping reliability
- **Performance tests** - Simulation speed benchmarks
- **User acceptance** - Mock draft scenarios

## Task Breakdown Preview

High-level task categories that will be created:
- [ ] **Data Infrastructure**: Set up SQLite schema, implement scrapers for FantasyPros/Sleeper APIs, create data models (3 days)
- [ ] **VORP Engine**: Build configurable VORP calculator with positional adjustments and league settings (2 days)
- [ ] **LLM Integration**: Implement Gemini API client, create prompt templates, build insight generation pipeline (2 days)
- [ ] **Monte Carlo Simulator**: Develop vectorized simulation engine with NumPy, implement draft logic (3 days)
- [ ] **Streamlit UI**: Create dashboard layout, player tables, recommendation panels, roster tracker (3 days)
- [ ] **Draft Management**: Build real-time pick tracking, state management, positional needs analysis (2 days)
- [ ] **Caching & Optimization**: Implement data caching, pre-compute simulations, performance tuning (2 days)
- [ ] **Testing & Documentation**: Write core tests, create user guide, deployment instructions (2 days)

## Dependencies

### External Service Dependencies
- **FantasyPros website** - Must remain scrapeable
- **Sleeper API** - Requires stable endpoint structure
- **Google Gemini API** - Need API key and quota
- **Internet connectivity** - For data updates

### Python Package Dependencies
- `streamlit>=1.28.0` - UI framework
- `pandas>=2.0.0` - Data manipulation
- `numpy>=1.24.0` - Numerical computing
- `aiohttp>=3.8.0` - Async HTTP client
- `beautifulsoup4>=4.12.0` - Web scraping
- `google-generativeai>=0.3.0` - Gemini API
- `sqlite3` - Built-in database

### Development Dependencies
- Python 3.11+ environment
- 8GB+ RAM for simulations
- Stable internet for API calls

## Success Criteria (Technical)

### Performance Benchmarks
- Monte Carlo simulation: <15 seconds for 5 players × 1000 iterations
- Data refresh: <3 minutes for full player dataset
- UI response: <500ms for user interactions
- LLM insights: <5 seconds per player

### Quality Gates
- 80% test coverage for core logic
- No blocking operations in UI thread
- Graceful degradation without internet
- All recommendations traceable to data

### Acceptance Criteria
- Successfully complete mock draft without errors
- Generate top-3 projected roster per FantasyPros
- Provide recommendations within 20-second window
- Support 10/12/14 team leagues with standard scoring

## Estimated Effort

### Overall Timeline
- **Total Duration**: 3 weeks (15 business days)
- **Development Effort**: ~120 hours
- **Testing & Documentation**: ~20 hours

### Resource Requirements
- 1 Full-stack Python developer
- Access to fantasy football domain knowledge
- $20/month for API costs during development

### Critical Path Items
1. Data scraping implementation (blocks everything)
2. VORP calculation (blocks simulations)
3. Monte Carlo engine (blocks recommendations)
4. Streamlit UI (blocks user testing)

## Tasks Created
- [ ] 001.md - Data Infrastructure (parallel: true)
- [ ] 002.md - VORP Engine (parallel: false, depends on 001)
- [ ] 003.md - LLM Integration (parallel: true)
- [ ] 004.md - Monte Carlo Simulator (parallel: false, depends on 002)
- [ ] 005.md - Streamlit UI (parallel: true, depends on 001)
- [ ] 006.md - Draft Management (parallel: false, depends on 005)
- [ ] 007.md - Caching & Optimization (parallel: false, depends on 004, 006)
- [ ] 008.md - Testing & Documentation (parallel: false, depends on all)

Total tasks: 8
Parallel tasks: 3
Sequential tasks: 5
Estimated total effort: 190-237 hours