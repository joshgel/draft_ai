---
name: initial-draft-ai-tool
description: AI-powered fantasy football draft assistant using VORP, Monte Carlo simulations, and LLM analysis
status: backlog
created: 2025-08-28T00:45:50Z
---

# PRD: initial-draft-ai-tool

## Executive Summary

Vor-TeX is a best-in-class, AI-powered draft assistant for fantasy football that provides significant strategic advantage during live drafts. The application combines Value Over Replacement Player (VORP) modeling, predictive Monte Carlo simulations, and LLM-powered qualitative analysis to deliver data-driven recommendations that account for value, scarcity, and long-term roster construction.

## Problem Statement

Fantasy football managers currently rely on static rankings and gut feelings during live drafts, leading to suboptimal decisions made under time pressure. Existing tools lack the sophistication to:
- Synthesize multiple data sources effectively to prepare for the draft
- Predict draft flow and optimal strategies in real-time
- Combine quantitative metrics with qualitative insights
- Adapt recommendations based on actual draft progression

This tool addresses these gaps by providing an intelligent "co-pilot" that processes complex data and scenarios to recommend optimal picks.

## User Stories

### Primary User Persona
**Technically-inclined fantasy football manager** who wants to leverage advanced data analysis to make optimal draft picks during live drafts.

### Core User Stories

#### 1. Data Aggregation & VORP Engine
**As a user**, I want the application to automatically pull and synthesize player projections, Average Draft Position (ADP), and expert rankings from multiple sources so I have a comprehensive and reliable dataset.

**Acceptance Criteria:**
- System scrapes/pulls data via API from FantasyPros (Consensus Projections, ADP), Pro-Football-Reference (Historical Stats), and Sleeper API (Player Data)
- System calculates VORP score for every player
- VORP calculation is customizable based on league settings (teams, roster spots, PPR/Half-PPR scoring)
- VORP calculation is customizable based on various factors presented to the user pre-draft (e.g., positional scarcity, injury risk adjustments, aggressiveness)

#### 2. LLM-Powered Qualitative Insights
**As a user**, I want the application to perform pre-draft research by using an LLM to find and summarize expert opinions on players, so I can understand the qualitative narrative behind the numbers.

**Acceptance Criteria:**
- Pre-draft script iterates through top ~150 players
- LLM (e.g., Gemini API) searches and summarizes recent analysis on upside, risk, injuries, team situation
- LLM takes summaries and creates our own internal ranking this gets combined with VORP
- Text summaries are stored and linked to each player for UI display

#### 3. Monte Carlo Draft Simulator
**As a user**, on my turn, I want the system to simulate the rest of the draft thousands of times for each of my top potential picks, so I can see which choice leads to the best probable outcome for my final roster.

**Acceptance Criteria:**
- Application identifies top 5-7 available players by VORP
- Runs minimum 1,000 simulations per candidate
- CPU draft logic is stochastic (VORP + ADP + randomness)
- Calculates average final-roster strength (total VORP) per scenario
- Recommends player with highest average outcome

#### 4. Live Draft Assistant Dashboard
**As a user**, I need an intuitive, all-in-one dashboard that I can use during my live draft to track the board and get clear, actionable recommendations.

**Acceptance Criteria:**
- Simple input method for real-time draft picks
- Prominent display of top recommended pick from Monte Carlo simulation
- Best Available Players table ranked by VORP
- Player Analysis panel showing:
  - Multi-website expert ranking comparison
  - LLM-generated "Scouting Report" summary
- Current roster display with remaining positional needs

## Requirements

### Functional Requirements

**Core Features:**
1. Multi-source data aggregation and synthesis
2. VORP calculation engine with league customization
3. LLM-powered player research and summarization
4. Monte Carlo draft simulation (1,000+ iterations)
5. Real-time draft tracking interface
6. Player recommendation system
7. Roster construction analysis

**Data Processing:**
- Web scraping from FantasyPros, Pro-Football-Reference
- API integration with Sleeper
- Data normalization and storage
- Real-time updates during draft

**UI Components:**
- Draft board with pick tracking
- Recommendation panel
- Best available players list
- Player detail view with analysis
- Roster management view
- Positional needs tracker

### Non-Functional Requirements

**Performance:**
- Monte Carlo simulation for single player must complete in <20 seconds
- UI must update within 2 seconds of input
- Data refresh must complete within 5 minutes

**User Experience:**
- Clean, high-contrast interface
- Information presented without clutter
- Easy to read at a glance
- Mobile-responsive design preferred

**Scalability:**
- Support for different league sizes (8-14 teams)
- Handle various scoring formats (Standard, PPR, Half-PPR, Custom)
- Accommodate different roster configurations

**Reliability:**
- Graceful handling of API/scraping failures
- Local data caching for offline functionality
- Error recovery mechanisms

## Success Criteria

1. **Draft Performance:** Final generated roster consistently projected in top 3 by post-draft analyzers (e.g., FantasyPros Draft Analyzer)
2. **Usability:** User successfully completes live draft without technical issues
3. **Decision Quality:** Recommendations are logical, well-justified by on-screen data
4. **User Confidence:** User makes confident decisions based on tool guidance
5. **Time Efficiency:** Recommendations generated within draft clock constraints

## Constraints & Assumptions

### Technical Constraints
- Limited to 20-second processing window per pick
- Dependent on third-party data availability
- API rate limits for LLM and data sources

### Resource Constraints
- Single developer implementation
- Limited compute resources for simulations
- Budget constraints for API usage

### Assumptions
- Users have stable internet connection during draft
- Data sources remain accessible and structured consistently
- Users understand basic fantasy football concepts
- Draft occurs in snake format (not auction)

## Out of Scope

1. **Auction draft support** - Initial version focuses on snake drafts only
2. **Dynasty/Keeper league features** - No multi-year player valuation
3. **In-season management** - Tool is draft-only, no waiver/trade advice
4. **Mobile native app** - Web-based interface only
5. **Real-time sync with draft platforms** - Manual input only
6. **Custom scoring beyond standard formats** - Limited to common scoring systems
7. **IDP (Individual Defensive Players)** - Offense and team defense only
8. **Historical draft analysis** - Focus on current season only

## Dependencies

### External Dependencies
- **FantasyPros API/Website** - Player projections, ADP, rankings
- **Pro-Football-Reference** - Historical statistics
- **Sleeper API** - Player data and IDs
- **LLM API (Gemini/OpenAI)** - Qualitative analysis generation
- **Internet connectivity** - Required for data updates and LLM calls

### Internal Dependencies
- **Python ecosystem** - Core language and libraries
- **Streamlit framework** - UI development
- **Data storage solution** - SQLite or CSV/Parquet files

## Technical Stack

- **Backend Language:** Python
- **Data Manipulation:** Pandas, NumPy
- **Web Scraping/API:** Requests, BeautifulSoup
- **LLM Integration:** google-generativeai or openai library
- **Frontend UI:** Streamlit
- **Database:** SQLite or CSV/Parquet files

## Risk Mitigation

1. **Data Source Changes:** Implement flexible scraping patterns and fallback sources
2. **API Rate Limiting:** Cache responses and implement request throttling
3. **Performance Issues:** Pre-compute simulations where possible, optimize algorithms
4. **LLM Costs:** Set usage limits, implement caching of responses
5. **Draft Time Pressure:** Provide quick-pick recommendations as fallback