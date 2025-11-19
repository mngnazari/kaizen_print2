# 📚 مستندات کامل ربات کایزن پرینت

> این فایل شامل ساختار کامل، کلاس‌ها، توابع و محاوره‌های ربات می‌باشد.
> برای توسعه‌های آینده، این فایل را به هوش مصنوعی ارسال کنید تا با دریافت نام تابع یا کلاس، متن کامل آن را از شما بخواهد.

---

## 🏗️ ساختار کلی پروژه

```
kaizen_print2/
├── main.py                          # فایل اصلی اجرای ربات
├── database/                        # پایگاه داده
│   ├── connection.py               # اتصال به دیتابیس
│   ├── models.py                   # مدل‌های اصلی
│   ├── editor_models.py            # مدل‌های ادیتور
│   ├── broadcast_models.py         # مدل‌های پخش پیام
│   ├── crud.py                     # عملیات CRUD اصلی
│   ├── editor_crud.py              # عملیات CRUD ادیتور
│   ├── group_crud.py               # عملیات CRUD گروهی
│   ├── broadcast_crud.py           # عملیات CRUD پخش پیام
│   └── schemas.py                  # اسکیماها
├── handlers/                        # هندلرهای ربات
│   ├── start.py                    # شروع و ثبت‌نام
│   ├── registration.py             # ثبت‌نام کاربران
│   ├── admin.py                    # عملیات ادمین
│   ├── admin_inline.py             # منوی اینلاین ادمین
│   ├── admin_settings.py           # تنظیمات ادمین
│   ├── customer.py                 # عملیات مشتری
│   ├── customer_management.py      # مدیریت مشتریان
│   ├── operator.py                 # عملیات اپراتور
│   ├── operator_files.py           # فایل‌های اپراتور
│   ├── editor.py                   # عملیات ادیتور
│   ├── editor_file_handler.py      # آپلود فایل ادیتور
│   ├── editor_status.py            # وضعیت ادیتور
│   ├── file_submission.py          # ارسال فایل مشتری
│   ├── file_archive.py             # آرشیو فایل‌ها
│   ├── delivery_scheduler.py       # زمان‌بندی تحویل
│   ├── receipt_submission.py       # ارسال رسید
│   ├── receipt_management.py       # مدیریت رسیدها
│   ├── vault_management.py         # مدیریت صندوق‌ها
│   ├── wallet_invoice.py           # کیف پول و فاکتور
│   ├── transaction_viewer.py       # نمایش تراکنش‌ها
│   ├── independent_income.py       # درآمد مستقل
│   ├── broadcast_handler.py        # پخش پیام گروهی
│   ├── holiday_manager.py          # مدیریت تعطیلات
│   ├── group_admin.py              # مدیریت گروه
│   ├── group_callbacks.py          # کالبک‌های گروهی
│   ├── group_stats.py              # آمار گروهی
│   └── test_handler.py             # تست
├── keyboards/                       # کیبوردها
│   ├── admin.py
│   ├── admin_settings.py
│   ├── customer.py
│   ├── customer_management.py
│   ├── operator.py
│   ├── operator_files.py
│   ├── editor.py
│   ├── editor_status.py
│   ├── file_keyboards.py
│   ├── receipt_management.py
│   ├── vault_management.py
│   ├── vault_menu.py
│   ├── broadcast_keyboards.py
│   └── group_stats.py
└── utils/                           # ابزارها
    ├── discount_calculator.py      # محاسبه تخفیف
    ├── delivery_grouping.py        # گروه‌بندی تحویل
    ├── editor_state_manager.py     # مدیریت وضعیت ادیتور
    ├── file_naming.py              # نام‌گذاری فایل
    ├── factor.py                   # فاکتور
    ├── group_analytics.py          # تحلیل گروهی
    ├── group_settings.py           # تنظیمات گروهی
    ├── group_config.py             # پیکربندی گروه
    ├── notification_scheduler.py   # زمان‌بندی اعلان
    ├── mapping_parser.py           # تجزیه فایل نقشه
    ├── stl_renderer.py             # رندر فایل STL
    ├── referral.py                 # سیستم دعوت
    └── broadcast_utils.py          # ابزار پخش پیام
```

---

## 🗄️ پایگاه داده - مدل‌ها (Database Models)

### **1. database/models.py - مدل‌های اصلی**

#### کلاس User
```python
class User(Base):
    __tablename__ = "users"
```
**فیلدها:**
- `id` (BigInteger): شناسه تلگرام کاربر
- `full_name` (String): نام کامل
- `phone_number` (String): شماره تلفن (یکتا)
- `customer_code` (String): کد مشتری (مثل c1, c2, ...)
- `referral_code` (String): کد معرف
- `referrer_id` (BigInteger): شناسه معرف
- `referrals_count` (Integer): تعداد افراد دعوت شده
- `max_referrals` (Integer): حداکثر تعداد دعوت (پیش‌فرض: 3)
- `discount_credit` (Float): اعتبار تخفیف
- `wallet_balance` (Float): موجودی کیف پول
- `role` (String): نقش (admin, operator, editor, visitor, customer)
- `notification_status` (String): وضعیت دریافت پیام (all, promotional_only, none)
- `print_price_per_gram` (Float): قیمت پرینت اختصاصی مشتری
- `created_at` (DateTime): تاریخ ایجاد

**روابط (Relationships):**
- `referrer`: معرف
- `referred_users`: افراد دعوت شده
- `file_orders`: سفارشات فایل
- `invoices`: فاکتورها
- `wallet_transactions`: تراکنش‌های کیف پول

---

#### کلاس ReferralCode
```python
class ReferralCode(Base):
    __tablename__ = "referral_codes"
```
**فیلدها:**
- `id` (Integer): شناسه
- `creator_id` (BigInteger): شناسه سازنده
- `creator_role` (String): نقش سازنده
- `referral_code` (String): کد معرف (یکتا)
- `is_active` (Boolean): فعال/غیرفعال
- `created_at` (DateTime): تاریخ ایجاد

---

#### کلاس FileOrder
```python
class FileOrder(Base):
    __tablename__ = "file_orders"
```
**فیلدها:**
- `id` (Integer): شناسه سفارش
- `user_id` (BigInteger): شناسه کاربر
- `username` (String): نام کاربری
- `file_id` (String): شناسه فایل تلگرام
- `file_name` (String): نام فایل
- `file_size` (Integer): حجم فایل
- `edit_deadline` (DateTime): مهلت ویرایش
- `print_count` (Integer): تعداد پرینت (پیش‌فرض: 1)
- `description` (Text): توضیحات
- `status` (String): وضعیت (pending, confirmed, cancelled, invoiced)
- `message_id` (Integer): شناسه پیام
- `delivery_datetime` (DateTime): زمان تحویل
- `created_at` (DateTime): تاریخ ایجاد
- `confirmed_at` (DateTime): تاریخ تایید
- `has_invoice` (Boolean): آیا فاکتور صادر شده؟
- `sent_to_operator` (Boolean): آیا به اپراتور ارسال شده؟
- `operator_filename` (String): نام فایل تغییریافته
- `sent_to_operator_at` (DateTime): زمان ارسال به اپراتور
- `assigned_editor_id` (BigInteger): شناسه ادیتور مسئول
- `editor_status` (String): وضعیت ادیتور (pending, assigned, approved, rejected)
- `editor_assigned_at` (DateTime): زمان تخصیص به ادیتور
- `editor_notes` (Text): یادداشت ادیتور

