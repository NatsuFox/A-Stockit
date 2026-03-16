# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Initial release of A-Stockit bundle-first skill library
- 25 quantitative analysis skills across the complete workflow
- Bilingual documentation (Chinese and English)
- Integration guide for Claude Code and Agent Skills-compatible frameworks
- Comprehensive contributing guidelines and code of conduct
- MIT License

### Skills Included

**Data & Context:**
- `market-data` - Data pipeline normalization and quality review
- `stock-data` - Individual stock data retrieval
- `fundamental-context` - Non-price context gathering

**Analysis:**
- `market-brief` - Quick one-symbol overview (default entry point)
- `analysis` - Deep research memo with artifacts
- `market-analyze` - Trend and regime interpretation
- `technical-scan` - Technical indicator analysis

**Screening & Monitoring:**
- `market-screen` - Breadth-first universe ranking
- `watchlist` - Watchlist management
- `watchlist-import` - Raw list normalization
- `decision-dashboard` - Batch action summary

**Decision Support:**
- `decision-support` - Conditional action and sizing guidance
- `strategy-design` - Execution plan generation
- `strategy-chat` - Multi-turn strategy discussion

**Review & Evaluation:**
- `reports` - Artifact inspection
- `analysis-history` - Historical analysis review
- `backtest-evaluator` - Strategy backtesting

**Integration:**
- `feishu-notify` - Feishu notification delivery
- `session-status` - Runtime context inspection

### Documentation
- Complete skill catalog with SKILL.md contracts
- Architecture design documentation (index.md)
- Quantitative workflow framework
- Runtime interface specifications
- Installation and integration guides

## [0.1.0] - 2026-03-15

### Added
- Initial project structure
- Core shared implementation modules
- Bundle registry and metadata system
- Basic skill implementations

---

## Version History

### Versioning Scheme

We use [Semantic Versioning](https://semver.org/):
- **MAJOR** version for incompatible API changes
- **MINOR** version for backwards-compatible functionality additions
- **PATCH** version for backwards-compatible bug fixes

### Release Notes Format

Each release includes:
- **Added**: New features
- **Changed**: Changes in existing functionality
- **Deprecated**: Soon-to-be removed features
- **Removed**: Removed features
- **Fixed**: Bug fixes
- **Security**: Security vulnerability fixes

---

[Unreleased]: https://github.com/yourusername/A-Stockit/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/yourusername/A-Stockit/releases/tag/v0.1.0
