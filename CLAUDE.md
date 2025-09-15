# Claude Code Instructions for BeatBox Pro

This document provides context for Claude Code when working on the BeatBox Pro project.

## Project Overview

BeatBox Pro is a web-based music production platform starting from a simple drum sequencer MVP and evolving into a comprehensive collaborative music creation tool.

## Current State

- **MVP Status**: Functional single-file HTML application
- **Location**: `initial/Guitar Drone & Beats – Mobile Ready.html`
- **Technology**: Vanilla JavaScript + Web Audio API
- **Features**: 8-step drum sequencer, guitar drone synth, mobile-responsive

## Development Guidelines

### Testing the Application
```bash
# Run the current MVP
open "initial/Guitar Drone & Beats – Mobile Ready.html"
```

### Key Files
- `Product_Requirements_Document.md` - Complete product vision and technical roadmap
- `initial/Guitar Drone & Beats – Mobile Ready.html` - Current working MVP
- `README.md` - Project overview and quick start guide
- `.github/workflows/claude.yml` - Claude Code integration for GitHub issues/PRs
- `.github/workflows/claude-code-review.yml` - Automated PR reviews by Claude

### Code Standards
- The current MVP uses vanilla JavaScript for simplicity
- Web Audio API is used for all audio synthesis
- Mobile-first responsive design
- Self-contained architecture (no external dependencies)

### Future Architecture (Per PRD)
- **Frontend**: React/Vue.js with TypeScript
- **Backend**: Microservices (Node.js/Deno, Go)
- **Database**: PostgreSQL + Redis + MongoDB
- **Audio**: WebAssembly DSP modules
- **Collaboration**: WebRTC for real-time features

### Development Commands
Currently no build process - the MVP is a single HTML file.

**Git & GitHub:**
- `git push` - Push changes to GitHub
- `git pull` - Pull latest changes
- Tag `@claude` in GitHub issues/PRs for AI assistance

When transitioning to the full platform:
- `npm run lint` - Code linting
- `npm run typecheck` - TypeScript validation  
- `npm run test` - Run test suite
- `npm run build` - Build production assets

### Important Notes
- Always test audio features in multiple browsers
- Consider Web Audio API browser compatibility
- Mobile touch interface is critical for user experience
- Maintain low latency (<20ms) for real-time audio
- Current version has no server dependencies - runs entirely in browser

## GitHub Integration

**Repository**: https://github.com/hashslingers/beat-box

**GitHub Actions enabled:**
- Automatic pull request reviews by Claude
- Tag `@claude` in issues/comments for AI assistance
- CI/CD workflows ready for future development phases

## Project Vision

Transforming from simple beat maker to comprehensive music platform with collaboration, AI features, plugin ecosystem, and professional audio capabilities. See PRD for complete roadmap.