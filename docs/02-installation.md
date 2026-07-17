# راهنمای نصب و راه‌اندازی Cursor

## سیستم‌های عامل پشتیبانی شده

Cursor برای تمام سیستم‌های عامل اصلی در دسترس است:
- **Windows**: ویندوز 10 و بالاتر
- **macOS**: macOS 10.15 (Catalina) و بالاتر
- **Linux**: توزیع‌های اصلی (Ubuntu, Debian, Fedora, Arch)

## مراحل نصب

### 1. دانلود Cursor

1. به وبسایت رسمی [cursor.com](https://cursor.com) مراجعه کنید
2. روی دکمه "Download" کلیک کنید
3. نسخه مناسب سیستم عامل خود به صورت خودکار انتخاب می‌شود

### 2. نصب در Windows

```bash
1. فایل Cursor-Setup.exe را اجرا کنید
2. مسیر نصب را انتخاب کنید (پیش‌فرض توصیه می‌شود)
3. گزینه "Add to PATH" را فعال کنید
4. روی Install کلیک کنید
```

### 3. نصب در macOS

```bash
1. فایل Cursor.dmg را باز کنید
2. آیکون Cursor را به پوشه Applications بکشید
3. از Launchpad یا Spotlight اجرا کنید
```

### 4. نصب در Linux

**Ubuntu/Debian:**
```bash
# دانلود فایل .deb
wget https://cursor.com/download/linux/cursor.deb

# نصب با dpkg
sudo dpkg -i cursor.deb

# رفع وابستگی‌ها (در صورت نیاز)
sudo apt-get install -f
```

**Fedora:**
```bash
# دانلود فایل .rpm
wget https://cursor.com/download/linux/cursor.rpm

# نصب با rpm
sudo rpm -i cursor.rpm
```

**Arch Linux (AUR):**
```bash
yay -S cursor-ide
# یا
paru -S cursor-ide
```

## راه‌اندازی اولیه

### 1. اولین اجرا
پس از نصب، Cursor را اجرا کنید. در اولین اجرا:
- صفحه خوش‌آمدگویی نمایش داده می‌شود
- می‌توانید تور آموزشی را مشاهده کنید

### 2. ورود به حساب کاربری
برای استفاده از ویژگی‌های AI:
1. روی "Sign In" کلیک کنید
2. با GitHub، Google یا ایمیل وارد شوید
3. دوره آزمایشی 14 روزه Pro فعال می‌شود

### 3. وارد کردن تنظیمات VS Code

اگر قبلاً از VS Code استفاده می‌کردید:

```bash
1. File → Preferences → Import VS Code Settings
2. موارد مورد نظر را انتخاب کنید:
   - Extensions (افزونه‌ها)
   - Settings (تنظیمات)
   - Keybindings (میانبرها)
   - Snippets (قطعه کدها)
3. روی Import کلیک کنید
```

### 4. نصب افزونه‌ها

Cursor با تمام افزونه‌های VS Code سازگار است:

```bash
1. Ctrl+Shift+X برای باز کردن Extensions
2. جستجوی افزونه مورد نظر
3. کلیک روی Install
```

**افزونه‌های پیشنهادی:**
- Prettier (فرمت کد)
- ESLint (بررسی کد JavaScript)
- GitLens (ابزارهای Git پیشرفته)
- Material Icon Theme (آیکون‌های زیبا)

## تنظیمات اساسی

### 1. انتخاب تم
```bash
File → Preferences → Color Theme
# یا
Ctrl+K → Ctrl+T
```

### 2. تنظیم فونت
```json
// settings.json
{
  "editor.fontFamily": "Fira Code, Consolas, 'Courier New'",
  "editor.fontSize": 14,
  "editor.fontLigatures": true
}
```

### 3. تنظیمات AI

در بخش تنظیمات Cursor:
```json
{
  // مدل پیش‌فرض برای Chat
  "cursor.chat.defaultModel": "gpt-4",
  
  // مدل برای تکمیل خودکار
  "cursor.tab.model": "cursor-small",
  
  // فعال‌سازی تکمیل خودکار
  "cursor.tab.enabled": true,
  
  // تاخیر تکمیل خودکار (میلی‌ثانیه)
  "cursor.tab.debounceDelay": 300
}
```

### 4. میانبرهای صفحه‌کلید

می‌توانید بین سه حالت انتخاب کنید:
- **Default**: میانبرهای پیش‌فرض Cursor
- **VS Code**: میانبرهای VS Code
- **Custom**: تنظیمات دلخواه

```bash
File → Preferences → Keyboard Shortcuts
# یا
Ctrl+K → Ctrl+S
```

## حل مشکلات رایج

### 1. Cursor باز نمی‌شود
- آنتی‌ویروس را موقتاً غیرفعال کنید
- با حالت Administrator اجرا کنید
- فایروال را بررسی کنید

### 2. AI کار نمی‌کند
- اتصال اینترنت را بررسی کنید
- مطمئن شوید وارد حساب کاربری شده‌اید
- از Settings → Cursor بررسی کنید API فعال است

### 3. افزونه‌ها نصب نمی‌شوند
- Cursor را restart کنید
- پوشه extensions را پاک کنید:
  - Windows: `%USERPROFILE%\.cursor\extensions`
  - macOS/Linux: `~/.cursor/extensions`

### 4. کندی عملکرد
- تعداد افزونه‌ها را کاهش دهید
- از مدل‌های سبک‌تر AI استفاده کنید
- حافظه RAM را افزایش دهید (حداقل 8GB توصیه می‌شود)

## به‌روزرسانی Cursor

Cursor به صورت خودکار بررسی و به‌روزرسانی می‌شود:
- هر بار در هنگام شروع
- می‌توانید دستی بررسی کنید: `Help → Check for Updates`

## نکات امنیتی

1. **حریم خصوصی کد**: Cursor کد شما را ذخیره نمی‌کند
2. **رمزنگاری**: تمام ارتباطات رمزنگاری شده‌اند
3. **Local Mode**: امکان استفاده آفلاین (بدون AI)
4. **تنظیمات Privacy**: از Settings → Privacy قابل تنظیم است