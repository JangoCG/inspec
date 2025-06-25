# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Chef InSpec is an open-source testing framework for infrastructure with a human- and machine-readable language for specifying compliance, security and policy requirements. It's written in Ruby and provides a comprehensive DSL for testing system configurations, security compliance, and infrastructure state.

## Key Commands

### Development
- `bundle install` - Install dependencies
- `bundle exec inspec help` - Show available InSpec commands

### Testing
- `bundle exec rake test` - Run all tests (unit + functional)
- `bundle exec rake test:unit` - Run unit tests only
- `bundle exec rake test:functional` - Run functional tests only
- `bundle exec rake test:lint` - Run RuboCop linting
- `bundle exec rake test:parallel` - Run tests in parallel threads
- `bundle exec rake test:isolated` - Run tests in isolation
- `bundle exec m test/unit/resources/user_test.rb` - Run single test file
- `bundle exec m test/unit/resources/user_test.rb -l 123` - Run single test at line number

### Integration Testing
- `KITCHEN_YAML=kitchen.dokken.yml bundle exec kitchen list` - List test instances
- `KITCHEN_YAML=kitchen.dokken.yml bundle exec kitchen test <INSTANCE_NAME>` - Test specific instance
- `KITCHEN_YAML=kitchen.dokken.yml bundle exec kitchen test -c 3` - Test all instances in parallel

### Building and Installation
- `rake install` - Install InSpec locally
- `gem build inspec.gemspec && gem install inspec-*.gem` - Build and install gem locally

## Architecture Overview

### Core Components

**CLI and Execution Engine** (`lib/inspec/cli.rb`, `lib/inspec/runner.rb`)
- Main command-line interface and execution framework
- Handles profile loading, dependency resolution, and test execution

**Profile System** (`lib/inspec/profile.rb`, `lib/inspec/profile_context.rb`)
- Manages InSpec profiles containing controls, resources, and metadata
- Handles profile inheritance, dependencies, and vendoring

**Resource Framework** (`lib/inspec/resources/`)
- 100+ built-in resources for testing system components (files, packages, services, etc.)
- Resource DSL for creating custom resources
- Platform-specific implementations for different operating systems

**Control Evaluation** (`lib/inspec/control_eval_context.rb`, `lib/inspec/objects/`)
- Executes InSpec controls and describes blocks
- Manages test state and result collection

**Backends and Transport** (`lib/inspec/backend.rb`)
- Abstracts different execution targets (local, SSH, WinRM, Docker, cloud APIs)
- Uses Train library for transport layer

**Reporters** (`lib/inspec/reporters/`)
- Multiple output formats (JSON, YAML, JUnit, CLI, Automate)
- Streaming and file-based reporting

**Plugin System** (`lib/inspec/plugin/`)
- v1 and v2 plugin architectures
- Extensible system for custom resources, reporters, and CLI commands

### Key Directories

- `lib/inspec/` - Core InSpec library code
- `lib/inspec/resources/` - Built-in resource implementations
- `lib/plugins/` - Bundled plugins (compliance, init, reporters, etc.)
- `test/unit/` - Unit tests
- `test/functional/` - Functional tests
- `test/fixtures/` - Test data and mock files
- `examples/` - Example profiles and usage patterns

### Testing Infrastructure

The project uses multiple testing strategies:
- **Unit tests**: Fast tests for individual components using Minitest
- **Functional tests**: Integration tests that exercise the CLI and full workflows
- **Integration tests**: Docker-based tests across different operating systems using Test Kitchen

### Resource Development

Resources are the primary extension point. Each resource:
- Inherits from `Inspec::Resource` base class
- Implements matchers for testing specific attributes
- Handles platform-specific logic and command execution
- Uses FilterTable for complex data filtering and querying

### Profile Structure

InSpec profiles follow a standard structure:
- `inspec.yml` - Profile metadata and dependencies
- `controls/` - Control definitions with tests
- `libraries/` - Custom resource definitions
- `inputs.yml` - Input values for parameterized profiles

## Development Notes

- Ruby 3.1+ required
- Uses Train library for backend abstraction
- RSpec-like DSL but built on Minitest for core testing
- Heavy use of singleton patterns and global state management
- Platform detection and OS-specific resource implementations
- Licensed under Apache 2.0 with Chef EULA for packaged distributions