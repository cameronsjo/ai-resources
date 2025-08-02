# GitHub Copilot Instructions Template

## Environment Context

- **Operating System**: [Windows/macOS/Linux]
- **Default Shell**: [PowerShell/bash/zsh]
- **Project Type**: [React/Vue/Angular + Node.js/Python/etc.]
- **Build System**: [Vite/Webpack/etc.]

## Automation Guidelines

### Multi-Command Operations

When a task requires running more than one command, create a temporary script instead of running commands individually. This improves efficiency and reduces user frustration.

**When to create scripts:**

- Multiple git operations (commits, adds, etc.)
- Batch file operations
- Complex setup/teardown tasks
- Any sequence of 3+ commands

**Script requirements:**

```bash
#!/bin/bash
# Template for temporary automation scripts

function execute_task_group() {
    local group_name="$1"
    local commands="$2"

    echo "✓ Executing: $group_name"
    if eval "$commands"; then
        echo "✓ Completed: $group_name"
    else
        echo "✗ Failed: $group_name" >&2
        exit 1
    fi
}

# Your grouped operations here
execute_task_group "Task 1" "command1 && command2"

# Self-destruct after successful completion
echo "All tasks completed successfully. Removing script..."
rm "$0"
```

**Key principles:**

- Use meaningful function names and error handling
- Group related operations logically
- Include progress feedback with colored output
- Add self-destruct capability for temporary scripts
- Generate ready-to-execute commands instead of asking yes/no questions
- Proactively suggest commits at logical stopping points

## Command Guidelines

### Use Platform-Appropriate Syntax

```bash
# Correct - Platform-specific commands
cd "/path/to/project"
npm run dev
source .venv/bin/activate  # Linux/macOS
# .\.venv\Scripts\activate  # Windows
ls -la *.js               # Linux/macOS
# Get-ChildItem *.js       # Windows PowerShell
```

## Repository Setup & Initialization

### Initial Repository Setup

**Complete repository initialization script:**

```bash
#!/bin/bash
# * Repository Setup: Complete initialization script for new projects
# ! IMPORTANT: Run this script only once when creating a new repository
# * Usage: ./setup-repo.sh [project-name] [project-type]

PROJECT_NAME=${1:-"new-project"}
PROJECT_TYPE=${2:-"fullstack"}  # fullstack, frontend, backend, python, node, etc.

echo "🚀 Setting up repository: $PROJECT_NAME ($PROJECT_TYPE)"

# Initialize Git repository
if [ ! -d ".git" ]; then
    git init
    echo "✓ Git repository initialized"
else
    echo "✓ Git repository already exists"
fi

# Create essential directories
mkdir -p .github/workflows
mkdir -p .github/ISSUE_TEMPLATE
mkdir -p .github/PULL_REQUEST_TEMPLATE
mkdir -p docs
mkdir -p scripts
mkdir -p tests
echo "✓ Directory structure created"

# Generate .gitignore
cat > .gitignore << 'EOF'
# Dependencies
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*
package-lock.json
yarn.lock

# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
env/
venv/
.venv/
pip-log.txt
pip-delete-this-directory.txt
.pytest_cache/
.coverage
htmlcov/
.tox/
.cache
.mypy_cache/
.dmypy.json
dmypy.json

# Build outputs
dist/
build/
*.egg-info/
.next/
out/
target/

# Environment variables
.env
.env.local
.env.development.local
.env.test.local
.env.production.local
.env.*
!.env.example

# IDE and editors
.vscode/
.idea/
*.swp
*.swo
*~
.DS_Store
Thumbs.db

# Logs
logs/
*.log

# Runtime data
pids/
*.pid
*.seed
*.pid.lock

# Database
*.sqlite
*.sqlite3
*.db

# Temporary files
tmp/
temp/
.tmp/

# OS generated files
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# Docker
.dockerignore

# Coverage reports
coverage/
.nyc_output/

# Dependency directories
jspm_packages/

# Optional npm cache directory
.npm

# Optional REPL history
.node_repl_history

# Output of 'npm pack'
*.tgz

# Webpack bundles
webpack-stats.json

# Visual Studio Code
.vscode/
*.code-workspace

# JetBrains IDEs
.idea/
*.iml
*.ipr
*.iws

# Sublime Text
*.sublime-project
*.sublime-workspace

# Archives
*.zip
*.tar.gz
*.rar

# Backup files
*.bak
*.backup
*.swp
*.swo
*~

# Security
secrets/
*.key
*.pem
*.p12
*.pfx
config/secrets.yml
EOF

echo "✓ .gitignore created"

# Generate .dockerignore
cat > .dockerignore << 'EOF'
# Version control
.git
.gitignore
.github/

# Dependencies
node_modules
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Python
__pycache__
*.pyc
*.pyo
*.pyd
.Python
.pytest_cache/
.mypy_cache/
.coverage
htmlcov/
.tox/
.venv/
venv/
env/

# Build artifacts
build/
dist/
target/
out/
.next/

# Development files
.env.local
.env.development.local
.env.test.local
README.md
docs/
tests/
*.md
LICENSE

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Logs
logs/
*.log

# Temporary files
tmp/
temp/
.tmp/

# Coverage
coverage/
.nyc_output/

# Archives
*.zip
*.tar.gz
*.rar

# Backup files
*.bak
*.backup
*~
EOF

echo "✓ .dockerignore created"

# Generate .editorconfig
cat > .editorconfig << 'EOF'
# EditorConfig is awesome: https://EditorConfig.org

# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
charset = utf-8

# Indentation override for all JS, TS, CSS, HTML files
[*.{js,ts,jsx,tsx,css,html,json,yml,yaml}]
indent_style = space
indent_size = 2

# Python files
[*.py]
indent_style = space
indent_size = 4
max_line_length = 88

# Markdown files
[*.md]
trim_trailing_whitespace = false
max_line_length = 80

# Makefiles
[Makefile]
indent_style = tab

# Windows files
[*.{cmd,bat,ps1}]
end_of_line = crlf
EOF

echo "✓ .editorconfig created"

echo "✨ Repository setup complete!"
echo "Next steps:"
echo "1. Run language-specific setup: ./setup-{language}.sh"
echo "2. Configure your IDE/editor"
echo "3. Set up CI/CD pipelines"
echo "4. Create your first commit"

# Self-destruct
rm "$0"
```

### Language-Specific Setup Scripts

**Node.js/TypeScript Setup:**

```bash
#!/bin/bash
# * Setup: Node.js/TypeScript project configuration
# ! Run after basic repository setup

echo "🟢 Setting up Node.js/TypeScript project..."

# Initialize package.json if it doesn't exist
if [ ! -f "package.json" ]; then
    npm init -y
    echo "✓ package.json initialized"
fi

# Install development dependencies
npm install --save-dev \
    typescript \
    @types/node \
    eslint \
    @eslint/js \
    @typescript-eslint/parser \
    @typescript-eslint/eslint-plugin \
    prettier \
    husky \
    lint-staged \
    jest \
    @types/jest \
    ts-jest \
    @testing-library/jest-dom

echo "✓ Development dependencies installed"

# Generate tsconfig.json
cat > tsconfig.json << 'EOF'
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "ESNext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "declaration": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "removeComments": true,
    "sourceMap": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  },
  "include": [
    "src/**/*",
    "tests/**/*"
  ],
  "exclude": [
    "node_modules",
    "dist",
    "build"
  ]
}
EOF

echo "✓ tsconfig.json created"

# Generate ESLint configuration
cat > .eslintrc.js << 'EOF'
module.exports = {
  env: {
    browser: true,
    es2022: true,
    node: true,
    jest: true,
  },
  extends: [
    'eslint:recommended',
    '@typescript-eslint/recommended',
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    project: './tsconfig.json',
  },
  plugins: [
    '@typescript-eslint',
  ],
  rules: {
    '@typescript-eslint/no-unused-vars': 'error',
    '@typescript-eslint/explicit-function-return-type': 'warn',
    '@typescript-eslint/no-explicit-any': 'warn',
    'prefer-const': 'error',
    'no-var': 'error',
  },
  ignorePatterns: [
    'dist/',
    'build/',
    'node_modules/',
    '*.min.js',
  ],
};
EOF

echo "✓ .eslintrc.js created"

# Generate Prettier configuration
cat > .prettierrc << 'EOF'
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "arrowParens": "avoid",
  "endOfLine": "lf"
}
EOF

echo "✓ .prettierrc created"

# Generate Jest configuration
cat > jest.config.js << 'EOF'
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  testMatch: [
    '**/tests/**/*.test.ts',
    '**/src/**/*.test.ts'
  ],
  collectCoverageFrom: [
    'src/**/*.{ts,tsx}',
    '!src/**/*.d.ts',
    '!src/**/*.test.ts'
  ],
  coverageDirectory: 'coverage',
  coverageReporters: [
    'text',
    'lcov',
    'html'
  ],
  transform: {
    '^.+\\.tsx?$': 'ts-jest',
  },
  setupFilesAfterEnv: ['<rootDir>/tests/setup.ts'],
};
EOF

echo "✓ jest.config.js created"

# Update package.json scripts
npx json -I -f package.json -e '
this.scripts = {
  "build": "tsc",
  "dev": "tsc --watch",
  "test": "jest",
  "test:watch": "jest --watch",
  "test:coverage": "jest --coverage",
  "lint": "eslint . --ext .js,.ts,.tsx --fix",
  "lint:check": "eslint . --ext .js,.ts,.tsx",
  "format": "prettier --write .",
  "format:check": "prettier --check .",
  "type-check": "tsc --noEmit",
  "prepare": "husky install"
}'

echo "✓ package.json scripts updated"

# Setup Husky and lint-staged
npx husky install
npx husky add .husky/pre-commit "npx lint-staged"

# Generate lint-staged configuration
cat > .lintstagedrc.json << 'EOF'
{
  "*.{js,ts,tsx}": [
    "eslint --fix",
    "prettier --write"
  ],
  "*.{json,css,md}": [
    "prettier --write"
  ]
}
EOF

echo "✓ Git hooks configured"

echo "✨ Node.js/TypeScript setup complete!"
rm "$0"
```

