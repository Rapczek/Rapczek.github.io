<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Enter code</title>
  <style>
    :root{font-family:Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;}
    body{display:flex;align-items:center;justify-content:center;min-height:100vh;margin:0;background:#f3f4f6}
    .card{background:white;padding:28px;border-radius:12px;box-shadow:0 8px 20px rgba(16,24,40,0.08);width:320px;}
    h1{font-size:18px;margin:0 0 12px}
    .row{display:flex;gap:8px}
    input[type="text"]{flex:1;padding:10px 12px;border:1px solid #d1d5db;border-radius:8px;font-size:15px}
    button{padding:10px 14px;border-radius:8px;border:0;background:#111827;color:white;font-weight:600;cursor:pointer}
    .msg{margin-top:14px;font-size:15px;min-height:1.2em}
    .msg.success{color:#065f46}
    .msg.error{color:#b91c1c}
    .small{font-size:13px;color:#6b7280;margin-top:8px}
  </style>
</head>
<body>
  <main class="card" role="main">
    <h1>Enter code</h1>
    <div class="row">
      <label for="code" class="sr-only">Code</label>
      <input id="code" type="text" inputmode="numeric" placeholder="Enter code..." aria-label="Enter code" />
      <button id="check">Check</button>
    </div>
    <div id="message" class="msg" aria-live="polite"></div>
  </main>

  <script>
    (function(){
      const input = document.getElementById('code');
      const btn = document.getElementById('check');
      const msg = document.getElementById('message');
      const SECRET = '2001';

      function show(text, type){
        msg.textContent = text;
        msg.className = 'msg ' + (type === 'success' ? 'success' : (type === 'error' ? 'error' : ''));
      }

      function check(){
        const val = input.value.trim();
        if(!val){ show('Please enter a code.', 'error'); return; }
        if(val === SECRET){ show('Barry kintzerhad', 'success'); }
        else{ show('Incorrect code.', 'error'); }
      }

      btn.addEventListener('click', check);
      input.addEventListener('keydown', function(e){ if(e.key === 'Enter') check(); });
    })();
  </script>
</body>
</html>
