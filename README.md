<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
  <meta name="theme-color" content="#0a0d16" />
  <title>Навигатор Призвания — ИИ‑наставник</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Ruslan+Display&family=Inter:wght@300;400;600;700&display=swap');

    :root{
      --bg:#0a0d16;
      --bg-2:#0e1222;
      --text:#e6e8ef;
      --muted:#9aa3b2;
      --accent:#8b5cf6; /* violet */
      --accent-2:#06b6d4; /* cyan */
      --success:#22c55e;
      --danger:#ef4444;
      --card:rgba(255,255,255,0.06);
      --card-2:rgba(255,255,255,0.08);
      --glass:rgba(12,16,30,0.55);
      --border:rgba(255,255,255,0.18);
      --ring:rgba(139,92,246,0.35);
      --shadow:0 10px 30px rgba(0,0,0,0.35);
    }

    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      color:var(--text);
      background: radial-gradient(1200px 600px at 20% -20%, #1a1440 0%, transparent 60%),
                  radial-gradient(900px 500px at 80% -10%, #062933 0%, transparent 55%),
                  linear-gradient(180deg, var(--bg) 0%, var(--bg-2) 100%);
      font-family: Inter, system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, "Helvetica Neue", Arial, "Noto Sans", "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
      line-height:1.6;
      overflow:hidden;
    }

    /* Mystic background image */
    .bg-image{
      position:fixed; inset:0; z-index:-3;
      background-image:url('https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?q=80&w=1920&auto=format&fit=crop');
      background-size:cover; background-position:center;
      filter:contrast(1.05) brightness(0.55) saturate(1.1);
      opacity:0.35;
    }

    /* Stars canvas */
    canvas#stars{
      position:fixed; inset:0; z-index:-1; opacity:0.7; pointer-events:none;
    }

    /* Subtle noise */
    .noise{
      position:fixed; inset:0; z-index:-2; pointer-events:none;
      background-image:url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" opacity="0.05" width="140" height="140" viewBox="0 0 140 140"><filter id="n"><feTurbulence type="fractalNoise" baseFrequency="0.65" numOctaves="3" stitchTiles="stitch"/></filter><rect width="100%" height="100%" filter="url(%23n)"/></svg>');
      mix-blend-mode:soft-light;
    }

    .container{
      position:relative;
      max-width:980px;
      margin:0 auto;
      padding:20px clamp(14px,4vw,28px);
      height:100%;
      display:flex;
      flex-direction:column;
      gap:16px;
    }

    header.site{
      position:sticky; top:0;
      display:flex; align-items:center; justify-content:space-between;
      padding:14px 16px;
      background:linear-gradient(180deg, rgba(10,13,22,0.7) 0%, rgba(10,13,22,0) 100%);
      backdrop-filter:saturate(120%) blur(10px);
      border-radius:16px;
      border:1px solid var(--border);
      box-shadow:var(--shadow);
      z-index:5;
    }

    .brand{
      display:flex; gap:12px; align-items:center;
    }
    .sigil{
      width:40px; height:40px; border-radius:12px;
      background:
        radial-gradient(circle at 30% 30%, rgba(255,255,255,0.2), transparent 40%),
        linear-gradient(135deg, rgba(139,92,246,0.55), rgba(6,182,212,0.55));
      display:grid; place-items:center;
      border:1px solid var(--border);
      box-shadow:0 6px 16px rgba(139,92,246,0.35), inset 0 0 0 1px rgba(255,255,255,0.12);
    }
    .sigil span{
      font-family:"Ruslan Display", serif;
      font-size:22px; letter-spacing:1px;
      color:#f5f5ff;
      text-shadow:0 0 12px rgba(139,92,246,0.6);
      transform:translateY(1px);
    }
    .ttl{
      display:flex; flex-direction:column; line-height:1.2;
    }
    .ttl h1{
      margin:0; font-size:clamp(18px,2.2vw,24px);
      font-family:"Ruslan Display", serif; letter-spacing:0.3px;
    }
    .ttl small{
      color:var(--muted); font-size:12px;
    }

    .hint{
      display:none;
    }
    @media (min-width:700px){
      .hint{
        display:flex; gap:8px; align-items:center; color:var(--muted); font-size:13px;
        opacity:0.9;
      }
      .pill{
        background:linear-gradient(180deg, rgba(139,92,246,0.15), rgba(6,182,212,0.15));
        border:1px solid var(--border);
        padding:6px 10px; border-radius:999px;
      }
    }

    .chat{
      flex:1 1 auto;
      overflow:auto;
      scroll-behavior:smooth;
      padding:6px;
      display:flex; flex-direction:column; gap:14px;
    }
    .msg{
      display:flex; gap:10px; align-items:flex-start;
      opacity:0; transform:translateY(8px);
      animation:in 420ms ease forwards;
    }
    @keyframes in{to{opacity:1; transform:translateY(0)}}

    .avatar{
      width:36px; height:36px; flex:0 0 36px; border-radius:10px;
      display:grid; place-items:center;
      color:#fff; font-size:16px; line-height:1;
      border:1px solid var(--border);
      background:linear-gradient(135deg, rgba(139,92,246,0.5), rgba(6,182,212,0.5));
      box-shadow:0 8px 18px rgba(139,92,246,0.35);
    }
    .avatar.user{
      background:linear-gradient(135deg, rgba(6,182,212,0.45), rgba(99,102,241,0.45));
      box-shadow:0 8px 18px rgba(6,182,212,0.35);
    }

    .bubble{
      position:relative;
      max-width:100%;
      padding:14px 16px;
      background:linear-gradient(180deg, rgba(255,255,255,0.08), rgba(255,255,255,0.06));
      border:1px solid var(--border);
      border-radius:14px;
      box-shadow:var(--shadow);
    }
    .bubble.user{
      background:linear-gradient(180deg, rgba(6,182,212,0.08), rgba(6,182,212,0.05));
      border-color:rgba(6,182,212,0.35);
    }

    .bubble h3{
      margin:0 0 8px 0;
      font-family:"Ruslan Display", serif;
      font-weight:400;
      font-size:18px;
      letter-spacing:0.2px;
      color:#eae7ff;
      text-shadow:0 0 10px rgba(139,92,246,0.35);
    }

    .k-sep{
      height:1px; background:linear-gradient(90deg, transparent, rgba(255,255,255,0.15), transparent);
      margin:10px 0;
    }

    .list{
      margin:0; padding-left:18px; color:#e8eaf5;
    }
    .list li{margin:6px 0;}
    .muted{color:var(--muted)}
    .accent{
      color:#eae7ff;
      background:linear-gradient(90deg, #a78bfa, #22d3ee);
      -webkit-background-clip:text; background-clip:text; -webkit-text-fill-color:transparent;
      text-shadow:0 0 18px rgba(139,92,246,0.35);
      font-family:"Ruslan Display", serif;
      letter-spacing:0.2px;
    }

    .chips{
      display:flex; flex-wrap:wrap; gap:10px; margin-top:10px;
    }
    .chip{
      cursor:pointer;
      border:1px solid var(--border);
      background:
        linear-gradient(180deg, rgba(255,255,255,0.08), rgba(255,255,255,0.06));
      color:var(--text);
      padding:8px 12px; border-radius:999px;
      font-weight:600; font-size:13px;
      transition:.2s transform ease, .2s box-shadow ease, .2s background ease;
      box-shadow:0 6px 14px rgba(0,0,0,0.25);
    }
    .chip:hover{ transform:translateY(-1px); box-shadow:0 10px 20px rgba(139,92,246,0.22)}
    .chip:active{ transform:translateY(0)}
    .chip .sub{font-weight:400; color:var(--muted); margin-left:6px}

    .composer{
      position:sticky; bottom:0;
      padding:12px; border-radius:16px;
      background:linear-gradient(180deg, rgba(10,13,22,0), rgba(10,13,22,0.8));
      backdrop-filter:blur(8px) saturate(120%);
      border:1px solid var(--border);
      box-shadow:var(--shadow);
      display:flex; gap:10px; align-items:flex-end;
      z-index:5;
    }
    .textarea{
      flex:1; position:relative;
      background:linear-gradient(180deg, rgba(255,255,255,0.08), rgba(255,255,255,0.06));
      border:1px solid var(--border);
      border-radius:14px; padding:10px 12px;
      display:flex; align-items:center;
    }
    textarea{
      width:100%; min-height:52px; max-height:160px; resize:vertical;
      background:transparent; border:0; outline:none; color:var(--text);
      font:400 15px/1.5 Inter, system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, "Helvetica Neue", Arial;
    }
    .cta{
      flex:0 0 auto;
      display:inline-flex; gap:10px; align-items:center; justify-content:center;
      padding:12px 16px;
      border-radius:12px; cursor:pointer;
      background:linear-gradient(135deg, #8b5cf6, #22d3ee);
      color:#0b0f18; font-weight:800; letter-spacing:0.3px;
      border:none;
      box-shadow:0 10px 24px rgba(139,92,246,0.35), 0 0 0 1px rgba(255,255,255,0.2) inset;
      transition:transform .2s ease;
    }
    .cta:hover{ transform:translateY(-1px)}
    .cta:active{ transform:translateY(0)}

    .typing{
      display:inline-flex; gap:5px; align-items:center; color:var(--muted); font-size:13px;
    }
    .dot{width:6px; height:6px; border-radius:50%; background:var(--muted); opacity:.6; animation:blink 1.4s infinite}
    .dot:nth-child(2){animation-delay:.2s}
    .dot:nth-child(3){animation-delay:.4s}
    @keyframes blink{0%,80%,100%{opacity:.2; transform:translateY(0)} 40%{opacity:.9; transform:translateY(-2px)}}

    .footer{
      margin-top:10px; padding:12px 16px;
      display:flex; flex-wrap:wrap; gap:8px; align-items:center; justify-content:center;
      color:var(--muted); font-size:13px;
    }
    .links{
      display:flex; gap:8px; flex-wrap:wrap;
    }
    .link{
      display:inline-flex; gap:8px; align-items:center;
      border:1px solid var(--border);
      background:linear-gradient(180deg, rgba(255,255,255,0.08), rgba(255,255,255,0.06));
      color:#eaf2ff; text-decoration:none; padding:8px 12px; border-radius:999px;
      font-weight:600; font-size:13px;
      transition:.2s transform ease, .2s box-shadow ease;
    }
    .link:hover{ transform:translateY(-1px); box-shadow:0 8px 20px rgba(139,92,246,0.22)}
    .icon{
      width:16px; height:16px; display:inline-block;
    }

    /* Scrollbar */
    .chat::-webkit-scrollbar{width:10px}
    .chat::-webkit-scrollbar-thumb{
      background:linear-gradient(180deg, rgba(139,92,246,0.4), rgba(34,211,238,0.4));
      border-radius:999px; border:2px solid rgba(0,0,0,0);
      background-clip:padding-box;
    }
    .chat::-webkit-scrollbar-track{background:rgba(255,255,255,0.04); border-radius:999px}

    /* Small helper tags */
    .badge{
      display:inline-flex; gap:6px; align-items:center;
      padding:4px 8px; border-radius:999px; font-size:12px; font-weight:700;
      border:1px solid var(--border);
      background:linear-gradient(180deg, rgba(139,92,246,0.18), rgba(6,182,212,0.14));
      color:#eaf2ff;
    }

    .grid{
      display:grid; gap:10px;
    }
    @media(min-width:680px){
      .grid.cols-2{grid-template-columns:1fr 1fr;}
      .grid.cols-3{grid-template-columns:repeat(3, 1fr)}
    }

    .blockquote{
      margin:8px 0 0 0; padding-left:10px; border-left:2px solid rgba(139,92,246,0.35); color:#cfd3e2;
      font-style:italic;
    }

    .restart{
      margin-top:12px;
      display:inline-flex; gap:10px; align-items:center;
      padding:10px 12px; border-radius:10px; border:1px solid var(--border);
      background:linear-gradient(180deg, rgba(255,255,255,0.08), rgba(255,255,255,0.06));
      cursor:pointer; font-weight:700; color:#eaf2ff;
    }
    .restart:hover{ box-shadow:0 8px 18px rgba(139,92,246,0.18) }
  </style>
</head>
<body>
  <div class="bg-image" aria-hidden="true"></div>
  <canvas id="stars"></canvas>
  <div class="noise" aria-hidden="true"></div>

  <div class="container">
    <header class="site" role="banner">
      <div class="brand">
        <div class="sigil" aria-hidden="true"><span>Ѫ</span></div>
        <div class="ttl">
          <h1>Навигатор Призвания</h1>
          <small>ИИ‑наставник — Gemini 2.5 Pro</small>
        </div>
      </div>
      <div class="hint">
        <span class="pill">Минимализм · Мистика · Древнеславянский стиль</span>
      </div>
    </header>

    <main class="chat" id="chat" role="log" aria-live="polite" aria-label="Диалог с ИИ-наставником"></main>

    <form class="composer" id="composer" autocomplete="off">
      <div class="textarea">
        <textarea id="input" placeholder="Напиши ответ здесь..." aria-label="Ваш ответ"></textarea>
      </div>
      <button class="cta" type="submit" id="sendBtn">Отправить</button>
    </form>

    <footer class="footer">
      <span>Создатель сайта:</span>
      <div class="links">
        <a class="link" href="https://www.instagram.com/iwegod?igsh=MWswMDRsbzN4aGhxcA==" target="_blank" rel="noopener">
          <svg class="icon" viewBox="0 0 24 24" fill="none"><path d="M7 2h10a5 5 0 0 1 5 5v10a5 5 0 0 1-5 5H7a5 5 0 0 1-5-5V7a5 5 0 0 1 5-5Z" stroke="currentColor" opacity=".6"/><rect x="7" y="7" width="10" height="10" rx="5" stroke="currentColor"/><circle cx="17.5" cy="6.5" r="1" fill="currentColor"/></svg>
          Instagram
        </a>
        <a class="link" href="https://t.me/ty_iskusstvo" target="_blank" rel="noopener">
          <svg class="icon" viewBox="0 0 24 24" fill="none"><path d="M21.5 3.5 2.5 10.5c-.9.33-.86 1.62.06 1.89l4.96 1.51 1.93 6.54c.27.92 1.5 1.08 2.01.26l2.78-4.49 5.19 3.84c.74.55 1.8.14 2.02-.77l3-12.5c.24-1-0.67-1.84-1.75-1.28Z" stroke="currentColor"/></svg>
          Telegram
        </a>
      </div>
    </footer>
  </div>

  <script>
    // Starfield background
    (function stars(){
      const c = document.getElementById('stars');
      const ctx = c.getContext('2d');
      let w, h, stars = [];
      function resize(){
        w = c.width = window.innerWidth;
        h = c.height = window.innerHeight;
        stars = new Array(Math.min(200, Math.floor(w*h/9000))).fill().map(()=>({
          x:Math.random()*w,
          y:Math.random()*h,
          r:Math.random()*1.2+0.2,
          a:Math.random()*0.6+0.2,
          s:(Math.random()*0.3+0.15)* (Math.random()<.5?-1:1)
        }));
      }
      function draw(){
        ctx.clearRect(0,0,w,h);
        for(const s of stars){
          s.a += s.s*0.02;
          const o = 0.35 + 0.35*Math.sin(s.a);
          ctx.beginPath();
          ctx.arc(s.x, s.y, s.r, 0, Math.PI*2);
          const grad = ctx.createRadialGradient(s.x, s.y, 0, s.x, s.y, s.r*3);
          grad.addColorStop(0, `rgba(167,139,250,${o})`);
          grad.addColorStop(1, 'rgba(0,0,0,0)');
          ctx.fillStyle = grad;
          ctx.fill();
        }
        requestAnimationFrame(draw);
      }
      window.addEventListener('resize', resize, {passive:true});
      resize(); draw();
    })();

    // Core app
    const chat = document.getElementById('chat');
    const input = document.getElementById('input');
    const composer = document.getElementById('composer');
    const sendBtn = document.getElementById('sendBtn');

    const state = {
      stage: 'intro', // intro -> awaiting_choice -> plan_shown
      suggestions: [],
      chosen: null,
      profile: {}
    };

    function scrollToBottom(){
      chat.scrollTop = chat.scrollHeight + 1000;
    }

    function el(tag, cls, html){
      const e = document.createElement(tag);
      if(cls) e.className = cls;
      if(html !== undefined) e.innerHTML = html;
      return e;
    }

    function addMessage(role, html){
      const wrap = el('div','msg');
      const avatar = el('div','avatar' + (role==='user'?' user':''), role==='user'?'Я':'ᚠ');
      const bubble = el('div', 'bubble' + (role==='user'?' user':''), html);
      wrap.appendChild(avatar); wrap.appendChild(bubble);
      chat.appendChild(wrap);
      scrollToBottom();
      return bubble;
    }

    function addTyping(){
      const b = addMessage('ai', `<span class="typing" aria-label="ИИ печатает"><span class="dot"></span><span class="dot"></span><span class="dot"></span></span>`);
      return {remove:()=>{ b.parentElement.remove(); }};
    }

    function initialPrompt(){
      return `
        <h3>Твоя история — ключ к призванию</h3>
        Привет! Я твой ИИ‑наставник <span class="badge">Gemini 2.5 Pro</span>. Чтобы точно подсветить твои сильные стороны и предложить профессии, ответь максимально подробно и по пунктам:
        <div class="k-sep"></div>
        <ul class="list">
          <li><b>Возраст и образование</b> (если учишься — где и на каком курсе)</li>
          <li><b>Что тебя по‑настоящему увлекает</b>: темы, задачи, формат работы</li>
          <li><b>Навыки и инструменты</b> (языки, программы, технологии, soft‑skills)</li>
          <li><b>Опыт</b>: учебные/рабочие проекты, хобби, волонтерство, достижения</li>
          <li><b>Куда хочешь двигаться</b> и какие навыки мечтаешь применять</li>
          <li><b>Предпочтения/ограничения</b>: удалёнка/офис, график, город, желаемый доход</li>
        </ul>
        <div class="blockquote">Пиши свободно: чем детальнее ответ — тем точнее рекомендации.</div>
      `;
    }

    function normalize(s){ return (s||'').toLowerCase().replace(/[ё]/g,'е'); }
    function countOcc(t, kw){
      let c=0;
      for(const k of kw){ if(t.includes(k)) c++; }
      return c;
    }
    function findAge(text){
      const m = text.match(/(\d{1,2})\s*(?:лет|года|год|годика|yrs|years)/i) || text.match(/\b(1[3-9]|[2-9]\d)\b/);
      if(!m) return null;
      const n = parseInt(m[1],10);
      if(isNaN(n) || n<10 || n>80) return null;
      return n;
    }

    function analyzeProfile(text){
      const raw = text || '';
      const t = normalize(raw);
      const age = findAge(raw);

      const dict = {
        it: ["програм","код","python","java","js","javascript","typescript","c++","с++","c#","csharp","разработ","frontend","бекенд","backend","веб","сайт","мобильн","android","ios","github","алгоритм","нейросет","ии","машин","data","данн","аналит","ml","ai","devops","qa","тестир","систем","кибер","security","unix","linux","sql","api","бот","скрипт"],
        design: ["дизайн","ux","ui","юикс","юай","figma","фигма","иллюстрац","график","motion","анимац","брендинг","типограф","прототип","архитектур","визуал","3d","blender","cinema4d","after effects","цвет"],
        marketing: ["маркет","smm","контент","копирайт","реклам","таргет","seo","sem","бренд","вирал","email","crm","growth","продвиж","воронк","трафик","лид"],
        product: ["менедж","product","продакт","scrum","agile","проект","pm","управл","roadmap","приорит","user story","hypothesis","custdev","юнит","unit economics","go-to-market","стратег"],
        finance: ["финанс","эконом","инвест","акци","крипт","крипто","трейд","бухгалтер","аудит","risk","банк","финмодель","отчет"],
        education: ["психолог","коуч","преподав","учитель","обуч","репетитор","образован","педагог","ментор","edtech"],
        art: ["музык","пев","композ","гитар","фортеп","вокал","искусств","театр","актер","кино","режисс","фото","видео","блог","ютуб","youtube","tik","tiktok","стрим","stream","оператор","монтаж"],
        engineering: ["инжен","электр","мехатрон","робот","машин","строит","черт","cad","solid","arduino","raspberry","embedd","железо","pcb","станок","cnc"],
        healthcare: ["медиц","врач","биолог","хим","фарма","здоров","диет","спорт","тренер","реабил"],
        law: ["юрист","право","legal","адвокат"],
        languages: ["язык","перевод","лингв","english","англ","кит","нем","фран","испан","переводчик"],
        gamedev: ["игр","game","unity","unreal","level","геймд","геймдев","гейм-дизайн"]
      };

      const score = {};
      for(const k in dict){ score[k] = countOcc(t, dict[k]); }

      // Additional signals
      const signals = {
        team: /команд|вместе|совмест/i.test(raw),
        lead: /руковод|лидер|возглав/i.test(raw),
        comms: /обща|коммуник|клиент|переговор/i.test(raw),
        analysis: /аналит|данн|логик|систем/i.test(raw),
        creative: /креатив|иде|рисую|рисовал|твор|муз|дизайн|сценар/i.test(raw),
        teaching: /учу|настав|препода|репетит/i.test(raw),
        discipline: /дедлайн|план|организ|самоорган/i.test(raw),
        sales: /продаж|догов|воронк|лид/i.test(raw),
        fear: /страх|сомнен|тревог|не уверен/i.test(raw),
        procrast: /прокраст|откладыв|лень/i.test(raw)
      };

      let sorted = Object.entries(score).sort((a,b)=>b[1]-a[1]).filter(([,v])=>v>0).map(([k])=>k);
      if(sorted.length===0){
        // default to broad if nothing detected
        sorted = ['it','design','marketing','product','education','art'];
      }

      // Build strengths
      const strengths = [];
      const pushUnique = (arr, val)=>{ if(!arr.includes(val)) arr.push(val); };

      // Category-based strengths
      const topCat = sorted[0];
      if(topCat==='it' || topCat==='data' || topCat==='gamedev'){
        pushUnique(strengths, "аналитическое мышление и внимание к деталям");
        pushUnique(strengths, "способность быстро учиться на практике");
      }
      if(topCat==='design'){ pushUnique(strengths, "развитое визуальное мышление и эмпатия к пользователю"); }
      if(topCat==='marketing'){ pushUnique(strengths, "ориентация на гипотезы, метрики и рост"); }
      if(topCat==='product'){ pushUnique(strengths, "системное мышление и приоритизация"); }
      if(topCat==='education'){ pushUnique(strengths, "умение объяснять сложное простыми словами"); }
      if(topCat==='engineering'){ pushUnique(strengths, "техническая смекалка и усидчивость"); }

      // Text-based strengths
      if(signals.team) pushUnique(strengths, "умение работать в команде");
      if(signals.lead) pushUnique(strengths, "лидерство и ответственность");
      if(signals.comms) pushUnique(strengths, "сильные коммуникативные навыки");
      if(signals.analysis) pushUnique(strengths, "структурное мышление");
      if(signals.creative) pushUnique(strengths, "креативность и насмотренность");
      if(signals.teaching) pushUnique(strengths, "наставничество и педагогические способности");
      if(signals.discipline) pushUnique(strengths, "самоорганизация и дисциплина");

      if(strengths.length===0){
        strengths.push("потенциал к быстрому обучению");
        strengths.push("высокая мотивация к развитию");
      }

      // Risks and growth areas
      const risks = [];
      const growth = [];

      if(raw.length < 280) risks.push("недостаточно данных для точной оценки — добавь детали об опыте и инструментах");
      if((sorted.length>3) || /все понемногу|много всего|многое интересно|все интересно/i.test(raw)) risks.push("разброс интересов — риск распыления внимания");
      if(signals.fear) risks.push("сомнения/страхи мешают скорости решений");
      if(signals.procrast) risks.push("прокрастинация — риск срыва сроков");

      const hasProjects = /проект|портф|github|кейсы|case|репозитор/i.test(raw);
      if(!hasProjects) growth.push("собрать портфолио из 2–3 прикладных кейсов за 2–3 месяца");

      const growthByCat = {
        it: ["усилить алгоритмическое мышление (структуры данных, задачи)", "укрепить базу по Git, тестированию и работе с API"],
        design: ["прокачать research: JTBD, интервью, CJM", "оформить дизайн‑систему и 2 кейса в Figma с метриками"],
        marketing: ["освоить аналитику воронки (GA4/Я.Метрика, UTM, когортный анализ)", "настроить и протестировать 2–3 канала трафика"],
        product: ["отработать discovery (интервью, критерии решений)", "собрать метрики: North Star, unit‑экономика, приоритизация"],
        engineering: ["углубить матчасть и инструменты (CAD/электроника)", "реализовать pet‑project с документацией"],
        education: ["упаковать методики, ТЗ и чек‑листы", "подготовить демо‑урок/мини‑курс"]
      };
      const catKey = topCat in growthByCat ? topCat : (sorted.includes('it')?'it':null);
      if(catKey){ growth.push(...growthByCat[catKey]); }

      // Build suggestions
      const bank = {
        it: [
          {title:"Frontend‑разработчик", why:"любовь к интерфейсам, JS/TS, визуальное мышление"},
          {title:"Backend‑разработчик", why:"интерес к архитектуре и данным, API, базы"},
          {title:"Data Analyst", why:"аналитика данных, SQL, продуктовые метрики"},
          {title:"ML‑инженер", why:"интерес к ИИ/математике и экспериментам"},
          {title:"QA‑инженер", why:"внимание к деталям и системность"}
        ],
        design: [
          {title:"UX/UI дизайнер", why:"эмпатия к пользователю и Figma‑навыки"},
          {title:"Product Designer", why:"мышление продуктом и метрики"},
          {title:"Motion Designer", why:"анимации и сторителлинг"},
          {title:"3D‑дизайнер", why:"интерес к 3D/визуализации"}
        ],
        marketing: [
          {title:"Digital‑маркетолог", why:"гипотезы, каналы трафика и аналитика"},
          {title:"SMM‑стратег", why:"контент‑план и сообщество"},
          {title:"Копирайтер/Контент‑креатор", why:"тексты/видео и креатив"},
          {title:"Performance‑маркетолог", why:"цифры, A/B и оптимизация"}
        ],
        product: [
          {title:"Product Manager", why:"дискавери, приоритизация и метрики"},
          {title:"Project Manager", why:"управление сроками и ресурсами"},
          {title:"Бизнес‑аналитик", why:"требования, процессы, системность"}
        ],
        gamedev: [
          {title:"Unity‑разработчик", why:"прототипирование и C#"},
          {title:"Level Designer", why:"гейм‑механики и баланс"},
          {title:"Technical Artist", why:"соединение кода и графики"}
        ],
        engineering: [
          {title:"Инженер‑электроник", why:"железо, схемотехника"},
          {title:"Робототехник", why:"мехатроника и управление"},
          {title:"CAD‑инженер", why:"3D‑моделирование и производство"}
        ],
        education: [
          {title:"Онлайн‑ментор/Преподаватель", why:"объяснять и вести к результату"},
          {title:"Методист EdTech", why:"структура и контроль качества обучения"},
          {title:"Коуч/Психолог", why:"работа с целями и состояниями"}
        ],
        art: [
          {title:"Видеомейкер", why:"монтаж/съемка и сторителлинг"},
          {title:"Фотограф", why:"визуальный вкус и свет"},
          {title:"Иллюстратор", why:"рисунок и стилизация"}
        ],
        finance: [
          {title:"Финансовый аналитик", why:"модели, отчетность, метрики"},
          {title:"Инвест‑аналитик", why:"рынки и риски"},
          {title:"Бухгалтер/Аудитор", why:"точность и регламенты"}
        ]
      };

      const suggestions = [];
      const addSug = (arr)=>arr.forEach(s=>{ if(suggestions.length<7 && !suggestions.find(x=>x.title===s.title)) suggestions.push(s); });

      for(const cat of sorted.slice(0,3)){
        if(bank[cat]) addSug(bank[cat]);
      }
      // If still few, mix generic IT/design
      if(suggestions.length<4){
        addSug(bank.it || []); addSug(bank.design || []);
      }

      return {
        age, sorted, strengths, risks, growth, suggestions, raw
      };
    }

    function renderAnalysis(a){
      // Save suggestions to state
      state.suggestions = a.suggestions;

      const badgeAge = a.age ? `<span class="badge" title="Возраст">Возраст: ${a.age}</span>` : `<span class="badge" title="Возраст">Возраст: не указан</span>`;

      const strengthsHTML = a.strengths.map(s=>`<li>${s}</li>`).join('');
      const risksHTML = (a.risks.length? a.risks.map(s=>`<li>${s}</li>`).join('') : `<li>явных рисков по описанию не обнаружено</li>`);
      const growthHTML = a.growth.map(s=>`<li>${s}</li>`).join('');

      const optionsHTML = a.suggestions.slice(0,6).map(s=>`
        <button class="chip" data-option="${s.title}">
          ${s.title}<span class="sub">— ${s.why}</span>
        </button>
      `).join('');

      return `
        <h3>Что я вижу по твоему описанию</h3>
        <div class="grid cols-2">
          <div>
            ${badgeAge}
          </div>
          <div><span class="badge">Фокус: <span class="accent">${prettyCat(a.sorted[0])}</span></span></div>
        </div>
        <div class="k-sep"></div>
        <div class="grid cols-2">
          <div>
            <b>Сильные стороны</b>
            <ul class="list">${strengthsHTML}</ul>
          </div>
          <div>
            <b>Минусы/риски</b>
            <ul class="list">${risksHTML}</ul>
          </div>
        </div>
        <div class="k-sep"></div>
        <b>Зоны роста</b>
        <ul class="list">${growthHTML}</ul>
        <div class="k-sep"></div>
        <b>Куда двигаться дальше — варианты профессий</b>
        <div class="chips" id="options">${optionsHTML}</div>
        <div class="blockquote">Выбери вариант кнопкой выше или напиши свой ответ: “Хочу двигаться в ...”</div>
      `;
    }

    function prettyCat(k){
      const map = {
        it:"IT/Разработка", design:"Дизайн", marketing:"Маркетинг", product:"Управление продуктом",
        finance:"Финансы", education:"Образование/Психология", art:"Креатив/Медиа",
        engineering:"Инженерия", healthcare:"Здоровье/Спорт", law:"Право", languages:"Языки/Переводы",
        gamedev:"Геймдев"
      };
      return map[k] || "Общее развитие";
    }

    function chooseDirectionFromText(t){
      const text = normalize(t);
      const all = state.suggestions.map(s=>s.title);
      // Direct match
      for(const opt of all){
        const n = normalize(opt);
        if(text.includes(n.split(' ')[0])) return opt;
      }
      // Heuristic mapping
      if(/front|фронт|интерфейс|верстк|react|vue|angular|js|javascript|typescript/.test(text)) return "Frontend‑разработчик";
      if(/back|бек|сервер|spring|django|node|php|go|java|python/.test(text)) return "Backend‑разработчик";
      if(/data|аналит|sql|таблиц|excel|bi|power bi|табло|metabase|look/.test(text)) return "Data Analyst";
      if(/ml|data scient|машин|нейросет|tensorflow|pytorch/.test(text)) return "ML‑инженер";
      if(/ux|ui|дизайн|figma|прототип/.test(text)) return "UX/UI дизайнер";
      if(/маркет|smm|контент|performance|таргет/.test(text)) return "Digital‑маркетолог";
      if(/product|продакт|pm|менедж/.test(text)) return "Product Manager";
      if(/qa|тест/.test(text)) return "QA‑инженер";
      if(/devops|инфра|docker|kube|linux|сервер/.test(text)) return "DevOps‑инженер";
      if(/робот|инжен|cad|электр|arduino|embedded/.test(text)) return "Инженер‑робототехник";
      if(/видео|блог|контент|ютуб|фото|монтаж/.test(text)) return "Контент‑креатор/Видеомейкер";
      if(/учитель|преподав|ментор|коуч/.test(text)) return "Онлайн‑ментор/Преподаватель";
      return all[0] || "Frontend‑разработчик";
    }

    function generatePlan(direction){
      const d = normalize(direction);
      let key = 'general';
      if(/front/.test(d) || /фронт|интерфейс|верст/.test(d)) key='frontend';
      else if(/back|бек|сервер|django|spring|node|php|go/.test(d)) key='backend';
      else if(/data analyst|аналит|sql|bi|excel|таб/.test(d)) key='analyst';
      else if(/ml|data scient|машин|нейросет/.test(d)) key='ml';
      else if(/ux|ui|дизайн|figma/.test(d)) key='ux';
      else if(/маркет|smm|контент|performance|таргет/.test(d)) key='marketing';
      else if(/product|продакт|pm|менедж/.test(d)) key='product';
      else if(/qa|тест/.test(d)) key='qa';
      else if(/devops|docker|kube|linux/.test(d)) key='devops';
      else if(/робот|инжен|cad|электр|arduino|embedded/.test(d)) key='engineering';
      else if(/видео|блог|ютуб|фото|контент|монтаж/.test(d)) key='content';
      else if(/учитель|преподав|ментор|коуч/.test(d)) key='teacher';

      const title = direction;
      const p = planByKey(key);
      const months = p.months.map((m,i)=>`
        <div class="bubble" style="margin-top:10px">
          <b>Месяц ${i+1}. ${m.title}</b>
          <ul class="list">${m.tasks.map(t=>`<li>${t}</li>`).join('')}</ul>
        </div>
      `).join('');

      const first3 = p.months.slice(0,3).flatMap(m=>m.tasks.slice(0,1)).slice(0,3);
      return `
        <h3>Пошаговый план на год: ${escapeHtml(title)}</h3>
        <div class="grid">${months}</div>
        <div class="k-sep"></div>
        <b>Критический анализ</b>
        <ul class="list">
          <li>Риски: перегрузка, распыление, нехватка практики. Антидот — <b>ритм 5/2</b> (5 дней учёбы/практики, 2 — отдых/рефлексия).</li>
          <li>Валидация выбора: <b>2 мини‑проекта за 30 дней</b> и 5 собеседований/брифов — измерь интерес и энергию.</li>
          <li>Если ты уже миддл — ускоряй темп, сдвигай план на 2–3 месяца и усложняй проекты.</li>
        </ul>
        <div class="k-sep"></div>
        <b>Как бы поступил на твоём месте человек с такими навыками</b>
        <ul class="list">
          <li>Сразу выбрал один фокус на 3 месяца и довёл до готового кейса.</li>
          <li>Еженедельно выкладывал прогресс и собирал ревью у экспертов.</li>
          <li>Делал маленькие, но частые итерации — гипотеза → действие → метрика → вывод.</li>
        </ul>
        <div class="k-sep"></div>
        <b>Итог диалога</b>
        <ul class="list">
          <li>Направление: <b>${escapeHtml(title)}</b></li>
          <li>Ближайшие шаги (7 дней): ${first3.map((t,i)=>`${i?'; ':''}${t}`).join('')}</li>
          <li>Через 90 дней: 2 законченных проекта + оформленное портфолио/резюме.</li>
        </ul>
        <button class="restart" id="restartBtn" type="button" aria-label="Начать заново">
          <svg class="icon" viewBox="0 0 24 24" fill="none"><path d="M12 5V2L7 7l5 5V9a6 6 0 1 1-6 6" stroke="currentColor" stroke-width="1.5"/></svg>
          Начать заново
        </button>
      `;
    }

    function planByKey(key){
      const commonSoft = [
        "Рефлексия: фиксируй прогресс в заметках 2–3 раза в неделю",
        "Собери мини‑комьюнити: 3–5 людей того же пути (взаимные ревью)",
        "Планируй недели блоками: теория → практика → обратная связь"
      ];
      const m = (title, tasks)=>({title, tasks});

      const maps = {
        frontend:[
          m("Ориентация и база", ["HTML5/CSS3 основы, семантика, адаптивность", "Git + GitHub (ветки, pull request)", "Сверстай 1 лендинг по макету"]),
          m("Core JS", ["Современный JS: типы, замыкания, промисы, fetch", "Сборщики/бандлеры (Vite), npm/yarn", "Мини‑проект: галерея/ToDo без фреймворков"]),
          m("Фреймворк", ["React (хуки, состояние, маршрутизация)", "TypeScript базовый", "Проект: SPA с внешним API"]),
          m("Проект 1", ["Архитектура, состояние, формы", "UI‑библиотека, анимации", "Деплой (Vercel/Netlify)"]),
          m("Качество", ["Тестирование (Jest/RTL)", "А11y и Lighthouse", "Чистый код и рефакторинг"]),
          m("Портфолио", ["Оформление 2 кейсов (описание задач/метрик)", "Личный сайт‑портфолио", "Нетворкинг: ревью от 3 специалистов"]),
          m("Расширение стека", ["Next.js/SSR", "Работа с формами/валидацией", "Проект 2: сложный UI + серверные данные"]),
          m("Инструменты", ["Состояние (Zustand/Redux)", "Оптимизация производительности", "Тех.статьи/резюме"]),
          m("Трудоустройство", ["CV/LinkedIn/ХабрКарьерa", "Codewars/LeetCode 2–3 задачки в день", "Отклики: 15–20 в неделю"]),
          m("Собеседования", ["System design для фронта", "Mock‑интервью", "Финальная полировка портфолио"]),
          m("Практика", ["Фриланс/стажировка", "План роста до миддла", "Ревью кода у ментора"]),
          m("Усиление", ["UI‑анимации/перфоманс", "Side‑project с пользователями", "Делись опытом в сообществе"])
        ],
        backend:[
          m("Основы", ["Язык (Python/Java/Go) — синтаксис, ООП", "Git, Docker базовый", "Проект: REST API «Заметки»"]),
          m("Базы данных", ["SQL (индексы, join), транзакции", "ORM (SQLAlchemy/Hibernate)", "Проект: CRUD + auth"]),
          m("Архитектура", ["REST/GraphQL, кеширование", "Тесты (unit/integration)", "Проект: сервис с файловым хранилищем"]),
          m("Инфраструктура", ["CI/CD, контейнеризация", "Логи/мониторинг", "Деплой в облако"]),
          m("Продвинутый", ["Асинхронщина, очереди", "Безопасность", "Ревью/рефакторинг"]),
          m("Портфолио", ["2 офор. кейса, документация OpenAPI", "Резюме и GitHub Readme", "Нетворкинг"]),
          m("Углубление", ["DDD/чистая архитектура", "Проект 2 с несколькими сервисами", "Нагрузочное тестирование"]),
          m("Алгоритмы", ["Структуры данных", "LeetCode по графику", "Mock‑интервью"]),
          m("Джоб‑поиск", ["Отклики/сопров.письма", "Задачи на собесах", "Доработка кейсов"]),
          m("Практика", ["Фриланс/стажировка", "Ревью сеньора", "План роста"]),
          m("Опыт", ["Консолидация знаний", "Артефакты: статьи/доклады", "Автоматизация рутины"]),
          m("Итог", ["Подведение итогов года", "Фокус на сильные стороны", "Цели на следующий год"])
        ],
        analyst:[
          m("База", ["Excel/Google Sheets (формулы, сводные)", "SQL (select, join, group by)", "Мини‑решение: 3 аналитических отчёта"]),
          m("Python", ["Pandas/Numpy, визуализация (Matplotlib/Seaborn)", "EDA на датасете", "Кейс: исследование данных"]),
          m("BI", ["Power BI / Tableau / Metabase", "Дашборд с показателями", "Презентация инсайтов"]),
          m("Продуктовая аналитика", ["Метрики (AARRR, North Star)", "Когорты, юнит‑экономика", "Кейс: воронка и ретеншен"]),
          m("Эксперименты", ["A/B‑тесты, статистика", "Построение гипотез", "Кейс: дизайн эксперимента"]),
          m("Портфолио", ["2–3 кейса с данными/ноутбуками", "Оформление Readme", "Ревью у аналитиков"]),
          m("SQL‑углубление", ["Окна, CTE, оптимизация", "Практика задач", "Интервью‑подготовка"]),
          m("Инструменты", ["Airflow/скрипты", "ETL основы", "Автоматизация отчетов"]),
          m("Трудоустройство", ["CV, LinkedIn", "Отклики 10–15/неделя", "Тестовые задания"]),
          m("Собеседования", ["Продуктовые кейсы", "SQL‑live", "Коммуникация инсайтов"]),
          m("Практика", ["Фриланс/стажировка", "Реальный дашборд", "Ретро по задачам"]),
          m("Итог", ["Систематизация знаний", "Цели на рост до миддла", "Публичный кейс"])
        ],
        ml:[
          m("Основы", ["Python, Numpy, Pandas", "Статистика и математика", "EDA на 2 датасетах"]),
          m("Классика ML", ["Scikit‑learn: классификация/регрессия", "Метрики качества", "Кросс‑валидация, пайплайны"]),
          m("Фичи и продакшн", ["Feature engineering", "MLflow, эксперименты", "Kaggle: 1 соревнование"]),
          m("DL базовый", ["PyTorch/TensorFlow", "Полносвязные/сверточные сети", "Проект: CNN/табличные"]),
          m("NLP / CV", ["Выбери трек (NLP/CV)", "Трансформеры / детекторы", "Проект по треку"]),
          m("MLOps", ["Контейнеризация, инференс", "Оркестрация (Airflow)", "Мониторинг дрейфа"]),
          m("Портфолио", ["2 кейса + стат.отчёты", "Демо/Colab/документирование", "Ревью у практиков"]),
          m("Интервью", ["Алгоритмы/ML‑теория", "Системный дизайн ML", "Mock‑интервью"]),
          m("Поиск", ["CV/портфолио", "Отклики и сети", "Пет‑проект на бизнес‑данных"]),
          m("Практика", ["Фриланс/стажировка", "Оптимизация моделей", "MLOps улучшения"]),
          m("Опыт", ["Статья/доклад", "Контриб в open‑source", "Менторство джунам"]),
          m("Итог", ["Аналитика прогресса", "Фокус на нишу", "План роста"])
        ],
        ux:[
          m("Research", ["JTBD, интервью, CJM", "Анализ конкурентов", "Персоны и сценарии"]),
          m("Информационная архитектура", ["Флоу, карты экранов", "Wireframes", "Навигация"]),
          m("UI и система", ["Типографика/сеткa/цвет", "Компоненты и токены", "Дизайн‑система в Figma"]),
          m("Прототипы", ["Интерактив в Figma", "Тесты с пользователями", "Кейс: улучшение метрики"]),
          m("Хенд‑офф", ["Спека, авто‑layout", "Работа с разработкой", "Документация"]),
          m("Портфолио", ["2 сильных кейса с метриками", "Оформление презентации", "Ревью у синьоров"]),
          m("Motion/Анимации", ["Микровзаимодействия", "Лоутинги/переходы", "UI‑анимации"]),
          m("Исследования", ["Карта инсайтов", "Гипотезы и A/B", "Продуктовый подход"]),
          m("Поиск", ["Резюме/Behance/Dribbble", "Отклики 10–15/неделя", "Тестовые"]),
          m("Собеседования", ["Питч кейсов", "Кейсовые задания", "Коммуникация с бизнесом"]),
          m("Практика", ["Фриланс/стажировка", "Ежемесячный кейс", "Сообщество"]),
          m("Итог", ["Систематизация", "План роста", "Public case"])
        ],
        marketing:[
          m("База", ["Маркетинговая аналитика: AARRR", "ICP/персоны и рынок", "Карта каналов"]),
          m("Контент", ["Контент‑стратегия и календарь", "Копирайтинг/сторителлинг", "15 публикаций/shorts"]),
          m("Performance", ["UTM/пиксели/события", "GA4/Метрика", "Запуск двух каналов"]),
          m("Креативы", ["Гипотезы и A/B", "Крео‑пакеты", "Сбор метрик"]),
          m("CRM", ["E‑mail/чат‑воронки", "Сегментация", "Автоворонки"]),
          m("Портфолио", ["2 кейса: рост метрик", "Публичные отчёты", "Ревью у экспертов"]),
          m("Медиа", ["Партнёрки/инфлюенс", "PR‑публикации", "Кросс‑промо"]),
          m("Юнит‑экономика", ["С/САС/LTV/ROMI", "Калькулятор", "Оптимизация бюджета"]),
          m("Поиск", ["Резюме/портфолио", "Отклики, брифы", "Тестовые"]),
          m("Собеседования", ["Кейс‑интервью", "Дашборды", "План первых 90 дней"]),
          m("Практика", ["Фриланс/агентство", "Ниша/вертикаль", "Система экспериментов"]),
          m("Итог", ["Доклад/пост", "Сеть контактов", "Цели на год"])
        ],
        product:[
          m("Дискавери", ["Проблемные интервью", "Карта гипотез", "MVP‑гипотеза"]),
          m("Метрики", ["North Star, пирамиды метрик", "Unit economics", "Дашборд"]),
          m("Приоритизация", ["RICE/ICE", "Roadmap", "Коммуникация со стейкхолдерами"]),
          m("Delivery", ["User stories/acceptance", "Процессы и ретро", "Качество релизов"]),
          m("Монетизация", ["Ценообразование", "Каналы роста", "Эксперименты"]),
          m("Портфолио", ["Кейсы: до/после", "Презентация решений", "Ревью у СРО/PM"]),
          m("Технологии", ["Базово: API, схемы", "Аналитика/SQL", "Системное мышление"]),
          m("Командные практики", ["Scrum/Kanban", "Договорённости и SLA", "1:1/фасилитация"]),
          m("Поиск", ["CV/LinkedIn", "Собеседования", "План 90‑дневки"]),
          m("Практика", ["Стаж/фриланс", "Менторство", "Анализ конкурентов"]),
          m("Рост", ["Продуктовые доклады", "Ментальные модели", "Сеть контактов"]),
          m("Итог", ["Обновление портфолио", "Цели на год", "Баланс глубины/ширины"])
        ],
        qa:[
          m("Основы тестирования", ["Виды тестов, уровни", "Чек‑листы/тест‑кейсы", "Практика на pet‑проекте"]),
          m("Инструменты", ["Postman/Swagger", "Логи/консоль", "Репорты и баг‑трекинг"]),
          m("Автотесты", ["JS/Python для автотестов", "Selenium/Playwright", "Построение набора тестов"]),
          m("API/БД", ["REST/GraphQL", "SQL базовый", "Фикстуры и данные"]),
          m("Процессы", ["CI, отчётность", "Приёмка релизов", "Коммуникации"]),
          m("Портфолио", ["Набор тестов + отчёты", "Документация", "Ревью у QA Lead"]),
          m("Расширение", ["Нагрузочное/безопасность", "Мобильное тестирование", "Интеграции"]),
          m("Интервью", ["Теория/практика", "Тестовые задания", "Mock‑интервью"]),
          m("Поиск", ["CV/отклики", "Задания", "Демонстрация стенда"]),
          m("Практика", ["Стаж/фриланс", "Автоматизация рутины", "Улучшение покрытия"]),
          m("Рост", ["Менторство джунам", "Ретро", "Цели"]),
          m("Итог", ["Обновление артефактов", "План развития", "Баланс нагрузки"])
        ],
        devops:[
          m("Linux & сети", ["Linux CLI, файловая система", "TCP/IP основы", "Bash‑скрипты"]),
          m("Контейнеры", ["Docker, образы, сети", "Compose", "Практика деплоя"]),
          m("CI/CD", ["GitHub Actions/GitLab CI", "Артефакты, среды", "Пайплайны"]),
          m("Kubernetes", ["k8s базовый", "Helm", "Мониторинг"]),
          m("Облака", ["AWS/GCP/YC", "IaC (Terraform)", "Секреты и безопасность"]),
          m("Портфолио", ["Инфра‑кейс с описанием", "Диаграммы", "Ревью у DevOps"]),
          m("Надёжность", ["SLO/SLA/SLI", "Алёрты", "Логи/трассировка"]),
          m("Оптимизация", ["Стоимость/перфоманс", "Кэш/балансировщик", "Автоскейлинг"]),
          m("Поиск", ["CV", "Отклики", "Стенд для демонстрации"]),
          m("Собесы", ["Практика задач", "Архитектурные вопросы", "Инцидент‑менеджмент"]),
          m("Практика", ["Стаж/фриланс", "Автоматизация", "Документация"]),
          m("Итог", ["Артефакты", "Цели", "Сеть контактов"])
        ],
        engineering:[
          m("Матчасть", ["Электроника/мехатика базово", "Чтение схем/чертежей", "Пайка/прототипирование"]),
          m("CAD", ["Fusion/SolidWorks", "3D‑модели", "Сборка деталей"]),
          m("Контроллеры", ["Arduino/STM32", "Датчики/приводы", "Мини‑робот"]),
          m("Проект 1", ["ТЗ, BOM, сборка", "Тестирование", "Видео‑демо"]),
          m("Производство", ["PCB (KiCad)", "Заказ плат/корпусов", "Сбор и отладка"]),
          m("Портфолио", ["Документация/чертежи", "Кейс‑видео", "Ревью"]),
          m("Углубление", ["Управление/алгоритмы", "Энергопитание", "Безопасность"]),
          m("Проект 2", ["Сложный прототип", "Оптимизация", "Демо заказчику"]),
          m("Трудоустройство", ["CV/порт", "Отклики", "Собеседования"]),
          m("Практика", ["Стаж/фриланс", "Производственный цикл", "Конструкторская культура"]),
          m("Рост", ["Сертификаты", "Доклады", "Ниша"]),
          m("Итог", ["Итоги и цели", "План развития", "Сеть"])
        ],
        content:[
          m("Стратегия", ["Тематика/ниша", "Контент‑план", "Пилотные форматы"]),
          m("Производство", ["Сценарии, съёмка", "Монтаж (Premiere/CapCut)", "15 шортов/месяц"]),
          m("Визуал", ["Свет/кадр/цвет", "Брендинг канала", "Насмотренность"]),
          m("Платформы", ["YouTube/TikTok/Telegram", "Аналитика", "A/B креативов"]),
          m("Монетизация", ["Партнёрки/спонсоры", "Платные продукты", "Медиа‑кит"]),
          m("Портфолио", ["Кейсы с цифрами", "Лендинг/Link‑in‑bio", "Ревью контента"]),
          m("Рост", ["Коллаборации", "Сценарные сетки", "Автопилот процессов"]),
          m("Аудит", ["Контент‑аудит", "Оптимизация", "Саб‑ниша"]),
          m("Комьюнити", ["Чаты/закрытые форматы", "UGC", "Мероприятия"]),
          m("Стабильность", ["Пайплайн 2–3 выпуска/нед", "Репурпоз", "Ассистенты"]),
          m("Масштаб", ["Длинные форматы", "Собственные продукты", "Автоворонки"]),
          m("Итог", ["Итоги, цифры", "План на год", "Команду — в помощь"])
        ],
        teacher:[
          m("Позиционирование", ["Ниша и портрет ученика", "Программа/результаты", "Демо‑урок"]),
          m("Методика", ["Материалы/чек‑листы", "Диагностика", "Геймификация"]),
          m("Контент", ["Статьи/видео", "Первый набор", "Обратная связь"]),
          m("Курс/менторинг", ["Онбординг", "Структура занятий", "Промежуточные метрики"]),
          m("Качество", ["Домашки/проверка", "Оценка удовлетворённости", "Рефактор курса"]),
          m("Портфолио", ["Кейсы учеников", "Отзывы", "Лендинг"]),
          m("Маркетинг", ["Фunnels", "Партнёрства", "Вебинар/сессия"]),
          m("Продукты", ["Мини‑пакеты", "Сопровождение", "Комьюнити"]),
          m("Стабилизация", ["Процессы/шаблоны", "Ассистент", "Автоматизация"]),
          m("Масштаб", ["Группы/потоки", "Методисты", "Партнёры"]),
          m("Экспертность", ["Доклады/кейсы", "Публикации", "Рейтинги"]),
          m("Итог", ["Анализ результатов", "План роста", "Баланс нагрузки"])
        ],
        general:[
          m("Фокус", ["Выбери направление и сформулируй цель 90 дней", "Тест‑проект №1"]),
          m("База навыков", ["Ключевые инструменты", "Календарь обучения"]),
          m("Мини‑проект", ["Проект/кейс", "Обратная связь"]),
          m("Проект 1", ["Доработка", "Демо"]),
          m("Качество", ["Чек‑листы", "Документация"]),
          m("Портфолио", ["Оформление кейсов", "Личный сайт"]),
          m("Расширение", ["Инструменты", "Рефакторинг"]),
          m("Связи", ["Комьюнити", "Ментор"]),
          m("Поиск", ["CV", "Отклики"]),
          m("Собесы", ["Практика", "Тестовые"]),
          m("Практика", ["Фриланс/стажировка", "Ретро"]),
          m("Итог", ["Аналитика", "Цели"])
        ]
      };
      return { months: maps[key] || maps.general };
    }

    function escapeHtml(s){
      return String(s).replace(/[&<>"']/g, m=>({ '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m]));
    }

    // Event handlers
    function onOptionClick(e){
      const btn = e.target.closest('.chip');
      if(!btn) return;
      const selected = btn.getAttribute('data-option');
      if(!selected) return;
      // echo user choice
      addMessage('user', escapeHtml(selected));
      const t = addTyping();
      setTimeout(()=>{
        t.remove();
        const html = generatePlan(selected);
        addMessage('ai', html);
        state.stage = 'plan_shown';
        attachRestart();
        scrollToBottom();
      }, 500);
    }

    function attachRestart(){
      const btn = document.getElementById('restartBtn');
      if(btn){
        btn.addEventListener('click', ()=>{
          chat.innerHTML = '';
          state.stage = 'intro';
          state.suggestions = [];
          state.chosen = null;
          state.profile = {};
          input.value = '';
          setTimeout(()=>addMessage('ai', initialPrompt()), 50);
        });
      }
    }

    composer.addEventListener('submit', (e)=>{
      e.preventDefault();
      const val = input.value.trim();
      if(!val) return;
      addMessage('user', escapeHtml(val));
      input.value = '';
      const typing = addTyping();

      if(state.stage === 'intro'){
        setTimeout(()=>{
          typing.remove();
          const analysis = analyzeProfile(val);
          state.profile = analysis;
          const html = renderAnalysis(analysis);
          const bubble = addMessage('ai', html);
          const opts = bubble.querySelector('#options');
          if(opts) opts.addEventListener('click', onOptionClick);
          state.stage = 'awaiting_choice';
        }, 600);
      } else if(state.stage === 'awaiting_choice'){
        setTimeout(()=>{
          typing.remove();
          const chosen = chooseDirectionFromText(val);
          const html = generatePlan(chosen);
          addMessage('ai', html);
          state.stage = 'plan_shown';
          attachRestart();
        }, 500);
      } else {
        // After plan is shown: treat as restart trigger to refine?
        setTimeout(()=>{
          typing.remove();
          addMessage('ai', `Если хочешь начать заново — нажми «Начать заново» ниже или напиши: «Старт» и опиши себя подробнее.`);
        }, 400);
      }
    });

    // Init
    window.addEventListener('load', ()=>{
      addMessage('ai', initialPrompt());
      input.setAttribute('placeholder', 'Например: мне 21, учусь на 3 курсе, люблю визуальные интерфейсы и код...');

      // Keyboard submit with Ctrl/Cmd+Enter
      input.addEventListener('keydown', (e)=>{
        if((e.ctrlKey||e.metaKey) && e.key==='Enter'){
          e.preventDefault();
          sendBtn.click();
        }
      });
    });
  </script>
</body>
</html>