**Python Setup:**

```bash
#!/bin/bash
# * Setup: Python project configuration
# ! Run after basic repository setup

echo "🐍 Setting up Python project..."

# Create virtual environment
python -m venv .venv
echo "✓ Virtual environment created"

# Activate virtual environment
source .venv/bin/activate  # Linux/macOS
# .\.venv\Scripts\activate  # Windows

# Create requirements files
cat > requirements.txt << 'EOF'
# Production dependencies
# Add your main dependencies here
EOF

cat > requirements-dev.txt << 'EOF'
# Development dependencies
black>=23.0.0
isort>=5.12.0
pylint>=2.17.0
mypy>=1.5.0
pytest>=7.4.0
pytest-cov>=4.1.0
pre-commit>=3.3.0
bandit>=1.7.5
safety>=2.3.0
flake8>=6.0.0
EOF

echo "✓ Requirements files created"

# Install development dependencies
pip install -r requirements-dev.txt
echo "✓ Development dependencies installed"

# Generate pyproject.toml
cat > pyproject.toml << 'EOF'
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "project-name"
version = "0.1.0"
description = "Project description"
authors = [{name = "Your Name", email = "your.email@example.com"}]
license = {text = "MIT"}
readme = "README.md"
requires-python = ">=3.8"
classifiers = [
    "Development Status :: 3 - Alpha",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
]
dependencies = []

[project.optional-dependencies]
dev = [
    "black>=23.0.0",
    "isort>=5.12.0",
    "pylint>=2.17.0",
    "mypy>=1.5.0",
    "pytest>=7.4.0",
    "pytest-cov>=4.1.0",
    "pre-commit>=3.3.0",
]

[tool.black]
line-length = 88
target-version = ['py38']
include = '\.pyi?$'

[tool.isort]
profile = "black"
multi_line_output = 3
line_length = 88

[tool.pylint.messages_control]
disable = [
    "too-few-public-methods",
    "too-many-arguments",
    "too-many-instance-attributes",
]

[tool.mypy]
python_version = "3.8"
warn_return_any = true
warn_unused_configs = true
disallow_untyped_defs = true

[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = ["test_*.py", "*_test.py"]
addopts = "-v --tb=short --cov=src --cov-report=html --cov-report=term"

[tool.coverage.run]
source = ["src"]
omit = ["tests/*", "*/tests/*"]

[tool.coverage.report]
exclude_lines = [
    "pragma: no cover",
    "def __repr__",
    "raise AssertionError",
    "raise NotImplementedError",
]
EOF

echo "✓ pyproject.toml created"

# Generate pre-commit configuration
cat > .pre-commit-config.yaml << 'EOF'
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-json
      - id: check-toml
      - id: check-merge-conflict
      - id: debug-statements

  - repo: https://github.com/psf/black
    rev: 23.7.0
    hooks:
      - id: black

  - repo: https://github.com/pycqa/isort
    rev: 5.12.0
    hooks:
      - id: isort
        args: ["--profile", "black"]

  - repo: https://github.com/pycqa/flake8
    rev: 6.0.0
    hooks:
      - id: flake8
        additional_dependencies: [flake8-docstrings]

  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.5.1
    hooks:
      - id: mypy
        additional_dependencies: [types-requests]

  - repo: https://github.com/PyCQA/bandit
    rev: 1.7.5
    hooks:
      - id: bandit
        args: ["-c", ".bandit"]
EOF

echo "✓ .pre-commit-config.yaml created"

# Install pre-commit hooks
pre-commit install
echo "✓ Pre-commit hooks installed"

# Create basic project structure
mkdir -p src tests docs
touch src/__init__.py
touch tests/__init__.py

# Create basic test setup
cat > tests/conftest.py << 'EOF'
"""Test configuration and fixtures."""
import pytest

@pytest.fixture
def sample_data():
    """Sample test data."""
    return {"test": "data"}
EOF

echo "✓ Project structure created"

echo "✨ Python setup complete!"
rm "$0"
```

### Container Configuration

**Dockerfile Template:**

```dockerfile
# * Container: Multi-stage Dockerfile template
# * Usage: Customize stages and dependencies for your specific project

# Development stage
FROM node:18-alpine AS development
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]

# Build stage for frontend
FROM node:18-alpine AS frontend-build
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine AS production
WORKDIR /app

# Create non-root user
RUN addgroup -g 1001 -S appgroup && \
    adduser -S appuser -u 1001 -G appgroup

# Copy built application
COPY --from=frontend-build --chown=appuser:appgroup /app/dist ./dist
COPY --from=frontend-build --chown=appuser:appgroup /app/package*.json ./
RUN npm ci --only=production && npm cache clean --force

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:3000/health || exit 1

# Security
USER appuser
EXPOSE 3000

# Start application
CMD ["npm", "start"]
```

**Docker Compose Template:**

```yaml
# * Container: Docker Compose development environment
version: '3.8'

services:
  app:
    build:
      context: .
      target: development
    ports:
      - "${PORT:-3000}:3000"
    environment:
      - NODE_ENV=development
      - DATABASE_URL=${DATABASE_URL}
    env_file:
      - .env
      - .env.local
    volumes:
      - .:/app
      - /app/node_modules
      - app_data:/app/data
    depends_on:
      - database
      - redis
    restart: unless-stopped

  database:
    image: postgres:15-alpine
    environment:
      - POSTGRES_DB=${DB_NAME:-app_db}
      - POSTGRES_USER=${DB_USER:-app_user}
      - POSTGRES_PASSWORD=${DB_PASSWORD:-app_password}
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./scripts/init-db.sql:/docker-entrypoint-initdb.d/init.sql
    ports:
      - "${DB_PORT:-5432}:5432"
    restart: unless-stopped

  redis:
    image: redis:7-alpine
    command: redis-server --appendonly yes
    volumes:
      - redis_data:/data
    ports:
      - "${REDIS_PORT:-6379}:6379"
    restart: unless-stopped

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./ssl:/etc/nginx/ssl:ro
    depends_on:
      - app
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
  app_data:

networks:
  default:
    name: app_network
```

### Environment Configuration

**Environment Variables Template:**

```bash
# .env.example - Copy to .env and fill in your values
# ! Never commit real values to version control

# Application
NODE_ENV=development
PORT=3000
APP_NAME=Your App Name
APP_VERSION=1.0.0

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/dbname
DB_HOST=localhost
DB_PORT=5432
DB_NAME=app_db
DB_USER=app_user
DB_PASSWORD=your_secure_password

# Redis
REDIS_URL=redis://localhost:6379
REDIS_HOST=localhost
REDIS_PORT=6379

# Authentication
JWT_SECRET=your_jwt_secret_key_here
JWT_EXPIRES_IN=24h
SESSION_SECRET=your_session_secret_here

# External APIs
API_KEY=your_api_key_here
WEBHOOK_SECRET=your_webhook_secret

# Email
SMTP_HOST=smtp.example.com
SMTP_PORT=587
SMTP_USER=your_email@example.com
SMTP_PASSWORD=your_email_password

# Cloud Storage
AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
AWS_BUCKET_NAME=your_bucket_name
AWS_REGION=us-east-1

# Monitoring
SENTRY_DSN=your_sentry_dsn
LOG_LEVEL=info

# Feature Flags
FEATURE_NEW_UI=false
FEATURE_ADVANCED_SEARCH=true
```

**Environment Loading Script:**

