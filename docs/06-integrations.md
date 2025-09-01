# ادغام‌ها و یکپارچه‌سازی در Cursor

## 1. ادغام با Git

### ویژگی‌های Git در Cursor

#### Commit Messages هوشمند
```bash
# روش استفاده:
1. تغییرات را Stage کنید
2. در Git panel روی "Generate commit message" کلیک کنید
3. Cursor پیام مناسب را بر اساس تغییرات پیشنهاد می‌دهد

# مثال پیام‌های تولید شده:
feat: Add user authentication with JWT tokens
fix: Resolve memory leak in data processing module
refactor: Optimize database queries for better performance
docs: Update API documentation with new endpoints
```

#### حل تعارضات با AI
```bash
# هنگام merge conflict:
1. Cursor تعارضات را تشخیص می‌دهد
2. گزینه "Resolve with AI" را انتخاب کنید
3. AI با درک هر دو نسخه، راه‌حل پیشنهاد می‌دهد

# مثال:
<<<<<<< HEAD
const apiUrl = 'https://api.production.com';
=======
const apiUrl = 'https://api.staging.com';
>>>>>>> feature-branch

# AI پیشنهاد می‌دهد:
const apiUrl = process.env.NODE_ENV === 'production' 
  ? 'https://api.production.com' 
  : 'https://api.staging.com';
```

#### Git History Analysis
```bash
# در Chat بپرسید:
"What changes were made to the authentication module last week?"
"Who has been working on the payment feature?"
"Show me the evolution of the user model"
```

### تنظیمات Git

```json
{
  "git.enableSmartCommit": true,
  "git.autofetch": true,
  "git.confirmSync": false,
  "cursor.git.aiCommitMessages": true,
  "cursor.git.smartMergeConflicts": true
}
```

## 2. @Docs - دسترسی به مستندات

### مستندات پشتیبانی شده

#### فریم‌ورک‌های Frontend
- React
- Vue.js
- Angular
- Next.js
- Svelte
- Solid.js

#### فریم‌ورک‌های Backend
- Express.js
- Django
- Flask
- FastAPI
- Ruby on Rails
- Spring Boot

#### زبان‌ها
- JavaScript/TypeScript
- Python
- Java
- Go
- Rust
- C++

### نحوه استفاده از @Docs

```bash
# در Chat:
@docs:react How to implement custom hooks?
@docs:django What's the best way to handle migrations?
@docs:typescript How to use generics?

# ترکیب با کد:
@docs:express @file:server.js How to add middleware for authentication?
```

### مثال‌های عملی

#### React Documentation
```typescript
// سوال: @docs:react How to optimize re-renders?

// پاسخ با مثال از docs:
const ExpensiveComponent = React.memo(({ data }) => {
  return <ComplexVisualization data={data} />;
}, (prevProps, nextProps) => {
  return prevProps.data.id === nextProps.data.id;
});
```

#### Python Documentation
```python
# سوال: @docs:python How to use async context managers?

# پاسخ با مثال:
async with aiofiles.open('file.txt', mode='r') as f:
    contents = await f.read()
    print(contents)
```

## 3. @Web - جستجوی وب

### کاربردهای @Web

1. **اطلاعات به‌روز**
```bash
@web What's new in React 19?
@web Latest security vulnerabilities in npm packages
@web Best practices for Kubernetes in 2024
```

2. **حل مشکلات**
```bash
@web "TypeError: Cannot read property 'map' of undefined" React
@web How to fix CORS error in Express.js
@web PostgreSQL performance tuning tips
```

3. **مقایسه تکنولوژی‌ها**
```bash
@web Comparison between Prisma and TypeORM
@web When to use Redis vs Memcached
@web Vue 3 vs React performance benchmark
```

### ترکیب @Web با دیگر ابزارها
```bash
# جستجو + کد محلی
@web @file:package.json How to upgrade these dependencies safely?

# جستجو + docs
@web @docs:react What are the community best practices for state management?
```

## 4. Model Context Protocol (MCP)

### MCP چیست؟

MCP یک پروتکل استاندارد برای اتصال Cursor به منابع داده خارجی است. این امکان را فراهم می‌کند که AI به اطلاعات سفارشی شما دسترسی داشته باشد.

### موارد استفاده MCP

#### 1. اتصال به Database
```javascript
// mcp-config.json
{
  "database": {
    "type": "postgresql",
    "connection": {
      "host": "localhost",
      "port": 5432,
      "database": "myapp",
      "user": "dbuser"
    },
    "permissions": ["read", "explain"]
  }
}

// در Chat:
"Show me all users who registered last month"
"What's the most efficient query for finding inactive accounts?"
```

#### 2. اتصال به API داخلی
```javascript
// mcp-api-config.json
{
  "apis": {
    "internal": {
      "baseUrl": "https://api.internal.company.com",
      "auth": {
        "type": "bearer",
        "token": "${INTERNAL_API_TOKEN}"
      },
      "endpoints": [
        "/users",
        "/products",
        "/analytics"
      ]
    }
  }
}
```

#### 3. مستندات سفارشی
```javascript
// mcp-docs-config.json
{
  "documentation": {
    "sources": [
      {
        "name": "Company Wiki",
        "type": "confluence",
        "url": "https://wiki.company.com",
        "spaces": ["ENGR", "ARCH", "API"]
      },
      {
        "name": "Internal Docs",
        "type": "markdown",
        "path": "./docs/internal"
      }
    ]
  }
}
```

### پیاده‌سازی MCP Server

