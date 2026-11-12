[spelling-bee-round2.html](https://github.com/user-attachments/files/34727032/spelling-bee-round2.html)
<!DOCTYPE html>
<html lang="zh-CN"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Spelling Bee 模拟练习 · Round 2</title>
<style>
:root{--bg:#f4f6fb;--card:#fff;--tx:#1d2433;--mu:#667085;--ac:#2563eb;--ok:#16a34a;--no:#dc2626;--bd:#d9dfec;color-scheme:light;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#10131a;--card:#1a1f2b;--tx:#eef1f8;--mu:#9aa4b8;--ac:#6d9bff;--ok:#4ade80;--no:#f87171;--bd:#2b3345;color-scheme:dark}}
:root[data-theme="dark"]{--bg:#10131a;--card:#1a1f2b;--tx:#eef1f8;--mu:#9aa4b8;--ac:#6d9bff;--ok:#4ade80;--no:#f87171;--bd:#2b3345;color-scheme:dark}
html{height:100%;scroll-padding-top:env(safe-area-inset-top,0px)}
body{margin:0;min-height:100%;background:var(--bg);color:var(--tx);font-family:-apple-system,BlinkMacSystemFont,"Segoe UI","PingFang SC","Microsoft YaHei",Roboto,sans-serif;display:flex;justify-content:center}
main{width:100%;max-width:920px;padding:20px;box-sizing:border-box}
.card{background:var(--card);border:1px solid var(--bd);border-radius:20px;padding:26px;margin-bottom:16px}
h1{font-size:1.6rem;margin:4px 0 6px}h2{font-size:1.05rem;margin:18px 0 8px;color:var(--mu);font-weight:600}
p.s{color:var(--mu);margin:0 0 6px}
.row{display:flex;flex-wrap:wrap;gap:10px}
button{font:inherit;border:2px solid var(--bd);background:var(--card);color:var(--tx);border-radius:14px;padding:14px 20px;font-size:1.05rem;cursor:pointer;touch-action:manipulation}
button.on{border-color:var(--ac);background:var(--ac);color:#fff}
button.big{font-size:1.3rem;padding:18px 28px;border-color:var(--ac);background:var(--ac);color:#fff;width:100%;margin-top:22px}
button.ok{background:var(--ok);border-color:var(--ok);color:#fff}button.no{background:var(--no);border-color:var(--no);color:#fff}
.hide{display:none!important}
.top{display:flex;justify-content:space-between;align-items:center;color:var(--mu);font-size:1.05rem}
.bar{height:14px;border-radius:7px;background:var(--bd);overflow:hidden;margin:12px 0}
.bar>i{display:block;height:100%;width:100%;background:var(--ac);transition:background .2s}
.bar.low>i{background:var(--no)}
.letter{font-size:clamp(4rem,14vw,7rem);font-weight:800;letter-spacing:.08em;text-align:center;color:var(--ac);line-height:1.1;margin:10px 0}
.def{font-size:clamp(1.5rem,4.2vw,2.3rem);line-height:1.4;text-align:center;margin:8px 0 18px}
.t{font-size:3rem;font-weight:800;text-align:center}
.ans{font-size:clamp(2.6rem,10vw,4.5rem);font-weight:800;text-align:center;color:var(--ok);letter-spacing:.05em}
.ex{text-align:center;color:var(--mu);font-size:1.2rem;margin:6px 0 14px}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:14px}
ul.w{columns:2;margin:8px 0;padding-left:22px;font-size:1.15rem}
</style></head><body><main>

<section id="start" class="card">
<h1>✏️ Spelling Bee 模拟练习</h1>
<p class="s">Level 01 · 共 <b id="tot"></b> 个词。决赛形式：屏幕显示释义、例句、首字母和字母数，孩子在答题板上写出单词。</p>
<h2>每题时间</h2><div class="row" id="secs"></div>
<h2>题目数量</h2><div class="row" id="cnt"></div>
<h2>提示</h2><div class="row"><button id="lenb">显示字母个数：关</button></div>
<button class="big" id="go">开始考试</button>
<button id="wrongBtn" class="hide" style="width:100%;margin-top:12px"></button>
</section>

<section id="quiz" class="card hide">
<div class="top"><span id="pg"></span><span id="sc"></span></div>
<div class="bar" id="bar"><i id="fill"></i></div>
<div class="t" id="tm"></div>
<div class="letter" id="let"></div>
<div class="def" id="def"></div>
<div class="ex" id="exq"></div>
<div id="ansBox" class="hide">
<div class="ans" id="ans"></div>
<div class="ex" id="ex"></div>
<div class="row" style="justify-content:center"><button id="spk">🔊 听发音</button></div>
<div class="grid2"><button class="ok" id="yes">✓ 写对了</button><button class="no" id="nah">✗ 写错了</button></div>
</div>
<button class="big" id="show">显示答案</button>
<button id="quit" style="margin-top:14px;width:100%">结束本次练习</button>
</section>

<section id="res" class="card hide">
<h1 id="rt"></h1><p class="s" id="rs"></p>
<h2>需要再练的词</h2><ul class="w" id="wl"></ul>
<button class="big" id="again">再来一次</button>
<button id="retry" class="hide" style="width:100%;margin-top:12px">只练这次的错词</button>
</section>
</main>
<script>
const WORDS=[{"w": "network", "d": "a system of computers that are connected to one or more other computers.", "e": "The office network allows users to share files and software."}, {"w": "online", "d": "to be connected to a computer, a computer network, or the internet.", "e": "Most people use online banking to pay their bills."}, {"w": "talk", "d": "to say words in order to express your thoughts, feelings, speak and opinions.", "e": "The teacher is talking to the little boy."}, {"w": "win", "d": "to finish first or get the most points.", "e": "She is going to win the race."}, {"w": "net", "d": "the thing that you must hit the ball over in games such as tennis.", "e": "The ball didn't go over the net."}, {"w": "pop", "d": "modern music that is popular, especially with young people.", "e": "I like pop, but my parents don't."}, {"w": "gold", "d": "If something is ____, it is made of ____ or is the color ____.", "e": "Gold coins are worth a lot of money."}, {"w": "tax", "d": "an amount of money that you have to pay to the government.", "e": "They have to pay taxes according to their income."}, {"w": "make", "d": "to build, create, or produce something.", "e": "Anna makes cookies with her grandmother."}, {"w": "lost", "d": "Someone who is ____ does not know where they are.", "e": "The man is lost and cannot find his hotel."}, {"w": "link", "d": "to join or connect two or more things together.", "e": "Everyone in the company is linked through email."}, {"w": "have", "d": "to eat or drink it.", "e": "They have pizza once a week."}, {"w": "stay", "d": "to be at the same place for a period of time.", "e": "The man stays at the hotel for work."}, {"w": "album", "d": "several pieces of music collected into a single item.", "e": "I bought this album on a Cd."}, {"w": "move", "d": "to change from one place or position to another.", "e": "When you exercise, you move your body to become fit."}, {"w": "park", "d": "a large open area with grass and trees, where people can walk, play games, and have fun.", "e": "They often play sports at the park."}, {"w": "off", "d": "not to be switched on or connected.", "e": "Turn off the television if you are not watching it."}, {"w": "place", "d": "a position, building, town, or area.", "e": "Which places have you been to?"}, {"w": "drug", "d": "a substance used as a medicine.", "e": "Scientists are working on a new drug for cancer."}, {"w": "study", "d": "to read, memorize facts, attend school, and do work in order to learn about a subject.", "e": "Alice is studying her notes in the library after class."}, {"w": "look", "d": "to turn your eyes toward it so you can see it in greater detail or with more intention.", "e": "They're looking at the toys."}, {"w": "public", "d": "affecting all the people in a community.", "e": "The country's leader made a public announcement about the budget for defense."}, {"w": "skin", "d": "the outer part of a person's whole body.", "e": "They take care of their skin."}, {"w": "internet", "d": "a system that connects computers throughout the world.", "e": "The boy is searching for information on the internet."}, {"w": "body", "d": "the whole physical person.", "e": "Exercise is good for the body."}, {"w": "cast", "d": "a hard covering that is put on the body when there is a broken bone.", "e": "Adam broke his arm, so he has to wear a cast for a month."}, {"w": "line", "d": "a long narrow mark on a surface.", "e": "She is drawing a line with a ruler."}, {"w": "else", "d": "to say that something is in addition to something.", "e": "She's trying to remember what else she needs."}, {"w": "region", "d": "a large or particular area of a country or of the world.", "e": "The southern region of the country is very beautiful."}, {"w": "impact", "d": "a powerful force, effect, or influence.", "e": "The high winds had a terrible impact on the tall trees."}, {"w": "gas", "d": "a substance such as air, which is neither solid nor liquid.", "e": "Gas is coming out of a hole in the earth."}, {"w": "play", "d": "to take part in a sport or game.", "e": "They play a lot of video games."}, {"w": "via", "d": "traveling through a place on the way to a destination.", "e": "We could only get to New Zealand via Australia."}, {"w": "hold", "d": "to stop someone or something from moving.", "e": "The police hold the woman to stop her from running away."}, {"w": "data", "d": "information that is produced or stored by a computer.", "e": "They are collecting data for research."}, {"w": "record", "d": "to store music and sound on a computer or tape.", "e": "She is here to record a song."}, {"w": "full-time", "d": "to do something for the same number of hours unemployed that are considered standard or normal for a job.", "e": "Brian stays home with his son full-time while his wife works."}, {"w": "understand", "d": "to know why or how something happens or works.", "e": "I don't understand how to do this homework."}, {"w": "even", "d": "one that can be divided into perfectly equal parts.", "e": "Two, four, six, eight, and ten are all even numbers."}, {"w": "company", "d": "an organization that makes, buys, and sells something for money.", "e": "Greg works at a software company."}, {"w": "fat", "d": "A ____ person or animal weighs too much.", "e": "skinny, thin My cat is very fat."}, {"w": "party", "d": "a social event where people enjoy themselves.", "e": "I want to have a party outside for my wedding."}, {"w": "program", "d": "a show on television or the radio.", "e": "This is a program about cooking."}, {"w": "shot", "d": "a part of a movie that is filmed by one camera without stopping.", "e": "The Tv show's opening shot is of the city."}, {"w": "laptop", "d": "a portable computer that can work with a battery.", "e": "He uses a laptop for work when he travels."}, {"w": "forget", "d": "to be unable to think of or remember something.", "e": "Jake forgot how to say\"hello\"in German."}, {"w": "get", "d": "to take, receive, or buy it.", "e": "They went to the supermarket to get some food."}, {"w": "unit", "d": "a part of a machine or system that has a particular use.", "e": "The building has several air conditioning units."}, {"w": "wind", "d": "moving air, especially when it moves strongly or quickly.", "e": "There's a strong wind today."}, {"w": "film", "d": "a story that is told with sound and moving pictures.", "e": "The actress is making a film."}, {"w": "sister", "d": "a girl or woman who has the same parents as you.", "e": "Luke loves his little sister."}, {"w": "card", "d": "a small piece of plastic from a bank that you use to pay for things or get money.", "e": "He's paying with a card."}, {"w": "computer", "d": "an electronic machine that is used to store and sort information.", "e": "The students are learning how to use a computer."}, {"w": "buy", "d": "to get something by paying money for it.", "e": "They want to buy a new house."}, {"w": "evil", "d": "enjoy in ghar ming others.", "e": "People say that the haunted house is full of evil spirits."}, {"w": "result", "d": "the information you get from an exam, experiment, or test.", "e": "The patient received good results from the doctor."}, {"w": "mental", "d": "connected with or happening in the mind.", "e": "They have a mental image of what they want their new home to look like."}, {"w": "normal", "d": "When something is ____, it is usual or ordinary.", "e": "It is normal to feel tired after exercising."}, {"w": "away", "d": "Someone is ____ when they are not at home or near a certain place, person, or thing.", "e": "They are going away for the weekend."}, {"w": "gun", "d": "a wea pont hat shoots bullets.", "e": "The soldier has to carry a gun to protect herself."}, {"w": "software", "d": "a program that runs on a computer and performs certain functions.", "e": "He is installing software on his computer."}, {"w": "hit", "d": "to strike it quickly and with force, with your hand a bat, or a racket.", "e": "He can hit the ball hard."}, {"w": "pay", "d": "to give money in order to buy something.", "e": "Most people pay for things by credit card."}, {"w": "fit", "d": "strong and healthy.", "e": "He rides a bike to stay fit."}, {"w": "policy", "d": "an accepted set of ideas or rules about the way things are done.", "e": "The new policy affected all of the staff."}, {"w": "game", "d": "an activity or sport that people play according to rules.", "e": "A group of friends is trying a new game."}, {"w": "list", "d": "a series of names, words, or numbers.", "e": "She is writing a list of what she needs to prepare for the presentation."}, {"w": "website", "d": "a collection of computer documents connected to the internet, where a company or a person puts information.", "e": "The company's website makes it easy to do online shopping."}, {"w": "legal", "d": "you are allowed to do it by law.", "e": "My dad is a lawyer so people always ask for legal advice."}, {"w": "risk", "d": "the possibility that something dangerous may happen.", "e": "The police were well aware of the risks involved."}, {"w": "final", "d": "If something is ____, it comes at the end or is the last.", "e": "The final score was 0-0."}, {"w": "less", "d": "If there is ____ of something, it is smaller in size or value than another.", "e": "She has less water than he does."}, {"w": "master", "d": "to become very good at it.", "e": "Karen is frying to master the piano."}, {"w": "art", "d": "paintings, drawings, and sculptures.", "e": "She is taking an art class."}, {"w": "cup", "d": "a round container used for drinking.", "e": "I drink with my blue cup."}, {"w": "club", "d": "a group of people who meet to participate in an activity.", "e": "The boys are a part of an after-school soccer club."}, {"w": "report", "d": "a spoken or written description of an event or situation.", "e": "All of the information I gathered went into my report."}, {"w": "find", "d": "to discover, see, or get it.", "e": "She's at the store to find some new clothes."}, {"w": "media", "d": "them a in way that large numbers of people receive information and entertainment.", "e": "A lot of people access media on the internet."}, {"w": "plus", "d": "the part of a sum which says a number will be added.", "e": "One plus one equals two."}, {"w": "news", "d": "new information about something that has happened recently.", "e": "They received very good news."}, {"w": "try", "d": "to attempt to do something.", "e": "She is trying to decide what to buy."}, {"w": "term", "d": "a word or expression used within a particular subject or area.", "e": "The term used for a first-year student in high school or college is \"freshman.\""}, {"w": "tell", "d": "to say something to someone.", "e": "Betty is telling a secret to her daughter."}, {"w": "both", "d": "to talk about two people or things together.", "e": "Both my brother and my sister work at the store."}, {"w": "take", "d": "to bring it or them with you when you go somewhere.", "e": "When we go shopping, we take our own shopping bag."}, {"w": "back", "d": "the part of the body behind you from your neck to your waist.", "e": "She has a pain in her back."}, {"w": "video", "d": "a short movie that is made to go with a piece of popular music.", "e": "They're making a video for their new song."}, {"w": "union", "d": "a group of people who share a common goal.", "e": "The students'union met to organize a charity event."}, {"w": "full", "d": "If someone is ____, they have had enough to eat and do not want any more.", "e": "The woman already ate, so she's full."}, {"w": "police", "d": "a group of people whose job is to stop others from breaking rules and laws.", "e": "The police wear uniforms and often carry guns."}, {"w": "well", "d": "Someone who is ____ is healthy, not sick.", "e": "He was sick yesterday, but he's feeling well today."}, {"w": "demand", "d": "to ask strongly for something.", "e": "I demand that you give me back my pencil!"}, {"w": "band", "d": "a group of musicians or singers who perform together.", "e": "The band practices after school."}, {"w": "team", "d": "a group of people who play a sport or game together against another group.", "e": "We're on a baseball team."}, {"w": "digital", "d": "using or characterized by computer technology.", "e": "In this digital world, the internet is part of our everyday lives."}, {"w": "mark", "d": "the score you get for a piece of work.", "e": "Mike got a great mark on the test he took."}, {"w": "cost", "d": "the price of something.", "e": "The cost of a taxi ride depends on the distance traveled."}, {"w": "often", "d": "When something happens ____, it happens over and over.", "e": "Nick is in good shape because he exercises often."}, {"w": "army", "d": "a large group of soldiers who fight on the ground.", "e": "These soldiers are members of the South Korean army."}];
const $=id=>document.getElementById(id);
const fix=s=>s.replace(/(^|[\s(])"/g,'$1“').replace(/"/g,'”').replace(/'/g,'’');
let secs=15,count=30,showLen=true,queue=[],idx=0,results=[],timer=null,endAt=0,answered=false;
let stored=[];try{stored=JSON.parse(localStorage.getItem('sb2_wrong')||'[]')}catch(e){}
function save(){try{localStorage.setItem('sb2_wrong',JSON.stringify(stored))}catch(e){}}
function shuffle(a){a=a.slice();for(let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[a[i],a[j]]=[a[j],a[i]]}return a}
function show(id){['start','quiz','res'].forEach(s=>$(s).classList.toggle('hide',s!==id));window.scrollTo(0,0)}
function opts(box,vals,cur,fmt,set){box.innerHTML='';vals.forEach(v=>{const b=document.createElement('button');b.textContent=fmt(v);b.className=v===cur?'on':'';b.onclick=()=>{set(v);[...box.children].forEach(c=>c.className='');b.className='on'};box.appendChild(b)})}
function home(){$('tot').textContent=WORDS.length;
 opts($('secs'),[10,15,20,30],secs,v=>v+' 秒',v=>secs=v);
 opts($('cnt'),[10,30,50,WORDS.length],count,v=>v===WORDS.length?'全部 '+v:v===30?'30 题（决赛）':v+' 题',v=>count=v);
 $('lenb').textContent='显示字母个数：'+(showLen?'开':'关');
 $('wrongBtn').classList.toggle('hide',!stored.length);$('wrongBtn').textContent='📒 只练错题本（'+stored.length+' 个词）';show('start')}
$('lenb').onclick=()=>{showLen=!showLen;$('lenb').textContent='显示字母个数：'+(showLen?'开':'关')};
function begin(list){queue=shuffle(list);idx=0;results=[];show('quiz');ask()}
$('go').onclick=()=>begin(shuffle(WORDS).slice(0,count));
$('wrongBtn').onclick=()=>begin(WORDS.filter(w=>stored.includes(w.w)));
function beep(){try{const c=new(window.AudioContext||window.webkitAudioContext)(),o=c.createOscillator(),g=c.createGain();o.connect(g);g.connect(c.destination);o.frequency.value=880;g.gain.value=.2;o.start();o.stop(c.currentTime+.35)}catch(e){}}
const IRR={forget:'forgot|forget'};
function blankEx(w,s){const base=(IRR[w]||w)+'(?:s|es|ed|d|ing|ly)?';const st=w.length>2&&w.endsWith('y')?base+'|'+w.slice(0,-1)+'(?:ies|ied|ying)':base;return s.replace(new RegExp('\\b(?:'+st+')\\b','gi'),'_____')}
function ask(){const q=queue[idx];answered=false;clearInterval(timer);
 $('pg').textContent='第 '+(idx+1)+' / '+queue.length+' 题';$('sc').textContent='✓ '+results.filter(r=>r.ok).length;
 const rest=[...q.w].slice(1).map(c=>c==='-'?'-':'_').join(' ');
 $('let').textContent=q.w[0]+(showLen?' '+rest:' …');
 const d=fix(q.d);$('def').textContent=d[0].toUpperCase()+d.slice(1);
 $('exq').textContent=fix(blankEx(q.w,q.e));$('exq').classList.remove('hide');
 $('ansBox').classList.add('hide');$('show').classList.remove('hide');$('show').textContent='提前显示答案';
 endAt=Date.now()+secs*1000;tick();timer=setInterval(tick,100)}
function tick(){const left=Math.max(0,endAt-Date.now());
 $('fill').style.width=(left/(secs*1000)*100)+'%';$('bar').classList.toggle('low',left<=3000);
 $('tm').textContent=left>0?Math.ceil(left/1000):'时间到！';
 if(left<=0){clearInterval(timer);beep();$('show').textContent='显示答案'}}
function reveal(){clearInterval(timer);const q=queue[idx];answered=true;
 $('tm').textContent='';$('exq').classList.add('hide');$('ans').textContent=q.w;$('ex').textContent=fix(q.e);
 $('ansBox').classList.remove('hide');$('show').classList.add('hide');speak()}
function speak(){try{const q=queue[idx];speechSynthesis.cancel();const u=new SpeechSynthesisUtterance(q.w);u.lang='en-US';u.rate=.8;speechSynthesis.speak(u)}catch(e){}}
$('show').onclick=reveal;$('spk').onclick=speak;
function mark(ok){results.push({w:queue[idx].w,ok});idx++;idx<queue.length?ask():finish()}
$('yes').onclick=()=>mark(true);$('nah').onclick=()=>mark(false);
$('quit').onclick=()=>{clearInterval(timer);results.length?finish():home()};
function finish(){clearInterval(timer);const ok=results.filter(r=>r.ok).length,bad=results.filter(r=>!r.ok).map(r=>r.w);
 stored=stored.filter(w=>!results.some(r=>r.ok&&r.w===w));bad.forEach(w=>{if(!stored.includes(w))stored.push(w)});save();
 $('rt').textContent=ok+' / '+results.length+' 正确（'+Math.round(ok/results.length*100)+'%）';
 $('rs').textContent=bad.length?'错题已加入错题本，可以单独再练。':'全部答对，太棒了！';
 $('wl').innerHTML='';bad.forEach(w=>{const li=document.createElement('li');li.textContent=w;$('wl').appendChild(li)});
 $('retry').classList.toggle('hide',!bad.length);$('retry').onclick=()=>begin(WORDS.filter(x=>bad.includes(x.w)));show('res')}
$('again').onclick=home;
home();
</script></body></html>
