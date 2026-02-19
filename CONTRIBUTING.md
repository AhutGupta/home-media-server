# Contributing to Home Media Server

Thank you for your interest in contributing! This project welcomes contributions from the community.

## How to Contribute

### Reporting Issues

1. **Search existing issues** first to avoid duplicates
2. **Use issue templates** when creating new issues
3. **Provide details:**
   - Your environment (OS, Docker version)
   - Steps to reproduce
   - Expected vs actual behavior
   - Relevant logs (use Portainer or `docker logs`)

### Suggesting Enhancements

1. **Check existing feature requests** first
2. **Explain the use case** - why would this be valuable?
3. **Keep it relevant** to home media server setups
4. **Consider complexity** - simpler is better

### Code Contributions

#### Before You Start

1. **Open an issue** to discuss major changes
2. **Check for existing work** - someone might already be working on it
3. **Follow the project structure** - keep things organized

#### Development Process

1. **Fork the repository**
2. **Create a feature branch** from `main`
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes**
   - Follow existing code style
   - Test your changes thoroughly
   - Update documentation as needed
4. **Commit with clear messages**
   ```bash
   git commit -m "Add feature: brief description"
   ```
5. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```
6. **Create a Pull Request**
   - Describe what changed and why
   - Link related issues
   - Include testing steps

#### Guidelines

**Docker Compose Changes:**
- Test locally before submitting
- Maintain backward compatibility when possible
- Document any new environment variables in `.env.example`
- Update SETUP_GUIDE.md if adding new services

**Documentation:**
- Keep it concise
- Link to official docs when possible (reduce maintenance)
- Update relevant sections in README.md
- Add examples where helpful

**Configuration:**
- Use environment variables for user-specific settings
- Provide sensible defaults
- Comment complex configurations

## Code Style

- **YAML:** 2-space indentation, consistent formatting
- **Markdown:** Clear headers, proper formatting
- **Comments:** Explain *why*, not *what*

## Testing

Before submitting a PR:

1. **Build and start services:**
   ```bash
   cd compose_files
   docker-compose up -d
   ```

2. **Verify services are healthy:**
   ```bash
   docker-compose ps
   ```

3. **Check logs for errors:**
   ```bash
   docker-compose logs
   ```

4. **Test the affected functionality**

5. **Clean up:**
   ```bash
   docker-compose down
   ```

## What We're Looking For

**Welcomed contributions:**
- Bug fixes
- Documentation improvements
- New service integrations (media-related)
- Configuration improvements
- Performance optimizations
- Security enhancements

**Please avoid:**
- Massive refactors without discussion
- Adding services unrelated to media management
- Breaking changes without strong justification
- Removing features without discussion

## License

By contributing, you agree that your contributions will be licensed under the Apache License 2.0.

Your contributions must:
- Be your own original work
- Not violate any third-party rights
- Be properly documented (Apache 2.0 requires documenting changes)

## Questions?

- **General questions:** Open a GitHub Discussion
- **Bugs:** Open an Issue
- **Feature ideas:** Open an Issue with [Feature Request] tag
- **Security issues:** See SECURITY.md

## Community Guidelines

- Be respectful and constructive
- Help others when you can
- Share your knowledge
- Focus on the problem, not the person

## Recognition

Contributors will be recognized in:
- Git commit history
- GitHub contributors page
- Release notes (for significant contributions)

Thank you for making this project better! 🎉