```python
# mcp_server.py
from mcp import Server, Context, Tool

class CompanyMCPServer(Server):
    def __init__(self):
        super().__init__()
        self.register_tool(self.query_database)
        self.register_tool(self.get_api_docs)
    
    @Tool(
        name="query_database",
        description="Query the company database"
    )
    async def query_database(self, ctx: Context, query: str):
        # اجرای query و برگرداندن نتیجه
        result = await self.db.execute(query)
        return result
    
    @Tool(
        name="get_api_docs",
        description="Get internal API documentation"
    )
    async def get_api_docs(self, ctx: Context, endpoint: str):
        # دریافت مستندات API
        docs = await self.fetch_docs(endpoint)
        return docs
```

## 5. Extensions و Plugins

### نصب Extensions
```bash
# از طریق UI:
Ctrl+Shift+X → Search → Install

# از طریق command line:
cursor --install-extension <extension-id>
```

### Extensions محبوب برای Cursor

#### توسعه وب
- **Live Server**: پیش‌نمایش زنده
- **REST Client**: تست API
- **Thunder Client**: Postman جایگزین
- **Prettier**: فرمت خودکار کد

#### دیتابیس
- **SQLTools**: مدیریت دیتابیس
- **MongoDB for VS Code**: کار با MongoDB
- **Redis**: مدیریت Redis

#### DevOps
- **Docker**: مدیریت containers
- **Kubernetes**: مدیریت clusters
- **AWS Toolkit**: کار با AWS
- **Azure Tools**: کار با Azure

### ساخت Extension سفارشی

```javascript
// extension.js
const vscode = require('vscode');

function activate(context) {
    // ثبت command جدید
    let disposable = vscode.commands.registerCommand(
        'myextension.enhanceWithAI',
        async function () {
            const editor = vscode.window.activeTextEditor;
            if (editor) {
                const selection = editor.selection;
                const text = editor.document.getText(selection);
                
                // ارسال به Cursor AI
                const enhanced = await vscode.commands.executeCommand(
                    'cursor.enhanceCode',
                    { code: text, instruction: 'Optimize this code' }
                );
                
                // جایگزینی کد
                editor.edit(editBuilder => {
                    editBuilder.replace(selection, enhanced);
                });
            }
        }
    );
    
    context.subscriptions.push(disposable);
}

module.exports = { activate };
```

## 6. Terminal Integration

### قابلیت‌های Terminal

#### اجرای دستورات با AI
```bash
# در Chat:
"Run npm install and then start the dev server"
"Create a new Python virtual environment and install requirements"
"Deploy to Heroku with the latest changes"
```

#### Debug خطاهای Terminal
```bash
# وقتی خطایی رخ می‌دهد:
"This error appeared in terminal: [paste error]. How do I fix it?"

# Cursor:
1. تحلیل خطا
2. ارائه راه‌حل
3. پیشنهاد دستورات اصلاحی
```

### Terminal Profiles

```json
{
  "terminal.integrated.profiles.windows": {
    "PowerShell": {
      "source": "PowerShell",
      "icon": "terminal-powershell"
    },
    "Git Bash": {
      "source": "Git Bash"
    },
    "WSL": {
      "path": "C:\\Windows\\System32\\wsl.exe",
      "args": []
    }
  },
  "terminal.integrated.defaultProfile.windows": "PowerShell"
}
```

## 7. دیگر یکپارچه‌سازی‌ها

### Jupyter Notebooks
```python
# Cursor پشتیبانی کامل از Jupyter دارد
# ویژگی‌ها:
- اجرای سلول با Shift+Enter
- IntelliSense در notebooks
- تکمیل کد AI در سلول‌ها
- تبدیل notebook به script
```

### Live Share
```bash
# همکاری real-time با تیم
1. نصب extension "Live Share"
2. شروع session: Ctrl+Shift+P → "Live Share: Start"
3. اشتراک لینک با تیم
4. کدنویسی همزمان با AI assistance
```

### Remote Development
```bash
# توسعه روی سرور remote
1. نصب "Remote - SSH" extension
2. اتصال به سرور: Ctrl+Shift+P → "Remote-SSH: Connect"
3. استفاده از Cursor AI روی سرور remote
```

## بهترین شیوه‌ها برای Integrations

### 1. امنیت
```javascript
// همیشه از environment variables استفاده کنید
const apiKey = process.env.API_KEY;

// هرگز credentials را commit نکنید
// .gitignore
.env
.env.local
*.key
*.pem
```

### 2. Performance
```json
{
  // محدود کردن extensions
  "extensions.autoUpdate": false,
  "extensions.ignoreRecommendations": true,
  
  // بهینه‌سازی terminal
  "terminal.integrated.gpuAcceleration": "on",
  "terminal.integrated.scrollback": 1000
}
```

### 3. سازماندهی
```bash
project/
├── .vscode/
│   ├── settings.json      # تنظیمات پروژه
│   ├── extensions.json    # extensions پیشنهادی
│   └── tasks.json        # task runner
├── .cursor/
│   ├── rules.md          # قوانین AI
│   └── mcp-config.json   # تنظیمات MCP
└── .env.example          # نمونه environment variables
```

## نکات پیشرفته

### 1. Chain کردن ابزارها
```bash
# ترکیب چند ابزار برای نتیجه بهتر
@docs:react @web @file:App.tsx 
"How to implement infinite scroll with latest best practices?"
```

### 2. Automation با Tasks
```json
// tasks.json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Deploy with AI Check",
      "type": "shell",
      "command": "npm run build && cursor-ai check && npm run deploy",
      "group": "build"
    }
  ]
}
```

### 3. Custom Commands
```javascript
// ایجاد دستورات سفارشی که با AI کار می‌کنند
vscode.commands.registerCommand('myapp.optimizeImports', async () => {
    await vscode.commands.executeCommand('cursor.inlineEdit', {
        instruction: 'Optimize and sort all imports'
    });
});
```