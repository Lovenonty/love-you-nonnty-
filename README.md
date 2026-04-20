<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مملكة نونتي ❤️</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&family=Amiri:ital,wght@1,700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>
    <style>
        :root { --p: #ff4d6d; --bg: #0b090a; --glass: rgba(255, 255, 255, 0.08); }
        body { margin: 0; background: #000; color: white; font-family: 'Cairo', sans-serif; overflow-x: hidden; text-align: center; user-select: none; }
        canvas#stars { position: fixed; inset: 0; z-index: -1; background: radial-gradient(circle, #1a0a0d, #000); }
        #lock { position: fixed; inset: 0; background: #000; display: flex; align-items: center; justify-content: center; z-index: 10000; transition: 1s; }
        .login-card { background: var(--glass); padding: 40px; border-radius: 30px; backdrop-filter: blur(15px); border: 1px solid rgba(255,255,255,0.1); width: 85%; max-width: 400px; box-shadow: 0 0 30px var(--p); }
        input { padding: 15px; border-radius: 12px; border: none; width: 80%; margin: 20px 0; text-align: center; font-size: 18px; outline: none; background: white; color: black; }
        .main-btn { padding: 12px 30px; border-radius: 50px; background: var(--p); color: white; border: none; cursor: pointer; font-weight: bold; transition: 0.3s; box-shadow: 0 5px 15px rgba(255, 77, 109, 0.3); margin: 5px; }
        #content { display: none; opacity: 0; transition: 2s; padding: 15px; padding-bottom: 100px; }
        .playlist { background: var(--glass); padding: 20px; border-radius: 25px; margin: 20px auto; max-width: 550px; border: 1px solid var(--p); backdrop-filter: blur(10px); }
        .song-btn { background: rgba(255,255,255,0.05); border: 1px solid var(--p); color: white; padding: 10px 15px; border-radius: 12px; margin: 5px; cursor: pointer; transition: 0.3s; font-size: 0.9em; }
        .song-btn.active { background: var(--p); box-shadow: 0 0 15px var(--p); }
        .timer-container { background: var(--glass); border-radius: 25px; padding: 20px; margin: 25px auto; max-width: 500px; border-bottom: 3px solid var(--p); }
        .timer-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-top: 10px; }
        .timer-item { background: var(--p); padding: 12px 5px; border-radius: 12px; font-weight: bold; font-size: 1.2em; }
        .timer-item span { display: block; font-size: 0.6em; opacity: 0.9; }
        .flip-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; max-width: 500px; margin: 30px auto; }
        .flip-card { height: 130px; perspective: 1000px; }
        .flip-inner { position: relative; width: 100%; height: 100%; transition: transform 0.8s; transform-style: preserve-3d; cursor: pointer; }
        .flip-card:hover .flip-inner { transform: rotateY(180deg); }
        .flip-front, .flip-back { position: absolute; width: 100%; height: 100%; backface-visibility: hidden; display: flex; align-items: center; justify-content: center; border-radius: 15px; padding: 10px; box-sizing: border-box; font-weight: bold; border: 1px solid var(--p); }
        .flip-front { background: var(--glass); color: var(--p); font-size: 1.8em; }
        .flip-back { background: var(--p); color: white; transform: rotateY(180deg); font-size: 0.85em; }
        .photo-gallery { display: flex; flex-direction: column; align-items: center; gap: 40px; margin: 50px 0; }
        .polaroid { background: white; padding: 10px 10px 40px 10px; border-radius: 2px; box-shadow: 0 15px 30px rgba(0,0,0,0.8); width: 85%; max-width: 350px; animation: float 4s ease-in-out infinite; }
        .polaroid img { width: 100%; display: block; }
        @keyframes float { 0%, 100% { transform: translateY(0) rotate(var(--r)); } 50% { transform: translateY(-15px) rotate(calc(var(--r) * -1)); } }
        #secret-letter { background: #fff1f2; color: #333; padding: 30px; border-radius: 20px; max-width: 500px; margin: 30px auto; font-family: 'Amiri', serif; font-size: 1.4em; border-left: 8px solid var(--p); text-align: right; line-height: 1.8; box-shadow: 0 10px 30px rgba(0,0,0,0.5); min-height: 100px; display: none; }
        .heart-particle { position: absolute; pointer-events: none; animation: pop 0.8s ease-out forwards; color: var(--p); z-index: 9999; }
        @keyframes pop { 0% { transform: scale(0); opacity: 1; } 100% { transform: scale(2) translateY(-100px); opacity: 0; } }
    </style>
