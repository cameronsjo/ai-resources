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

**Remember**: These instructions are designed to be read by AI assistants, not humans. They should be comprehensive and detailed to provide maximum context for automated assistance.
