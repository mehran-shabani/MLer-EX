# نکات پیشرفته Cursor

## 1. بهینه‌سازی Context

### درک Context Window

Context مقدار اطلاعاتی است که AI می‌تواند همزمان پردازش کند. مدیریت صحیح آن کلید عملکرد بهتر است.

#### اندازه Context مدل‌ها
- **GPT-4**: 8,000 توکن
- **GPT-4-32k**: 32,000 توکن
- **Claude**: 100,000 توکن
- **Cursor Small**: 2,000 توکن (برای تکمیل سریع)

#### بهینه‌سازی Context

```javascript
// ❌ Bad: Context پر از اطلاعات غیرضروری
// تمام فایل 1000 خطی را include نکنید

// ✅ Good: فقط بخش‌های مرتبط
@file:auth.js:45-89  // فقط تابع authentication
@file:types.ts:UserType  // فقط type مورد نیاز
```

### استراتژی‌های Context

#### 1. Selective Context
```bash
# به جای:
@folder:src  # همه فایل‌ها

# استفاده کنید:
@file:src/components/Header.tsx
@file:src/hooks/useAuth.ts
@file:src/api/userAPI.ts
```

#### 2. Symbol-based Context
```bash
# ارجاع به symbols خاص
@symbol:calculateTotalPrice
@symbol:UserAuthenticationService
@symbol:type:OrderStatus
```

#### 3. Pattern-based Context
```bash
# فقط فایل‌های test
@pattern:**/*.test.ts

# فقط components
@pattern:src/components/**/*.tsx
```

## 2. Prompt Engineering پیشرفته

### ساختار Prompt مؤثر

#### 1. دستورات Clear و Specific
```bash
❌ "Make this better"
✅ "Refactor this function to:
   1. Handle null inputs gracefully
   2. Add TypeScript types
   3. Reduce complexity to under 10 lines
   4. Add JSDoc comments"
```

#### 2. استفاده از Examples
```bash
"Convert this class component to functional component.
Example pattern:
- Replace state with useState
- Replace lifecycle methods with useEffect
- Keep the same prop interface"
```

#### 3. تعیین Output Format
```bash
"Create a REST API endpoint for user registration.
Output should include:
- Route definition
- Input validation
- Error handling
- Success response format
- Unit test example"
```

### Chain of Thought Prompting

```bash
"Let's build a caching system step by step:
1. First, what are the requirements?
2. What caching strategy should we use?
3. How do we handle cache invalidation?
4. Implement the solution with these considerations"
```

## 3. Multi-file Refactoring

### استراتژی‌های Refactoring بزرگ

#### 1. تقسیم به مراحل
```bash
# Phase 1: شناسایی
"Find all places where the old API is used"

# Phase 2: طراحی
"Design the new API structure maintaining backward compatibility"

# Phase 3: پیاده‌سازی
"Implement the new API with the designed structure"

# Phase 4: مهاجرت
"Migrate existing code to use the new API"

# Phase 5: پاکسازی
"Remove deprecated code and update tests"
```

#### 2. استفاده از Checkpoints
```bash
# قبل از هر تغییر بزرگ
1. ایجاد checkpoint
2. انجام refactoring
3. تست
4. در صورت مشکل، بازگشت به checkpoint
```

#### 3. Batch Operations
```javascript
// دستور به Agent:
"For all React components in src/components:
1. Convert class components to functional
2. Replace componentDidMount with useEffect
3. Update imports to use named exports
4. Add proper TypeScript types"
```

## 4. Performance Optimization

### تنظیمات برای پروژه‌های بزرگ

```json
{
  // Indexing
  "cursor.indexing.exclude": [
    "**/node_modules",
    "**/dist",
    "**/build",
    "**/.git",
    "**/coverage"
  ],
  "cursor.indexing.maxFileSizeKB": 200,
  
  // Context
  "cursor.context.maxTokens": 4000,
  "cursor.context.smartSelection": true,
  
  // Tab completion
  "cursor.tab.prefetchCount": 2,
  "cursor.tab.cacheResults": true
}
```

### Memory Management

#### 1. محدود کردن فایل‌های باز
```bash
# بستن فایل‌های غیرضروری
Ctrl+K → W  # بستن فایل فعلی
Ctrl+K → Ctrl+W  # بستن همه فایل‌ها
```

#### 2. پاکسازی Cache
```bash
# دستور Command Palette
"Developer: Reload Window"
"Clear Editor History"
```

## 5. Custom Workflows

### ایجاد Workflow Templates

#### 1. Feature Development Workflow
```markdown
# .cursor/workflows/new-feature.md

## New Feature Checklist
1. [ ] Create feature branch
2. [ ] Design component structure
3. [ ] Implement with TDD
4. [ ] Add documentation
5. [ ] Create PR with tests

## Commands
- Create structure: "Generate folder structure for {feature}"
- Add tests: "Create comprehensive tests for this component"
- Document: "Add JSDoc comments and README"
```