---

#### کلاس Invoice
```python
class Invoice(Base):
    __tablename__ = "invoices"
```
**فیلدها:**
- `id` (Integer): شناسه فاکتور
- `customer_id` (BigInteger): شناسه مشتری
- `operator_id` (BigInteger): شناسه اپراتور
- `file_order_id` (Integer): شناسه سفارش فایل
- `weight_grams` (Float): وزن به گرم
- `price_per_gram` (Float): قیمت هر گرم
- `total_amount` (Float): مبلغ کل
- `photo_file_id` (String): شناسه عکس پرینت
- `created_at` (DateTime): تاریخ ایجاد

---

#### کلاس WalletTransaction
```python
class WalletTransaction(Base):
    __tablename__ = "wallet_transactions"
```
**فیلدها:**
- `id` (Integer): شناسه تراکنش
- `user_id` (BigInteger): شناسه کاربر
- `admin_id` (BigInteger): شناسه ادمین
- `amount` (Float): مبلغ (مثبت = شارژ، منفی = برداشت)
- `transaction_type` (String): نوع تراکنش (referral_bonus, invoice_payment, admin_adjustment)
- `description` (Text): توضیحات
- `created_at` (DateTime): تاریخ ایجاد

---

#### کلاس SystemSettings
```python
class SystemSettings(Base):
    __tablename__ = "system_settings"
```
**فیلدها:**
- `id` (Integer): شناسه
- `setting_key` (String): کلید تنظیم (price_per_gram, last_customer_number, ...)
- `setting_value` (String): مقدار تنظیم
- `description` (Text): توضیحات
- `updated_at` (DateTime): تاریخ بروزرسانی

---

#### کلاس Holiday
```python
class Holiday(Base):
    __tablename__ = "holidays"
```
**فیلدها:**
- `id` (Integer): شناسه
- `date` (String): تاریخ (فرمت YYYY-MM-DD)
- `is_holiday` (Boolean): تعطیل/کاری
- `created_at` (DateTime): تاریخ ایجاد
- `updated_at` (DateTime): تاریخ بروزرسانی

---

#### کلاس DeliverySchedule
```python
class DeliverySchedule(Base):
    __tablename__ = "delivery_schedules"
```
**فیلدها:**
- `id` (Integer): شناسه
- `delivery_number` (Integer): شماره تحویل در روز (1, 2, 3)
- `cutoff_time` (String): ساعت مرجع (فرمت HH:MM)
- `cutoff_day_offset` (Integer): فاصله روز (0=همان روز، -1=روز قبل)
- `delivery_offset_hours` (Integer): تعداد ساعت تا تحویل
- `edit_deadline_hours` (Integer): مهلت ویرایش (ساعت)
- `is_active` (Boolean): فعال/غیرفعال
- `created_at` (DateTime): تاریخ ایجاد
- `updated_at` (DateTime): تاریخ بروزرسانی

---

#### کلاس Receipt
```python
class Receipt(Base):
    __tablename__ = "receipts"
```
**فیلدها:**
- `id` (Integer): شناسه رسید
- `user_id` (BigInteger): شناسه کاربر
- `file_id` (String): شناسه فایل عکس رسید
- `file_name` (String): نام فایل
- `file_size` (Integer): حجم فایل
- `description` (Text): توضیحات کاربر
- `status` (String): وضعیت (pending, approved, rejected)
- `admin_notes` (Text): یادداشت ادمین
- `created_at` (DateTime): تاریخ ایجاد
- `reviewed_at` (DateTime): تاریخ بررسی
- `reviewed_by` (BigInteger): شناسه بررسی‌کننده

---

#### کلاس Vault
```python
class Vault(Base):
    __tablename__ = "vaults"
```
**فیلدها:**
- `id` (Integer): شناسه صندوق
- `name` (String): نام صندوق
- `balance` (Float): موجودی نقد
- `gold_balance` (Float): موجودی طلا (گرم)
- `allocation_percentage` (Float): درصد تخصیص
- `is_active` (Boolean): فعال/غیرفعال
- `created_at` (DateTime): تاریخ ایجاد
- `updated_at` (DateTime): تاریخ بروزرسانی

**روابط:**
- `transactions`: تراکنش‌های نقدی
- `gold_transactions`: معاملات طلا

---

#### کلاس VaultTransaction
```python
class VaultTransaction(Base):
    __tablename__ = "vault_transactions"
```
**فیلدها:**
- `id` (Integer): شناسه تراکنش
- `vault_id` (Integer): شناسه صندوق
- `amount` (Float): مبلغ (مثبت = واریز، منفی = برداشت)
- `transaction_type` (String): نوع (auto_allocation, manual_expense, adjustment)
- `description` (Text): توضیحات
- `receipt_id` (Integer): شناسه رسید مرتبط
- `created_by` (BigInteger): شناسه ثبت‌کننده
- `created_at` (DateTime): تاریخ ایجاد

---

#### کلاس VaultGoldTransaction
```python
class VaultGoldTransaction(Base):
    __tablename__ = "vault_gold_transactions"
```
**فیلدها:**
- `id` (Integer): شناسه معامله
- `vault_id` (Integer): شناسه صندوق
- `transaction_type` (String): نوع معامله (buy, sell)
- `gold_weight_grams` (Float): وزن طلا (گرم)
- `price_per_gram_dollar` (Float): قیمت هر گرم (دلار)
- `price_per_gram_toman` (Float): قیمت هر گرم (تومان)
- `total_amount` (Float): مبلغ کل معامله
- `description` (Text): توضیحات
- `created_by` (BigInteger): شناسه ثبت‌کننده
- `created_at` (DateTime): تاریخ ایجاد

---

### **2. database/editor_models.py - مدل‌های ادیتور**

#### کلاس EditorWorkSession
```python
class EditorWorkSession(Base):
    __tablename__ = "editor_work_sessions"
```
**فیلدها:**
- `id` (Integer): شناسه جلسه
- `editor_id` (BigInteger): شناسه ادیتور
- `status` (String): وضعیت (idle, working, completed)
- `original_file_count` (Integer): تعداد فایل‌های اصلی
- `created_at` (DateTime): تاریخ ایجاد
- `mapping_received_at` (DateTime): زمان دریافت نقشه
- `completed_at` (DateTime): زمان تکمیل

**روابط:**
- `editor`: ادیتور
- `original_files`: فایل‌های اصلی
- `file_mapping`: نقشه تبدیل

---