```bash
#!/bin/bash
# * Environment: Load environment-specific configuration

load_environment() {
    local env_name=${1:-development}
    local env_file=".env.${env_name}"

    if [[ -f "$env_file" ]]; then
        echo "Loading environment: $env_file"
        export $(grep -v '^#' "$env_file" | xargs)
    elif [[ -f ".env" ]]; then
        echo "Loading default environment: .env"
        export $(grep -v '^#' .env | xargs)
    else
        echo "Warning: No environment file found"
    fi

    # Validate required environment variables
    required_vars=("DATABASE_URL" "JWT_SECRET")
    missing_vars=()

    for var in "${required_vars[@]}"; do
        if [[ -z "${!var}" ]]; then
            missing_vars+=("$var")
        fi
    done

    if [[ ${#missing_vars[@]} -gt 0 ]]; then
        echo "Error: Missing required environment variables: ${missing_vars[*]}"
        exit 1
    fi

    echo "✓ Environment loaded successfully"
}

# Usage: source ./load-env.sh [environment]
load_environment "$1"
```

### CI/CD Pipeline Templates

**GitHub Actions Workflow:**

```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16.x, 18.x, 20.x]

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linting
        run: npm run lint:check

      - name: Run type checking
        run: npm run type-check

      - name: Run tests
        run: npm run test:coverage

      - name: Upload coverage reports
        uses: codecov/codecov-action@v3
        with:
          file: ./coverage/lcov.info
          flags: unittests
          name: codecov-umbrella

  build:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build application
        run: npm run build

      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        with:
          name: build-files
          path: dist/

  security:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run security audit
        run: npm audit --audit-level=moderate

      - name: Run Snyk to check for vulnerabilities
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}

  deploy:
    if: github.ref == 'refs/heads/main'
    needs: [test, build, security]
    runs-on: ubuntu-latest

    steps:
      - name: Deploy to staging
        run: echo "Deploy to staging environment"

      - name: Run integration tests
        run: echo "Run integration tests"

      - name: Deploy to production
        if: success()
        run: echo "Deploy to production environment"
```

### Template Cleanup Instructions

**⚠️ IMPORTANT: Remove these setup instructions after completing initial repository configuration**

After successfully setting up your repository with the above scripts and configurations, follow this cleanup checklist:

**Setup Removal Checklist:**

- [ ] Delete setup scripts: `rm setup-repo.sh setup-*.sh`
- [ ] Remove template-specific comments (lines starting with `# *`, `# !`)
- [ ] Update README.md with project-specific information
- [ ] Customize package.json with correct project name and details
- [ ] Replace placeholder values in configuration files
- [ ] Remove unused language configurations (keep only what you need)
- [ ] Update Docker configurations for your specific stack
- [ ] Customize CI/CD workflows for your deployment process
- [ ] Remove this "Repository Setup & Initialization" section entirely
- [ ] Remove "Template Cleanup Instructions" section
- [ ] Commit cleaned-up configuration: `git add .; git commit -m "chore: finalize repository setup"`

**Maintenance Schedule:**

- Review and update dependencies monthly
- Update security configurations quarterly
- Review and update CI/CD pipelines as needed
- Archive or remove obsolete configuration files
- Update documentation to reflect current project state

## Git & Development Workflow

### Commit Organization

When dealing with large numbers of changed files, organize commits logically by feature/concern:

**Commit Categories:**

- Database changes: Models, migrations, schema updates
- Backend integration: API handlers, service logic
- Frontend components: UI components, styling, interfaces
- Configuration: Build configs, package updates, environment settings
- Documentation: README updates, inline docs, migration guides
- Utilities: Helper scripts, tools, utilities

**Commit Message Format (Conventional Commits):**
Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Common Types:**

- `feat:` - New feature for the user
- `fix:` - Bug fix for the user
- `docs:` - Documentation changes
- `style:` - Formatting, missing semi colons, etc; no production code change
- `refactor:` - Refactoring production code, eg. renaming a variable
- `test:` - Adding missing tests, refactoring tests; no production code change
- `chore:` - Updating grunt tasks etc; no production code change
- `build:` - Changes that affect the build system
- `ci:` - Changes to CI configuration files and scripts
- `perf:` - Performance improvements
- `revert:` - Reverts a previous commit

### Commit Strategy & Timing

**Good Stopping Points for Commits:**

