<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مكتبة حبيب قدوس الرقمية ✝️</title>
    <style>
        /* تنسيقات حبيب قدوس - نسخة نظيفة واحترافية */
        body {
            background-color: #f4f6f9;
            direction: rtl;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
        }

        .container {
            max-width: 600px;
            margin: 40px auto;
            background: #ffffff;
            padding: 35px;
            border-radius: 25px;
            box-shadow: 0 15px 45px rgba(0,0,0,0.1);
            text-align: center;
            border-top: 10px solid #4a148c;
        }

        .header-title {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 15px;
            margin-bottom: 5px;
        }

        .header-title h1 {
            color: #4a148c;
            font-size: 28px;
            margin: 0;
        }

        .cross {
            color: #b71c1c;
            font-size: 35px;
            font-weight: bold;
        }

        .subtitle {
            color: #777;
            font-size: 15px;
            margin-bottom: 30px;
        }

        /* تصميم صندوق البحث والعدسة */
        .search-area {
            position: relative;
            display: flex;
            align-items: center;
        }

        input {
            width: 100%;
            padding: 15px 20px 15px 60px;
            border-radius: 30px;
            border: 2px solid #4a148c;
            font-size: 18px;
            outline: none;
            transition: all 0.3s ease;
        }

        input:focus {
            border-color: #7b1fa2;
            box-shadow: 0 0 10px rgba(123, 31, 162, 0.2);
        }

        .search-btn {
            position: absolute;
            left: 7px;
            background: #4a148c;
            border: none;
            width: 45px;
            height: 45px;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: 0.3s;
        }

        .search-btn:hover {
            background: #6a1b9a;
            transform: scale(1.05);
        }

        /* تنسيق نتائج البحث */
        #results {
            margin-top: 25px;
            text-align: right;
        }

        .verse-card {
            background: #fffdf7;
            padding: 18px;
            border-radius: 12px;
            border-right: 6px solid #b71c1c;
            margin-bottom: 15px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            animation: fadeIn 0.5s ease-in-out;
        }

        .ref {
            font-weight: bold;
            color: #e65100;
            display: block;
            margin-bottom: 8px;
            font-size: 14px;
        }

        .text {
            font-size: 19px;
            color: #2c3e50;
            line-height: 1.7;
        }

        .info-msg {
            color: #4a148c;
            font-weight: bold;
            margin-top: 20px;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body>

    <div class="container">
        <div class="header-title">
            <span class="cross">†</span>
            <h1>مكتبة حبيب قدوس</h1>
            <span class="cross">†</span>
        </div>
        <p class="subtitle">بحث شامل في العهدين القديم والجديد</p>

        <div class="search-area">
            <input type="text" id="bibleInput" placeholder="اكتب كلمة (مثلاً: يسوع، المحبة)...">
            <button class="search-btn" id="searchBtn">
                <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="3" stroke-linecap="round" stroke-linejoin="round">
                    <circle cx="11" cy="11" r="8"></circle>
                    <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
                </svg>
            </button>
        </div>

        <div id="status"></div>
        <div id="results"></div>
    </div>

    <script>
        const btn = document.getElementById('searchBtn');
        const input = document.getElementById('bibleInput');
        const resDiv = document.getElementById('results');
        const status = document.getElementById('status');

        async function fetchBible() {
            const query = input.value.trim();
            if (!query) return;

            status.innerHTML = "<p class='info-msg'>🔍 جاري البحث في الأسفار المقدسة...</p>";
            resDiv.innerHTML = "";

            try {
                // الربط مع قاعدة البيانات الشاملة
                const response = await fetch(`https://bolls.life/get-search/?search=${encodeURIComponent(query)}&translation=SVD`);
                const data = await response.json();

                status.innerHTML = "";

                if (!data || data.length === 0) {
                    resDiv.innerHTML = "<p>لم نجد نتائج لهذه الكلمة، حاول مرة أخرى.</p>";
                    return;
                }

                // عرض أول 40 نتيجة لسرعة الأداء
                data.slice(0, 40).forEach(v => {
                    const card = document.createElement('div');
                    card.className = 'verse-card';
                    card.innerHTML = `
                        <span class="ref">${v.book_name} ${v.chapter}:${v.verse}</span>
                        <div class="text">${v.text}</div>
                    `;
                    resDiv.appendChild(card);
                });

            } catch (err) {
                status.innerHTML = "<p style='color:red;'>⚠️ حدث خطأ في الاتصال، حاول مجدداً.</p>";
            }
        }

        btn.addEventListener('click', fetchBible);
        input.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') fetchBible();
        });
    </script>
</body>
</html>