#### کلاس EditorOriginalFile
```python
class EditorOriginalFile(Base):
    __tablename__ = "editor_original_files"
```
**فیلدها:**
- `id` (Integer): شناسه
- `session_id` (Integer): شناسه جلسه
- `file_order_id` (Integer): شناسه سفارش
- `original_filename` (String): نام فایل اصلی
- `file_id` (String): شناسه فایل تلگرام
- `assigned_at` (DateTime): زمان تخصیص

**روابط:**
- `session`: جلسه کاری
- `file_order`: سفارش فایل
- `processed_files`: فایل‌های پردازش شده

---

#### کلاس EditorFileMapping
```python
class EditorFileMapping(Base):
    __tablename__ = "editor_file_mappings"
```
**فیلدها:**
- `id` (Integer): شناسه
- `session_id` (Integer): شناسه جلسه
- `mapping_content` (Text): محتوای نقشه
- `created_at` (DateTime): تاریخ ایجاد

---

#### کلاس ProcessedFile
```python
class ProcessedFile(Base):
    __tablename__ = "processed_files"
```
**فیلدها:**
- `id` (Integer): شناسه
- `original_file_id` (Integer): شناسه فایل اصلی
- `original_filename` (String): نام فایل اصلی (مبدا)
- `processed_filename` (String): نام فایل پردازش شده (مقصد)
- `stl_required` (Boolean): آیا STL مورد نیاز است؟
- `jpg_required` (Boolean): آیا JPG مورد نیاز است؟
- `zip_required` (Boolean): آیا ZIP مورد نیاز است؟
- `stl_file_id` (String): شناسه فایل STL
- `jpg_file_id` (String): شناسه فایل JPG
- `zip_file_id` (String): شناسه فایل ZIP
- `stl_received` (Boolean): STL دریافت شده؟
- `jpg_received` (Boolean): JPG دریافت شده؟
- `zip_received` (Boolean): ZIP دریافت شده؟
- `is_completed` (Boolean): تمام فایل‌ها دریافت شده؟
- `created_at` (DateTime): تاریخ ایجاد
- `stl_received_at` (DateTime): زمان دریافت STL
- `jpg_received_at` (DateTime): زمان دریافت JPG
- `zip_received_at` (DateTime): زمان دریافت ZIP

---

### **3. database/broadcast_models.py - مدل‌های پخش پیام**

#### کلاس BroadcastMessage
```python
class BroadcastMessage(Base):
    __tablename__ = "broadcast_messages"
```
**فیلدها:**
- `id` (Integer): شناسه پیام
- `message_type` (String): نوع (promotional, occasional)
- `content` (Text): محتوای پیام
- `media_file_id` (String): شناسه فایل رسانه
- `media_type` (String): نوع رسانه (photo, video, document)
- `created_by` (BigInteger): شناسه ایجادکننده
- `created_at` (DateTime): تاریخ ایجاد
- `start_datetime` (DateTime): شروع فعالیت (برای occasional)
- `end_datetime` (DateTime): پایان فعالیت (برای occasional)
- `total_recipients` (Integer): تعداد کل دریافت‌کنندگان
- `successful_sends` (Integer): تعداد ارسال موفق
- `failed_sends` (Integer): تعداد ارسال ناموفق

**روابط:**
- `creator`: سازنده پیام
- `logs`: لاگ‌های ارسال

---

#### کلاس BroadcastLog
```python
class BroadcastLog(Base):
    __tablename__ = "broadcast_logs"
```
**فیلدها:**
- `id` (Integer): شناسه لاگ
- `broadcast_message_id` (Integer): شناسه پیام پخش
- `customer_id` (BigInteger): شناسه مشتری
- `sent_at` (DateTime): زمان ارسال
- `status` (String): وضعیت (success, failed)
- `telegram_message_id` (Integer): شناسه پیام تلگرام
- `error_message` (Text): پیام خطا

**روابط:**
- `broadcast_message`: پیام پخش
- `customer`: مشتری

---

## 📡 هندلرها (Handlers)

### **handlers/start.py**
**ثابت‌ها:**
- `OPERATORS_IDS`: لیست شناسه‌های اپراتورها

**توابع:**
- `start_command(update, context)`: دستور /start - بررسی ثبت‌نام و نمایش منوی مناسب

---

### **handlers/registration.py**
**وضعیت‌های ConversationHandler:**
- `GET_PHONE_NUMBER`: دریافت شماره تلفن
- `GET_FULL_NAME`: دریافت نام کامل

**توابع:**
- `start_registration(update, context)`: شروع فرآیند ثبت‌نام
- `get_full_name(update, context)`: دریافت شماره تلفن و درخواست نام
- `save_user_info(update, context)`: ذخیره اطلاعات کاربر و تکمیل ثبت‌نام

---

### **handlers/admin.py**
**وضعیت‌های ConversationHandler:**
- `GET_USER_ID_TO_UPDATE`: دریافت شناسه کاربر برای بروزرسانی
- `GET_NEW_REFERRAL_LIMIT`: دریافت سقف جدید دعوت
- `GET_CUSTOMER_ID`: دریافت شناسه مشتری
- `GET_PRINT_PRICE`: دریافت قیمت پرینت
- `GET_WALLET_AMOUNT`: دریافت مبلغ کیف پول
- `GET_TRANSACTION_DESCRIPTION`: دریافت توضیحات تراکنش

**توابع:**
- `get_bot_username(context)`: دریافت نام کاربری ربات
- `generate_referral_code_handler(update, context)`: ایجاد کد معرف توسط ادمین
- `view_users_handler(update, context)`: نمایش لیست کاربران
- `show_referral_tree_handler(update, context)`: نمایش درخت معرف
- `set_referral_limit_start(update, context)`: شروع تنظیم سقف دعوت
- `get_user_id_to_update_handler(update, context)`: دریافت شناسه کاربر
- `get_new_referral_limit_handler(update, context)`: دریافت و ذخیره سقف جدید
- `cancel_handler(update, context)`: لغو محاوره
- `admin_stats_handler(update, context)`: نمایش آمار ادمین
- `wallet_management_handler(update, context)`: مدیریت کیف پول
- `handle_customer_selection(update, context)`: انتخاب مشتری از کیبورد
- `handle_wallet_operations(update, context)`: عملیات کیف پول
- `get_print_price_handler(update, context)`: دریافت قیمت پرینت جدید
- `get_wallet_amount_handler(update, context)`: دریافت مبلغ شارژ/برداشت
- `get_transaction_description_handler(update, context)`: دریافت توضیحات و اجرای تراکنش
- `show_group_settings(update, context)`: نمایش تنظیمات گروهی
- `handle_admin_settings_callbacks(update, context)`: مدیریت کالبک‌های تنظیمات

---

### **handlers/admin_inline.py**
**توابع:**
- `show_admin_main_menu(update, context)`: نمایش منوی اصلی ادمین (اینلاین)
- `handle_admin_callbacks(update, context)`: مدیریت کالبک‌های ادمین (لینک دعوت، لیست کاربران، درخت معرف، مدیریت مشتریان، رسیدها، تنظیمات، آمار)

---