- After completing a single feature or bug fix
- When tests pass and code is in a working state
- After refactoring a component or module
- When adding/updating documentation
- After configuration changes that affect build/deployment
- Before switching to work on a different feature
- When a logical unit of work is complete (even if feature isn't finished)

**One-Click Git Commands:**
Instead of asking "Do you want me to commit these changes?", generate ready-to-execute commands:

```bash
# Generate specific commands for the user to copy/paste or click
echo "Ready to commit? Run this command:"
echo "git add .; git commit -m 'feat(auth): add user authentication system

- implement JWT token validation
- add login/logout endpoints
- create user session management
- add password hashing utilities'"
```

### Branch Management & Pull Request Workflow

**Branch Creation Strategy:**
Always create feature branches for development work, following naming conventions:

```bash
# Feature branches
git checkout -b feature/user-authentication
git checkout -b feature/product-catalog
git checkout -b feature/websocket-integration

# Bug fix branches
git checkout -b fix/memory-leak-issue
git checkout -b fix/login-validation

# Hotfix branches (for production issues)
git checkout -b hotfix/security-patch
git checkout -b hotfix/critical-bug

# Documentation branches
git checkout -b docs/api-documentation
git checkout -b docs/setup-guide

# Refactoring branches
git checkout -b refactor/database-layer
git checkout -b refactor/component-architecture
```

**Branch Naming Conventions:**

- Use lowercase with hyphens
- Include type prefix: `feature/`, `fix/`, `hotfix/`, `docs/`, `refactor/`, `chore/`
- Be descriptive but concise
- Reference issue numbers when applicable: `feature/123-user-auth`

**Development Workflow:**

```bash
# 1. Create and switch to feature branch
git checkout -b feature/new-feature

# 2. Work in small, logical commits
git add specific-files
git commit -m "feat(scope): implement core functionality"

# 3. Keep branch updated with main/master
git fetch origin
git rebase origin/main  # or git merge origin/main

# 4. Push branch and create draft PR early
git push -u origin feature/new-feature
# Then create draft PR in GitHub/GitLab
```

**Draft Pull Request Best Practices:**

**When to Create Draft PRs:**

- As soon as you push your first commit to a feature branch
- For work-in-progress features that need early feedback
- When implementing complex features that benefit from ongoing review
- For collaborative features where multiple developers contribute

**Draft PR Benefits:**

- Early visibility into development progress
- Continuous integration feedback on every push
- Opportunity for early design feedback
- Documentation of development decisions and approach
- Easier collaboration and code sharing

**Draft PR Creation:**

```bash
# After pushing your feature branch
echo "Create draft PR with:"
echo "Title: [DRAFT] feat(scope): Brief description of feature"
echo "Body: Include checklist, implementation notes, and questions"
echo "Convert to ready for review when feature is complete"
```

**Draft PR Template:**

```markdown
## 🚧 Work in Progress

### Overview
Brief description of what this PR implements.

### Progress Checklist
- [ ] Core functionality implemented
- [ ] Unit tests added
- [ ] Integration tests passing
- [ ] Documentation updated
- [ ] Error handling implemented
- [ ] Performance considerations addressed
- [ ] Security review completed
- [ ] Ready for review

### Implementation Notes
- Key architectural decisions
- Challenges encountered and solutions
- Dependencies or breaking changes
- Performance impact

### Questions for Reviewers
- Specific areas needing feedback
- Alternative approaches to consider
- Integration concerns

### Testing
- [ ] Manual testing completed
- [ ] Edge cases covered
- [ ] Cross-browser testing (if applicable)
- [ ] Performance testing completed

### Related Issues
Closes #123
Related to #456
```

**Ready for Review Transition:**

```bash
# Before converting draft to ready for review:
echo "Pre-review checklist:"
echo "1. All tests passing"
echo "2. Code self-reviewed"
echo "3. Documentation updated"
echo "4. Commits properly organized"
echo "5. Branch rebased on latest main"
echo "6. All checklist items completed"

# Convert draft to ready
echo "✓ Convert draft PR to 'Ready for Review'"
echo "✓ Add specific reviewers"
echo "✓ Update title to remove [DRAFT] prefix"
```

**Git Best Practices:**

**Commit Hygiene:**

```bash
# Amend last commit for small fixes
git add fix-file.js
git commit --amend --no-edit

# Interactive rebase for commit cleanup
git rebase -i HEAD~3  # Clean up last 3 commits

# Squash related commits before PR merge
git rebase -i HEAD~5  # Squash feature commits
```

**Branch Cleanup:**

```bash
# After PR is merged, clean up local and remote branches
git checkout main
git pull origin main
git branch -d feature/completed-feature  # Delete local branch
git push origin --delete feature/completed-feature  # Delete remote branch

# Clean up tracking references
git fetch --prune
```

**Conflict Resolution:**

```bash
# When rebasing encounters conflicts
git status  # See conflicted files
# Edit files to resolve conflicts
git add resolved-file.js
git rebase --continue

# If rebase gets complex, abort and merge instead
git rebase --abort
git merge origin/main
```

**Multi-Developer Collaboration:**

```bash
# Working on shared feature branch
git fetch origin
git rebase origin/feature/shared-feature  # Stay updated
git push --force-with-lease origin feature/shared-feature  # Safe force push

# Collaborating without conflicts
git pull --rebase origin feature/shared-feature
# Make changes
git push origin feature/shared-feature
```

**Release Workflow:**

```bash
# Preparing for release
git checkout -b release/v1.2.0
# Update version numbers, changelog
git commit -m "chore(release): prepare v1.2.0"
git push origin release/v1.2.0
# Create PR to main, then tag after merge

# Creating release tags
git checkout main
git pull origin main
git tag -a v1.2.0 -m "Release version 1.2.0"
git push origin v1.2.0
```

**Emergency Hotfix Workflow:**

```bash
# For critical production issues
git checkout main
git pull origin main
git checkout -b hotfix/critical-security-fix

# Make minimal necessary changes
git commit -m "fix: resolve critical security vulnerability"
git push origin hotfix/critical-security-fix

# Create emergency PR with expedited review
# After merge, immediately deploy and create patch release
```

**Git Configuration Best Practices:**

```bash
# Set up useful git aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'

# Set up proper line endings for cross-platform work
git config --global core.autocrlf input  # Linux/Mac
git config --global core.autocrlf true   # Windows

# Set up GPG signing for commits (security best practice)
git config --global commit.gpgsign true
git config --global user.signingkey YOUR_GPG_KEY_ID
```

**Automated Git Workflows:**

```bash
# Script for starting new features
function new-feature() {
    local feature_name="$1"
    git checkout main
    git pull origin main
    git checkout -b "feature/$feature_name"
    echo "✓ Created feature branch: feature/$feature_name"
    echo "Next: Make changes, commit, and push to create draft PR"
}

# Script for finishing features
function finish-feature() {
    local current_branch=$(git branch --show-current)
    git fetch origin
    git rebase origin/main
    git push origin "$current_branch"
    echo "✓ Feature branch updated and pushed"
    echo "Next: Create/update PR and request review"
}
```

## Code Quality Standards

### Commenting Guidelines

**AI-Assisted Development Comments:**
Write comments that follow language conventions and enhance native IntelliSense while serving both human developers and AI agents:

```typescript
/**
 * Authenticates user credentials and returns session data
 * @param credentials - User login credentials
 * @param credentials.username - User's username
 * @param credentials.password - User's password
 * @returns Promise resolving to authentication result with optional token and user data
 * @throws {AuthenticationError} When credentials are invalid
 * @example
 * const result = await authenticateUser({ username: "john", password: "secret123" });
 * if (result.success) console.log("Welcome", result.user?.name);
 */
async function authenticateUser(credentials: LoginCredentials): Promise<AuthResult> {
    // TODO: Implement JWT token validation logic
    // * Implementation: Use bcrypt for password hashing, jsonwebtoken for JWT
    // * Security: Rate limit login attempts, log failed attempts
}

interface LoginCredentials {
    username: string;
    password: string;
}

interface AuthResult {
    success: boolean;
    token?: string;
    user?: UserObject;
    error?: string;
}
```

```python
def authenticate_user(username: str, password: str) -> AuthResult:
    """
    Authenticates user credentials and returns session data.

    Args:
        username: User's username (must be non-empty)
        password: User's password (minimum 8 characters)

    Returns:
        AuthResult: Dictionary containing success status, optional token and user data

    Raises:
        AuthenticationError: When credentials are invalid
        ValidationError: When input parameters are malformed

    Example:
        >>> result = authenticate_user("john", "secret123")
        >>> if result["success"]:
        ...     print(f"Welcome {result['user']['name']}")

    Note:
        * Security: Passwords are hashed using bcrypt
        * Performance: Results cached for 5 minutes
        * Rate limiting: Max 5 attempts per minute per IP
    """
    # TODO: Implement JWT token validation logic
    # * Implementation: Use bcrypt for password hashing, PyJWT for tokens
    # ! Security: Always hash passwords, never store plaintext
    pass
```

```csharp
/// <summary>
/// Authenticates user credentials and returns session data
/// </summary>
/// <param name="credentials">User login credentials containing username and password</param>
/// <returns>
/// Task containing AuthResult with success status, optional JWT token and user data
/// </returns>
/// <exception cref="ArgumentNullException">Thrown when credentials parameter is null</exception>
/// <exception cref="AuthenticationException">Thrown when credentials are invalid</exception>
/// <example>
/// var result = await AuthenticateUserAsync(new LoginCredentials
/// {
///     Username = "john",
///     Password = "secret123"
/// });
/// if (result.Success) Console.WriteLine($"Welcome {result.User?.Name}");
/// </example>
/// <remarks>
/// * Security: Passwords are hashed using BCrypt
/// * Performance: Results cached for 5 minutes using IMemoryCache
/// * Rate limiting: Max 5 attempts per minute per IP address
/// </remarks>
public async Task<AuthResult> AuthenticateUserAsync(LoginCredentials credentials)
{
    // TODO: Implement JWT token validation logic
    // * Implementation: Use BCrypt.Net for password hashing, System.IdentityModel.Tokens.Jwt
    // ! Security: Always validate and sanitize input parameters
}
```

```go
// AuthenticateUser validates user credentials and returns session data.
// It performs bcrypt password verification and generates JWT tokens for valid users.
//
// Parameters:
//   - username: User's username (required, non-empty)
//   - password: User's password (required, minimum 8 characters)
//
// Returns:
//   - *AuthResult: Authentication result containing success status and optional token/user data
//   - error: Authentication error if credentials are invalid or system error occurred
//
// Example:
//   result, err := AuthenticateUser("john", "secret123")
//   if err != nil {
//       log.Printf("Auth failed: %v", err)
//       return
//   }
//   if result.Success {
//       fmt.Printf("Welcome %s", result.User.Name)
//   }
//
// Security notes:
//   - Passwords are hashed using bcrypt with cost factor 12
//   - Rate limiting: 5 attempts per minute per IP
//   - Failed attempts are logged for security monitoring
func AuthenticateUser(username, password string) (*AuthResult, error) {
    // TODO: Implement JWT token validation logic
    // * Implementation: Use golang.org/x/crypto/bcrypt and github.com/golang-jwt/jwt
    // ! Security: Always validate input parameters and sanitize user data
}

**Better Comments Integration:**
Use these prefixes for enhanced visual display in editors with Better Comments plugin:

```javascript
// ! CRITICAL: Production operations require explicit confirmation
// ? Question: Should we cache these API responses for better performance?
// TODO: Implement user authentication with JWT tokens
// * Important: This function handles both single and multi-select modes
// // Commented out code - temporary removal for testing
```

**Better Comments Color Coding:**

- `// !` - **RED** - Critical warnings, production safety, breaking changes
- `// ?` - **BLUE** - Questions, uncertain decisions, needs review
- `// TODO:` - **ORANGE** - Tasks to be completed, feature requests
- `// *` - **GREEN** - Important information, documentation, explanations
- `// //` - **GRAY** - Commented out code, temporary removals

### Error Handling

- Always include try-catch blocks for external operations
- Provide meaningful error messages with context
- Use colored output for user feedback
- Fail fast with clear exit codes

### Progress Feedback

- Use colored console output for different message types:
  - Green: Success/Progress
  - Yellow: Warnings/Info
  - Red: Errors
  - Cyan: Process start
  - Gray: Detailed information

## Advanced Developer Experience

### AI Context Preservation

**Maintain AI Context Across Sessions:**

- Include brief project status comments at the top of work sessions
- Reference recent changes and current focus areas
- Mention known issues or temporary workarounds
- Document the "why" behind current architecture decisions

```javascript
// * Current Focus: [Feature Name] integration (Phase X of Y)
// * Recent Changes: [Brief description of recent work]
// * Known Issues: [Any temporary workarounds or known problems]
// * Next Steps: [What needs to be done next]
```

### Debugging Enhancement

**Structured Logging Patterns:**

```javascript
// * Debug: Use structured logging for AI-parseable output
logger.info("operation.success", {
    action: "create_item",
    item_id: id,
    user_id: userId,
    timestamp: new Date().toISOString()
});

// * Error Context: Include enough detail for AI debugging assistance
logger.error("operation.failed", {
    error: error.message,
    error_type: error.constructor.name,
    context: "user_initiated_action",
    stack_trace: error.stack
});
```

### Performance & Monitoring

**Performance Breadcrumbs:**

```javascript
// * Performance: Expected execution time < 100ms for typical dataset
// * Optimization: Uses caching/indexing for improved performance
// * Monitoring: Log slow operations (>500ms) for investigation
// * Scale: Tested with up to [X] items
function performOperation() {
```

### Security & Privacy

**Security Context:**

```javascript
// * Security: Input sanitization required for all user-provided data
// * Privacy: No sensitive data logged (user IDs hashed in logs)
// * Validation: Injection prevention via parameterized queries/sanitization
// * Authorization: User can only access their own data
function processUserData(userId, data) {
```

## Database Operations (if applicable)

**⚠️ CRITICAL: Database Safety Guidelines**

**Database Locations:**

- **Development Database**: `[path to dev database]` (safe to modify/reset)
- **Production Database**: `[path to production database]` (NEVER modify directly)

**Production Database Protection:**

```bash
# ALWAYS use database path parameters in scripts
DATABASE_PATH="${1:-dev_database.db}"  # Default to dev database
PRODUCTION_FLAG="${2:-false}"          # Explicit flag for production

if [ "$PRODUCTION_FLAG" = "true" ]; then
    echo "⚠️  WARNING: Using PRODUCTION database!"
    read -p "Type 'CONFIRM' to proceed with production database: " confirmation
    if [ "$confirmation" != "CONFIRM" ]; then
        echo "Operation cancelled for safety."
        exit 0
    fi
fi
```

**Database Safety Rules:**

- **NEVER** hardcode production database paths
- **ALWAYS** default to development database
- **ALWAYS** require explicit confirmation for production operations
- **ALWAYS** create backups before production changes
- **NEVER** run experimental scripts against production
- **ALWAYS** test on development database first

## Containerization & Environment Management

### Environment Variables & Configuration

**Environment Variable Priority:**
Always favor environment variables over hardcoded configuration:

```javascript
// * Configuration: Environment variables take precedence over defaults
const config = {
    port: process.env.PORT || 3000,
    dbUrl: process.env.DATABASE_URL || 'sqlite://./dev.db',
    apiKey: process.env.API_KEY, // Required, no default
    logLevel: process.env.LOG_LEVEL || 'info',
    maxRetries: parseInt(process.env.MAX_RETRIES) || 3
};

// * Validation: Ensure required environment variables are set
const requiredEnvVars = ['API_KEY', 'DATABASE_URL'];
const missingVars = requiredEnvVars.filter(varName => !process.env[varName]);
if (missingVars.length > 0) {
    console.error(`Missing required environment variables: ${missingVars.join(', ')}`);
    process.exit(1);
}
```

**Environment File Loading:**
Ensure applications load `.env` files properly:

```javascript
// * Environment: Load .env file early in application startup
import dotenv from 'dotenv';
import path from 'path';

// Load environment-specific .env files
const envFile = process.env.NODE_ENV ? `.env.${process.env.NODE_ENV}` : '.env';
dotenv.config({ path: path.resolve(process.cwd(), envFile) });

// Fallback to default .env if environment-specific file doesn't exist
if (process.env.NODE_ENV && !require('fs').existsSync(envFile)) {
    dotenv.config({ path: path.resolve(process.cwd(), '.env') });
}
```

```python
# * Environment: Python environment loading with validation
import os
from pathlib import Path
from dotenv import load_dotenv

def load_environment():
    """Load environment variables with proper fallback and validation."""
    # Load environment-specific .env file
    env_name = os.getenv('ENVIRONMENT', 'development')
    env_file = Path(f'.env.{env_name}')

    if env_file.exists():
        load_dotenv(env_file)
    else:
        # Fallback to default .env
        load_dotenv('.env')

    # Validate required variables
    required_vars = ['DATABASE_URL', 'SECRET_KEY']
    missing_vars = [var for var in required_vars if not os.getenv(var)]

    if missing_vars:
        raise EnvironmentError(f"Missing required environment variables: {', '.join(missing_vars)}")

# Call early in application startup
load_environment()
```

### Feature Flags & Configuration Management

**Feature Flag Implementation:**

```javascript
// * Feature Flags: Environment-driven feature toggles
class FeatureFlags {
    static isEnabled(featureName) {
        const envVar = `FEATURE_${featureName.toUpperCase()}`;
        const value = process.env[envVar];

        // Support various boolean representations
        return ['true', '1', 'yes', 'on'].includes(value?.toLowerCase());
    }

    static getConfig(featureName, defaultValue = null) {
        const envVar = `FEATURE_${featureName.toUpperCase()}_CONFIG`;
        const value = process.env[envVar];

        if (!value) return defaultValue;

        try {
            return JSON.parse(value);
        } catch (e) {
            return value; // Return as string if not valid JSON
        }
    }
}

// * Usage: Feature flag checks throughout application
if (FeatureFlags.isEnabled('advanced_search')) {
    // Enable advanced search functionality
}

const retryConfig = FeatureFlags.getConfig('retry_logic', { maxAttempts: 3, backoff: 1000 });
```

**Environment-Specific Configuration:**

```bash
# .env.development
NODE_ENV=development
LOG_LEVEL=debug
FEATURE_ADVANCED_SEARCH=true
FEATURE_ANALYTICS=false
DATABASE_URL=sqlite://./dev.db

# .env.production
NODE_ENV=production
LOG_LEVEL=info
FEATURE_ADVANCED_SEARCH=true
FEATURE_ANALYTICS=true
DATABASE_URL=${DATABASE_URL}  # Injected by container orchestration
```

### Secrets Management

**Secrets from Environment Paths:**

```javascript
// * Secrets: Load secrets from filesystem paths provided via environment variables
import fs from 'fs/promises';
import path from 'path';

class SecretsManager {
    static async loadSecret(secretName) {
        const secretPathEnv = `${secretName.toUpperCase()}_SECRET_PATH`;
        const secretPath = process.env[secretPathEnv];

        if (!secretPath) {
            throw new Error(`Secret path environment variable ${secretPathEnv} not set`);
        }

        try {
            const secretValue = await fs.readFile(secretPath, 'utf8');
            return secretValue.trim();
        } catch (error) {
            throw new Error(`Failed to load secret from ${secretPath}: ${error.message}`);
        }
    }

    static async loadAllSecrets() {
        const secrets = {};
        const secretEnvVars = Object.keys(process.env)
            .filter(key => key.endsWith('_SECRET_PATH'));

        for (const envVar of secretEnvVars) {
            const secretName = envVar.replace('_SECRET_PATH', '').toLowerCase();
            try {
                secrets[secretName] = await this.loadSecret(secretName);
            } catch (error) {
                console.error(`Warning: Could not load secret ${secretName}: ${error.message}`);
            }
        }

        return secrets;
    }
}

// * Usage: Load secrets during application startup
const secrets = await SecretsManager.loadAllSecrets();
```

**Docker Secrets Integration:**

```bash
# Environment variables pointing to Docker secrets
DATABASE_PASSWORD_SECRET_PATH=/run/secrets/db_password
API_KEY_SECRET_PATH=/run/secrets/api_key
JWT_SECRET_SECRET_PATH=/run/secrets/jwt_secret
```

### Container Runtime Detection

**Container Engine Detection:**

```javascript
// * Container: Detect available container runtime (Podman > Rancher > Docker)
import { execSync } from 'child_process';

class ContainerRuntime {
    static async detectRuntime() {
        const runtimes = ['podman', 'rancher', 'docker'];

        for (const runtime of runtimes) {
            if (await this.isRuntimeAvailable(runtime)) {
                return runtime;
            }
        }

        throw new Error('No container runtime detected (podman, rancher, or docker)');
    }

    static async isRuntimeAvailable(runtime) {
        try {
            execSync(`${runtime} --version`, { stdio: 'pipe' });
            return true;
        } catch (error) {
            return false;
        }
    }

    static async getContainerInfo() {
        const runtime = await this.detectRuntime();

        try {
            const version = execSync(`${runtime} --version`, { encoding: 'utf8' });
            const info = execSync(`${runtime} info --format json`, { encoding: 'utf8' });

            return {
                runtime,
                version: version.trim(),
                info: JSON.parse(info)
            };
        } catch (error) {
            return {
                runtime,
                version: 'unknown',
                error: error.message
            };
        }
    }
}
```

```bash
# * Container: Shell script for container runtime detection
detect_container_runtime() {
    local runtimes=("podman" "rancher" "docker")

    for runtime in "${runtimes[@]}"; do
        if command -v "$runtime" >/dev/null 2>&1; then
            echo "Detected container runtime: $runtime"
            return 0
        fi
    done

    echo "Error: No container runtime found. Please install podman, rancher, or docker."
    exit 1
}

# Usage in scripts
CONTAINER_RUNTIME=$(detect_container_runtime)
```

```powershell
# * Container: PowerShell container runtime detection
function Get-ContainerRuntime {
    $runtimes = @('podman', 'rancher', 'docker')

    foreach ($runtime in $runtimes) {
        try {
            $null = & $runtime --version 2>$null
            if ($LASTEXITCODE -eq 0) {
                Write-Host "Detected container runtime: $runtime" -ForegroundColor Green
                return $runtime
            }
        }
        catch {
            continue
        }
    }

    throw "No container runtime found. Please install podman, rancher, or docker."
}

# Usage
$containerRuntime = Get-ContainerRuntime
```

### Docker & Container Best Practices

**Dockerfile Best Practices:**

```dockerfile
# * Container: Multi-stage build for optimized production images
FROM node:18-alpine AS builder

# Install dependencies only
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# Build application
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine AS production

# Create non-root user for security
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nextjs -u 1001

# Set working directory
WORKDIR /app

# Copy built application from builder stage
COPY --from=builder --chown=nextjs:nodejs /app/dist ./dist
COPY --from=builder --chown=nextjs:nodejs /app/node_modules ./node_modules
COPY --from=builder --chown=nextjs:nodejs /app/package.json ./

# Environment variables with defaults
ENV NODE_ENV=production
ENV PORT=3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:$PORT/health || exit 1

# Switch to non-root user
USER nextjs

# Expose port
EXPOSE $PORT

# Start command
CMD ["node", "dist/index.js"]
```

**Docker Compose Configuration:**

```yaml
# * Container: Docker Compose with environment-driven configuration
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "${PORT:-3000}:3000"
    environment:
      - NODE_ENV=${NODE_ENV:-production}
      - DATABASE_URL=${DATABASE_URL}
      - LOG_LEVEL=${LOG_LEVEL:-info}
      - FEATURE_ANALYTICS=${FEATURE_ANALYTICS:-false}
    env_file:
      - .env
      - .env.${NODE_ENV:-production}
    secrets:
      - db_password
      - api_key
    volumes:
      - ./logs:/app/logs
      - uploads:/app/uploads
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: ${POSTGRES_DB:-app_db}
      POSTGRES_USER: ${POSTGRES_USER:-app_user}
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
    secrets:
      - db_password
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./init-scripts:/docker-entrypoint-initdb.d
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER:-app_user} -d ${POSTGRES_DB:-app_db}"]
      interval: 10s
      timeout: 5s
      retries: 5

secrets:
  db_password:
    file: ./secrets/db_password.txt
  api_key:
    file: ./secrets/api_key.txt

volumes:
  postgres_data:
  uploads:
```

**Container Development Workflow:**

```bash
# * Container: Development workflow with container runtime detection
#!/bin/bash

# Detect container runtime
if command -v podman >/dev/null 2>&1; then
    CONTAINER_RUNTIME="podman"
elif command -v rancher >/dev/null 2>&1; then
    CONTAINER_RUNTIME="rancher"
elif command -v docker >/dev/null 2>&1; then
    CONTAINER_RUNTIME="docker"
else
    echo "Error: No container runtime found"
    exit 1
fi

echo "Using container runtime: $CONTAINER_RUNTIME"

# Build development image
build_dev() {
    $CONTAINER_RUNTIME build -t app:dev -f Dockerfile.dev .
}

# Run development container with live reload
run_dev() {
    $CONTAINER_RUNTIME run -it --rm \
        -p 3000:3000 \
        -v $(pwd):/app \
        -v /app/node_modules \
        --env-file .env.development \
        app:dev
}

# Build production image
build_prod() {
    $CONTAINER_RUNTIME build -t app:prod .
}

# Run production container
run_prod() {
    $CONTAINER_RUNTIME run -d \
        --name app-prod \
        -p 3000:3000 \
        --env-file .env.production \
        --restart unless-stopped \
        app:prod
}

# Container health checks
health_check() {
    $CONTAINER_RUNTIME exec app-prod curl -f http://localhost:3000/health
}

# Container logs
logs() {
    $CONTAINER_RUNTIME logs -f app-prod
}

# Cleanup
cleanup() {
    $CONTAINER_RUNTIME stop app-prod 2>/dev/null || true
    $CONTAINER_RUNTIME rm app-prod 2>/dev/null || true
    $CONTAINER_RUNTIME rmi app:dev app:prod 2>/dev/null || true
}

case "$1" in
    "build-dev") build_dev ;;
    "run-dev") run_dev ;;
    "build-prod") build_prod ;;
    "run-prod") run_prod ;;
    "health") health_check ;;
    "logs") logs ;;
    "cleanup") cleanup ;;
    *) echo "Usage: $0 {build-dev|run-dev|build-prod|run-prod|health|logs|cleanup}" ;;
esac
```

**Container Security Best Practices:**

```dockerfile
# * Security: Container security hardening
FROM node:18-alpine

# Update packages and remove cache
RUN apk update && apk upgrade && \
    apk add --no-cache dumb-init && \
    rm -rf /var/cache/apk/*

# Create non-root user with specific UID/GID
RUN addgroup -g 1001 -S appgroup && \
    adduser -S appuser -u 1001 -G appgroup

# Set secure file permissions
COPY --chown=appuser:appgroup . /app
WORKDIR /app

# Install dependencies as root, then switch to non-root
RUN npm ci --only=production && \
    npm cache clean --force

# Switch to non-root user
USER appuser

# Use dumb-init for proper signal handling
ENTRYPOINT ["dumb-init", "--"]
CMD ["node", "index.js"]
```

**Production Container Monitoring:**

```javascript
// * Monitoring: Container-aware health checks and metrics
class ContainerHealth {
    static async getHealthStatus() {
        const health = {
            status: 'healthy',
            timestamp: new Date().toISOString(),
            uptime: process.uptime(),
            memory: process.memoryUsage(),
            version: process.env.npm_package_version || 'unknown'
        };

        // Check database connectivity
        try {
            await this.checkDatabase();
            health.database = 'connected';
        } catch (error) {
            health.status = 'unhealthy';
            health.database = 'disconnected';
            health.error = error.message;
        }

        // Check external services
        health.externalServices = await this.checkExternalServices();

        return health;
    }

    static async checkDatabase() {
        // Implement database connectivity check
    }

    static async checkExternalServices() {
        // Implement external service health checks
        return {};
    }
}

// Health endpoint for container orchestration
app.get('/health', async (req, res) => {
    try {
        const health = await ContainerHealth.getHealthStatus();
        const statusCode = health.status === 'healthy' ? 200 : 503;
        res.status(statusCode).json(health);
    } catch (error) {
        res.status(503).json({
            status: 'unhealthy',
            error: error.message,
            timestamp: new Date().toISOString()
        });
    }
});

// Graceful shutdown handling
process.on('SIGTERM', async () => {
    console.log('Received SIGTERM, starting graceful shutdown');

    // Close server
    server.close(() => {
        console.log('HTTP server closed');

        // Close database connections
        // Close other resources

        process.exit(0);
    });

    // Force exit after 10 seconds
    setTimeout(() => {
        console.error('Forced shutdown after timeout');
        process.exit(1);
    }, 10000);
});
```

## Code Quality & Analysis Tools

### Essential Development Tools Setup

**Always ensure these tools are properly configured for any project:**

```bash
# Essential code quality tools to install and configure
npm install --save-dev \
    eslint @eslint/js \
    prettier \
    husky lint-staged \
    @typescript-eslint/parser @typescript-eslint/eslint-plugin \
    eslint-plugin-security \
    eslint-plugin-import \
    eslint-plugin-node \
    eslint-plugin-promise
```

```python
# Python code quality tools (add to requirements-dev.txt)
black>=23.0.0           # Code formatting
isort>=5.12.0          # Import sorting
pylint>=2.17.0         # Linting and static analysis
mypy>=1.5.0            # Type checking
bandit>=1.7.5          # Security vulnerability scanning
safety>=2.3.0          # Dependency vulnerability scanning
pre-commit>=3.3.0      # Git hooks management
ruff>=0.0.280          # Fast Python linter (alternative to pylint)
```

### Linting & Static Analysis

**ESLint Configuration (.eslintrc.js):**

```javascript
// * Linting: Comprehensive ESLint configuration for modern projects
module.exports = {
    env: {
        browser: true,
        es2022: true,
        node: true,
    },
    extends: [
        'eslint:recommended',
        '@typescript-eslint/recommended',
        'plugin:security/recommended',
        'plugin:import/recommended',
        'plugin:import/typescript',
    ],
    parser: '@typescript-eslint/parser',
    parserOptions: {
        ecmaVersion: 'latest',
        sourceType: 'module',
        project: './tsconfig.json',
    },
    plugins: [
        '@typescript-eslint',
        'security',
        'import',
    ],
    rules: {
        // Code Quality
        'no-console': 'warn',
        'no-debugger': 'error',
        'no-unused-vars': 'off', // Handled by TypeScript
        '@typescript-eslint/no-unused-vars': 'error',

        // Security
        'security/detect-object-injection': 'error',
        'security/detect-non-literal-regexp': 'error',
        'security/detect-unsafe-regex': 'error',

        // Import Organization
        'import/order': ['error', {
            'groups': [
                'builtin',
                'external',
                'internal',
                'parent',
                'sibling',
                'index'
            ],
            'newlines-between': 'always',
        }],

        // TypeScript Specific
        '@typescript-eslint/explicit-function-return-type': 'warn',
        '@typescript-eslint/no-explicit-any': 'error',
        '@typescript-eslint/prefer-const': 'error',

        // Best Practices
        'prefer-const': 'error',
        'no-var': 'error',
        'object-shorthand': 'error',
        'prefer-template': 'error',
    },
    ignorePatterns: [
        'dist/',
        'build/',
        'node_modules/',
        '*.min.js',
    ],
};
```

**Python Linting (pyproject.toml):**

```toml
# * Linting: Python code quality configuration
[tool.pylint.main]
load-plugins = ["pylint.extensions.docparams"]
extension-pkg-whitelist = ["pydantic"]

[tool.pylint.messages_control]
disable = [
    "too-few-public-methods",
    "too-many-arguments",
    "too-many-instance-attributes",
    "missing-module-docstring",
]

[tool.pylint.format]
max-line-length = 88  # Black's default

[tool.mypy]
python_version = "3.11"
warn_return_any = true
warn_unused_configs = true
disallow_untyped_defs = true
disallow_incomplete_defs = true
check_untyped_defs = true
disallow_untyped_decorators = true
no_implicit_optional = true
warn_redundant_casts = true
warn_unused_ignores = true
warn_no_return = true
warn_unreachable = true
strict_equality = true

[tool.bandit]
exclude_dirs = ["tests", "test_*"]
skips = ["B101"]  # Skip assert_used test

[tool.ruff]
line-length = 88
target-version = "py311"
select = [
    "E",   # pycodestyle errors
    "W",   # pycodestyle warnings
    "F",   # pyflakes
    "I",   # isort
    "B",   # flake8-bugbear
    "C4",  # flake8-comprehensions
    "UP",  # pyupgrade
    "S",   # bandit (security)
]
ignore = [
    "E501",  # line too long (handled by black)
    "B008",  # do not perform function calls in argument defaults
]

[tool.ruff.per-file-ignores]
"tests/*" = ["S101"]  # Allow assert in tests
```

### Code Formatting & Prettification

**Prettier Configuration (.prettierrc):**

```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "avoid",
  "endOfLine": "lf",
  "overrides": [
    {
      "files": "*.md",
      "options": {
        "printWidth": 100,
        "proseWrap": "always"
      }
    },
    {
      "files": "*.json",
      "options": {
        "printWidth": 120
      }
    }
  ]
}
```

**Black Configuration (pyproject.toml):**

```toml
[tool.black]
line-length = 88
target-version = ['py311']
include = '\.pyi?$'
extend-exclude = '''
/(
  # directories
  \.eggs
  | \.git
  | \.mypy_cache
  | \.pytest_cache
  | \.venv
  | build
  | dist
)/
'''

[tool.isort]
profile = "black"
multi_line_output = 3
line_length = 88
known_first_party = ["your_project"]
skip_gitignore = true
```

### Type Checking

**TypeScript Configuration (tsconfig.json):**

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "node",
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "allowJs": false,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "noImplicitOverride": true,
    "removeComments": false,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true
  },
  "include": [
    "src/**/*"
  ],
  "exclude": [
    "node_modules",
    "dist",
    "**/*.test.ts",
    "**/*.spec.ts"
  ]
}
```

### Security Scanning & Vulnerability Detection

**Security Scanning Tools:**

```bash
# * Security: Install security scanning tools
npm install --save-dev \
    eslint-plugin-security \
    npm-audit-resolver \
    audit-ci

