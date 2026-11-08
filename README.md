<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>行動・実績ログ</title>
  <style>
    :root {
      --primary: #2563eb;
      --bg: #f8fafc;
      --card: #ffffff;
      --text: #1e293b;
      --muted: #64748b;
      --border: #e2e8f0;
      --danger: #ef4444;
      --edit-color: #0284c7;
      --empty-color: #cbd5e1;
      --cal-color: #ea580c;
      --cal-bg: #fff7ed;
      --multi-color: #8b5cf6;
      --multi-bg: #f5f3ff;
      --timer-bg: #eff6ff;
      --success: #10b981;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
      background-color: var(--bg);
      color: var(--text);
      line-height: 1.5;
      padding: 16px;
      max-width: 600px;
      margin: 0 auto;
    }

    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
      padding-bottom: 12px;
      border-bottom: 1px solid var(--border);
      flex-wrap: wrap;
      gap: 8px;
    }

    h1 {
      font-size: 1.2rem;
      font-weight: 700;
    }

    .tab-buttons {
      display: flex;
      gap: 6px;
    }

    .tab-btn {
      padding: 6px 10px;
      border: 1px solid var(--border);
      background: var(--card);
      border-radius: 8px;
      font-size: 0.8rem;
      cursor: pointer;
      color: var(--muted);
    }

    .tab-btn.active {
      background: var(--primary);
      color: white;
      border-color: var(--primary);
      font-weight: 600;
    }

    .section {
      display: none;
    }

    .section.active {
      display: block;
    }

    /* リアルタイム計測・タイマーエリア */
    .active-timers-container {
      margin-bottom: 16px;
    }

    .active-timer-card {
      background: var(--timer-bg);
      border: 2px solid var(--primary);
      border-radius: 12px;
      padding: 12px 16px;
      margin-bottom: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      box-shadow: 0 2px 4px rgba(37,99,235,0.1);
    }

    .timer-info-name {
      font-weight: 700;
      font-size: 1rem;
      color: var(--primary);
    }

    .timer-info-time {
      font-size: 0.85rem;
      color: var(--muted);
    }

    .timer-display {
      font-size: 1.25rem;
      font-weight: 800;
      font-variant-numeric: tabular-nums;
      color: var(--primary);
      margin: 0 12px;
    }

    .btn-stop-timer {
      background: var(--danger);
      color: white;
      border: none;
      padding: 8px 16px;
      border-radius: 8px;
      font-weight: 700;
      cursor: pointer;
      font-size: 0.9rem;
    }

    /* コンディション記録カード */
    .condition-card {
      background: var(--card);
      border-radius: 12px;
      padding: 14px 16px;
      margin-bottom: 16px;
      border: 1px solid var(--border);
    }

    .condition-title-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
    }

    .condition-row {
      display: flex;
      gap: 10px;
      align-items: flex-end;
      flex-wrap: wrap;
    }

    .condition-field {
      flex: 1;
      min-width: 90px;
    }

    .condition-field label {
      display: block;
      font-size: 0.75rem;
      font-weight: 600;
      color: var(--muted);
      margin-bottom: 4px;
    }

    .condition-field input,
    .condition-field select {
      width: 100%;
      padding: 8px 10px;
      font-size: 0.9rem;
    }

    .condition-badge-row {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      margin-bottom: 10px;
    }

    .cond-badge {
      font-size: 0.75rem;
      font-weight: 600;
      padding: 3px 8px;
      border-radius: 6px;
      background: #f1f5f9;
      color: var(--text);
      border: 1px solid var(--border);
    }

    /* ツリー選択エリア */
    .tree-container {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
      gap: 12px;
      margin-bottom: 20px;
    }

    .tree-root-btn {
      background: var(--card);
      border: 2px solid var(--genre-color, var(--primary));
      color: var(--genre-color, var(--primary));
      padding: 20px 12px;
      border-radius: 12px;
      font-size: 1.05rem;
      font-weight: 700;
      cursor: pointer;
      text-align: center;
      transition: all 0.2s;
    }

    .tree-root-btn:active {
      transform: scale(0.97);
    }

    /* 記録ボタンエリア */
    .action-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
      gap: 12px;
      margin-bottom: 16px;
    }

    .action-btn {
      background: var(--card);
      border: 2px solid var(--genre-color, var(--primary));
      color: var(--genre-color, var(--primary));
      padding: 16px 12px;
      border-radius: 12px;
      font-size: 1rem;
      font-weight: 700;
      cursor: pointer;
      transition: all 0.15s ease;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 4px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.04);
    }

    .action-btn:active {
      transform: scale(0.96);
    }

    .action-count {
      font-size: 0.75rem;
      font-weight: normal;
      opacity: 0.8;
    }

    .back-nav {
      margin-bottom: 12px;
    }

    .btn-back {
      background: none;
      border: none;
      color: var(--muted);
      font-size: 0.9rem;
      cursor: pointer;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: 4px;
    }

    /* カード共通 */
    .card {
      background: var(--card);
      border-radius: 12px;
      padding: 16px;
      margin-bottom: 16px;
      border: 1px solid var(--border);
    }

    .card-title {
      font-size: 1rem;
      font-weight: 700;
      margin-bottom: 12px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    /* 目標シートカード */
    .goal-card {
      background: var(--card);
      border-radius: 12px;
      padding: 16px;
      margin-bottom: 14px;
      border: 1px solid var(--border);
      box-shadow: 0 1px 3px rgba(0,0,0,0.03);
    }

    .goal-header {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      margin-bottom: 12px;
    }

    .goal-title {
      font-size: 1.05rem;
      font-weight: 700;
    }

    .goal-cat-tag {
      display: inline-block;
      font-size: 0.75rem;
      font-weight: 600;
      background: #eff6ff;
      color: var(--primary);
      padding: 2px 8px;
      border-radius: 6px;
      margin-top: 4px;
    }

    .goal-body {
      display: flex;
      align-items: center;
      gap: 16px;
    }

    .goal-donut {
      width: 76px;
      height: 76px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-shrink: 0;
      position: relative;
    }

    .goal-donut-inner {
      width: 56px;
      height: 56px;
      border-radius: 50%;
      background: var(--card);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.85rem;
      font-weight: 800;
      color: var(--text);
    }

    .goal-stats {
      flex: 1;
      font-size: 0.85rem;
    }

    .goal-progress-bar {
      width: 100%;
      height: 10px;
      background: #e2e8f0;
      border-radius: 999px;
      overflow: hidden;
      margin: 8px 0;
    }

    .goal-progress-fill {
      height: 100%;
      background: var(--primary);
      border-radius: 999px;
      transition: width 0.3s ease;
    }

    .goal-progress-fill.done {
      background: var(--success);
    }

    /* 1日ごとのまとめカード */
    .daily-summary-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
      padding: 0 4px;
    }

    .timeline-day-card {
      background: var(--card);
      border-radius: 12px;
      padding: 16px;
      margin-bottom: 16px;
      border: 1px solid var(--border);
      box-shadow: 0 1px 3px rgba(0,0,0,0.03);
    }

    .timeline-date-header {
      font-size: 1rem;
      font-weight: 700;
      color: var(--text);
      margin-bottom: 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 6px;
    }

    .cal-total-badge {
      background: var(--cal-bg);
      color: var(--cal-color);
      font-size: 0.75rem;
      font-weight: 700;
      padding: 2px 8px;
      border-radius: 12px;
      border: 1px solid #fed7aa;
    }

    .multi-total-badge {
      background: var(--multi-bg);
      color: var(--multi-color);
      font-size: 0.75rem;
      font-weight: 700;
      padding: 2px 8px;
      border-radius: 12px;
      border: 1px solid #ddd6fe;
    }

    .timeline-bar-container {
      position: relative;
      background: var(--empty-color);
      border-radius: 8px;
      overflow: hidden;
      border: 1px solid var(--border);
      margin-bottom: 6px;
    }

    .timeline-block {
      position: absolute;
      background: var(--block-color, var(--primary));
      opacity: 0.92;
      border-right: 1px solid #ffffff;
      border-bottom: 1px solid rgba(255,255,255,0.6);
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-size: 0.7rem;
      font-weight: 600;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      padding: 0 4px;
    }

    .timeline-hours {
      display: flex;
      justify-content: space-between;
      font-size: 0.7rem;
      color: var(--muted);
      padding: 0 2px;
      margin-bottom: 12px;
    }

    /* マルチリング・ドーナツグラフ表示エリア */
    .stats-container {
      display: flex;
      gap: 16px;
      align-items: center;
      background: #f8fafc;
      padding: 12px;
      border-radius: 8px;
      border: 1px solid var(--border);
      margin-bottom: 14px;
    }

    .donut-wrapper {
      position: relative;
      width: 104px;
      height: 104px;
      flex-shrink: 0;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .donut-ring {
      position: absolute;
      border-radius: 50%;
    }

    .donut-center-hole {
      position: absolute;
      width: 32px;
      height: 32px;
      border-radius: 50%;
      background: #f8fafc;
      z-index: 10;
    }

    .stats-legend {
      flex: 1;
      font-size: 0.8rem;
    }

    .legend-section-title {
      font-size: 0.7rem;
      font-weight: 700;
      color: var(--muted);
      margin-bottom: 4px;
      margin-top: 6px;
    }

    .legend-section-title:first-child {
      margin-top: 0;
    }

    .legend-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 3px;
    }

    .legend-item:last-child {
      margin-bottom: 0;
    }

    .legend-color-box {
      width: 10px;
      height: 10px;
      border-radius: 3px;
      display: inline-block;
      margin-right: 6px;
    }

    /* 日別カード内の履歴リスト */
    .day-logs-section {
      border-top: 1px dashed var(--border);
      padding-top: 10px;
    }

    .day-logs-title {
      font-size: 0.8rem;
      font-weight: 700;
      color: var(--muted);
      margin-bottom: 4px;
    }

    .log-list {
      list-style: none;
    }

    .log-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 8px 0;
      border-bottom: 1px solid var(--border);
      font-size: 0.9rem;
    }

    .log-item:last-child {
      border-bottom: none;
      padding-bottom: 0;
    }

    .log-name {
      font-weight: 600;
    }

    .log-time {
      font-size: 0.8rem;
      color: var(--muted);
      margin-top: 2px;
    }

    .log-badge-memo {
      font-size: 0.75rem;
      color: var(--badge-color, var(--primary));
      background: var(--badge-bg, #eff6ff);
      padding: 1px 6px;
      border-radius: 4px;
      display: inline-block;
      margin-top: 4px;
      margin-right: 4px;
      font-weight: 600;
    }

    .log-badge-cal {
      font-size: 0.75rem;
      color: var(--cal-color);
      background: var(--cal-bg);
      padding: 1px 6px;
      border-radius: 4px;
      display: inline-block;
      margin-top: 4px;
      font-weight: 700;
    }

    .log-duration {
      display: inline-block;
      background: #eff6ff;
      color: var(--primary);
      font-size: 0.75rem;
      font-weight: 600;
      padding: 1px 6px;
      border-radius: 4px;
      margin-left: 6px;
    }

    .log-actions {
      display: flex;
      gap: 4px;
      align-items: center;
      flex-shrink: 0;
    }

    .edit-log-btn {
      background: none;
      border: none;
      color: var(--edit-color);
      cursor: pointer;
      font-size: 0.8rem;
      padding: 4px 8px;
      font-weight: 600;
    }

    .edit-log-btn:hover {
      text-decoration: underline;
    }

    .delete-log-btn {
      background: none;
      border: none;
      color: var(--muted);
      cursor: pointer;
      font-size: 0.8rem;
      padding: 4px 8px;
    }

    .delete-log-btn:hover {
      color: var(--danger);
    }

    /* 編集エリア */
    .add-form {
      display: flex;
      gap: 8px;
      margin-bottom: 12px;
      flex-wrap: wrap;
      align-items: center;
    }

    input[type="text"],
    input[type="number"],
    input[type="date"],
    input[type="time"],
    select {
      padding: 10px 12px;
      border: 1px solid var(--border);
      border-radius: 8px;
      font-size: 0.95rem;
      font-family: inherit;
      background: white;
      color: var(--text);
    }

    input[type="color"] {
      -webkit-appearance: none;
      border: none;
      width: 38px;
      height: 38px;
      border-radius: 8px;
      cursor: pointer;
      background: none;
      padding: 0;
    }

    input[type="color"]::-webkit-color-swatch-wrapper {
      padding: 0;
    }

    input[type="color"]::-webkit-color-swatch {
      border: 1px solid var(--border);
      border-radius: 8px;
    }

    input[type="text"] {
      flex: 1;
      min-width: 120px;
    }

    .btn-primary {
      background: var(--primary);
      color: white;
      border: none;
      padding: 10px 16px;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
      font-size: 0.95rem;
    }

    .btn-secondary {
      background: var(--bg);
      color: var(--text);
      border: 1px solid var(--border);
      padding: 10px 16px;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
      font-size: 0.95rem;
    }

    .edit-item {
      display: flex;
      gap: 8px;
      align-items: center;
      margin-bottom: 10px;
      flex-wrap: wrap;
    }

    .btn-danger {
      background: #fee2e2;
      color: var(--danger);
      border: none;
      padding: 8px 12px;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
      font-size: 0.85rem;
    }

    /* モーダル共通 */
    .modal-overlay {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(15, 23, 42, 0.5);
      z-index: 100;
      align-items: center;
      justify-content: center;
      padding: 16px;
    }

    .modal-overlay.show {
      display: flex;
    }

    .modal-content {
      background: var(--card);
      border-radius: 16px;
      padding: 20px;
      width: 100%;
      max-width: 400px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.15);
      max-height: 90vh;
      overflow-y: auto;
    }

    .modal-title {
      font-size: 1.125rem;
      font-weight: 700;
      margin-bottom: 16px;
      color: var(--primary);
    }

    .form-group {
      margin-bottom: 14px;
    }

    .form-group label {
      display: block;
      font-size: 0.85rem;
      font-weight: 600;
      color: var(--muted);
      margin-bottom: 4px;
    }

    .form-group input {
      width: 100%;
    }

    .time-row {
      display: flex;
      gap: 12px;
    }

    .time-row .form-group {
      flex: 1;
    }

    .choice-modal-btns {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-top: 16px;
    }

    .choice-btn {
      padding: 14px;
      border-radius: 10px;
      font-size: 1rem;
      font-weight: 700;
      cursor: pointer;
      text-align: center;
      border: 1px solid var(--border);
    }

    .choice-btn-timer {
      background: #eff6ff;
      color: var(--primary);
      border-color: #bfdbfe;
    }

    .choice-btn-manual {
      background: #f8fafc;
      color: var(--text);
    }

    .preset-chips {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      margin-top: 8px;
    }

    .preset-chip {
      background: #f1f5f9;
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 4px 10px;
      font-size: 0.75rem;
      cursor: pointer;
      color: var(--text);
      display: flex;
      align-items: center;
      gap: 4px;
    }

    .preset-chip-cal {
      color: var(--cal-color);
      font-weight: 700;
    }

    .modal-actions {
      display: flex;
      gap: 10px;
      margin-top: 20px;
    }

    .modal-actions button {
      flex: 1;
    }

    #toast {
      position: fixed;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      background: #1e293b;
      color: white;
      padding: 10px 20px;
      border-radius: 24px;
      font-size: 0.875rem;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.3s;
      z-index: 200;
    }

    #toast.show {
      opacity: 1;
    }
  </style>
