# HSix Labs GitHub Actions

This repository contains reusable GitHub Actions for standardizing CI/CD workflows across HSix Labs projects.

## Philosophy

We follow these principles for our GitHub Actions:

1. **Use direct commands for simple operations** - Linting, testing, and building are handled directly in workflows for maximum clarity and flexibility
2. **Create reusable actions for complex or reusable operations** - Tasks that are reusable, involve complexity, error-prone setups, or specialized knowledge are abstracted into actions
3. **Incorporate actions directly in repository templates** - We update our repository templates to directly use any relevant actions

## Repository Templates

Our repository templates directly incorporate these reusable actions when relevant to that repository type. This approach ensures that:

- New repositories start with the correct actions already integrated
- Best practices are automatically included in new projects
- Teams start with a working configuration they can customize

Each time an action is created, updated, or deleted, we update all relevant repository templates accordingly.

## Documentation

Each action includes its own README with:

- Detailed usage instructions
- Parameter descriptions
- Practical examples
- Common configurations

This documentation serves as the primary reference for how to use each action correctly.

## Usage Guidelines

1. Reference these actions using the `@main` tag for the latest version
2. When an action becomes stable, we may extract it to its own repository and version it separately
3. For best results, start with one of our repository templates that already incorporates these actions

## Contributing

When adding a new action:

1. Create a new directory with a descriptive name
2. Include a comprehensive README.md explaining what the action does and how to use it, with examples
3. Create an action.yml file following the [GitHub Actions specification](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions)
4. Update all relevant repository templates to incorporate the new action where appropriate
