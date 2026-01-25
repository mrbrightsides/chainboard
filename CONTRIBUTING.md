# Contributing to ChainBoard

Thank you for your interest in contributing to **ChainBoard** - a trust-centric Web3 project governance platform! 🎉

We welcome contributions from the community to help build better tools for transparent, accountable, and verifiable project management.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Contribution Workflow](#contribution-workflow)
- [Coding Standards](#coding-standards)
- [Commit Guidelines](#commit-guidelines)
- [Pull Request Process](#pull-request-process)
- [Testing Guidelines](#testing-guidelines)
- [Documentation](#documentation)
- [Community & Support](#community--support)

---

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive environment for everyone, regardless of:
- Experience level
- Gender identity and expression
- Sexual orientation
- Disability
- Personal appearance
- Body size
- Race, ethnicity, or nationality
- Age
- Religion or lack thereof

### Expected Behavior

- **Be respectful** - Value different viewpoints and experiences
- **Be collaborative** - Work together towards common goals
- **Be constructive** - Provide helpful feedback
- **Be patient** - Help newcomers learn and grow
- **Be transparent** - Communicate openly and honestly

### Unacceptable Behavior

- Harassment, discrimination, or derogatory comments
- Trolling, insulting, or personal attacks
- Public or private harassment
- Publishing others' private information
- Any conduct that could be considered inappropriate in a professional setting

### Enforcement

Violations can be reported to **support@elpeef.com**. All reports will be reviewed and investigated promptly and fairly.

---

## How Can I Contribute?

### 1. Report Bugs 🐛

Found a bug? Help us fix it!

**Before submitting:**
- Check [existing issues](https://github.com/mrbrightsides/chainboard/issues) to avoid duplicates
- Gather reproduction steps and environment details

**When reporting:**
- Use a clear, descriptive title
- Describe expected vs actual behavior
- Provide step-by-step reproduction instructions
- Include screenshots/videos if applicable
- Specify your environment (browser, OS, wallet, etc.)

**Template:**
```markdown
**Bug Description**
A clear description of what the bug is.

**Steps to Reproduce**
1. Go to '...'
2. Click on '...'
3. See error

**Expected Behavior**
What you expected to happen.

**Actual Behavior**
What actually happened.

**Environment**
- Browser: Chrome 120
- OS: macOS 14.2
- Wallet: MetaMask 11.5.0
- ChainBoard Version: 1.0.0

**Screenshots**
If applicable, add screenshots.
```

### 2. Suggest Features 💡

Have an idea? We'd love to hear it!

**Before suggesting:**
- Check [existing feature requests](https://github.com/mrbrightsides/chainboard/issues?q=is%3Aissue+label%3Aenhancement)
- Consider if it aligns with ChainBoard's trust-centric mission

**When suggesting:**
- Use a clear, descriptive title with `[FEATURE]` prefix
- Explain the problem this feature solves
- Describe your proposed solution
- Consider alternatives
- Think about implementation complexity

**Template:**
```markdown
**Feature Request: [Title]**

**Problem**
Describe the problem this feature would solve.

**Proposed Solution**
How would this feature work?

**Alternatives Considered**
What other approaches did you think about?

**Use Case**
Who would benefit and how?

**Implementation Notes**
Any technical considerations?
```

### 3. Improve Documentation 📚

Documentation is crucial for adoption!

- Fix typos or unclear explanations
- Add examples or tutorials
- Translate to other languages
- Improve API documentation
- Create video guides or demos

### 4. Write Code 💻

Ready to contribute code? Great!

**Good first issues:**
- Look for issues labeled `good first issue` or `help wanted`
- UI/UX improvements
- Accessibility enhancements
- Performance optimizations
- Test coverage improvements

**Advanced contributions:**
- Blockchain integration enhancements
- AI feature improvements
- Real-time collaboration features
- Multi-chain support
- Smart contract development

---

## Development Setup

See [SETUP.md](./SETUP.md) for detailed installation and configuration instructions.

**Quick Start:**

```bash
# Clone the repository
git clone https://github.com/mrbrightsides/chainboard.git
cd chainboard

# Install dependencies
npm install
# or
pnpm install

# Set up environment variables
cp .env.example .env.local
# Edit .env.local with your API keys

# Run development server
npm run dev
# or
pnpm dev

# Open http://localhost:3000
```

---

## Contribution Workflow

### 1. Fork the Repository

Click the "Fork" button on [GitHub](https://github.com/mrbrightsides/chainboard).

### 2. Create a Branch

```bash
# Create a feature branch
git checkout -b feature/your-feature-name

# Or a bugfix branch
git checkout -b fix/bug-description
```

**Branch naming conventions:**
- `feature/` - New features
- `fix/` - Bug fixes
- `docs/` - Documentation changes
- `refactor/` - Code refactoring
- `test/` - Adding tests
- `chore/` - Maintenance tasks

### 3. Make Your Changes

- Write clean, readable code
- Follow coding standards (see below)
- Add tests if applicable
- Update documentation as needed

### 4. Test Your Changes

```bash
# Run type checking
npm run type-check

# Build the project
npm run build

# Test in development mode
npm run dev
```

### 5. Commit Your Changes

```bash
# Stage your changes
git add .

# Commit with a descriptive message
git commit -m "feat: add trust score calculation"
```

See [Commit Guidelines](#commit-guidelines) below.

### 6. Push to Your Fork

```bash
git push origin feature/your-feature-name
```

### 7. Open a Pull Request

- Go to your fork on GitHub
- Click "New Pull Request"
- Select your branch
- Fill out the PR template
- Submit!

---

## Coding Standards

### TypeScript Guidelines

1. **Strict Typing**
   ```typescript
   // ✅ Good - Explicit types
   function calculateTrustScore(tasks: Task[]): number {
     return tasks.filter(t => t.status === 'done').length / tasks.length * 100;
   }

   // ❌ Bad - Implicit any
   function calculateTrustScore(tasks) {
     return tasks.filter(t => t.status === 'done').length / tasks.length * 100;
   }
   ```

2. **No `any` Types**
   - Use proper types or `unknown`
   - Define interfaces for all data structures

3. **Type Imports**
   ```typescript
   // ✅ Good
   import type { Task } from '@/types';

   // ❌ Bad
   import { Task } from '@/types';
   ```

### React Best Practices

1. **Functional Components**
   ```typescript
   // ✅ Good
   const TaskCard: React.FC<{ task: Task }> = ({ task }) => {
     return <div>{task.title}</div>;
   };

   // ❌ Bad - Class components
   class TaskCard extends React.Component { ... }
   ```

2. **Hooks Rules**
   - Always use hooks at the top level
   - Don't call hooks conditionally
   - Use custom hooks for reusable logic

3. **Component Organization**
   ```typescript
   // 1. Imports
   import React from 'react';
   import type { Task } from '@/types';

   // 2. Types/Interfaces
   interface TaskCardProps {
     task: Task;
     onUpdate: (task: Task) => void;
   }

   // 3. Component
   const TaskCard: React.FC<TaskCardProps> = ({ task, onUpdate }) => {
     // 4. Hooks
     const [isEditing, setIsEditing] = useState(false);

     // 5. Handlers
     const handleSave = () => { ... };

     // 6. Render
     return <div>...</div>;
   };

   // 7. Export
   export default TaskCard;
   ```

### Styling Guidelines

1. **Tailwind CSS**
   - Use Tailwind utility classes
   - Follow mobile-first approach
   - Use design tokens (colors, spacing)

2. **Class Naming**
   ```tsx
   // ✅ Good - Descriptive, organized
   <div className="flex items-center gap-4 p-4 rounded-lg bg-white dark:bg-gray-800">

   // ❌ Bad - Too generic, hard to read
   <div className="flex p-4">
   ```

3. **Responsive Design**
   ```tsx
   // ✅ Good - Mobile-first
   <div className="text-sm md:text-base lg:text-lg">

   // ❌ Bad - Desktop-first
   <div className="text-lg md:text-sm">
   ```

### File Organization

```
src/
├── app/              # Next.js app router
├── components/       # React components
│   ├── ui/          # Reusable UI primitives
│   └── ...          # Feature components
├── lib/             # Utilities and helpers
├── types/           # TypeScript type definitions
└── contexts/        # React context providers
```

### Naming Conventions

- **Files**: PascalCase for components (`TaskCard.tsx`), camelCase for utilities (`storage.ts`)
- **Components**: PascalCase (`TaskCard`, `DashboardPanel`)
- **Functions**: camelCase (`calculateTrustScore`, `handleSubmit`)
- **Constants**: UPPER_SNAKE_CASE (`API_BASE_URL`, `MAX_TASKS`)
- **Types/Interfaces**: PascalCase (`Task`, `NFTRecord`)

---

## Commit Guidelines

We follow [Conventional Commits](https://www.conventionalcommits.org/) specification.

### Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation changes
- `style` - Code style changes (formatting, no logic change)
- `refactor` - Code refactoring
- `perf` - Performance improvements
- `test` - Adding or updating tests
- `chore` - Maintenance tasks
- `ci` - CI/CD changes
- `build` - Build system changes

### Examples

```bash
# Feature
git commit -m "feat(blockchain): add trust score calculation"

# Bug fix
git commit -m "fix(kanban): resolve drag-and-drop on mobile"

# Documentation
git commit -m "docs(readme): update installation instructions"

# Breaking change
git commit -m "feat(api)!: migrate to v2 endpoints

BREAKING CHANGE: API v1 endpoints are deprecated"
```

### Scope

Common scopes:
- `blockchain` - Blockchain/Web3 features
- `ui` - User interface components
- `api` - API routes and services
- `storage` - LocalStorage and data persistence
- `auth` - Authentication (SIWE)
- `kanban` - Task management
- `meeting` - Meeting features
- `analytics` - Analytics and insights

---

## Pull Request Process

### PR Template

When opening a PR, include:

```markdown
## Description
Brief description of what this PR does.

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Related Issues
Closes #123

## Changes Made
- Added trust score calculation
- Updated blockchain panel UI
- Fixed mobile responsiveness

## Testing
- [ ] Tested on Chrome
- [ ] Tested on Safari
- [ ] Tested on mobile
- [ ] Wallet connection works
- [ ] NFT minting works

## Screenshots
If applicable, add screenshots.

## Checklist
- [ ] Code follows project style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex logic
- [ ] Documentation updated
- [ ] No new warnings or errors
- [ ] Tested in production build
```

### Review Process

1. **Automated Checks**
   - TypeScript compilation
   - Build verification
   - Code formatting

2. **Manual Review**
   - Code quality and readability
   - Functionality testing
   - UI/UX review
   - Security considerations

3. **Feedback**
   - Address reviewer comments
   - Push updates to your branch
   - Request re-review when ready

4. **Merge**
   - PRs are merged by maintainers
   - Branch is automatically deleted after merge

---

## Testing Guidelines

### Manual Testing

Before submitting a PR, test:

1. **Core Functionality**
   - Task creation, editing, deletion
   - Drag-and-drop between columns
   - Wallet connection (SIWE)
   - NFT minting on task completion

2. **UI/UX**
   - Responsive design (mobile, tablet, desktop)
   - Theme switching (dark/light mode)
   - Accessibility (keyboard navigation, screen readers)
   - Loading states and error handling

3. **Browser Compatibility**
   - Chrome/Edge (Chromium)
   - Firefox
   - Safari
   - Mobile browsers (iOS Safari, Chrome Android)

4. **Blockchain Integration**
   - Wallet connection
   - Transaction signing
   - NFT minting
   - Etherscan verification

### Future: Automated Testing

We're working on adding:
- Unit tests (Jest, React Testing Library)
- Integration tests
- E2E tests (Playwright)
- Visual regression tests

---

## Documentation

### What to Document

1. **Code Comments**
   - Complex algorithms
   - Non-obvious decisions
   - Important constraints

2. **README Updates**
   - New features
   - Configuration changes
   - Breaking changes

3. **API Documentation**
   - New API routes
   - Request/response formats
   - Error codes

4. **Architecture Docs**
   - Design decisions
   - System architecture changes
   - Data flow diagrams

### Documentation Style

- Use clear, simple language
- Include code examples
- Add diagrams where helpful
- Keep it up-to-date with code changes

---

## Community & Support

### Getting Help

- **Discord**: https://discord.com/channels/@khudri_61362
- **Telegram**: https://t.me/khudriakhmad
- **Email**: support@elpeef.com
- **GitHub Discussions**: Coming soon!

### Recognition

Contributors are recognized in:
- README.md contributors section
- Release notes
- Special mentions in community updates

### Maintainers

- **@mrbrightsides** - Project lead and primary maintainer

---

## License

By contributing to ChainBoard, you agree that your contributions will be licensed under the same license as the project (see [LICENSE](./LICENSE)).

---

## Questions?

Don't hesitate to ask! We're here to help:
- Open an issue with the `question` label
- Join our Discord community
- Send us an email

**Thank you for contributing to ChainBoard! Together, we're building trust infrastructure for the Web3 era.** 🚀🛡️

---

**Last Updated**: January 2026  
**Version**: 1.0.0  
**Repository**: https://github.com/mrbrightsides/chainboard
