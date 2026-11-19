# 網站連結

[點我前往課程網站](https://skypai0326.github.io/courses/courses/gemini-ai/chapters/CH1.html)

＃提示詞
[提示詞檢測]
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Prompt 品質健診器</title>
  <!-- 載入 Google Fonts：Shippori Mincho 與 Noto Sans TC -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700&family=Shippori+Mincho:wght@600;700&display=swap" rel="stylesheet">
  <!-- 載入 Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /* 無印良品風格自訂基礎樣式：純色、無陰影、無漸層、圓角 6px */
    body {
      background-color: #f5f3ee;
      color: #3a3834;
      font-family: 'Noto Sans TC', sans-serif;
      -webkit-font-smoothing: antialiased;
    }

    h1, h2, h3, .font-serif-title {
      font-family: 'Shippori Mincho', serif;
    }

    .muji-card {
      background-color: #ffffff;
      border: 1px solid #d8d4cb;
      border-radius: 6px;
    }

    .muji-input {
      background-color: #ffffff;
      border: 1px solid #d8d4cb;
      border-radius: 6px;
      transition: border-color 0.2s ease;
    }

    .muji-input:focus {
      outline: none;
      border-color: #7f7a70;
    }

    .muji-btn-primary {
      background-color: #3a3834;
      color: #ffffff;
      border: 1px solid #3a3834;
      border-radius: 6px;
      transition: background-color 0.2s ease, border-color 0.2s ease;
    }

    .muji-btn-primary:hover {
      background-color: #54514b;
      border-color: #54514b;
    }

    .muji-btn-secondary {
      background-color: #f5f3ee;
      color: #3a3834;
      border: 1px solid #d8d4cb;
      border-radius: 6px;
      transition: background-color 0.2s ease;
    }

    .muji-btn-secondary:hover {
      background-color: #ebe8e0;
    }

    /* 進度條平滑動畫 */
    .progress-fill {
      transition: width 0.55s cubic-bezier(0.22, 1, 0.36, 1);
      border-radius: 6px;
    }

    /* 自訂訊息提示框（取代 alert） */
    .toast-banner {
      border-radius: 6px;
      transition: opacity 0.25s ease, transform 0.25s ease;
    }
  </style>
</head>
<body class="min-h-screen py-8 px-4 sm:px-6 lg:px-8">

  <div class="max-w-4xl mx-auto">
    <!-- 頁首標題區 -->
    <header class="mb-8 border-b border-[#d8d4cb] pb-5 flex flex-col sm:flex-row sm:items-end justify-between gap-3">
      <div>
        <p class="text-xs tracking-widest text-[#7f7a70] mb-1">PROMPT QUALITY DIAGNOSTIC TOOL</p>
        <h1 class="text-2xl sm:text-3xl font-bold text-[#2b2926] tracking-wide">Prompt 品質健診器</h1>
      </div>
      <p class="text-sm text-[#6e6a61]">透過結構化四維度檢視，讓每一次與 AI 的對話更精準。</p>
    </header>

    <!-- 提示訊息區（無 alert 設計） -->
    <div id="messageBox" class="hidden toast-banner mb-5 px-4 py-3 bg-[#ffffff] border border-[#b5703a] text-[#3a3834] text-sm flex items-center justify-between">
      <span id="messageText">請先輸入或貼上您要檢測的 Prompt 內容。</span>
      <button type="button" onclick="hideMessage()" class="text-xs text-[#7f7a70] hover:text-[#2b2926] ml-4 px-2 py-1 border border-[#d8d4cb] rounded-[6px]">關閉</button>
    </div>

    <!-- 主內容區：雙欄響應式配置 -->
    <main class="grid grid-cols-1 lg:grid-cols-12 gap-6">
      
      <!-- 左側：Prompt 輸入區塊 -->
      <section class="lg:col-span-6 muji-card p-5 sm:p-6 flex flex-col justify-between">
        <div>
          <div class="flex items-center justify-between mb-3">
            <label for="promptInput" class="font-serif-title text-lg font-semibold text-[#2b2926]">輸入 Prompt 內容</label>
            <span id="charCount" class="text-xs text-[#7f7a70]">0 字</span>
          </div>
          
          <textarea
            id="promptInput"
            rows="11"
            class="muji-input w-full p-4 text-sm sm:text-base leading-relaxed text-[#2b2926] resize-y"
            placeholder="請在此貼入您撰寫的 Prompt...&#10;&#10;例如：你是一位擁有十年經驗的資深產品經理。請幫我整理這份使用者訪談紀錄的核心痛點，並分析背後的商業機會。請避免使用艱澀的技術術語，字數限制在 500 字以內，並以 Markdown 表格格式呈現。"
          ></textarea>
        </div>

        <!-- 操作按鈕列 -->
        <div class="mt-4 pt-4 border-t border-[#ebe8e0] flex flex-wrap items-center justify-between gap-3">
          <div class="flex items-center gap-2">
            <button type="button" id="sampleBtn" class="muji-btn-secondary px-3 py-2 text-xs sm:text-sm font-medium cursor-pointer">
              填入範例
            </button>
            <button type="button" id="clearBtn" class="muji-btn-secondary px-3 py-2 text-xs sm:text-sm font-medium cursor-pointer">
              清空內容
            </button>
          </div>
          <button type="button" id="diagnoseBtn" class="muji-btn-primary px-6 py-2.5 text-sm sm:text-base font-medium tracking-wider cursor-pointer">
            健診
          </button>
        </div>
      </section>

      <!-- 右側：健診結果與四維度評分區塊 -->
      <section class="lg:col-span-6 muji-card p-5 sm:p-6 flex flex-col justify-between">
        <div>
          <div class="flex items-center justify-between border-b border-[#ebe8e0] pb-4 mb-5">
            <div>
              <h2 class="font-serif-title text-lg font-semibold text-[#2b2926]">綜合診斷結果</h2>
              <p class="text-xs text-[#7f7a70] mt-0.5">滿分 100 分（四個維度各佔 25 分）</p>
            </div>
            
            <!-- 總分與等級顯示 -->
            <div class="flex items-center gap-3">
              <div class="text-right">
                <span id="totalScore" class="font-serif-title text-3xl sm:text-4xl font-bold text-[#2b2926]">--</span>
                <span class="text-xs text-[#7f7a70]">/ 100</span>
              </div>
              <span id="gradeBadge" class="px-3 py-1 text-xs sm:text-sm font-medium border border-[#d8d4cb] bg-[#f5f3ee] text-[#7f7a70] rounded-[6px]">
                尚未檢測
              </span>
            </div>
          </div>

          <!-- 四個評分條 -->
          <div class="space-y-4">
            <!-- ① 角色清晰度 (#b5703a) -->
            <div>
              <div class="flex justify-between items-center text-sm mb-1.5">
                <span class="font-medium text-[#2b2926] flex items-center gap-2">
                  <span class="inline-block w-2.5 h-2.5 rounded-[2px]" style="background-color: #b5703a;"></span>
                  ① 角色清晰度
                </span>
                <span class="text-xs font-medium text-[#54514b]"><span id="scoreRole">0</span> / 25 分</span>
              </div>
              <div class="w-full h-3 bg-[#f5f3ee] border border-[#d8d4cb] rounded-[6px] overflow-hidden">
                <div id="barRole" class="progress-fill h-full" style="width: 0%; background-color: #b5703a;"></div>
              </div>
              <p id="matchRole" class="text-[11px] text-[#7f7a70] mt-1">偵測關鍵字：你是、扮演、角色、身分...</p>
            </div>

            <!-- ② 任務明確度 (#c9963a) -->
            <div>
              <div class="flex justify-between items-center text-sm mb-1.5">
                <span class="font-medium text-[#2b2926] flex items-center gap-2">
                  <span class="inline-block w-2.5 h-2.5 rounded-[2px]" style="background-color: #c9963a;"></span>
                  ② 任務明確度
                </span>
                <span class="text-xs font-medium text-[#54514b]"><span id="scoreTask">0</span> / 25 分</span>
              </div>
              <div class="w-full h-3 bg-[#f5f3ee] border border-[#d8d4cb] rounded-[6px] overflow-hidden">
                <div id="barTask" class="progress-fill h-full" style="width: 0%; background-color: #c9963a;"></div>
              </div>
              <p id="matchTask" class="text-[11px] text-[#7f7a70] mt-1">偵測關鍵字：寫、整理、分析、生成、製作...</p>
            </div>

            <!-- ③ 限制完整度 (#5a7a5a) -->
            <div>
              <div class="flex justify-between items-center text-sm mb-1.5">
                <span class="font-medium text-[#2b2926] flex items-center gap-2">
                  <span class="inline-block w-2.5 h-2.5 rounded-[2px]" style="background-color: #5a7a5a;"></span>
                  ③ 限制完整度
                </span>
                <span class="text-xs font-medium text-[#54514b]"><span id="scoreConstraint">0</span> / 25 分</span>
              </div>
              <div class="w-full h-3 bg-[#f5f3ee] border border-[#d8d4cb] rounded-[6px] overflow-hidden">
                <div id="barConstraint" class="progress-fill h-full" style="width: 0%; background-color: #5a7a5a;"></div>
              </div>
              <p id="matchConstraint" class="text-[11px] text-[#7f7a70] mt-1">偵測關鍵字：不要、避免、限制、字數、語氣...</p>
            </div>

            <!-- ④ 呈現具體度 (#6b7fa3) -->
            <div>
              <div class="flex justify-between items-center text-sm mb-1.5">
                <span class="font-medium text-[#2b2926] flex items-center gap-2">
                  <span class="inline-block w-2.5 h-2.5 rounded-[2px]" style="background-color: #6b7fa3;"></span>
                  ④ 呈現具體度
                </span>
                <span class="text-xs font-medium text-[#54514b]"><span id="scoreFormat">0</span> / 25 分</span>
              </div>
              <div class="w-full h-3 bg-[#f5f3ee] border border-[#d8d4cb] rounded-[6px] overflow-hidden">
                <div id="barFormat" class="progress-fill h-full" style="width: 0%; background-color: #6b7fa3;"></div>
              </div>
              <p id="matchFormat" class="text-[11px] text-[#7f7a70] mt-1">偵測關鍵字：格式、條列、表格、範本、範例...</p>
            </div>
          </div>
        </div>

        <!-- 等級基準說明小字 -->
        <div class="mt-6 pt-3 border-t border-[#ebe8e0] flex flex-wrap justify-between text-[11px] text-[#7f7a70] gap-2">
          <span>評級標準：</span>
          <span>90+ 優秀</span>
          <span>70–89 良好</span>
          <span>50–69 尚可</span>
          <span>50 以下 需加強</span>
        </div>
      </section>

      <!-- 下方全寬：具體改善建議卡片 -->
      <section class="lg:col-span-12 muji-card p-5 sm:p-6">
        <div class="flex items-center justify-between mb-4">
          <h2 class="font-serif-title text-lg font-semibold text-[#2b2926]">具體改善建議</h2>
          <span id="suggestionStatus" class="text-xs text-[#7f7a70]">按下「健診」後將依據低分項目提供優化方向</span>
        </div>
        
        <div id="suggestionsContainer" class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div class="p-4 bg-[#f5f3ee] border border-[#d8d4cb] rounded-[6px] text-sm text-[#6e6a61] md:col-span-2 text-center py-6">
            尚未進行診斷。請在上方輸入您的 Prompt 並點擊「健診」按鈕。
          </div>
        </div>
      </section>

    </main>

    <!-- 頁尾 -->
    <footer class="mt-8 text-center text-xs text-[#7f7a70]">
      Prompt 結構化品質評估工具・簡約無印設計
    </footer>
  </div>

  <script>
    /**
     * 四個評估維度的配置與關鍵字規則
     * 每個維度滿分為 25 分
     */
    const DIMENSIONS = [
      {
        id: 'role',
        name: '① 角色清晰度',
        color: '#b5703a',
        scoreEl: document.getElementById('scoreRole'),
        barEl: document.getElementById('barRole'),
        matchEl: document.getElementById('matchRole'),
        defaultHint: '偵測關鍵字：你是、扮演、角色、身分...',
        primaryKeywords: ['你是', '扮演', '角色', '身分', '身份', '身為'],
        secondaryKeywords: ['專家', '顧問', '老師', '工程師', '分析師', '編輯', '助理', '立場', '視角'],
        suggestionTitle: '補強「角色清晰度」：賦予 AI 明確的專業視角',
        suggestionDesc: '目前缺少明確的角色設定。建議在開頭加入「你是一位擁有 10 年經驗的〇〇專家」或「請扮演〇〇角色」，能有效收斂 AI 的知識庫範圍與回答專業度。',
        exampleSnippet: '「你是一位資深商業分析師，擅長從使用者回饋中提煉產品策略。」'
      },
      {
        id: 'task',
        name: '② 任務明確度',
        color: '#c9963a',
        scoreEl: document.getElementById('scoreTask'),
        barEl: document.getElementById('barTask'),
        matchEl: document.getElementById('matchTask'),
        defaultHint: '偵測關鍵字：寫、整理、分析、生成、製作...',
        primaryKeywords: ['寫', '整理', '分析', '生成', '製作'],
        secondaryKeywords: ['撰寫', '歸納', '總結', '評估', '翻譯', '比較', '設計', '規劃', '改寫', '提供', '列出', '解釋'],
        suggestionTitle: '強化「任務明確度」：使用具體的行動動詞指令',
        suggestionDesc: '任務指令較為模糊或缺乏明確動詞。建議明確指出希望 AI 執行的動作，例如使用「整理」、「分析」、「撰寫」、「生成」或「製作」等具體動詞，並說明處理對象。',
        exampleSnippet: '「請分析以下三份會議紀錄，並整理出五項最急迫的待辦事項。」'
      },
      {
        id: 'constraint',
        name: '③ 限制完整度',
        color: '#5a7a5a',
        scoreEl: document.getElementById('scoreConstraint'),
        barEl: document.getElementById('barConstraint'),
        matchEl: document.getElementById('matchConstraint'),
        defaultHint: '偵測關鍵字：不要、避免、限制、字數、語氣...',
        primaryKeywords: ['不要', '避免', '限制', '字數', '語氣'],
        secondaryKeywords: ['不可', '禁止', '勿', '僅限', '控制在', '以內', '左右', '風格', '口吻', '對象', '受眾', '條件'],
        suggestionTitle: '補充「限制完整度」：設定邊界條件與語氣規範',
        suggestionDesc: '缺少對輸出內容的限制條件，容易導致 AI 回答過長或偏離預期。建議加入「字數限制」、「語氣要求（如客觀、白話）」或「避免出現的內容」。',
        exampleSnippet: '「語氣請保持客觀專業，避免使用艱澀術語，總字數限制在 300 字以內。」'
      },
      {
        id: 'format',
        name: '④ 呈現具體度',
        color: '#6b7fa3',
        scoreEl: document.getElementById('scoreFormat'),
        barEl: document.getElementById('barFormat'),
        matchEl: document.getElementById('matchFormat'),
        defaultHint: '偵測關鍵字：格式、條列、表格、範本、範例...',
        primaryKeywords: ['格式', '條列', '表格', '範本', '範例'],
        secondaryKeywords: ['清單', '項目符號', '分段', '標題', 'Markdown', 'JSON', '步驟', '結構', '模板', '輸出如下'],
        suggestionTitle: '提升「呈現具體度」：指定結構化的輸出排版',
        suggestionDesc: '未指定輸出排版方式，AI 可能會回傳一大段難以閱讀的純文字。建議明確要求使用「條列式」、「表格」呈現，或提供一段「輸出範例／範本」供其依循。',
        exampleSnippet: '「請以 Markdown 表格格式輸出，欄位包含：問題點、影響程度、建議對策。」'
      }
    ];

    const promptInput = document.getElementById('promptInput');
    const charCount = document.getElementById('charCount');
    const diagnoseBtn = document.getElementById('diagnoseBtn');
    const clearBtn = document.getElementById('clearBtn');
    const sampleBtn = document.getElementById('sampleBtn');
    const messageBox = document.getElementById('messageBox');
    const messageText = document.getElementById('messageText');

    // 即時更新字數統計
    promptInput.addEventListener('input', () => {
      const len = promptInput.value.trim().length;
      charCount.textContent = `${len} 字`;
      if (len > 0) hideMessage();
    });

    // 顯示提示訊息（取代 alert）
    function showMessage(text) {
      messageText.textContent = text;
      messageBox.classList.remove('hidden');
    }

    function hideMessage() {
      messageBox.classList.add('hidden');
    }

    /**
     * 計算單一維度得分（0～25 分）
     * 邏輯：
     * - 偵測到核心關鍵字：第 1 個給 16 分，每多 1 個不同核心詞加 4.5 分（最高 25 分）
     * - 偵測到輔助語意詞：每個給 4 分（若無核心詞最高累積至 14 分，若有核心詞可補足至 25 分）
     */
    function evaluateDimension(text, dim) {
      const matchedPrimary = dim.primaryKeywords.filter(kw => text.includes(kw));
      const matchedSecondary = dim.secondaryKeywords.filter(kw => text.includes(kw));

      let score = 0;
      if (matchedPrimary.length > 0) {
        score = 16 + (matchedPrimary.length - 1) * 4.5 + matchedSecondary.length * 2.5;
      } else if (matchedSecondary.length > 0) {
        score = Math.min(14, matchedSecondary.length * 5);
      }

      // 限制在 0～25 之間並取整數
      score = Math.min(25, Math.max(0, Math.round(score)));

      const allMatched = [...new Set([...matchedPrimary, ...matchedSecondary])];
      return {
        score,
        matchedWords: allMatched
      };
    }

    /**
     * 執行完整健診並更新畫面
     */
    function runDiagnosis() {
      const text = promptInput.value.trim();

      if (!text) {
        showMessage('請先在左側輸入框貼入您寫的 Prompt，再按下「健診」按鈕。');
        promptInput.focus();
        return;
      }

      hideMessage();

      let totalScore = 0;
      const results = [];

      DIMENSIONS.forEach(dim => {
        const evalResult = evaluateDimension(text, dim);
        totalScore += evalResult.score;

        // 更新數字與進度條寬度
        dim.scoreEl.textContent = evalResult.score;
        const percentage = (evalResult.score / 25) * 100;
        dim.barEl.style.width = `${percentage}%`;

        // 更新偵測到的關鍵字提示
        if (evalResult.matchedWords.length > 0) {
          dim.matchEl.innerHTML = `<span class="text-[#3a3834] font-medium">已偵測：</span>「${evalResult.matchedWords.join('」、「')}」`;
        } else {
          dim.matchEl.innerHTML = `<span class="text-[#b5703a]">未偵測到相關關鍵詞</span>（建議包含：${dim.primaryKeywords.join('、')}）`;
        }

        results.push({
          ...dim,
          score: evalResult.score
        });
      });

      // 更新總分與等級標籤
      updateTotalAndGrade(totalScore);

      // 根據低分維度生成改善建議
      renderSuggestions(results, totalScore);
    }

    /**
     * 更新總分與等級（90+: 優秀、70-89: 良好、50-69: 尚可、50以下: 需加強）
     */
    function updateTotalAndGrade(total) {
      const totalScoreEl = document.getElementById('totalScore');
      const gradeBadgeEl = document.getElementById('gradeBadge');

      totalScoreEl.textContent = total;

      let label = '';
      let badgeStyle = '';

      if (total >= 90) {
        label = '優秀';
        badgeStyle = 'background-color: #5a7a5a; color: #ffffff; border-color: #5a7a5a;';
      } else if (total >= 70) {
        label = '良好';
        badgeStyle = 'background-color: #6b7fa3; color: #ffffff; border-color: #6b7fa3;';
      } else if (total >= 50) {
        label = '尚可';
        badgeStyle = 'background-color: #c9963a; color: #ffffff; border-color: #c9963a;';
      } else {
        label = '需加強';
        badgeStyle = 'background-color: #b5703a; color: #ffffff; border-color: #b5703a;';
      }

      gradeBadgeEl.textContent = label;
      gradeBadgeEl.style.cssText = badgeStyle;
    }

    /**
     * 根據低分維度動態渲染改善建議
     */
    function renderSuggestions(results, totalScore) {
      const container = document.getElementById('suggestionsContainer');
      const statusEl = document.getElementById('suggestionStatus');

      // 篩選出低於 20 分的維度，並依分數由低到高排序（優先改善最弱項目）
      const weakDimensions = results
        .filter(item => item.score < 20)
        .sort((a, b) => a.score - b.score);

      container.innerHTML = '';

      if (weakDimensions.length === 0) {
        statusEl.textContent = '四個維度皆已具備完整結構';
        container.innerHTML = `
          <div class="md:col-span-2 p-5 bg-[#f5f3ee] border border-[#5a7a5a] rounded-[6px]">
            <div class="flex items-center gap-2 mb-2">
              <span class="inline-block w-3 h-3 rounded-[2px]" style="background-color: #5a7a5a;"></span>
              <h3 class="font-serif-title text-base font-bold text-[#2b2926]">結構完整！這是一份高品質的 Prompt</h3>
            </div>
            <p class="text-sm text-[#3a3834] leading-relaxed">
              您的指令已同時涵蓋「角色定位」、「明確任務動詞」、「邊界限制」與「具體呈現格式」。若想進一步提升效果，可嘗試補充更多背景脈絡資料（Context）或提供一組輸入／輸出的對照範例（Few-shot）。
            </p>
          </div>
        `;
        return;
      }

      statusEl.textContent = `發現 ${weakDimensions.length} 個可加強維度（依優先順序排列）`;

      weakDimensions.forEach(item => {
        const urgencyTag = item.score === 0 ? '缺失項目（0 分）' : `尚可補強（${item.score} / 25 分）`;
        const card = document.createElement('div');
        card.className = 'p-4 bg-[#f5f3ee] border border-[#d8d4cb] rounded-[6px] flex flex-col justify-between';
        card.style.borderLeftWidth = '4px';
        card.style.borderLeftColor = item.color;

        card.innerHTML = `
          <div>
            <div class="flex items-center justify-between gap-2 mb-2">
              <span class="text-xs font-bold tracking-wide" style="color: ${item.color};">${item.name}</span>
              <span class="text-[11px] px-2 py-0.5 bg-[#ffffff] border border-[#d8d4cb] rounded-[6px] text-[#54514b]">${urgencyTag}</span>
            </div>
            <h3 class="font-serif-title text-base font-bold text-[#2b2926] mb-1.5">${item.suggestionTitle}</h3>
            <p class="text-xs sm:text-sm text-[#3a3834] leading-relaxed mb-3">${item.suggestionDesc}</p>
          </div>
          <div class="p-2.5 bg-[#ffffff] border border-[#d8d4cb] rounded-[6px] text-xs text-[#54514b]">
            <span class="font-medium text-[#2b2926]">參考句型：</span>${item.exampleSnippet}
          </div>
        `;
        container.appendChild(card);
      });
    }

    // 綁定「健診」按鈕事件
    diagnoseBtn.addEventListener('click', runDiagnosis);

    // 綁定「填入範例」按鈕事件（方便快速測試）
    sampleBtn.addEventListener('click', () => {
      promptInput.value = '請幫我寫一篇關於遠距工作優缺點的文章，要能吸引上班族閱讀。';
      charCount.textContent = `${promptInput.value.length} 字`;
      hideMessage();
      runDiagnosis();
    });

    // 綁定「清空內容」按鈕事件
    clearBtn.addEventListener('click', () => {
      promptInput.value = '';
      charCount.textContent = '0 字';
      hideMessage();

      document.getElementById('totalScore').textContent = '--';
      const gradeBadgeEl = document.getElementById('gradeBadge');
      gradeBadgeEl.textContent = '尚未檢測';
      gradeBadgeEl.style.cssText = '';

      DIMENSIONS.forEach(dim => {
        dim.scoreEl.textContent = '0';
        dim.barEl.style.width = '0%';
        dim.matchEl.textContent = dim.defaultHint;
      });

      document.getElementById('suggestionStatus').textContent = '按下「健診」後將依據低分項目提供優化方向';
      document.getElementById('suggestionsContainer').innerHTML = `
        <div class="p-4 bg-[#f5f3ee] border border-[#d8d4cb] rounded-[6px] text-sm text-[#6e6a61] md:col-span-2 text-center py-6">
          尚未進行診斷。請在上方輸入您的 Prompt 並點擊「健診」按鈕。
        </div>
      `;
    });
  </script>
</body>
</html>