### **handlers/admin_settings.py**
**وضعیت‌های ConversationHandler:**
- `WAITING_EDITOR_DELAY = 100`: انتظار برای تاخیر ادیتور

**توابع:**
- `show_admin_settings(update, context)`: نمایش منوی تنظیمات ادمین
- `show_settings_statistics(update, context)`: نمایش آمار تنظیمات سیستم
- `handle_settings_callback(update, context)`: مدیریت کالبک‌های تنظیمات
- `handle_delivery_start(update, context)`: شروع محاوره تنظیم تحویل
- `get_delivery_schedule_conversation()`: ایجاد ConversationHandler برای زمان‌بندی تحویل
- `show_editor_delay_menu(query_or_update, context)`: نمایش منوی تاخیر ادیتور
- `start_editor_delay_setting(update, context)`: شروع تنظیم تاخیر دسترسی ادیتور
- `save_editor_delay(update, context)`: ذخیره مقدار جدید تاخیر
- `cancel_editor_delay_setting(update, context)`: لغو تنظیم تاخیر
- `show_editor_delay_menu_inline(query, context)`: نمایش منوی تاخیر (نسخه اینلاین)
- `cmd_set_delay(update, context)`: دستور مستقیم تنظیم تاخیر: /setdelay

---

### **handlers/broadcast_handler.py**
**وضعیت‌های ConversationHandler:**
- `BROADCAST_SELECT_TYPE`: انتخاب نوع پیام
- `BROADCAST_GET_CONTENT`: دریافت محتوای پیام
- `BROADCAST_GET_START_DATE`: دریافت تاریخ شروع
- `BROADCAST_GET_START_TIME`: دریافت ساعت شروع
- `BROADCAST_GET_END_DATE`: دریافت تاریخ پایان
- `BROADCAST_GET_END_TIME`: دریافت ساعت پایان
- `BROADCAST_CONFIRM`: تایید و ارسال

**توابع:**
- `broadcast_start(update, context)`: شروع فرآیند پخش پیام
- `select_broadcast_type(update, context)`: انتخاب نوع پیام (تبلیغاتی/موقت)
- `receive_broadcast_content(update, context)`: دریافت محتوای پیام از ادمین
- `get_discount_start_date(update, context)`: دریافت تاریخ شروع تخفیف
- `get_discount_start_time(update, context)`: دریافت ساعت شروع تخفیف
- `get_discount_end_date(update, context)`: دریافت تاریخ پایان تخفیف
- `get_discount_end_time(update, context)`: دریافت ساعت پایان تخفیف
- `show_broadcast_preview(update, context)`: پیش‌نمایش پیام قبل از ارسال
- `confirm_and_send_broadcast(update, context)`: تایید و ارسال به همه مشتریان
- `edit_broadcast_message(update, context)`: ویرایش پیام (شروع دوباره)
- `show_broadcast_statistics(update, context)`: نمایش آمار کلی پخش پیام
- `show_broadcast_history(update, context)`: نمایش تاریخچه پیام‌های ارسالی
- `show_broadcast_detail(update, context)`: نمایش جزئیات یک پیام
- `show_broadcast_logs(update, context)`: نمایش لاگ‌های ارسال
- `cancel_broadcast(update, context)`: لغو فرآیند پخش
- `get_broadcast_conversation_handler()`: برگرداندن ConversationHandler سیستم پخش

---

### **handlers/customer_management.py**
**وضعیت‌های ConversationHandler:**
- `CUSTOMER_WALLET_AMOUNT`: دریافت مبلغ کیف پول
- `CUSTOMER_WALLET_DESC`: دریافت توضیحات کیف پول
- `CUSTOMER_DISCOUNT_AMOUNT`: دریافت مبلغ تخفیف
- `CUSTOMER_DISCOUNT_DESC`: دریافت توضیحات تخفیف
- `CUSTOMER_PRINT_PRICE`: دریافت قیمت پرینت

**توابع:**
- `show_customers_list(update, context)`: نمایش لیست مشتریان برای مدیریت
- `handle_customer_selection_callback(update, context)`: مدیریت انتخاب مشتری
- `handle_toggle_notification_status(update, context)`: تغییر وضعیت دریافت اعلان مشتری
- `handle_customer_operations_callback(update, context)`: مدیریت عملیات روی مشتریان
- `get_customer_wallet_amount(update, context)`: دریافت مبلغ شارژ/برداشت کیف پول
- `get_customer_wallet_description(update, context)`: دریافت توضیحات و اجرای تراکنش کیف پول
- `get_customer_discount_amount(update, context)`: دریافت مبلغ اعتبار تخفیف
- `get_customer_discount_description(update, context)`: دریافت توضیحات و اجرای تراکنش تخفیف
- `get_customer_print_price(update, context)`: دریافت قیمت پرینت جدید برای مشتری
- `cancel_customer_management(update, context)`: لغو فرآیند مدیریت مشتری

---

### **handlers/customer.py**
**توابع:**
- `show_customer_main_menu(update, context)`: نمایش منوی اصلی مشتری (اینلاین)
- `handle_customer_inline_callbacks(update, context)`: مدیریت کالبک‌های اینلاین مشتری (وضعیت اعتبار، آرشیو، زیرمجموعه‌ها، آمار گروهی، کیف پول، دعوت)
- `get_bot_username(context)`: دریافت نام کاربری ربات
- `handle_customer_menu(update, context)`: مدیریت دکمه‌های منوی مشتری
- `generate_user_referral_code_handler(update, context)`: ایجاد کد معرف برای مشتری
- `show_customer_invoices(update, context)`: نمایش فاکتورهای مشتری
- `show_invoice_by_index(message, context, index, invoice)`: نمایش فاکتور با شماره مشخص
- `handle_invoice_navigation(update, context)`: مدیریت ناوبری بین فاکتورها
- `show_referrals_handler(update, context)`: نمایش زیرمجموعه‌های مشتری

---

### **handlers/editor.py**
**ثابت‌ها:**
- `EDITORS_IDS`: لیست شناسه‌های ادیتورها

**توابع:**
- `get_shamsi_time_display(file_order)`: تبدیل زمان به شمسی برای نمایش
- `handle_editor_menu(update, context)`: مدیریت منوی ادیتور
- `handle_editor_file_upload(update, context)`: مدیریت آپلود فایل توسط ادیتور
- `handle_mapping_file(update, context, state_manager, db)`: مدیریت فایل نقشه
- `handle_processed_file(update, context, state_manager, db)`: مدیریت فایل‌های پردازش شده (STL, JPG, ZIP)
- `show_editor_main_menu(update, context)`: نمایش منوی اصلی ادیتور
- `show_editor_pending_files(update, context)`: نمایش فایل‌های در انتظار ویرایش (با فیلتر دسترسی)
- `show_editor_delivery_customers(update, context)`: نمایش مشتریان برای زمان تحویل خاص
- `send_files_to_editor(update, context)`: ارسال فایل‌ها به ادیتور
- `send_all_files_for_delivery(query, edit_deadline_time, editor_id)`: ارسال همه فایل‌ها برای یک زمان تحویل
- `send_customer_files_for_delivery(query, customer_code, edit_deadline_time, editor_id)`: ارسال فایل‌های مشتری خاص
- `escape_markdown(text)`: Escape کردن کاراکترهای ویژه Markdown
- `show_remaining_files(update, context)`: نمایش فایل‌های باقیمانده ادیتور
- `handle_editor_callbacks(update, context)`: مدیریت کالبک‌های ادیتور