# For Python projects
pip install safety bandit semgrep
```

**Security Configuration (.github/workflows/security.yml):**

```yaml
# * Security: Automated security scanning workflow
name: Security Scanning

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run ESLint Security
        run: npx eslint . --ext .js,.ts,.tsx --format=compact

      - name: Run npm audit
        run: npm audit --audit-level=moderate

      - name: Run Security Audit
        run: npx audit-ci --moderate

      # Python security scanning
      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install Python security tools
        run: |
          pip install safety bandit

      - name: Run Bandit security scan
        run: bandit -r . -f json -o bandit-report.json || true

      - name: Run Safety check
        run: safety check --json --output safety-report.json || true

      - name: Upload security reports
        uses: actions/upload-artifact@v3
        with:
          name: security-reports
          path: |
            bandit-report.json
            safety-report.json
```

**Bandit Configuration (.bandit):**

```yaml
# * Security: Bandit configuration for Python security scanning
exclude_dirs:
  - tests
  - test_*
  - .venv
  - venv
  - env

tests:
  - B101  # Skip assert_used in production code
  - B201  # Flask debug mode
  - B501  # SSL verification
  - B601  # Parameterized shell injection

skips:
  - B101  # Allow assert in test files
```

### Pre-commit Hooks & Git Integration

**Pre-commit Configuration (.pre-commit-config.yaml):**

```yaml
# * Pre-commit: Automated code quality checks before commits
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-json
      - id: check-toml
      - id: check-xml
      - id: check-merge-conflict
      - id: check-case-conflict
      - id: check-docstring-first
      - id: debug-statements
      - id: requirements-txt-fixer

  # JavaScript/TypeScript
  - repo: https://github.com/pre-commit/mirrors-eslint
    rev: v8.44.0
    hooks:
      - id: eslint
        files: \.(js|ts|tsx)$
        types: [file]
        additional_dependencies:
          - eslint@8.44.0
          - '@typescript-eslint/parser@6.0.0'
          - '@typescript-eslint/eslint-plugin@6.0.0'

  - repo: https://github.com/pre-commit/mirrors-prettier
    rev: v3.0.0
    hooks:
      - id: prettier
        files: \.(js|ts|tsx|json|md|yml|yaml)$

  # Python
  - repo: https://github.com/psf/black
    rev: 23.7.0
    hooks:
      - id: black
        language_version: python3.11

  - repo: https://github.com/pycqa/isort
    rev: 5.12.0
    hooks:
      - id: isort
        args: ["--profile", "black"]

  - repo: https://github.com/pycqa/pylint
    rev: v2.17.4
    hooks:
      - id: pylint
        args: ["--rcfile=pyproject.toml"]

  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.5.1
    hooks:
      - id: mypy
        additional_dependencies: [types-all]

  - repo: https://github.com/PyCQA/bandit
    rev: 1.7.5
    hooks:
      - id: bandit
        args: ["-c", ".bandit"]

  # Security
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
        args: ['--baseline', '.secrets.baseline']

  # Git commit message validation
  - repo: https://github.com/commitizen-tools/commitizen
    rev: v3.6.0
    hooks:
      - id: commitizen
        stages: [commit-msg]

