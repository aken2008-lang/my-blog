---
title: 关于我
description: Aken的个人主页与学习进度
---

<!-- 1. 样式部分：直接保留你网页的 CSS 美化 -->
<style>
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
</style>

<!-- 2. HTML 结构部分 -->
<p>你好，我是<strong>aken</strong>。我现在<strong>52岁</strong>，正在备考法考，同时也非常热爱网页制作和各种软件工具！</p>

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

<!-- 待办事项卡片 -->
<div class="card" style="background-color: #f8f9fa; border-radius: 12px; padding: 20px; margin-top: 25px; border: 1px solid #e9ecef;">
  <h3 style="margin-top: 0; color: #2c3e50;">📝 我的待办事项（法考/学习进度）</h3>
  
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

  // 2. 带 LocalStorage 记忆功能的 Todo List 逻辑
  const todoInput = document.getElementById('todo-input');
  const addBtn = document.getElementById('add-btn');
  const todoList = document.getElementById('todo-list');

  // 从本地存储中读取数据，如果没有就初始化为一个空数组
  let todos = JSON.parse(localStorage.getItem('my_todos')) || [];

  // 渲染函数：把数组里的待办事项画到页面上
  function renderTodos() {
    todoList.innerHTML = ''; // 每次渲染前先清空页面列表

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

      // 点击文字：切换完成状态并保存
      const span = li.querySelector('span');
      span.addEventListener('click', function() {
        todos[index].completed = !todos[index].completed;
        saveAndRender();
      });

      // 点击删除：从数组中删掉这项并保存
      const deleteBtn = li.querySelector('.delete-btn');
      deleteBtn.addEventListener('click', function() {
        todos.splice(index, 1); // 从数组中移除第 index 个元素
        saveAndRender();
      });

      todoList.appendChild(li);
    });
  }

  // 保存到本地存储并重新渲染页面的封装函数
  function saveAndRender() {
    localStorage.setItem('my_todos', JSON.stringify(todos));
    renderTodos();
  }

  // 页面刚加载时，先渲染一次本地存储里的数据
  renderTodos();

  // 点击“添加”按钮触发
  if (addBtn) {
    addBtn.addEventListener('click', function() {
      const text = todoInput.value.trim(); 
      if (text === '') {
        alert('请输入待办内容！');
        return;
      }

      // 往数组里推入一个新的待办对象
      todos.push({
        text: text,
        completed: false
      });

      todoInput.value = ''; // 清空输入框
      saveAndRender();      // 保存并刷新界面
    });
  }
</script>
