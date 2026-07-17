# ویژگی‌های اصلی Cursor

## 1. Tab - تکمیل خودکار هوشمند

### معرفی
Tab قدرتمندترین ویژگی Cursor برای تکمیل خودکار کد است که می‌تواند چندین خط کد را پیش‌بینی کند.

### نحوه استفاده
- **فعال‌سازی**: کافیست شروع به تایپ کنید
- **پذیرش**: کلید Tab را فشار دهید
- **رد کردن**: ESC یا ادامه تایپ

### ویژگی‌های کلیدی
```python
# مثال: Cursor می‌تواند کل تابع را پیش‌بینی کند
def calculate_average(numbers):
    # با فشردن Tab، Cursor ادامه را تکمیل می‌کند
    if not numbers:
        return 0
    return sum(numbers) / len(numbers)
```

### تنظیمات پیشرفته
```json
{
  "cursor.tab.enabled": true,
  "cursor.tab.debounceDelay": 300,
  "cursor.tab.model": "cursor-small",
  "cursor.tab.acceptOnSpace": false
}
```

## 2. Inline Edit (Ctrl+K) - ویرایش درون‌خطی

### کاربردها
- بازنویسی کد انتخاب شده
- اضافه کردن error handling
- تبدیل کد به async/await
- بهینه‌سازی performance

### مثال‌های عملی

**1. اضافه کردن Type Safety:**
```typescript
// کد اولیه را انتخاب کنید و Ctrl+K بزنید
// بنویسید: "add TypeScript types"

// قبل:
function getUser(id) {
  return users.find(u => u.id === id);
}

// بعد:
function getUser(id: number): User | undefined {
  return users.find((u: User) => u.id === id);
}
```

**2. تبدیل به Async:**
```javascript
// بنویسید: "convert to async/await"

// قبل:
function fetchData() {
  return fetch('/api/data')
    .then(res => res.json())
    .then(data => console.log(data));
}

// بعد:
async function fetchData() {
  const res = await fetch('/api/data');
  const data = await res.json();
  console.log(data);
}
```

## 3. Agent (Ctrl+I) - دستیار هوشمند

### قابلیت‌های اصلی
1. **Multi-file Editing**: تغییر همزمان چندین فایل
2. **Terminal Commands**: اجرای دستورات shell
3. **Code Analysis**: تحلیل و بهبود کدبیس
4. **Bug Fixing**: شناسایی و رفع خطاها

### حالت‌های Agent

#### Agent Mode
- کاملترین حالت با تمام ابزارها
- می‌تواند فایل بسازد، ویرایش کند، دستور اجرا کند
- برای تغییرات پیچیده و چندمرحله‌ای

#### Ask Mode
- فقط برای پرسش و پاسخ
- دسترسی به کدبیس دارد اما تغییری نمی‌دهد
- برای درک کد و دریافت توضیحات

#### Manual Mode
- شما کنترل کامل دارید
- Agent پیشنهاد می‌دهد، شما اعمال می‌کنید
- برای موارد حساس و critical

### مثال استفاده از Agent

```bash
# دستور به Agent:
"Create a REST API with Express.js for a todo app with CRUD operations"

# Agent انجام می‌دهد:
1. ایجاد ساختار پروژه
2. نصب dependencies
3. ایجاد server.js
4. پیاده‌سازی routes
5. اضافه کردن middleware
6. تست با curl
```

## 4. Chat - گفتگوی هوشمند

### ویژگی‌های Chat

#### Multi-tab Support
- **ایجاد تب جدید**: Ctrl+T
- **جابجایی بین تب‌ها**: Ctrl+Tab
- **بستن تب**: Ctrl+W

#### Context Management
```bash
# اضافه کردن context:
@file:app.js          # فایل خاص
@folder:src           # کل پوشه
@workspace           # کل پروژه
@web                 # جستجوی وب
@docs               # مستندات رسمی
```

#### Chat History
- **دسترسی**: Alt+Ctrl+'
- **جستجو**: Ctrl+F در history
- **Export**: به فرمت Markdown

### نکات حرفه‌ای Chat

1. **استفاده از Mentions**:
```
@calculateTotal explain how this function works
```

2. **ترکیب Contexts**:
```
@file:api.js @docs:express how to add authentication?
```

3. **سوالات دنباله‌دار**:
```
First: "What's wrong with my React component?"
Then: "How can I optimize the re-renders?"
```

## 5. Checkpoints - نقاط بازگشت

### عملکرد
- ذخیره خودکار قبل از تغییرات Agent
- امکان بازگشت به حالت‌های قبلی
- مدیریت تاریخچه تغییرات

### استفاده
```bash
# مشاهده checkpoints
Ctrl+Shift+P → "Show Checkpoints"

# بازگشت به checkpoint
Click on checkpoint → "Restore"

# ایجاد دستی
Before major changes → "Create Checkpoint"
```

## 6. Terminal Integration

### ویژگی‌ها
- Agent می‌تواند دستورات را اجرا کند
- نمایش real-time خروجی
- تشخیص خطاها و پیشنهاد راه‌حل

### تنظیمات امنیتی
```json
{
  "cursor.agent.terminal.autoExecute": false,
  "cursor.agent.terminal.requireConfirmation": true,
  "cursor.agent.terminal.maxExecutionTime": 30000
}
```

## 7. Apply Code Blocks

### نحوه کار
1. Agent کد پیشنهاد می‌دهد
2. شما می‌توانید:
   - **Accept All**: پذیرش همه تغییرات
   - **Review**: بررسی خط به خط
   - **Reject**: رد تغییرات

### Diff View
```diff
- const oldFunction = () => {
-   return "old";
- }
+ const newFunction = () => {
+   return "improved";
+ }
```

## 8. ویژگی‌های پیشرفته

### Cursor Indexing
- تحلیل semantic کدبیس
- جستجوی سریع symbols
- پیشنهادات context-aware

### Smart Rename
```bash
# Rename variable across entire codebase
F2 on variable → Enter new name → Enter
```

### Git Integration
```bash
# AI-powered commit messages
Git panel → Stage changes → "Generate commit message"
```

### Debugging Assistant
```bash
# When error occurs:
"Debug this error: [paste error]"
# Cursor analyzes stack trace and suggests fixes
```

## بهترین شیوه‌ها

### 1. برای Tab
- صبور باشید، اجازه دهید مدل فکر کند
- از context کافی استفاده کنید
- الگوهای consistent داشته باشید

### 2. برای Agent
- دستورات واضح و مشخص بدهید
- تغییرات بزرگ را به مراحل کوچک تقسیم کنید
- همیشه checkpoint داشته باشید

### 3. برای Chat
- از mentions استفاده کنید
- سوالات را specific بپرسید
- context مرتبط اضافه کنید

## محدودیت‌ها

1. **حجم Context**: حداکثر 32K توکن
2. **اندازه فایل**: فایل‌های بزرگتر از 1MB ممکن است کند شوند
3. **Rate Limits**: محدودیت در تعداد درخواست‌ها
4. **زبان‌های پشتیبانی شده**: بهترین عملکرد در زبان‌های محبوب