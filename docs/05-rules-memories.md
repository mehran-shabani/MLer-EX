# قوانین (Rules) و حافظه‌ها (Memories) در Cursor

## قوانین (Rules) چیست؟

قوانین دستورالعمل‌های سفارشی هستند که رفتار AI را در پروژه شما کنترل می‌کنند. این قوانین به AI می‌گویند چگونه کد بنویسد، از چه الگوهایی پیروی کند و چه استانداردهایی را رعایت کند.

## انواع قوانین

### 1. قوانین سطح پروژه (Project Rules)

در فایل `.cursorrules` در ریشه پروژه:

```markdown
# .cursorrules

## Code Style
- Use TypeScript with strict mode enabled
- Prefer functional components over class components
- Use async/await instead of promises
- Always handle errors with try-catch blocks

## Architecture
- Follow Clean Architecture principles
- Separate business logic from UI components
- Use dependency injection pattern
- Keep functions under 20 lines

## Naming Conventions
- Components: PascalCase (UserProfile)
- Functions: camelCase (getUserData)
- Constants: UPPER_SNAKE_CASE (MAX_RETRY_COUNT)
- Files: kebab-case (user-profile.tsx)

## Testing
- Write unit tests for all utility functions
- Use React Testing Library for components
- Maintain 80% code coverage
- Follow AAA pattern (Arrange, Act, Assert)
```

### 2. قوانین کاربری (User Rules)

در تنظیمات Cursor:

```json
{
  "cursor.rules.user": [
    "Always use Persian comments in code",
    "Prefer RTL-friendly CSS solutions",
    "Use Vazir font for Persian text",
    "Include accessibility features"
  ]
}
```

### 3. قوانین Workspace

برای تیم‌ها و پروژه‌های مشترک:

```json
// .vscode/settings.json
{
  "cursor.rules.workspace": [
    "Follow company coding standards",
    "Use approved libraries only",
    "Include JIRA ticket number in commits",
    "Document all public APIs"
  ]
}
```

## مثال‌های عملی Rules

### برای React پروژه

```markdown
# .cursorrules

## React Best Practices
- Use functional components with hooks
- Implement error boundaries for critical components
- Use React.memo for expensive components
- Prefer composition over inheritance

## State Management
- Use Context API for simple global state
- Implement Redux Toolkit for complex state
- Keep component state local when possible
- Use custom hooks for reusable logic

## Performance
- Lazy load routes and heavy components
- Optimize images with next/image
- Use useCallback and useMemo appropriately
- Implement virtual scrolling for long lists

## Code Example Template
When creating a new component, use this structure:

```tsx
import React, { useState, useEffect } from 'react';
import styles from './ComponentName.module.css';

interface ComponentNameProps {
  // Props definition
}

export const ComponentName: React.FC<ComponentNameProps> = ({ ...props }) => {
  // State and hooks
  const [state, setState] = useState();
  
  // Effects
  useEffect(() => {
    // Effect logic
  }, []);
  
  // Handlers
  const handleAction = () => {
    // Handler logic
  };
  
  // Render
  return (
    <div className={styles.container}>
      {/* Component JSX */}
    </div>
  );
};
```
```

### برای Node.js/Express

```markdown
# .cursorrules

## API Development
- Use async/await for all asynchronous operations
- Implement proper error handling middleware
- Validate all inputs with Joi or Zod
- Use environment variables for configuration

## Security
- Implement rate limiting on all endpoints
- Use helmet.js for security headers
- Sanitize all user inputs
- Hash passwords with bcrypt (min 10 rounds)

## Database
- Use transactions for multi-step operations
- Implement soft deletes
- Add indexes for frequently queried fields
- Use migrations for schema changes

## Response Format
All API responses should follow this format:
```json
{
  "success": boolean,
  "data": {} | [],
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message"
  },
  "metadata": {
    "timestamp": "ISO 8601",
    "version": "1.0.0"
  }
}
```
```

### برای Python/Django

```markdown
# .cursorrules

## Django Standards
- Use class-based views for complex logic
- Implement custom managers for models
- Use Django REST Framework for APIs
- Follow PEP 8 style guide

## Models
- Add verbose names for all fields
- Implement str method for all models
- Use UUID for public identifiers
- Add created_at and updated_at to all models

## Testing
- Use pytest instead of unittest
- Mock external services
- Test all endpoints and models
- Use factories for test data
```

## حافظه‌ها (Memories)

### حافظه چیست؟

حافظه‌ها قوانین خودکاری هستند که Cursor از مکالمات شما یاد می‌گیرد و ذخیره می‌کند. این ویژگی به AI کمک می‌کند تا:
- تصمیمات گذشته را به خاطر بسپارد
- الگوهای کدنویسی شما را یاد بگیرد
- در مکالمات آینده context بهتری داشته باشد

### نحوه کارکرد Memories

