# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

### Changed

### Fixed

## [1.2.0] - 2026-01-23

### Added

### Changed

### Fixed

## [1.1.0] - 2026-01-21

### Added
- Homebrew Tap Update reusable workflow

### Changed

### Fixed

## [1.0.1] - 2025-11-19

### Added

### Changed

### Fixed

## [1.0.0] - 2025-10-03

### Added

### Changed
- Improved README layout by consolidating logo, badges, and description into single centered block

### Fixed

## [0.15.0] - 2025-10-03

### Added

### Changed

### Fixed
- Fixed post-release workflow committing downloaded `update-changelog.py` script to target repositories
- Improved script handling by using temporary directory instead of working directory for downloaded scripts
- Enhanced workflow reliability with automatic cleanup of temporary files


## [0.14.0] - 2025-10-03

### Added

### Changed

### Fixed

## [0.13.0] - 2025-10-03

### Added

### Changed

### Fixed
- Removed incorrect `repository_dispatch` trigger from reusable post-release workflow (was causing local tag pushes to trigger the reusable workflow)

## [0.12.0] - 2025-10-03

### Added

### Changed

### Fixed
- Fixed invalid `@latest` reference in README examples (GitHub Actions doesn't support `@latest`)
- Corrected workflow paths in documentation examples
- Updated all references to use valid GitHub Actions syntax (`@main` instead of `@latest`)
- Fixed `GITHUB_TOKEN` secret name collision in reusable workflow (changed to `TOKEN`)

## [0.11.0] - 2025-10-01

### Added

### Changed

### Fixed
- Reusable workflows relocated to the .github directory

## [0.10.0] - 2025-10-01

### Added
- Core release and post-release workflows for automated GitHub releases
- Reusable workflow system for cross-repository usage
- Python script for automatic changelog updates
- Comprehensive documentation and examples
- Build extension points for customizable build processes
- Version control system for script downloads
- Workflow reorganization into separate directories
- Support for multiple programming languages and frameworks
- Automatic version detection from workflow tags
- Generic, language-agnostic design
- Single version management system
- Repository dispatch trigger system for reliable post-release workflows
- Enhanced reusable workflows with dual trigger support (workflow_call + repository_dispatch)
- Improved version passing between release and post-release workflows

### Changed
- Simplified version management by removing redundant parameters
- Reorganized workflows into `.github/workflows/` (local) and `workflows/` (reusable)
- Updated all documentation to reflect new structure
- Improved user experience with cleaner examples
- Auto-derive script version from workflow tag
- Eliminated need for separate version parameters
- Switched from release events to repository dispatch for post-release triggers
- Removed build artifact functionality to focus on changelog-based releases
- Consolidated examples into README.md for single source of truth
- Enhanced workflow permissions for better reliability

### Fixed
- Removed hardcoded version references in favor of dynamic detection
- Eliminated version duplication across files
- Streamlined configuration for easier adoption
- Version sync issues between workflow and script
- Redundant QUICK_START.md file
- Post-release workflow not triggering after successful releases
- File pattern matching errors causing "Too many retries" failures
- Version passing issues in repository dispatch events
- Workflow permissions causing 403 errors
- Redundant examples directory maintenance burden

### Technical Details
- **Workflows**: 4 total (2 local, 2 reusable)
- **Scripts**: 1 Python script for changelog automation
- **Documentation**: README.md
- **Version Control**: Single version per workflow (auto-derived)
- **Architecture**: Modular design with repository dispatch triggers
- **Trigger System**: Repository dispatch for reliable post-release execution

### Features
- **Automated Releases**: Create GitHub releases with customizable build processes
- **Changelog Management**: Automatic updates after releases with PR creation
- **Cross-Repository**: Reusable workflows that can be referenced from any repo
- **Single Version Control**: One version per workflow, automatically synced
- **Build Extensibility**: Clear extension points for any language or framework
- **Documentation**: Complete setup guides and examples for immediate use

