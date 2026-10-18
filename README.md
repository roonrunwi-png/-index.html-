# -index.html-
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>To-do List</title>
  <style>
    * { box-sizing: border-box; }

    body {
      margin: 0;
      min-height: 100vh;
      font-family: "Segoe UI", Tahoma, sans-serif;
      background: #f3f4f6;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      padding: 40px 16px;
    }

    .app {
      width: 100%;
      max-width: 480px;
      background: #ffffff;
      border-radius: 16px;
      padding: 24px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
    }

    h1 {
      margin: 0 0 20px;
      font-size: 24px;
      color: #111827;
      text-align: center;
    }

    form {
      display: flex;
      gap: 8px;
      margin-bottom: 20px;
    }

    input {
      flex: 1;
      padding: 12px;
      border: 1px solid #d1d5db;
      border-radius: 10px;
      font-size: 16px;
    }

    input:focus {
      outline: 2px solid #2563eb;
      border-color: transparent;
    }

    button {
      padding: 12px 18px;
      border: none;
      border-radius: 10px;
      background: #2563eb;
      color: white;
      font-size: 16px;
      cursor: pointer;
    }

    button:hover {
      background: #1d4ed8;
    }

    ul {
      list-style: none;
      margin: 0;
      padding: 0;
      display: grid;
      gap: 10px;
    }

    li {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 12px;
      border: 1px solid #e5e7eb;
      border-radius: 10px;
      background: #f9fafb;
    }

    li.done span {
      text-decoration: line-through;
      color: #9ca3af;
    }

    li span {
      flex: 1;
    }

    .delete {
      background: #ef4444;
      padding: 8px 12px;
    }

    .delete:hover {
      background: #dc2626;
    }

    .empty {
      text-align: center;
      color: #6b7280;
    }
  </style>
</head>
<body>
  <main class="app">
    <h1>รายการสิ่งที่ต้องทำ</h1>

    <form id="todo-form">
      <input
        id="todo-input"
        type="text"
        placeholder="พิมพ์งานที่ต้องทำ..."
        autocomplete="off"
        required
      />
      <button type="submit">เพิ่ม</button>
    </form>

    <ul id="todo-list"></ul>
    <p id="empty-message" class="empty">ยังไม่มีรายการ</p>
  </main>

  <script>
    const form = document.querySelector("#todo-form");
    const input = document.querySelector("#todo-input");
    const list = document.querySelector("#todo-list");
    const emptyMessage = document.querySelector("#empty-message");

    const todos = [];

    function renderTodos() {
      list.innerHTML = "";

      todos.forEach((todo, index) => {
        const item = document.createElement("li");
        if (todo.completed) item.classList.add("done");

        const checkbox = document.createElement("input");
        checkbox.type = "checkbox";
        checkbox.checked = todo.completed;
        checkbox.addEventListener("change", () => {
          todos[index].completed = checkbox.checked;
          renderTodos();
        });

        const text = document.createElement("span");
        text.textContent = todo.text;

        const deleteButton = document.createElement("button");
        deleteButton.textContent = "ลบ";
        deleteButton.className = "delete";
        deleteButton.addEventListener("click", () => {
          todos.splice(index, 1);
          renderTodos();
        });

        item.append(checkbox, text, deleteButton);
        list.appendChild(item);
      });

      emptyMessage.style.display = todos.length === 0 ? "block" : "none";
    }

    form.addEventListener("submit", (event) => {
      event.preventDefault();

      const text = input.value.trim();
      if (!text) return;

      todos.push({ text, completed: false });
      input.value = "";
      input.focus();

      renderTodos();
    });

    renderTodos();
  </script>
</body>
</html>
