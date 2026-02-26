<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Duka Points | تطبيق المكافآت</title>
    <!-- إضافة Font Awesome للأيقونات -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', sans-serif; }
        
        body {
            background: #1a1a1a;
            height: 100vh; display: flex; justify-content: center; align-items: center; 
            color: white; overflow: hidden;
        }

        /* --- شاشة البداية المتحركة (Splash Screen) --- */
        #splash-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: #d63031; /* أحمر دوكا */
            display: flex; justify-content: center; align-items: center;
            z-index: 9999; transition: opacity 0.8s ease;
        }

        .splash-content {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 20px;
        }

        /* الأيقونة */
        .splash-icon {
            font-size: 90px;
            color: #fff;
            text-shadow: 0 0 15px rgba(255,255,255,0.6);
            opacity: 0;
            animation: iconPop 0.8s forwards;
            animation-delay: 0.1s; /* تظهر مع أول حرف تقريباً */
        }

        @keyframes iconPop {
            0% {
                transform: scale(0) rotate(-90deg);
                opacity: 0;
            }
            60% {
                transform: scale(1.2) rotate(10deg);
                opacity: 1;
            }
            100% {
                transform: scale(1) rotate(0deg);
                opacity: 1;
            }
        }

        .loader-text {
            font-size: 70px;
            font-weight: 900;
            letter-spacing: 15px;
            display: flex;
            direction: ltr;
            color: #fff;
            text-shadow: 0 0 10px rgba(255,255,255,0.5);
        }

        .loader-text span {
            display: inline-block;
            opacity: 0;
            animation: bounceFlip 0.8s forwards;
            transform-origin: center;
        }

        /* توقيت ظهور كل حرف */
        .loader-text span:nth-child(1) { animation-delay: 0.2s; }
        .loader-text span:nth-child(2) { animation-delay: 0.5s; }
        .loader-text span:nth-child(3) { animation-delay: 0.8s; }
        .loader-text span:nth-child(4) { animation-delay: 1.1s; }

        /* حركة الحروف */
        @keyframes bounceFlip {
            0% {
                transform: translateY(100px) rotate(-20deg) scale(0.5);
                opacity: 0;
            }
            60% {
                transform: translateY(-20px) rotate(5deg) scale(1.1);
                opacity: 1;
            }
            100% {
                transform: translateY(0) rotate(0deg) scale(1);
                opacity: 1;
            }
        }

        /* --- المحتوى الرئيسي بعد الإخفاء --- */
        #main-content {
            display: none; width: 100%; height: 100%;
            background: linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.8)), 
                        url('https://images.unsplash.com/photo-1504674900247-0877df9cc836?auto=format&fit=crop&w=1350&q=80');
            background-size: cover; background-position: center;
            justify-content: center; align-items: center;
        }

        .container {
            width: 90%; max-width: 400px; text-align: center; padding: 25px;
            background: rgba(255, 255, 255, 0.1); backdrop-filter: blur(15px);
            border-radius: 25px; border: 1px solid rgba(255,255,255,0.2);
            margin: auto;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }

        /* العناصر المشتركة */
        .logo-circle {
            background: white; width: 70px; height: 70px; border-radius: 50%;
            margin: 0 auto 15px; display: flex; flex-direction: column; 
            justify-content: center; align-items: center; border: 3px solid #ffcc00;
        }
        input { 
            width: 100%; padding: 12px; margin: 8px 0; border-radius: 12px; 
            border: none; text-align: center; font-size: 16px;
        }
        .btn { 
            display: block; width: 100%; padding: 14px; margin-top: 10px; 
            border-radius: 15px; font-weight: bold; border: none; cursor: pointer; 
            transition: 0.3s; font-size: 16px;
        }
        .btn-red { background: #d63031; color: white; }
        .btn-white { background: white; color: #d63031; }
        .btn:active { transform: scale(0.98); }
        
        .page { display: none; }
        .active { display: block; }
        .points-display { background: white; color: #333; padding: 15px; border-radius: 15px; margin: 15px 0; }
        .points-num { font-size: 40px; font-weight: bold; color: #d63031; }

        .back-link {
            margin-top: 10px; cursor: pointer; font-size: 12px; color: #ffcc00;
            transition: color 0.3s;
        }
        .back-link:hover { color: #fff; }
    </style>
</head>
<body>

    <div id="splash-screen">
        <div class="splash-content">
            <!-- أيقونة الشيف (أدوات المطبخ) -->
            <i class="fas fa-utensils splash-icon"></i>
            <div class="loader-text">
                <span>D</span><span>U</span><span>K</span><span>A</span>
            </div>
        </div>
    </div>

    <div id="main-content">
        
        <!-- صفحة البداية -->
        <div id="start-page" class="page active container">
            <div class="logo-circle"><p style="color:#d63031; font-weight:bold; font-size:12px;">نقاط</p></div>
            <h2>مرحباً بك في دوكا</h2>
            <p style="font-size: 13px; margin: 10px 0;">هنا دوكا حيث الوطن</p>
            <button class="btn btn-white" onclick="showPage('signup-page')">إنشاء حساب جديد</button>
            <button class="btn btn-red" onclick="showPage('login-page')">تسجيل الدخول</button>
        </div>

        <!-- صفحة إنشاء حساب -->
        <div id="signup-page" class="page container">
            <h3>إنشاء حساب جديد</h3>
            <input type="text" id="regName" placeholder="الاسم بالكامل">
            <input type="email" id="regEmail" placeholder="البريد الإلكتروني">
            <input type="tel" id="regPhone" placeholder="رقم الموبايل">
            <button class="btn btn-red" onclick="handleSignup()">إتمام التسجيل</button>
            <p class="back-link" onclick="showPage('start-page')">رجوع</p>
        </div>

        <!-- صفحة تسجيل الدخول -->
        <div id="login-page" class="page container">
            <h3>تسجيل الدخول</h3>
            <input type="email" id="loginEmail" placeholder="البريد الإلكتروني">
            <input type="tel" id="loginPhone" placeholder="رقم الموبايل">
            <button class="btn btn-red" onclick="handleLogin()">دخول</button>
            <p class="back-link" onclick="showPage('start-page')">رجوع</p>
        </div>

        <!-- لوحة التحكم -->
        <div id="dashboard-page" class="page container">
            <h3 id="welcomeUser">مرحباً</h3>
            <div class="points-display">
                <p>رصيد الكاش باك الحالي</p>
                <div class="points-num" id="userPoints">0</div>
                <p>جنيه مصري</p>
            </div>
            <div style="background:rgba(255,255,255,0.1); padding:10px; border-radius:10px;">
                <label style="font-size:12px;">سجل قيمة الأوردر:</label>
                <input type="number" id="orderVal" placeholder="المبلغ بالجنية">
                <button class="btn btn-white" onclick="addOrder()">إضافة كاش باك (5%)</button>
            </div>
            <button class="btn btn-red" onclick="window.location.href='https://duka7.odoo.com'">الذهاب للمتجر 🛒</button>
            <p class="back-link" onclick="logout()">خروج</p>
        </div>

    </div>

    <script>
        // 1. شاشة البداية
        window.addEventListener('load', () => {
            setTimeout(() => {
                const splash = document.getElementById('splash-screen');
                const main = document.getElementById('main-content');
                
                splash.style.opacity = '0';
                setTimeout(() => {
                    splash.style.display = 'none';
                    main.style.display = 'flex';
                }, 800);
            }, 2500); // 2.5 ثانية مدة الحركة
        });

        // 2. التنقل بين الصفحات
        function showPage(id) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        }

        // 3. إنشاء حساب
        function handleSignup() {
            const name = document.getElementById('regName').value.trim();
            const email = document.getElementById('regEmail').value.trim();
            const phone = document.getElementById('regPhone').value.trim();

            if (!name || !email || !phone) {
                alert("برجاء إكمال جميع البيانات");
                return;
            }

            if (localStorage.getItem(email)) {
                alert("هذا البريد مسجل بالفعل، يرجى تسجيل الدخول");
                return;
            }

            const user = {
                name: name,
                email: email,
                phone: phone,
                points: 0
            };

            localStorage.setItem(email, JSON.stringify(user));
            loginUser(user);
        }

        // 4. تسجيل الدخول
        function handleLogin() {
            const email = document.getElementById('loginEmail').value.trim();
            const phone = document.getElementById('loginPhone').value.trim();

            if (!email || !phone) {
                alert("برجاء إدخال البريد الإلكتروني ورقم الموبايل");
                return;
            }

            const savedData = localStorage.getItem(email);
            if (!savedData) {
                alert("الحساب غير موجود، يرجى إنشاء حساب أولاً");
                return;
            }

            const user = JSON.parse(savedData);
            if (user.phone === phone) {
                loginUser(user);
            } else {
                alert("رقم الموبايل غير صحيح");
            }
        }

        // 5. تسجيل دخول المستخدم
        function loginUser(user) {
            sessionStorage.setItem('currentUser', user.email);
            document.getElementById('welcomeUser').innerText = "أهلاً " + user.name;
            document.getElementById('userPoints').innerText = user.points.toFixed(2);
            showPage('dashboard-page');
        }

        // 6. إضافة كاش باك
        function addOrder() {
            const amount = parseFloat(document.getElementById('orderVal').value);
            if (isNaN(amount) || amount <= 0) {
                alert("برجاء إدخال مبلغ صحيح");
                return;
            }

            const email = sessionStorage.getItem('currentUser');
            if (!email) {
                alert("يجب تسجيل الدخول أولاً");
                logout();
                return;
            }

            const user = JSON.parse(localStorage.getItem(email));
            user.points += amount * 0.05;
            localStorage.setItem(email, JSON.stringify(user));
            document.getElementById('userPoints').innerText = user.points.toFixed(2);
            document.getElementById('orderVal').value = "";
            alert("تم إضافة الكاش باك بنجاح!");
        }

        // 7. تسجيل الخروج
        function logout() {
            sessionStorage.clear();
            showPage('start-page');
        }

        // 8. التحقق من وجود جلسة مسبقة
        window.addEventListener('load', function() {
            const currentEmail = sessionStorage.getItem('currentUser');
            if (currentEmail) {
                const userData = localStorage.getItem(currentEmail);
                if (userData) {
                    const user = JSON.parse(userData);
                    loginUser(user);
                } else {
                    sessionStorage.clear();
                }
            }
        });
    </script>
</body>
</html>
