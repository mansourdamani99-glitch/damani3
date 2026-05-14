<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>الحقني | المساعدة العاجلة على الطريق - متصل بـ Supabase</title>
    <!-- Fonts & Icons -->
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <!-- Supabase JS -->
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Cairo', sans-serif;
            background: radial-gradient(ellipse at 30% 40%, #0a0f1e, #03050b);
            color: #eef5ff;
            scroll-behavior: smooth;
            overflow-x: hidden;
            position: relative;
        }

        .stars {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
            overflow: hidden;
        }

        .star {
            position: absolute;
            background-color: #fff;
            border-radius: 50%;
            opacity: 0.8;
            box-shadow: 0 0 8px rgba(255,255,200,0.8);
            animation: floatStar linear infinite;
        }

        @keyframes floatStar {
            0% { transform: translateY(0vh) translateX(0) rotate(0deg); opacity: 0.3; }
            50% { opacity: 1; }
            100% { transform: translateY(100vh) translateX(20px) rotate(360deg); opacity: 0.2; }
        }

        @keyframes twinkle {
            0% { opacity: 0.2; transform: scale(1);}
            100% { opacity: 1; transform: scale(1.3);}
        }

        .container {
            position: relative;
            z-index: 2;
            max-width: 1400px;
            margin: 0 auto;
            padding: 1rem 2rem;
        }

        .glow-text {
            text-shadow: 0 0 6px #b0f0ff, 0 0 12px #4effdc, 0 0 20px #00a6c4;
        }

        h1, h2, h3, .logo { font-weight: 700; }
        h2 {
            font-size: 2rem;
            margin-bottom: 1rem;
            border-right: 4px solid #0ff;
            padding-right: 1rem;
            display: inline-block;
            text-shadow: 0 0 5px cyan;
        }

        nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            background: rgba(0,0,0,0.65);
            backdrop-filter: blur(12px);
            border-radius: 60px;
            padding: 0.8rem 2rem;
            margin-bottom: 2rem;
            border: 1px solid rgba(0,255,255,0.2);
            box-shadow: 0 0 20px rgba(0,180,220,0.2);
        }

        .logo i { font-size: 2rem; color: #0ff; margin-left: 0.5rem; }
        .nav-links { display: flex; gap: 1rem; flex-wrap: wrap; align-items: center; }
        .nav-links a {
            color: #eef5ff;
            text-decoration: none;
            font-weight: 500;
            padding: 0.5rem 1rem;
            border-radius: 40px;
            transition: 0.2s;
        }
        .nav-links a:hover, .nav-links a.active {
            background: rgba(0, 255, 255, 0.2);
            text-shadow: 0 0 6px cyan;
            box-shadow: 0 0 10px rgba(0,255,255,0.4);
        }
        .user-badge {
            background: linear-gradient(135deg, #00b8b0, #0088aa);
            border-radius: 40px;
            padding: 0.4rem 1rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            font-size: 0.9rem;
        }
        .logout-btn {
            background: rgba(255,80,80,0.3);
            border: 1px solid #ff6666;
            border-radius: 30px;
            padding: 0.3rem 0.8rem;
            cursor: pointer;
            font-size: 0.8rem;
            transition: 0.2s;
        }
        .logout-btn:hover { background: rgba(255,80,80,0.6); }

        .auth-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.95);
            backdrop-filter: blur(15px);
            z-index: 1000;
            display: flex;
            justify-content: center;
            align-items: center;
            transition: 0.3s;
        }
        .auth-card {
            background: rgba(10, 20, 40, 0.95);
            border-radius: 2rem;
            padding: 2rem;
            width: 90%;
            max-width: 450px;
            border: 1px solid #0ff;
            box-shadow: 0 0 50px rgba(0,255,255,0.3);
            text-align: center;
        }
        .auth-card h2 { border: none; margin-bottom: 1.5rem; font-size: 1.8rem; }
        .auth-tabs {
            display: flex;
            gap: 1rem;
            margin-bottom: 1.5rem;
            border-bottom: 1px solid rgba(0,255,255,0.3);
            padding-bottom: 0.5rem;
        }
        .auth-tab {
            flex: 1;
            background: transparent;
            border: none;
            color: #ccddf8;
            font-size: 1.2rem;
            padding: 0.5rem;
            cursor: pointer;
            border-radius: 30px;
            transition: 0.2s;
        }
        .auth-tab.active {
            background: rgba(0,255,255,0.2);
            color: #0ff;
            text-shadow: 0 0 5px cyan;
        }
        .auth-input {
            width: 100%;
            padding: 0.8rem 1.2rem;
            margin: 0.8rem 0;
            background: rgba(20, 30, 55, 0.8);
            border: 1px solid rgba(0, 255, 200, 0.6);
            border-radius: 60px;
            color: white;
            font-family: 'Cairo', sans-serif;
        }
        .auth-btn {
            background: linear-gradient(95deg, #00b8b0, #0088aa);
            width: 100%;
            padding: 0.8rem;
            border-radius: 60px;
            font-weight: bold;
            margin-top: 1rem;
            cursor: pointer;
        }
        .close-modal {
            position: absolute;
            top: 20px;
            left: 20px;
            background: rgba(255,80,80,0.5);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 1.5rem;
        }
        .page { display: none; animation: fadeSlide 0.5s ease-out; background: rgba(8, 12, 25, 0.55); backdrop-filter: blur(2px); border-radius: 2rem; padding: 2rem; margin-top: 1rem; border: 1px solid rgba(0, 255, 255, 0.25); }
        .active-page { display: block; }
        @keyframes fadeSlide { from { opacity: 0; transform: translateY(15px);} to { opacity: 1; transform: translateY(0);} }
        .services-grid, .features-grid, .faq-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 1.8rem; margin-top: 2rem; }
        .card { background: rgba(0, 0, 0, 0.65); backdrop-filter: blur(5px); border-radius: 1.5rem; padding: 1.5rem; transition: 0.25s; border: 1px solid rgba(0, 255, 255, 0.3); cursor: pointer; }
        .card i { font-size: 2.5rem; color: #0ff; margin-bottom: 1rem; }
        .card:hover { transform: translateY(-8px); border-color: #0ff; box-shadow: 0 0 25px rgba(0,255,255,0.4); }
        .benefits-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 1.8rem; margin-top: 2rem; }
        .benefit-card { background: rgba(15, 25, 45, 0.35); backdrop-filter: blur(12px); border-radius: 1.8rem; padding: 1.5rem; text-align: center; border: 1px solid rgba(0, 255, 255, 0.4); animation: glowPulse 2.5s infinite; }
        @keyframes glowPulse { 0% { border-color: rgba(0, 255, 255, 0.2); } 50% { border-color: rgba(0, 255, 255, 0.9); } 100% { border-color: rgba(0, 255, 255, 0.2); } }
        .btn { background: linear-gradient(95deg, #00b8b0, #0088aa); border: none; padding: 0.7rem 1.4rem; border-radius: 2rem; font-weight: bold; color: white; cursor: pointer; transition: 0.2s; }
        .btn:hover { transform: scale(1.02); box-shadow: 0 0 15px cyan; }
        .btn-outline { background: transparent; border: 1px solid #0ff; color: #0ff; }
        .modern-input, .modern-textarea { width: 100%; padding: 0.9rem 1.5rem; margin: 0.8rem 0; background: rgba(20, 30, 55, 0.5); backdrop-filter: blur(8px); border: 1px solid rgba(0, 255, 200, 0.6); border-radius: 60px; color: white; font-family: 'Cairo', sans-serif; }
        .order-item { border-bottom: 1px solid cyan; padding: 1rem; margin-bottom: 0.5rem; }
        .status-badge { display: inline-block; padding: 0.2rem 1rem; border-radius: 30px; font-size: 0.75rem; font-weight: bold; }
        .status-pending { background: #f0b400; color: #1e1a00; }
        .status-progress { background: #0a6eff; color: white; }
        .status-completed { background: #00cc88; color: #002b1a; }
        .status-cancelled { background: #aa2e4e; color: white; }
        .developers-section { display: flex; justify-content: center; gap: 2.5rem; flex-wrap: wrap; margin: 1.5rem 0; }
        .dev-card { display: flex; align-items: center; gap: 0.8rem; background: rgba(0, 20, 40, 0.6); padding: 0.6rem 1.8rem; border-radius: 3rem; border: 1px solid rgba(0,255,200,0.5); }
        .fading-star { animation: starFade 1.8s infinite; display: inline-block; }
        @keyframes starFade { 0% { opacity: 0.2; } 50% { opacity: 1; text-shadow: 0 0 15px #ffaa33; } 100% { opacity: 0.2; } }
        .dev-name { background: linear-gradient(135deg, #aaffff, #00d4ff); -webkit-background-clip: text; background-clip: text; color: transparent; font-weight: bold; }
        .footer-social { margin-top: 3rem; background: rgba(0,0,0,0.7); border-radius: 2rem; padding: 2rem; text-align: center; }
        .social-icons { display: flex; justify-content: center; gap: 2rem; margin: 1.5rem 0; flex-wrap: wrap; }
        .social-icons a { color: #0ff; font-size: 1.8rem; transition: 0.2s; }
        .social-icons a:hover { transform: scale(1.2); text-shadow: 0 0 15px cyan; }
        .loading { opacity: 0.6; pointer-events: none; }
        @media (max-width: 780px) { .container { padding: 1rem; } nav { flex-direction: column; gap: 1rem; } }
    </style>
</head>
<body>
<div class="stars" id="starsContainer"></div>
<div class="container">
    <nav>
        <div class="logo glow-text"><i class="fas fa-car-crash"></i> الحقني</div>
        <div class="nav-links" id="navLinks">
            <a href="#" data-page="home" class="active">🏠 الرئيسية</a>
            <a href="#" data-page="services">🛠️ الخدمات</a>
            <a href="#" data-page="request">📢 طلب مساعدة</a>
            <a href="#" data-page="myorders">📋 طلباتي</a>
            <a href="#" data-page="features">✨ الميزات</a>
            <a href="#" data-page="faq">❓ الأسئلة</a>
            <a href="#" data-page="contact">📞 تواصل</a>
            <div id="userNavArea"></div>
        </div>
    </nav>

    <div id="home" class="page active-page">
        <h2 class="glow-text">⭐ مرحباً بك في الحقني</h2>
        <p style="font-size:1.2rem; margin-top:1rem;">تطبيق المساعدة العاجلة للسيارات والشاحنات في الجزائر — 24 ساعة، سرعة فائقة، خدمات متكاملة.</p>
        <div class="services-grid"><div class="card"><i class="fas fa-wrench"></i><h3>ميكانيكي متنقل</h3><p>إصلاح عاجل في موقع العطل.</p></div><div class="card"><i class="fas fa-gas-pump"></i><h3>توصيل وقود</h3><p>نوصل لك الوقود أينما كنت.</p></div><div class="card"><i class="fas fa-truck"></i><h3>ديبناج (سحب)</h3><p>شاحنة رفع لنقل سيارتك.</p></div></div>
        <button class="btn" id="quickRequestBtnHome">📱 اطلب المساعدة الآن</button>
        <div class="benefits-grid" style="margin-top:2rem;"><div class="benefit-card"><i class="fas fa-bolt"></i><p>سرعة الاستجابة - وصول المساعدة في أقل من 20 دقيقة</p></div><div class="benefit-card"><i class="fas fa-shield-alt"></i><p>الراحة والأمان - خدمة 24 ساعة</p></div></div>
    </div>
    <div id="services" class="page"><h2 class="glow-text">🚛 جميع الخدمات</h2><div class="services-grid"><div class="card"><i class="fas fa-tools"></i><h3>إصلاح ميكانيكي</h3><p>ميكانيكي محترف يصل إلى موقعك خلال دقائق.</p></div><div class="card"><i class="fas fa-tint"></i><h3>توصيل الزيوت</h3><p>زيت محرك, ناقل حركة حسب الطلب.</p></div><div class="card"><i class="fas fa-charging-station"></i><h3>توصيل الوقود</h3><p>بنزين، ديزل نصل لأي مكان.</p></div><div class="card"><i class="fas fa-truck-moving"></i><h3>خدمة السحب</h3><p>سيارة ديبناج مجهزة لسحب جميع أنواع المركبات.</p></div></div></div>
    <div id="request" class="page"><h2 class="glow-text">📢 طلب مساعدة فورية</h2><div class="card" style="max-width:700px; margin:1rem auto;"><form id="helpRequestForm"><label>نوع الخدمة *</label><select id="serviceType" class="modern-input"><option value="ميكانيكي متنقل">🔧 ميكانيكي متنقل</option><option value="توصيل وقود">⛽ توصيل وقود</option><option value="توصيل زيوت">🛢️ توصيل زيوت</option><option value="ديبناج (سحب)">🚚 ديبناج / سحب</option><option value="توجيه إلى ورشة">🗺️ توجيه إلى ورشة</option></select><label>وصف المشكلة</label><textarea id="problemDesc" class="modern-textarea" rows="2"></textarea><label>رقم هاتفك *</label><input type="tel" id="phoneReq" required class="modern-input"><div style="display:flex; gap:0.5rem; flex-wrap:wrap;"><button type="button" id="getLocationBtn" class="btn btn-outline" style="flex:1">📍 تحديد موقعي</button><input type="text" id="locationLink" placeholder="رابط الخريطة أو العنوان" class="modern-input" style="flex:2"></div><button type="submit" class="btn" style="width:100%; margin-top:1rem;">✨ إرسال الطلب ✨</button></form></div><p class="glow-text" style="text-align:center;">⚡ هز هاتفك للإبلاغ السريع</p></div>
    <div id="myorders" class="page"><h2 class="glow-text">📋 طلباتي</h2><div id="ordersContainer" class="order-list"><p style="text-align:center;">✨ سيتم عرض طلباتك بعد تسجيل الدخول ✨</p></div><button id="refreshOrdersBtn" class="btn btn-outline">🔄 تحديث الطلبات</button></div>
    <div id="features" class="page"><h2 class="glow-text">💎 مميزات التطبيق</h2><div class="features-grid"><div class="card"><i class="fas fa-bolt"></i><h3>هز الهاتف للتبليغ</h3><p>هز جهازك لفتح طلب مساعدة بسرعة البرق.</p></div><div class="card"><i class="fas fa-moon"></i><h3>وضع ليلي</h3><p>واجهة مريحة مع نجوم متلألئة.</p></div><div class="card"><i class="fas fa-chart-line"></i><h3>متابعة الطلبات</h3><p>حالة الطلب: انتظار، تنفيذ، مكتمل.</p></div><div class="card"><i class="fas fa-map-pin"></i><h3>GPS دقيق</h3><p>نسخ رابط موقع Google Maps بدقة عالية.</p></div></div></div>
    <div id="faq" class="page"><h2 class="glow-text">❓ الأسئلة الشائعة</h2><div class="faq-grid"><div class="card"><h3>كيف أطلب المساعدة؟</h3><p>اختر الخدمة من صفحة الطلب، حدد موقعك، ثم أرسل الطلب.</p></div><div class="card"><h3>كم سرعة الاستجابة؟</h3><p>فريقنا المنتشر يستجيب خلال 15-30 دقيقة حسب الموقع.</p></div><div class="card"><h3>هل التطبيق مجاني؟</h3><p>نعم، جميع الخدمات الأساسية مجانية.</p></div><div class="card"><h3>هل تغطيون كل الجزائر؟</h3><p>نعمل حالياً في كبرى المدن والطرق السريعة.</p></div></div></div>
    <div id="contact" class="page"><h2 class="glow-text">📞 تواصل مع فريق الحقني</h2><div class="card" style="max-width:700px; margin:1rem auto;"><p><i class="fas fa-envelope"></i> support@alhaqni.com</p><p><i class="fab fa-whatsapp"></i> واتساب: +213 789 456 123</p><div class="developers-section"><div class="dev-card"><span class="fading-star">⭐</span><i class="fas fa-user-astronaut"></i><span class="dev-name">دماني نعيمة</span></div><div class="dev-card"><span class="fading-star">⭐</span><i class="fas fa-user-astronaut"></i><span class="dev-name">بلعدل فاطيمة</span></div></div><form id="reportIssue"><textarea id="reportMsg" placeholder="الإبلاغ عن مشكلة أو اقتراح..." class="modern-textarea" rows="3"></textarea><button type="submit" class="btn" style="width:100%;">إرسال التبليغ 🌟</button></form></div></div>
    <div class="footer-social"><div class="social-icons"><a href="#"><i class="fab fa-facebook-f"></i></a><a href="#"><i class="fab fa-instagram"></i></a><a href="#"><i class="fab fa-twitter"></i></a><a href="#"><i class="fab fa-linkedin-in"></i></a><a href="#"><i class="fab fa-youtube"></i></a></div><p>© 2025 الحقني — مساعدة الطريق بلا حدود | تصميم فضائي احترافي</p></div>
</div>

<div id="authModal" class="auth-modal">
    <div class="close-modal" id="closeModalBtn">✖</div>
    <div class="auth-card">
        <div class="auth-tabs">
            <button class="auth-tab active" data-auth-tab="login">تسجيل الدخول</button>
            <button class="auth-tab" data-auth-tab="signup">إنشاء حساب</button>
        </div>
        <div id="loginForm">
            <input type="email" id="loginEmail" placeholder="البريد الإلكتروني" class="auth-input" autocomplete="email">
            <input type="password" id="loginPassword" placeholder="كلمة السر" class="auth-input" autocomplete="current-password">
            <button id="doLoginBtn" class="btn auth-btn">دخول</button>
        </div>
        <div id="signupForm" style="display:none;">
            <input type="email" id="signupEmail" placeholder="البريد الإلكتروني" class="auth-input" autocomplete="email">
            <input type="text" id="signupFullName" placeholder="الاسم الكامل (اختياري)" class="auth-input">
            <input type="tel" id="signupPhone" placeholder="رقم الهاتف (اختياري)" class="auth-input">
            <input type="password" id="signupPassword" placeholder="كلمة السر (6 أحرف على الأقل)" class="auth-input" autocomplete="new-password">
            <button id="doSignupBtn" class="btn auth-btn">إنشاء حساب</button>
        </div>
    </div>
</div>

<script>
    // ===================== تكوين Supabase =====================
    // ⚠️ IMPORTANT: استبدل هذه القيم ببيانات مشروعك من Supabase
    const SUPABASE_URL = 'https://mpbnwlzlxnqkhugdxmtb.supabase.co';  // ضع رابط مشروعك هنا
    const SUPABASE_ANON_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im1wYm53bHpseG5xa2h1Z2R4bXRiIiwicm9sZSI6ImFub24iLCJpYXQiOjE3Nzc1NTE4MjEsImV4cCI6MjA5MzEyNzgyMX0.l5GJ4Wiol38evG7GSX8GzENwyC_tKWd0-P4Y1gZ9kyI';  // ضع المفتاح العام هنا
    
    let supabase = null;
    let currentUser = null;
    
    // محاولة تهيئة Supabase
    if (typeof supabaseJs !== 'undefined') {
        try {
            supabase = supabaseJs.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);
            console.log("✅ Supabase متصل بنجاح");
        } catch(e) { console.error("Supabase error:", e); }
    }
    
    // ===================== دوال المصادقة مع Supabase =====================
    async function signupWithSupabase(email, password, fullName, phone) {
        if (!supabase) return { success: false, message: 'Supabase غير مهيأ، تأكد من إعداد URL و ANON_KEY' };
        if (password.length < 6) return { success: false, message: 'كلمة السر يجب أن تكون 6 أحرف على الأقل' };
        
        try {
            // 1. تسجيل المستخدم في Auth
            const { data: authData, error: authError } = await supabase.auth.signUp({
                email: email,
                password: password,
                options: { data: { full_name: fullName, phone: phone } }
            });
            
            if (authError) return { success: false, message: authError.message };
            
            // 2. إدخال البيانات في جدول users (إذا كان RLS يسمح)
            if (authData.user) {
                const { error: insertError } = await supabase
                    .from('users')
                    .insert([{
                        id: authData.user.id,
                        email: email,
                        full_name: fullName || null,
                        phone: phone || null,
                        role: 'user',
                        created_at: new Date().toISOString()
                    }]);
                if (insertError) console.warn("خطأ في إدخال users:", insertError);
            }
            
            return { success: true, user: authData.user };
        } catch (err) {
            return { success: false, message: err.message };
        }
    }
    
    async function loginWithSupabase(email, password) {
        if (!supabase) return { success: false, message: 'Supabase غير مهيأ' };
        try {
            const { data, error } = await supabase.auth.signInWithPassword({ email, password });
            if (error) return { success: false, message: error.message };
            return { success: true, user: data.user };
        } catch (err) {
            return { success: false, message: err.message };
        }
    }
    
    async function logoutFromSupabase() {
        if (supabase) await supabase.auth.signOut();
        currentUser = null;
        localStorage.removeItem('haqni_session_supabase');
        renderUserNav();
        renderOrders();
        showPage('home');
    }
    
    async function checkSession() {
        if (!supabase) return;
        const { data: { session } } = await supabase.auth.getSession();
        if (session) {
            currentUser = session.user;
            localStorage.setItem('haqni_session_supabase', JSON.stringify({ id: currentUser.id, email: currentUser.email }));
            renderUserNav();
            renderOrders();
        } else {
            const saved = localStorage.getItem('haqni_session_supabase');
            if (saved) {
                try {
                    const { data } = await supabase.auth.getUser();
                    if (data.user) {
                        currentUser = data.user;
                        renderUserNav();
                        renderOrders();
                    } else throw new Error();
                } catch(e) { localStorage.removeItem('haqni_session_supabase'); currentUser = null; }
            } else currentUser = null;
        }
        renderUserNav();
    }
    
    // ===================== دوال الطلبات مع Supabase =====================
    async function addOrderToSupabase(service, phone, location, description) {
        if (!currentUser) { alert('يرجى تسجيل الدخول أولاً'); openAuthModal(); return null; }
        if (!supabase) { alert('Supabase غير متصل'); return null; }
        
        const newOrder = {
            user_id: currentUser.id,
            service_type: service,
            phone: phone,
            location: location || 'تم تحديد الموقع',
            description: description || '',
            status: 'pending',
            created_at: new Date().toISOString()
        };
        
        try {
            const { data, error } = await supabase.from('assistance_requests').insert([newOrder]).select();
            if (error) throw error;
            alert('✅ تم إرسال الطلب بنجاح!');
            renderOrders();
            return data[0];
        } catch (err) {
            console.error(err);
            alert('❌ فشل إرسال الطلب: ' + err.message);
            return null;
        }
    }
    
    async function renderOrders() {
        const container = document.getElementById('ordersContainer');
        if (!container) return;
        if (!currentUser) { container.innerHTML = '<p style="text-align:center;">🔐 يرجى تسجيل الدخول لمشاهدة طلباتك</p>'; return; }
        if (!supabase) { container.innerHTML = '<p style="text-align:center;">⚠️ Supabase غير متصل</p>'; return; }
        
        container.innerHTML = '<p style="text-align:center;">⏳ جاري تحميل الطلبات...</p>';
        
        try {
            const { data, error } = await supabase
                .from('assistance_requests')
                .select('*')
                .eq('user_id', currentUser.id)
                .order('created_at', { ascending: false });
            
            if (error) throw error;
            
            if (!data || data.length === 0) {
                container.innerHTML = '<p style="text-align:center;">✨ لا توجد طلبات بعد. قم بتقديم طلب جديد ✨</p>';
                return;
            }
            
            container.innerHTML = '';
            data.forEach(order => {
                let statusClass = { pending:'status-pending', in_progress:'status-progress', completed:'status-completed', cancelled:'status-cancelled' }[order.status] || 'status-pending';
                let statusText = { pending:'قيد الانتظار', in_progress:'قيد التنفيذ', completed:'مكتمل', cancelled:'ملغي' }[order.status] || 'قيد الانتظار';
                const div = document.createElement('div'); div.className = 'order-item';
                div.innerHTML = `<strong><i class="fas fa-concierge-bell"></i> ${order.service_type}</strong><br>📞 ${order.phone} | 📍 ${order.location}<br>📝 ${order.description || 'لا يوجد'}<br><span class="status-badge ${statusClass}">${statusText}</span><small style="float:left;">${new Date(order.created_at).toLocaleString('ar-DZ')}</small><div style="clear:both"></div>`;
                container.appendChild(div);
            });
        } catch (err) {
            container.innerHTML = '<p style="text-align:center;">❌ خطأ في تحميل الطلبات</p>';
            console.error(err);
        }
    }
    
    // ===================== دوال الواجهة =====================
    function openAuthModal() { document.getElementById('authModal').style.display = 'flex'; }
    function closeAuthModal() { document.getElementById('authModal').style.display = 'none'; }
    function switchAuthTab(tab) {
        document.getElementById('loginForm').style.display = tab === 'login' ? 'block' : 'none';
        document.getElementById('signupForm').style.display = tab === 'signup' ? 'block' : 'none';
        document.querySelectorAll('.auth-tab').forEach(btn => btn.classList.remove('active'));
        document.querySelector(`.auth-tab[data-auth-tab="${tab}"]`).classList.add('active');
    }
    
    function renderUserNav() {
        const navArea = document.getElementById('userNavArea');
        if (!navArea) return;
        if (currentUser) {
            const emailShort = currentUser.email ? currentUser.email.split('@')[0] : 'مستخدم';
            navArea.innerHTML = `<div class="user-badge"><i class="fas fa-user-circle"></i> ${emailShort} <span class="logout-btn" id="logoutBtn">خروج</span></div>`;
            document.getElementById('logoutBtn')?.addEventListener('click', () => logoutFromSupabase());
        } else {
            navArea.innerHTML = `<a href="#" id="showAuthBtn">🔐 دخول / حساب جديد</a>`;
            document.getElementById('showAuthBtn')?.addEventListener('click', (e) => { e.preventDefault(); openAuthModal(); });
        }
    }
    
    const pages = ['home','services','request','myorders','features','faq','contact'];
    function showPage(pageId) {
        pages.forEach(p => document.getElementById(p)?.classList.remove('active-page'));
        document.getElementById(pageId)?.classList.add('active-page');
        document.querySelectorAll('.nav-links a[data-page]').forEach(link => link.classList.remove('active'));
        const activeLink = document.querySelector(`.nav-links a[data-page="${pageId}"]`);
        if (activeLink) activeLink.classList.add('active');
        localStorage.setItem('currentPage', pageId);
        window.scrollTo({ top: 0 });
    }
    
    // ===================== أحداث الصفحة =====================
    generateMovingStars();
    function generateMovingStars() {
        const starsDiv = document.getElementById('starsContainer');
        starsDiv.innerHTML = '';
        for(let i=0;i<200;i++) {
            let star = document.createElement('div');
            star.classList.add('star');
            star.style.width = (Math.random()*3+1) + 'px';
            star.style.height = star.style.width;
            star.style.left = Math.random()*100 + '%';
            star.style.top = Math.random()*100 + '%';
            star.style.animation = `floatStar ${8+Math.random()*15}s linear infinite`;
            starsDiv.appendChild(star);
        }
    }
    
    document.querySelectorAll('.nav-links a[data-page]').forEach(link => {
        link.addEventListener('click', (e) => { e.preventDefault(); showPage(link.getAttribute('data-page')); });
    });
    document.getElementById('quickRequestBtnHome')?.addEventListener('click', () => showPage('request'));
    document.getElementById('refreshOrdersBtn')?.addEventListener('click', () => renderOrders());
    document.getElementById('closeModalBtn')?.addEventListener('click', closeAuthModal);
    document.querySelectorAll('.auth-tab').forEach(btn => {
        btn.addEventListener('click', () => switchAuthTab(btn.getAttribute('data-auth-tab')));
    });
    
    document.getElementById('doLoginBtn')?.addEventListener('click', async () => {
        const email = document.getElementById('loginEmail').value.trim();
        const password = document.getElementById('loginPassword').value;
        if (!email || !password) { alert('املأ البريد وكلمة السر'); return; }
        const res = await loginWithSupabase(email, password);
        if (res.success) {
            currentUser = res.user;
            localStorage.setItem('haqni_session_supabase', JSON.stringify({ id: currentUser.id, email: currentUser.email }));
            closeAuthModal();
            renderUserNav();
            renderOrders();
            showPage('home');
            alert(`مرحباً ${currentUser.email}`);
        } else alert(res.message);
    });
    
    document.getElementById('doSignupBtn')?.addEventListener('click', async () => {
        const email = document.getElementById('signupEmail').value.trim();
        const fullName = document.getElementById('signupFullName').value.trim();
        const phone = document.getElementById('signupPhone').value.trim();
        const password = document.getElementById('signupPassword').value;
        if (!email || !password) { alert('البريد الإلكتروني وكلمة السر مطلوبة'); return; }
        const res = await signupWithSupabase(email, password, fullName, phone);
        if (res.success) {
            alert('تم إنشاء الحساب بنجاح! يمكنك تسجيل الدخول الآن.');
            switchAuthTab('login');
            document.getElementById('loginEmail').value = email;
            document.getElementById('loginPassword').value = '';
        } else alert(res.message);
    });
    
    document.getElementById('helpRequestForm')?.addEventListener('submit', async (e) => {
        e.preventDefault();
        if (!currentUser) { alert('يرجى تسجيل الدخول أولاً'); openAuthModal(); return; }
        const service = document.getElementById('serviceType').value;
        const desc = document.getElementById('problemDesc').value;
        const phone = document.getElementById('phoneReq').value;
        let loc = document.getElementById('locationLink').value;
        if (!phone) { alert('أدخل رقم الهاتف'); return; }
        if (!loc) loc = "لم يتم تحديد الموقع";
        await addOrderToSupabase(service, phone, loc, desc);
        document.getElementById('helpRequestForm').reset();
        showPage('myorders');
    });
    
    document.getElementById('getLocationBtn')?.addEventListener('click', () => {
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(pos => {
                document.getElementById('locationLink').value = `https://www.google.com/maps?q=${pos.coords.latitude},${pos.coords.longitude}`;
                alert('✅ تم تحديد موقعك بنجاح');
            }, () => alert('⚠️ تعذر تحديد الموقع'));
        } else alert("المتصفح لا يدعم تحديد الموقع");
    });
    
    document.getElementById('reportIssue')?.addEventListener('submit', (e) => {
        e.preventDefault();
        const msg = document.getElementById('reportMsg').value;
        if (msg.trim()) alert(`شكراً لك، تم استلام بلاغك: ${msg.substring(0,50)}...`);
        else alert('الرجاء كتابة تفاصيل المشكلة');
        e.target.reset();
    });
    
    // هز الهاتف
    if (window.DeviceMotionEvent) {
        let lastShake = 0;
        window.addEventListener('devicemotion', (e) => {
            let acc = e.accelerationIncludingGravity;
            if (acc && (Math.abs(acc.x) > 15 || Math.abs(acc.y) > 15 || Math.abs(acc.z) > 15)) {
                let now = Date.now();
                if (now - lastShake > 1500) {
                    lastShake = now;
                    if (confirm('🚨 هزاز الهاتف تم تفعيله! هل تريد طلب مساعدة عاجلة؟')) {
                        if (!currentUser) openAuthModal();
                        else showPage('request');
                    }
                }
            }
        });
    }
    
    // بدء التطبيق
    checkSession();
    if (!currentUser) setTimeout(() => openAuthModal(), 500);
</script>
</body>
</html>
