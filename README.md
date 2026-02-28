<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Duka Points | نقاط Duka</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Cairo', sans-serif; background: #fef7e9; min-height: 100vh; color: #2d3e50; }

        /* الشاشة الافتتاحية */
        #splash-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100vh;
            background: #000;
            display: flex; justify-content: center; align-items: center; z-index: 10000; transition: opacity 0.5s ease;
            overflow: hidden;
        }

        /* حاوية الفيديو لضمان ملء الشاشة */
        .video-background {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            z-index: -1;
            pointer-events: none;
        }

        #splash-video {
            position: absolute;
            top: 50%;
            left: 50%;
            width: 100vw;
            height: 56.25vw; /* نسبة 16:9 */
            min-height: 100vh;
            min-width: 177.77vh; /* لضمان تغطية الارتفاع */
            transform: translate(-50%, -50%) scale(1.2); /* تكبير بسيط لضمان عدم وجود حواف */
            border: 0;
        }

        .overlay {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.45);
            z-index: 0;
        }

        .splash-content { 
            position: relative;
            z-index: 1;
            width: 100%; 
            max-width: 450px; 
            padding: 20px; 
            text-align: center; 
            color: white; 
        }
        
        .duka-logo-circle {
            background: white; border-radius: 50%; width: 130px; height: 130px; margin: 0 auto 30px;
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            border: 6px solid #333; box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }
        .duka-logo-circle .pts { color: #d31111; font-size: 30px; font-weight: 900; line-height: 1; }
        .duka-logo-circle .brand { color: #333; font-size: 22px; font-weight: 900; }

        .main-title { font-size: 30px; font-weight: 700; margin-bottom: 15px; line-height: 1.3; text-shadow: 2px 2px 4px rgba(0,0,0,0.6); }
        .description { font-size: 16px; line-height: 1.6; margin-bottom: 35px; opacity: 0.95; text-shadow: 1px 1px 2px rgba(0,0,0,0.6); }

        .btn-splash-white { background: white; color: #d31111; border: none; padding: 16px; border-radius: 15px; font-size: 18px; font-weight: 700; cursor: pointer; width: 100%; margin-bottom: 10px; }
        .btn-splash-red { background: #ff0000; color: white; border: none; padding: 16px; border-radius: 15px; font-size: 18px; font-weight: 700; cursor: pointer; width: 100%; }

        /* باقي المحتوى الأصلي */
        #main-content { display: none; padding: 16px; max-width: 500px; margin: 0 auto; }
        .page { display: none; animation: slideUp 0.4s ease; }
        .page.active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .food-card { background: white; border-radius: 25px; padding: 20px; margin-bottom: 20px; box-shadow: 0 5px 15px rgba(0,0,0,0.05); }
        .input-group { margin-bottom: 15px; }
        .input-group label { display: block; margin-bottom: 8px; font-weight: 600; }
        .input-group input { width: 100%; padding: 15px; border: 2px solid #eee; border-radius: 15px; font-family: 'Cairo'; font-size: 16px; }
        .points-card { background: linear-gradient(145deg, #fff3e0, #ffe9d6); border-radius: 25px; padding: 25px; display: flex; align-items: center; justify-content: space-between; border: 2px solid #d31111; margin-bottom: 20px; position: relative; }
        .points-number { font-size: 42px; font-weight: 900; color: #2d3e50; }
        .points-icon { width: 60px; height: 60px; background: #d31111; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-size: 25px; }
        .btn-action { background: #d31111; color: white; border: none; padding: 16px; border-radius: 60px; width: 100%; font-size: 18px; font-weight: 700; cursor: pointer; margin-top: 10px; display: flex; align-items: center; justify-content: center; gap: 10px; }
        .btn-reset { background: #666; color: white; border: none; padding: 6px 15px; border-radius: 20px; font-size: 13px; font-weight: 600; cursor: pointer; margin-top: 12px; display: inline-flex; align-items: center; gap: 6px; }
        .back-link { text-align: center; margin-top: 20px; color: #d31111; cursor: pointer; font-weight: 600; display: block; }
        .alert { position: fixed; top: 20px; left: 50%; transform: translateX(-50%) translateY(-100px); background: white; padding: 12px 25px; border-radius: 60px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); z-index: 10001; transition: 0.3s; border-right: 5px solid #d31111; font-weight: 600; }
        .alert.show { transform: translateX(-50%) translateY(0); }
    </style>
</head>
<body>

    <div id="customAlert" class="alert"></div>

    <div id="splash-screen">
        <div class="video-background">
            <iframe id="splash-video" 
                src="https://www.youtube.com/embed/WW0SLuX8HsI?autoplay=1&mute=1&controls=0&loop=1&playlist=WW0SLuX8HsI&start=5&modestbranding=1&showinfo=0&rel=0&enablejsapi=1" 
                frameborder="0" 
                allow="autoplay; encrypted-media">
            </iframe>
        </div>
        <div class="overlay"></div>

        <div class="splash-content">
            <div class="duka-logo-circle">
                <span class="pts">نقاط</span>
                <span class="brand">DUKA</span>
            </div>
            <h1 class="main-title">أهلاً بك في برنامج نقاط عالم Duka</h1>
            <p class="description">سجل حسابك الآن وابدأ بتجميع النقاط عند كل عملية شراء لمنتجات Duka واستبدلها بمكافآت رائعة.</p>
            <button class="btn-splash-white" onclick="showPage('signup-page')">تسجيل حساب جديد</button>
            <button class="btn-splash-red" onclick="showPage('login-page')">تسجيل الدخول</button>
        </div>
    </div>

    <div id="main-content">
        <div id="signup-page" class="page">
            <div class="food-card">
                <h2 style="text-align: center; margin-bottom: 20px; color:#d31111">حساب جديد</h2>
                <div class="input-group">
                    <label>الاسم</label>
                    <input type="text" id="regName" placeholder="مثال: محمد علي">
                </div>
                <div class="input-group">
                    <label>البريد الإلكتروني</label>
                    <input type="email" id="regEmail" placeholder="example@mail.com">
                </div>
                <div class="input-group">
                    <label>رقم الموبايل</label>
                    <input type="tel" id="regPhone" placeholder="01xxxxxxxxx">
                </div>
                <button class="btn-action" onclick="handleSignup()">إتمام التسجيل</button>
                <a class="back-link" onclick="location.reload()">رجوع</a>
            </div>
        </div>

        <div id="login-page" class="page">
            <div class="food-card">
                <h2 style="text-align: center; margin-bottom: 20px; color:#d31111">تسجيل دخول</h2>
                <div class="input-group">
                    <label>البريد الإلكتروني</label>
                    <input type="email" id="loginEmail" placeholder="example@mail.com">
                </div>
                <div class="input-group">
                    <label>رقم الموبايل</label>
                    <input type="tel" id="loginPhone" placeholder="01xxxxxxxxx">
                </div>
                <button class="btn-action" onclick="handleLogin()">دخول</button>
                <a class="back-link" onclick="location.reload()">رجوع</a>
            </div>
        </div>

        <div id="dashboard-page" class="page">
            <div class="points-card">
                <div>
                    <h3 style="color: #d31111; margin-bottom: 5px;">رصيد الكاش باك</h3>
                    <span class="points-number" id="cashbackBalance">0.00</span> جنيه
                    <br>
                    <button class="btn-reset" onclick="resetBalance()">
                        <i class="fas fa-sync-alt"></i> إعادة تعيين الرصيد
                    </button>
                </div>
                <div class="points-icon"><i class="fas fa-coins"></i></div>
            </div>
            <div class="food-card" style="text-align: center;">
                <h3 id="welcomeUser" style="color: #d31111;">أهلاً بك</h3>
                <p style="color: #777;">كل طلب يزيد رصيدك 5%</p>
            </div>
            <div class="food-card">
                <h3 style="margin-bottom: 15px;"><i class="fas fa-cart-plus"></i> إضافة طلبية</h3>
                <div style="display: flex; gap: 10px; margin-bottom: 15px;">
                    <input type="number" id="orderAmount" value="100" style="flex:1; padding:12px; border-radius:10px; border:1px solid #ddd;">
                    <button onclick="addOrder()" style="padding:10px 20px; background:#d31111; color:white; border:none; border-radius:10px;"><i class="fas fa-plus"></i></button>
                </div>
                <!-- الرابط المعدل حسب طلبك: https://duka.vatrin.app -->
                <a href="https://duka.vatrin.app" target="_blank" style="text-decoration:none; background:#2d3e50; color:white; padding:12px; display:block; text-align:center; border-radius:10px;">تسوق من المتجر</a>
            </div>
            <button class="back-link" onclick="logout()" style="width:100%; border:none; background:none;">تسجيل خروج</button>
        </div>
    </div>

    <script>
        function showPage(pageId) {
            document.getElementById('splash-screen').style.opacity = '0';
            const iframe = document.getElementById('splash-video');
            if(iframe) iframe.src = ''; 
            setTimeout(() => {
                document.getElementById('splash-screen').style.display = 'none';
                document.getElementById('main-content').style.display = 'block';
                document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
                document.getElementById(pageId).classList.add('active');
            }, 500);
        }

        function showAlert(msg) {
            const alert = document.getElementById('customAlert');
            alert.textContent = msg;
            alert.classList.add('show');
            setTimeout(() => alert.classList.remove('show'), 2500);
        }

        function handleSignup() {
            const name = document.getElementById('regName').value.trim();
            const email = document.getElementById('regEmail').value.trim();
            const phone = document.getElementById('regPhone').value.trim();
            if (!name || !email || !phone) return showAlert('املأ جميع البيانات');
            if (localStorage.getItem(email)) return showAlert('البريد مسجل مسبقاً');
            const user = { name, email, phone, points: 0 };
            localStorage.setItem(email, JSON.stringify(user));
            loginUser(user);
        }

        function handleLogin() {
            const email = document.getElementById('loginEmail').value.trim();
            const phone = document.getElementById('loginPhone').value.trim();
            const savedUser = localStorage.getItem(email);
            if (!savedUser) return showAlert('الحساب غير موجود');
            const user = JSON.parse(savedUser);
            if (user.phone !== phone) return showAlert('رقم الموبايل خطأ');
            loginUser(user);
        }

        function loginUser(user) {
            sessionStorage.setItem('currentUser', user.email);
            document.getElementById('welcomeUser').textContent = `أهلاً ${user.name}`;
            document.getElementById('cashbackBalance').textContent = user.points.toFixed(2);
            showPage('dashboard-page');
        }

        function addOrder() {
            const amount = parseFloat(document.getElementById('orderAmount').value);
            const email = sessionStorage.getItem('currentUser');
            const user = JSON.parse(localStorage.getItem(email));
            user.points += amount * 0.05;
            localStorage.setItem(email, JSON.stringify(user));
            document.getElementById('cashbackBalance').textContent = user.points.toFixed(2);
            showAlert('تمت إضافة الكاش باك!');
        }

        function resetBalance() {
            const email = sessionStorage.getItem('currentUser');
            const user = JSON.parse(localStorage.getItem(email));
            if (confirm("هل تريد تصفير الرصيد؟")) {
                user.points = 0;
                localStorage.setItem(email, JSON.stringify(user));
                document.getElementById('cashbackBalance').textContent = "0.00";
            }
        }

        function logout() { sessionStorage.clear(); location.reload(); }

        window.onload = () => {
            const email = sessionStorage.getItem('currentUser');
            if (email) {
                const user = JSON.parse(localStorage.getItem(email));
                if (user) loginUser(user);
            }
        };
    </script>
</body>
</html>