1. **ایجاد خودکار**: وقتی در Chat تصمیمی می‌گیرید
2. **ذخیره‌سازی**: به صورت محلی در پروژه
3. **استفاده مجدد**: در مکالمات آینده

### مثال‌های Memory

```markdown
## Memories (Auto-generated)

### API Design Decision
- Date: 2024-01-15
- Decision: Use GraphQL instead of REST for main API
- Reason: Better performance for nested data queries

### Authentication Method
- Date: 2024-01-20
- Decision: Implement JWT with refresh tokens
- Details: Access token: 15min, Refresh token: 7 days

### Database Choice
- Date: 2024-01-22
- Decision: PostgreSQL for main data, Redis for caching
- Migration tool: Alembic
```

### مدیریت Memories

#### مشاهده Memories
```bash
Ctrl+Shift+P → "Show Memories"
```

#### ویرایش Memories
```json
// .cursor/memories.json
{
  "memories": [
    {
      "id": "mem_123",
      "content": "Always use RTL CSS for Persian content",
      "context": "UI Development",
      "created": "2024-01-15T10:00:00Z",
      "confidence": 0.95
    }
  ]
}
```

#### حذف Memories
- از UI: در پنل Memories
- دستی: حذف از فایل memories.json

## بهترین شیوه‌ها برای Rules

### 1. واضح و مشخص بنویسید
```markdown
❌ Bad: "Write good code"
✅ Good: "Use meaningful variable names with at least 3 characters"
```

### 2. مثال‌های کد ارائه دهید
```markdown
## Error Handling
Always wrap async operations:
```javascript
try {
  const result = await someAsyncOperation();
  return { success: true, data: result };
} catch (error) {
  logger.error('Operation failed:', error);
  return { success: false, error: error.message };
}
```
```

### 3. اولویت‌بندی کنید
```markdown
## Priority Rules
1. [CRITICAL] Never expose sensitive data in logs
2. [HIGH] Always validate user input
3. [MEDIUM] Prefer const over let
4. [LOW] Use descriptive variable names
```

### 4. برای تیم قابل فهم باشد
```markdown
## Team Conventions
- Branch naming: feature/JIRA-123-description
- PR template: docs/pull_request_template.md
- Code review: Minimum 2 approvals required
```

## Rules پیشرفته

### استفاده از متغیرها
```markdown
## Configuration
PROJECT_NAME = "MyAwesomeApp"
API_VERSION = "v2"
MAX_FILE_SIZE = "10MB"

## Rules
- All API endpoints should start with /api/${API_VERSION}
- File uploads limited to ${MAX_FILE_SIZE}
- Use ${PROJECT_NAME} in all documentation
```

### Rules شرطی
```markdown
## Environment-specific Rules

### IF environment === "production"
- Enable all security features
- Use CDN for static assets
- Implement rate limiting: 100 req/min

### IF environment === "development"
- Enable debug mode
- Use local database
- Skip email verification
```

### Rules با الگو
```markdown
## File Structure Pattern
For each new feature, create:
```
src/
  features/
    ${FEATURE_NAME}/
      components/
        ${FEATURE_NAME}.tsx
        ${FEATURE_NAME}.test.tsx
      hooks/
        use${FEATURE_NAME}.ts
      services/
        ${FEATURE_NAME}Service.ts
      types/
        ${FEATURE_NAME}.types.ts
```
```

## ترکیب Rules و Memories

### سناریوی عملی

1. **تعریف Rule اولیه**:
```markdown
# .cursorrules
Use Material-UI for all UI components
```

2. **تصمیم در Chat**:
```
You: "Let's use Tailwind instead of Material-UI for this project"
Cursor: "Understood. I'll use Tailwind CSS for styling."
```

3. **Memory ایجاد می‌شود**:
```json
{
  "decision": "Use Tailwind CSS instead of Material-UI",
  "overrides": "rule:use-material-ui",
  "date": "2024-01-25"
}
```

4. **در مکالمات آینده**: Cursor از Tailwind استفاده می‌کند

## Debugging Rules

### بررسی Rules فعال
```bash
Ctrl+Shift+P → "Show Active Rules"
```

### تست Rules
```bash
# در Chat بپرسید:
"What rules are you following for this project?"
"Show me how you would implement a new component based on current rules"
```

### رفع تداخل Rules
اولویت Rules:
1. Inline instructions (در Chat)
2. Memories
3. Project rules (.cursorrules)
4. Workspace rules
5. User rules

## نکات کاربردی

1. **Rules را ساده نگه دارید**: خیلی پیچیده نکنید
2. **به‌روزرسانی منظم**: با تغییر پروژه، Rules را update کنید
3. **Document کنید**: دلیل هر Rule را بنویسید
4. **تیمی فکر کنید**: Rules باید برای همه قابل فهم باشد
5. **تست کنید**: مطمئن شوید AI درست متوجه می‌شود