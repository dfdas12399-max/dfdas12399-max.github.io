<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Chess</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;}
body{background:#1a1a1a;display:flex;flex-direction:column;align-items:center;min-height:100vh;padding:24px 16px;font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;color:#e8e0d0;}
h1{font-size:18px;font-weight:500;letter-spacing:0.08em;color:#aaa;margin-bottom:12px;text-transform:uppercase;}
#save-bar{display:flex;gap:8px;margin-bottom:12px;align-items:center;}
.sbtn{padding:6px 14px;font-size:12px;border:0.5px solid #444;border-radius:6px;background:#252525;color:#ccc;cursor:pointer;display:flex;align-items:center;gap:6px;}
.sbtn:hover{background:#2e2e2e;}
.sbtn svg{width:13px;height:13px;fill:none;stroke:#aaa;stroke-width:2;stroke-linecap:round;stroke-linejoin:round;}
#save-msg{font-size:12px;color:#555;min-width:160px;}
#app{display:flex;flex-direction:column;align-items:center;}
.pbar{width:560px;display:flex;align-items:center;justify-content:space-between;padding:10px 14px;background:#252525;border:0.5px solid #3a3a3a;}
#top-bar{border-radius:10px 10px 0 0;border-bottom:none;}
#bot-bar{border-radius:0 0 10px 10px;border-top:none;}
.pname{font-size:14px;font-weight:500;color:#e8e0d0;}
.pelo{font-size:12px;color:#888;margin-top:2px;}
.cbadge{font-size:11px;color:#888;background:#1e1e1e;border:0.5px solid #3a3a3a;border-radius:6px;padding:3px 10px;}
#username-wrap{display:flex;align-items:center;gap:8px;}
#username-input{font-size:14px;font-weight:500;border:none;background:transparent;color:#e8e0d0;width:130px;outline:none;border-bottom:0.5px solid transparent;}
#username-input:focus{border-bottom:0.5px solid #555;}
.elbl{font-size:11px;color:#555;cursor:pointer;}
.elbl:hover{color:#888;}
#sbar{width:560px;padding:7px 14px;background:#1e1e1e;border-left:0.5px solid #3a3a3a;border-right:0.5px solid #3a3a3a;display:flex;align-items:center;gap:8px;}
#stxt{font-size:13px;color:#888;flex:1;text-align:center;}
#stxt.danger{color:#e05555;}
#stxt.success{color:#5ab05a;}
#nbtn{padding:5px 14px;font-size:12px;border:0.5px solid #444;border-radius:6px;background:#252525;color:#ccc;cursor:pointer;}
#nbtn:hover{background:#2e2e2e;}
#cwrap{position:relative;}
canvas#board{display:block;cursor:pointer;border-left:0.5px solid #3a3a3a;border-right:0.5px solid #3a3a3a;}
#pover{display:none;position:absolute;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.65);align-items:center;justify-content:center;z-index:10;}
#pover.show{display:flex;}
#pbox{background:#252525;border-radius:12px;border:0.5px solid #444;padding:20px;text-align:center;}
#pbox p{font-size:13px;color:#888;margin-bottom:12px;}
#pchoices{display:flex;gap:10px;}
#pchoices canvas{cursor:pointer;border-radius:8px;border:0.5px solid #444;}
#pchoices canvas:hover{border-color:#777;}
#gpanel{display:none;width:560px;padding:14px;background:#252525;border:0.5px solid #3a3a3a;border-top:none;border-radius:0 0 10px 10px;flex-direction:column;gap:10px;}
#gpanel.show{display:flex;}
#glabel{font-size:13px;color:#888;}
#grow{display:flex;align-items:center;gap:10px;}
#gslider{flex:1;-webkit-appearance:none;appearance:none;height:4px;border-radius:2px;background:#444;outline:none;}
#gslider::-webkit-slider-thumb{-webkit-appearance:none;width:16px;height:16px;border-radius:50%;background:#c8b89a;cursor:pointer;}
#gslider::-moz-range-thumb{width:16px;height:16px;border-radius:50%;background:#c8b89a;cursor:pointer;border:none;}
#gval{font-size:14px;font-weight:500;min-width:36px;color:#e8e0d0;text-align:right;}
#gbtn{padding:6px 16px;font-size:12px;border:0.5px solid #555;border-radius:6px;background:#1e1e1e;color:#ccc;cursor:pointer;}
#gbtn:hover{background:#2a2a2a;}
#gresult{font-size:13px;color:#888;display:none;}
#finput{display:none;}
</style>
</head>
<body>
<h1>Chess</h1>
 
<div id="save-bar">
  <button class="sbtn" onclick="downloadSave()">
    <svg viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4"/><polyline points="7 10 12 15 17 10"/><line x1="12" y1="15" x2="12" y2="3"/></svg>
    Download save
  </button>
  <button class="sbtn" onclick="document.getElementById('finput').click()">
    <svg viewBox="0 0 24 24"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4"/><polyline points="17 8 12 3 7 8"/><line x1="12" y1="3" x2="12" y2="15"/></svg>
    Load save
  </button>
  <span id="save-msg">Progress also saves in your browser automatically</span>
  <input type="file" id="finput" accept=".chesssave" onchange="uploadSave(event)">
</div>
 
<div id="app">
  <div class="pbar" id="top-bar">
    <div>
      <div class="pname">AI</div>
      <div class="pelo" id="ai-elo">ELO: ???</div>
    </div>
    <div class="cbadge">Black</div>
  </div>
 
  <div id="sbar">
    <button id="nbtn" onclick="newGame()">New game</button>
    <span id="stxt">Your turn</span>
    <div style="width:72px"></div>
  </div>
 
  <div id="cwrap">
    <canvas id="board" width="560" height="560"></canvas>
    <div id="pover">
      <div id="pbox">
        <p>Promote pawn to:</p>
        <div id="pchoices"></div>
      </div>
    </div>
  </div>
 
  <div class="pbar" id="bot-bar">
    <div id="username-wrap">
      <input id="username-input" maxlength="20" value="You" />
      <span class="elbl" onclick="document.getElementById('username-input').focus()">edit</span>
    </div>
    <div style="display:flex;align-items:center;gap:12px;">
      <div style="text-align:right;">
        <div class="pelo" id="player-elo">ELO: 800</div>
        <div class="pelo" id="player-stats">0W 0L 0D</div>
      </div>
      <div class="cbadge">White</div>
    </div>
  </div>
 
  <div id="gpanel">
    <div id="glabel">Game over! Guess the AI's ELO to earn bonus ELO (max +60):</div>
    <div id="grow">
      <input type="range" id="gslider" min="400" max="2000" step="1" value="800">
      <span id="gval">800</span>
      <button id="gbtn" onclick="submitGuess()">Submit guess</button>
    </div>
    <div id="gresult"></div>
  </div>
</div>
 
<script>
const TILE=70;
const cv=document.getElementById('board');
const ctx=cv.getContext('2d');
const LIGHT='#F0D9B5',DARK='#B58863';
const SEL_C='rgba(80,200,80,0.45)',HINT_C='rgba(30,140,255,0.35)',LAST_C='rgba(205,210,56,0.40)',CHECK_C='rgba(220,20,60,0.45)';
 
let playerElo=800,aiElo=800,username='You';
let wins=0,losses=0,draws=0,gamesPlayed=0;
let selected=null,hints=[],lastMove=null;
let board,whiteTurn,enPassant,castling,gameOver;
let guessSubmitted=false;
 
// ── Save/load ─────────────────────────────────────────────────────────────────
function getSaveObj(){return{version:1,elo:playerElo,name:username,wins,losses,draws,gamesPlayed};}
 
function applySave(obj){
  try{
    const d=typeof obj==='string'?JSON.parse(obj):obj;
    if(!d||d.version!==1)throw 0;
    playerElo=d.elo||800; username=d.name||'You';
    wins=d.wins||0; losses=d.losses||0; draws=d.draws||0; gamesPlayed=d.gamesPlayed||0;
    document.getElementById('username-input').value=username;
    refreshHUD();
    return true;
  }catch(e){return false;}
}
 
function downloadSave(){
  persistLocal();
  const blob=new Blob([JSON.stringify(getSaveObj(),null,2)],{type:'application/json'});
  const url=URL.createObjectURL(blob);
  const a=document.createElement('a');
  a.href=url;
  a.download=(username||'player').replace(/\s+/g,'_')+'.chesssave';
  a.click();
  URL.revokeObjectURL(url);
  msg('Save file downloaded!','#5ab05a');
}
 
function uploadSave(e){
  const file=e.target.files[0];if(!file)return;
  const reader=new FileReader();
  reader.onload=ev=>{
    if(applySave(ev.target.result)){
      persistLocal();
      msg('Loaded: '+username+' (ELO '+playerElo+')','#5ab05a');
      newGame();
    }else{
      msg('Invalid save file','#e05555');
    }
  };
  reader.readAsText(file);
  e.target.value='';
}
 
function msg(text,color){
  const el=document.getElementById('save-msg');
  el.textContent=text;el.style.color=color||'#555';
  clearTimeout(el._t);
  el._t=setTimeout(()=>{el.textContent='Progress also saves in your browser automatically';el.style.color='#555';},3500);
}
 
function persistLocal(){
  try{localStorage.setItem('chess_v3',JSON.stringify(getSaveObj()));}catch(e){}
}
function loadLocal(){
  try{const r=localStorage.getItem('chess_v3');if(r)applySave(r);}catch(e){}
}
 
document.getElementById('username-input').addEventListener('change',function(){
  username=this.value.trim()||'You';
  this.value=username;
  persistLocal();
});
 
function refreshHUD(){
  document.getElementById('player-elo').textContent='ELO: '+playerElo;
  document.getElementById('player-stats').textContent=wins+'W '+losses+'L '+draws+'D';
}
 
// ── Guess panel ───────────────────────────────────────────────────────────────
const gslider=document.getElementById('gslider');
const gval=document.getElementById('gval');
gslider.addEventListener('input',()=>{gval.textContent=gslider.value;});
 
function setAiElo(){
  aiElo=Math.max(400,Math.min(2400,playerElo+Math.round((Math.random()-0.5)*200)));
  document.getElementById('ai-elo').textContent='ELO: ???';
  gslider.value=playerElo; gval.textContent=playerElo;
}
function revealAiElo(){document.getElementById('ai-elo').textContent='ELO: '+aiElo;}
 
function showGuessPanel(){
  guessSubmitted=false;
  document.getElementById('gresult').style.display='none';
  document.getElementById('gbtn').style.display='';
  gslider.value=playerElo; gval.textContent=playerElo;
  document.getElementById('gpanel').classList.add('show');
}
 
function submitGuess(){
  if(guessSubmitted)return;
  guessSubmitted=true;
  const guess=parseInt(gslider.value);
  const diff=Math.abs(guess-aiElo);
  let bonus=diff===0?60:diff<=10?55:diff<=25?45:diff<=50?35:diff<=100?20:diff<=150?10:diff<=200?5:0;
  revealAiElo();
  playerElo+=bonus;
  refreshHUD();
  persistLocal();
  const res=document.getElementById('gresult');
  res.style.display='block';
  if(diff===0){res.textContent='Perfect! +'+bonus+' ELO. AI was exactly '+aiElo+'.';res.style.color='#5ab05a';}
  else if(bonus>0){res.textContent='Off by '+diff+'. +'+bonus+' ELO! AI was '+aiElo+'.';res.style.color='#5ab05a';}
  else{res.textContent='Off by '+diff+'. No bonus. AI was '+aiElo+'.';res.style.color='#888';}
  document.getElementById('gbtn').style.display='none';
}
 
// ── Board ─────────────────────────────────────────────────────────────────────
function initBoard(){return[['r','n','b','q','k','b','n','r'],['p','p','p','p','p','p','p','p'],[null,null,null,null,null,null,null,null],[null,null,null,null,null,null,null,null],[null,null,null,null,null,null,null,null],[null,null,null,null,null,null,null,null],['P','P','P','P','P','P','P','P'],['R','N','B','Q','K','B','N','R']];}
 
function newGame(){
  board=initBoard();whiteTurn=true;enPassant=null;
  castling={K:true,Q:true,k:true,q:true};
  gameOver=false;selected=null;hints=[];lastMove=null;
  setAiElo();
  document.getElementById('gpanel').classList.remove('show');
  setStatus('Your turn');
  drawBoard();
}
 
// ── Chess logic ───────────────────────────────────────────────────────────────
const isW=p=>p&&p===p.toUpperCase();
const isB=p=>p&&p===p.toLowerCase();
const sameColor=(a,b)=>(isW(a)&&isW(b))||(isB(a)&&isB(b));
const inBounds=(r,c)=>r>=0&&r<8&&c>=0&&c<8;
 
function pawnMoves(b,r,c,ep){
  const p=b[r][c],mv=[],d=isW(p)?-1:1,sr=isW(p)?6:1;
  if(inBounds(r+d,c)&&!b[r+d][c]){mv.push([r+d,c]);if(r===sr&&!b[r+2*d][c])mv.push([r+2*d,c]);}
  for(const dc of[-1,1]){const nr=r+d,nc=c+dc;if(inBounds(nr,nc)){if(b[nr][nc]&&!sameColor(p,b[nr][nc]))mv.push([nr,nc]);if(ep&&nr===ep[0]&&nc===ep[1])mv.push([nr,nc]);}}
  return mv;
}
function knightMoves(b,r,c){const p=b[r][c];return[[-2,-1],[-2,1],[-1,-2],[-1,2],[1,-2],[1,2],[2,-1],[2,1]].map(([dr,dc])=>[r+dr,c+dc]).filter(([nr,nc])=>inBounds(nr,nc)&&!sameColor(p,b[nr][nc]));}
function slidingMoves(b,r,c,dirs){const p=b[r][c],mv=[];for(const[dr,dc]of dirs){let nr=r+dr,nc=c+dc;while(inBounds(nr,nc)){if(!b[nr][nc])mv.push([nr,nc]);else{if(!sameColor(p,b[nr][nc]))mv.push([nr,nc]);break;}nr+=dr;nc+=dc;}}return mv;}
function pseudoMoves(b,r,c,ep){
  const p=b[r][c];if(!p)return[];const t=p.toUpperCase();
  if(t==='P')return pawnMoves(b,r,c,ep);
  if(t==='N')return knightMoves(b,r,c);
  if(t==='B')return slidingMoves(b,r,c,[[-1,-1],[-1,1],[1,-1],[1,1]]);
  if(t==='R')return slidingMoves(b,r,c,[[-1,0],[1,0],[0,-1],[0,1]]);
  if(t==='Q')return slidingMoves(b,r,c,[[-1,-1],[-1,1],[1,-1],[1,1],[-1,0],[1,0],[0,-1],[0,1]]);
  if(t==='K')return[[-1,-1],[-1,0],[-1,1],[0,-1],[0,1],[1,-1],[1,0],[1,1]].map(([dr,dc])=>[r+dr,c+dc]).filter(([nr,nc])=>inBounds(nr,nc)&&!sameColor(p,b[nr][nc]));
  return[];
}
function findKing(b,white){const t=white?'K':'k';for(let r=0;r<8;r++)for(let c=0;c<8;c++)if(b[r][c]===t)return[r,c];return null;}
function isAttacked(b,r,c,byW,ep=null){for(let rr=0;rr<8;rr++)for(let cc=0;cc<8;cc++){const p=b[rr][cc];if(p&&isW(p)===byW&&pseudoMoves(b,rr,cc,ep).some(([tr,tc])=>tr===r&&tc===c))return true;}return false;}
function inCheck(b,white,ep=null){const k=findKing(b,white);if(!k)return true;return isAttacked(b,k[0],k[1],!white,ep);}
 
function applyMove(b,fr,fc,tr,tc,ep,cast,promo){
  const nb=b.map(row=>[...row]);const p=nb[fr][fc];let nep=null,nc={...cast};
  if(p&&p.toUpperCase()==='P'&&ep&&tr===ep[0]&&tc===ep[1])nb[tr+(isW(p)?1:-1)][tc]=null;
  if(p&&p.toUpperCase()==='K'&&Math.abs(tc-fc)===2){if(tc>fc){nb[fr][tc-1]=nb[fr][7];nb[fr][7]=null;}else{nb[fr][tc+1]=nb[fr][0];nb[fr][0]=null;}}
  nb[tr][tc]=p;nb[fr][fc]=null;
  if(p==='P'&&tr===0)nb[tr][tc]=promo||'Q';
  if(p==='p'&&tr===7)nb[tr][tc]=promo?promo.toLowerCase():'q';
  if(p&&p.toUpperCase()==='P'&&Math.abs(tr-fr)===2)nep=[(fr+tr)/2,fc];
  if(p==='K'){nc.K=false;nc.Q=false;}if(p==='k'){nc.k=false;nc.q=false;}
  if(fr===7&&fc===7||tr===7&&tc===7)nc.K=false;if(fr===7&&fc===0||tr===7&&tc===0)nc.Q=false;
  if(fr===0&&fc===7||tr===0&&tc===7)nc.k=false;if(fr===0&&fc===0||tr===0&&tc===0)nc.q=false;
  return[nb,nep,nc];
}
 
function legalMoves(b,r,c,ep,cast,wt){
  const p=b[r][c];if(!p||isW(p)!==wt)return[];
  let mv=pseudoMoves(b,r,c,ep);
  if(p==='K'&&r===7&&c===4){
    if(cast.K&&!b[7][5]&&!b[7][6]&&!inCheck(b,true)&&!isAttacked(b,7,5,false)&&!isAttacked(b,7,6,false))mv.push([7,6]);
    if(cast.Q&&!b[7][3]&&!b[7][2]&&!b[7][1]&&!inCheck(b,true)&&!isAttacked(b,7,3,false)&&!isAttacked(b,7,2,false))mv.push([7,2]);
  }
  if(p==='k'&&r===0&&c===4){
    if(cast.k&&!b[0][5]&&!b[0][6]&&!inCheck(b,false)&&!isAttacked(b,0,5,true)&&!isAttacked(b,0,6,true))mv.push([0,6]);
    if(cast.q&&!b[0][3]&&!b[0][2]&&!b[0][1]&&!inCheck(b,false)&&!isAttacked(b,0,3,true)&&!isAttacked(b,0,2,true))mv.push([0,2]);
  }
  return mv.filter(([tr,tc])=>{const[nb,nep]=applyMove(b,r,c,tr,tc,ep,cast);return!inCheck(nb,isW(p),nep);});
}
function allLegal(b,ep,cast,wt){
  const out=[];
  for(let r=0;r<8;r++)for(let c=0;c<8;c++){const p=b[r][c];if(p&&isW(p)===wt)legalMoves(b,r,c,ep,cast,wt).forEach(m=>out.push([[r,c],m]));}
  return out;
}
 
// ── AI ────────────────────────────────────────────────────────────────────────
const PV={p:100,n:320,b:330,r:500,q:900,k:20000};
const PST={
  P:[[0,0,0,0,0,0,0,0],[50,50,50,50,50,50,50,50],[10,10,20,30,30,20,10,10],[5,5,10,25,25,10,5,5],[0,0,0,20,20,0,0,0],[5,-5,-10,0,0,-10,-5,5],[5,10,10,-20,-20,10,10,5],[0,0,0,0,0,0,0,0]],
  N:[[-50,-40,-30,-30,-30,-30,-40,-50],[-40,-20,0,0,0,0,-20,-40],[-30,0,10,15,15,10,0,-30],[-30,5,15,20,20,15,5,-30],[-30,0,15,20,20,15,0,-30],[-30,5,10,15,15,10,5,-30],[-40,-20,0,5,5,0,-20,-40],[-50,-40,-30,-30,-30,-30,-40,-50]],
  B:[[-20,-10,-10,-10,-10,-10,-10,-20],[-10,0,0,0,0,0,0,-10],[-10,0,5,10,10,5,0,-10],[-10,5,5,10,10,5,5,-10],[-10,0,10,10,10,10,0,-10],[-10,10,10,10,10,10,10,-10],[-10,5,0,0,0,0,5,-10],[-20,-10,-10,-10,-10,-10,-10,-20]],
  R:[[0,0,0,0,0,0,0,0],[5,10,10,10,10,10,10,5],[-5,0,0,0,0,0,0,-5],[-5,0,0,0,0,0,0,-5],[-5,0,0,0,0,0,0,-5],[-5,0,0,0,0,0,0,-5],[-5,0,0,0,0,0,0,-5],[0,0,0,5,5,0,0,0]],
  Q:[[-20,-10,-10,-5,-5,-10,-10,-20],[-10,0,0,0,0,0,0,-10],[-10,0,5,5,5,5,0,-10],[-5,0,5,5,5,5,0,-5],[0,0,5,5,5,5,0,-5],[-10,5,5,5,5,5,0,-10],[-10,0,5,0,0,0,0,-10],[-20,-10,-10,-5,-5,-10,-10,-20]],
  K:[[-30,-40,-40,-50,-50,-40,-40,-30],[-30,-40,-40,-50,-50,-40,-40,-30],[-30,-40,-40,-50,-50,-40,-40,-30],[-30,-40,-40,-50,-50,-40,-40,-30],[-20,-30,-30,-40,-40,-30,-30,-20],[-10,-20,-20,-20,-20,-20,-20,-10],[20,20,0,0,0,0,20,20],[20,30,10,0,0,10,30,20]]
};
function evaluate(b){let s=0;for(let r=0;r<8;r++)for(let c=0;c<8;c++){const p=b[r][c];if(!p)continue;const t=p.toUpperCase();const v=(PV[p.toLowerCase()]||0)+(PST[t]?isW(p)?PST[t][r][c]:PST[t][7-r][c]:0);s+=isW(p)?-v:v;}return s;}
function minimax(b,ep,cast,depth,alpha,beta,max){
  const mv=allLegal(b,ep,cast,!max);
  if(depth===0||mv.length===0){if(mv.length===0)return max?(inCheck(b,false,ep)?10000:0):(inCheck(b,true,ep)?-10000:0);return evaluate(b);}
  if(max){let best=-Infinity;for(const[[fr,fc],[tr,tc]]of mv){const[nb,nep,nc]=applyMove(b,fr,fc,tr,tc,ep,cast);best=Math.max(best,minimax(nb,nep,nc,depth-1,alpha,beta,false));alpha=Math.max(alpha,best);if(beta<=alpha)break;}return best;}
  else{let best=Infinity;for(const[[fr,fc],[tr,tc]]of mv){const[nb,nep,nc]=applyMove(b,fr,fc,tr,tc,ep,cast);best=Math.min(best,minimax(nb,nep,nc,depth-1,alpha,beta,true));beta=Math.min(beta,best);if(beta<=alpha)break;}return best;}
}
function aiDepth(){return playerElo<700?1:playerElo<1000?2:3;}
function pickMove(moves){
  const noise=Math.max(0,130-(playerElo-600)*0.28);
  if(Math.random()*260<noise)return moves[Math.floor(Math.random()*moves.length)];
  const depth=aiDepth();let best=-Infinity,bestMove=moves[0];
  for(const[[fr,fc],[tr,tc]]of moves){const[nb,nep,nc]=applyMove(board,fr,fc,tr,tc,enPassant,castling);let score=minimax(nb,nep,nc,depth,-Infinity,Infinity,false)+(Math.random()-0.5)*20;if(score>best){best=score;bestMove=[[fr,fc],[tr,tc]];}}
  return bestMove;
}
 
// ── Status ────────────────────────────────────────────────────────────────────
function setStatus(text,cls=''){const el=document.getElementById('stxt');el.textContent=text;el.className=cls;}
 
function updateStatus(){
  const mv=allLegal(board,enPassant,castling,whiteTurn);
  if(!mv.length){
    gamesPlayed++;
    if(inCheck(board,whiteTurn,enPassant)){
      if(!whiteTurn){wins++;playerElo+=30;setStatus('Checkmate! You win! +30 ELO','success');}
      else{losses++;setStatus('Checkmate! AI wins.','danger');}
    }else{draws++;setStatus('Stalemate — draw.');}
    refreshHUD();persistLocal();gameOver=true;showGuessPanel();
  }else if(inCheck(board,whiteTurn,enPassant)){
    setStatus(whiteTurn?'You are in check!':'AI is in check!','danger');
  }else{
    setStatus(whiteTurn?'Your turn':'AI thinking...');
  }
}
 
// ── Promotion ─────────────────────────────────────────────────────────────────
function askPromotion(){
  return new Promise(resolve=>{
    const ov=document.getElementById('pover'),cd=document.getElementById('pchoices');
    cd.innerHTML='';
    ['Q','R','B','N'].forEach(pc=>{
      const c=document.createElement('canvas');c.width=70;c.height=70;
      const cx=c.getContext('2d');cx.fillStyle='#c8b89a';cx.fillRect(0,0,70,70);
      drawPiece(cx,pc,35,35,28,true);
      c.title={Q:'Queen',R:'Rook',B:'Bishop',N:'Knight'}[pc];
      c.onclick=()=>{ov.classList.remove('show');resolve(pc);};
      cd.appendChild(c);
    });
    ov.classList.add('show');
  });
}
 
// ── Input ─────────────────────────────────────────────────────────────────────
async function handleClick(e){
  if(gameOver||!whiteTurn)return;
  const rect=cv.getBoundingClientRect();
  const mx=e.clientX-rect.left,my=e.clientY-rect.top;
  const c=Math.floor(mx/TILE),r=Math.floor(my/TILE);
  if(c<0||c>7||r<0||r>7)return;
  if(selected){
    const[sr,sc]=selected;
    if(hints.find(([hr,hc])=>hr===r&&hc===c)){
      const promo=board[sr][sc]==='P'&&r===0?await askPromotion():null;
      const[nb,nep,nc]=applyMove(board,sr,sc,r,c,enPassant,castling,promo);
      board=nb;enPassant=nep;castling=nc;lastMove=[[sr,sc],[r,c]];
      selected=null;hints=[];whiteTurn=false;updateStatus();drawBoard();
      if(!gameOver)setTimeout(doAiMove,300);
      return;
    }
    selected=null;hints=[];
  }
  const p=board[r][c];
  if(p&&isW(p)){const mv=legalMoves(board,r,c,enPassant,castling,true);if(mv.length){selected=[r,c];hints=mv;}}
  drawBoard();
}
 
function doAiMove(){
  const mv=allLegal(board,enPassant,castling,false);if(!mv.length)return;
  const[[fr,fc],[tr,tc]]=pickMove(mv);
  const promo=board[fr][fc]==='p'&&tr===7?'Q':null;
  const[nb,nep,nc]=applyMove(board,fr,fc,tr,tc,enPassant,castling,promo);
  board=nb;enPassant=nep;castling=nc;lastMove=[[fr,fc],[tr,tc]];
  whiteTurn=true;updateStatus();drawBoard();
}
cv.addEventListener('click',handleClick);
 
// ── Drawing ───────────────────────────────────────────────────────────────────
function drawPiece(cx,piece,x,y,size,forceW){
  const white=forceW!==undefined?forceW:isW(piece);
  const t=piece.toUpperCase();
  const fill=white?'#fff':'#1a1a1a',stroke=white?'#999':'#666';
  cx.save();cx.strokeStyle=stroke;cx.fillStyle=fill;cx.lineWidth=1.5;
  if(t==='K'){
    cx.beginPath();cx.ellipse(x,y+size*.15,size*.35,size*.45,0,0,Math.PI*2);cx.fill();cx.stroke();
    cx.fillRect(x-size*.08,y-size*.65,size*.16,size*.5);cx.strokeRect(x-size*.08,y-size*.65,size*.16,size*.5);
    cx.fillRect(x-size*.25,y-size*.5,size*.5,size*.15);cx.strokeRect(x-size*.25,y-size*.5,size*.5,size*.15);
  }else if(t==='Q'){
    cx.beginPath();cx.ellipse(x,y+size*.2,size*.32,size*.38,0,0,Math.PI*2);cx.fill();cx.stroke();
    for(let i=0;i<5;i++){const a=-Math.PI/2+i*(Math.PI*2/5);cx.beginPath();cx.arc(x+Math.cos(a)*size*.28,y-size*.15+Math.sin(a)*size*.28,size*.08,0,Math.PI*2);cx.fill();cx.stroke();}
  }else if(t==='R'){
    cx.fillRect(x-size*.28,y-size*.25,size*.56,size*.6);cx.strokeRect(x-size*.28,y-size*.25,size*.56,size*.6);
    for(const ox of[-size*.23,0,size*.23]){cx.fillRect(x+ox-size*.09,y-size*.5,size*.18,size*.28);cx.strokeRect(x+ox-size*.09,y-size*.5,size*.18,size*.28);}
  }else if(t==='B'){
    cx.beginPath();cx.ellipse(x,y+size*.2,size*.25,size*.38,0,0,Math.PI*2);cx.fill();cx.stroke();
    cx.beginPath();cx.ellipse(x,y-size*.28,size*.18,size*.28,0,0,Math.PI*2);cx.fill();cx.stroke();
    cx.beginPath();cx.arc(x,y-size*.58,size*.07,0,Math.PI*2);cx.fill();cx.stroke();
  }else if(t==='N'){
    cx.beginPath();cx.moveTo(x-size*.1,y+size*.4);cx.lineTo(x+size*.3,y+size*.4);cx.lineTo(x+size*.3,y+size*.15);cx.lineTo(x+size*.15,y-size*.05);cx.lineTo(x+size*.3,y-size*.35);cx.lineTo(x+size*.1,y-size*.55);cx.lineTo(x-size*.15,y-size*.35);cx.lineTo(x-size*.3,y-size*.1);cx.lineTo(x-size*.3,y+size*.15);cx.closePath();cx.fill();cx.stroke();
    cx.fillStyle=white?'#888':'#aaa';cx.beginPath();cx.arc(x+size*.05,y-size*.3,size*.06,0,Math.PI*2);cx.fill();
  }else if(t==='P'){
    cx.beginPath();cx.arc(x,y-size*.22,size*.22,0,Math.PI*2);cx.fill();cx.stroke();
    cx.beginPath();cx.ellipse(x,y+size*.2,size*.28,size*.3,0,0,Math.PI*2);cx.fill();cx.stroke();
    cx.fillRect(x-size*.08,y-size*.02,size*.16,size*.22);cx.strokeRect(x-size*.08,y-size*.02,size*.16,size*.22);
  }
  cx.restore();
}
 
function drawBoard(){
  ctx.clearRect(0,0,560,560);
  for(let r=0;r<8;r++)for(let c=0;c<8;c++){ctx.fillStyle=(r+c)%2===0?LIGHT:DARK;ctx.fillRect(c*TILE,r*TILE,TILE,TILE);}
  if(lastMove)for(const[lr,lc]of lastMove){ctx.fillStyle=LAST_C;ctx.fillRect(lc*TILE,lr*TILE,TILE,TILE);}
  if(inCheck(board,whiteTurn,enPassant)){const k=findKing(board,whiteTurn);if(k){ctx.fillStyle=CHECK_C;ctx.fillRect(k[1]*TILE,k[0]*TILE,TILE,TILE);}}
  if(selected){ctx.fillStyle=SEL_C;ctx.fillRect(selected[1]*TILE,selected[0]*TILE,TILE,TILE);}
  for(const[hr,hc]of hints){ctx.fillStyle=HINT_C;ctx.beginPath();ctx.arc(hc*TILE+TILE/2,hr*TILE+TILE/2,TILE*.17,0,Math.PI*2);ctx.fill();}
  for(let r=0;r<8;r++)for(let c=0;c<8;c++){const p=board[r][c];if(p)drawPiece(ctx,p,c*TILE+TILE/2,r*TILE+TILE/2,TILE*.38);}
  ctx.font='11px sans-serif';ctx.textBaseline='top';
  for(let i=0;i<8;i++){ctx.fillStyle=i%2===0?DARK:LIGHT;ctx.fillText(String(8-i),3,i*TILE+3);ctx.fillText('abcdefgh'[i],i*TILE+TILE-10,7*TILE+TILE-14);}
}
 
// ── Start ─────────────────────────────────────────────────────────────────────
loadLocal();
refreshHUD();
newGame();
</script>
</body>
</html>
