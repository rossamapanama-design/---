<!doctype html>
<html lang="th">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>เกมจำแนกกรด-เบส-เกลือ</title>
  <script src="/_sdk/element_sdk.js"></script>
  <style>
    body {
      box-sizing: border-box;
    }
    
    * {
      box-sizing: border-box;
    }
    
    html, body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      font-family: 'Sarabun', 'Noto Sans Thai', sans-serif;
    }
    
    .game-container {
      width: 100%;
      height: 100%;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      padding: 20px;
      overflow-y: auto;
    }
    
    .game-header {
      text-align: center;
      color: white;
      margin-bottom: 20px;
    }
    
    .game-header h1 {
      margin: 0 0 10px 0;
      font-size: 32px;
      text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
    }
    
    .game-info {
      display: flex;
      justify-content: center;
      gap: 30px;
      margin-bottom: 20px;
      flex-wrap: wrap;
    }
    
    .info-box {
      background: rgba(255,255,255,0.2);
      padding: 10px 20px;
      border-radius: 10px;
      color: white;
      font-size: 18px;
      font-weight: bold;
    }
    
    .beakers-container {
      display: flex;
      justify-content: center;
      gap: 15px;
      margin-bottom: 30px;
      flex-wrap: wrap;
    }
    
    .beaker {
      background: rgba(255,255,255,0.95);
      border-radius: 15px;
      padding: 15px;
      min-width: 150px;
      box-shadow: 0 8px 16px rgba(0,0,0,0.2);
      transition: transform 0.2s;
    }
    
    .beaker.drag-over {
      transform: scale(1.05);
      background: rgba(173,216,230,0.95);
      border: 3px dashed #667eea;
    }
    
    .beaker-header {
      text-align: center;
      font-weight: bold;
      color: #764ba2;
      margin-bottom: 10px;
      padding-bottom: 10px;
      border-bottom: 2px solid #667eea;
      font-size: 16px;
    }
    
    .beaker-content {
      min-height: 100px;
      display: flex;
      flex-direction: column;
      gap: 5px;
    }
    
    .words-container {
      background: rgba(255,255,255,0.95);
      border-radius: 15px;
      padding: 20px;
      max-width: 1000px;
      margin: 0 auto;
      box-shadow: 0 8px 16px rgba(0,0,0,0.2);
    }
    
    .words-title {
      text-align: center;
      font-size: 20px;
      font-weight: bold;
      color: #764ba2;
      margin-bottom: 15px;
    }
    
    .words-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
      gap: 10px;
    }
    
    .word-card {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      padding: 12px;
      border-radius: 8px;
      text-align: center;
      cursor: move;
      font-weight: bold;
      box-shadow: 0 4px 6px rgba(0,0,0,0.2);
      transition: transform 0.2s;
      user-select: none;
    }
    
    .word-card:hover {
      transform: translateY(-3px);
      box-shadow: 0 6px 12px rgba(0,0,0,0.3);
    }
    
    .word-card.dragging {
      opacity: 0.5;
    }
    
    .word-card.placed {
      background: linear-gradient(135deg, #48bb78 0%, #38a169 100%);
    }
    
    .buttons-container {
      text-align: center;
      margin-top: 20px;
      display: flex;
      justify-content: center;
      gap: 15px;
      flex-wrap: wrap;
    }
    
    .btn {
      padding: 12px 30px;
      font-size: 18px;
      font-weight: bold;
      border: none;
      border-radius: 25px;
      cursor: pointer;
      transition: all 0.3s;
      box-shadow: 0 4px 6px rgba(0,0,0,0.2);
    }
    
    .btn-check {
      background: linear-gradient(135deg, #48bb78 0%, #38a169 100%);
      color: white;
    }
    
    .btn-check:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 12px rgba(0,0,0,0.3);
    }
    
    .btn-reset {
      background: linear-gradient(135deg, #f56565 0%, #e53e3e 100%);
      color: white;
    }
    
    .btn-reset:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 12px rgba(0,0,0,0.3);
    }
    
    .result-message {
      text-align: center;
      margin-top: 20px;
      padding: 15px;
      border-radius: 10px;
      font-size: 20px;
      font-weight: bold;
    }
    
    .result-message.success {
      background: rgba(72,187,120,0.9);
      color: white;
    }
    
    .result-message.error {
      background: rgba(245,101,101,0.9);
      color: white;
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
  <script src="https://cdn.tailwindcss.com" type="text/javascript"></script>
 </head>
 <body>
  <div class="game-container">
   <div class="game-header">
    <h1 id="game-title">🧪 เกมจำแนกกรด-เบส-เกลือ 🧪</h1>
    <div class="game-info">
     <div class="info-box">
      ⏱️ เวลา: <span id="timer">00:00</span>
     </div>
     <div class="info-box">
      ✅ ถูกต้อง: <span id="correct-count">0</span>/40
     </div>
    </div>
   </div>
   <div class="beakers-container" id="beakers-container">
    <div class="beaker" data-category="strong-acid">
     <div class="beaker-header">
      กรดแก่
     </div>
     <div class="beaker-content" id="beaker-strong-acid"></div>
    </div>
    <div class="beaker" data-category="strong-base">
     <div class="beaker-header">
      เบสแก่
     </div>
     <div class="beaker-content" id="beaker-strong-base"></div>
    </div>
    <div class="beaker" data-category="weak-acid">
     <div class="beaker-header">
      กรดอ่อน
     </div>
     <div class="beaker-content" id="beaker-weak-acid"></div>
    </div>
    <div class="beaker" data-category="weak-base">
     <div class="beaker-header">
      เบสอ่อน
     </div>
     <div class="beaker-content" id="beaker-weak-base"></div>
    </div>
    <div class="beaker" data-category="salt">
     <div class="beaker-header">
      เกลือ
     </div>
     <div class="beaker-content" id="beaker-salt"></div>
    </div>
   </div>
   <div class="words-container">
    <div class="words-title">
     ลากคำศัพท์ไปใส่บีกเกอร์ที่ถูกต้อง
    </div>
    <div class="words-grid" id="words-grid"></div>
   </div>
   <div class="buttons-container"><button class="btn btn-check" id="check-btn">ตรวจคำตอบ</button> <button class="btn btn-reset" id="reset-btn">เริ่มเกมใหม่</button>
   </div>
   <div id="result-message"></div>
  </div>
  <script>
    const defaultConfig = {
      game_title: "🧪 เกมจำแนกกรด-เบส-เกลือ 🧪",
      background_color: "#667eea",
      surface_color: "#ffffff",
      text_color: "#764ba2",
      primary_action_color: "#48bb78",
      secondary_action_color: "#f56565",
      font_family: "Sarabun",
      font_size: 16
    };

    const chemicalWords = [
      { word: "HCl", category: "strong-acid" },
      { word: "H₂SO₄", category: "strong-acid" },
      { word: "HNO₃", category: "strong-acid" },
      { word: "HBr", category: "strong-acid" },
      { word: "HI", category: "strong-acid" },
      { word: "HClO₄", category: "strong-acid" },
      { word: "NaOH", category: "strong-base" },
      { word: "KOH", category: "strong-base" },
      { word: "Ca(OH)₂", category: "strong-base" },
      { word: "Ba(OH)₂", category: "strong-base" },
      { word: "LiOH", category: "strong-base" },
      { word: "RbOH", category: "strong-base" },
      { word: "CsOH", category: "strong-base" },
      { word: "CH₃COOH", category: "weak-acid" },
      { word: "H₂CO₃", category: "weak-acid" },
      { word: "H₃PO₄", category: "weak-acid" },
      { word: "HF", category: "weak-acid" },
      { word: "H₂S", category: "weak-acid" },
      { word: "HCOOH", category: "weak-acid" },
      { word: "HCN", category: "weak-acid" },
      { word: "H₂SO₃", category: "weak-acid" },
      { word: "NH₃", category: "weak-base" },
      { word: "NH₄OH", category: "weak-base" },
      { word: "Mg(OH)₂", category: "weak-base" },
      { word: "Al(OH)₃", category: "weak-base" },
      { word: "Fe(OH)₂", category: "weak-base" },
      { word: "Zn(OH)₂", category: "weak-base" },
      { word: "NaCl", category: "salt" },
      { word: "KBr", category: "salt" },
      { word: "CaCO₃", category: "salt" },
      { word: "Na₂SO₄", category: "salt" },
      { word: "KNO₃", category: "salt" },
      { word: "MgCl₂", category: "salt" },
      { word: "CaSO₄", category: "salt" },
      { word: "NaHCO₃", category: "salt" },
      { word: "H₂O", category: "decoy" },
      { word: "O₂", category: "decoy" },
      { word: "CO₂", category: "decoy" },
      { word: "N₂", category: "decoy" },
      { word: "CH₄", category: "decoy" }
    ];

    let timerInterval;
    let seconds = 0;
    let placedWords = new Map();

    function shuffleArray(array) {
      const shuffled = [...array];
      for (let i = shuffled.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
      }
      return shuffled;
    }

    function initGame() {
      const wordsGrid = document.getElementById('words-grid');
      wordsGrid.innerHTML = '';
      placedWords.clear();
      
      document.querySelectorAll('.beaker-content').forEach(beaker => {
        beaker.innerHTML = '';
      });
      
      const shuffledWords = shuffleArray(chemicalWords);
      
      shuffledWords.forEach((item, index) => {
        const wordCard = document.createElement('div');
        wordCard.className = 'word-card';
        wordCard.textContent = item.word;
        wordCard.draggable = true;
        wordCard.dataset.word = item.word;
        wordCard.dataset.category = item.category;
        wordCard.dataset.id = `word-${index}`;
        
        wordCard.addEventListener('dragstart', handleDragStart);
        wordCard.addEventListener('dragend', handleDragEnd);
        
        wordsGrid.appendChild(wordCard);
      });
      
      document.querySelectorAll('.beaker-content').forEach(beaker => {
        beaker.addEventListener('dragover', handleDragOver);
        beaker.addEventListener('dragleave', handleDragLeave);
        beaker.addEventListener('drop', handleDrop);
      });
      
      seconds = 0;
      updateTimer();
      startTimer();
      
      document.getElementById('correct-count').textContent = '0';
      document.getElementById('result-message').innerHTML = '';
    }

    function handleDragStart(e) {
      e.target.classList.add('dragging');
      e.dataTransfer.effectAllowed = 'move';
      e.dataTransfer.setData('text/html', e.target.dataset.id);
    }

    function handleDragEnd(e) {
      e.target.classList.remove('dragging');
    }

    function handleDragOver(e) {
      e.preventDefault();
      e.dataTransfer.dropEffect = 'move';
      const beaker = e.currentTarget.closest('.beaker');
      beaker.classList.add('drag-over');
      return false;
    }

    function handleDragLeave(e) {
      const beaker = e.currentTarget.closest('.beaker');
      beaker.classList.remove('drag-over');
    }

    function handleDrop(e) {
      e.stopPropagation();
      e.preventDefault();
      
      const beaker = e.currentTarget.closest('.beaker');
      beaker.classList.remove('drag-over');
      
      const wordId = e.dataTransfer.getData('text/html');
      const wordCard = document.querySelector(`[data-id="${wordId}"]`);
      
      if (wordCard && !wordCard.classList.contains('placed')) {
        const beakerCategory = beaker.dataset.category;
        e.currentTarget.appendChild(wordCard);
        wordCard.classList.add('placed');
        
        placedWords.set(wordId, {
          word: wordCard.dataset.word,
          category: wordCard.dataset.category,
          placedIn: beakerCategory
        });
        
        updateCorrectCount();
      }
      
      return false;
    }

    function updateCorrectCount() {
      let correctCount = 0;
      placedWords.forEach(item => {
        if (item.category === item.placedIn) {
          correctCount++;
        }
      });
      document.getElementById('correct-count').textContent = correctCount;
    }

    function startTimer() {
      if (timerInterval) {
        clearInterval(timerInterval);
      }
      timerInterval = setInterval(() => {
        seconds++;
        updateTimer();
      }, 1000);
    }

    function updateTimer() {
      const minutes = Math.floor(seconds / 60);
      const secs = seconds % 60;
      document.getElementById('timer').textContent = 
        `${String(minutes).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
    }

    function checkAnswers() {
      if (placedWords.size === 0) {
        showResult('กรุณาลากคำศัพท์ลงบีกเกอร์ก่อนตรวจคำตอบ', 'error');
        return;
      }
      
      clearInterval(timerInterval);
      
      let correctCount = 0;
      let incorrectCount = 0;
      let decoyCount = 0;
      
      placedWords.forEach((item, wordId) => {
        const wordCard = document.querySelector(`[data-id="${wordId}"]`);
        
        if (item.category === 'decoy') {
          decoyCount++;
          wordCard.style.background = 'linear-gradient(135deg, #ed8936 0%, #dd6b20 100%)';
        } else if (item.category === item.placedIn) {
          correctCount++;
          wordCard.style.background = 'linear-gradient(135deg, #48bb78 0%, #38a169 100%)';
        } else {
          incorrectCount++;
          wordCard.style.background = 'linear-gradient(135deg, #f56565 0%, #e53e3e 100%)';
        }
      });
      
      const totalReal = chemicalWords.filter(item => item.category !== 'decoy').length;
      const message = `
        <div>✅ ถูกต้อง: ${correctCount}/${totalReal}</div>
        <div>❌ ผิด: ${incorrectCount}</div>
        <div>⚠️ คำลวง: ${decoyCount}/5</div>
        <div>⏱️ เวลาที่ใช้: ${document.getElementById('timer').textContent}</div>
      `;
      
      if (correctCount === totalReal && decoyCount === 0 && placedWords.size === totalReal) {
        showResult('🎉 ยินดีด้วย! คุณตอบถูกทุกข้อและไม่ติดกับดัก! 🎉' + message, 'success');
      } else if (correctCount === totalReal) {
        showResult('👏 เก่งมาก! คุณจำแนกสารได้ถูกต้องทั้งหมด! 👏' + message, 'success');
      } else {
        showResult('💪 พยายามอีกนิดนะ!' + message, 'error');
      }
    }

    function showResult(message, type) {
      const resultDiv = document.getElementById('result-message');
      resultDiv.innerHTML = message;
      resultDiv.className = `result-message ${type}`;
    }

    function resetGame() {
      if (timerInterval) {
        clearInterval(timerInterval);
      }
      initGame();
    }

    document.getElementById('check-btn').addEventListener('click', checkAnswers);
    document.getElementById('reset-btn').addEventListener('click', resetGame);

    async function onConfigChange(config) {
      const customFont = config.font_family || defaultConfig.font_family;
      const baseFontStack = 'Sarabun, Noto Sans Thai, sans-serif';
      const baseSize = config.font_size || defaultConfig.font_size;
      
      document.body.style.fontFamily = `${customFont}, ${baseFontStack}`;
      
      const gameContainer = document.querySelector('.game-container');
      gameContainer.style.background = `linear-gradient(135deg, ${config.background_color || defaultConfig.background_color} 0%, ${config.text_color || defaultConfig.text_color} 100%)`;
      
      document.querySelector('.game-header h1').textContent = config.game_title || defaultConfig.game_title;
      document.querySelector('.game-header h1').style.fontSize = `${baseSize * 2}px`;
      
      document.querySelectorAll('.info-box').forEach(box => {
        box.style.fontSize = `${baseSize * 1.125}px`;
      });
      
      document.querySelectorAll('.beaker').forEach(beaker => {
        beaker.style.background = `rgba(255,255,255,0.95)`;
      });
      
      document.querySelectorAll('.beaker-header').forEach(header => {
        header.style.color = config.text_color || defaultConfig.text_color;
        header.style.fontSize = `${baseSize}px`;
      });
      
      document.querySelector('.words-container').style.background = `rgba(255,255,255,0.95)`;
      
      document.querySelector('.words-title').style.color = config.text_color || defaultConfig.text_color;
      document.querySelector('.words-title').style.fontSize = `${baseSize * 1.25}px`;
      
      document.querySelectorAll('.word-card').forEach(card => {
        if (!card.classList.contains('placed')) {
          card.style.background = `linear-gradient(135deg, ${config.background_color || defaultConfig.background_color} 0%, ${config.text_color || defaultConfig.text_color} 100%)`;
        }
        card.style.fontSize = `${baseSize}px`;
      });
      
      document.querySelector('.btn-check').style.background = `linear-gradient(135deg, ${config.primary_action_color || defaultConfig.primary_action_color} 0%, #38a169 100%)`;
      document.querySelector('.btn-reset').style.background = `linear-gradient(135deg, ${config.secondary_action_color || defaultConfig.secondary_action_color} 0%, #e53e3e 100%)`;
      
      document.querySelectorAll('.btn').forEach(btn => {
        btn.style.fontSize = `${baseSize * 1.125}px`;
      });
      
      document.getElementById('result-message').style.fontSize = `${baseSize * 1.25}px`;
    }

    if (window.elementSdk) {
      window.elementSdk.init({
        defaultConfig,
        onConfigChange,
        mapToCapabilities: (config) => ({
          recolorables: [
            {
              get: () => config.background_color || defaultConfig.background_color,
              set: (value) => {
                config.background_color = value;
                window.elementSdk.setConfig({ background_color: value });
              }
            },
            {
              get: () => config.surface_color || defaultConfig.surface_color,
              set: (value) => {
                config.surface_color = value;
                window.elementSdk.setConfig({ surface_color: value });
              }
            },
            {
              get: () => config.text_color || defaultConfig.text_color,
              set: (value) => {
                config.text_color = value;
                window.elementSdk.setConfig({ text_color: value });
              }
            },
            {
              get: () => config.primary_action_color || defaultConfig.primary_action_color,
              set: (value) => {
                config.primary_action_color = value;
                window.elementSdk.setConfig({ primary_action_color: value });
              }
            },
            {
              get: () => config.secondary_action_color || defaultConfig.secondary_action_color,
              set: (value) => {
                config.secondary_action_color = value;
                window.elementSdk.setConfig({ secondary_action_color: value });
              }
            }
          ],
          borderables: [],
          fontEditable: {
            get: () => config.font_family || defaultConfig.font_family,
            set: (value) => {
              config.font_family = value;
              window.elementSdk.setConfig({ font_family: value });
            }
          },
          fontSizeable: {
            get: () => config.font_size || defaultConfig.font_size,
            set: (value) => {
              config.font_size = value;
              window.elementSdk.setConfig({ font_size: value });
            }
          }
        }),
        mapToEditPanelValues: (config) => new Map([
          ["game_title", config.game_title || defaultConfig.game_title]
        ])
      });
    }

    initGame();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9a4b3e4e01dd3e3d',t:'MTc2NDE3OTU4Ni4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
