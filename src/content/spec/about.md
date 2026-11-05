---
title: 关于我
description: Aken的个人主页与学习进度
---

<!-- 1. 样式部分 -->
<style>
  /* 倒计时卡片样式 */
  .countdown-card {
    text-align: center;
    background: linear-gradient(135deg, #2980b9, #2c3e50);
    color: white;
    padding: 20px;
    border-radius: 12px;
    margin-bottom: 20px;
  }
  .countdown-card h3 {
    color: white !important;
  }
  .countdown-timer {
    display: flex;
    justify-content: center;
    gap: 15px;
    margin-top: 15px;
  }
  .time-box {
    background: rgba(255, 255, 255, 0.15);
    padding: 10px 15px;
    border-radius: 8px;
    min-width: 60px;
  }
  .time-box .number {
    font-size: 24px;
    font-weight: bold;
    display: block;
  }
  .time-box .label {
    font-size: 12px;
    opacity: 0.8;
  }

  /* 通用卡片容器 */
  .custom-card {
    background-color: #f8f9fa;
    border-radius: 12px;
    padding: 20px;
    margin-top: 25px;
    border: 1px solid #e9ecef;
  }
  .custom-card h3 {
    margin-top: 0;
    color: #2c3e50;
  }

  /* Todo List 样式 */
  .todo-input-group {
    display: flex;
    gap: 10px;
    margin-bottom: 15px;
  }
  .todo-input-group input {
    flex: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 6px;
    font-size: 14px;
  }
  .todo-list {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  .todo-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    background-color: #fff;
    border: 1px solid #eee;
    border-radius: 6px;
    margin-bottom: 8px;
  }
  .todo-item span {
    cursor: pointer;
    color: #333;
  }
  .todo-item.completed span {
    text-decoration: line-through;
    color: #888;
  }
  .delete-btn {
    background-color: #e74c3c;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 4px;
    cursor: pointer;
  }
  .delete-btn:hover {
    background-color: #c0392b;
  }

  /* 打卡日历样式 */
  .calendar-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 15px;
  }
  .calendar-grid {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 8px;
    text-align: center;
  }
  .calendar-day-head {
    font-weight: bold;
    color: #7f8c8d;
    font-size: 14px;
    padding-bottom: 5px;
  }
  .calendar-day {
    padding: 12px 0;
    background: #fff;
    border-radius: 8px;
    border: 1px solid #e9ecef;
    font-size: 14px;
    cursor: pointer;
    transition: all 0.2s;
    user-select: none;
  }
  .calendar-day:hover {
    border-color: #2980b9;
  }
  .calendar-day.empty {
    background: transparent;
    border: none;
    cursor: default;
  }
  .calendar-day.today {
    border: 2px solid #2980b9;
    font-weight: bold;
  }
  .calendar-day.checked {
    background-color: #27ae60 !important;
    color: white !important;
    font-weight: bold;
    border-color: #27ae60;
  }
  .streak-info {
    font-size: 14px;
    color: #27ae60;
    font-weight: bold;
  }
</style>

<!-- 2. HTML 结构部分 -->
<p>大家好，我是<strong>Aken</strong>。我现在正在备考法考，同时也非常热爱网页制作和各种软件工具！</p>

<!-- 法考倒计时卡片 -->
<div class="card countdown-card">
  <h3>⏳ 距离2027法考客观题考试还有</h3>
  <div class="countdown-timer">
    <div class="time-box">
      <span id="days" class="number">00</span>
      <span class="label">天</span>
    </div>
    <div class="time-box">
      <span id="hours" class="number">00</span>
      <span class="label">时</span>
    </div>
    <div class="time-box">
      <span id="minutes" class="number">00</span>
      <span class="label">分</span>
    </div>
    <div class="time-box">
      <span id="seconds" class="number">00</span>
      <span class="label">秒</span>
    </div>
  </div>
</div>

<!-- 新增：法考打卡日历卡片 -->
<div class="custom-card">
  <div class="calendar-header">
    <h3 id="calendar-month-title" style="margin: 0;">📅 法考复习打卡日历</h3>
    <span class="streak-info">🔥 累计打卡：<span id="total-checks">0</span> 天</span>
  </div>
  <p style="font-size: 13px; color: #666; margin-top: 0;">点击当天的日期格子即可打卡/取消打卡（打卡记录将同步保存在本地）</p>
  <div class="calendar-grid" id="calendar-grid">
    <!-- 星期表头 -->
    <div class="calendar-day-head">日</div>
    <div class="calendar-day-head">一</div>
    <div class="calendar-day-head">二</div>
    <div class="calendar-day-head">三</div>
    <div class="calendar-day-head">四</div>
    <div class="calendar-day-head">五</div>
    <div class="calendar-day-head">六</div>
    <!-- 日期格子由 JS 动态生成 -->
  </div>
</div>

<!-- 待办事项卡片 -->
<div class="custom-card">
  <h3>📝 我的待办事项（法考/学习进度）</h3>
  
  <div class="todo-input-group">
    <input type="text" id="todo-input" placeholder="输入新的待办事项，如：背民法法条...">
    <button id="add-btn" style="background-color: #2980b9; color: white; padding: 10px 20px; border: none; border-radius: 6px; cursor: pointer; font-weight: bold;">添加</button>
  </div>

  <ul id="todo-list" class="todo-list">
    <!-- JS 会动态把 <li> 放到这里 -->
  </ul>
