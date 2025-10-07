# nf-core Vale Style

Custom Vale linting style for [nf-core](https://nf-co.re) documentation, ensuring consistent terminology and branding across the community.

## Overview

This repository contains Vale rules organized into domain-specific categories using namespaced filenames:

- **Branding Rules**: nf-core branding and organization names
- **Technology Rules**: Tool and workflow engine capitalization (Nextflow, Conda, Docker, etc.)
- **Style Rules**: Writing style preferences (pronouns, etc.)
- **General Rules**: Catch-all terminology

## Rule Files

```
nf-core/
├── Branding-nfcore.yml           # nf-core branding
├── Branding-Organizations.yml    # Seqera, SciLifeLab
├── Technologies-Nextflow.yml     # Nextflow capitalization (error)
├── Technologies-Conda.yml        # Conda capitalization (warning)
├── Technologies-Tools.yml        # Docker, Singularity, FastQC, etc.
├── Style-Pronouns.yml            # First-person plural pronouns
└── General-Terminology.yml       # General terminology
```

## Resources

- Vale documentation: https://vale.sh/docs/topics/styles/
- Vale Studio (rule creator): https://studio.vale.sh/

## Installation

Add to your `.vale.ini`:

```ini
StylesPath = styles
MinAlertLevel = suggestion

[*.{md,mdx}]
BasedOnStyles = nf-core
```

Download the latest release from GitHub or use:

```bash
# Manual installation
wget https://github.com/nf-core/vale/releases/latest/download/nf-core.zip
unzip nf-core.zip -d styles/
```

## Development

### Quick Release (until Actions are fixed)

```bash
zip -r nf-core.zip nf-core -x "*.DS_Store"
gh release upload 0.0.1 nf-core.zip
```

## How to skip Vale

https://github.com/angular/angular/tree/main/aio/tools/doc-linter#if-all-else-fails

```md
<!-- vale off -->

Text the linter does not check for any style problem.

<!-- vale on -->

<!-- vale Style.Rule = NO -->
<!-- vale Style.Rule = YES -->
```

# Tests

NF-core
NF_core
nf_core
nf-core

Seqera
seqera
sequera

scilifelab
scilifeLab
SciLifelab
SciLifeLab

nextflow
next flow
next-flow
Next Flow
NextFlow

Conda
conda
