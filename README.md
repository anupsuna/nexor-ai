
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>NEXORA AI</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#080b14;color:#fff;font-family:Arial,sans-serif}
header{padding:20px 7%;display:flex;justify-content:space-between;align-items:center;border-bottom:1px solid #20283b}
.logo{font-size:25px;font-weight:900;color:#9d8cff}
.logo span{color:#fff}
.badge{background:#18223a;padding:9px 14px;border-radius:20px;color:#9d8cff;font-size:12px}
main{max-width:1050px;margin:auto;padding:35px 18px}
.hero{text-align:center;padding:35px 0}
.hero h1{font-size:clamp(35px,7vw,65px);margin:10px 0;background:linear-gradient(90deg,#fff,#a28bff,#56d9ff);background-clip:text;color:transparent}
.hero p{color:#9ca8bd;line-height:1.7}
.tabs{display:flex;gap:10px;justify-content:center;flex-wrap:wrap;margin:25px 0}
button{cursor:pointer;border:0;color:white;font-weight:bold}
.tab{background:#151c2c;padding:13px 22px;border-radius:12px}
.tab.active{background:linear-gradient(100deg,#7457ff,#278eff)}
.panel{background:#101727;border:1px solid #26324a;padding:22px;border-radius:20px;max-width:780px;margin:auto;box-shadow:0 15px 60px #0006}
.panel h2{margin-top:0}
textarea{width:100%;height:125px;background:#080e1b;color:white;border:1px solid #303d57;border-radius:12px;padding:15px;resize:vertical;font-size:15px;outline:none}
textarea:focus{border-color:#8872ff}
.actions{display:flex;gap:12px;flex-wrap:wrap;margin-top:15px}
.primary{background:linear-gradient(100deg,#795bff,#278eff);padding:14px 22px;border-radius:11px;flex:1}
.secondary{background:#26334b;padding:14px 20px;border-radius:11px}
.result{margin-top:20px;background:#080e1b;border-radius:12px;padding:17px;min-height:80px;white-space:pre-wrap;overflow-wrap:anywhere;color:#d6def0;line-height:1.6}
.cards{display:grid;grid-template-columns:repeat(auto-fit,minmax(190px,1fr));gap:15px;margin-top:30px}
.card{background:#111a2c;padding:20px;border:1px solid #26334b;border-radius:16px}
.card .emoji{font-size:28px}
.card p{color:#a5b1c5;line-height:1.5;font-size:14px}
footer{text-align:center;color:#71809a;padding:35px;font-size:13px}
input[type=file]{max-width:100%;color:#abb7cc}
.hidden{display:none}
</style>
</head>
<body>
<header>
 <div class="logo">NEXORA<span> AI.</span></div>
 <div class="badge">✦ YOUR AI STUDIO</div>
</header>
<main>
 <section class="hero">
  <div class="badge">THE FUTURE OF CREATIVITY</div>
  <h1>Imagine. Create. Explore.</h1>
  <p>One intelligent space for knowledge, images and videos.<br>Turn your imagination into something amazing.</p>
 </section>

 <div class="tabs">
  <button class="tab active" onclick="show('chat',this)">✦ AI Chat</button>
  <button class="tab" onclick="show('image',this)">◈ AI Image</button>
  <button class="tab" onclick="show('video',this)">▶ AI Video</button>
 </div>

 <section class="panel" id="chat">
  <h2>✦ Ask NEXORA</h2>
  <p>Ask questions, learn, brainstorm or write code.</p>
  <textarea id="chatPrompt" placeholder="Ask me anything..."></textarea>
  <div class="actions">
   <button class="primary" onclick="send('chat')">Generate Answer ↗</button>
   <button class="secondary" onclick="clearAll('chatPrompt','chatResult')">Clear</button>
  </div>
  <div class="result" id="chatResult">Your AI response will appear here.</div>
 </section>

 <section class="panel hidden" id="image">
  <h2>◈ AI Image Generator</h2>
  <p>Describe the image you want to create.</p>
  <textarea id="imagePrompt" placeholder="A futuristic city at sunset, cinematic lighting, ultra realistic..."></textarea>
  <div class="actions">
   <button class="primary" onclick="send('image')">Create Image ↗</button>
  </div>
  <div class="result" id="imageResult">Your generated image will appear here.</div>
 </section>

 <section class="panel hidden" id="video">
  <h2>▶ AI Video Studio</h2>
  <p>Describe your dream video.</p>
  <textarea id="videoPrompt" placeholder="A cinematic drone shot over a beautiful futuristic city..."></textarea>
  <div class="actions">
   <button class="primary" onclick="send('video')">Create Video ↗</button>
  </div>
  <div class="result" id="videoResult">Your generated video will appear here.</div>
 </section>

 <div class="cards">
  <div class="card"><div class="emoji">🧠</div><h3>Smart Knowledge</h3><p>Learn science, maths, technology and more.</p></div>
  <div class="card"><div class="emoji">🎨</div><h3>Image Creation</h3><p>Transform creative prompts into artwork.</p></div>
  <div class="card"><div class="emoji">🎬</div><h3>Video Creation</h3><p>Bring your stories and ideas to life.</p></div>
 </div>
</main>
<footer>© 2026 NEXORA AI · Made for imagination</footer>

<script>
function show(id,btn){
 document.querySelectorAll('.panel').forEach(p=>p.classList.add('hidden'));
 document.getElementById(id).classList.remove('hidden');
 document.querySelectorAll('.tab').forEach(b=>b.classList.remove('active'));
 btn.classList.add('active');
}
function clearAll(a,b){
 document.getElementById(a).value='';
 document.getElementById(b).textContent='Ready for your next idea.';
}
async function send(type){
 const prompt=document.getElementById(type+'Prompt').value.trim();
 const result=document.getElementById(type+'Result');
 if(!prompt){result.textContent='Please enter a prompt first.';return}
 result.textContent='Connecting to AI...';
 try{
  const response=await fetch('/api/'+type,{
   method:'POST',
   headers:{'Content-Type':'application/json'},
   body:JSON.stringify({prompt})
  });
  if(!response.ok) throw new Error('AI service is not connected yet.');
  const data=await response.json();
  if(type==='chat') result.textContent=data.text||'No response received.';
  if(type==='image' && data.url){
   result.innerHTML='';
   const img=document.createElement('img');
   img.src=data.url;img.style.width='100%';img.style.borderRadius='12px';
   result.appendChild(img);
  }
  if(type==='video' && data.url){
   result.innerHTML='';
   const video=document.createElement('video');
   video.src=data.url;video.controls=true;
   video.style.width='100%';video.style.borderRadius='12px';
   result.appendChild(video);
  }
 }catch(e){
  result.textContent='AI backend not connected. Add a secure server and connect an AI provider to enable this feature.';
 }
}
</script>
</body>
</html>
