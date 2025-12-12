<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>My GitHub Repository</title>
<meta name="viewport" content="width=device-width, initial-scale=1">

<style>
  /* ====== Общий стиль ====== */
  body {
    margin: 0;
    font-family: "Inter", Arial, sans-serif;
    background: linear-gradient(135deg, #0d1117 0%, #161b22 100%);
    color: #c9d1d9;
    overflow-x: hidden;
  }

  /* ====== Анимация появления ====== */
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(15px); }
    to { opacity: 1; transform: translateY(0); }
  }

  header {
    text-align: center;
    padding: 90px 20px 50px;
    animation: fadeIn 1.1s ease-out;
  }

  h1 {
    font-size: 48px;
    background: linear-gradient(90deg, #58a6ff, #a371f7);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 10px;
  }

  p.subtitle {
    font-size: 20px;
    opacity: 0.75;
  }

  /* ====== Карточки (NEO GLASS UI) ====== */
  .container {
    max-width: 960px;
    margin: 0 auto;
    padding: 0 20px 80px;
  }

  .card {
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 255, 255, 0.08);
    border-radius: 14px;
    padding: 28px;
    margin-bottom: 30px;
    backdrop-filter: blur(10px);
    box-shadow: 0 10px 30px rgba(0,0,0,0.25);
    animation: fadeIn 1s ease-out;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

  .card:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 38px rgba(0,0,0,0.35);
  }

  h2 {
    color: #79c0ff;
    margin-top: 0;
    margin-bottom: 15px;
    font-size: 24px;
  }

  pre {
    background: #0d1117;
    border: 1px solid #30363d;
    padding: 14px;
    border-radius: 8px;
    overflow-x: auto;
  }

  code { color: #58a6ff; font-size: 15px; }

  a {
    color: #58a6ff;
    text-decoration: none;
  }

  a:hover {
    text-decoration: underline;
  }

  footer {
    text-align: center;
    padding: 40px;
    opacity: 0.55;
    font-size: 14px;
  }

</style>
</head>

<body>

<header>
  <h1>My GitHub Repository</h1>
  <p class="subtitle">Modern. Clean. Fast. Built for GitHub Pages.</p>
</header>

<div class="container">

  <div class="card">
    <h2>📌 Project Overview</h2>
    <p>This repository hosts a new project built using clean and modern principles.  
       The page is optimized for GitHub Pages and designed for easy customization.</p>
  </div>

  <div class="card">
    <h2>🚀 Clone This Repository</h2>
    <pre><code>git clone https://github.com/USERNAME/REPOSITORY.git
cd REPOSITORY</code></pre>
  </div>

  <div class="card">
    <h2>📁 Folder Structure</h2>
    <pre><code>/  
├── index.html  
├── src/  
│   └── app.js  
└── docs/  
    └── readme.md</code></pre>
  </div>

  <div class="card">
    <h2>🔗 Useful Links</h2>
    <p>Repository URL:<br>
      <a href="#">https://github.com/USERNAME/REPOSITORY</a></p>

    <p>GitHub Pages URL:<br>
      <code>https://USERNAME.github.io/REPOSITORY/</code></p>
  </div>

</div>

<footer>
  © 2025 — Enhanced GitHub Repo Template
</footer>

</body>
</html>
