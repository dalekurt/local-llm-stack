# Changelog

All notable changes to the Docker Local AI LLM project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.2.0] - 2025-06-06

### Fixed
- Fixed watchtower configuration in docker-compose.yml by replacing the invalid `--label-filter=ai-network` flag with the correct `--scope ai-network` flag
- Resolved watchtower container restart loop caused by invalid command flag

### Added
- Added watchtower service for monitoring container updates
  - Configured to check for updates every 30 seconds
  - Cleanup mode enabled to remove old images after updates
  - Monitor-only mode enabled to avoid automatic updates
  - Label-enable flag added to only watch containers with watchtower enable label
  - Scope limited to ai-network containers

- Added pipelines service for Open WebUI
  - Using image from ghcr.io/open-webui/pipelines:main
  - Configured with restart policy set to unless-stopped
  - Volume mounted at ./data/pipelines:/app/pipelines
  - Environment variable PIPELINES_API_KEY set

## [0.1.0] - 2025-06-01

### Added
- Initial project setup with core AI services
- Docker Compose configuration for local AI and LLM services
- Network configuration for AI services communication 