# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Models as a Service (MaaS) on Red Hat OpenShift AI 3.x** is an Antora-based documentation project that provides a complete course-in-a-box for deploying and managing MaaS solutions. The course covers business value, architecture, hands-on deployment, and troubleshooting of MaaS using vLLM (inference), Gateway API (routing), and Kuadrant (governance).

## Development Commands

```bash
# Install dependencies
npm install

# Build documentation site (Docker)
docker run -u $(id -u) -v $PWD:/antora:Z --rm -t antora/antora antora-playbook.yml

# Build documentation site (NPM)
npm run build
# or
npx antora antora-playbook.yml

# Watch for changes and auto-rebuild
npm run watch:adoc

# Serve built site locally (port 8080)
npm run serve

# View the site
open build/site/index.html
```

## Course Structure

The course is organized into Antora modules:

- **ROOT** (`modules/ROOT/`) - Home page and navigation
- **Chapter 1** (`modules/chapter1/`) - Main course content:
  - `index.adoc` - Foundations (Introduction & Value)
  - `section1.adoc` - Strategy (Well-Lit Paths)
  - `section2.adoc` - Taxonomy
  - `section3.adoc` - Architecture Deep Dive
  - `section4.adoc` - Hands-On Lab
  - `section5.adoc` - Troubleshooting
  - `quiz.adoc` - Knowledge Check

## Content Guidelines

This is an educational/training course, so content should be:

- **Hands-on focused** - Include practical deployment steps with actual commands
- **Business value driven** - Connect technical concepts to business outcomes (efficiency, security, simplicity)
- **Progressive** - Build from fundamentals through advanced topics
- **Platform-specific** - Focused on Red Hat OpenShift AI 3.x with KServe and vLLM

## Key Technologies Covered

- **Red Hat OpenShift AI 3.x** - ML/AI platform operator
- **KServe / LLMInferenceService** - Model serving framework
- **vLLM** - High-performance LLM inference engine
- **Gateway API** - Kubernetes networking/routing
- **Kuadrant** - API governance and rate limiting
- **llm-d** - Model deployment tool

## File Organization

```
rhoai3-maas/
├── antora.yml              # Antora component metadata
├── antora-playbook.yml     # Site generation configuration
├── devfile.yaml            # Dev Spaces/DevWorkspace configuration
├── package.json            # NPM dependencies and scripts
├── modules/
│   ├── ROOT/               # Root module (home page)
│   │   ├── nav.adoc        # Navigation structure
│   │   └── pages/
│   └── chapter1/           # Main course content
│       ├── nav.adoc
│       └── pages/
├── images/                 # Course images and diagrams
├── supplemental-ui/        # UI customizations
├── ui-bundle/              # Antora UI bundle
└── build/                  # Generated site output
```

## Deployment Context

The course teaches deployment on:
- **OpenShift 4.x** with Red Hat OpenShift AI 3.x operator
- **GPU nodes** (NVIDIA) for production workloads
- **KServe** model serving platform
- **Channel**: `stable-3.3` operator channel

## Development Scripts

- `course-init.sh` - Initialize course type (hol/bfx) and lab environment
- `create-ui-bundle.sh` - Generate Antora UI bundle
- `pdfgen.sh` - Generate PDF version of course

## Antora Build System

The documentation is built using Antora 3.1.3:

- **Source format**: AsciiDoc (`.adoc` files)
- **Site generator**: `@antora/site-generator` 3.1.3
- **Local preview**: `http-server` on port 8080
- **Watch mode**: Auto-rebuilds on file changes in `modules/`

When editing content, modify `.adoc` files in `modules/chapter1/pages/` and navigation in `nav.adoc` files.