---

### **handlers/delivery_scheduler.py**
**وضعیت‌های ConversationHandler:**
- `GET_DELIVERY_COUNT`: دریافت تعداد تحویل‌های روزانه
- `GET_CUTOFF_TIME`: دریافت ساعت مرجع
- `GET_OFFSET_HOURS`: دریافت فاصله ساعتی تا تحویل
- `GET_EDIT_DEADLINE`: دریافت مهلت ویرایش

**ثابت‌ها:**
- `DEFAULT_PRINT_COUNT = 1`
- `DEFAULT_REFERENCE_TIME = 17`
- `DEFAULT_DELIVERY_OFFSET_HOURS = 20`
- `DEFAULT_EDIT_DEADLINE_OFFSET_HOURS = 2`

**توابع:**
- `calculate_file_times()`: محاسبه edit_deadline و delivery_time بر اساس تنظیمات
- `start_delivery_setup(update, context)`: شروع تنظیم زمان تحویل
- `get_delivery_count(update, context)`: دریافت تعداد تحویل‌های روزانه
- `ask_cutoff_time(update, context)`: درخواست ساعت مرجع
- `get_cutoff_time(update, context)`: دریافت و پردازش ساعت مرجع
- `get_offset_hours(update, context)`: دریافت فاصله ساعتی تا تحویل
- `get_edit_deadline(update, context)`: دریافت مهلت مجاز ویرایش
- `save_delivery_schedules(update, context)`: ذخیره تنظیمات در دیتابیس
- `calculate_sample_delivery_time(cutoff_time, day_offset, offset_hours)`: محاسبه زمان تحویل نمونه
- `cancel_delivery_setup(update, context)`: لغو تنظیم زمان تحویل
- `calculate_delivery_time(order_datetime)`: محاسبه زمان تحویل بر اساس زمان سفارش

---

### **handlers/file_submission.py**
**توابع:**
- `handle_file(update, context)`: مدیریت ارسال فایل توسط مشتری
- `handle_callback_query(update, context)`: مدیریت کالبک تایید/لغو سفارش
- `handle_reply(update, context)`: مدیریت پاسخ به پیام (برای ثبت توضیحات)

---

### **handlers/file_archive.py**
**توابع:**
- `gregorian_to_jalali(gregorian_date)`: تبدیل میلادی به شمسی
- `gregorian_to_jalali_with_time(gregorian_datetime)`: تبدیل میلادی به شمسی با ساعت
- `get_file_archive_keyboard(weekly_count, monthly_count, total_count)`: ایجاد کیبورد آرشیو
- `show_file_archive_menu(update, context)`: نمایش منوی آرشیو فایل‌ها
- `send_file_archive_version(context, user_id, file_order, file_number)`: ارسال فایل با کپشن آرشیو
- `handle_archive_callback_unique(update, context)`: مدیریت کالبک‌های آرشیو

---

### **handlers/holiday_manager.py**
**توابع:**
- `show_holiday_calendar(update, context)`: نمایش تقویم تعطیلات
- `toggle_holiday(update, context)`: تغییر وضعیت روز (تعطیل/کاری)

---

### **handlers/group_admin.py**
**توابع:**
- `show_group_settings_menu(update, context)`: نمایش منوی تنظیمات گروهی
- `show_group_statistics(update, context)`: نمایش آمار گروهی
- `reset_group_settings(update, context)`: بازنشانی تنظیمات گروه
- `get_silver_settings_conversation()`: دریافت ConversationHandler تنظیمات نقره‌ای
- `get_gold_settings_conversation()`: دریافت ConversationHandler تنظیمات طلایی

---

### **handlers/group_callbacks.py**
**توابع:**
- `handle_group_stats_callbacks(update, context)`: مدیریت کالبک‌های آمار گروهی

---

### **handlers/operator.py**
**وضعیت‌های ConversationHandler:**
- `GET_WEIGHT`: دریافت وزن
- `GET_PHOTO`: دریافت عکس

**توابع:**
- `handle_operator_menu(update, context)`: مدیریت منوی اپراتور
- `show_operator_main_menu(update, context)`: نمایش منوی اصلی اپراتور
- `show_operator_invoice_menu(update, context)`: نمایش منوی صدور فاکتور
- `handle_operator_callbacks(update, context)`: مدیریت کالبک‌های اپراتور
- `show_customers_for_invoice(update, context)`: نمایش مشتریان برای صدور فاکتور
- `handle_customer_file_selection(update, context)`: مدیریت انتخاب فایل مشتری
- `get_weight_handler(update, context)`: دریافت وزن
- `get_photo_handler(update, context)`: دریافت عکس و صدور فاکتور
- `cancel_invoice_handler(update, context)`: لغو صدور فاکتور
- `show_operator_stats(update, context)`: نمایش آمار اپراتور
- `show_customers_list(update, context)`: نمایش لیست مشتریان
- `show_operator_settings(update, context)`: نمایش تنظیمات اپراتور

---

### **handlers/operator_files.py**
**توابع:**
- `operator_files_command(update, context)`: دستور /files برای اپراتور
- `show_operator_files_main(update, context)`: نمایش منوی اصلی فایل‌های اپراتور
- `show_delivery_customers(update, context)`: نمایش مشتریان برای تحویل
- `send_customer_files(update, context)`: ارسال فایل‌های یک مشتری
- `send_all_delivery_files(update, context)`: ارسال همه فایل‌های تحویل
- `handle_no_files(update, context)`: مدیریت عدم وجود فایل

---

### **handlers/receipt_management.py**
**وضعیت‌های ConversationHandler:**
- `RECEIPT_CHARGE_AMOUNT`: دریافت مبلغ شارژ
- `RECEIPT_CHARGE_DESC`: دریافت توضیحات شارژ

**توابع:**
- `start_charge_from_receipt(update, context)`: شروع شارژ از رسید
- `get_receipt_charge_amount(update, context)`: دریافت مبلغ شارژ
- `get_receipt_charge_description(update, context)`: دریافت توضیحات و ثبت شارژ

---

### **handlers/receipt_submission.py**
**وضعیت‌های ConversationHandler:**
- `RECEIPT_UPLOAD`: آپلود رسید
- `RECEIPT_DESCRIPTION`: دریافت توضیحات رسید

**توابع:**
- `start_receipt_submission(update, context)`: شروع ارسال رسید
- `handle_receipt_photo(update, context)`: دریافت عکس رسید
- `handle_end_command(update, context)`: مدیریت دستور /end
- `handle_receipt_description(update, context)`: دریافت توضیحات رسید
- `handle_finish_callback(update, context)`: مدیریت کالبک پایان
- `cancel_receipt_submission(update, context)`: لغو ارسال رسید

