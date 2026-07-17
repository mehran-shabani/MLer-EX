# راهنمای سریع Cursor

## میانبرهای ضروری

### AI Features
- **Tab**: تکمیل خودکار
- **Ctrl+K**: ویرایش درون‌خطی
- **Ctrl+I**: دستیار Agent
- **Ctrl+L**: باز کردن Chat

### ویرایش
- **Ctrl+D**: انتخاب کلمه بعدی
- **Alt+Click**: Multi-cursor
- **Ctrl+/**: Comment/Uncomment
- **Alt+Shift+F**: Format document

## دستورات مفید Chat

### Context اضافه کردن
```bash
@file:filename.js      # فایل خاص
@folder:src           # پوشه کامل
@workspace           # کل پروژه
@web                # جستجوی وب
@docs:react         # مستندات رسمی
```

### نمونه دستورات
```bash
# توضیح کد
"Explain this function in simple terms"

# رفع باگ
"Debug this error: [paste error message]"

# بهینه‌سازی
"Optimize this code for performance"

# تست نویسی
"Write comprehensive tests for this component"

# مستندسازی
"Add JSDoc comments to all functions"
```

## قوانین پروژه (.cursorrules)

### نمونه قوانین عمومی
```markdown
# Code Style
- Use 2 spaces for indentation
- Prefer const over let
- Use async/await over promises
- Add error handling to all async functions

# Naming
- Components: PascalCase
- Functions: camelCase
- Constants: UPPER_SNAKE_CASE
- Files: kebab-case

# Best Practices
- Keep functions under 20 lines
- Write self-documenting code
- Add tests for new features
- Comment complex logic only
```

## رفع مشکلات رایج

### AI کار نمی‌کند
1. اتصال اینترنت را چک کنید
2. مطمئن شوید login کرده‌اید
3. Reload window: `Ctrl+Shift+P` → "Reload Window"

### کندی عملکرد
1. تعداد فایل‌های باز را کم کنید
2. Extensions غیرضروری را disable کنید
3. از مدل‌های سبک‌تر استفاده کنید

### Context overflow
1. فایل‌های غیرمرتبط را exclude کنید
2. از symbol-based context استفاده کنید
3. Context window را کاهش دهید

## نکات طلایی

### 1. Context Management
- فقط فایل‌های مرتبط را include کنید
- از @symbol برای دقت بیشتر استفاده کنید
- در پروژه‌های بزرگ selective باشید

### 2. Prompt Writing
- دستورات clear و specific بدهید
- مثال‌های مورد انتظار را ذکر کنید
- خروجی مورد نظر را مشخص کنید

### 3. Workflow
- برای تغییرات بزرگ checkpoint بگیرید
- از Agent برای multi-file changes استفاده کنید
- Chat tabs را برای موضوعات مختلف جدا کنید

### 4. Performance
- Indexing را برای فایل‌های بزرگ غیرفعال کنید
- Cache را دوره‌ای clear کنید
- از prefetch برای تکمیل سریع‌تر استفاده کنید

## منابع مفید

- **Forum**: [forum.cursor.com](https://forum.cursor.com)
- **Discord**: [discord.gg/cursor](https://discord.gg/cursor)
- **GitHub**: [github.com/cursor](https://github.com/cursor)
- **Twitter**: [@cursor_ai](https://twitter.com/cursor_ai)

## چک‌لیست شروع

- [ ] Cursor را نصب کنید
- [ ] تنظیمات VS Code را import کنید
- [ ] دوره آزمایشی Pro را فعال کنید
- [ ] میانبرهای اصلی را یاد بگیرید
- [ ] اولین `.cursorrules` را بسازید
- [ ] با Tab و Ctrl+K شروع کنید
- [ ] Agent را برای تغییرات بزرگ امتحان کنید
- [ ] از Chat برای سوالات استفاده کنید