</head>
<body>

  <header>
    <h1>行動・実績ログ</h1>
    <div class="tab-buttons">
      <button class="tab-btn active" onclick="switchTab('record', this)">記録する</button>
      <button class="tab-btn" onclick="switchTab('goals', this)">目標シート</button>
      <button class="tab-btn" onclick="switchTab('settings', this)">カテゴリ編集</button>
    </div>
  </header>

  <!-- メイン画面：記録と実績 -->
  <div id="tab-record" class="section active">
    
    <!-- リアルタイム計測中のタイマー表示エリア -->
    <div id="active-timers-container" class="active-timers-container"></div>

    <!-- コンディション記録（体調・体重・メンタル） -->
    <div class="condition-card">
      <div class="condition-title-row">
        <span style="font-size: 1rem; font-weight: 700;">🩺 今日のコンディション</span>
      </div>
      <div style="margin-bottom: 10px;">
        <label style="display: block; font-size: 0.75rem; font-weight: 600; color: var(--muted); margin-bottom: 4px;">対象日付</label>
        <input type="date" id="cond-date" style="padding: 6px 10px; font-size: 0.850rem; width: 100%; max-width: 180px;" onchange="loadConditionForDate()">
      </div>
      <div class="condition-row">
        <div class="condition-field">
          <label>体重 (kg)</label>
          <input type="number" id="cond-weight" step="0.1" placeholder="72.0" inputmode="decimal">
        </div>
        <div class="condition-field">
          <label>体調 (1〜5)</label>
          <select id="cond-body">
            <option value="5">5: 絶好調</option>
            <option value="4">4: 良好</option>
            <option value="3" selected>3: 普通</option>
            <option value="2">2: 疲労</option>
            <option value="1">1: 不調</option>
          </select>
        </div>
        <div class="condition-field">
          <label>メンタル (1〜5)</label>
          <select id="cond-mental">
            <option value="5">5: 最高</option>
            <option value="4">4: 前向き</option>
            <option value="3" selected>3: 普通</option>
            <option value="2">2: モヤモヤ</option>
            <option value="1">1: 低下</option>
          </select>
        </div>
        <button class="btn-primary" style="padding: 9px 14px;" onclick="saveCondition()">記録</button>
      </div>
    </div>

    <!-- ジャンル選択画面（初期表示） -->
    <div id="tree-root-view">
      <div class="card" style="border: none; background: transparent; padding: 0;">
        <div class="card-title" style="margin-bottom: 8px;">ジャンルを選択</div>
      </div>
      <div class="tree-container" id="genre-buttons"></div>
    </div>

    <!-- カテゴリ選択ビュー -->
    <div id="tree-category-view" style="display: none;">
      <div class="back-nav">
        <button class="btn-back" onclick="backToRoot()">← ジャンル選択に戻る</button>
      </div>
      <div class="card" style="border: none; background: transparent; padding: 0 0 8px 0;">
        <div id="selected-genre-title" style="font-size: 0.95rem; font-weight: 700;"></div>
      </div>
      <div class="action-grid" id="category-buttons"></div>
    </div>

    <!-- 1日ごとのタイムライン＋履歴まとめエリア -->
    <div class="daily-summary-header">
      <span style="font-size: 1rem; font-weight: 700;">日別のタイムライン・履歴</span>
      <span style="font-size: 0.8rem; color: var(--muted);" id="total-count">合計 0 件</span>
    </div>
    <div id="daily-container"></div>
  </div>

  <!-- 目標シートタブ -->
  <div id="tab-goals" class="section">
    <div class="card">
      <div class="card-title">🎯 新しい目標を設定</div>
      <div class="add-form">
        <input type="text" id="new-goal-title" placeholder="目標名（例：月間ランニング、資格勉強）" style="width: 100%; flex: none;">
        <select id="new-goal-cat" style="flex: 1; min-width: 130px;"></select>
        <select id="new-goal-type" style="width: 105px;" onchange="onGoalTypeChange(this.value)">
          <option value="time">時間目標</option>
          <option value="distance">距離目標</option>
        </select>
        <input type="number" id="new-goal-value" placeholder="目標値(h/km)" min="1" step="0.5" style="width: 110px;">
        <button class="btn-primary" onclick="addGoal()">追加</button>
      </div>
    </div>

    <div id="goals-list-container"></div>
  </div>

  <!-- 設定画面：ジャンルとカテゴリの追加・編集・削除 -->
  <div id="tab-settings" class="section">
    <div class="card">
      <div class="card-title">ジャンルの追加・編集</div>
      <div class="add-form">
        <input type="color" id="new-genre-color" value="#2563eb">
        <select id="new-genre-mode">
          <option value="time">⏱️ 時間記録</option>
          <option value="memo">📝 メモ記録</option>
        </select>
        <input type="text" id="new-genre-input" placeholder="例：仕事、睡眠...">
        <button class="btn-primary" onclick="addGenre()">追加</button>
      </div>
      <div id="genre-edit-list" style="margin-top: 12px;"></div>
    </div>

    <div class="card">
      <div class="card-title">新しいカテゴリを追加</div>
      <div class="add-form">
        <select id="new-cat-type"></select>
        <input type="text" id="new-cat-input" placeholder="例：牛丼、ランニング...">
        <button class="btn-primary" onclick="addCategory()">追加</button>
      </div>
    </div>

    <div class="card">
      <div class="card-title">登録済みカテゴリの編集</div>
      <div id="cat-edit-list"></div>
    </div>
  </div>

  <!-- 入力方法選択モーダル（リアルタイム or 手動） -->
  <div id="choice-modal" class="modal-overlay">
    <div class="modal-content">
      <div class="modal-title" id="choice-modal-title">記録方法を選択</div>
      <div class="choice-modal-btns">
        <button class="choice-btn choice-btn-timer" onclick="chooseTimerMethod()">⏱️ リアルタイム計測を開始する</button>
        <button class="choice-btn choice-btn-manual" onclick="chooseManualMethod()">🕒 過去の時間を手動で入力する</button>
      </div>
      <div style="margin-top: 16px;">
        <button class="btn-secondary" style="width: 100%;" onclick="closeChoiceModal()">キャンセル</button>
      </div>
    </div>
  </div>

  <!-- 手動記録・編集用モーダル -->
  <div id="record-modal" class="modal-overlay">
    <div class="modal-content">
      <div class="modal-title" id="modal-cat-name">記録する</div>
      
      <div id="modal-time-datetime-section">
        <div class="form-group">
          <label>開始日時</label>
          <div class="time-row">
            <input type="date" id="modal-start-date" style="flex: 1.3;">
            <input type="time" id="modal-start-time" style="flex: 1;">
          </div>
        </div>
        <div class="form-group">
          <label>終了日時</label>
          <div class="time-row">
            <input type="date" id="modal-end-date" style="flex: 1.3;">
            <input type="time" id="modal-end-time" style="flex: 1;">
          </div>
        </div>
        <div class="form-group" id="modal-distance-group">
          <label>距離 (km)（任意）</label>
          <input type="number" id="modal-distance" step="0.1" placeholder="例：10.5" min="0" inputmode="decimal">
        </div>
      </div>

      <div id="modal-memo-date-section" class="form-group" style="display: none;">
        <label>日付</label>
        <input type="date" id="modal-memo-date">
      </div>

      <div id="modal-meal-section" style="display: none;">
        <div class="form-group">
          <label>メニュー・詳細（任意）</label>
          <input type="text" id="modal-meal" placeholder="例：牛丼（並）、プロテインなど" oninput="onMealInput(this.value)">
          <div id="preset-chips-container" class="preset-chips"></div>
        </div>
        <div class="form-group">
          <label>カロリー (kcal)（任意）</label>
          <input type="number" id="modal-calories" placeholder="例：650" min="0" inputmode="numeric">
        </div>
      </div>

      <div class="modal-actions">
        <button class="btn-secondary" onclick="closeModal()">キャンセル</button>
        <button class="btn-primary" onclick="confirmRecord()">保存する</button>
      </div>
    </div>
  </div>

  <div id="toast">保存しました！</div>

  <script>
    const STORAGE_GENRES = 'habit_app_genres';
    const STORAGE_CATS = 'habit_app_categories';
    const STORAGE_LOGS = 'habit_app_logs';
    const STORAGE_PRESETS = 'habit_app_calorie_presets';
    const STORAGE_TIMERS = 'habit_app_active_timers';
    const STORAGE_GOALS = 'habit_app_goals';
    const STORAGE_CONDITIONS = 'habit_app_conditions';

    let genres = JSON.parse(localStorage.getItem(STORAGE_GENRES)) || [
      { id: 'food', name: '🥗 飲食', mode: 'memo', color: '#059669' },
      { id: 'action', name: '🎯 行動', mode: 'time', color: '#2563eb' }
    ];

    let categories = JSON.parse(localStorage.getItem(STORAGE_CATS)) || [
      { id: '1', name: '筋トレ', type: 'action' },
      { id: '2', name: 'ランニング', type: 'action' },
      { id: '3', name: '外食・食事', type: 'food' },
      { id: '4', name: 'プロテイン・間食', type: 'food' }
    ];

    let logs = JSON.parse(localStorage.getItem(STORAGE_LOGS)) || [];
    let caloriePresets = JSON.parse(localStorage.getItem(STORAGE_PRESETS)) || {};
    let activeTimers = JSON.parse(localStorage.getItem(STORAGE_TIMERS)) || [];
    let goals = JSON.parse(localStorage.getItem(STORAGE_GOALS)) || [];
    let conditions = JSON.parse(localStorage.getItem(STORAGE_CONDITIONS)) || {};

    let currentGenreId = null;
    let selectedItem = null;
    let editingLogId = null;
    let pendingCategory = null;

    function saveData() {
      localStorage.setItem(STORAGE_GENRES, JSON.stringify(genres));
      localStorage.setItem(STORAGE_CATS, JSON.stringify(categories));
      localStorage.setItem(STORAGE_LOGS, JSON.stringify(logs));
      localStorage.setItem(STORAGE_PRESETS, JSON.stringify(caloriePresets));
      localStorage.setItem(STORAGE_TIMERS, JSON.stringify(activeTimers));
      localStorage.setItem(STORAGE_GOALS, JSON.stringify(goals));
      localStorage.setItem(STORAGE_CONDITIONS, JSON.stringify(conditions));
    }

    function switchTab(tabName, btnEl) {
      document.querySelectorAll('.section').forEach(el => el.classList.remove('active'));
      document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
      
      document.getElementById('tab-' + tabName).classList.add('active');
      if (btnEl) btnEl.classList.add('active');
      if (tabName === 'record') backToRoot();
      render();
    }

    function selectGenre(genreId) {
      currentGenreId = genreId;
      document.getElementById('tree-root-view').style.display = 'none';
      document.getElementById('tree-category-view').style.display = 'block';
      renderCategoriesForGenre();
    }

    function backToRoot() {
      currentGenreId = null;
      document.getElementById('tree-root-view').style.display = 'block';
      document.getElementById('tree-category-view').style.display = 'none';
    }

    function getTodayStr() {
      const now = new Date();
      const yyyy = now.getFullYear();
      const mm = String(now.getMonth() + 1).padStart(2, '0');
      const dd = String(now.getDate()).padStart(2, '0');
      return `${yyyy}-${mm}-${dd}`;
    }

    function getCurrentTimeStr() {
      const now = new Date();
      const hh = String(now.getHours()).padStart(2, '0');
      const min = String(now.getMinutes()).padStart(2, '0');
      return `${hh}:${min}`;
    }

    function initConditionDate() {
      const dateInput = document.getElementById('cond-date');
      if (dateInput && !dateInput.value) {
        dateInput.value = getTodayStr();
      }
      loadConditionForDate();
    }

    function loadConditionForDate() {
      const dateInput = document.getElementById('cond-date');
      if (!dateInput) return;
      const dateStr = dateInput.value || getTodayStr();
      const data = conditions[dateStr];
      if (data) {
        document.getElementById('cond-weight').value = data.weight !== undefined ? data.weight : '';
        document.getElementById('cond-body').value = data.body || '3';
        document.getElementById('cond-mental').value = data.mental || '3';
      } else {
        document.getElementById('cond-weight').value = '';
        document.getElementById('cond-body').value = '3';
        document.getElementById('cond-mental').value = '3';
      }
    }

    function saveCondition() {
      const dateInput = document.getElementById('cond-date');
      const dateStr = dateInput ? (dateInput.value || getTodayStr()) : getTodayStr();
      const weightVal = parseFloat(document.getElementById('cond-weight').value);
      const bodyVal = document.getElementById('cond-body').value;
      const mentalVal = document.getElementById('cond-mental').value;

      conditions[dateStr] = {
        weight: !isNaN(weightVal) ? weightVal : undefined,
        body: bodyVal,
        mental: mentalVal
      };
      saveData();
      render();
      showToast(`${dateStr} のコンディションを記録しました！`);
    }

    function onGoalTypeChange(type) {
      const valInput = document.getElementById('new-goal-value');
      if (type === 'distance') {
        valInput.placeholder = '目標距離(km)';
      } else {
        valInput.placeholder = '目標時間(h)';
      }
    }

    function addGoal() {
      const titleInput = document.getElementById('new-goal-title');
      const catSelect = document.getElementById('new-goal-cat');
      const typeSelect = document.getElementById('new-goal-type');
      const valInput = document.getElementById('new-goal-value');

      const title = titleInput.value.trim();
      const catId = catSelect.value;
      const goalType = typeSelect.value;
      const targetVal = parseFloat(valInput.value);

      if (!title || !catId || isNaN(targetVal) || targetVal <= 0) {
        alert('目標名・対象カテゴリ・目標値を正しく入力してください。');
        return;
      }

      const newGoal = {
        id: 'goal_' + Date.now(),
        title: title,
        catId: catId,
        type: goalType
      };

      if (goalType === 'distance') {
        newGoal.targetDistance = targetVal;
      } else {
        newGoal.targetMinutes = Math.round(targetVal * 60);
      }

      goals.push(newGoal);

      titleInput.value = '';
      valInput.value = '';
      saveData();
      render();
      showToast(`目標「${title}」を追加しました！`);
    }

    function deleteGoal(goalId) {
      if (confirm('この目標を削除しますか？')) {
        goals = goals.filter(g => g.id !== goalId);
        saveData();
        render();
        showToast('目標を削除しました');
      }
    }

    function calcLogDurationMinutes(log) {
      if (!log.startTime || !log.endTime) return 0;
      const sDate = log.startDate || log.date;
      const eDate = log.endDate || sDate;
      if (!sDate) return 0;

      const startDt = new Date(`${sDate}T${log.startTime}`);
      const endDt = new Date(`${eDate}T${log.endTime}`);
      let diffMin = Math.floor((endDt.getTime() - startDt.getTime()) / 60000);
      if (diffMin < 0) diffMin += 24 * 60;
      return diffMin > 0 ? diffMin : 0;
    }

    function formatMinutesToHours(mins) {
      const h = Math.floor(mins / 60);
      const m = Math.round(mins % 60);
      if (h > 0 && m > 0) return `${h}時間${m}分`;
      if (h > 0) return `${h}時間`;
      return `${m}分`;
    }

    function renderGoals() {
      const catSelect = document.getElementById('new-goal-cat');
      if (!catSelect) return;
      catSelect.innerHTML = '';
      categories.forEach(c => {
        const g = genres.find(item => item.id === (c.type || 'action'));
        if (!g || g.mode === 'time') {
          const opt = document.createElement('option');
          opt.value = c.id;
          opt.textContent = c.name;
          catSelect.appendChild(opt);
        }
      });

      const container = document.getElementById('goals-list-container');
      container.innerHTML = '';

      if (goals.length === 0) {
        container.innerHTML = '<div class="card" style="color: var(--muted); font-size: 0.9rem; text-align: center; padding: 24px;">まだ目標が設定されていません。上のフォームから目標を追加しましょう！</div>';
        return;
      }

      goals.forEach(goal => {
        const cat = categories.find(c => c.id === goal.catId);
        const catName = cat ? cat.name : '不明なカテゴリ';
        const isDistance = goal.type === 'distance';

        let achievedVal = 0;
        logs.forEach(log => {
          if (log.catId === goal.catId || (cat && log.name === cat.name)) {
            if (isDistance) {
              achievedVal += (log.distance || 0);
            } else {
              achievedVal += calcLogDurationMinutes(log);
            }
          }
        });

        const targetVal = isDistance ? (goal.targetDistance || 100) : (goal.targetMinutes || 60);
        const remainVal = Math.max(targetVal - achievedVal, 0);
        const pct = Math.min(Math.round((achievedVal / targetVal) * 1000) / 10, 100);
        const isDone = achievedVal >= targetVal;
        const fillColor = isDone ? 'var(--success)' : 'var(--primary)';

        let achievedStr = '';
        let targetStr = '';
        let remainStr = '';

        if (isDistance) {
          achievedStr = `${achievedVal.toFixed(1)} km`;
          targetStr = `${targetVal} km`;
          remainStr = `あと ${remainVal.toFixed(1)} km`;
        } else {
          achievedStr = formatMinutesToHours(achievedVal);
          targetStr = formatMinutesToHours(targetVal);
          remainStr = `あと ${formatMinutesToHours(remainVal)}`;
        }

        const card = document.createElement('div');
        card.className = 'goal-card';
        card.innerHTML = `
          <div class="goal-header">
            <div>
              <div class="goal-title">${goal.title}</div>
              <span class="goal-cat-tag">対象: ${catName} (${isDistance ? '距離目標' : '時間目標'})</span>
            </div>
            <button class="btn-danger" onclick="deleteGoal('${goal.id}')">削除</button>
          </div>
          <div class="goal-body">
            <div class="goal-donut" style="background: conic-gradient(${fillColor} ${pct * 3.6}deg, #e2e8f0 0deg);">
              <div class="goal-donut-inner">${pct}%</div>
            </div>
            <div class="goal-stats">
              <div>達成: <strong>${achievedStr}</strong> / 目標: ${targetStr}</div>
              <div class="goal-progress-bar">
                <div class="goal-progress-fill ${isDone ? 'done' : ''}" style="width: ${pct}%;"></div>
              </div>
              <div style="color: ${isDone ? 'var(--success)' : 'var(--muted)'}; font-weight: 700;">
                ${isDone ? '🎉 目標達成！' : remainStr}
              </div>
            </div>
          </div>
        `;
        container.appendChild(card);
      });
    }

    function handleCategoryClick(catId, catName, genreId, mode) {
      if (mode === 'memo') {
        openRecordModal(catId, catName, mode);
      } else {
        pendingCategory = { id: catId, name: catName, type: genreId, mode: mode };
        document.getElementById('choice-modal-title').textContent = `「${catName}」の記録方法`;
        document.getElementById('choice-modal').classList.add('show');
      }
    }

    function closeChoiceModal() {
      document.getElementById('choice-modal').classList.remove('show');
      pendingCategory = null;
    }

    function chooseTimerMethod() {
      if (!pendingCategory) return;
      const { id, name, type } = pendingCategory;
      closeChoiceModal();
      startTimer(id, name, type);
    }

    function chooseManualMethod() {
      if (!pendingCategory) return;
      const { id, name, mode } = pendingCategory;
      closeChoiceModal();
      openRecordModal(id, name, mode);
    }

    function startTimer(catId, catName, genreId) {
      const now = new Date();
      const yyyy = now.getFullYear();
      const mm = String(now.getMonth() + 1).padStart(2, '0');
      const dd = String(now.getDate()).padStart(2, '0');
      const hh = String(now.getHours()).padStart(2, '0');
      const min = String(now.getMinutes()).padStart(2, '0');

      const newTimer = {
        id: 'timer_' + Date.now(),
        catId: catId,
        name: catName,
        type: genreId,
        startDate: `${yyyy}-${mm}-${dd}`,
        startTime: `${hh}:${min}`,
        startTimestamp: now.getTime()
      };

      activeTimers.push(newTimer);
      saveData();
      backToRoot();
      render();
      showToast(`「${catName}」の計測を開始しました！`);
    }

    function stopTimer(timerId) {
      const idx = activeTimers.findIndex(t => t.id === timerId);
      if (idx === -1) return;

      const timer = activeTimers[idx];
      const now = new Date();
      const yyyy = now.getFullYear();
      const mm = String(now.getMonth() + 1).padStart(2, '0');
      const dd = String(now.getDate()).padStart(2, '0');
      const hh = String(now.getHours()).padStart(2, '0');
      const min = String(now.getMinutes()).padStart(2, '0');

      const newLog = {
        id: Date.now().toString(),
        catId: timer.catId,
        name: timer.name,
        type: timer.type,
        mode: 'time',
        startDate: timer.startDate,
        startTime: timer.startTime,
        endDate: `${yyyy}-${mm}-${dd}`,
        endTime: `${hh}:${min}`,
        date: timer.startDate,
        timestamp: new Date().toISOString()
      };

      logs.unshift(newLog);
      activeTimers.splice(idx, 1);
      saveData();
      render();
      showToast(`「${timer.name}」の記録を保存しました！`);
    }

    function openRecordModal(catId, catName, mode, existingLog = null) {
      editingLogId = existingLog ? existingLog.id : null;
      selectedItem = { id: catId, name: catName, type: existingLog ? existingLog.type : currentGenreId, mode: mode };
      
      document.getElementById('modal-cat-name').textContent = existingLog ? `「${catName}」を編集` : `「${catName}」を手動で記録`;

      const today = getTodayStr();
      const currentTime = getCurrentTimeStr();

      const startDate = existingLog ? (existingLog.startDate || existingLog.date || today) : today;
      const startTime = existingLog ? (existingLog.startTime || currentTime) : currentTime;
      const endDate = existingLog ? (existingLog.endDate || existingLog.date || today) : today;
      const endTime = existingLog ? (existingLog.endTime || currentTime) : currentTime;

      document.getElementById('modal-start-date').value = startDate;
      document.getElementById('modal-start-time').value = startTime;
      document.getElementById('modal-end-date').value = endDate;
      document.getElementById('modal-end-time').value = endTime;
      document.getElementById('modal-memo-date').value = existingLog ? (existingLog.date || today) : today;

      document.getElementById('modal-meal').value = existingLog && existingLog.meal ? existingLog.meal : '';
      document.getElementById('modal-calories').value = existingLog && existingLog.calories ? existingLog.calories : '';
      document.getElementById('modal-distance').value = existingLog && existingLog.distance !== undefined ? existingLog.distance : '';

      if (mode === 'memo') {
        document.getElementById('modal-time-datetime-section').style.display = 'none';
        document.getElementById('modal-memo-date-section').style.display = 'block';
        document.getElementById('modal-meal-section').style.display = 'block';
        if (!existingLog && caloriePresets[catName]) {
          document.getElementById('modal-calories').value = caloriePresets[catName];
        }
        renderPresetChips();
      } else {
        document.getElementById('modal-time-datetime-section').style.display = 'block';
        document.getElementById('modal-memo-date-section').style.display = 'none';
        document.getElementById('modal-meal-section').style.display = 'none';
      }

      document.getElementById('record-modal').classList.add('show');
    }

    function editLog(logId) {
      const log = logs.find(l => l.id === logId);
      if (!log) return;

      const g = genres.find(item => item.id === log.type);
      const mode = (g && g.mode === 'memo') || log.mode === 'memo' || log.type === 'food' ? 'memo' : 'time';
      
      currentGenreId = log.type;
      openRecordModal(log.catId || log.type, log.name, mode, log);
    }

    function onMealInput(val) {
      const trimmed = val.trim();
      if (trimmed && caloriePresets[trimmed]) {
        document.getElementById('modal-calories').value = caloriePresets[trimmed];
      }
    }

    function renderPresetChips() {
      const container = document.getElementById('preset-chips-container');
      container.innerHTML = '';
      const keys = Object.keys(caloriePresets);
      if (keys.length === 0) return;

      keys.slice(-8).reverse().forEach(name => {
        const cal = caloriePresets[name];
        const chip = document.createElement('button');
        chip.type = 'button';
        chip.className = 'preset-chip';
        chip.innerHTML = `<span>${name}</span><span class="preset-chip-cal">${cal}kcal</span>`;
        chip.onclick = () => {
          document.getElementById('modal-meal').value = name;
          document.getElementById('modal-calories').value = cal;
        };
        container.appendChild(chip);
      });
    }

    function closeModal() {
      document.getElementById('record-modal').classList.remove('show');
      selectedItem = null;
      editingLogId = null;
    }

    function confirmRecord() {
      if (!selectedItem) return;

      const distVal = parseFloat(document.getElementById('modal-distance').value);

      if (editingLogId) {
        const log = logs.find(l => l.id === editingLogId);
        if (log) {
          log.name = selectedItem.name;
          log.type = selectedItem.type;
          log.mode = selectedItem.mode;

          if (selectedItem.mode === 'memo') {
            log.date = document.getElementById('modal-memo-date').value;
            const mealText = document.getElementById('modal-meal').value.trim();
            const calVal = parseInt(document.getElementById('modal-calories').value, 10);
            log.meal = mealText;
            log.calories = (!isNaN(calVal) && calVal > 0) ? calVal : undefined;
            delete log.startDate;
            delete log.endDate;
            delete log.startTime;
            delete log.endTime;
            delete log.distance;

            if (log.calories) {
              const key = mealText || selectedItem.name;
              caloriePresets[key] = log.calories;
            }
          } else {
            log.startDate = document.getElementById('modal-start-date').value;
            log.startTime = document.getElementById('modal-start-time').value;
            log.endDate = document.getElementById('modal-end-date').value;
            log.endTime = document.getElementById('modal-end-time').value;
            log.date = log.startDate;
            log.distance = !isNaN(distVal) && distVal > 0 ? distVal : undefined;
            delete log.meal;
            delete log.calories;
          }
        }
        showToast('変更を保存しました！');
      } else {
        const newLog = {
          id: Date.now().toString(),
          catId: selectedItem.id,
          name: selectedItem.name,
          type: selectedItem.type,
          mode: selectedItem.mode,
          timestamp: new Date().toISOString()
        };

        if (selectedItem.mode === 'memo') {
          newLog.date = document.getElementById('modal-memo-date').value;
          const mealText = document.getElementById('modal-meal').value.trim();
          const calVal = parseInt(document.getElementById('modal-calories').value, 10);
          newLog.meal = mealText;
          if (!isNaN(calVal) && calVal > 0) {
            newLog.calories = calVal;
            const key = mealText || selectedItem.name;
            caloriePresets[key] = calVal;
          }
        } else {
          newLog.startDate = document.getElementById('modal-start-date').value;
          newLog.startTime = document.getElementById('modal-start-time').value;
          newLog.endDate = document.getElementById('modal-end-date').value;
          newLog.endTime = document.getElementById('modal-end-time').value;
          newLog.date = newLog.startDate;
          if (!isNaN(distVal) && distVal > 0) {
            newLog.distance = distVal;
          }
        }

        logs.unshift(newLog);
        showToast(`「${newLog.name}」を記録しました！`);
      }

      saveData();
      closeModal();
      backToRoot();
      render();
    }

    function deleteLog(logId) {
      if (confirm('この記録を削除しますか？')) {
        logs = logs.filter(log => log.id !== logId);
        saveData();
        render();
        showToast('削除しました');
      }
    }

    function addGenre() {
      const input = document.getElementById('new-genre-input');
      const modeSelect = document.getElementById('new-genre-mode');
      const colorInput = document.getElementById('new-genre-color');
      const name = input.value.trim();
      if (!name) return;

      genres.push({
        id: 'genre_' + Date.now(),
        name: name,
        mode: modeSelect.value,
        color: colorInput.value
      });
      input.value = '';
      saveData();
      render();
      showToast(`ジャンル「${name}」を追加しました`);
    }

    function updateGenreName(genreId, newName) {
      const g = genres.find(item => item.id === genreId);
      if (g && newName.trim()) {
        g.name = newName.trim();
        saveData();
        render();
        showToast('ジャンル名を変更しました');
      }
    }

    function updateGenreMode(genreId, newMode) {
      const g = genres.find(item => item.id === genreId);
      if (g) {
        g.mode = newMode;
        saveData();
        render();
        showToast('記録方式を変更しました');
      }
    }

    function updateGenreColor(genreId, newColor) {
      const g = genres.find(item => item.id === genreId);
      if (g) {
        g.color = newColor;
        saveData();
        render();
      }
    }

    function deleteGenre(genreId) {
      if (genres.length <= 1) {
        alert('ジャンルは最低1つ必要です。');
        return;
      }
      if (confirm('このジャンルを削除しますか？')) {
        genres = genres.filter(g => g.id !== genreId);
        saveData();
        render();
      }
    }

    function addCategory() {
      const input = document.getElementById('new-cat-input');
      const typeSelect = document.getElementById('new-cat-type');
      const name = input.value.trim();
      if (!name) return;

      categories.push({
        id: Date.now().toString(),
        name: name,
        type: typeSelect.value
      });
      input.value = '';
      saveData();
      render();
      showToast(`「${name}」を追加しました`);
    }

    function updateCategoryName(catId, newName) {
      const cat = categories.find(c => c.id === catId);
      if (cat && newName.trim()) {
        cat.name = newName.trim();
        saveData();
        showToast('名前を変更しました');
      }
    }

    function updateCategoryType(catId, newType) {
      const cat = categories.find(c => c.id === catId);
      if (cat) {
        cat.type = newType;
        saveData();
        render();
        showToast('ジャンルを変更しました');
      }
    }

    function deleteCategory(catId) {
      if (confirm('このカテゴリを削除しますか？')) {
        categories = categories.filter(c => c.id !== catId);
        saveData();
        render();
      }
    }

    function hexToRgba(hex, alpha) {
      let c = hex.replace('#', '');
      if (c.length === 3) c = c.split('').map(x => x + x).join('');
      const num = parseInt(c, 16);
      return `rgba(${(num >> 16) & 255}, ${(num >> 8) & 255}, ${num & 255}, ${alpha})`;
    }

    function formatLogTime(log) {
      const g = genres.find(item => item.id === log.type);
      const isMemo = (g && g.mode === 'memo') || log.mode === 'memo' || log.type === 'food';
      
      let detailStr = '';
      if (log.distance) {
        detailStr += ` <span class="log-duration" style="background:#f0fdf4; color:#16a34a;">🏃‍♂️ ${log.distance}km</span>`;
      }

      if (isMemo) {
        const parts = log.date ? log.date.split('-') : [];
        const dateStr = parts.length === 3 ? `${Number(parts[1])}/${Number(parts[2])}` : '';
        const label = g ? g.name : 'メモ';
        return `${dateStr} ${label}`;
      } else if (log.startTime) {
        const sParts = (log.startDate || log.date || '').split('-');
        const eParts = (log.endDate || log.startDate || log.date || '').split('-');
        const sDateStr = sParts.length === 3 ? `${Number(sParts[1])}/${Number(sParts[2])}` : '';
        const eDateStr = eParts.length === 3 ? `${Number(eParts[1])}/${Number(eParts[2])}` : '';

        let timeStr = '';
        if (log.startDate && log.endDate && log.startDate !== log.endDate) {
          timeStr = `${sDateStr} ${log.startTime} 〜 ${eDateStr} ${log.endTime}`;
        } else {
          timeStr = `${sDateStr} ${log.startTime} 〜 ${log.endTime}`;
        }

        const diffMin = calcLogDurationMinutes(log);
        let durStr = '';
        if (diffMin >= 60) {
          const h = Math.floor(diffMin / 60);
          const m = diffMin % 60;
          durStr = m > 0 ? `${h}時間${m}分` : `${h}時間`;
        } else if (diffMin > 0) {
          durStr = `${diffMin}分`;
        }
        const durBadge = durStr ? `<span class="log-duration">${durStr}</span>` : '';
        return `${timeStr}${durBadge}${detailStr}`;
      }
      const d = new Date(log.timestamp);
      return `${d.getMonth() + 1}/${d.getDate()} ${String(d.getHours()).padStart(2, '0')}:${String(d.getMinutes()).padStart(2, '0')}${detailStr}`;
    }

    function showToast(msg) {
      const toast = document.getElementById('toast');
      toast.textContent = msg;
      toast.classList.add('show');
      setTimeout(() => toast.classList.remove('show'), 2000);
    }

    function renderCategoriesForGenre() {
      const genre = genres.find(g => g.id === currentGenreId);
      if (!genre) return;

      const titleEl = document.getElementById('selected-genre-title');
      titleEl.textContent = `${genre.name} カテゴリを選択`;
      titleEl.style.color = genre.color || 'var(--primary)';

      const container = document.getElementById('category-buttons');
      container.innerHTML = '';

      const filteredCats = categories.filter(c => (c.type || 'action') === currentGenreId);

      if (filteredCats.length === 0) {
        container.innerHTML = '<div style="grid-column: 1 / -1; color: var(--muted); font-size: 0.9rem; padding: 12px 0;">このジャンルにはまだカテゴリがありません。「カテゴリ編集」から追加してください。</div>';
        return;
      }

      filteredCats.forEach(cat => {
        const count = logs.filter(l => l.catId === cat.id || l.name === cat.name).length;
        const btn = document.createElement('button');
        btn.className = 'action-btn';
        btn.style.setProperty('--genre-color', genre.color || '#2563eb');
        btn.innerHTML = `
          <span>${cat.name}</span>
          <span class="action-count">累計 ${count} 回</span>
        `;
        btn.onclick = () => handleCategoryClick(cat.id, cat.name, genre.id, genre.mode);
        container.appendChild(btn);
      });
    }

    function renderActiveTimers() {
      const container = document.getElementById('active-timers-container');
      container.innerHTML = '';
      if (activeTimers.length === 0) return;

      activeTimers.forEach(timer => {
        const now = Date.now();
        const diffSec = Math.floor((now - timer.startTimestamp) / 1000);
        const hh = String(Math.floor(diffSec / 3600)).padStart(2, '0');
        const mm = String(Math.floor((diffSec % 3600) / 60)).padStart(2, '0');
        const ss = String(diffSec % 60).padStart(2, '0');

        const div = document.createElement('div');
        div.className = 'active-timer-card';
        div.innerHTML = `
          <div>
            <div class="timer-info-name">⏱️ ${timer.name} 計測中...</div>
            <div class="timer-info-time">開始: ${timer.startDate.slice(5)} ${timer.startTime}</div>
          </div>
          <div style="display: flex; align-items: center;">
            <div class="timer-display">${hh}:${mm}:${ss}</div>
            <button class="btn-stop-timer" onclick="stopTimer('${timer.id}')">終了</button>
          </div>
        `;
        container.appendChild(div);
      });
    }

    function render() {
      renderActiveTimers();
      renderGoals();

      const genreContainer = document.getElementById('genre-buttons');
      genreContainer.innerHTML = '';
      genres.forEach(g => {
        const btn = document.createElement('button');
        btn.className = 'tree-root-btn';
        btn.style.setProperty('--genre-color', g.color || '#2563eb');
        btn.textContent = g.name;
        btn.onclick = () => selectGenre(g.id);
        genreContainer.appendChild(btn);
      });

      if (currentGenreId) {
        renderCategoriesForGenre();
      }

      const dailyContainer = document.getElementById('daily-container');
      const totalCountEl = document.getElementById('total-count');
      if (totalCountEl) totalCountEl.textContent = `合計 ${logs.length} 件`;
      dailyContainer.innerHTML = '';

      const dailySplits = {};

      Object.keys(conditions).forEach(d => {
        if (!dailySplits[d]) dailySplits[d] = { logs: [], timeSegments: [] };
      });

      logs.forEach(log => {
        const g = genres.find(item => item.id === log.type);
        const isMemo = (g && g.mode === 'memo') || log.mode === 'memo' || log.type === 'food';

        if (isMemo) {
          const d = log.date || '未分類';
          if (!dailySplits[d]) dailySplits[d] = { logs: [], timeSegments: [] };
          dailySplits[d].logs.push(log);
        } else if (log.startTime && log.endTime) {
          const sDate = log.startDate || log.date;
          const eDate = log.endDate || sDate;

          if (!sDate) return;

          const startDt = new Date(`${sDate}T${log.startTime}`);
          const endDt = new Date(`${eDate}T${log.endTime}`);

          if (endDt <= startDt) {
            endDt.setDate(endDt.getDate() + 1);
          }

          let curr = new Date(startDt);
          while (curr < endDt) {
            const yyyy = curr.getFullYear();
            const mm = String(curr.getMonth() + 1).padStart(2, '0');
            const dd = String(curr.getDate()).padStart(2, '0');
            const dateStr = `${yyyy}-${mm}-${dd}`;

            const dayStart = new Date(`${dateStr}T00:00`);
            const dayEnd = new Date(`${dateStr}T24:00`);

            const segStart = curr > dayStart ? curr : dayStart;
            const segEnd = endDt < dayEnd ? endDt : dayEnd;

            if (segEnd > segStart) {
              const sh = segStart.getHours();
              const sm = segStart.getMinutes();
              const eh = segEnd.getHours();
              const em = segEnd.getMinutes();
              const startHm = `${String(sh).padStart(2, '0')}:${String(sm).padStart(2, '0')}`;
              const endHm = `${String(eh).padStart(2, '0')}:${String(em).padStart(2, '0')}`;

              if (!dailySplits[dateStr]) dailySplits[dateStr] = { logs: [], timeSegments: [] };
              dailySplits[dateStr].timeSegments.push({
                ...log,
                subStartTime: startHm,
                subEndTime: (eh === 0 && em === 0) ? '24:00' : endHm,
                subStartMin: sh * 60 + sm,
                subEndMin: (eh === 0 && em === 0) ? 24 * 60 : eh * 60 + em
              });
            }

            curr = new Date(dayEnd);
          }

          if (!dailySplits[sDate]) dailySplits[sDate] = { logs: [], timeSegments: [] };
          if (!dailySplits[sDate].logs.includes(log)) {
            dailySplits[sDate].logs.push(log);
          }
        }
      });

      const dates = Object.keys(dailySplits).sort().reverse();

      if (dates.length === 0) {
        dailyContainer.innerHTML = '<div class="card" style="color: var(--muted); font-size: 0.9rem; text-align: center; padding: 24px;">まだ記録がありません</div>';
      } else {
        dates.forEach(dateStr => {
          const dayData = dailySplits[dateStr];
          const parts = dateStr.split('-');
          const displayDate = parts.length === 3 ? `${parts[0]}年${Number(parts[1])}月${Number(parts[2])}日` : dateStr;

          const segments = dayData.timeSegments.sort((a, b) => {
            if (a.subStartMin !== b.subStartMin) return a.subStartMin - b.subStartMin;
            return b.subEndMin - a.subEndMin;
          });

          const laneEndTimes = [];
          segments.forEach(seg => {
            let assignedLane = -1;
            for (let i = 0; i < laneEndTimes.length; i++) {
              if (laneEndTimes[i] <= seg.subStartMin) {
                assignedLane = i;
                laneEndTimes[i] = seg.subEndMin;
                break;
              }
            }
            if (assignedLane === -1) {
              assignedLane = laneEndTimes.length;
              laneEndTimes.push(seg.subEndMin);
            }
            seg.lane = assignedLane;
          });

          const laneCount = Math.max(laneEndTimes.length, 1);
          const laneHeight = 28;
          const barTotalHeight = Math.max(laneCount * laneHeight + 4, 36);

          const laneMinutes = {};
          let maxLaneIndex = 0;

          segments.forEach(seg => {
            const duration = seg.subEndMin - seg.subStartMin;
            const lane = seg.lane;
            if (lane > maxLaneIndex) maxLaneIndex = lane;

            if (!laneMinutes[lane]) laneMinutes[lane] = {};
            const g = genres.find(item => item.id === seg.type);
            const gId = g ? g.id : 'other';
            laneMinutes[lane][gId] = (laneMinutes[lane][gId] || 0) + duration;
          });

          const dayCalories = dayData.logs.reduce((sum, log) => sum + (log.calories || 0), 0);
          const dayDistance = dayData.logs.reduce((sum, log) => sum + (log.distance || 0), 0);
          
          const calBadgeHtml = dayCalories > 0 ? `<span class="cal-total-badge">🔥 ${dayCalories.toLocaleString()} kcal</span>` : '';
          const distBadgeHtml = dayDistance > 0 ? `<span class="cal-total-badge" style="background:#f0fdf4; color:#16a34a; border-color:#bbf7d0;">🏃‍♂️ ${dayDistance.toFixed(1)} km</span>` : '';
          const multiBadgeHtml = maxLaneIndex > 0 ? `<span class="multi-total-badge">⚡ 多重並行 (最大 ${maxLaneIndex + 1}層)</span>` : '';

          const dayCard = document.createElement('div');
          dayCard.className = 'timeline-day-card';

          const header = document.createElement('div');
          header.className = 'timeline-date-header';
          header.innerHTML = `<span>${displayDate}</span><div>${multiBadgeHtml} ${distBadgeHtml} ${calBadgeHtml} <span style="font-size:0.75rem; color:var(--muted); margin-left:4px;">${dayData.logs.length}件</span></div>`;
          dayCard.appendChild(header);

          const cond = conditions[dateStr];
          if (cond) {
            const condRow = document.createElement('div');
            condRow.className = 'condition-badge-row';
            let html = '';
            if (cond.weight !== undefined) html += `<span class="cond-badge">⚖️ ${cond.weight} kg</span>`;
            if (cond.body) html += `<span class="cond-badge">💪 体調: ${cond.body}/5</span>`;
            if (cond.mental) html += `<span class="cond-badge">🧠 メンタル: ${cond.mental}/5</span>`;
            condRow.innerHTML = html;
            dayCard.appendChild(condRow);
          }

          const barContainer = document.createElement('div');
          barContainer.className = 'timeline-bar-container';
          barContainer.style.height = `${barTotalHeight}px`;

          segments.forEach(seg => {
            const totalDayMin = 24 * 60;
            const leftPercent = (seg.subStartMin / totalDayMin) * 100;
            const widthPercent = Math.max(((seg.subEndMin - seg.subStartMin) / totalDayMin) * 100, 2);

            const g = genres.find(item => item.id === seg.type);
            const color = g ? (g.color || '#2563eb') : '#2563eb';

            const block = document.createElement('div');
            block.className = 'timeline-block';
            block.style.left = `${leftPercent}%`;
            block.style.width = `${widthPercent}%`;
            block.style.top = `${seg.lane * laneHeight + 2}px`;
            block.style.height = `${laneHeight - 2}px`;
            block.style.setProperty('--block-color', color);
            block.textContent = seg.name;
            barContainer.appendChild(block);
          });

          dayCard.appendChild(barContainer);

          const hours = document.createElement('div');
          hours.className = 'timeline-hours';
          hours.innerHTML = '<span>0:00</span><span>6:00</span><span>12:00</span><span>18:00</span><span>24:00</span>';
          dayCard.appendChild(hours);

          const statsContainer = document.createElement('div');
          statsContainer.className = 'stats-container';

          const donutWrapper = document.createElement('div');
          donutWrapper.className = 'donut-wrapper';

          const totalDayMinutes = 24 * 60;
          const ringWidth = 8;
          const baseInset = 4;

          for (let laneIdx = 0; laneIdx <= maxLaneIndex; laneIdx++) {
            const ring = document.createElement('div');
            ring.className = 'donut-ring';
            const inset = baseInset + (laneIdx * (ringWidth + 2));
            ring.style.inset = `${inset}px`;

            const laneMins = laneMinutes[laneIdx] || {};
            let stops = [];
            let currentAngle = 0;
            let accountedMins = 0;

            Object.keys(laneMins).forEach(gId => {
              const mins = laneMins[gId];
              const angle = Math.min((mins / totalDayMinutes) * 360, 360 - currentAngle);
              const g = genres.find(item => item.id === gId);
              const color = g ? (g.color || '#2563eb') : '#2563eb';
              stops.push(`${color} ${currentAngle}deg ${currentAngle + angle}deg`);
              currentAngle += angle;
              accountedMins += mins;
            });

            if (laneIdx === 0 && accountedMins < totalDayMinutes) {
              const freeMins = totalDayMinutes - accountedMins;
              const freeAngle = (freeMins / totalDayMinutes) * 360;
              stops.push(`#cbd5e1 ${currentAngle}deg ${currentAngle + freeAngle}deg`);
            } else if (currentAngle < 360) {
              stops.push(`transparent ${currentAngle}deg 360deg`);
            }

            ring.style.background = `conic-gradient(${stops.join(', ')})`;
            donutWrapper.appendChild(ring);
          }

          const centerHole = document.createElement('div');
          centerHole.className = 'donut-center-hole';
          donutWrapper.appendChild(centerHole);

          statsContainer.appendChild(donutWrapper);

          const legendBox = document.createElement('div');
          legendBox.className = 'stats-legend';

          const baseTitle = document.createElement('div');
          baseTitle.className = 'legend-section-title';
          baseTitle.textContent = '● 内側リング：ベース時間（第1層）';
          legendBox.appendChild(baseTitle);

          const baseMins = laneMinutes[0] || {};
          let baseTotalMins = 0;
          Object.keys(baseMins).forEach(gId => {
            const mins = baseMins[gId];
            baseTotalMins += mins;
            const pct = ((mins / totalDayMinutes) * 100).toFixed(1);
            const g = genres.find(item => item.id === gId);
            const name = g ? g.name : 'その他';
            const color = g ? (g.color || '#2563eb') : '#2563eb';

            const item = document.createElement('div');
            item.className = 'legend-item';
            item.innerHTML = `
              <div><span class="legend-color-box" style="background:${color};"></span>${name}</div>
              <div style="font-weight:700;">${pct}% <span style="font-size:0.7rem; color:var(--muted); font-weight:normal;">(${Math.floor(mins/60)}h${Math.floor(mins%60)}m)</span></div>
            `;
            legendBox.appendChild(item);
          });

          const freeMins = Math.max(totalDayMinutes - baseTotalMins, 0);
          if (freeMins > 0) {
            const freePct = ((freeMins / totalDayMinutes) * 100).toFixed(1);
            const freeItem = document.createElement('div');
            freeItem.className = 'legend-item';
            freeItem.innerHTML = `
              <div><span class="legend-color-box" style="background:#cbd5e1;"></span>⏳ 空き時間</div>
              <div style="font-weight:700;">${freePct}% <span style="font-size:0.7rem; color:var(--muted); font-weight:normal;">(${Math.floor(freeMins/60)}h${Math.floor(freeMins%60)}m)</span></div>
            `;
            legendBox.appendChild(freeItem);
          }

          for (let laneIdx = 1; laneIdx <= maxLaneIndex; laneIdx++) {
            const laneMins = laneMinutes[laneIdx];
            if (!laneMins) continue;

            const outerTitle = document.createElement('div');
            outerTitle.className = 'legend-section-title';
            outerTitle.textContent = `◎ 外側リング：並行タスク（第${laneIdx + 1}層）`;
            legendBox.appendChild(outerTitle);

            Object.keys(laneMins).forEach(gId => {
              const mins = laneMins[gId];
              const pct = ((mins / totalDayMinutes) * 100).toFixed(1);
              const g = genres.find(item => item.id === gId);
              const name = g ? g.name : 'その他';
              const color = g ? (g.color || '#8b5cf6') : '#8b5cf6';

              const item = document.createElement('div');
              item.className = 'legend-item';
              item.innerHTML = `
                <div><span class="legend-color-box" style="background:${color};"></span>${name}</div>
                <div style="font-weight:700; color:var(--multi-color);">+${pct}% <span style="font-size:0.7rem; color:var(--muted); font-weight:normal;">(${Math.floor(mins/60)}h${Math.floor(mins%60)}m)</span></div>
              `;
              legendBox.appendChild(item);
            });
          }

          statsContainer.appendChild(legendBox);
          dayCard.appendChild(statsContainer);

          if (dayData.logs.length > 0) {
            const logsSection = document.createElement('div');
            logsSection.className = 'day-logs-section';
            logsSection.innerHTML = '<div class="day-logs-title">この日の記録</div>';

            const ul = document.createElement('ul');
            ul.className = 'log-list';

            const sortedDayLogs = [...dayData.logs].sort((a, b) => {
              const timeA = a.startTime || '00:00';
              const timeB = b.startTime || '00:00';
              if (timeA !== timeB) return timeB.localeCompare(timeA);
              return b.id - a.id;
            });

            sortedDayLogs.forEach(log => {
              const li = document.createElement('li');
              li.className = 'log-item';
              let detailHtml = '';
              const g = genres.find(item => item.id === log.type);
              const isMemo = (g && g.mode === 'memo') || log.mode === 'memo' || log.type === 'food';

              if (isMemo) {
                const color = g ? (g.color || '#059669') : '#059669';
                const bg = hexToRgba(color, 0.1);
                if (log.meal) {
                  detailHtml += `<br><span class="log-badge-memo" style="color: ${color}; background: ${bg};">📝 ${log.meal}</span>`;
                }
                if (log.calories) {
                  if (!log.meal) detailHtml += `<br>`;
                  detailHtml += `<span class="log-badge-cal">🔥 ${log.calories} kcal</span>`;
                }
              }

              if (log.distance) {
                detailHtml += `<br><span class="log-duration" style="background:#f0fdf4; color:#16a34a;">🏃‍♂️ 距離: ${log.distance} km</span>`;
              }

              li.innerHTML = `
                <div>
                  <div class="log-name">${log.name}</div>
                  <div class="log-time">${formatLogTime(log)}${detailHtml}</div>
                </div>
                <div class="log-actions">
                  <button class="edit-log-btn" onclick="editLog('${log.id}')">編集</button>
                  <button class="delete-log-btn" onclick="deleteLog('${log.id}')">削除</button>
                </div>
              `;
              ul.appendChild(li);
            });

            logsSection.appendChild(ul);
            dayCard.appendChild(logsSection);
          }

          dailyContainer.appendChild(dayCard);
        });
      }

      const genreEditList = document.getElementById('genre-edit-list');
      genreEditList.innerHTML = '';
      genres.forEach(g => {
        const div = document.createElement('div');
        div.className = 'edit-item';
        div.innerHTML = `
          <input type="color" value="${g.color || '#2563eb'}" onchange="updateGenreColor('${g.id}', this.value)">
          <select onchange="updateGenreMode('${g.id}', this.value)">
            <option value="time" ${g.mode === 'time' ? 'selected' : ''}>時間</option>
            <option value="memo" ${g.mode === 'memo' ? 'selected' : ''}>メモ</option>
          </select>
          <input type="text" value="${g.name}" onchange="updateGenreName('${g.id}', this.value)">
          <button class="btn-danger" onclick="deleteGenre('${g.id}')">削除</button>
        `;
        genreEditList.appendChild(div);
      });

      const newCatTypeSelect = document.getElementById('new-cat-type');
      newCatTypeSelect.innerHTML = '';
      genres.forEach(g => {
        const opt = document.createElement('option');
        opt.value = g.id;
        opt.textContent = g.name;
        newCatTypeSelect.appendChild(opt);
      });

      const editList = document.getElementById('cat-edit-list');
      editList.innerHTML = '';
      categories.forEach(cat => {
        const currentType = cat.type || 'action';
        const div = document.createElement('div');
        div.className = 'edit-item';
        
        let optionsHtml = '';
        genres.forEach(g => {
          optionsHtml += `<option value="${g.id}" ${currentType === g.id ? 'selected' : ''}>${g.name}</option>`;
        });

        div.innerHTML = `
          <select onchange="updateCategoryType('${cat.id}', this.value)">
            ${optionsHtml}
          </select>
          <input type="text" value="${cat.name}" onchange="updateCategoryName('${cat.id}', this.value)">
          <button class="btn-danger" onclick="deleteCategory('${cat.id}')">削除</button>
        `;
        editList.appendChild(div);
      });
    }

    setInterval(() => {
      if (activeTimers.length > 0) {
        renderActiveTimers();
      }
    }, 1000);

    initConditionDate();
    render();
  </script>
</body>
</html>