---

### **handlers/vault_management.py**
**وضعیت‌های ConversationHandler:**
- `GOLD_BUY_WEIGHT`: دریافت وزن خرید طلا
- `GOLD_BUY_PRICE_TOMAN`: دریافت قیمت تومانی خرید
- `GOLD_BUY_EXCHANGE_RATE`: دریافت نرخ تبدیل خرید
- `GOLD_BUY_DESC`: دریافت توضیحات خرید
- `GOLD_SELL_WEIGHT`: دریافت وزن فروش طلا
- `GOLD_SELL_PRICE_TOMAN`: دریافت قیمت تومانی فروش
- `GOLD_SELL_EXCHANGE_RATE`: دریافت نرخ تبدیل فروش
- `GOLD_SELL_DESC`: دریافت توضیحات فروش
- `VAULT_CREATE_NAME`: دریافت نام صندوق جدید
- `VAULT_CREATE_PERCENT`: دریافت درصد تخصیص
- `VAULT_SET_PERCENT`: تنظیم درصد صندوق
- `VAULT_RENAME`: تغییر نام صندوق
- `EXPENSE_AMOUNT`: دریافت مبلغ هزینه
- `EXPENSE_DESC`: دریافت توضیحات هزینه
- `PETTY_CASH_WITHDRAW_AMOUNT`: دریافت مبلغ برداشت تنخواه
- `PETTY_CASH_WITHDRAW_DESC`: دریافت توضیحات برداشت

**توابع:**
- `show_vaults_list(update, context)`: نمایش لیست صندوق‌ها
- `show_vaults_menu(update, context)`: نمایش منوی صندوق‌ها
- `show_vault_detail(update, context)`: نمایش جزئیات صندوق
- `show_vault_transactions(update, context)`: نمایش تراکنش‌های صندوق
- `start_create_vault(update, context)`: شروع ایجاد صندوق
- `get_vault_name(update, context)`: دریافت نام صندوق
- `get_vault_percent_and_create(update, context)`: دریافت درصد و ایجاد صندوق
- `start_set_percent(update, context)`: شروع تنظیم درصد
- `update_vault_percent(update, context)`: بروزرسانی درصد صندوق
- `start_rename_vault(update, context)`: شروع تغییر نام
- `update_vault_name_handler(update, context)`: بروزرسانی نام صندوق
- `delete_vault_confirm(update, context)`: تایید حذف صندوق
- `delete_vault_execute(update, context)`: اجرای حذف صندوق
- `start_expense_record(update, context)`: شروع ثبت هزینه
- `get_expense_amount(update, context)`: دریافت مبلغ هزینه
- `get_expense_description(update, context)`: دریافت توضیحات هزینه
- `record_expense_final(update, context)`: ثبت نهایی هزینه
- `cancel_vault_operation(update, context)`: لغو عملیات صندوق
- `handle_vault_callbacks(update, context)`: مدیریت کالبک‌های صندوق
- `show_gold_menu(update, context)`: نمایش منوی طلا
- `show_gold_balance(update, context)`: نمایش موجودی طلا
- `start_buy_gold(update, context)`: شروع خرید طلا
- `get_gold_buy_weight(update, context)`: دریافت وزن خرید
- `get_gold_buy_price(update, context)`: دریافت قیمت خرید
- `get_gold_buy_price_toman(update, context)`: دریافت قیمت تومانی
- `execute_gold_buy(update, context)`: اجرای خرید طلا
- `start_sell_gold(update, context)`: شروع فروش طلا
- `get_gold_sell_weight(update, context)`: دریافت وزن فروش
- `get_gold_sell_price(update, context)`: دریافت قیمت فروش
- `get_gold_sell_price_toman(update, context)`: دریافت قیمت تومانی فروش
- `execute_gold_sell(update, context)`: اجرای فروش طلا
- `show_gold_history(update, context)`: نمایش تاریخچه طلا
- `get_gold_buy_exchange_rate(update, context)`: دریافت نرخ تبدیل خرید
- `get_gold_sell_exchange_rate(update, context)`: دریافت نرخ تبدیل فروش
- `start_petty_cash_withdraw(update, context)`: شروع برداشت از تنخواه
- `get_petty_cash_withdraw_amount(update, context)`: دریافت مبلغ برداشت
- `execute_petty_cash_withdraw(update, context)`: اجرای برداشت از تنخواه

---

### **handlers/wallet_invoice.py**
**توابع:**
- `handle_wallet_callbacks(update, context)`: مدیریت کالبک‌های کیف پول و فاکتور

---

### **handlers/transaction_viewer.py**
**توابع:**
- `handle_transaction_callbacks(update, context)`: مدیریت کالبک‌های تراکنش
- `handle_admin_transaction_view(update, context)`: نمایش تراکنش‌ها برای ادمین
- `show_transactions_list(update, context)`: نمایش لیست تراکنش‌ها

---

### **handlers/independent_income.py**
**وضعیت‌های ConversationHandler:**
- `INCOME_AMOUNT`: دریافت مبلغ درآمد
- `INCOME_DESC`: دریافت توضیحات درآمد

**توابع:**
- `start_independent_income(update, context)`: شروع ثبت درآمد مستقل
- `get_income_amount(update, context)`: دریافت مبلغ درآمد
- `record_independent_income(update, context)`: ثبت درآمد مستقل
- `cancel_independent_income(update, context)`: لغو ثبت درآمد

---

### **handlers/test_handler.py**
**توابع:**
- `handle_test_command(update, context)`: مدیریت دستور /test
- `handle_test_callback(update, context)`: مدیریت کالبک تست
- `handle_test_main_menu(update, context)`: نمایش منوی اصلی تست

---

## 🎹 کیبوردها (Keyboards)

### **keyboards/admin.py**
**توابع:**
- ایجاد کیبوردهای ادمین

### **keyboards/admin_settings.py**
**توابع:**
- ایجاد کیبوردهای تنظیمات ادمین

### **keyboards/customer.py**
**توابع:**
- ایجاد کیبوردهای مشتری

### **keyboards/customer_management.py**
**توابع:**
- ایجاد کیبوردهای مدیریت مشتریان

### **keyboards/operator.py**
**توابع:**
- ایجاد کیبوردهای اپراتور

### **keyboards/operator_files.py**
**توابع:**
- ایجاد کیبوردهای فایل‌های اپراتور

### **keyboards/editor.py**
**توابع:**
- ایجاد کیبوردهای ادیتور

### **keyboards/editor_status.py**
**توابع:**
- ایجاد کیبوردهای وضعیت ادیتور

### **keyboards/file_keyboards.py**
**توابع:**
- ایجاد کیبوردهای فایل

### **keyboards/receipt_management.py**
**توابع:**
- ایجاد کیبوردهای مدیریت رسید

### **keyboards/vault_management.py**
**توابع:**
- ایجاد کیبوردهای مدیریت صندوق

### **keyboards/vault_menu.py**
**توابع:**
- ایجاد کیبوردهای منوی صندوق

