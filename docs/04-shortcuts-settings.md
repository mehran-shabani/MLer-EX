# میانبرها و تنظیمات Cursor

## میانبرهای کلیدی اصلی

### میانبرهای AI

| عملکرد | Windows/Linux | macOS |
|--------|---------------|--------|
| تکمیل خودکار (Tab) | Tab | Tab |
| ویرایش درون‌خطی | Ctrl+K | Cmd+K |
| باز کردن Agent | Ctrl+I | Cmd+I |
| باز کردن Chat | Ctrl+L | Cmd+L |
| تب جدید Chat | Ctrl+T | Cmd+T |
| پذیرش تکمیل جزئی | Ctrl+→ | Cmd+→ |
| رد تکمیل | ESC | ESC |

### میانبرهای عمومی

| عملکرد | Windows/Linux | macOS |
|--------|---------------|--------|
| Command Palette | Ctrl+Shift+P | Cmd+Shift+P |
| Quick Open | Ctrl+P | Cmd+P |
| جستجو در فایل | Ctrl+F | Cmd+F |
| جستجو در پروژه | Ctrl+Shift+F | Cmd+Shift+F |
| Settings | Ctrl+, | Cmd+, |
| نمایش/مخفی Sidebar | Ctrl+B | Cmd+B |
| Terminal | Ctrl+` | Cmd+` |

### میانبرهای ویرایش

| عملکرد | Windows/Linux | macOS |
|--------|---------------|--------|
| انتخاب چندگانه | Alt+Click | Option+Click |
| کپی خط | Ctrl+C (بدون انتخاب) | Cmd+C |
| حذف خط | Ctrl+Shift+K | Cmd+Shift+K |
| جابجایی خط | Alt+↑/↓ | Option+↑/↓ |
| تکرار خط | Alt+Shift+↑/↓ | Option+Shift+↑/↓ |
| Comment/Uncomment | Ctrl+/ | Cmd+/ |
| Format Document | Alt+Shift+F | Option+Shift+F |

### میانبرهای Navigation

| عملکرد | Windows/Linux | macOS |
|--------|---------------|--------|
| Go to Definition | F12 | F12 |
| Peek Definition | Alt+F12 | Option+F12 |
| Go to Line | Ctrl+G | Cmd+G |
| Go Back | Alt+← | Ctrl+- |
| Go Forward | Alt+→ | Ctrl+Shift+- |
| Next Error | F8 | F8 |
| Previous Error | Shift+F8 | Shift+F8 |

## تنظیمات مهم Cursor

### 1. تنظیمات AI Models

```json
{
  // انتخاب مدل پیش‌فرض Chat
  "cursor.chat.defaultModel": "gpt-4",
  
  // مدل برای تکمیل خودکار
  "cursor.tab.model": "cursor-small",
  
  // مدل برای ویرایش درون‌خطی
  "cursor.inlineEdit.model": "gpt-4",
  
  // استفاده از مدل‌های محلی
  "cursor.useLocalModels": false
}
```

### 2. تنظیمات Tab Completion

```json
{
  // فعال/غیرفعال کردن Tab
  "cursor.tab.enabled": true,
  
  // تاخیر قبل از نمایش پیشنهاد (ms)
  "cursor.tab.debounceDelay": 300,
  
  // حداقل کاراکتر برای شروع
  "cursor.tab.minCharacters": 2,
  
  // پذیرش با Space
  "cursor.tab.acceptOnSpace": false,
  
  // نمایش در comments
  "cursor.tab.showInComments": true,
  
  // نمایش در strings
  "cursor.tab.showInStrings": false
}
```

### 3. تنظیمات Agent

```json
{
  // اجرای خودکار دستورات terminal
  "cursor.agent.terminal.autoExecute": false,
  
  // نیاز به تایید برای دستورات
  "cursor.agent.terminal.requireConfirmation": true,
  
  // حداکثر زمان اجرا (ms)
  "cursor.agent.terminal.maxExecutionTime": 30000,
  
  // حداکثر فایل برای تغییر همزمان
  "cursor.agent.maxFilesToEdit": 10,
  
  // نمایش diff قبل از اعمال
  "cursor.agent.showDiffBeforeApply": true
}
```

### 4. تنظیمات Privacy

```json
{
  // ارسال telemetry
  "cursor.telemetry.enabled": false,
  
  // ذخیره history در cloud
  "cursor.cloud.syncHistory": false,
  
  // استفاده از مدل‌های آنلاین
  "cursor.useOnlineModels": true,
  
  // حالت محلی (بدون اتصال)
  "cursor.localMode": false
}
```

