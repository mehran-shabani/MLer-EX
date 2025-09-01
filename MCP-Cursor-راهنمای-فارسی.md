# راهنمای جامع استفاده از MCP در Cursor

## فهرست مطالب
1. [مقدمه](#مقدمه)
2. [MCP چیست؟](#mcp-چیست)
3. [مزایای استفاده از MCP](#مزایای-استفاده-از-mcp)
4. [نصب و راه‌اندازی](#نصب-و-راه‌اندازی)
5. [پیکربندی MCP در Cursor](#پیکربندی-mcp-در-cursor)
6. [ساخت سرور MCP سفارشی](#ساخت-سرور-mcp-سفارشی)
7. [مثال‌های کاربردی](#مثال‌های-کاربردی)
8. [عیب‌یابی و رفع مشکلات](#عیب‌یابی-و-رفع-مشکلات)
9. [منابع مفید](#منابع-مفید)

## مقدمه

این راهنما به شما کمک می‌کند تا با پروتکل زمینه مدل (Model Context Protocol - MCP) در محیط Cursor آشنا شوید و بتوانید از آن برای بهبود فرآیند توسعه نرم‌افزار خود استفاده کنید.

## MCP چیست؟

پروتکل زمینه مدل (MCP) یک استاندارد باز است که به مدل‌های هوش مصنوعی اجازه می‌دهد تا به طور امن و کارآمد با منابع داده، API‌ها و ابزارهای خارجی ارتباط برقرار کنند. این پروتکل مانند یک پل ارتباطی بین دستیار هوش مصنوعی Cursor و سیستم‌های خارجی عمل می‌کند.

### ویژگی‌های کلیدی MCP:
- **استاندارد باز**: قابل استفاده در پلتفرم‌های مختلف
- **امنیت بالا**: کنترل دقیق بر روی دسترسی‌ها
- **انعطاف‌پذیری**: قابلیت اتصال به انواع منابع داده
- **توسعه‌پذیری**: امکان ایجاد سرورهای سفارشی

## مزایای استفاده از MCP

### 1. دسترسی به داده‌های زنده
با استفاده از MCP، Cursor می‌تواند به داده‌های real-time دسترسی داشته باشد و بر اساس آن‌ها پیشنهادات دقیق‌تری ارائه دهد.

### 2. اتوماسیون فرآیندها
امکان اتصال به ابزارهای مختلف و اجرای خودکار دستورات

### 3. بهبود دقت کد
دسترسی به مستندات API و اطلاعات پروژه برای تولید کد دقیق‌تر

### 4. کاهش زمان توسعه
استفاده از اطلاعات موجود برای تسریع فرآیند کدنویسی

## نصب و راه‌اندازی

### مرحله 1: نصب Cursor
```bash
# دانلود از سایت رسمی
# https://cursor.sh/

# یا استفاده از package manager
# macOS
brew install --cask cursor

# Windows (با استفاده از winget)
winget install cursor
```

### مرحله 2: نصب ابزارهای مورد نیاز برای MCP
```bash
# نصب Node.js (برای سرورهای JavaScript)
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs

# نصب Python (برای سرورهای Python)
sudo apt-get update
sudo apt-get install python3 python3-pip

# نصب uv (برای مدیریت محیط Python)
pip install uv
```

## پیکربندی MCP در Cursor

### 1. یافتن فایل پیکربندی
فایل پیکربندی MCP در مسیرهای زیر قرار دارد:

- **macOS**: `~/Library/Application Support/Cursor/User/mcp.json`
- **Windows**: `%APPDATA%\Cursor\User\mcp.json`
- **Linux**: `~/.config/Cursor/User/mcp.json`

### 2. ساختار فایل پیکربندی
```json
{
  "mcpServers": {
    "weather-server": {
      "command": "python",
      "args": ["/path/to/weather_server.py"],
      "env": {
        "API_KEY": "your-api-key"
      }
    },
    "database-server": {
      "command": "node",
      "args": ["/path/to/db_server.js"],
      "env": {
        "DB_CONNECTION": "postgresql://localhost/mydb"
      }
    }
  }
}
```

### 3. مثال پیکربندی سرور Apidog
```json
{
  "mcpServers": {
    "apidog": {
      "command": "npx",
      "args": ["@apidog/mcp-server"],
      "env": {
        "APIDOG_API_TOKEN": "your-api-token",
        "APIDOG_PROJECT_ID": "your-project-id"
      }
    }
  }
}
```

## ساخت سرور MCP سفارشی

### معماری MCP

قبل از شروع ساخت سرور، بیایید معماری MCP را بررسی کنیم:

```
┌─────────────┐     JSON-RPC      ┌─────────────┐
│   Cursor    │ ←───────────────→ │ MCP Server  │
│     AI      │                    │   (Tools)   │
└─────────────┘                    └─────────────┘
       ↑                                   ↓
       │                           ┌───────────────┐
       │                           │ External APIs │
       └───────────────────────────┤   Databases   │
                                   │  File Systems │
                                   └───────────────┘
```

### مثال 1: سرور ساده Python با FastMCP

#### نصب FastMCP
```bash
pip install fastmcp
```

#### کد سرور (weather_server.py)
```python
from fastmcp import FastMCP
import random
from datetime import datetime

# ایجاد instance از FastMCP
server = FastMCP("سرور آب و هوا")

# تعریف ابزار برای دریافت آب و هوا
@server.tool()
def get_weather(city: str, unit: str = "celsius") -> dict:
    """
    دریافت اطلاعات آب و هوای یک شهر
    
    Args:
        city: نام شهر به فارسی یا انگلیسی
        unit: واحد دما (celsius یا fahrenheit)
    
    Returns:
        دیکشنری حاوی اطلاعات آب و هوا
    """
    # داده‌های نمونه (در حالت واقعی از API استفاده کنید)
    weather_data = {
        "تهران": {"temp": 25, "condition": "آفتابی", "humidity": 45},
        "مشهد": {"temp": 18, "condition": "ابری", "humidity": 60},
        "اصفهان": {"temp": 22, "condition": "صاف", "humidity": 30},
        "شیراز": {"temp": 28, "condition": "گرم و خشک", "humidity": 25},
        "تبریز": {"temp": 15, "condition": "بارانی", "humidity": 70}
    }
    
    # اگر شهر در لیست نباشد، داده تصادفی تولید کن
    if city not in weather_data:
        temp = random.randint(10, 35)
        conditions = ["آفتابی", "ابری", "بارانی", "برفی", "طوفانی"]
        weather_data[city] = {
            "temp": temp,
            "condition": random.choice(conditions),
            "humidity": random.randint(20, 80)
        }
    
    data = weather_data[city]
    
    # تبدیل دما اگر نیاز باشد
    if unit == "fahrenheit":
        data["temp"] = (data["temp"] * 9/5) + 32
        unit_symbol = "F"
    else:
        unit_symbol = "C"
    
    return {
        "city": city,
        "temperature": f"{data['temp']}°{unit_symbol}",
        "condition": data["condition"],
        "humidity": f"{data['humidity']}%",
        "timestamp": datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    }

# تعریف ابزار برای پیش‌بینی آب و هوا
@server.tool()
def forecast_weather(city: str, days: int = 7) -> dict:
    """
    پیش‌بینی آب و هوا برای روزهای آینده
    
    Args:
        city: نام شهر
        days: تعداد روزهای پیش‌بینی (حداکثر 14 روز)
    
    Returns:
        پیش‌بینی آب و هوا
    """
    if days > 14:
        days = 14
    
    forecast = []
    conditions = ["آفتابی", "ابری", "بارانی", "برفی", "نیمه ابری"]
    
    for i in range(days):
        forecast.append({
            "day": i + 1,
            "min_temp": random.randint(5, 20),
            "max_temp": random.randint(20, 35),
            "condition": random.choice(conditions),
            "rain_chance": f"{random.randint(0, 100)}%"
        })
    
    return {
        "city": city,
        "forecast_days": days,
        "forecast": forecast
    }

# اجرای سرور
if __name__ == "__main__":
    server.run()
```

### مثال 2: سرور Node.js برای مدیریت فایل

#### نصب وابستگی‌ها
```bash
npm init -y
npm install @modelcontextprotocol/sdk
```

#### کد سرور (file_server.js)
```javascript
const { Server } = require('@modelcontextprotocol/sdk');
const fs = require('fs').promises;
const path = require('path');

// ایجاد سرور MCP
const server = new Server({
  name: 'سرور مدیریت فایل',
  version: '1.0.0',
  description: 'سرور MCP برای عملیات فایل'
});

// ابزار خواندن فایل
server.addTool({
  name: 'read_file',
  description: 'خواندن محتوای یک فایل',
  inputSchema: {
    type: 'object',
    properties: {
      path: {
        type: 'string',
        description: 'مسیر فایل'
      }
    },
    required: ['path']
  },
  handler: async ({ path: filePath }) => {
    try {
      const content = await fs.readFile(filePath, 'utf-8');
      return {
        success: true,
        content: content,
        path: filePath
      };
    } catch (error) {
      return {
        success: false,
        error: error.message
      };
    }
  }
});

// ابزار نوشتن فایل
server.addTool({
  name: 'write_file',
  description: 'نوشتن محتوا در یک فایل',
  inputSchema: {
    type: 'object',
    properties: {
      path: {
        type: 'string',
        description: 'مسیر فایل'
      },
      content: {
        type: 'string',
        description: 'محتوای فایل'
      }
    },
    required: ['path', 'content']
  },
  handler: async ({ path: filePath, content }) => {
    try {
      await fs.writeFile(filePath, content, 'utf-8');
      return {
        success: true,
        message: 'فایل با موفقیت نوشته شد',
        path: filePath
      };
    } catch (error) {
      return {
        success: false,
        error: error.message
      };
    }
  }
});

// ابزار لیست کردن فایل‌ها
server.addTool({
  name: 'list_files',
  description: 'لیست فایل‌های یک دایرکتوری',
  inputSchema: {
    type: 'object',
    properties: {
      directory: {
        type: 'string',
        description: 'مسیر دایرکتوری'
      }
    },
    required: ['directory']
  },
  handler: async ({ directory }) => {
    try {
      const files = await fs.readdir(directory);
      const fileDetails = await Promise.all(
        files.map(async (file) => {
          const filePath = path.join(directory, file);
          const stats = await fs.stat(filePath);
          return {
            name: file,
            path: filePath,
            isDirectory: stats.isDirectory(),
            size: stats.size,
            modified: stats.mtime
          };
        })
      );
      return {
        success: true,
        files: fileDetails
      };
    } catch (error) {
      return {
        success: false,
        error: error.message
      };
    }
  }
});

// شروع سرور
server.start();
```

### مثال 3: سرور پایگاه داده

```python
from fastmcp import FastMCP
import sqlite3
import json
from typing import List, Dict, Any

server = FastMCP("سرور پایگاه داده")

# اتصال به پایگاه داده
def get_db_connection():
    conn = sqlite3.connect('example.db')
    conn.row_factory = sqlite3.Row
    return conn

# ایجاد جداول اولیه
def init_db():
    conn = get_db_connection()
    conn.execute('''
        CREATE TABLE IF NOT EXISTS users (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            email TEXT UNIQUE NOT NULL,
            created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        )
    ''')
    conn.execute('''
        CREATE TABLE IF NOT EXISTS tasks (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            user_id INTEGER,
            title TEXT NOT NULL,
            description TEXT,
            status TEXT DEFAULT 'pending',
            created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
            FOREIGN KEY (user_id) REFERENCES users (id)
        )
    ''')
    conn.commit()
    conn.close()

# ابزار اضافه کردن کاربر
@server.tool()
def add_user(name: str, email: str) -> Dict[str, Any]:
    """
    اضافه کردن کاربر جدید به پایگاه داده
    
    Args:
        name: نام کاربر
        email: ایمیل کاربر
    
    Returns:
        اطلاعات کاربر ایجاد شده
    """
    try:
        conn = get_db_connection()
        cursor = conn.cursor()
        cursor.execute(
            "INSERT INTO users (name, email) VALUES (?, ?)",
            (name, email)
        )
        user_id = cursor.lastrowid
        conn.commit()
        conn.close()
        
        return {
            "success": True,
            "user": {
                "id": user_id,
                "name": name,
                "email": email
            }
        }
    except sqlite3.IntegrityError:
        return {
            "success": False,
            "error": "ایمیل تکراری است"
        }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

# ابزار دریافت لیست کاربران
@server.tool()
def get_users(limit: int = 10) -> Dict[str, Any]:
    """
    دریافت لیست کاربران
    
    Args:
        limit: حداکثر تعداد کاربران
    
    Returns:
        لیست کاربران
    """
    try:
        conn = get_db_connection()
        users = conn.execute(
            "SELECT * FROM users ORDER BY created_at DESC LIMIT ?",
            (limit,)
        ).fetchall()
        conn.close()
        
        return {
            "success": True,
            "users": [dict(user) for user in users]
        }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

# ابزار اضافه کردن وظیفه
@server.tool()
def add_task(user_id: int, title: str, description: str = "") -> Dict[str, Any]:
    """
    اضافه کردن وظیفه جدید برای کاربر
    
    Args:
        user_id: شناسه کاربر
        title: عنوان وظیفه
        description: توضیحات وظیفه
    
    Returns:
        اطلاعات وظیفه ایجاد شده
    """
    try:
        conn = get_db_connection()
        cursor = conn.cursor()
        cursor.execute(
            "INSERT INTO tasks (user_id, title, description) VALUES (?, ?, ?)",
            (user_id, title, description)
        )
        task_id = cursor.lastrowid
        conn.commit()
        conn.close()
        
        return {
            "success": True,
            "task": {
                "id": task_id,
                "user_id": user_id,
                "title": title,
                "description": description,
                "status": "pending"
            }
        }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

# ابزار جستجوی وظایف
@server.tool()
def search_tasks(user_id: int = None, status: str = None) -> Dict[str, Any]:
    """
    جستجوی وظایف بر اساس فیلترها
    
    Args:
        user_id: شناسه کاربر (اختیاری)
        status: وضعیت وظیفه (pending, completed, cancelled)
    
    Returns:
        لیست وظایف فیلتر شده
    """
    try:
        conn = get_db_connection()
        query = "SELECT * FROM tasks WHERE 1=1"
        params = []
        
        if user_id:
            query += " AND user_id = ?"
            params.append(user_id)
        
        if status:
            query += " AND status = ?"
            params.append(status)
        
        query += " ORDER BY created_at DESC"
        
        tasks = conn.execute(query, params).fetchall()
        conn.close()
        
        return {
            "success": True,
            "tasks": [dict(task) for task in tasks]
        }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

# اجرای سرور
if __name__ == "__main__":
    init_db()
    server.run()
```

### مثال 4: سرور API برای اتصال به سرویس‌های خارجی

```python
from fastmcp import FastMCP
import aiohttp
import asyncio
from typing import Dict, Any, List

server = FastMCP("سرور API خارجی")

# کلاس برای مدیریت درخواست‌های API
class APIClient:
    def __init__(self):
        self.session = None
        
    async def get_session(self):
        if not self.session:
            self.session = aiohttp.ClientSession()
        return self.session
    
    async def close(self):
        if self.session:
            await self.session.close()

api_client = APIClient()

@server.tool()
async def github_user_info(username: str) -> Dict[str, Any]:
    """
    دریافت اطلاعات کاربر GitHub
    
    Args:
        username: نام کاربری GitHub
    
    Returns:
        اطلاعات کاربر
    """
    try:
        session = await api_client.get_session()
        async with session.get(f"https://api.github.com/users/{username}") as response:
            if response.status == 200:
                data = await response.json()
                return {
                    "success": True,
                    "user": {
                        "name": data.get("name", "نامشخص"),
                        "company": data.get("company", "نامشخص"),
                        "location": data.get("location", "نامشخص"),
                        "bio": data.get("bio", "بدون توضیحات"),
                        "public_repos": data.get("public_repos", 0),
                        "followers": data.get("followers", 0),
                        "following": data.get("following", 0)
                    }
                }
            else:
                return {
                    "success": False,
                    "error": f"کاربر یافت نشد (کد: {response.status})"
                }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

@server.tool()
async def search_stackoverflow(query: str, tags: List[str] = None) -> Dict[str, Any]:
    """
    جستجو در Stack Overflow
    
    Args:
        query: عبارت جستجو
        tags: تگ‌های مرتبط (مثلا ['python', 'javascript'])
    
    Returns:
        نتایج جستجو
    """
    try:
        session = await api_client.get_session()
        params = {
            "order": "desc",
            "sort": "votes",
            "q": query,
            "site": "stackoverflow"
        }
        
        if tags:
            params["tagged"] = ";".join(tags)
        
        async with session.get(
            "https://api.stackexchange.com/2.3/search",
            params=params
        ) as response:
            if response.status == 200:
                data = await response.json()
                results = []
                for item in data.get("items", [])[:5]:  # حداکثر 5 نتیجه
                    results.append({
                        "title": item.get("title"),
                        "link": item.get("link"),
                        "score": item.get("score"),
                        "answer_count": item.get("answer_count"),
                        "is_answered": item.get("is_answered")
                    })
                
                return {
                    "success": True,
                    "results": results,
                    "total": len(results)
                }
            else:
                return {
                    "success": False,
                    "error": f"خطا در جستجو (کد: {response.status})"
                }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

# مدیریت بستن session
@server.on_shutdown()
async def cleanup():
    await api_client.close()

if __name__ == "__main__":
    server.run()
```

### مثال 5: سرور Git برای عملیات کنترل نسخه

```python
from fastmcp import FastMCP
import git
import os
from typing import List, Dict, Any

server = FastMCP("سرور Git")

@server.tool()
def git_status(repo_path: str = ".") -> Dict[str, Any]:
    """
    بررسی وضعیت مخزن Git
    
    Args:
        repo_path: مسیر مخزن (پیش‌فرض: دایرکتوری جاری)
    
    Returns:
        وضعیت مخزن
    """
    try:
        repo = git.Repo(repo_path)
        
        # فایل‌های تغییر یافته
        modified_files = [item.a_path for item in repo.index.diff(None)]
        
        # فایل‌های جدید
        untracked_files = repo.untracked_files
        
        # فایل‌های staged
        staged_files = [item.a_path for item in repo.index.diff("HEAD")]
        
        # شاخه فعلی
        current_branch = repo.active_branch.name
        
        return {
            "success": True,
            "status": {
                "branch": current_branch,
                "modified": modified_files,
                "untracked": untracked_files,
                "staged": staged_files,
                "is_dirty": repo.is_dirty()
            }
        }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

@server.tool()
def git_commit(message: str, files: List[str] = None, repo_path: str = ".") -> Dict[str, Any]:
    """
    ایجاد کامیت جدید
    
    Args:
        message: پیام کامیت
        files: لیست فایل‌ها برای اضافه کردن (None = همه فایل‌ها)
        repo_path: مسیر مخزن
    
    Returns:
        اطلاعات کامیت
    """
    try:
        repo = git.Repo(repo_path)
        
        # اضافه کردن فایل‌ها
        if files:
            repo.index.add(files)
        else:
            repo.git.add(A=True)  # اضافه کردن همه فایل‌ها
        
        # ایجاد کامیت
        commit = repo.index.commit(message)
        
        return {
            "success": True,
            "commit": {
                "hash": str(commit.hexsha),
                "message": commit.message,
                "author": str(commit.author),
                "date": str(commit.authored_datetime)
            }
        }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

@server.tool()
def git_branch_list(repo_path: str = ".") -> Dict[str, Any]:
    """
    لیست شاخه‌های مخزن
    
    Args:
        repo_path: مسیر مخزن
    
    Returns:
        لیست شاخه‌ها
    """
    try:
        repo = git.Repo(repo_path)
        branches = []
        
        for branch in repo.branches:
            branches.append({
                "name": branch.name,
                "commit": str(branch.commit.hexsha[:7]),
                "is_active": branch == repo.active_branch
            })
        
        return {
            "success": True,
            "branches": branches,
            "current": repo.active_branch.name
        }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

@server.tool()
def git_log(limit: int = 10, repo_path: str = ".") -> Dict[str, Any]:
    """
    نمایش تاریخچه کامیت‌ها
    
    Args:
        limit: تعداد کامیت‌ها
        repo_path: مسیر مخزن
    
    Returns:
        لیست کامیت‌ها
    """
    try:
        repo = git.Repo(repo_path)
        commits = []
        
        for commit in repo.iter_commits(max_count=limit):
            commits.append({
                "hash": commit.hexsha[:7],
                "message": commit.message.strip(),
                "author": str(commit.author),
                "date": str(commit.authored_datetime),
                "files_changed": len(commit.stats.files)
            })
        
        return {
            "success": True,
            "commits": commits
        }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

if __name__ == "__main__":
    server.run()
```

### مثال 6: سرور Docker برای مدیریت کانتینرها

```python
from fastmcp import FastMCP
import docker
from typing import List, Dict, Any

server = FastMCP("سرور Docker")

# اتصال به Docker daemon
docker_client = docker.from_env()

@server.tool()
def docker_list_containers(all: bool = False) -> Dict[str, Any]:
    """
    لیست کانتینرهای Docker
    
    Args:
        all: نمایش همه کانتینرها (شامل متوقف شده‌ها)
    
    Returns:
        لیست کانتینرها
    """
    try:
        containers = docker_client.containers.list(all=all)
        container_list = []
        
        for container in containers:
            container_list.append({
                "id": container.short_id,
                "name": container.name,
                "image": container.image.tags[0] if container.image.tags else "unknown",
                "status": container.status,
                "ports": container.ports
            })
        
        return {
            "success": True,
            "containers": container_list,
            "total": len(container_list)
        }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

@server.tool()
def docker_run_container(image: str, name: str = None, ports: Dict[str, int] = None, 
                        environment: Dict[str, str] = None) -> Dict[str, Any]:
    """
    اجرای کانتینر جدید
    
    Args:
        image: نام ایمیج
        name: نام کانتینر (اختیاری)
        ports: پورت‌ها (مثلا {'80/tcp': 8080})
        environment: متغیرهای محیطی
    
    Returns:
        اطلاعات کانتینر ایجاد شده
    """
    try:
        container = docker_client.containers.run(
            image,
            name=name,
            ports=ports,
            environment=environment,
            detach=True
        )
        
        return {
            "success": True,
            "container": {
                "id": container.short_id,
                "name": container.name,
                "status": container.status
            }
        }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

@server.tool()
def docker_stop_container(container_id: str) -> Dict[str, Any]:
    """
    متوقف کردن کانتینر
    
    Args:
        container_id: شناسه یا نام کانتینر
    
    Returns:
        وضعیت عملیات
    """
    try:
        container = docker_client.containers.get(container_id)
        container.stop()
        
        return {
            "success": True,
            "message": f"کانتینر {container_id} متوقف شد"
        }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

@server.tool()
def docker_logs(container_id: str, lines: int = 50) -> Dict[str, Any]:
    """
    نمایش لاگ‌های کانتینر
    
    Args:
        container_id: شناسه یا نام کانتینر
        lines: تعداد خطوط آخر
    
    Returns:
        لاگ‌های کانتینر
    """
    try:
        container = docker_client.containers.get(container_id)
        logs = container.logs(tail=lines).decode('utf-8')
        
        return {
            "success": True,
            "logs": logs,
            "container": container.name
        }
    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }

if __name__ == "__main__":
    server.run()
```

## مثال‌های کاربردی

### 1. استفاده از سرور آب و هوا در Cursor

پس از راه‌اندازی سرور و پیکربندی آن در Cursor:

```python
# در Cursor می‌توانید بگویید:
# "آب و هوای تهران چطور است؟"
# Cursor به طور خودکار از ابزار get_weather استفاده می‌کند

# یا برای پیش‌بینی:
# "پیش‌بینی آب و هوای اصفهان برای 5 روز آینده"
```

### 2. استفاده از سرور پایگاه داده

```python
# مثال‌هایی از دستورات در Cursor:

# "یک کاربر جدید با نام علی رضایی و ایمیل ali@example.com اضافه کن"

# "لیست همه کاربران را نشان بده"

# "یک وظیفه جدید برای کاربر با شناسه 1 ایجاد کن: بررسی ایمیل‌ها"

# "همه وظایف در حال انجام را نشان بده"
```

### 3. ترکیب چند سرور MCP

```json
{
  "mcpServers": {
    "weather": {
      "command": "python",
      "args": ["/home/user/mcp-servers/weather_server.py"]
    },
    "database": {
      "command": "python",
      "args": ["/home/user/mcp-servers/db_server.py"]
    },
    "files": {
      "command": "node",
      "args": ["/home/user/mcp-servers/file_server.js"]
    }
  }
}
```

## سناریوهای کاربردی جامع

### سناریو 1: ساخت اپلیکیشن Full-Stack با MCP

#### هدف: ایجاد یک سیستم مدیریت وظایف کامل

**1. راه‌اندازی پروژه:**
```bash
# در Cursor بگویید:
"یک پروژه جدید برای task manager بساز با React و FastAPI"
```

**2. سرور MCP برای مدیریت پروژه:**
```python
# project_manager_server.py
from fastmcp import FastMCP
import os
import subprocess
from typing import Dict, Any

server = FastMCP("مدیر پروژه Full-Stack")

@server.tool()
def create_project_structure(name: str, frontend: str = "react", backend: str = "fastapi") -> Dict[str, Any]:
    """
    ایجاد ساختار پروژه Full-Stack
    """
    try:
        # ساختار دایرکتوری
        directories = [
            f"{name}/frontend/src/components",
            f"{name}/frontend/src/pages",
            f"{name}/frontend/src/services",
            f"{name}/backend/app/api",
            f"{name}/backend/app/models",
            f"{name}/backend/app/services",
            f"{name}/database",
            f"{name}/docker"
        ]
        
        for dir in directories:
            os.makedirs(dir, exist_ok=True)
        
        # فایل‌های اولیه
        files = {
            f"{name}/README.md": f"# {name}\n\nیک اپلیکیشن Full-Stack با {frontend} و {backend}",
            f"{name}/docker-compose.yml": """version: '3.8'
services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
  backend:
    build: ./backend
    ports:
      - "8000:8000"
  db:
    image: postgres:14
    environment:
      POSTGRES_DB: taskdb
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
""",
            f"{name}/frontend/package.json": """{
  "name": "frontend",
  "version": "1.0.0",
  "dependencies": {
    "react": "^18.2.0",
    "axios": "^1.4.0",
    "react-router-dom": "^6.11.0"
  }
}""",
            f"{name}/backend/requirements.txt": """fastapi==0.104.0
uvicorn==0.24.0
sqlalchemy==2.0.23
psycopg2-binary==2.9.9
pydantic==2.4.2
"""
        }
        
        for path, content in files.items():
            with open(path, 'w', encoding='utf-8') as f:
                f.write(content)
        
        return {
            "success": True,
            "message": f"پروژه {name} با موفقیت ایجاد شد",
            "structure": directories
        }
    except Exception as e:
        return {"success": False, "error": str(e)}

@server.tool()
def generate_api_endpoint(name: str, model: str, operations: list = ["create", "read", "update", "delete"]) -> Dict[str, Any]:
    """
    تولید endpoint API با عملیات CRUD
    """
    code = f'''from fastapi import APIRouter, HTTPException, Depends
from sqlalchemy.orm import Session
from typing import List
from ..models import {model}
from ..schemas import {model}Schema, {model}Create, {model}Update
from ..database import get_db

router = APIRouter(prefix="/{name}", tags=["{name}"])
'''
    
    if "create" in operations:
        code += f'''
@router.post("/", response_model={model}Schema)
def create_{name}({name}: {model}Create, db: Session = Depends(get_db)):
    """ایجاد {name} جدید"""
    db_{name} = {model}(**{name}.dict())
    db.add(db_{name})
    db.commit()
    db.refresh(db_{name})
    return db_{name}
'''
    
    if "read" in operations:
        code += f'''
@router.get("/", response_model=List[{model}Schema])
def read_{name}s(skip: int = 0, limit: int = 100, db: Session = Depends(get_db)):
    """دریافت لیست {name}ها"""
    return db.query({model}).offset(skip).limit(limit).all()

@router.get("/{{{name}_id}}", response_model={model}Schema)
def read_{name}({name}_id: int, db: Session = Depends(get_db)):
    """دریافت یک {name} خاص"""
    db_{name} = db.query({model}).filter({model}.id == {name}_id).first()
    if not db_{name}:
        raise HTTPException(status_code=404, detail="{name} پیدا نشد")
    return db_{name}
'''
    
    if "update" in operations:
        code += f'''
@router.put("/{{{name}_id}}", response_model={model}Schema)
def update_{name}({name}_id: int, {name}: {model}Update, db: Session = Depends(get_db)):
    """به‌روزرسانی {name}"""
    db_{name} = db.query({model}).filter({model}.id == {name}_id).first()
    if not db_{name}:
        raise HTTPException(status_code=404, detail="{name} پیدا نشد")
    
    for key, value in {name}.dict(exclude_unset=True).items():
        setattr(db_{name}, key, value)
    
    db.commit()
    db.refresh(db_{name})
    return db_{name}
'''
    
    if "delete" in operations:
        code += f'''
@router.delete("/{{{name}_id}}")
def delete_{name}({name}_id: int, db: Session = Depends(get_db)):
    """حذف {name}"""
    db_{name} = db.query({model}).filter({model}.id == {name}_id).first()
    if not db_{name}:
        raise HTTPException(status_code=404, detail="{name} پیدا نشد")
    
    db.delete(db_{name})
    db.commit()
    return {{"message": "{name} با موفقیت حذف شد"}}
'''
    
    return {
        "success": True,
        "code": code,
        "endpoint": f"/{name}",
        "operations": operations
    }

if __name__ == "__main__":
    server.run()
```

**3. استفاده در Cursor:**
```python
# "یک پروژه task-manager بساز"
# "یک API endpoint برای tasks با تمام عملیات CRUD بساز"
# "کامپوننت React برای نمایش لیست tasks بساز"
# "Docker compose را راه‌اندازی کن"
```

### سناریو 2: اتوماسیون DevOps با MCP

#### هدف: اتوماسیون فرآیند CI/CD

**سرور MCP برای DevOps:**
```python
# devops_server.py
from fastmcp import FastMCP
import subprocess
import yaml
from typing import Dict, Any, List

server = FastMCP("سرور DevOps")

@server.tool()
def create_github_workflow(project_name: str, tests: bool = True, deploy: bool = True) -> Dict[str, Any]:
    """
    ایجاد GitHub Actions workflow
    """
    workflow = {
        "name": f"CI/CD for {project_name}",
        "on": {
            "push": {"branches": ["main", "develop"]},
            "pull_request": {"branches": ["main"]}
        },
        "jobs": {}
    }
    
    # Job تست
    if tests:
        workflow["jobs"]["test"] = {
            "runs-on": "ubuntu-latest",
            "steps": [
                {"uses": "actions/checkout@v3"},
                {"name": "Setup Python", "uses": "actions/setup-python@v4", "with": {"python-version": "3.9"}},
                {"name": "Install dependencies", "run": "pip install -r requirements.txt"},
                {"name": "Run tests", "run": "pytest tests/"}
            ]
        }
    
    # Job دیپلوی
    if deploy:
        workflow["jobs"]["deploy"] = {
            "runs-on": "ubuntu-latest",
            "needs": "test" if tests else None,
            "if": "github.ref == 'refs/heads/main'",
            "steps": [
                {"uses": "actions/checkout@v3"},
                {"name": "Deploy to server", "run": """
                    echo "${{ secrets.SSH_KEY }}" > key.pem
                    chmod 600 key.pem
                    scp -i key.pem -r ./* user@server:/app/
                    ssh -i key.pem user@server 'cd /app && docker-compose up -d'
                """}
            ]
        }
    
    # ذخیره فایل
    os.makedirs(".github/workflows", exist_ok=True)
    with open(".github/workflows/ci-cd.yml", "w") as f:
        yaml.dump(workflow, f, default_flow_style=False, allow_unicode=True)
    
    return {
        "success": True,
        "workflow_path": ".github/workflows/ci-cd.yml",
        "jobs": list(workflow["jobs"].keys())
    }

@server.tool()
def docker_build_and_push(image_name: str, tag: str = "latest", registry: str = "docker.io") -> Dict[str, Any]:
    """
    ساخت و push کردن Docker image
    """
    try:
        # Build image
        build_cmd = f"docker build -t {image_name}:{tag} ."
        subprocess.run(build_cmd, shell=True, check=True)
        
        # Tag for registry
        full_image = f"{registry}/{image_name}:{tag}"
        tag_cmd = f"docker tag {image_name}:{tag} {full_image}"
        subprocess.run(tag_cmd, shell=True, check=True)
        
        # Push to registry
        push_cmd = f"docker push {full_image}"
        subprocess.run(push_cmd, shell=True, check=True)
        
        return {
            "success": True,
            "image": full_image,
            "size": subprocess.check_output(f"docker images {image_name}:{tag} --format '{{{{.Size}}}}'", shell=True).decode().strip()
        }
    except subprocess.CalledProcessError as e:
        return {"success": False, "error": str(e)}

@server.tool()
def kubernetes_deploy(app_name: str, image: str, replicas: int = 3, port: int = 8000) -> Dict[str, Any]:
    """
    دیپلوی اپلیکیشن در Kubernetes
    """
    deployment = {
        "apiVersion": "apps/v1",
        "kind": "Deployment",
        "metadata": {"name": app_name},
        "spec": {
            "replicas": replicas,
            "selector": {"matchLabels": {"app": app_name}},
            "template": {
                "metadata": {"labels": {"app": app_name}},
                "spec": {
                    "containers": [{
                        "name": app_name,
                        "image": image,
                        "ports": [{"containerPort": port}]
                    }]
                }
            }
        }
    }
    
    service = {
        "apiVersion": "v1",
        "kind": "Service",
        "metadata": {"name": f"{app_name}-service"},
        "spec": {
            "selector": {"app": app_name},
            "ports": [{"protocol": "TCP", "port": 80, "targetPort": port}],
            "type": "LoadBalancer"
        }
    }
    
    # ذخیره فایل‌های YAML
    with open(f"{app_name}-deployment.yaml", "w") as f:
        yaml.dump(deployment, f)
    
    with open(f"{app_name}-service.yaml", "w") as f:
        yaml.dump(service, f)
    
    # اعمال در کلاستر
    try:
        subprocess.run(f"kubectl apply -f {app_name}-deployment.yaml", shell=True, check=True)
        subprocess.run(f"kubectl apply -f {app_name}-service.yaml", shell=True, check=True)
        
        return {
            "success": True,
            "deployment": f"{app_name}-deployment",
            "service": f"{app_name}-service",
            "replicas": replicas
        }
    except subprocess.CalledProcessError as e:
        return {"success": False, "error": str(e)}

if __name__ == "__main__":
    server.run()
```

### سناریو 3: تحلیل داده و گزارش‌گیری خودکار

#### هدف: تحلیل داده‌های فروش و تولید گزارش

**سرور MCP برای تحلیل داده:**
```python
# analytics_server.py
from fastmcp import FastMCP
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from datetime import datetime, timedelta
import json
from typing import Dict, Any, List

server = FastMCP("سرور تحلیل داده")

@server.tool()
def analyze_sales_data(file_path: str, period: str = "monthly") -> Dict[str, Any]:
    """
    تحلیل داده‌های فروش
    
    Args:
        file_path: مسیر فایل CSV داده‌ها
        period: دوره تحلیل (daily, weekly, monthly, yearly)
    """
    try:
        # خواندن داده‌ها
        df = pd.read_csv(file_path, parse_dates=['date'])
        
        # تحلیل بر اساس دوره
        if period == "daily":
            grouped = df.groupby(df['date'].dt.date)
        elif period == "weekly":
            grouped = df.groupby(df['date'].dt.to_period('W'))
        elif period == "monthly":
            grouped = df.groupby(df['date'].dt.to_period('M'))
        else:  # yearly
            grouped = df.groupby(df['date'].dt.year)
        
        # محاسبات
        sales_summary = grouped.agg({
            'amount': ['sum', 'mean', 'count'],
            'quantity': 'sum'
        }).round(2)
        
        # بهترین محصولات
        top_products = df.groupby('product')['amount'].sum().nlargest(5)
        
        # روند فروش
        trend = df.groupby(df['date'].dt.date)['amount'].sum().rolling(7).mean()
        
        return {
            "success": True,
            "summary": {
                "total_sales": float(df['amount'].sum()),
                "average_sale": float(df['amount'].mean()),
                "total_transactions": len(df),
                "period": period
            },
            "top_products": top_products.to_dict(),
            "growth_rate": float(((trend.iloc[-1] - trend.iloc[0]) / trend.iloc[0] * 100).round(2))
        }
    except Exception as e:
        return {"success": False, "error": str(e)}

@server.tool()
def generate_report(data: Dict[str, Any], format: str = "html") -> Dict[str, Any]:
    """
    تولید گزارش از داده‌های تحلیل شده
    
    Args:
        data: داده‌های تحلیل شده
        format: فرمت گزارش (html, pdf, markdown)
    """
    try:
        report_date = datetime.now().strftime("%Y-%m-%d")
        
        if format == "html":
            html_content = f"""
<!DOCTYPE html>
<html dir="rtl" lang="fa">
<head>
    <meta charset="UTF-8">
    <title>گزارش فروش - {report_date}</title>
    <style>
        body {{ font-family: 'Vazir', Arial; padding: 20px; }}
        .summary {{ background: #f0f0f0; padding: 15px; border-radius: 5px; }}
        .metric {{ display: inline-block; margin: 10px; padding: 10px; background: white; border-radius: 5px; }}
        table {{ width: 100%; border-collapse: collapse; margin-top: 20px; }}
        th, td {{ border: 1px solid #ddd; padding: 8px; text-align: right; }}
        th {{ background-color: #4CAF50; color: white; }}
    </style>
</head>
<body>
    <h1>گزارش تحلیل فروش</h1>
    <p>تاریخ گزارش: {report_date}</p>
    
    <div class="summary">
        <h2>خلاصه عملکرد</h2>
        <div class="metric">
            <h3>کل فروش</h3>
            <p>{data['summary']['total_sales']:,.0f} تومان</p>
        </div>
        <div class="metric">
            <h3>میانگین فروش</h3>
            <p>{data['summary']['average_sale']:,.0f} تومان</p>
        </div>
        <div class="metric">
            <h3>تعداد تراکنش</h3>
            <p>{data['summary']['total_transactions']}</p>
        </div>
        <div class="metric">
            <h3>نرخ رشد</h3>
            <p>{data['growth_rate']:.1f}%</p>
        </div>
    </div>
    
    <h2>محصولات پرفروش</h2>
    <table>
        <tr>
            <th>محصول</th>
            <th>مبلغ فروش</th>
        </tr>
        {"".join([f"<tr><td>{product}</td><td>{amount:,.0f}</td></tr>" for product, amount in data['top_products'].items()])}
    </table>
</body>
</html>
"""
            
            # ذخیره فایل
            file_name = f"sales_report_{report_date}.html"
            with open(file_name, 'w', encoding='utf-8') as f:
                f.write(html_content)
            
            return {
                "success": True,
                "file": file_name,
                "format": format
            }
        
        elif format == "markdown":
            md_content = f"""# گزارش تحلیل فروش

**تاریخ گزارش:** {report_date}

## خلاصه عملکرد

- **کل فروش:** {data['summary']['total_sales']:,.0f} تومان
- **میانگین فروش:** {data['summary']['average_sale']:,.0f} تومان
- **تعداد تراکنش:** {data['summary']['total_transactions']}
- **نرخ رشد:** {data['growth_rate']:.1f}%

## محصولات پرفروش

| محصول | مبلغ فروش |
|--------|-----------|
{"".join([f"| {product} | {amount:,.0f} |\n" for product, amount in data['top_products'].items()])}
"""
            
            file_name = f"sales_report_{report_date}.md"
            with open(file_name, 'w', encoding='utf-8') as f:
                f.write(md_content)
            
            return {
                "success": True,
                "file": file_name,
                "format": format
            }
        
    except Exception as e:
        return {"success": False, "error": str(e)}

@server.tool()
def create_visualization(data: Dict[str, Any], chart_type: str = "bar") -> Dict[str, Any]:
    """
    ایجاد نمودار از داده‌ها
    """
    try:
        plt.figure(figsize=(10, 6))
        plt.rcParams['font.family'] = ['DejaVu Sans']
        
        if chart_type == "bar":
            products = list(data['top_products'].keys())
            values = list(data['top_products'].values())
            plt.bar(products, values, color='skyblue')
            plt.title('Top 5 Products by Sales')
            plt.xlabel('Product')
            plt.ylabel('Sales Amount')
            plt.xticks(rotation=45)
        
        elif chart_type == "pie":
            plt.pie(data['top_products'].values(), 
                   labels=data['top_products'].keys(), 
                   autopct='%1.1f%%')
            plt.title('Sales Distribution by Product')
        
        # ذخیره نمودار
        file_name = f"sales_chart_{datetime.now().strftime('%Y%m%d_%H%M%S')}.png"
        plt.tight_layout()
        plt.savefig(file_name, dpi=300, bbox_inches='tight')
        plt.close()
        
        return {
            "success": True,
            "chart": file_name,
            "type": chart_type
        }
    except Exception as e:
        return {"success": False, "error": str(e)}

if __name__ == "__main__":
    server.run()
```

**استفاده در Cursor:**
```python
# "داده‌های فروش فایل sales.csv را تحلیل کن"
# "گزارش HTML از تحلیل فروش بساز"
# "نمودار میله‌ای از محصولات پرفروش رسم کن"
# "روند فروش ماهانه را نشان بده"
```

### 4. سناریوهای کاربردی واقعی

#### سناریو 1: توسعه API با مستندات خودکار
```python
# در Cursor:
# "اطلاعات API endpoint /users را نشان بده"
# "یک تست برای این endpoint بنویس"
# "مستندات Swagger برای این API تولید کن"
```

#### سناریو 2: مدیریت پروژه با Git
```python
# "وضعیت git را بررسی کن"
# "تغییرات را با پیام 'اضافه کردن قابلیت احراز هویت' کامیت کن"
# "تاریخچه 5 کامیت آخر را نشان بده"
```

#### سناریو 3: دیپلوی با Docker
```python
# "لیست کانتینرهای در حال اجرا را نشان بده"
# "یک کانتینر nginx روی پورت 8080 اجرا کن"
# "لاگ‌های کانتینر my-app را نمایش بده"
```

## عیب‌یابی و رفع مشکلات جامع

### 1. راه‌اندازی اولیه سرور

#### مشکل: سرور راه‌اندازی نمی‌شود
```bash
# بررسی نصب Python
python --version
python3 --version

# بررسی نصب Node.js
node --version
npm --version

# بررسی مسیر فایل
ls -la /path/to/your/server.py

# اجرای مستقیم برای تست
python /path/to/your/server.py
```

**راه‌حل‌ها:**
1. مطمئن شوید runtime مناسب نصب شده است
2. مسیر فایل را با مسیر مطلق بنویسید
3. دسترسی‌های فایل را بررسی کنید (`chmod +x server.py`)
4. وابستگی‌ها را نصب کنید (`pip install -r requirements.txt`)

### 2. مشکلات پیکربندی

#### مشکل: ابزارها در Cursor ظاهر نمی‌شوند

**مرحله 1: بررسی فایل پیکربندی**
```json
// ~/.config/Cursor/User/mcp.json
{
  "mcpServers": {
    "test-server": {
      "command": "python3",  // توجه: python3 نه python
      "args": ["/absolute/path/to/server.py"],
      "env": {
        "PYTHONUNBUFFERED": "1"  // برای نمایش بهتر لاگ‌ها
      }
    }
  }
}
```

**مرحله 2: اعتبارسنجی JSON**
```bash
# بررسی syntax JSON
python -m json.tool ~/.config/Cursor/User/mcp.json
```

**مرحله 3: راه‌اندازی مجدد Cursor**
- Command Palette (Ctrl/Cmd + Shift + P)
- "Developer: Reload Window"

### 3. دیباگ سرورهای MCP

#### روش 1: لاگ‌گذاری تفصیلی
```python
import logging
import sys
from datetime import datetime

# پیکربندی لاگ‌گذاری
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler(f'mcp_server_{datetime.now().strftime("%Y%m%d")}.log'),
        logging.StreamHandler(sys.stdout)
    ]
)

logger = logging.getLogger(__name__)

@server.tool()
def debug_tool(param: str) -> dict:
    logger.debug(f"ورودی دریافت شد: {param}")
    try:
        result = process_data(param)
        logger.info(f"عملیات موفق: {result}")
        return {"success": True, "data": result}
    except Exception as e:
        logger.error(f"خطا رخ داد: {str(e)}", exc_info=True)
        return {"success": False, "error": str(e)}
```

#### روش 2: استفاده از Debug Mode
```python
# server.py
import os

DEBUG = os.environ.get('MCP_DEBUG', 'false').lower() == 'true'

if DEBUG:
    print("=== MCP Server Debug Mode ===")
    print(f"Python Version: {sys.version}")
    print(f"Working Directory: {os.getcwd()}")
    print(f"Environment Variables: {dict(os.environ)}")
```

در فایل پیکربندی:
```json
{
  "mcpServers": {
    "debug-server": {
      "command": "python",
      "args": ["server.py"],
      "env": {
        "MCP_DEBUG": "true"
      }
    }
  }
}
```

### 4. مشکلات رایج و راه‌حل‌ها

#### مشکل: Connection timeout
```python
# افزایش timeout در سرور
server = FastMCP("my-server", timeout=30)  # 30 ثانیه

# یا در پیکربندی
{
  "mcpServers": {
    "slow-server": {
      "command": "python",
      "args": ["server.py"],
      "timeout": 30000  // میلی‌ثانیه
    }
  }
}
```

#### مشکل: Memory leak در سرورهای طولانی‌مدت
```python
import gc
import psutil
import os

@server.tool()
def memory_intensive_operation(data: str) -> dict:
    # قبل از عملیات
    process = psutil.Process(os.getpid())
    initial_memory = process.memory_info().rss / 1024 / 1024  # MB
    
    try:
        # عملیات سنگین
        result = heavy_processing(data)
        
        # پاکسازی حافظه
        gc.collect()
        
        # بررسی مصرف حافظه
        final_memory = process.memory_info().rss / 1024 / 1024
        memory_used = final_memory - initial_memory
        
        return {
            "success": True,
            "result": result,
            "memory_used_mb": memory_used
        }
    finally:
        # اطمینان از پاکسازی
        cleanup_resources()
        gc.collect()
```

### 5. بررسی لاگ‌های سیستم

#### macOS/Linux:
```bash
# لاگ‌های Cursor
tail -f ~/.cursor/logs/main.log | grep -i mcp

# لاگ‌های سیستم
journalctl -f | grep cursor

# Process monitoring
ps aux | grep -E "(cursor|mcp|python.*server)"
```

#### Windows PowerShell:
```powershell
# لاگ‌های Cursor
Get-Content "$env:APPDATA\Cursor\logs\main.log" -Tail 50 -Wait | Select-String "mcp"

# Event Viewer
Get-EventLog -LogName Application -Source "Cursor" -Newest 20

# Process monitoring
Get-Process | Where-Object {$_.Name -match "cursor|python"}
```

### 6. تست ابزارهای MCP

#### ایجاد تست خودکار
```python
# test_mcp_server.py
import unittest
import asyncio
from your_server import server, get_weather

class TestMCPServer(unittest.TestCase):
    def test_get_weather(self):
        result = get_weather("تهران", "celsius")
        self.assertTrue(result["success"])
        self.assertIn("temperature", result)
        self.assertIn("°C", result["temperature"])
    
    def test_invalid_city(self):
        result = get_weather("شهر_نامعتبر", "celsius")
        # باید handle شود
        self.assertIsNotNone(result)

if __name__ == "__main__":
    unittest.main()
```

### 7. مانیتورینگ عملکرد

```python
import time
from functools import wraps

def monitor_performance(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        start_memory = psutil.Process().memory_info().rss / 1024 / 1024
        
        try:
            result = func(*args, **kwargs)
            success = True
        except Exception as e:
            result = {"error": str(e)}
            success = False
        
        end_time = time.time()
        end_memory = psutil.Process().memory_info().rss / 1024 / 1024
        
        logger.info(f"""
        Function: {func.__name__}
        Duration: {end_time - start_time:.2f}s
        Memory Delta: {end_memory - start_memory:.2f}MB
        Success: {success}
        """)
        
        return result
    return wrapper

@server.tool()
@monitor_performance
def expensive_operation(data: str) -> dict:
    # عملیات پرهزینه
    pass
```

## بهترین شیوه‌ها و بهینه‌سازی

### 1. امنیت پیشرفته

#### 1.1. مدیریت امن اطلاعات حساس
```python
import os
from cryptography.fernet import Fernet
from typing import Dict, Any
import keyring

class SecureConfig:
    """مدیریت امن تنظیمات و کلیدها"""
    
    def __init__(self, app_name: str):
        self.app_name = app_name
        self.cipher_suite = None
        self._init_encryption()
    
    def _init_encryption(self):
        """راه‌اندازی رمزنگاری"""
        # کلید رمزنگاری را از keyring دریافت یا ایجاد کن
        key = keyring.get_password(self.app_name, "encryption_key")
        if not key:
            key = Fernet.generate_key().decode()
            keyring.set_password(self.app_name, "encryption_key", key)
        self.cipher_suite = Fernet(key.encode())
    
    def get_secret(self, key: str) -> str:
        """دریافت امن مقدار رمزنگاری شده"""
        # ابتدا از متغیرهای محیطی
        value = os.environ.get(key)
        if value:
            return value
        
        # سپس از keyring
        value = keyring.get_password(self.app_name, key)
        if value:
            return value
        
        # در نهایت از فایل رمزنگاری شده
        try:
            with open(f".secrets/{key}.enc", "rb") as f:
                encrypted = f.read()
                return self.cipher_suite.decrypt(encrypted).decode()
        except:
            return None
    
    def set_secret(self, key: str, value: str):
        """ذخیره امن مقدار حساس"""
        # در keyring
        keyring.set_password(self.app_name, key, value)
        
        # و همچنین رمزنگاری شده در فایل
        os.makedirs(".secrets", exist_ok=True)
        encrypted = self.cipher_suite.encrypt(value.encode())
        with open(f".secrets/{key}.enc", "wb") as f:
            f.write(encrypted)

# استفاده در سرور MCP
config = SecureConfig("my-mcp-server")

@server.tool()
def secure_api_call(endpoint: str) -> Dict[str, Any]:
    """فراخوانی امن API"""
    api_key = config.get_secret("API_KEY")
    if not api_key:
        return {"success": False, "error": "کلید API یافت نشد"}
    
    headers = {"Authorization": f"Bearer {api_key}"}
    # ادامه عملیات...
```

#### 1.2. احراز هویت و مجوزدهی
```python
from functools import wraps
import jwt
from datetime import datetime, timedelta

class MCPAuth:
    """سیستم احراز هویت برای MCP"""
    
    def __init__(self, secret_key: str):
        self.secret_key = secret_key
        self.authorized_users = {}
    
    def generate_token(self, user_id: str, permissions: List[str]) -> str:
        """تولید توکن JWT"""
        payload = {
            "user_id": user_id,
            "permissions": permissions,
            "exp": datetime.utcnow() + timedelta(hours=24)
        }
        return jwt.encode(payload, self.secret_key, algorithm="HS256")
    
    def verify_token(self, token: str) -> Dict[str, Any]:
        """اعتبارسنجی توکن"""
        try:
            payload = jwt.decode(token, self.secret_key, algorithms=["HS256"])
            return {"valid": True, "payload": payload}
        except jwt.ExpiredSignatureError:
            return {"valid": False, "error": "توکن منقضی شده است"}
        except jwt.InvalidTokenError:
            return {"valid": False, "error": "توکن نامعتبر است"}
    
    def require_permission(self, permission: str):
        """دکوریتور برای بررسی مجوز"""
        def decorator(func):
            @wraps(func)
            def wrapper(*args, **kwargs):
                # دریافت توکن از context
                token = kwargs.get("auth_token")
                if not token:
                    return {"success": False, "error": "احراز هویت مورد نیاز است"}
                
                result = self.verify_token(token)
                if not result["valid"]:
                    return {"success": False, "error": result["error"]}
                
                if permission not in result["payload"]["permissions"]:
                    return {"success": False, "error": "مجوز کافی ندارید"}
                
                return func(*args, **kwargs)
            return wrapper
        return decorator

# استفاده
auth = MCPAuth(config.get_secret("JWT_SECRET"))

@server.tool()
@auth.require_permission("admin:write")
def sensitive_operation(data: str, auth_token: str = None) -> Dict[str, Any]:
    """عملیات حساس که نیاز به مجوز دارد"""
    # عملیات امن
    pass
```

#### 1.3. رمزنگاری ارتباطات
```python
import ssl
import certifi
from aiohttp import ClientSession, TCPConnector

class SecureMCPClient:
    """کلاینت امن برای ارتباطات MCP"""
    
    def __init__(self):
        self.ssl_context = ssl.create_default_context(cafile=certifi.where())
        self.ssl_context.check_hostname = True
        self.ssl_context.verify_mode = ssl.CERT_REQUIRED
    
    async def secure_request(self, url: str, data: Dict[str, Any]) -> Dict[str, Any]:
        """درخواست HTTPS امن"""
        connector = TCPConnector(ssl=self.ssl_context)
        async with ClientSession(connector=connector) as session:
            async with session.post(url, json=data) as response:
                return await response.json()
```

### 2. بهینه‌سازی کارایی

#### 2.1. کش پیشرفته
```python
from functools import lru_cache
import redis
import pickle
from typing import Any, Optional
import asyncio

class AdvancedCache:
    """سیستم کش چندسطحی"""
    
    def __init__(self, redis_url: str = None):
        self.memory_cache = {}
        self.redis_client = redis.from_url(redis_url) if redis_url else None
    
    async def get(self, key: str) -> Optional[Any]:
        """دریافت از کش"""
        # سطح 1: حافظه
        if key in self.memory_cache:
            return self.memory_cache[key]
        
        # سطح 2: Redis
        if self.redis_client:
            value = self.redis_client.get(key)
            if value:
                deserialized = pickle.loads(value)
                self.memory_cache[key] = deserialized
                return deserialized
        
        return None
    
    async def set(self, key: str, value: Any, ttl: int = 3600):
        """ذخیره در کش"""
        # حافظه
        self.memory_cache[key] = value
        
        # Redis
        if self.redis_client:
            serialized = pickle.dumps(value)
            self.redis_client.setex(key, ttl, serialized)
    
    def invalidate(self, pattern: str = None):
        """پاکسازی کش"""
        if pattern:
            # پاکسازی بر اساس الگو
            keys_to_delete = [k for k in self.memory_cache.keys() if pattern in k]
            for key in keys_to_delete:
                del self.memory_cache[key]
            
            if self.redis_client:
                for key in self.redis_client.scan_iter(match=f"*{pattern}*"):
                    self.redis_client.delete(key)
        else:
            # پاکسازی کامل
            self.memory_cache.clear()
            if self.redis_client:
                self.redis_client.flushdb()

# استفاده در سرور
cache = AdvancedCache("redis://localhost:6379")

@server.tool()
async def cached_expensive_operation(param: str) -> Dict[str, Any]:
    """عملیات سنگین با کش"""
    cache_key = f"expensive:{param}"
    
    # بررسی کش
    cached_result = await cache.get(cache_key)
    if cached_result:
        return cached_result
    
    # اجرای عملیات
    result = await perform_expensive_operation(param)
    
    # ذخیره در کش
    await cache.set(cache_key, result, ttl=1800)  # 30 دقیقه
    
    return result
```

#### 2.2. Connection Pooling و Resource Management
```python
from contextlib import asynccontextmanager
import asyncpg
import aiohttp
from typing import AsyncGenerator

class ResourcePool:
    """مدیریت منابع با pooling"""
    
    def __init__(self):
        self.db_pool = None
        self.http_session = None
        self.initialized = False
    
    async def initialize(self):
        """راه‌اندازی pool ها"""
        if not self.initialized:
            # Database pool
            self.db_pool = await asyncpg.create_pool(
                host='localhost',
                port=5432,
                user='user',
                password='password',
                database='mydb',
                min_size=10,
                max_size=20,
                command_timeout=60
            )
            
            # HTTP session با connection pooling
            connector = aiohttp.TCPConnector(
                limit=100,  # حداکثر اتصالات
                limit_per_host=30,  # حداکثر اتصال به هر host
                ttl_dns_cache=300  # کش DNS
            )
            self.http_session = aiohttp.ClientSession(connector=connector)
            
            self.initialized = True
    
    @asynccontextmanager
    async def get_db_connection(self) -> AsyncGenerator[asyncpg.Connection, None]:
        """دریافت connection از pool"""
        if not self.initialized:
            await self.initialize()
        
        async with self.db_pool.acquire() as connection:
            yield connection
    
    async def cleanup(self):
        """پاکسازی منابع"""
        if self.db_pool:
            await self.db_pool.close()
        if self.http_session:
            await self.http_session.close()

# استفاده
resources = ResourcePool()

@server.tool()
async def efficient_db_operation(query: str) -> Dict[str, Any]:
    """عملیات دیتابیس با connection pooling"""
    async with resources.get_db_connection() as conn:
        results = await conn.fetch(query)
        return {
            "success": True,
            "data": [dict(row) for row in results]
        }

# پاکسازی در shutdown
@server.on_shutdown()
async def cleanup():
    await resources.cleanup()
```

#### 2.3. بهینه‌سازی حافظه و پردازش موازی
```python
import asyncio
from concurrent.futures import ProcessPoolExecutor
import numpy as np
from typing import List, Dict, Any

class OptimizedProcessor:
    """پردازشگر بهینه با قابلیت موازی‌سازی"""
    
    def __init__(self, max_workers: int = 4):
        self.executor = ProcessPoolExecutor(max_workers=max_workers)
        self.semaphore = asyncio.Semaphore(max_workers * 2)
    
    async def process_batch(self, items: List[Any], processor_func) -> List[Any]:
        """پردازش دسته‌ای با محدودیت همزمانی"""
        async def process_with_limit(item):
            async with self.semaphore:
                loop = asyncio.get_event_loop()
                return await loop.run_in_executor(
                    self.executor, processor_func, item
                )
        
        tasks = [process_with_limit(item) for item in items]
        return await asyncio.gather(*tasks)
    
    def optimize_memory(self, data: np.ndarray) -> np.ndarray:
        """بهینه‌سازی نوع داده برای کاهش مصرف حافظه"""
        if data.dtype == np.float64:
            # تبدیل به float32 اگر دقت کافی است
            if np.all(np.abs(data) < 1e6):
                return data.astype(np.float32)
        elif data.dtype == np.int64:
            # تبدیل به int32 اگر محدوده کافی است
            if data.min() >= -2**31 and data.max() < 2**31:
                return data.astype(np.int32)
        return data

processor = OptimizedProcessor()

@server.tool()
async def parallel_data_processing(data_list: List[Dict[str, Any]]) -> Dict[str, Any]:
    """پردازش موازی داده‌ها"""
    def heavy_computation(item):
        # محاسبات سنگین
        result = complex_algorithm(item)
        return result
    
    # پردازش موازی
    results = await processor.process_batch(data_list, heavy_computation)
    
    return {
        "success": True,
        "processed": len(results),
        "results": results
    }
```

### 3. مانیتورینگ و Observability

```python
import time
from prometheus_client import Counter, Histogram, Gauge
import structlog
from opentelemetry import trace
from opentelemetry.exporter.jaeger import JaegerExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor

# تنظیم Prometheus metrics
request_count = Counter('mcp_requests_total', 'Total MCP requests', ['method', 'status'])
request_duration = Histogram('mcp_request_duration_seconds', 'Request duration', ['method'])
active_connections = Gauge('mcp_active_connections', 'Active connections')

# تنظیم structured logging
logger = structlog.get_logger()

# تنظیم tracing
trace.set_tracer_provider(TracerProvider())
tracer = trace.get_tracer(__name__)

jaeger_exporter = JaegerExporter(
    agent_host_name="localhost",
    agent_port=6831,
)
span_processor = BatchSpanProcessor(jaeger_exporter)
trace.get_tracer_provider().add_span_processor(span_processor)

def monitor_performance(func):
    """دکوریتور برای مانیتورینگ عملکرد"""
    @wraps(func)
    async def wrapper(*args, **kwargs):
        method_name = func.__name__
        
        # شروع trace
        with tracer.start_as_current_span(method_name) as span:
            # شروع زمان‌سنجی
            start_time = time.time()
            active_connections.inc()
            
            try:
                # اجرای تابع
                result = await func(*args, **kwargs)
                
                # ثبت موفقیت
                request_count.labels(method=method_name, status='success').inc()
                span.set_status(trace.Status(trace.StatusCode.OK))
                
                logger.info(
                    "operation_completed",
                    method=method_name,
                    duration=time.time() - start_time,
                    success=True
                )
                
                return result
                
            except Exception as e:
                # ثبت خطا
                request_count.labels(method=method_name, status='error').inc()
                span.set_status(trace.Status(trace.StatusCode.ERROR, str(e)))
                span.record_exception(e)
                
                logger.error(
                    "operation_failed",
                    method=method_name,
                    duration=time.time() - start_time,
                    error=str(e),
                    exc_info=True
                )
                
                raise
            
            finally:
                # ثبت مدت زمان
                duration = time.time() - start_time
                request_duration.labels(method=method_name).observe(duration)
                active_connections.dec()

@server.tool()
@monitor_performance
async def monitored_operation(param: str) -> Dict[str, Any]:
    """عملیات با مانیتورینگ کامل"""
    # عملیات اصلی
    pass
```

### 4. مدیریت خطای پیشرفته

```python
from typing import Type, Callable, Dict, Any
import traceback
from enum import Enum

class ErrorSeverity(Enum):
    LOW = "low"
    MEDIUM = "medium"
    HIGH = "high"
    CRITICAL = "critical"

class MCPError(Exception):
    """کلاس پایه خطاهای MCP"""
    def __init__(self, message: str, severity: ErrorSeverity = ErrorSeverity.MEDIUM, details: Dict[str, Any] = None):
        super().__init__(message)
        self.severity = severity
        self.details = details or {}

class ValidationError(MCPError):
    """خطای اعتبارسنجی ورودی"""
    def __init__(self, message: str, field: str = None):
        super().__init__(message, ErrorSeverity.LOW, {"field": field})

class ExternalServiceError(MCPError):
    """خطای سرویس خارجی"""
    def __init__(self, message: str, service: str, status_code: int = None):
        super().__init__(message, ErrorSeverity.HIGH, {
            "service": service,
            "status_code": status_code
        })

class ErrorHandler:
    """مدیریت مرکزی خطاها"""
    
    def __init__(self):
        self.error_handlers: Dict[Type[Exception], Callable] = {}
        self.fallback_handler = self.default_handler
    
    def register_handler(self, error_type: Type[Exception], handler: Callable):
        """ثبت handler برای نوع خطا"""
        self.error_handlers[error_type] = handler
    
    def handle_error(self, error: Exception) -> Dict[str, Any]:
        """مدیریت خطا"""
        # پیدا کردن handler مناسب
        for error_type, handler in self.error_handlers.items():
            if isinstance(error, error_type):
                return handler(error)
        
        # استفاده از fallback handler
        return self.fallback_handler(error)
    
    def default_handler(self, error: Exception) -> Dict[str, Any]:
        """Handler پیش‌فرض"""
        return {
            "success": False,
            "error": str(error),
            "type": type(error).__name__,
            "traceback": traceback.format_exc() if DEBUG else None
        }

# تنظیم error handler
error_handler = ErrorHandler()

# ثبت handlers اختصاصی
error_handler.register_handler(ValidationError, lambda e: {
    "success": False,
    "error": str(e),
    "field": e.details.get("field"),
    "severity": e.severity.value
})

error_handler.register_handler(ExternalServiceError, lambda e: {
    "success": False,
    "error": str(e),
    "service": e.details.get("service"),
    "status_code": e.details.get("status_code"),
    "severity": e.severity.value,
    "retry_after": 60  # ثانیه
})

def with_error_handling(func):
    """دکوریتور برای مدیریت خطا"""
    @wraps(func)
    async def wrapper(*args, **kwargs):
        try:
            return await func(*args, **kwargs)
        except Exception as e:
            return error_handler.handle_error(e)
    return wrapper

@server.tool()
@with_error_handling
async def robust_operation(data: Dict[str, Any]) -> Dict[str, Any]:
    """عملیات با مدیریت خطای قوی"""
    # اعتبارسنجی ورودی
    if not data.get("required_field"):
        raise ValidationError("فیلد اجباری وجود ندارد", field="required_field")
    
    # فراخوانی سرویس خارجی
    try:
        response = await external_api_call(data)
    except Exception as e:
        raise ExternalServiceError(
            "خطا در ارتباط با سرویس خارجی",
            service="external_api",
            status_code=500
        )
    
    return {"success": True, "data": response}
```

## توسعه پیشرفته

### 1. استفاده از Async/Await
```python
import asyncio
from fastmcp import FastMCP

server = FastMCP("سرور async")

@server.tool()
async def async_fetch_data(url: str) -> dict:
    """دریافت داده به صورت غیرهمزمان"""
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            data = await response.json()
            return {"success": True, "data": data}
```

### 2. استفاده از Streaming
```python
@server.tool()
def stream_large_file(file_path: str):
    """خواندن فایل بزرگ به صورت stream"""
    def file_generator():
        with open(file_path, 'r', encoding='utf-8') as f:
            for line in f:
                yield line.strip()
    
    return {
        "type": "stream",
        "generator": file_generator()
    }
```

### 3. پشتیبانی از چند زبان
```python
translations = {
    "fa": {
        "welcome": "خوش آمدید",
        "error": "خطا رخ داده است"
    },
    "en": {
        "welcome": "Welcome",
        "error": "An error occurred"
    }
}

@server.tool()
def get_message(key: str, lang: str = "fa") -> str:
    """دریافت پیام با پشتیبانی چند زبانه"""
    return translations.get(lang, {}).get(key, key)
```

## اکوسیستم MCP و سرورهای محبوب

### سرورهای MCP آماده استفاده

#### 1. سرورهای رسمی Anthropic
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@anthropic/mcp-server-filesystem", "/path/to/allowed/directory"]
    },
    "github": {
      "command": "npx",
      "args": ["@anthropic/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "your-github-token"
      }
    },
    "gitlab": {
      "command": "npx",
      "args": ["@anthropic/mcp-server-gitlab"],
      "env": {
        "GITLAB_TOKEN": "your-gitlab-token"
      }
    }
  }
}
```

#### 2. سرورهای توسعه و DevOps

**Kubernetes MCP Server:**
```json
{
  "kubernetes": {
    "command": "npx",
    "args": ["@mcp/kubernetes-server"],
    "env": {
      "KUBECONFIG": "/path/to/kubeconfig"
    }
  }
}
```

**Terraform MCP Server:**
```json
{
  "terraform": {
    "command": "python",
    "args": ["/path/to/terraform-mcp-server.py"],
    "env": {
      "TF_WORKSPACE": "production"
    }
  }
}
```

#### 3. سرورهای پایگاه داده

**PostgreSQL MCP Server:**
```json
{
  "postgres": {
    "command": "npx",
    "args": ["@mcp/postgres-server"],
    "env": {
      "DATABASE_URL": "postgresql://user:pass@localhost/db"
    }
  }
}
```

**MongoDB MCP Server:**
```json
{
  "mongodb": {
    "command": "npx",
    "args": ["@mcp/mongodb-server"],
    "env": {
      "MONGODB_URI": "mongodb://localhost:27017/mydb"
    }
  }
}
```

#### 4. سرورهای مانیتورینگ و Analytics

**Prometheus MCP Server:**
```json
{
  "prometheus": {
    "command": "python",
    "args": ["/path/to/prometheus-mcp.py"],
    "env": {
      "PROMETHEUS_URL": "http://localhost:9090"
    }
  }
}
```

**Grafana MCP Server:**
```json
{
  "grafana": {
    "command": "node",
    "args": ["/path/to/grafana-mcp.js"],
    "env": {
      "GRAFANA_URL": "http://localhost:3000",
      "GRAFANA_API_KEY": "your-api-key"
    }
  }
}
```

### ساخت پکیج MCP قابل توزیع

#### 1. ساختار پروژه استاندارد
```
my-mcp-server/
├── src/
│   ├── __init__.py
│   ├── server.py
│   ├── tools/
│   │   ├── __init__.py
│   │   └── handlers.py
│   └── utils/
│       ├── __init__.py
│       └── helpers.py
├── tests/
│   └── test_server.py
├── setup.py
├── pyproject.toml
├── requirements.txt
├── README.md
└── LICENSE
```

#### 2. فایل setup.py
```python
from setuptools import setup, find_packages

setup(
    name="my-mcp-server",
    version="1.0.0",
    author="Your Name",
    author_email="your.email@example.com",
    description="یک سرور MCP برای [توضیحات]",
    long_description=open("README.md", "r", encoding="utf-8").read(),
    long_description_content_type="text/markdown",
    url="https://github.com/yourusername/my-mcp-server",
    packages=find_packages(where="src"),
    package_dir={"": "src"},
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
    python_requires=">=3.8",
    install_requires=[
        "fastmcp>=0.1.0",
        "aiohttp>=3.8.0",
        # دیگر وابستگی‌ها
    ],
    entry_points={
        "console_scripts": [
            "my-mcp-server=my_mcp_server.server:main",
        ],
    },
)
```

#### 3. انتشار در PyPI
```bash
# ساخت پکیج
python setup.py sdist bdist_wheel

# آپلود به PyPI
pip install twine
twine upload dist/*

# نصب برای کاربران
pip install my-mcp-server
```

### راهنمای توسعه سرور سفارشی

#### 1. قالب پروژه کامل
```python
# template_server.py
from fastmcp import FastMCP
from typing import Dict, Any, List, Optional
import asyncio
import logging
from datetime import datetime

# تنظیمات لاگ
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class CustomMCPServer:
    """کلاس اصلی سرور MCP سفارشی"""
    
    def __init__(self, name: str, version: str = "1.0.0"):
        self.server = FastMCP(name)
        self.version = version
        self.start_time = datetime.now()
        self._setup_tools()
        self._setup_handlers()
    
    def _setup_tools(self):
        """تنظیم ابزارهای سرور"""
        
        @self.server.tool()
        async def get_server_info() -> Dict[str, Any]:
            """اطلاعات سرور"""
            uptime = (datetime.now() - self.start_time).total_seconds()
            return {
                "name": self.server.name,
                "version": self.version,
                "uptime_seconds": uptime,
                "status": "healthy"
            }
        
        @self.server.tool()
        async def echo(message: str) -> Dict[str, Any]:
            """ابزار تست - بازگرداندن پیام"""
            return {
                "success": True,
                "echo": message,
                "timestamp": datetime.now().isoformat()
            }
    
    def _setup_handlers(self):
        """تنظیم event handlers"""
        
        @self.server.on_startup()
        async def startup():
            logger.info(f"سرور {self.server.name} راه‌اندازی شد")
            # راه‌اندازی منابع
        
        @self.server.on_shutdown()
        async def shutdown():
            logger.info("در حال خاموش شدن سرور...")
            # پاکسازی منابع
    
    def run(self):
        """اجرای سرور"""
        logger.info(f"شروع سرور {self.server.name} نسخه {self.version}")
        self.server.run()

def main():
    """نقطه ورود اصلی"""
    server = CustomMCPServer("my-custom-server", "1.0.0")
    server.run()

if __name__ == "__main__":
    main()
```

#### 2. تست سرور MCP
```python
# test_mcp_server.py
import pytest
import asyncio
from unittest.mock import Mock, patch
from my_mcp_server import CustomMCPServer

@pytest.fixture
def server():
    """ایجاد instance سرور برای تست"""
    return CustomMCPServer("test-server")

@pytest.mark.asyncio
async def test_server_info(server):
    """تست ابزار server_info"""
    # شبیه‌سازی فراخوانی ابزار
    tools = server.server.tools
    server_info_tool = next(t for t in tools if t.name == "get_server_info")
    
    result = await server_info_tool.handler()
    
    assert result["name"] == "test-server"
    assert result["version"] == "1.0.0"
    assert "uptime_seconds" in result
    assert result["status"] == "healthy"

@pytest.mark.asyncio
async def test_echo_tool(server):
    """تست ابزار echo"""
    tools = server.server.tools
    echo_tool = next(t for t in tools if t.name == "echo")
    
    test_message = "سلام دنیا"
    result = await echo_tool.handler(message=test_message)
    
    assert result["success"] is True
    assert result["echo"] == test_message
    assert "timestamp" in result
```

### لیست سرورهای MCP پرکاربرد

| نام سرور | توضیحات | نصب |
|-----------|----------|------|
| **filesystem** | دسترسی به فایل سیستم | `npx @anthropic/mcp-server-filesystem` |
| **github** | تعامل با GitHub API | `npx @anthropic/mcp-server-github` |
| **slack** | ارسال پیام به Slack | `npm install @mcp/slack-server` |
| **jira** | مدیریت issues در Jira | `pip install mcp-jira-server` |
| **aws** | تعامل با سرویس‌های AWS | `npm install @mcp/aws-server` |
| **azure** | مدیریت منابع Azure | `pip install mcp-azure-server` |
| **notion** | تعامل با Notion API | `npx @mcp/notion-server` |
| **discord** | ارسال پیام به Discord | `npm install mcp-discord` |
| **elasticsearch** | جستجو در Elasticsearch | `pip install mcp-elasticsearch` |
| **redis** | مدیریت Redis | `npm install @mcp/redis-server` |

### نحوه مشارکت در اکوسیستم MCP

#### 1. ایجاد سرور جدید
1. از template بالا استفاده کنید
2. ابزارهای مفید اضافه کنید
3. تست‌های کامل بنویسید
4. مستندسازی واضح ارائه دهید

#### 2. بهبود سرورهای موجود
1. Fork کردن repository
2. اضافه کردن features جدید
3. رفع باگ‌ها
4. ارسال Pull Request

#### 3. اشتراک‌گذاری تجربیات
1. نوشتن مقاله یا آموزش
2. ایجاد ویدیوهای آموزشی
3. پاسخ به سوالات در انجمن‌ها

## منابع مفید

### مستندات رسمی
- [Model Context Protocol Specification](https://modelcontextprotocol.io/)
- [Cursor Documentation](https://cursor.sh/docs)
- [FastMCP Documentation](https://github.com/fastmcp/fastmcp)
- [MCP SDK Documentation](https://github.com/anthropics/model-context-protocol)

### آموزش‌های ویدیویی
- [دوره جامع MCP در Cursor (فارسی)](https://example.com/mcp-course-fa)
- [Building MCP Servers from Scratch](https://youtube.com/watch?v=example)
- [Advanced MCP Patterns](https://youtube.com/watch?v=example2)

### مخازن GitHub مفید
- [Awesome MCP Servers](https://github.com/awesome-mcp/awesome-mcp-servers)
- [MCP Server Examples](https://github.com/anthropics/mcp-server-examples)
- [Community MCP Servers](https://github.com/topics/mcp-server)

### انجمن‌ها و پشتیبانی
- [انجمن فارسی Cursor](https://t.me/cursor_fa)
- [MCP Developers Discord](https://discord.gg/mcp-dev)
- [Stack Overflow - Tag: mcp-cursor](https://stackoverflow.com/questions/tagged/mcp-cursor)
- [Reddit - r/ModelContextProtocol](https://reddit.com/r/ModelContextProtocol)

## نتیجه‌گیری

پروتکل زمینه مدل (MCP) یک ابزار قدرتمند برای گسترش قابلیت‌های Cursor است. با استفاده از این پروتکل می‌توانید:

1. **بهره‌وری را افزایش دهید**: با اتصال به منابع داده و ابزارهای مختلف
2. **کیفیت کد را بهبود بخشید**: با دسترسی به اطلاعات دقیق و به‌روز
3. **فرآیندها را خودکارسازی کنید**: با ایجاد سرورهای سفارشی
4. **تجربه توسعه را بهینه کنید**: با شخصی‌سازی محیط کار

امیدواریم این راهنما به شما در استفاده بهینه از MCP در Cursor کمک کند. برای سوالات و پیشنهادات می‌توانید با جامعه توسعه‌دهندگان در ارتباط باشید.

---

*آخرین به‌روزرسانی: دی ماه 1403*

*نسخه راهنما: 1.0.0*