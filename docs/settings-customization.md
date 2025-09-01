# تنظیمات و شخصی‌سازی

## تنظیمات AI

### 1. انتخاب مدل AI

#### مدل‌های موجود
- **Claude 3.5 Sonnet** - تعادل بین سرعت و دقت
- **Claude 3.5 Haiku** - سریع‌ترین مدل
- **GPT-4** - بالاترین دقت
- **GPT-3.5 Turbo** - سریع و اقتصادی

#### نحوه تغییر مدل
- Settings → AI → Model Selection
- انتخاب مدل مورد نظر
- تست عملکرد مدل

### 2. تنظیمات Context

#### اندازه Context Window
- **4K tokens** - برای پروژه‌های کوچک
- **8K tokens** - برای پروژه‌های متوسط
- **16K tokens** - برای پروژه‌های بزرگ
- **32K tokens** - برای پروژه‌های بسیار بزرگ

#### تنظیمات Context
- **Auto-include files** - شامل کردن خودکار فایل‌های مرتبط
- **Context sensitivity** - حساسیت به context
- **Memory retention** - حفظ حافظه بین session‌ها

### 3. تنظیمات پاسخ‌دهی

#### Temperature (خلاقیت)
- **0.0** - پاسخ‌های دقیق و قابل پیش‌بینی
- **0.5** - تعادل بین دقت و خلاقیت
- **1.0** - پاسخ‌های خلاقانه و متنوع

#### Max Tokens
- **512** - پاسخ‌های کوتاه
- **1024** - پاسخ‌های متوسط
- **2048** - پاسخ‌های طولانی
- **4096** - پاسخ‌های بسیار طولانی

### 4. تنظیمات امنیت

#### حریم خصوصی
- **Local processing** - پردازش محلی
- **Data encryption** - رمزگذاری داده‌ها
- **No data retention** - عدم ذخیره داده‌ها

#### فیلترهای محتوا
- **Code safety** - بررسی امنیت کد
- **Content filtering** - فیلتر محتوا
- **Sensitive data detection** - تشخیص داده‌های حساس

## تم‌ها و ظاهر

### 1. تم‌های پیش‌فرض

#### تم‌های تاریک
- **Dark+ (default dark)** - تم تاریک پیش‌فرض
- **Dark (Visual Studio)** - تم تاریک VS
- **Monokai** - تم تاریک با رنگ‌های روشن
- **Dracula** - تم تاریک بنفش

#### تم‌های روشن
- **Light+ (default light)** - تم روشن پیش‌فرض
- **Light (Visual Studio)** - تم روشن VS
- **Solarized Light** - تم روشن آفتابی
- **GitHub Light** - تم روشن GitHub

### 2. شخصی‌سازی رنگ‌ها

#### تنظیمات رنگ
- **Editor background** - رنگ پس‌زمینه ویرایشگر
- **Text color** - رنگ متن
- **Syntax highlighting** - رنگ‌بندی syntax
- **Selection color** - رنگ انتخاب

#### رنگ‌های سفارشی
```json
{
  "workbench.colorCustomizations": {
    "editor.background": "#1e1e1e",
    "editor.foreground": "#d4d4d4",
    "editor.selectionBackground": "#264f78"
  }
}
```

### 3. تنظیمات فونت

#### نوع فونت
- **Monospace fonts** - فونت‌های monospace
- **Programming fonts** - فونت‌های برنامه‌نویسی
- **Custom fonts** - فونت‌های سفارشی

#### تنظیمات فونت
- **Font family** - خانواده فونت
- **Font size** - اندازه فونت
- **Font weight** - ضخامت فونت
- **Line height** - ارتفاع خط

### 4. تنظیمات رابط کاربری

#### نوار ابزار
- **Show toolbar** - نمایش نوار ابزار
- **Customize toolbar** - شخصی‌سازی نوار ابزار
- **Icon size** - اندازه آیکون‌ها

#### پنل‌ها
- **Panel position** - موقعیت پنل‌ها
- **Panel size** - اندازه پنل‌ها
- **Auto-hide panels** - مخفی کردن خودکار پنل‌ها

## افزونه‌ها

### 1. افزونه‌های ضروری

#### زبان‌های برنامه‌نویسی
- **Python** - پشتیبانی از Python
- **JavaScript/TypeScript** - پشتیبانی از JS/TS
- **Java** - پشتیبانی از Java
- **C++** - پشتیبانی از C++

#### ابزارهای توسعه
- **GitLens** - تقویت Git
- **Prettier** - فرمت‌بندی کد
- **ESLint** - بررسی کیفیت کد
- **Debugger** - ابزارهای debug

### 2. افزونه‌های مفید

#### بهره‌وری
- **Auto Rename Tag** - تغییر خودکار تگ‌ها
- **Bracket Pair Colorizer** - رنگ‌بندی براکت‌ها
- **Indent Rainbow** - رنگ‌بندی indent
- **Path Intellisense** - تکمیل مسیر فایل‌ها

#### تم‌ها و آیکون‌ها
- **Material Icon Theme** - آیکون‌های Material
- **One Dark Pro** - تم One Dark
- **Night Owl** - تم Night Owl
- **Tokyo Night** - تم Tokyo Night

### 3. مدیریت افزونه‌ها

#### نصب افزونه
- Extensions → Search → نام افزونه
- کلیک روی Install
- Restart Cursor در صورت نیاز

#### تنظیمات افزونه
- Extensions → نام افزونه → Settings
- تنظیم پارامترهای مورد نظر
- تست عملکرد افزونه

#### حذف افزونه
- Extensions → نام افزونه → Uninstall
- Restart Cursor
- پاک کردن تنظیمات باقی‌مانده

## تنظیمات پیشرفته

### 1. تنظیمات JSON

#### فایل settings.json
```json
{
  "editor.fontSize": 14,
  "editor.fontFamily": "Fira Code",
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.wordWrap": "on",
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000
}
```

#### تنظیمات workspace
- `.vscode/settings.json` - تنظیمات پروژه
- `.vscode/extensions.json` - افزونه‌های پیشنهادی
- `.vscode/launch.json` - تنظیمات debug

### 2. تنظیمات کلیدهای میانبر

#### فایل keybindings.json
```json
[
  {
    "key": "ctrl+shift+r",
    "command": "workbench.action.terminal.runSelectedText"
  },
  {
    "key": "ctrl+shift+t",
    "command": "workbench.action.terminal.runActiveFile"
  }
]
```

### 3. تنظیمات Snippets

#### ایجاد Snippet
```json
{
  "Console log": {
    "prefix": "clg",
    "body": [
      "console.log($1);"
    ],
    "description": "Console log statement"
  }
}
```

## نکات شخصی‌سازی

### 1. بهینه‌سازی عملکرد
- افزونه‌های غیرضروری را غیرفعال کنید
- از فونت‌های بهینه استفاده کنید
- تنظیمات cache را بهینه کنید

### 2. سازگاری با تیم
- از تنظیمات مشترک استفاده کنید
- فایل‌های تنظیمات را به اشتراک بگذارید
- از coding standards یکسان استفاده کنید

### 3. پشتیبان‌گیری
- تنظیمات را export کنید
- از cloud sync استفاده کنید
- نسخه‌های پشتیبان تهیه کنید