</div>

<!-- 3. JavaScript 交互逻辑 -->
<script>
  // 1. 法考倒计时逻辑
  function updateCountdown() {
    const targetDate = new Date('2027-09-19T00:00:00').getTime();
    const now = new Date().getTime();
    const timeLeft = targetDate - now;

    if (timeLeft > 0) {
      const days = Math.floor(timeLeft / (1000 * 60 * 60 * 24));
      const hours = Math.floor((timeLeft % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
      const minutes = Math.floor((timeLeft % (1000 * 60 * 60)) / (1000 * 60));
      const seconds = Math.floor((timeLeft % (1000 * 60)) / 1000);

      const dEl = document.getElementById('days');
      const hEl = document.getElementById('hours');
      const mEl = document.getElementById('minutes');
      const sEl = document.getElementById('seconds');

      if (dEl) dEl.textContent = days;
      if (hEl) hEl.textContent = String(hours).padStart(2, '0');
      if (mEl) mEl.textContent = String(minutes).padStart(2, '0');
      if (sEl) sEl.textContent = String(seconds).padStart(2, '0');
    }
  }
  updateCountdown();
  setInterval(updateCountdown, 1000);

  // 2. 打卡日历核心逻辑
  const calendarGrid = document.getElementById('calendar-grid');
  const monthTitle = document.getElementById('calendar-month-title');
  const totalChecksEl = document.getElementById('total-checks');

  // 读取已打卡的日期数组（格式如：["2026-11-05", "2026-11-06"]）
  let checkIns = JSON.parse(localStorage.getItem('law_exam_checkins')) || [];

  function renderCalendar() {
    const now = new Date();
    const year = now.getFullYear();
    const month = now.getMonth(); // 0 - 11
    const todayStr = `${year}-${String(month + 1).padStart(2, '0')}-${String(now.getDate()).padStart(2, '0')}`;

    if (monthTitle) {
      monthTitle.textContent = `📅 ${year}年${month + 1}月 法考打卡日历`;
    }

    // 保留星期表头，清空后面的日期格子
    const dayHeads = Array.from(calendarGrid.querySelectorAll('.calendar-day-head'));
    calendarGrid.innerHTML = '';
    dayHeads.forEach(head => calendarGrid.appendChild(head));

    // 计算当月第一天是星期几 & 当月共有多少天
    const firstDayIndex = new Date(year, month, 1).getDay();
    const totalDays = new Date(year, month + 1, 0).getDate();

    // 填充当月第一天之前的空白格子
    for (let i = 0; i < firstDayIndex; i++) {
      const emptyDiv = document.createElement('div');
      emptyDiv.className = 'calendar-day empty';
      calendarGrid.appendChild(emptyDiv);
    }

    // 生成日期格子
    for (let d = 1; d <= totalDays; d++) {
      const dayStr = `${year}-${String(month + 1).padStart(2, '0')}-${String(d).padStart(2, '0')}`;
      const dayDiv = document.createElement('div');
      dayDiv.className = 'calendar-day';
      dayDiv.textContent = d;

      // 标记今天
      if (dayStr === todayStr) {
        dayDiv.classList.add('today');
      }

      // 标记已打卡
      if (checkIns.includes(dayStr)) {
        dayDiv.classList.add('checked');
      }

      // 点击切换打卡状态
      dayDiv.addEventListener('click', function() {
        if (checkIns.includes(dayStr)) {
          // 如果已打卡，取消打卡
          checkIns = checkIns.filter(item => item !== dayStr);
        } else {
          // 未打卡，添加打卡记录
          checkIns.push(dayStr);
        }
        localStorage.setItem('law_exam_checkins', JSON.stringify(checkIns));
        renderCalendar();
      });

      calendarGrid.appendChild(dayDiv);
    }

    if (totalChecksEl) {
      totalChecksEl.textContent = checkIns.length;
    }
  }

  if (calendarGrid) {
    renderCalendar();
  }

  // 3. Todo List 逻辑
  const todoInput = document.getElementById('todo-input');
  const addBtn = document.getElementById('add-btn');
  const todoList = document.getElementById('todo-list');

  let todos = JSON.parse(localStorage.getItem('my_todos')) || [];

  function renderTodos() {
    if (!todoList) return;
    todoList.innerHTML = '';

    todos.forEach(function(todo, index) {
      const li = document.createElement('li');
      li.className = 'todo-item';
      if (todo.completed) {
        li.classList.add('completed');
      }

      li.innerHTML = `
        <span>${todo.text}</span>
        <button class="delete-btn">删除</button>
      `;

      li.querySelector('span').addEventListener('click', function() {
        todos[index].completed = !todos[index].completed;
        saveAndRenderTodos();
      });

      li.querySelector('.delete-btn').addEventListener('click', function() {
        todos.splice(index, 1);
        saveAndRenderTodos();
      });

      todoList.appendChild(li);
    });
  }

  function saveAndRenderTodos() {
    localStorage.setItem('my_todos', JSON.stringify(todos));
    renderTodos();
  }

  renderTodos();

  if (addBtn) {
    addBtn.addEventListener('click', function() {
      const text = todoInput.value.trim(); 
      if (text === '') {
        alert('请输入待办内容！');
        return;
      }

      todos.push({
        text: text,
        completed: false
      });

      todoInput.value = '';
      saveAndRenderTodos();
    });
  }
</script>
