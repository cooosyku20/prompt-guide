# Prompt Guide

A comprehensive prompt engineering guide for Claude 4 series models, specifically optimized for Opus 4.1.

## Contents

- **[Claude Opus 4.1 Prompt Guide (English)](Claude/Prompt_Guide_for_Opus_4_1_EN.md)** - English guide
- **[Claude Opus 4.1 Prompt Guide (Japanese)](Claude/Prompt_Guide_for_Opus_4_1_JA.md)** - Japanese guide

## Overview

This guide covers six essential techniques for effective prompt engineering with Claude 4:

1. **Explicitness** - Clear specifications and success criteria
2. **Extended Thinking** - Internal processing without exposed reasoning
3. **Parallel Tool Execution** - Simultaneous processing of independent tasks
4. **Cleanup Management** - Proper handling of temporary files and processes
5. **Structured Output** - JSON schemas with XML fallbacks
6. **Long Context Processing** - Effective handling of large document sets

## Quick Start

The guide provides both theoretical foundations and practical examples, including a complete prompt template that demonstrates all six techniques in action.

## Implementation Notes

- Prioritize JSON schema + tools for production use
- Use XML format as human-readable alternative
- Consider token limits: 200K input / 32K output
- Budget Extended Thinking at 1,024-4,096 tokens initially

## License

This guide is provided for educational and practical use in prompt engineering.