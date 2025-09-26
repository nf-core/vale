# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains the **nf-core Vale style**, a custom Vale linting configuration specifically designed for nf-core documentation and community content. Vale is a command-line prose linter that enforces consistent terminology and writing standards.

## Architecture & Structure

### Core Components

**Style Configuration** (`.vale.ini`): Main configuration file that points to the nf-core style directory and applies rules to Markdown files.

**nf-core Style Directory** (`nf-core/`): Contains all Vale rule files (.yml) that define terminology enforcement:
- `Terms.yml` - Context-aware substitutions for major tools and technologies (Nextflow, Docker, GitHub, etc.)
- `nf-core.yml` - Enforces consistent "nf-core" spelling/capitalization
- `Nextflow.yml` - Ensures proper "Nextflow" capitalization
- `Seqera.yml` - Enforces "Seqera" company name spelling
- `SciLifeLab.yml` - Ensures proper "SciLifeLab" capitalization
- `We.yml` - Discourages first-person plural pronouns in documentation
- `SeqeraLabs.yml` - Company name variations
- `Conda.yml` - Proper "Conda" capitalization

**Vocabularies** (`nf-core/vocabularies/Docs/accept.txt`): Accepted terms that bypass spell-checking, including:
- Scientific terms (bioinformatics, subworkflows, BCFtools)
- People names (Ewels, Harshil, etc.)
- Company/organization names (Ardigen, Wellcome)

### Rule Pattern Architecture

**Substitution Rules**: Most rules use the `substitution` extension to enforce proper terminology. They include:
- `swap` dictionaries mapping incorrect → correct terms
- Regex patterns with negative lookbacks to avoid false positives (e.g., avoiding corrections in URLs, file extensions, commands)
- Context-aware matching that preserves technical usage while correcting prose

**Existence Rules**: Used for detecting unwanted patterns (like first-person pronouns in `We.yml`)

## Development Commands

### Local Testing
```bash
# Test Vale against documentation
vale README.md

# Test with custom config
vale --config=.vale.ini README.md

# Check Vale configuration
vale ls-config
```

### Release Process
```bash
# Create release package (matches GitHub Actions workflow)
zip -r nf-core.zip nf-core -x "*.DS_Store"

# Upload to GitHub release (manual until Actions are fixed)
gh release upload 0.0.1 nf-core.zip
```

### CI/CD Testing
The GitHub Actions workflow (`.github/workflows/main.yml`) runs:
- **Integration testing**: `vale README.md`
- **Feature testing**: Uses Cucumber with Ruby for behavior testing
- **Release packaging**: Automatically creates `nf-core.zip` on releases

Required dependencies for CI:
- Vale v2.26.0
- Python 3.10 (yamllint, pyyaml, docutils)
- Ruby 3.0 (Cucumber testing)

## Vale Style Development

### Adding New Rules
1. Create `.yml` file in `nf-core/` directory
2. Use appropriate `extends` type (substitution, existence, etc.)
3. Set `message`, `level` (error/warning/suggestion)
4. For substitution rules, define `swap` mappings
5. Test locally with `vale README.md`

### Rule Design Patterns
- **Regex negative lookbacks**: Prevent corrections in technical contexts
  ```yaml
  '(?i)(?<![\w.-])nextflow(?!\.io|\.config|\.nf| run| -|/)(?=[\s.,;!?]|$)': Nextflow
  ```
- **Context preservation**: Avoid correcting URLs, commands, file extensions
- **Case sensitivity**: Use `ignorecase: true/false` appropriately
- **Error levels**: Use `error` for terminology, `warning` for style preferences

### Vocabulary Management
Add accepted terms to `nf-core/vocabularies/Docs/accept.txt`:
- Scientific terms and tools
- Proper names (people, companies, places)
- Technical acronyms
- Use regex patterns like `[Hh]ackathon` for case variations

## Bypassing Vale (For Documentation)

```markdown
<!-- vale off -->
Content that Vale should ignore
<!-- vale on -->

<!-- Disable specific rule -->
<!-- vale Style.Rule = NO -->
Content with rule disabled
<!-- vale Style.Rule = YES -->
```

## Key Development Notes

- **Rule files must use `.yml` extension** (not `.yaml`)
- **Test thoroughly with edge cases** - regex patterns can have unintended matches
- **Preserve technical accuracy** - don't correct proper technical usage
- **Context-aware rules are preferred** - use negative lookbacks to avoid false positives
- **Release process is currently manual** - GitHub Actions needs fixing