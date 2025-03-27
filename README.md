# HSIX Labs GitHub Actions

This repository contains reusable GitHub Actions for standardizing CI/CD workflows across HSix Labs projects.

## Philosophy

We follow these principles for our GitHub Actions:

1. **Use direct commands for simple operations** - Linting, testing, and building are handled directly in workflows for maximum clarity and flexibility
2. **Create reusable actions for complex operations** - Tasks like LocalStack setup or semantic versioning are abstracted into actions due to their complexity
3. **Maintain workflow templates as examples** - Templates show how to combine direct commands with our actions

## Usage Guidelines

1. Reference these actions using the `@main` tag for the latest version
2. When an action becomes stable, we may extract it to its own repository and version it separately
3. For best results, copy and customize a workflow template to match your project's needs

## Contributing

When adding a new action:

1. Create a new directory with a descriptive name
2. Include a README.md explaining what the action does and how to use it
3. Create an action.yml file following the [GitHub Actions specification](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions)
4. Add examples showing how to integrate the action into workflows
