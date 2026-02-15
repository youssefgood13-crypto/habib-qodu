<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مكتبة حبيب قدوس الرقمية ✝️</title>
    <style>
        body {
            background: #f0f2f5;
            direction: rtl;
            font-family: 'Segoe UI', Tahoma, sans-serif;
            padding: 20px;
            margin: 0;
        }
        .bible-app { 
            max-width: 550px; 
            margin: 20px auto; 
            background: white; 
            padding: 40px; 
            border-radius: 30px; 
            text-align: center;
            box-shadow: 0 20px 50px rgba(0,0,0,0.15); 
            border-top: 12px solid #4a148c;
        }
        .main-header { 
            display: flex; 
            justify-content: center; 
            align-items: center; 
            gap: 15px; 
            margin-bottom: 10px; 
        }
        .main-header h1 { 
            color: #4a148c; 
            font-size: clamp(20px, 5vw, 32px); 
            margin: 0; 
        }
        .cross { 
            color: #b71c1c; 
            font-size: 40px; 
            font-weight: bold; 
        }
        .subtitle { 
            color: #555; 
            font-size: 16px; 
            margin-bottom: 30px; 
        }
        .search-box { 
            position: relative; 
            display: flex; 
            border-radius: 35px; 
        }
        input { 
            width: 100%; 
            padding: 18px 20px 18px 65px; 
            border-radius: 35px; 
            border: 2px solid #4a148c; 
            outline: none; 
            font-size: 18px; 
        }
        button { 
            position: absolute; 
            left: 8px; 
            top: 50%; 
            transform: translateY(-50%);
            background: #4a148c; 
            border: none; 
            width: 50px; 
            height: 50px; 
            border-radius: 50%; 
            cursor: pointer; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
        }
        #results { 
            margin-top: 20px; 
            text-align: right; 
        }
        .card { 
            background: #fffcf5; 
            padding: 20px; 
            border-radius: 15px; 
            border-right: 8px solid #b71c1c; 
            margin-top: 20px; 
            box-shadow: 0 5px 10px rgba(0,0,0,0.05); 
        }
        .ref { font-weight: bold; color: #e65100; display: block; margin-bottom: 8px; }
        .text { font-size: 20px; color: #2c3e50; line-height: 1.8; }
        .loading { color: #4a148c; font-weight: bold; margin-top: 20px; }
    </style>
</head>
<body>

    <div class="bible-app">
        <div class="main-header">
            <span class="cross">†</span>
            <h1>مكتبة حبيب قدوس الرقمية</h1>
            <span class="cross">†</span>
        </div>
        <p class="subtitle">الباحث الشامل في الكتاب المقدس - العهد القديم والجديد</p>

        <div class="search-box">
            <input type="text" id="bibleInput" placeholder="اكتب كلمة للبحث (مثلاً: يسوع، المحبة)...">
            <button id="searchBtn">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="3">
                    <circle cx="11" cy="11" r="8"></circle>
                    <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
                </svg>
            </button>
        </div>

        <div id="status"></div>
        <div id="results"></div>
    </div>

    <script>
        const searchBtn = document.getElementById('searchBtn');
        const inputField = document.getElementById('bibleInput');
        const resultsArea = document.getElementById('results');
        const status = document.getElementById('status');

        async function doSearch() {
            const query = inputField.value.trim();
            if (!query) return;

            status.innerHTML = "<p class='loading'>جاري البحث في كلمة الله...</p>";
            resultsArea.innerHTML = "";

            try {
                // استخدام API عالمي لجلب كل آيات الكتاب المقدس
                const response = await fetch(`https://bolls.life/get-search/?search=${encodeURIComponent(query)}&translation=SVD`);
                const data = await response.json();

                status.innerHTML = "";

                if (!data || data.length === 0) {
                    resultsArea.innerHTML = "<p>لم يتم العثور على نتائج. جرب كلمة أخرى.</p>";
                    return;
                }

                data.slice(0, 50).forEach(v => {
                    const card = document.createElement('div');
                    card.className = 'card';
                    card.innerHTML = `
                        <span class="ref">سفر ${v.book_name} ${v.chapter}:${v.verse}</span>
                        <div class="text">${v.text}</div>
                    `;
                    resultsArea.appendChild(card);
                });

            } catch (error) {
                status.innerHTML = "<p style='color:red;'>حدث خطأ في الاتصال. تأكد من الإنترنت.</p>";
            }
        }

        searchBtn.addEventListener('click', doSearch);
        inputField.addEventListener('keypress', (e) => { if (e.key === 'Enter') doSearch(); });
    </script>
</body>
</html>
