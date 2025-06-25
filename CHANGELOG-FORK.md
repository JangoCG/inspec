# Fork Changelog

All notable changes made to this fork are documented in this file.

This project is based on Chef InSpec (https://github.com/inspec/inspec) and contains modifications as required by the Apache License 2.0.

## Changes Made in This Fork

### Version Modifications
- **fix version** (3aec0a5b0) - Updated version strings in multiple files:
  - `inspec-bin/lib/inspec-bin/version.rb`
  - `lib/inspec/run_data/control.rb` 
  - `lib/inspec/version.rb`

- **disable telemetry and fix version** (5a9ae5563) - Disabled telemetry collection and updated version:
  - Modified `lib/inspec/utils/telemetry.rb` to disable telemetry functionality
  - Updated version in `inspec-bin/lib/inspec-bin/version.rb`
  - Modified CLI behavior in `lib/inspec/cli.rb`

- **introduce custom version** (6a56ba7a3) - Changed version identifier in `lib/inspec/version.rb`

### License and Chef Components
- **disable licence** (f277a6b22) - Removed Chef licensing requirements:
  - Modified `inspec-bin/bin/inspec` to disable license checks
  - Updated `lib/inspec/base_cli.rb` to remove licensing dependencies
  - Added `CLAUDE.md` with project documentation

### CI/CD Pipeline
- **add secrets** (15a1c59b1) - Enhanced GitHub Actions workflow with secrets management
- **remove tests from pipeline** (e9abb3719) - Streamlined CI pipeline by removing test stages
- **fix** (84bdc1c88) - Fixed GitHub Actions workflow configuration
- **bump action version** (7e6664235) - Updated GitHub Actions to newer versions
- **add pipeline** (7f03c58f2) - Added GitHub Actions workflow for building and publishing gems
  - Created `.github/workflows/build-and-publish-gem.yml`
  - Added `USING_FROM_GITHUB_PACKAGES.md` documentation

## Original Upstream

This fork is based on Chef InSpec, an open-source testing framework for infrastructure with a human- and machine-readable language for specifying compliance, security and policy requirements.

**Original Project**: https://github.com/inspec/inspec  
**License**: Apache License 2.0  
**Copyright**: Chef Software Inc. and InSpec contributors

## Modifications Summary

The main changes in this fork focus on:

1. **Removing Chef-specific licensing and telemetry** - Disabled Chef's commercial licensing system and telemetry collection to create a purely open-source distribution
2. **Custom versioning** - Implemented independent version numbering separate from upstream releases
3. **CI/CD customization** - Added custom GitHub Actions workflows for building and publishing this fork
4. **Documentation** - Added project-specific documentation and usage guidelines

All modifications maintain compatibility with the Apache License 2.0 and preserve attribution to the original creators.

## Apache License 2.0 Compliance

As required by the Apache License 2.0, this CHANGELOG-FORK.md file documents all modifications made to the original work. The original license and copyright notices are preserved, and this derivative work is also licensed under Apache License 2.0.

### Required Attribution

- Original work: Chef InSpec
- Original authors: Chef Software Inc. and InSpec contributors  
- Original license: Apache License 2.0
- This fork contains modifications by: Cengiz <cengiz.guertusgil@gmail.com>
- Fork license: Apache License 2.0

### Notice of Changes

This derivative work has been modified from the original. Changes include:
- Removal of Chef licensing and telemetry systems
- Custom version numbering
- Addition of custom CI/CD workflows  
- Documentation updates

The original copyright notice and license are preserved in the LICENSE file.