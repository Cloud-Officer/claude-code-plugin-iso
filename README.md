# claude-code-plugin-iso [![Build](https://github.com/Cloud-Officer/claude-code-plugin-iso/actions/workflows/build.yml/badge.svg)](https://github.com/Cloud-Officer/claude-code-plugin-iso/actions/workflows/build.yml)

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
- [Usage](#usage)
- [Contributing](#contributing)

## Introduction

Claude Code plugin for ISO 27001 procedure review and creation.

### Features

- Review procedure documents against ISO 27002:2022 controls
- Compare with legacy PDF procedures for missing details
- Construct missing sections from control references
- Apply fixes with minimal diff format
- Validate markdown formatting

## Installation

```bash
/plugin marketplace add cloud-officer/claude-code-plugin-iso
/plugin install co-iso@cloud-officer
```

### Local Development

```bash
claude --plugin-dir /path/to/claude-code-plugin-iso
```

## Usage

| Command                       | Description                          |
|-------------------------------|--------------------------------------|
| `/co-iso:review-iso <folder>` | Review ISO 27001 procedure documents |

## Contributing

We love your input! We want to make contributing to this project as easy and transparent as possible, whether it's:

* Reporting a bug
* Discussing the current state of the code
* Submitting a fix
* Proposing new features
* Becoming a maintainer

Pull requests are the best way to propose changes to the codebase. We actively welcome your pull requests:

1. Fork the repo and create your branch from `master`.
2. If you've added code that should be tested, add tests. Ensure the test suite passes.
3. Update the documentation.
4. Make sure your code lints.
5. Issue that pull request!

When you submit code changes, your submissions are understood to be under the same [License](license) that covers the
project. Feel free to contact the maintainers if that's a concern.

