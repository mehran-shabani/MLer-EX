# عیب‌یابی و پشتیبانی

## مشکلات رایج

### 1. مشکلات نصب و راه‌اندازی

#### مشکل: Cursor اجرا نمی‌شود
**علائم:**
- برنامه باز نمی‌شود
- خطای "Application failed to start"
- صفحه سیاه یا سفید

**راه‌حل‌ها:**
1. **بررسی سیستم عامل**
   - Windows 10+ یا macOS 10.15+ یا Linux معتبر
   - حداقل 4GB RAM
   - فضای دیسک کافی

2. **حذف و نصب مجدد**
   ```bash
   # Windows
   Control Panel → Programs → Uninstall Cursor
   
   # macOS
   Applications → Cursor → Move to Trash
   
   # Linux
   sudo apt remove cursor
   ```

3. **پاک کردن cache**
   ```bash
   # Windows
   %APPDATA%\Cursor\Cache
   
   # macOS
   ~/Library/Application Support/Cursor/Cache
   
   # Linux
   ~/.config/Cursor/Cache
   ```

#### مشکل: خطای احراز هویت
**علائم:**
- "Authentication failed"
- "Invalid credentials"
- عدم امکان ورود

**راه‌حل‌ها:**
1. **بررسی اتصال اینترنت**
2. **تأیید ایمیل**
3. **بازنشانی رمز عبور**
4. **پاک کردن تنظیمات احراز هویت**

### 2. مشکلات Chat AI

#### مشکل: AI پاسخ نمی‌دهد
**علائم:**
- پیام‌ها ارسال نمی‌شوند
- "Connection timeout"
- "Service unavailable"

**راه‌حل‌ها:**
1. **بررسی اتصال اینترنت**
2. **تست سرویس AI**
   ```bash
   curl https://api.cursor.com/health
   ```

3. **تغییر مدل AI**
   - Settings → AI → Model Selection
   - انتخاب مدل دیگر

4. **پاک کردن cache چت**
   - Chat → Settings → Clear History

#### مشکل: پاسخ‌های نامناسب
**علائم:**
- پاسخ‌های غیرمرتبط
- کد نادرست
- توضیحات مبهم

**راه‌حل‌ها:**
1. **بهبود سوالات**
   - مشخص‌تر باشید
   - context بیشتری فراهم کنید
   - مثال‌های عملی ارائه دهید

2. **تنظیم Temperature**
   - Settings → AI → Temperature
   - کاهش برای پاسخ‌های دقیق‌تر

3. **تغییر Context Size**
   - افزایش برای پروژه‌های بزرگ
   - کاهش برای سرعت بیشتر

### 3. مشکلات عملکرد

#### مشکل: کندی برنامه
**علائم:**
- تأخیر در تایپ
- کندی در جستجو
- مصرف بالای CPU/RAM

**راه‌حل‌ها:**
1. **بررسی منابع سیستم**
   ```bash
   # Windows
   Task Manager → Performance
   
   # macOS
   Activity Monitor
   
   # Linux
   htop
   ```

2. **غیرفعال کردن افزونه‌های غیرضروری**
   - Extensions → Disable
   - حذف افزونه‌های قدیمی

3. **پاک کردن cache**
   - File → Preferences → Clear Cache

4. **تنظیمات بهینه**
   ```json
   {
     "editor.minimap.enabled": false,
     "editor.wordWrap": "off",
     "files.autoSave": "off"
   }
   ```

#### مشکل: خطاهای حافظه
**علائم:**
- "Out of memory"
- "Memory limit exceeded"
- crash کردن برنامه

**راه‌حل‌ها:**
1. **افزایش حافظه مجازی**
2. **بستن فایل‌های غیرضروری**
3. **تنظیم محدودیت حافظه**
4. **استفاده از نسخه 64-bit**

### 4. مشکلات فایل و پروژه

#### مشکل: فایل‌ها باز نمی‌شوند
**علائم:**
- "File not found"
- "Permission denied"
- فایل‌های خراب

**راه‌حل‌ها:**
1. **بررسی مجوزها**
   ```bash
   # Linux/macOS
   chmod 644 filename
   
   # Windows
   Properties → Security
   ```

2. **بررسی مسیر فایل**
   - اطمینان از وجود فایل
   - بررسی کاراکترهای خاص در نام

3. **باز کردن با encoding مناسب**
   - File → Open → Encoding

#### مشکل: مشکلات Git
**علائم:**
- خطاهای Git
- عدم sync شدن
- مشکلات merge

**راه‌حل‌ها:**
1. **بررسی تنظیمات Git**
   ```bash
   git config --list
   git status
   ```

2. **پاک کردن cache Git**
   ```bash
   git rm -r --cached .
   git add .
   git commit -m "Clear cache"
   ```

3. **بازنشانی repository**
   ```bash
   git fetch origin
   git reset --hard origin/main
   ```

## راه‌حل‌های پیشرفته

