# Contributing to moonrepo Templates

This repository contains code generation templates for [moonrepo](https://moonrepo.dev). This guide explains how to contribute new templates or improve existing ones.

## 📋 Table of Contents

- [Getting Started](#getting-started)
- [Template Structure](#template-structure)
- [template.yml Configuration](#templateyml-configuration)
- [File and Folder Management](#file-and-folder-management)
- [Template Engine Usage](#template-engine-usage)
- [Testing Templates](#testing-templates)
- [Contribution Process](#contribution-process)
- [Best Practices](#best-practices)

## 🚀 Getting Started

### 1. Local Development Setup

```bash
# Clone the repository
git clone https://github.com/moonrepo/templates.git
cd templates

# Verify moon is installed
moon --version
```

### 2. Create a New Template

```bash
# Create a new template folder
mkdir my-template-name
cd my-template-name

# Create the template.yml file
touch template.yml
```

## 📁 Template Structure

Each template should follow this structure:

```
templates/
├── my-template-name/
│   ├── template.yml          # Required: Template configuration file
│   ├── src/                  # Template files
│   │   ├── index.ts.tera     # File using Tera syntax
│   │   └── [name].component.tsx  # File with variable interpolation
│   ├── package.json          # Regular template file
│   └── README.md.tera        # Documentation with Tera syntax
```

## ⚙️ template.yml Configuration

Every template root must contain a `template.yml` file:

```yaml
# Schema definition (recommended)
$schema: 'https://moonrepo.dev/schemas/template.json'

# Template basic information
title: 'My Template Name'
description: |
  Write a detailed description of your template.
  You can use multiple lines for this.

# Optional: Template ID override
id: 'my-template-name'

# Optional: Default destination path
destination: './packages/{{ name | kebab_case }}'

# Variable definitions
variables:
  name:
    type: 'string'
    required: true
    prompt: 'Enter project name:'
    default: 'my-project'

  description:
    type: 'string'
    required: false
    prompt: 'Enter project description:'
    default: ''

  author:
    type: 'string'
    required: false
    prompt: 'Enter author name:'
    
  language:
    type: 'enum'
    required: true
    prompt: 'Select the language to use:'
    multiple: false
    values:
      - typescript
      - javascript
      - rust
      
  features:
    type: 'enum'
    required: false
    prompt: 'Select features to include:'
    multiple: true
    values:
      - testing
      - linting
      - documentation
      - ci-cd
```

### Variable Types

- `string`: String values
- `boolean`: true/false values
- `number`: Numeric values
- `enum`: Selection options
- `array`: Array values

## 📂 File and Folder Management

### 1. File Name Variable Interpolation

You can use variables in file names to generate them dynamically:

```
src/[name].component.tsx        # Using name variable
tests/[name | snake_case].test.ts   # Using filters
```

### 2. File Extensions

- `.tera` or `.twig`: Uses Tera template engine syntax (extension removed when generated)
- `.raw`: Copied as-is (skips Tera rendering)

### 3. Partials

Reusable template fragments should be placed in a `partials` folder or include `partial` in the filename to exclude them from generation:

```
partials/header.tpl
components/button.partial.tsx
```

### 4. Frontmatter

Add YAML configuration at the top of files to control file generation:

```typescript
---
to: components/{{ name | pascal_case }}.tsx
force: true
skip: {{ language != "typescript" }}
---

export function {{ name | pascal_case }}() {
  return <div>{{ description }}</div>;
}
```

## 🔧 Template Engine Usage

### Variable Output

```tera
{{ name }}                    # Variable output
{{ name | upper }}           # Apply filter
{{ name | kebab_case }}      # Convert to kebab-case
{{ name | pascal_case }}     # Convert to PascalCase
{{ name | snake_case }}      # Convert to snake_case
```

### Conditionals

```tera
{% if language == "typescript" %}
export interface {{ name | pascal_case }}Props {
  children: React.ReactNode;
}
{% elif language == "javascript" %}
// JavaScript version
{% else %}
// Default version
{% endif %}
```

### Loops

```tera
{% for feature in features %}
  {% if feature == "testing" %}
import { test } from 'vitest';
  {% endif %}
{% endfor %}
```

### Available Filters

- **String**: `camel_case`, `pascal_case`, `snake_case`, `upper_snake_case`, `kebab_case`, `upper_kebab_case`, `lower_case`, `upper_case`
- **Path**: `path_join`, `path_relative`
- **Built-in**: All Tera built-in filters

### Available Variables

Variables always available in templates:

- `dest_dir`: Absolute path to the destination folder
- `dest_rel_dir`: Relative path from the working directory
- `working_dir`: Current working directory
- `workspace_root`: moon workspace root

### Available Functions

- `variables()`: Returns an object containing all variables in the current template

## 🧪 Testing Templates

### 1. Local Template Testing

```bash
# Set up moon workspace
echo 'generator:
  templates:
    - "file://path/to/this/templates/repo"' > .moon/workspace.yml

# Test template generation
moon generate my-template-name ./test-output
```

### 2. Test Different Variable Combinations

```bash
# Test with various variable values
moon generate my-template-name ./test-output -- --name "test-project" --language "typescript"
moon generate my-template-name ./test-output -- --name "another-test" --language "javascript"
```

### 3. Verify Generated Files

- Ensure all files are generated correctly
- Verify variables are properly substituted
- Check conditional files are appropriately generated/omitted

## 🤝 Contribution Process

### 1. Fork and Create Branch

```bash
# After forking on GitHub
git clone https://github.com/YOUR_USERNAME/templates.git
cd templates

# Create a new branch
git checkout -b add-my-template
```

### 2. Template Development

1. Create a new template folder
2. Configure `template.yml`
3. Write template files
4. Test locally

### 3. Documentation

Add a `README.md` file to your template folder with the following content:

```markdown
# Template Name

## Description
Explain what this template generates

## Usage
\`\`\`bash
moon generate template-name ./my-project
\`\`\`

## Variables
- `name`: Project name
- `description`: Project description

## Generated Files
- `package.json`
- `src/index.ts`
- `README.md`
```

### 4. Submit Pull Request

```bash
git add .
git commit -m "feat: add new template for [purpose]"
git push origin add-my-template
```

Create a Pull Request on GitHub and include the following information:

- Template purpose and use cases
- Generated file/folder structure
- Testing instructions
- Screenshots (if applicable)

## ✨ Best Practices

### 1. Template Design

- **Single Purpose**: Each template should have one clear purpose
- **Reusability**: Design for use across various projects
- **Flexibility**: Allow customization through variables

### 2. Naming Conventions

- Template folder names: Use `kebab-case`
- Variable names: Use `snake_case`
- File names: Follow the language/framework conventions

### 3. Documentation

- Write detailed descriptions in `template.yml`
- Provide clear `prompt` messages for each variable
- Create a README.md for each template

## 📖 References

- [moonrepo Code Generation Guide](https://moonrepo.dev/docs/guides/codegen)
- [Template Configuration Reference](https://moonrepo.dev/docs/config/template)
- [Tera Template Engine Documentation](https://keats.github.io/tera/)

---

Thank you for contributing to this repository! 🎉 