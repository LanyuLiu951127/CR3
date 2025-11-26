# AI 產出：CR3 檢查系統前端範本 (Flask/Jinja2)

此檔案包含 CR3 相同照片檢查系統 Web 版的前端 HTML/CSS 程式碼。這些範本是為 Python Flask 框架設計的，並使用了 Jinja2 樣板引擎語法。

您應將這些檔案儲存於 Flask 專案的 `templates` 資料夾下。

---

## 1. `templates/base.html` (基礎樣板)

這是一個基礎版面，所有其他頁面都會繼承它。它定義了整體的 HTML 結構、頁首、頁尾以及通用的 CSS 樣式。

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}CR3 檢查系統{% endblock %}</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
            background-color: #f0f2f5;
            color: #333;
            margin: 0;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }
        .container {
            max-width: 900px;
            margin: 2rem auto;
            padding: 2rem;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            flex-grow: 1;
        }
        header {
            background-color: #0056b3;
            color: white;
            padding: 1rem 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        header a {
            color: white;
            text-decoration: none;
            font-weight: 500;
        }
        h1, h2 {
            color: #0056b3;
            text-align: center;
        }
        .flash-messages {
            list-style: none;
            padding: 0;
            margin: 1rem 0;
        }
        .flash-messages .success { background-color: #d4edda; color: #155724; padding: 0.75rem; border-radius: 4px; }
        .flash-messages .error { background-color: #f8d7da; color: #721c24; padding: 0.75rem; border-radius: 4px; }
        .form-group { margin-bottom: 1rem; }
        label { display: block; margin-bottom: 0.5rem; font-weight: 500; }
        input[type="text"], input[type="password"], input[type="email"] {
            width: calc(100% - 20px);
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        .btn {
            display: inline-block;
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            background-color: #007bff;
            color: white;
            text-align: center;
            text-decoration: none;
            cursor: pointer;
            font-size: 1rem;
        }
        .btn:hover { background-color: #0056b3; }
        footer {
            text-align: center;
            padding: 1rem;
            background-color: #343a40;
            color: white;
            margin-top: auto;
        }
    </style>
</head>
<body>
    <header>
        <a href="{{ url_for('index') }}"><h1>CR3 檢查系統</h1></a>
        <nav>
            {% if g.user %}
                <span>歡迎, {{ g.user.username }}</span> &nbsp;
                <a href="{{ url_for('logout') }}">登出</a>
            {% else %}
                <a href="{{ url_for('login') }}">登入</a> &nbsp;
                <a href="{{ url_for('register') }}">註冊</a>
            {% endif %}
        </nav>
    </header>

    <div class="container">
        {% with messages = get_flashed_messages(with_categories=true) %}
            {% if messages %}
                <ul class="flash-messages">
                {% for category, message in messages %}
                    <li class="{{ category }}">{{ message }}</li>
                {% endfor %}
                </ul>
            {% endif %}
        {% endwith %}
        {% block content %}{% endblock %}
    </div>

    <footer>
        &copy; 2025 CR3 Checker
    </footer>
</body>
</html>
```

---

## 2. `templates/login.html` (登入頁面)

```html
{% extends "base.html" %}

{% block title %}登入 - {{ super() }}{% endblock %}

{% block content %}
    <h2>使用者登入</h2>
    <form method="post">
        <div class="form-group">
            <label for="username">使用者名稱</label>
            <input type="text" id="username" name="username" required>
        </div>
        <div class="form-group">
            <label for="password">密碼</label>
            <input type="password" id="password" name="password" required>
        </div>
        <button type="submit" class="btn">登入</button>
    </form>
{% endblock %}
```

---

## 3. `templates/register.html` (註冊頁面)

```html
{% extends "base.html" %}

{% block title %}註冊 - {{ super() }}{% endblock %}

{% block content %}
    <h2>建立新帳號</h2>
    <form method="post">
        <div class="form-group">
            <label for="username">使用者名稱</label>
            <input type="text" id="username" name="username" required>
        </div>
        <div class="form-group">
            <label for="email">電子郵件</label>
            <input type="email" id="email" name="email" required>
        </div>
        <div class="form-group">
            <label for="password">密碼</label>
            <input type="password" id="password" name="password" required>
        </div>
        <button type="submit" class="btn">註冊</button>
    </form>
{% endblock %}
```

---

## 4. `templates/index.html` (主操作頁面)

```html
{% extends "base.html" %}

{% block title %}主頁 - {{ super() }}{% endblock %}

{% block content %}
    <h2>相同照片檢查</h2>
    <p>請選擇包含 CR3 檔案的資料夾，然後點擊開始檢查。</p>
    
    <form id="upload-form" action="{{ url_for('check_photos') }}" method="post" enctype="multipart/form-data">
        <div class="form-group">
            <label for="photo_folder">選擇資料夾</label>
            <!-- 注意: 'webkitdirectory' 屬性讓使用者可選擇整個資料夾 -->
            <input type="file" id="photo_folder" name="files[]" webkitdirectory multiple required>
        </div>
        <button type="submit" class="btn">開始檢查</button>
    </form>

    <hr style="margin: 2rem 0;">

    <div id="results">
        <h2>檢查結果</h2>
        {% if results %}
            {% for group_hash, photos in results.items() %}
                <div class="photo-group">
                    <h3>第 {{ loop.index }} 組相同照片</h3>
                    <div class="photo-container">
                        {% for photo in photos %}
                            <div class="photo-item">
                                <!-- 這裡應顯示圖片縮圖，後端需提供縮圖路徑 -->
                                <img src="{{ url_for('get_thumbnail', filename=photo.thumbnail_path) }}" alt="{{ photo.name }}">
                                <p>{{ photo.name }}</p>
                            </div>
                        {% endfor %}
                    </div>
                </div>
            {% endfor %}
        {% else %}
            <p>尚未有檢查結果，或未發現相同照片。</p>
        {% endif %}
    </div>

    <style>
        .photo-group {
            margin-bottom: 2rem;
            padding: 1rem;
            border: 1px solid #ddd;
            border-radius: 8px;
        }
        .photo-container {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
        }
        .photo-item {
            text-align: center;
        }
        .photo-item img {
            max-width: 200px;
            max-height: 200px;
            border-radius: 4px;
            border: 1px solid #ccc;
        }
    </style>
{% endblock %}
```
