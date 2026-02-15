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
