<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>نظام الكاش باك والنقاط</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 40px;
            background-color: #f4f4f9;
            color: #333;
            line-height: 1.6;
        }
        .container {
            max-width: 600px;
            margin: auto;
            background: #fff;
            padding: 20px 30px;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        h2 {
            color: #007bff;
            border-bottom: 2px solid #007bff;
            padding-bottom: 10px;
            margin-bottom: 20px;
        }
        input[type="number"], button {
            width: 100%;
            padding: 12px;
            margin: 8px 0;
            display: inline-block;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
            font-size: 16px;
        }
        /* تنسيق الأزرار الأساسي */
        button {
            background-color: #28a745;
            color: white;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #1e7e34;
        }
        /* تنسيق زر مسح النقاط الجديد */
        .clear-button {
            background-color: #dc3545 !important;
            margin-top: 15px; /* لإضافة فاصل بصري */
        }
        .clear-button:hover {
            background-color: #c82333 !important;
        }
        .result-box {
            background-color: #e9ecef;
            padding: 15px;
            border-radius: 5px;
            margin-top: 15px;
        }
        .counter {
            font-size: 2.5em;
            font-weight: bold;
            color: #dc3545;
            display: block;
            margin-top: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>💰 نظام الكاش باك والنقاط</h2>

        <label for="orderValue">قيمة الأوردر (بالجنيه المصري):</label>
        <input type="number" id="orderValue" placeholder="أدخل قيمة الأوردر" min="0" value="0">

        <button onclick="addPoints()">إضافة النقاط و تحديث السجل</button>
        <button onclick="redeemPoints()" style="background-color: #ffc107; color: #333;">تحويل النقاط إلى كاش باك</button>
        
        <button onclick="clearPoints()" class="clear-button">🗑️ مسح جميع النقاط (تصفير)</button>

        <div class="result-box">
            <h3>سجل النقاط:</h3>
            <p>إجمالي نقاطك الحالية:</p>
            <span id="totalPointsDisplay" class="counter">0</span> نقطة

            <p style="margin-top: 10px;">ما يعادل كاش باك بقيمة:</p>
            <span id="cashbackValue" class="counter">0.00</span> جنيه مصري
        </div>

        <div class="result-box" style="margin-top: 25px;">
            <h3>نقاط وكاش باك الأوردر الأخير:</h3>
            <p id="newPoints">نقاط الأوردر الأخير: 0 نقطة</p>
            <p id="newCashback">كاش باك الأوردر الأخير: 0.00 جنيه</p>
        </div>
    </div>

    <script>
        const pointsPer100EGP = 10;
        const valuePerPoint = 0.5; // جنيه مصري

        // دالة تحديث العرض (لتجنب التكرار وللاستخدام عند التحميل)
        function updateDisplay(points) {
            const cashback = (points * valuePerPoint).toFixed(2);
            document.getElementById('totalPointsDisplay').textContent = points;
            document.getElementById('cashbackValue').textContent = cashback;
        }
        
        // تحميل النقاط المحفوظة من Local Storage عند بدء التشغيل
        let totalPoints = parseInt(localStorage.getItem('userPoints')) || 0;
        updateDisplay(totalPoints);


        /**
         * وظيفة إضافة نقاط الأوردر الجديد
         */
        function addPoints() {
            const orderInput = document.getElementById('orderValue');
            const orderValue = parseFloat(orderInput.value);

            if (isNaN(orderValue) || orderValue < 0) {
                alert('من فضلك أدخل قيمة أوردر صحيحة وموجبة.');
                return;
            }

            // الحساب: كل 100 جنيه بـ 10 نقاط (يتم استخدام Math.floor للحصول على نقاط المئات الكاملة فقط)
            const newPoints = Math.floor(orderValue / 100) * pointsPer100EGP;

            document.getElementById('newPoints').textContent = `نقاط الأوردر الأخير: ${newPoints} نقطة`;
            document.getElementById('newCashback').textContent = `كاش باك الأوردر الأخير: ${(newPoints * valuePerPoint).toFixed(2)} جنيه`;

            totalPoints += newPoints;
            localStorage.setItem('userPoints', totalPoints);

            animateCounter(totalPoints);

            orderInput.value = 0;
        }


        /**
         * وظيفة تحويل (استبدال) النقاط إلى كاش باك
         */
        function redeemPoints() {
            if (totalPoints <= 0) {
                alert('ليس لديك نقاط لاستبدالها!');
                return;
            }

            const redeemedCashback = (totalPoints * valuePerPoint).toFixed(2);
            const confirmation = confirm(`هل أنت متأكد من تحويل ${totalPoints} نقطة إلى كاش باك بقيمة ${redeemedCashback} جنيه مصري؟`);

            if (confirmation) {
                totalPoints = 0;
                localStorage.setItem('userPoints', totalPoints);
                animateCounter(totalPoints);
                document.getElementById('newPoints').textContent = `نقاط الأوردر الأخير: 0 نقطة`;
                document.getElementById('newCashback').textContent = `كاش باك الأوردر الأخير: 0.00 جنيه`;

                alert(`تم تحويل ${redeemedCashback} جنيه بنجاح! رصيد نقاطك الحالي هو 0.`);
            }
        }


        /**
         * وظيفة مسح جميع النقاط
         */
        function clearPoints() {
            if (totalPoints <= 0) {
                alert('لا توجد نقاط لمسحها!');
                return;
            }
            
            const confirmation = confirm('⚠️ هل أنت متأكد من مسح جميع النقاط؟ لا يمكن التراجع عن هذا الإجراء.');
            
            if (confirmation) {
                totalPoints = 0;
                localStorage.setItem('userPoints', totalPoints); 
                animateCounter(totalPoints);
                document.getElementById('newPoints').textContent = `نقاط الأوردر الأخير: 0 نقطة`;
                document.getElementById('newCashback').textContent = `كاش باك الأوردر الأخير: 0.00 جنيه`;

                alert('تم مسح جميع النقاط بنجاح. رصيد نقاطك الحالي هو 0.');
            }
        }


        /**
         * وظيفة عرض النقاط بشكل عداد متحرك
         */
        function animateCounter(finalValue) {
            const display = document.getElementById('totalPointsDisplay');
            const cashbackDisplay = document.getElementById('cashbackValue');
            let startValue = parseInt(display.textContent); 
            const duration = 1500; 
            let startTime;

            function step(timestamp) {
                if (!startTime) startTime = timestamp;
                const progress = timestamp - startTime;
                const percentage = Math.min(progress / duration, 1);
                
                // حساب القيمة الحالية للعداد
                const currentValue = Math.floor(startValue + (finalValue - startValue) * percentage);
                const currentCashback = (currentValue * valuePerPoint).toFixed(2);

                // تحديث العرض
                display.textContent = currentValue;
                cashbackDisplay.textContent = currentCashback;

                // استمرار الحركة حتى الاكتمال
                if (percentage < 1) {
                    window.requestAnimationFrame(step);
                }
            }

            // بدء حركة العداد
            if (startValue !== finalValue) {
                window.requestAnimationFrame(step);
            } else {
                // تحديث مباشر إذا لم يكن هناك تغيير
                updateDisplay(finalValue);
            }
        }
    </script>
</body>
</html>