### **keyboards/broadcast_keyboards.py**
**توابع:**
- ایجاد کیبوردهای پخش پیام

### **keyboards/group_stats.py**
**توابع:**
- ایجاد کیبوردهای آمار گروهی

---

## 🛠️ ابزارها (Utils)

### **utils/discount_calculator.py**
**توابع:**
- محاسبه تخفیف بر اساس قوانین گروهی

### **utils/delivery_grouping.py**
**توابع:**
- گروه‌بندی سفارشات بر اساس زمان تحویل

### **utils/editor_state_manager.py**
**توابع:**
- مدیریت وضعیت کاری ادیتور

### **utils/file_naming.py**
**توابع:**
- تولید نام فایل استاندارد (مثل c1_001.stl)

### **utils/factor.py**
**توابع:**
- تولید و مدیریت فاکتورها

### **utils/group_analytics.py**
**توابع:**
- تحلیل عملکرد گروهی

### **utils/group_settings.py**
**توابع:**
- مدیریت تنظیمات گروهی

### **utils/group_config.py**
**توابع:**
- پیکربندی گروه‌ها

### **utils/notification_scheduler.py**
**توابع:**
- زمان‌بندی ارسال اعلان‌ها

### **utils/mapping_parser.py**
**توابع:**
- تجزیه فایل نقشه تبدیل (mapping.txt)

### **utils/stl_renderer.py**
**توابع:**
- رندر و پیش‌نمایش فایل‌های STL

### **utils/referral.py**
**توابع:**
- مدیریت سیستم دعوت و معرف

### **utils/broadcast_utils.py**
**توابع:**
- ابزارهای کمکی پخش پیام

---

## 🗃️ عملیات CRUD

### **database/crud.py - عملیات اصلی**

#### مدیریت کاربران
- `get_user(db, user_id)`: دریافت کاربر با شناسه
- `create_user(db, user)`: ایجاد کاربر جدید با کد مشتری
- `get_all_users(db)`: دریافت همه کاربران
- `get_customers(db)`: دریافت لیست مشتریان
- `update_user_max_referrals(db, user_id, new_limit)`: بروزرسانی سقف دعوت
- `add_admin_if_not_exists(db, user_id, full_name, phone_number)`: افزودن/بروزرسانی ادمین
- `add_operator_if_not_exists(db, user_id, full_name, phone_number)`: افزودن/بروزرسانی اپراتور
- `add_editor_if_not_exists(db, user_id, full_name, phone_number)`: افزودن/بروزرسانی ادیتور
- `add_visitor_if_not_exists(db, user_id, full_name, phone_number)`: افزودن/بروزرسانی بازدیدکننده

#### کدهای معرف
- `generate_unique_referral_code(db)`: تولید کد معرف یکتا
- `get_referral_code_info(db, referral_code)`: دریافت اطلاعات معرف
- `create_referral_code(db, creator_id, creator_role, code)`: ایجاد کد معرف
- `create_admin_referral_code(db, admin_id, code)`: ایجاد کد معرف ادمین
- `create_customer_referral_code(db, customer_id)`: تولید کد معرف مشتری
- `get_user_referral_codes(db, user_id)`: دریافت کدهای معرف کاربر

#### سفارشات فایل
- `create_file_order(db, file_order)`: ذخیره سفارش فایل
- `get_file_order_by_id(db, order_id)`: دریافت سفارش با شناسه
- `update_file_order(db, order_id, **kwargs)`: بروزرسانی سفارش
- `get_customers_with_pending_orders(db)`: دریافت مشتریان با سفارش در انتظار
- `get_customer_pending_orders(db, customer_id)`: دریافت سفارشات در انتظار مشتری

#### فاکتورها
- `create_invoice(db, invoice_data)`: ایجاد فاکتور جدید
- `get_customer_invoices(db, customer_id)`: دریافت فاکتورهای مشتری

#### تنظیمات سیستم
- `get_system_setting(db, key, default_value)`: دریافت تنظیم سیستم
- `set_system_setting(db, key, value, description)`: تنظیم مقدار سیستم
- `get_price_per_gram(db)`: دریافت قیمت هر گرم
- `set_price_per_gram(db, price)`: تنظیم قیمت هر گرم
- `generate_customer_code(db)`: تولید کد مشتری (c1, c2, ...)

#### آرشیو و آمار
- `get_user_files_by_period(db, user_id, days_back)`: دریافت فایل‌ها در بازه زمانی
- `get_user_files_count_by_period(db, user_id, days_back)`: شمارش فایل‌ها در بازه
- `get_user_files_grouped_by_date(db, user_id, days_back)`: دریافت فایل‌ها گروه‌بندی شده
- `get_archive_statistics(db, user_id)`: دریافت آمار آرشیو
- `get_orders_stats(db)`: دریافت آمار سفارشات

#### تعطیلات
- `toggle_holiday_status(db, date_str)`: تغییر وضعیت روز (تعطیل/کاری)
- `is_date_holiday(db, date_str)`: بررسی تعطیل بودن روز
- `get_holidays_in_range(db, start_date, end_date)`: دریافت تعطیلات در بازه

#### زمان‌بندی تحویل
- `clear_delivery_schedules(db)`: حذف همه تنظیمات تحویل
- `save_delivery_schedule(db, schedule_data)`: ذخیره تنظیمات تحویل
- `get_active_delivery_schedules(db)`: دریافت تنظیمات فعال تحویل

---

### **database/editor_crud.py - عملیات ادیتور**

#### مدیریت جلسه کاری
- `get_editor_current_session(db, editor_id)`: دریافت جلسه کاری فعلی
- `create_editor_session(db, editor_id, file_orders)`: ایجاد جلسه کاری جدید
- `get_editor_status(db, editor_id)`: تشخیص وضعیت فعلی ادیتور
- `complete_editor_session(db, editor_id)`: تکمیل جلسه کاری
- `get_editor_progress_stats(db, editor_id)`: دریافت آمار پیشرفت کار

#### مدیریت فایل‌ها
- `save_mapping_file(db, editor_id, mapping_content)`: ذخیره فایل نقشه
- `get_pending_files_for_editor(db, editor_id)`: دریافت لیست فایل‌های در انتظار
- `check_filename_expected(db, editor_id, filename)`: بررسی انتظار فایل
- `receive_editor_file(db, editor_id, filename, file_id, file_type)`: ثبت دریافت فایل
- `check_file_completion(processed_file)`: بررسی تکمیل فایل پردازش شده

#### دسترسی ادیتور
- `is_file_accessible_for_editor(file_order, delay_minutes)`: بررسی دسترسی به فایل
- `get_accessible_files_for_editor(db, status)`: دریافت فایل‌های قابل دسترس

#### ابزارها
- `log_filename_details(filename, label)`: لاگ جزئیات نام فایل
- `normalize_filename(filename)`: نرمال‌سازی نام فایل

---

### **database/group_crud.py - عملیات گروهی**