#### 2. Bug Fix Workflow
```markdown
# .cursor/workflows/bug-fix.md

## Bug Fix Process
1. [ ] Reproduce the issue
2. [ ] Write failing test
3. [ ] Fix the bug
4. [ ] Verify all tests pass
5. [ ] Check for regressions

## AI Commands
- "Help me understand why this bug occurs"
- "Suggest a fix that won't break existing functionality"
- "Generate edge case tests for this fix"
```

### Automation Scripts

```javascript
// .cursor/scripts/optimize-imports.js
module.exports = {
  name: 'Optimize Imports',
  run: async (cursor) => {
    const files = await cursor.findFiles('**/*.{ts,tsx,js,jsx}');
    
    for (const file of files) {
      await cursor.executeCommand('cursor.inlineEdit', {
        file: file,
        instruction: 'Optimize imports: remove unused, sort alphabetically, group by type'
      });
    }
  }
};
```

## 6. Team Collaboration

### Shared Configuration

#### 1. تنظیمات تیمی
```json
// .vscode/settings.json
{
  "cursor.team.sharedRules": true,
  "cursor.team.codeReviewMode": true,
  "cursor.team.defaultReviewers": ["senior-dev", "tech-lead"]
}
```

#### 2. Code Review با AI
```bash
# در PR comments:
"@cursor review this PR for:
- Security vulnerabilities
- Performance issues
- Code style violations
- Missing tests"
```

### Knowledge Sharing

```markdown
# .cursor/team-knowledge.md

## Project Patterns
- Authentication: JWT with refresh tokens
- State Management: Redux Toolkit
- API Calls: RTK Query
- Styling: CSS Modules with Tailwind

## Common Issues & Solutions
1. CORS Error: Check API_URL in .env
2. Build Failure: Clear node_modules and reinstall
3. Test Timeout: Increase jest timeout in config
```

## 7. Security Best Practices

### محافظت از اطلاعات حساس

#### 1. استفاده از .cursorignore
```bash
# .cursorignore
.env
.env.*
*.pem
*.key
secrets/
credentials/
```

#### 2. تنظیمات Privacy
```json
{
  "cursor.privacy.shareCode": false,
  "cursor.privacy.shareTelemetry": false,
  "cursor.privacy.localProcessing": true
}
```

### Code Security Scanning

```bash
# استفاده از AI برای security audit
"Scan this code for:
- SQL injection vulnerabilities
- XSS possibilities
- Authentication bypasses
- Sensitive data exposure"
```

## 8. Debugging با AI

### استراتژی‌های Debug

#### 1. Error Analysis
```javascript
// خطا را paste کنید و بپرسید:
"Error: Cannot read property 'map' of undefined at line 45
Stack trace: [...]

Analyze this error and suggest:
1. Root cause
2. How to fix
3. How to prevent similar errors"
```

#### 2. Performance Profiling
```bash
"This function is slow. Help me:
1. Identify bottlenecks
2. Suggest optimizations
3. Add performance monitoring"
```

#### 3. Memory Leak Detection
```bash
"Analyze this component for potential memory leaks:
- Event listeners not removed
- Intervals/timeouts not cleared
- Large objects retained in closures"
```

## 9. Learning & Improvement

### استفاده از Cursor برای یادگیری

#### 1. Code Explanation
```bash
"Explain this code like I'm a beginner:
- What does each part do?
- Why is it written this way?
- What are the alternatives?"
```

#### 2. Best Practices Learning
```bash
"Show me best practices for:
- Error handling in async functions
- State management in React
- API design in REST"
```

#### 3. Code Review Learning
```bash
"Review my code and teach me:
- What I did well
- What could be improved
- Industry standards I should follow"
```

## 10. Advanced Shortcuts

### ترکیب‌های کمتر شناخته شده

```bash
# Multi-cursor productivity
Alt+Shift+I         # Insert cursor at end of each selected line
Ctrl+Alt+Up/Down    # Add cursor above/below
Ctrl+D              # Select next occurrence
Ctrl+Shift+L        # Select all occurrences

# Navigation shortcuts
Ctrl+K Ctrl+0       # Fold all regions
Ctrl+K Ctrl+J       # Unfold all regions
Alt+Z               # Toggle word wrap
Ctrl+K Z            # Zen mode

# AI-specific shortcuts
Ctrl+Shift+Space    # Trigger AI suggestion manually
Alt+\               # Toggle AI suggestions
Ctrl+Alt+Enter      # Accept partial suggestion
```

## نتیجه‌گیری

### چک‌لیست مهارت پیشرفته

- [ ] مدیریت مؤثر Context
- [ ] Prompt engineering حرفه‌ای
- [ ] Multi-file operations
- [ ] Performance optimization
- [ ] Custom workflows
- [ ] Team collaboration
- [ ] Security awareness
- [ ] Advanced debugging
- [ ] Continuous learning

### منابع یادگیری بیشتر

1. [Cursor Forum](https://forum.cursor.com) - بحث‌های تکنیکی
2. [GitHub Discussions](https://github.com/cursor/cursor/discussions) - ایده‌ها و feedback
3. [YouTube Tutorials](https://youtube.com/@cursor) - ویدیوهای آموزشی
4. [Twitter/X](https://twitter.com/cursor_ai) - آخرین اخبار و tips