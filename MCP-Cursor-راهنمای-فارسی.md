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

## عیب‌یابی و رفع مشکلات

### 1. سرور راه‌اندازی نمی‌شود
- مسیر فایل سرور را بررسی کنید
- مطمئن شوید Python/Node.js نصب شده است
- لاگ‌های Cursor را بررسی کنید

### 2. ابزارها در Cursor ظاهر نمی‌شوند
- Cursor را restart کنید
- فایل پیکربندی MCP را بررسی کنید
- از syntax صحیح JSON اطمینان حاصل کنید

### 3. خطاهای اجرا
```python
# برای دیباگ سرور Python:
import logging

logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    filename='mcp_server.log'
)
```

### 4. بررسی لاگ‌های Cursor
- **macOS/Linux**: `tail -f ~/.cursor/logs/main.log`
- **Windows**: در Event Viewer بررسی کنید

## بهترین شیوه‌ها

### 1. امنیت
- هرگز کلیدهای API را مستقیم در کد قرار ندهید
- از متغیرهای محیطی استفاده کنید
- دسترسی‌ها را محدود کنید

### 2. کارایی
- از کش برای داده‌های تکراری استفاده کنید
- connection pooling برای پایگاه داده
- rate limiting برای API های خارجی

### 3. مستندسازی
- همیشه docstring کامل برای ابزارها بنویسید
- پارامترها را به وضوح توضیح دهید
- خروجی‌ها را مستند کنید

### 4. مدیریت خطا
```python
@server.tool()
def safe_operation(param: str) -> dict:
    """عملیات امن با مدیریت خطا"""
    try:
        # عملیات اصلی
        result = perform_operation(param)
        return {
            "success": True,
            "data": result
        }
    except ValueError as e:
        return {
            "success": False,
            "error": f"خطای ورودی: {str(e)}",
            "error_type": "validation"
        }
    except Exception as e:
        logging.error(f"خطای غیرمنتظره: {str(e)}")
        return {
            "success": False,
            "error": "خطای سیستمی رخ داده است",
            "error_type": "system"
        }
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

## منابع مفید

### مستندات رسمی
- [Model Context Protocol Specification](https://modelcontextprotocol.io/)
- [Cursor Documentation](https://cursor.sh/docs)
- [FastMCP Documentation](https://github.com/fastmcp/fastmcp)

### آموزش‌های ویدیویی
- دوره جامع MCP در Cursor (فارسی)
- راه‌اندازی سرورهای MCP
- بهترین شیوه‌های توسعه

### مخازن GitHub
- [MCP Servers Collection](https://github.com/topics/mcp-server)
- [Cursor MCP Examples](https://github.com/cursor/mcp-examples)

### انجمن‌ها و پشتیبانی
- انجمن فارسی Cursor
- گروه تلگرام MCP Developers
- Stack Overflow با تگ `mcp-cursor`

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