- `get_user_referral_tree(db, user_id, max_depth)`: دریافت درخت کامل معرف
- `get_group_files_by_date_range(db, user_id, start_date, end_date)`: دریافت فایل‌های گروه در بازه
- `calculate_group_monthly_stats(db, user_id)`: محاسبه آمار ماهانه گروه
- `check_group_discount_eligibility_advanced(db, user_id, threshold)`: بررسی پیشرفته واجد شرایط تخفیف
- `get_group_performance_summary(db, user_id)`: خلاصه عملکرد کلی گروه

---

### **database/broadcast_crud.py - عملیات پخش پیام**

#### مدیریت پیام‌ها
- `create_broadcast_message(db, message_type, content, ...)`: ایجاد پیام جدید
- `get_broadcast_by_id(db, broadcast_id)`: دریافت پیام با شناسه
- `update_broadcast_stats(db, broadcast_id, ...)`: بروزرسانی آمار ارسال
- `get_all_broadcasts(db, limit)`: دریافت همه پیام‌های ارسالی
- `get_broadcasts_by_type(db, message_type, limit)`: دریافت پیام‌ها بر اساس نوع
- `get_broadcast_statistics(db)`: دریافت آمار کلی پخش پیام

#### لاگ‌ها
- `create_broadcast_log(db, broadcast_message_id, ...)`: ثبت لاگ ارسال
- `get_broadcast_logs(db, broadcast_id, limit)`: دریافت لاگ‌های ارسال
- `get_customer_broadcast_history(db, customer_id, limit)`: تاریخچه پیام‌های دریافتی مشتری

#### تخفیف‌های موقت
- `get_active_discounts(db)`: دریافت تخفیف‌های فعال
- `get_discount_by_id(db, discount_id)`: دریافت تخفیف خاص
- `is_discount_currently_active(db, discount_id)`: بررسی فعال بودن تخفیف

---

## 📋 جریان کاری (Workflow)

### جریان سفارش مشتری:
1. **مشتری**: ارسال فایل STL
2. **سیستم**: محاسبه زمان تحویل و مهلت ویرایش
3. **مشتری**: تایید سفارش
4. **ادیتور**: دریافت فایل (بعد از تاخیر مشخص)
5. **ادیتور**: آپلود فایل نقشه (mapping.txt)
6. **ادیتور**: آپلود فایل‌های پردازش شده (STL, JPG, ZIP)
7. **اپراتور**: دسترسی به فایل‌های آماده
8. **اپراتور**: صدور فاکتور با وزن و عکس
9. **مشتری**: مشاهده فاکتور و پرداخت

### سیستم معرف:
1. **کاربر**: ایجاد کد معرف
2. **کاربر**: اشتراک لینک دعوت
3. **کاربر جدید**: ثبت‌نام با کد معرف
4. **سیستم**: افزودن اعتبار به معرف و معرفی شده

### سیستم پخش پیام:
1. **ادمین**: انتخاب نوع پیام (تبلیغاتی/موقت)
2. **ادمین**: ارسال محتوا (متن/عکس)
3. **ادمین** (اگر موقت): تنظیم بازه زمانی
4. **سیستم**: ارسال به مشتریان واجد شرایط
5. **سیستم**: ثبت لاگ و آمار ارسال

---

## 🔑 نقش‌های کاربری

1. **Admin**: دسترسی کامل به همه بخش‌ها
2. **Operator**: صدور فاکتور، مشاهده مشتریان و فایل‌ها
3. **Editor**: ویرایش فایل‌ها و آپلود فایل‌های پردازش شده
4. **Customer**: ارسال فایل، مشاهده فاکتور، دعوت دوستان
5. **Visitor**: دسترسی محدود (قابل ارتقا به Customer)

---

## 📊 الگوهای callback_data

### ادمین:
- `admin_main`: منوی اصلی ادمین
- `admin_customer_mgmt`: مدیریت مشتریان
- `admin_vaults_menu`: منوی صندوق‌ها
- `admin_settings`: تنظیمات
- `admin_holidays`: مدیریت تعطیلات
- `admin_group_settings`: تنظیمات گروهی
- `admin_group_stats`: آمار گروهی
- `broadcast_main`: منوی پخش پیام

### مشتری:
- `customer_wallet`: کیف پول
- `customer_archive`: آرشیو فایل‌ها
- `customer_referrals`: زیرمجموعه‌ها
- `customer_group_stats`: آمار گروهی
- `customer_invite`: دعوت

### اپراتور:
- `operator_main_menu`: منوی اصلی
- `operator_invoice_menu`: صدور فاکتور
- `operator_my_stats`: آمار کارهای من
- `operator_view_customers`: مشاهده مشتریان
- `operator_delivery_{number}`: مشاهده تحویل خاص

### ادیتور:
- `editor_pending_files`: فایل‌های در انتظار
- `editor_delivery_{datetime}`: فایل‌های تحویل خاص
- `editor_customer_{code}_{datetime}`: فایل‌های مشتری خاص

### صندوق:
- `vault_detail_{id}`: جزئیات صندوق
- `vault_create_new`: ایجاد صندوق جدید
- `gold_menu_{id}`: منوی طلا
- `gold_buy_{id}`: خرید طلا
- `gold_sell_{id}`: فروش طلا

### آرشیو:
- `ARCHIVE_LAST_WEEK`: فایل‌های هفته گذشته
- `ARCHIVE_LAST_MONTH`: فایل‌های ماه گذشته
- `ARCHIVE_ALL`: همه فایل‌ها

---

## 🎯 نکات مهم برای توسعه

### برای اضافه کردن قابلیت جدید:
1. مدل جدید در `database/models.py` یا `database/editor_models.py`
2. توابع CRUD در فایل مناسب `database/crud.py`
3. هندلر در `handlers/` با وضعیت‌های ConversationHandler
4. کیبورد در `keyboards/`
5. ثبت در `main.py` (ConversationHandler یا CallbackQueryHandler)

### الویت‌بندی handlers در main.py:
1. Broadcast handlers
2. Archive callbacks
3. Test callbacks
4. Editor callbacks
5. Group settings
6. Holiday handlers
7. Operator handlers
8. Vault handlers
9. Admin handlers
10. Customer handlers
11. General file handlers

### استفاده از Context:
```python
context.user_data['key'] = value  # ذخیره موقت در جلسه
context.bot_data['key'] = value   # ذخیره سراسری
```

### دسترسی به Database:
```python
from database.connection import SessionLocal
with SessionLocal() as db:
    # انجام عملیات
```

---

## 📞 تماس و پشتیبانی

برای گزارش مشکل یا درخواست قابلیت جدید:
- این فایل را به هوش مصنوعی ارسال کنید
- نام تابع، کلاس یا هندلر مورد نظر را مشخص کنید
- هوش مصنوعی کد کامل را از شما درخواست می‌کند

---

**نسخه مستندات**: 1.0
**تاریخ بروزرسانی**: 2025-11-19
**تعداد کل فایل‌های پایتون**: 75
**تعداد کل مدل‌ها**: 15
**تعداد کل هندلرها**: 25+
**تعداد کل توابع**: 300+