### 5. تنظیمات Context

```json
{
  // حداکثر اندازه context (tokens)
  "cursor.context.maxTokens": 8000,
  
  // شامل کردن فایل‌های باز
  "cursor.context.includeOpenTabs": true,
  
  // شامل کردن تاریخچه اخیر
  "cursor.context.includeRecentHistory": true,
  
  // تعداد خطوط اطراف برای context
  "cursor.context.surroundingLines": 50
}
```

### 6. تنظیمات UI

```json
{
  // نمایش hints برای Tab
  "cursor.ui.showTabHints": true,
  
  // نمایش status در status bar
  "cursor.ui.showStatusBar": true,
  
  // رنگ highlight برای AI suggestions
  "cursor.ui.suggestionHighlightColor": "#4a9eff",
  
  // شفافیت پیشنهادات
  "cursor.ui.suggestionOpacity": 0.7
}
```

## سفارشی‌سازی میانبرها

### نحوه تغییر میانبرها

1. **باز کردن Keyboard Shortcuts:**
   ```
   Ctrl+K → Ctrl+S
   یا
   File → Preferences → Keyboard Shortcuts
   ```

2. **جستجوی command:**
   ```
   در search box نام command را تایپ کنید
   ```

3. **تغییر keybinding:**
   ```
   روی مداد کلیک کنید
   کلید جدید را فشار دهید
   Enter بزنید
   ```

### مثال: تنظیمات سفارشی

```json
// keybindings.json
[
  {
    "key": "alt+enter",
    "command": "cursor.acceptSuggestion",
    "when": "cursorTabVisible"
  },
  {
    "key": "ctrl+shift+k",
    "command": "cursor.inlineEdit",
    "when": "editorTextFocus"
  },
  {
    "key": "ctrl+alt+i",
    "command": "cursor.openAgent",
    "when": "editorFocus"
  }
]
```

## Profiles و Workspaces

### ایجاد Profile جدید

```bash
# برای پروژه‌های مختلف
File → Preferences → Profiles → Create Profile

# تنظیمات مختلف برای:
- Frontend Development
- Backend Development
- Data Science
- DevOps
```

### Workspace Settings

```json
// .vscode/settings.json در ریشه پروژه
{
  "cursor.rules.workspace": [
    "Use TypeScript strict mode",
    "Follow ESLint rules",
    "Use functional components in React"
  ],
  
  "cursor.agent.autoSuggestCommands": true,
  
  "cursor.tab.projectSpecificModel": "gpt-4"
}
```

## Extensions پیشنهادی

### برای JavaScript/TypeScript
- ESLint
- Prettier
- Path Intellisense
- Auto Rename Tag
- JavaScript (ES6) code snippets

### برای Python
- Python
- Pylance
- Black Formatter
- Python Docstring Generator

### برای Git
- GitLens
- Git History
- Git Graph

### برای UI/UX
- Material Icon Theme
- One Dark Pro
- Bracket Pair Colorizer

## بهینه‌سازی Performance

### 1. کاهش مصرف حافظه
```json
{
  "files.watcherExclude": {
    "**/node_modules/**": true,
    "**/.git/**": true,
    "**/dist/**": true
  },
  
  "search.exclude": {
    "**/node_modules": true,
    "**/bower_components": true
  }
}
```

### 2. تنظیمات برای پروژه‌های بزرگ
```json
{
  "cursor.indexing.maxFileSizeKB": 500,
  "cursor.indexing.excludePatterns": [
    "node_modules",
    "dist",
    "build",
    ".git"
  ]
}
```

### 3. بهینه‌سازی تکمیل خودکار
```json
{
  "cursor.tab.cacheSize": 100,
  "cursor.tab.prefetchCount": 3,
  "cursor.tab.maxSuggestionsPerFile": 5
}
```

## Troubleshooting تنظیمات

### مشکلات رایج و راه‌حل

1. **میانبرها کار نمی‌کنند:**
   - بررسی conflicts در Keyboard Shortcuts
   - Reset به default keybindings

2. **AI کند است:**
   - کاهش context size
   - استفاده از مدل‌های سبک‌تر
   - بررسی اتصال اینترنت

3. **مصرف زیاد RAM:**
   - غیرفعال کردن extension های غیرضروری
   - کاهش تعداد فایل‌های باز
   - استفاده از workspace برای پروژه‌های بزرگ