default_language_version:
  python: python3.11
  node: '18.17.0'
```

**Husky + Lint-staged Configuration (package.json):**

```json
{
  "scripts": {
    "prepare": "husky install",
    "lint": "eslint . --ext .js,.ts,.tsx --fix",
    "lint:check": "eslint . --ext .js,.ts,.tsx",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "type-check": "tsc --noEmit",
    "security:audit": "npm audit --audit-level=moderate",
    "security:scan": "eslint . --ext .js,.ts,.tsx --format=compact --config .eslintrc-security.js",
    "quality:check": "npm run lint:check && npm run format:check && npm run type-check && npm run security:audit"
  },
  "lint-staged": {
    "*.{js,ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{json,md,yml,yaml}": [
      "prettier --write"
    ],
    "*.py": [
      "black",
      "isort --profile black",
      "pylint",
      "mypy"
    ]
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged && npm run type-check",
      "commit-msg": "commitizen --hook || true",
      "pre-push": "npm run quality:check"
    }
  }
}
```

### IDE Integration & Development Experience

**VS Code Settings (.vscode/settings.json):**

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ],
  "python.defaultInterpreterPath": "./.venv/bin/python",
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": true,
  "python.linting.mypyEnabled": true,
  "python.linting.banditEnabled": true,
  "python.formatting.provider": "black",
  "python.sortImports.args": ["--profile", "black"],
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "files.associations": {
    ".eslintrc": "json",
    ".prettierrc": "json"
  }
}
```