### 1. تنظیمات پیشرفته

#### فایل settings.json
```json
{
  "editor.fontSize": 14,
  "editor.fontFamily": "Consolas",
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000,
  "workbench.colorTheme": "Default Dark+",
  "terminal.integrated.shell.windows": "C:\\Windows\\System32\\cmd.exe",
  "git.enabled": true,
  "git.autofetch": true
}
```

#### فایل launch.json برای debug
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch Program",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/app.js",
      "console": "integratedTerminal"
    }
  ]
}
```

### 2. خطاهای رایج و راه‌حل‌ها

#### خطای "Extension host terminated"
**راه‌حل:**
1. Restart Cursor
2. غیرفعال کردن افزونه‌های مشکل‌ساز
3. پاک کردن cache افزونه‌ها

#### خطای "Language server crashed"
**راه‌حل:**
1. Restart language server
2. بررسی نصب زبان برنامه‌نویسی
3. تنظیم مجدد path

#### خطای "Workspace trust"
**راه‌حل:**
1. File → Trust Folder
2. تنظیمات workspace trust
3. بررسی امنیت فایل‌ها

### 3. بهینه‌سازی عملکرد

#### تنظیمات سیستم
```bash
# Windows
# افزایش حافظه مجازی
# غیرفعال کردن برنامه‌های غیرضروری

# macOS
# تنظیمات Energy Saver
# پاک کردن cache سیستم

# Linux
# تنظیمات swap
# بهینه‌سازی I/O
```

#### تنظیمات Cursor
```json
{
  "editor.renderWhitespace": "none",
  "editor.renderControlCharacters": false,
  "editor.renderLineHighlight": "none",
  "editor.smoothScrolling": false,
  "workbench.enableExperiments": false,
  "telemetry.telemetryLevel": "off"
}
```

## منابع پشتیبانی

### 1. مستندات رسمی
- [مستندات Cursor](https://docs.cursor.com)
- [راهنمای نصب](https://docs.cursor.com/installation)
- [راهنمای تنظیمات](https://docs.cursor.com/settings)
- [API Reference](https://docs.cursor.com/api)

### 2. انجمن‌های کاربری
- [Discord Community](https://discord.gg/cursor)
- [GitHub Discussions](https://github.com/cursor-ai/cursor/discussions)
- [Reddit r/cursor](https://reddit.com/r/cursor)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/cursor)

### 3. کانال‌های پشتیبانی
- [ایمیل پشتیبانی](support@cursor.com)
- [توییتر](https://twitter.com/cursor_ai)
- [یوتیوب](https://youtube.com/cursor)
- [وبلاگ](https://cursor.com/blog)

### 4. ابزارهای عیب‌یابی

#### Cursor Diagnostics
```bash
# Windows
%APPDATA%\Cursor\logs

# macOS
~/Library/Logs/Cursor

# Linux
~/.config/Cursor/logs
```

#### System Information
```bash
# Windows
systeminfo
dxdiag

# macOS
system_profiler SPHardwareDataType

# Linux
lscpu
free -h
df -h
```

### 5. گزارش مشکلات

#### نحوه گزارش bug
1. **جمع‌آوری اطلاعات**
   - نسخه Cursor
   - سیستم عامل
   - خطای دقیق
   - مراحل تکرار

2. **ایجاد issue**
   - [GitHub Issues](https://github.com/cursor-ai/cursor/issues)
   - توضیح کامل مشکل
   - ضمیمه کردن log files

3. **پیگیری**
   - بررسی پاسخ‌ها
   - ارائه اطلاعات بیشتر
   - تست راه‌حل‌ها

## نکات پیشگیری

### 1. نگهداری منظم
- به‌روزرسانی منظم Cursor
- پاک کردن cache
- بررسی افزونه‌ها
- backup تنظیمات

### 2. بهترین شیوه‌ها
- استفاده از version control
- تست کد قبل از commit
- مستندسازی مناسب
- backup منظم پروژه‌ها

### 3. امنیت
- به‌روزرسانی سیستم عامل
- استفاده از antivirus
- محافظت از داده‌های حساس
- بررسی امنیت کد

## نتیجه‌گیری

### نکات کلیدی عیب‌یابی
1. **مشکل را دقیق شناسایی کنید**: علائم و شرایط را ثبت کنید
2. **راه‌حل‌های ساده را امتحان کنید**: restart، پاک کردن cache
3. **از منابع رسمی استفاده کنید**: مستندات و انجمن‌ها
4. **مشکل را گزارش دهید**: کمک به بهبود محصول
5. **از مشکلات درس بگیرید**: پیشگیری از تکرار

### منابع مفید
- [Troubleshooting Guide](https://docs.cursor.com/troubleshooting)
- [FAQ](https://docs.cursor.com/faq)
- [Known Issues](https://github.com/cursor-ai/cursor/issues)
- [Release Notes](https://github.com/cursor-ai/cursor/releases)