</head>
<body onclick="createHeart(event)">
    <canvas id="stars"></canvas>
    <div id="player-container" style="position: absolute; left: -9999px;"><div id="player"></div></div>
    <div id="lock">
        <div class="login-card">
            <h2 style="color: var(--p)">عالم نونتي ❤️</h2>
            <input type="text" id="pass" placeholder="Password...">
            <br>
            <button class="main-btn" onclick="unlock()">فتح عالمنا ❤️</button>
        </div>
    </div>
    <div id="content">
        <h1 style="font-family: 'Amiri'; font-size: 2.5em; color: var(--p);">وحشتيني يا نونتي ❤️</h1>
        <div class="timer-container">
            <p>إحنا مع بعض بقالنا بالثانية:</p>
            <div class="timer-grid">
                <div class="timer-item" id="days">0 <span>أيام</span></div>
                <div class="timer-item" id="hours">0 <span>ساعات</span></div>
                <div class="timer-item" id="minutes">0 <span>دقائق</span></div>
                <div class="timer-item" id="seconds">0 <span>ثواني</span></div>
            </div>
        </div>
        <div class="playlist">
            <p>راديو الحب 🎵 (اختاري أغنيتنا)</p>
            <button class="song-btn active" onclick="playSong('xMQieNosdII', this)">بيت كبير 🏠</button>
            <button class="song-btn" onclick="playSong('D5uGY-m7iDE', this)">إضحكي 😊</button>
            <button class="song-btn" onclick="playSong('NoQnWLgTDSE', this)">كلك عاجبني 😉</button>
            <button class="song-btn" onclick="playSong('nXoJDHUC63I', this)">أصابك عشق 💘</button>
        </div>
        <button class="main-btn" style="background: white; color: var(--p);" onclick="kissSurprise()">ابعتيلي بوسة هنا 💋</button>
        <div class="flip-grid">
            <div class="flip-card"><div class="flip-inner"><div class="flip-front">❤️</div><div class="flip-back">عشان حنيتك اللي بتطمن قلبي وتنسيني الهم</div></div></div>
            <div class="flip-card"><div class="flip-inner"><div class="flip-front">🤝</div><div class="flip-back">عشان جدعنتك اللي خلتني أحس إن في ظهري جبل</div></div></div>
            <div class="flip-card"><div class="flip-inner"><div class="flip-front">😂</div><div class="flip-back">عشان ضحكتك هي اللي بتنور دنيتي وتفرحني</div></div></div>
            <div class="flip-card"><div class="flip-inner"><div class="flip-front">👑</div><div class="flip-back">عشان إنتي "نونتي" الأميرة اللي حلمت بيها</div></div></div>
            <div class="flip-card"><div class="flip-inner"><div class="flip-front">🌸</div><div class="flip-back">عشان روحك الحلوة اللي بتخلي كل حاجة أجمل</div></div></div>
            <div class="flip-card"><div class="flip-inner"><div class="flip-front">🧠</div><div class="flip-back">عشان عقلك وتفكيرك اللي بيفهمني من غير كلام</div></div></div>
            <div class="flip-card"><div class="flip-inner"><div class="flip-front">💎</div><div class="flip-back">عشان أصلك الطيب وجوهرك الغالي اللي ملوش ثمن</div></div></div>
            <div class="flip-card"><div class="flip-inner"><div class="flip-front">♾️</div><div class="flip-back">عشان وجودك في حياتي هو أكبر نعمة من ربنا</div></div></div>
        </div>
        <div class="photo-gallery">
            <div class="polaroid" style="--r: -3deg;"><img src="nona.1.png"><p style="color:#333; margin-top:10px; font-weight:bold;">البداية ❤️</p></div>
            <div class="polaroid" style="--r: 4deg;"><img src="nona.2.png"><p style="color:#333; margin-top:10px; font-weight:bold;">نورتي دنيتي ✨</p></div>
            <div class="polaroid" style="--r: -2deg;"><img src="nona.3.jpg"><p style="color:#333; margin-top:10px; font-weight:bold;">أجمل ذكرياتنا 😍</p></div>
        </div>
        <div style="background: var(--glass); padding: 30px; border-radius: 30px; margin: 40px auto; max-width: 500px; border: 2px dashed var(--p);">
            <div id="random-msg" style="font-size: 1.4em; font-weight: bold; color: var(--p); margin-bottom: 20px;">بحبك يا نونتي ❤️</div>
            <button class="main-btn" onclick="showMsg()">رسالة جديدة ليكي ✨</button>
        </div>
        <button class="main-btn" id="l-btn" onclick="startTyping()">افتحي الجواب السري ليكي ✉️</button>
        <div id="secret-letter"></div>
    </div>
    <script>
        const canvas = document.getElementById('stars'); const ctx = canvas.getContext('2d');
        canvas.width = window.innerWidth; canvas.height = window.innerHeight;
        const starArr = []; for(let i=0; i<150; i++) starArr.push({x: Math.random()*canvas.width, y: Math.random()*canvas.height, s: Math.random()*2, v: Math.random()*0.5});
        function draw() { ctx.clearRect(0,0,canvas.width,canvas.height); ctx.fillStyle="white"; starArr.forEach(s => { ctx.beginPath(); ctx.arc(s.x, s.y, s.s, 0, Math.PI*2); ctx.fill(); s.y += s.v; if(s.y>canvas.height) s.y=0; }); requestAnimationFrame(draw); } draw();
        const start = new Date("November 21, 2024 00:00:00").getTime();
        setInterval(() => { const diff = new Date().getTime() - start; document.getElementById("days").innerHTML = Math.floor(diff/86400000) + " <span>أيام</span>"; document.getElementById("hours").innerHTML = Math.floor((diff%86400000)/3600000) + " <span>ساعات</span>"; document.getElementById("minutes").innerHTML = Math.floor((diff%3600000)/60000) + " <span>دقائق</span>"; document.getElementById("seconds").innerHTML = Math.floor((diff%60000)/1000) + " <span>ثواني</span>"; }, 1000);
        var player;
        function onYouTubeIframeAPIReady() { player = new YT.Player('player', { height: '1', width: '1', videoId: 'xMQieNosdII', playerVars: { 'autoplay': 0, 'controls': 0 } }); }
        function playSong(id, btn) { if(player && player.loadVideoById) { player.loadVideoById(id); player.playVideo(); document.querySelectorAll('.song-btn').forEach(b => b.classList.remove('active')); btn.classList.add('active'); } }
        function unlock() { if(document.getElementById('pass').value.toLowerCase().trim() === 'love') { if(player && player.playVideo) player.playVideo(); confetti({ particleCount: 200, spread: 80, origin: { y: 0.6 } }); document.getElementById('lock').style.opacity = '0'; setTimeout(() => { document.getElementById('lock').style.display='none'; document.getElementById('content').style.display='block'; setTimeout(() => document.getElementById('content').style.opacity='1', 100); }, 1000); } else { alert("الكلمة غلط يا نونتي! 😂"); } }
        const text = "نونتي الغالية.. ❤️\n بكتبلك الكلام ده وأنا قلبي مليان حب وفخر بوجودك في حياتي. الموقع ده مش بس كود وصور، ده تعبير بسيط عن السعادة اللي حاسسها كل يوم وإنتي معايا. نفسي حكايتنا اللي بدأت دي تفضل مستمرة طول العمر ونبني مستقبلنا سوا. إنتي مش بس حبيبة قلبي، إنتي النور اللي بينور أيامي. بحبك جداً للأبد يا أميرة قلبي ❤️";
        function startTyping() { document.getElementById('l-btn').style.display = 'none'; const el = document.getElementById('secret-letter'); el.style.display = 'block'; let i = 0; function type() { if(i < text.length) { el.innerHTML += text.charAt(i); i++; setTimeout(type, 50); } } type(); }
        const msgs = ["انتي حلمي اللي اتحقق ❤️", "كل ثانية معاكي رزق ✨", "ضحكتك بتنور دنيتي 😍", "بحبك يا نونتي للأبد 💖", "انتي البيت الكبير 🏠", "وعد عليا هفضل أحبك 💍", "انتي ملكة قلبي 👑"];
        function showMsg() { const b = document.getElementById('random-msg'); b.style.opacity = '0'; setTimeout(() => { b.innerText = msgs[Math.floor(Math.random()*msgs.length)]; b.style.opacity = '1'; }, 300); }
        function kissSurprise() { for(let i=0; i<25; i++) { setTimeout(() => { const h = document.createElement('div'); h.innerHTML = '💋'; h.className = 'heart-particle'; h.style.left = Math.random()*100+'vw'; h.style.top = Math.random()*100+'vh'; h.style.fontSize = Math.random()*40+20+'px'; document.body.appendChild(h); setTimeout(() => h.remove(), 1000); }, i*60); } }
        function createHeart(e) { const h = document.createElement('div'); h.innerHTML = '❤️'; h.className = 'heart-particle'; h.style.left = e.pageX + 'px'; h.style.top = e.pageY + 'px'; document.body.appendChild(h); setTimeout(() => h.remove(), 800); }
        var tag = document.createElement('script'); tag.src = "https://www.youtube.com/iframe_api"; var firstScriptTag = document.getElementsByTagName('script')[0]; firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
    </script>
</body>
</html>