**VS Code Extensions (.vscode/extensions.json):**

```json
{
  "recommendations": [
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "ms-python.python",
    "ms-python.black-formatter",
    "ms-python.isort",
    "ms-python.pylint",
    "ms-python.mypy-type-checker",
    "charliermarsh.ruff",
    "bradlc.vscode-tailwindcss",
    "christian-kohler.path-intellisense",
    "formulahendry.auto-rename-tag",
    "aaron-bond.better-comments",
    "streetsidesoftware.code-spell-checker",
    "ms-vscode.vscode-typescript-next",
    "ms-vsliveshare.vsliveshare"
  ]
}
```

### Automated Quality Gates

**GitHub Actions Quality Pipeline (.github/workflows/quality.yml):**

```yaml
# * Quality: Comprehensive code quality pipeline
name: Code Quality

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  quality:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
          cache: 'pip'

      - name: Install dependencies
        run: |
          npm ci
          pip install -r requirements-dev.txt

      # JavaScript/TypeScript Quality Checks
      - name: ESLint Check
        run: npm run lint:check

      - name: Prettier Check
        run: npm run format:check

      - name: TypeScript Check
        run: npm run type-check

      - name: Security Audit
        run: npm run security:audit

      # Python Quality Checks
      - name: Black Check
        run: black --check .

      - name: isort Check
        run: isort --check-only --profile black .

      - name: Pylint Check
        run: pylint --rcfile=pyproject.toml src/

      - name: MyPy Check
        run: mypy src/

      - name: Bandit Security Check
        run: bandit -r src/ -f json

      - name: Safety Check
        run: safety check

      # Generate quality reports
      - name: Generate ESLint Report
        run: npx eslint . --ext .js,.ts,.tsx --format=json --output-file=eslint-report.json || true

      - name: Generate Coverage Report
        run: npm run test:coverage || true

      - name: Upload Quality Reports
        uses: actions/upload-artifact@v3
        if: always()
        with:
          name: quality-reports
          path: |
            eslint-report.json
            coverage/
            bandit-report.json
            safety-report.json
```

