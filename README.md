# Python--002
my04
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>简易待办事项</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 600px; margin: 20px auto; padding: 0 20px; }
        .todo-container { margin: 20px 0; }
        .todo-item { display: flex; align-items: center; margin: 10px 0; padding: 10px; background: #f5f5f5; border-radius: 5px; }
        .todo-item input { margin-right: 10px; }
        .todo-item span { flex: 1; }
        .todo-item button { background: #ff4444; color: white; border: none; border-radius: 3px; padding: 5px 10px; cursor: pointer; }
    </style>
</head>
<body>
    <h1>待办事项清单</h1>
    <input type="text" id="todo-input" placeholder="输入待办事项...">
    <button onclick="addTodo()">添加事项</button>
    <div class="todo-container" id="todo-list"></div>

    <script>
        // 从本地存储加载待办事项
        let todos = JSON.parse(localStorage.getItem('todos')) || [];
        
        // 渲染待办事项到页面
        function renderTodos() {
            const todoList = document.getElementById('todo-list');
            todoList.innerHTML = '';
            todos.forEach((todo, index) => {
                const item = document.createElement('div');
                item.className = 'todo-item';
                item.innerHTML = `
                    <input type="checkbox" ${todo.done ? 'checked' : ''} onchange="toggleDone(${index})">
                    <span style="${todo.done ? 'text-decoration: line-through; color: #999;' : ''}">${todo.text}</span>
                    <button onclick="deleteTodo(${index})">删除</button>
                `;
                todoList.appendChild(item);
            });
            // 保存到本地存储
            localStorage.setItem('todos', JSON.stringify(todos));
        }

        // 添加新待办事项
        function addTodo() {
            const input = document.getElementById('todo-input');
            const text = input.value.trim();
            if (text) {
                todos.push({ text, done: false });
                input.value = '';
                renderTodos();
            } else {
                alert('请输入待办事项内容！');
            }
        }

        // 切换事项完成状态
        function toggleDone(index) {
            todos[index].done = !todos[index].done;
            renderTodos();
        }

        // 删除待办事项
        function deleteTodo(index) {
            todos.splice(index, 1);
            renderTodos();
        }

        // 页面加载时渲染
        window.onload = renderTodos;
    </script>
</body>
</html>