### Quality Metrics & Monitoring

**SonarQube Configuration (sonar-project.properties):**

```properties
# * Quality: SonarQube configuration for continuous code quality
sonar.projectKey=your-project-key
sonar.projectName=Your Project Name
sonar.projectVersion=1.0

# Source and test directories
sonar.sources=src/
sonar.tests=tests/,src/**/*.test.ts,src/**/*.spec.ts
sonar.exclusions=**/*.min.js,**/node_modules/**,**/dist/**,**/build/**

# Language-specific settings
sonar.typescript.lcov.reportPaths=coverage/lcov.info
sonar.python.coverage.reportPaths=coverage.xml
sonar.python.xunit.reportPath=test-results.xml

# Quality gates
sonar.qualitygate.wait=true
sonar.coverage.exclusions=**/*.test.ts,**/*.spec.ts,**/test_*.py
```

### Development Automation Scripts

**Quality Check Scripts:**

```bash
#!/bin/bash
# * Quality: Comprehensive quality check script

set -e

echo "🔍 Running comprehensive quality checks..."

# JavaScript/TypeScript checks
if [ -f "package.json" ]; then
    echo "📋 Checking JavaScript/TypeScript..."
    npm run lint:check
    npm run format:check
    npm run type-check
    npm run security:audit
    echo "✅ JavaScript/TypeScript checks passed"
fi

# Python checks
if [ -f "requirements.txt" ] || [ -f "pyproject.toml" ]; then
    echo "🐍 Checking Python..."
    black --check .
    isort --check-only --profile black .
    pylint --rcfile=pyproject.toml src/
    mypy src/
    bandit -r src/
    safety check
    echo "✅ Python checks passed"
fi

# Git hooks validation
if [ -d ".git" ]; then
    echo "🪝 Validating Git hooks..."
    if command -v pre-commit &> /dev/null; then
        pre-commit run --all-files
    fi
    echo "✅ Git hooks validation passed"
fi

echo "🎉 All quality checks passed!"
```

```powershell
# * Quality: PowerShell quality check script for Windows
param(
    [switch]$Fix,
    [switch]$SkipTests
)

Write-Host "🔍 Running comprehensive quality checks..." -ForegroundColor Cyan

# JavaScript/TypeScript checks
if (Test-Path "package.json") {
    Write-Host "📋 Checking JavaScript/TypeScript..." -ForegroundColor Yellow

    if ($Fix) {
        npm run lint
        npm run format
    } else {
        npm run lint:check
        npm run format:check
    }

    npm run type-check
    npm run security:audit
    Write-Host "✅ JavaScript/TypeScript checks passed" -ForegroundColor Green
}

# Python checks
if ((Test-Path "requirements.txt") -or (Test-Path "pyproject.toml")) {
    Write-Host "🐍 Checking Python..." -ForegroundColor Yellow

    if ($Fix) {
        black .
        isort --profile black .
    } else {
        black --check .
        isort --check-only --profile black .
    }

    pylint --rcfile=pyproject.toml src/
    mypy src/
    bandit -r src/
    safety check
    Write-Host "✅ Python checks passed" -ForegroundColor Green
}

Write-Host "🎉 All quality checks passed!" -ForegroundColor Green
```

## Adaptive Learning & Evolution

### Self-Improving Instructions

**Dynamic Guidelines Discovery:**
When you (GitHub Copilot) notice patterns, preferences, or recurring issues during development sessions, suggest additions to these instructions:

```markdown
// * DISCOVERED PATTERN: [Brief description of observed pattern]
// * SUGGESTED INSTRUCTION: [Proposed guideline or best practice]
// * REASONING: [Why this would improve development workflow]
// * FREQUENCY: [How often this pattern/issue occurs]

Example format for suggestions:
"I've noticed you frequently [specific behavior/pattern].
Should we add an instruction about [proposed guideline]
to improve [specific aspect] in future sessions?"
```

**Learning Categories:**

- **Code Patterns**: Frequently used structures or architectures
- **Error Patterns**: Common mistakes or issues that arise
- **Workflow Preferences**: Preferred ways of organizing or executing tasks
- **Tool Usage**: Effective combinations of tools or commands
- **Performance Insights**: Optimizations that prove effective
- **Security Practices**: Security measures that get overlooked

**Instruction Evolution Process:**

1. **Observe**: Notice recurring patterns or issues
2. **Analyze**: Determine if pattern represents a best practice or common pitfall
3. **Suggest**: Propose specific instruction language
4. **Validate**: Confirm the instruction would improve future development
5. **Integrate**: Add to appropriate section of guidelines

This creates a "living document" that evolves based on actual development experience and discovered best practices.

---

## Project-Specific Customization Notes

**To customize this template for your project:**

1. **Environment Context**: Update OS, shell, project type, and build system
2. **Command Guidelines**: Adjust for your platform (Windows PowerShell vs Linux bash)
3. **Database Operations**: Customize database paths and safety procedures
4. **File Organization**: Add project-specific file structure guidelines
5. **Development Patterns**: Include project-specific architectural patterns
6. **Testing Strategy**: Add project-specific testing guidelines
7. **Deployment**: Include project-specific deployment procedures

## Template Cleanup & Maintenance

**After initial project setup, remove completed setup instructions:**

### Setup Instructions to Remove

- **Linting configurations** - Once `.eslintrc.js`, `.prettierrc`, `pyproject.toml` are configured
- **Git configuration examples** - After git aliases and hooks are set up
- **Container setup scripts** - Once Dockerfile and docker-compose.yml are created
- **Package installation commands** - After `package.json` and `requirements.txt` are finalized
- **Initial environment setup** - Once `.env` files and environment variables are configured
- **Tool installation guides** - After development tools are installed and working

### Instructions to Keep

- **Code quality standards** - Commenting guidelines, error handling patterns
- **Workflow guidelines** - Git workflows, commit strategies, branch management
- **Database safety rules** - Production protection, backup procedures
- **Security practices** - Environment variable usage, secrets management
- **Development patterns** - Architecture decisions, performance guidelines
- **Adaptive learning section** - For continuous improvement

### Cleanup Process

```markdown
// * SETUP COMPLETE: [Date] - Remove this section after confirming:
// * ✅ Linting tools configured and working
// * ✅ Git hooks installed and tested
// * ✅ Container environment verified
// * ✅ Development tools installed
// * ✅ Environment variables configured
// * ✅ All team members onboarded

// Keep only operational guidelines, remove setup instructions
```

### Periodic Review

- **Monthly**: Review and update workflow guidelines based on team feedback
- **Quarterly**: Assess if new tools or patterns should be added
- **After major changes**: Update instructions to reflect new architecture or processes
- **Team growth**: Ensure instructions remain clear for new team members

### Version Control for Instructions

```bash
# Track changes to copilot instructions
git add .github/copilot-instructions.md
git commit -m "docs(copilot): update workflow guidelines after Q3 retrospective

- remove outdated linting setup instructions
- add new performance monitoring guidelines
- update container deployment procedures"
```

**Remember**: These instructions are designed to be read by AI assistants, not humans. They should be comprehensive and detailed to provide maximum context for automated assistance.
