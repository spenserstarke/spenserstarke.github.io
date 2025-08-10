<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
  <title>SevenAteNine — Single File</title>
  <style>
    :root{
      --bg: #0a0a0a; /* neutral-950 */
      --panel: #1f2937; /* slate-800-ish */
      --panel2: #0f172a; /* slate-900 */
      --text: #e5e7eb; /* slate-200 */
      --muted: #94a3b8; /* slate-400 */
      --border: #334155; /* slate-700 */
      --danger: #ef4444; /* red-500 */
      --emerald: #10b981; /* emerald-500/600 mix */
      --emerald-dark: #059669;
      --amber: #d97706;
      --amber-dark: #b45309;
      --cyan: #0891b2;
      --cyan-dark: #0e7490;
      --slate-btn: #334155;
      --slate-btn-dark: #475569;
      --indigo-200: #c7d2fe;
      --black: #000;
      --amber-700: #b45309;
      --indigo-700: #4338ca;
      --rose-700: #be123c;
      --slate-600: #475569;
      --tile-gap: 6px;
      --radius: 12px;
      --radius-sm: 8px;
      --shadow: 0 8px 20px rgba(0,0,0,.25);
    }
    *{ box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
    html, body { height: 100%; }
    body {
      margin: 0; padding: 8px;
      background: var(--bg);
      color: var(--text);
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, Noto Sans, "Helvetica Neue", Arial, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    }
    h1 { margin: 4px 0 2px; font-size: 20px; font-weight: 800; text-align: center; }
    .center-col { width: 100%; display: flex; flex-direction: column; align-items: center; gap: 8px; }
    .statline { font-size: 12px; opacity: 0.85; text-align: center; }
    .grid-wrap { position: relative; padding: 8px; border-radius: 16px; background: #1f2937; }
    .grid { display: grid; }
    .tile {
      position: relative;
      background: var(--panel2);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      width: var(--tile);
      height: var(--tile);
      cursor: pointer;
      display: flex; align-items: center; justify-content: center;
    }
    .tile.exit { background: rgba(16,185,129,0.25); }
    .tile.selA { outline: 3px solid #fde047; /* yellow-300 */ }
    .tile.selB { outline: 3px solid #fb923c; /* orange-400 */ }
    .tile.danger { border-color: var(--danger); }
    .rock {
      position: absolute; inset: 4px; border-radius: 8px;
      background: rgba(100,116,139,0.75);
      color: #0b1020; font-size: 12px; display:flex; align-items:center; justify-content:center;
    }
    .monster {
      position: absolute; inset: 4px; border-radius: 8px;
      display:flex; align-items:center; justify-content:center; font-weight: 800; font-size: 14px;
    }
    .m-NORMAL  { background: var(--slate-600); }
    .m-BRUTE   { background: var(--black); color: #fff; }
    .m-HUNGRY  { background: var(--amber-700); }
    .m-STALWART{ background: var(--indigo-700); }
    .m-GUARD   { background: var(--rose-700); }
    .frozen { position:absolute; right:-4px; top:-6px; font-size:18px; }
    .loot {
      position: absolute; inset: 4px; border-radius: 8px;
      background: rgba(71,85,105,0.45);
      display:flex; align-items:center; justify-content:center; font-size: 18px;
    }
    .wizard { position: absolute; inset: 6px; border-radius: 6px; background: var(--indigo-200); }
    .btnrow { display:flex; flex-wrap: wrap; gap: 8px; align-items:center; justify-content:center; }
    .btn {
      border: 0; color: #fff; font-weight: 700; font-size: 14px;
      padding: 6px 12px; border-radius: 10px; cursor: pointer;
      transition: filter .15s ease, transform .05s ease;
      box-shadow: var(--shadow);
    }
    .btn:active { transform: translateY(1px) scale(0.99); }
    .btn.green { background: var(--emerald); }
    .btn.green:hover { filter: brightness(0.95); }
    .btn.amber { background: var(--amber); }
    .btn.amber:hover { filter: brightness(0.95); }
    .btn.cyan { background: var(--cyan); }
    .btn.cyan:hover { filter: brightness(0.95); }
    .btn.slate { background: var(--slate-btn); }
    .btn.slate:hover { filter: brightness(1.1); }
    .panel { width: 100%; max-width: var(--gridW); margin-top: 6px; padding: 8px; border-radius: 12px; background: rgba(30,41,59,0.6); }
    .inv { display:flex; gap: 10px; align-items:center; flex-wrap: wrap; }
    .inv button { display:flex; gap:6px; align-items:center; padding: 6px 10px; border-radius: 8px; background: #475569; color:#fff; border:0; cursor:pointer; }
    .inv button:disabled { opacity: 0.4; cursor: default; }
    .hud-msg { font-size: 12px; opacity: 0.9; text-align:center; }
    .footer { font-size: 11px; opacity: 0.7; text-align:center; padding-bottom: 16px; }
    .mana { display:flex; gap: 6px; align-items:center; justify-content:center; }
    .chip {
      display:inline-block; padding: 3px 8px; border-radius: 999px; background: #0e7490; color:#fff; font-weight:700; font-size: 12px;
    }
    .floating-right, .floating-left {
      position: fixed; z-index: 50; bottom: calc(env(safe-area-inset-bottom) + 12px);
    }
    .floating-right { right: 12px; }
    .floating-left  { left: 12px; }
    .thumbMain { width: 112px; height: 56px; border-radius: 999px; background: var(--emerald); color:#fff; font-weight:800; border:0; box-shadow: var(--shadow); }
    .thumbAlt  { width: 92px;  height: 48px; border-radius: 999px; background: var(--cyan); color:#fff; font-weight:700; border:0; box-shadow: var(--shadow); }
    @media (min-width: 768px){
      .floating-right, .floating-left { display:none; }
    }
  </style>
</head>
<body>
  <div id="app" class="center-col"></div>

  <script>
  (function(){
    // ----------------- Helpers & utilities -----------------
    const COLS = 5;
    const MType = { NORMAL:"NORMAL", BRUTE:"BRUTE", HUNGRY:"HUNGRY", STALWART:"STALWART", GUARD:"GUARD" };
    const mtypeColorClass = (t)=> 'm-'+t;

    const invItems = [
      { key:"armor",       label:"🛡", tip:"Armor: blocks one damage automatically on hit.", click:false },
      { key:"hungerScroll",label:"📜", tip:"Hunger Scroll: pick a monster; it counts as rank-2 this turn.", click:true },
      { key:"freeze",      label:"❄", tip:"Freeze: click center tile to stun 3x3 for a turn.", click:true },
      { key:"teleport",    label:"✦", tip:"Teleport: click an empty tile to blink.", click:true },
      { key:"raiseRank",   label:"⬆︎", tip:"Raise a monster by +1 rank (clamped 1..13).", click:true },
      { key:"lowerRank",   label:"⬇︎", tip:"Lower a monster by -1 rank (clamped 1..13).", click:true },
      { key:"swapTwo",     label:"⇄", tip:"Swap ANY two monsters (pick two targets).", click:true },
      { key:"gainMana3",   label:"+3", tip:"Gain +3 mana instantly.", click:true },
      { key:"revealRandom",label:"👁", tip:"Reveal a random hidden monster.", click:true },
      { key:"destroy",     label:"✖", tip:"Destroy a chosen monster.", click:true },
    ];

    function deepCloneGrid(g){ return g.map(r=>r.map(c=>c?{...c}:null)); }
    function inBounds(cols, rows, x, y){ return x>=0 && y>=0 && x<cols && y<rows; }
    function orth(x,y){ return [[x+1,y],[x-1,y],[x,y+1],[x,y-1]]; }
    function shuffled(xs){ const a=[...xs]; for(let i=a.length-1;i>0;i--){const j=(Math.random()*(i+1))|0; [a[i],a[j]]=[a[j],a[i]];} return a; }
    function drawDeck(){ let a=[],i=0; function resh(){a=Array.from({length:13},(_,j)=>j+1); for(let k=a.length-1;k>0;k--){const r=(Math.random()*(k+1))|0; [a[k],a[r]]=[a[r],a[k]];} i=0;} resh(); return ()=>{ if(i>=a.length) resh(); return a[i++]; }; }

    function levelSpecs(){
      return [
        { rows: 9,  specials: {}, powerups: {"2,7":"armor"}, dangerous: ["2,6"], dangerousExtra: 4, rocks: {"3,5":{strength:1}} },
        { rows: 10, specials: {"1,6":"BRUTE"}, powerups: {"1,5":"scroll","3,7":"freeze"}, dangerous: ["2,5","2,6"], dangerousExtra: 5, rocks: {} },
        { rows: 11, specials: {"2,3":"BRUTE","1,4":"BRUTE","3,6":"HUNGRY"}, powerups: {"4,7":"freeze","0,8":"teleport"}, dangerous: ["4,2","1,8"], dangerousExtra: 3, rocks: {"2,6":{strength:2}} },
        { rows: 12, specials: {"2,5":"HUNGRY","3,5":"HUNGRY","1,8":"STALWART"}, powerups: {"0,6":"teleport","4,3":"gem"}, dangerous: ["2,7","3,7","2,8"], dangerousExtra: 3, rocks: {} },
        { rows: 12, specials: {"1,5":"BRUTE","2,9":"HUNGRY","1,9":"STALWART"}, powerups: {"0,10":"armor","4,2":"gem","2,4":"scroll"}, dangerous: ["3,6","3,7","2,7"], dangerousExtra: 4, rocks: {"0,8":{strength:2},"4,3":{strength:3}} },
      ].map(s => ({ cols: COLS, ...s }));
    }

    function makeHiddenMask(spec, wizard, exit){
      const hide={};
      for(let y=0;y<spec.rows;y++){
        const dist=(spec.rows-1)-y;
        const toHide = dist<=1?0:Math.min(dist-1, spec.cols);
        if(toHide>0){
          const cols=shuffled(Array.from({length:spec.cols},(_,i)=>i));
          for(const x of cols.slice(0,toHide)){
            const k=`${x},${y}`;
            if(!(x===wizard[0]&&y===wizard[1]) && !(x===exit[0]&&y===exit[1])) hide[k]=true;
          }
        }
      }
      return hide;
    }

    function generateLevel(spec){
      const {cols, rows}=spec; const next=drawDeck();
      const wizard=[(Math.random()*cols)|0, rows-1];
      let exit=[(Math.random()*cols)|0, 0];
      for(let t=0;t<20;t++){ if(exit[0]!==wizard[0]) break; exit=[(Math.random()*cols)|0,0]; }
      const grid=Array.from({length:rows},()=>Array(cols).fill(null));
      const obstacles={...(spec.rocks||{})};
      for(let y=0;y<rows;y++) for(let x=0;x<cols;x++){
        if((x===wizard[0]&&y===wizard[1])||(x===exit[0]&&y===exit[1])) continue;
        const k=`${x},${y}`; if(obstacles[k]) continue;
        const mtype = spec.specials[k] || MType.NORMAL;
        grid[y][x] = { rank: next(), mtype, frozen:0, killCount:0, revealed:true, eatenStack:[] };
      }
      const loot={};
      Object.entries(spec.powerups||{}).forEach(([k,kind])=>{
        const [x,y]=k.split(',').map(Number);
        if(inBounds(cols,rows,x,y) && !obstacles[k] && !(x===wizard[0]&&y===wizard[1]) && !(x===exit[0]&&y===exit[1])){
          grid[y][x]=null; loot[k]=kind;
        }
      });
      const forbidden = new Set([`${wizard[0]},${wizard[1]}`,`${exit[0]},${exit[1]}`]);
      for(const [dx,dy] of [[1,0],[-1,0],[0,1],[0,-1],[1,1],[-1,1],[1,-1],[-1,-1]]) forbidden.add(`${wizard[0]+dx},${wizard[1]+dy}`);
      for(const k of (spec.dangerous||[])){ const [x,y]=k.split(',').map(Number); if(inBounds(cols,rows,x,y)&&grid[y][x]) grid[y][x].mtype=MType.GUARD; }
      const extras=spec.dangerousExtra||0, cands=[];
      for(let y=0;y<rows;y++) for(let x=0;x<cols;x++){ const k=`${x},${y}`; if(forbidden.has(k)) continue; if(!grid[y][x]) continue; if(grid[y][x].mtype!==MType.NORMAL) continue; cands.push([x,y]); }
      for(const [x,y] of shuffled(cands).slice(0,extras)) grid[y][x].mtype=MType.GUARD;
      const hidden=makeHiddenMask(spec, wizard, exit);
      for(let y=0;y<rows;y++) for(let x=0;x<cols;x++){ const m=grid[y][x]; if(m) m.revealed = !hidden[`${x},${y}`]; }
      return { cols, rows, grid, wizard, exit, loot, obstacles };
    }

    function pathReveal(state){
      const {cols,rows}=state; const [wx,wy]=state.wizard;
      const seen=new Set([`${wx},${wy}`]); const q=[[wx,wy]];
      const walk=(x,y)=> inBounds(cols,rows,x,y) && !state.obstacles[`${x},${y}`] && !state.grid[y][x];
      while(q.length){
        const [x,y]=q.shift();
        for(const [nx,ny] of orth(x,y)){
          const k=`${nx},${ny}`; if(!inBounds(cols,rows,nx,ny)||seen.has(k)) continue;
          if(walk(nx,ny)){ seen.add(k); q.push([nx,ny]); }
        }
      }
      const grid=deepCloneGrid(state.grid);
      for(let y=0;y<rows;y++) for(let x=0;x<cols;x++){
        const m=grid[y][x]; if(!m||m.revealed) continue;
        for(const [nx,ny] of orth(x,y)){ if(seen.has(`${nx},${ny}`)){ m.revealed=true; break; } }
      }
      return grid;
    }

    function hasAnyEatablePair(state){
      for(let y=0;y<state.rows;y++) for(let x=0;x<state.cols;x++){
        const A=state.grid[y][x]; if(!A||!A.revealed) continue;
        for(const [nx,ny] of orth(x,y)){
          if(!inBounds(state.cols,state.rows,nx,ny)) continue;
          const B=state.grid[ny][nx]; if(!B||!B.revealed) continue;
          const ar=A.rank, br=B.rank;
          const ok = (B.mtype===MType.BRUTE) ? (br===ar||br===ar+1) : (br===ar||br===ar+1||br===ar+2);
          if(ok) return true;
        }
      }
      return false;
    }

    function makeLevel(spec, levelIndex=0){
      let tries=0, L;
      while(tries<50){
        tries++; L=generateLevel(spec);
        const base={
          ...L,
          mana:2, health:1,
          inventory:{ armor:0,hungerScroll:0,freeze:0,teleport:0,raiseRank:0,lowerRank:0,swapTwo:0,gainMana3:0,revealRandom:0,destroy:0 },
          selectedA:null, selectedB:null,
          mode:"normal", powerType:null, powerFirst:null,
          rankOverrides:{},
          message:"Select two monsters, then SPACE to eat. S=Swap, M=Move. Use power-ups below.",
          turn:1, armorDrops:0,
          score:0, highScore:0, bestChainThisLevel:0, totalEatenThisLevel:0
        };
        if(levelIndex>=1){
          const kinds=["armor","scroll","freeze","teleport","gem"];
          const empties=[]; for(let y=0;y<base.rows;y++) for(let x=0;x<base.cols;x++) if(!base.grid[y][x] && !base.obstacles[`${x},${y}`]) empties.push([x,y]);
          for(const [x,y] of shuffled(empties).slice(0,3)) base.loot[`${x},${y}`]=kinds[(Math.random()*kinds.length)|0];
        }
        base.grid=pathReveal(base);
        if(hasAnyEatablePair(base)) return base;
      }
      return L;
    }

    function effRank(ns,x,y){ const k=`${x},${y}`; const m=ns.grid[y][x]; return (ns.rankOverrides && ns.rankOverrides[k]!=null) ? ns.rankOverrides[k] : m.rank; }

    // ----------------- State & rendering -----------------
    const specs = levelSpecs();
    let level = 0;
    let state = makeLevel(specs[0],0);
    let tile = 52; const GAP=4;
    let gridW = 0;

    function setState(next){
      state = next;
      render();
    }
    function mergeState(patch){ setState({...state, ...patch}); }

    function recomputeTile(){
      const vw = Math.max(320, Math.min(window.innerWidth, 428));
      const cols = state.cols;
      tile = Math.max(36, Math.floor((vw - GAP * (cols - 1)) / cols));
      gridW = tile * cols + GAP * (cols - 1);
      document.documentElement.style.setProperty('--tile', tile+'px');
      document.documentElement.style.setProperty('--gridW', gridW+'px');
    }

    function restart(){
      const spec = specs[level]; const base = makeLevel(spec, level);
      base.highScore = Math.max(state.highScore||0, state.score||0);
      setState(base);
    }
    function atExit(){ return state.wizard[0]===state.exit[0] && state.wizard[1]===state.exit[1]; }
    function finishLevel(ns){
      const gained = (ns.mana||0) + (ns.bestChainThisLevel||0) + (ns.totalEatenThisLevel||0);
      const newScore = (ns.score||0)+gained;
      const newHigh  = Math.max(ns.highScore||0, newScore);
      const li = Math.min(level+1, specs.length-1);
      const next = makeLevel(specs[li], li);
      next.inventory = { ...ns.inventory };
      next.score = newScore; next.highScore = newHigh;
      level = li; setState(next);
    }

    function applyPathReveal(ns){ ns.grid = pathReveal(ns); }

    function moveWizard(dx,dy){
      const [x,y]=state.wizard; const nx=x+dx, ny=y+dy;
      if(!inBounds(state.cols,state.rows,nx,ny)) return;
      const k=`${nx},${ny}`;
      if(state.obstacles[k]) return mergeState({message:"Rock blocks the way."});
      if(state.grid[ny][nx]) return mergeState({message:"Monster blocks the way."});
      let ns={...state, wizard:[nx,ny], selectedA:null, selectedB:null};
      if(ns.loot[k]){
        const kind=ns.loot[k]; const loot={...ns.loot}; delete loot[k]; ns.loot=loot;
        if(kind==="armor") ns.inventory.armor=(ns.inventory.armor||0)+1;
        if(kind==="scroll") ns.inventory.hungerScroll=(ns.inventory.hungerScroll||0)+1;
        if(kind==="freeze") ns.inventory.freeze=(ns.inventory.freeze||0)+1;
        if(kind==="teleport") ns.inventory.teleport=(ns.inventory.teleport||0)+1;
        if(kind==="gem") ns.mana+=5;
        ns.message=`Picked up ${kind}!`;
      }
      if(ns.wizard[0]===ns.exit[0] && ns.wizard[1]===ns.exit[1]){ finishLevel(ns); return; }
      applyPathReveal(ns); endTurnFrom(ns);
    }

    function canEatPair(ns, ax, ay, bx, by){
      if(!inBounds(ns.cols,ns.rows,ax,ay) || !inBounds(ns.cols,ns.rows,bx,by)) return false;
      if(Math.abs(ax-bx)+Math.abs(ay-by)!==1) return false;
      const A=ns.grid[ay][ax], B=ns.grid[by][bx]; if(!A||!B) return false;
      if(!A.revealed || !B.revealed) return false; if(A.frozen>0) return false;
      const ar = effRank(ns, ax, ay); const br = effRank(ns, bx, by);
      if (B.mtype===MType.BRUTE) return (br===ar || br===ar+1);
      return (br===ar || br===ar+1 || br===ar+2);
    }

    function maybeSpawnDrop(ns, bx, by, predator, prey){
      let loot={...ns.loot}; let armorDrops=ns.armorDrops;
      if((prey.rank<=5 || (predator.killCount||0)>=6) && Math.random()<0.35 && armorDrops<2){
        const spots=[];
        for(const [nx,ny] of orth(bx,by)){
          if(inBounds(ns.cols,ns.rows,nx,ny) && !ns.grid[ny][nx] && !ns.obstacles[`${nx},${ny}`] && !(nx===ns.wizard[0]&&ny===ns.wizard[1])) spots.push([nx,ny]);
        }
        if(spots.length){ const [pxx,pyy]=spots[(Math.random()*spots.length)|0]; loot[`${pxx},${pyy}`]="armor"; armorDrops+=1; }
      }
      ns.loot=loot; ns.armorDrops=armorDrops;
    }

    function eatFrom(ns, ax, ay, bx, by){
      const g = ns.grid; const predator = g[ay][ax]; const prey = g[by][bx];
      const ar = effRank(ns, ax, ay); const br = effRank(ns, bx, by);
      ns.mana += (ar===br) ? 2 : 1;
      const preyKills = prey.killCount || 0;
      predator.killCount = (predator.killCount||0) + 1 + preyKills;
      predator.eatenStack = [ ...(predator.eatenStack||[]), { rank: prey.rank, mtype: prey.mtype, eatenStack: prey.eatenStack||[] } ];
      ns.bestChainThisLevel = Math.max(ns.bestChainThisLevel||0, predator.killCount||0);
      ns.totalEatenThisLevel = (ns.totalEatenThisLevel||0)+1;
      g[by][bx]=predator; g[ay][ax]=null;
      maybeSpawnDrop(ns, bx, by, predator, prey);
      ns.grid = pathReveal(ns);
      ns.selectedA=null; ns.selectedB=null;
      ns.message=`${predator.rank} eats ${prey.rank}! ${ar===br?'+2':'+1'} mana.`;
    }

    function doEat(){
      const ns={...state};
      const A=ns.selectedA, B=ns.selectedB; if(!A||!B){ mergeState({message:"Select two adjacent monsters, then press Space."}); return false; }
      const [ax,ay]=A,[bx,by]=B; let did=false;
      if (canEatPair(ns, ax, ay, bx, by)) { eatFrom(ns, ax, ay, bx, by); endTurnFrom(ns); did=true; }
      else if (canEatPair(ns, bx, by, ax, ay)) { eatFrom(ns, bx, by, ax, ay); endTurnFrom(ns); did=true; }
      else { mergeState({message:"Those two can't eat each other."}); }
      return did;
    }

    function doSwap(){
      const A=state.selectedA, B=state.selectedB;
      if(!A||!B) return false;
      if(state.mana<1){ mergeState({message:"Need 1 mana."}); return false; }
      const [ax,ay]=A,[bx,by]=B; const keyA=`${ax},${ay}`, keyB=`${bx},${by}`;
      const monA=state.grid[ay][ax], monB=state.grid[by][bx];
      const lootA=state.loot[keyA], lootB=state.loot[keyB];
      if((monA && monA.mtype===MType.STALWART) || (monB && monB.mtype===MType.STALWART)){ mergeState({message:"Stalwarts cannot be swapped."}); return false; }
      if(lootA&&lootB){ mergeState({message:"Can't swap two powerups."}); return false; }
      if(!monA && !monB){ mergeState({message:"Pick at least one monster."}); return false; }
      const emptyA = !monA && !lootA, emptyB = !monB && !lootB;
      if(emptyA || emptyB){ mergeState({message:"Can't swap with an empty tile. Use Move (M)."}); return false; }
      const grid=deepCloneGrid(state.grid); const loot={...state.loot}; let did=false;
      if(monA&&monB){ const t=grid[ay][ax]; grid[ay][ax]=grid[by][bx]; grid[by][bx]=t; did=true; }
      else if(monA&&lootB){ if(bx===state.wizard[0]&&by===state.wizard[1]){ mergeState({message:"Can't swap onto the wizard."}); return false; } grid[by][bx]=monA; grid[ay][ax]=null; delete loot[keyB]; loot[keyA]=lootB; did=true; }
      else if(monB&&lootA){ if(ax===state.wizard[0]&&ay===state.wizard[1]){ mergeState({message:"Can't swap onto the wizard."}); return false; } grid[ay][ax]=monB; grid[by][bx]=null; delete loot[keyA]; loot[keyB]=lootA; did=true; }
      if(!did){ mergeState({message:"No valid swap available."}); return false; }
      const ns={...state, grid, loot, mana:state.mana-1, selectedA:null, selectedB:null, message:"Swapped (-1)."};
      endTurnFrom(ns); return true;
    }

    function moveMonster(){
      const A=state.selectedA, B=state.selectedB; if(!A||!B) return false;
      if(state.mana<1){ mergeState({message:"Need 1 mana."}); return false; }
      let [ax,ay]=A, [tx,ty]=B;
      const aIsEmpty = !state.grid[ay][ax] && !state.obstacles[`${ax},${ay}`] && !(ax===state.wizard[0]&&ay===state.wizard[1]);
      const bIsMonster = !!state.grid[ty][tx];
      if(aIsEmpty && bIsMonster){ [ax,ay,tx,ty] = [tx,ty,ax,ay]; }
      if(Math.abs(ax-tx)+Math.abs(ay-ty)!==1){ mergeState({message:"Destination must be adjacent."}); return false; }
      if(!state.grid[ay][ax]){ mergeState({message:"Pick a monster then destination."}); return false; }
      if(tx===state.wizard[0]&&ty===state.wizard[1]){ mergeState({selectedA:null,selectedB:null,message:"Can't move into the wizard."}); return false; }
      const destK=`${tx},${ty}`; if(state.loot[destK]){ mergeState({selectedA:null,selectedB:null,message:"Can't move into a powerup; swap instead."}); return false; }
      const grid=deepCloneGrid(state.grid); const src=grid[ay][ax];
      if(state.obstacles[destK]){
        if(src.rank<=3 && state.obstacles[destK].strength<=2){ const obs={...state.obstacles}; delete obs[destK]; grid[ty][tx]=src; grid[ay][ax]=null; const ns={...state,grid,obstacles:obs,mana:state.mana-1,selectedA:null,selectedB:null,message:"Crushed rock!"}; endTurnFrom(ns); return true; }
        mergeState({selectedA:null,selectedB:null,message:"Too weak to crush."}); return false;
      }
      if(grid[ty][tx]){ mergeState({selectedA:null,selectedB:null,message:"Destination not empty."}); return false; }
      grid[ty][tx]=src; grid[ay][ax]=null; const ns={...state,grid,mana:state.mana-1,selectedA:null,selectedB:null,message:"Moved monster (-1)."}; endTurnFrom(ns); return true;
    }

    function applyPowerUpOne(ns, type, x, y){
      const grid = deepCloneGrid(ns.grid);
      if(type==="teleport"){
        if(grid[y][x] || ns.obstacles[`${x},${y}`]){ ns.message="Teleport needs an empty tile."; return false; }
        ns.wizard=[x,y]; ns.inventory.teleport--; ns.grid=pathReveal({...ns,grid}); ns.message="Blink!"; return true;
      }
      if(type==="freeze"){
        for(let yy=y-1; yy<=y+1; yy++) for(let xx=x-1; xx<=x+1; xx++){
          if(!inBounds(ns.cols,ns.rows,xx,yy)) continue; if(ns.obstacles[`${xx},${yy}`]) continue;
          const m=grid[yy][xx]; if(m) m.frozen=Math.max(m.frozen,1);
        }
        ns.inventory.freeze--; ns.grid=grid; ns.grid=pathReveal(ns); ns.message="Freeze!"; return true;
      }
      const m = grid[y][x]; if(!m){ ns.message="Pick a monster."; return false; }
      if(type==="hungerScroll"){ if(!(ns.inventory.hungerScroll>0)) return false; const key=`${x},${y}`; ns.rankOverrides={...ns.rankOverrides,[key]:Math.max(1,m.rank-2)}; ns.inventory.hungerScroll--; ns.grid=grid; ns.message="Hunger Scroll applied (-2 this turn)."; return true; }
      if(type==="raiseRank"){ if(!(ns.inventory.raiseRank>0)) return false; m.rank=Math.min(13,m.rank+1); ns.inventory.raiseRank--; ns.grid=grid; ns.message="+1 rank."; return true; }
      if(type==="lowerRank"){ if(!(ns.inventory.lowerRank>0)) return false; m.rank=Math.max(1,m.rank-1); ns.inventory.lowerRank--; ns.grid=grid; ns.message="-1 rank."; return true; }
      if(type==="destroy"){ if(!(ns.inventory.destroy>0)) return false; grid[y][x]=null; ns.inventory.destroy--; ns.grid=grid; ns.grid=pathReveal(ns); ns.message="Destroyed."; return true; }
      ns.message="That power isn’t implemented yet."; return false;
    }
    function applyPowerUpTwo(ns, type, a, b){
      if(type!=="swapTwo") return false; if(!(ns.inventory.swapTwo>0)) return false;
      const [ax,ay]=a,[bx,by]=b;
      if(!inBounds(ns.cols,ns.rows,ax,ay)||!inBounds(ns.cols,ns.rows,bx,by)) { ns.message="Pick two monsters."; return false; }
      const grid=deepCloneGrid(ns.grid); const A=grid[ay][ax], B=grid[by][bx];
      if(!A||!B){ ns.message="Pick two monsters."; return false; }
      if (A.mtype===MType.STALWART || B.mtype===MType.STALWART) { ns.message="Stalwarts cannot be swapped."; return false; }
      grid[ay][ax]=B; grid[by][bx]=A; ns.grid=grid; ns.inventory.swapTwo--; ns.message="Swapped two monsters."; ns.grid=pathReveal(ns); return true;
    }

    function endTurnFrom(base){
      const grid=deepCloneGrid(base.grid);
      const {cols,rows}=base; let inv={...base.inventory}; let pendingDamage=0; let rankOverrides={};
      for(let y=0;y<rows;y++) for(let x=0;x<cols;x++){ const m=grid[y][x]; if(m&&m.frozen>0) m.frozen-=1; }
      for(let y=0;y<rows;y++) for(let x=0;x<cols;x++){
        const m=grid[y][x]; if(!m||m.mtype!==MType.HUNGRY||m.frozen>0||!m.revealed) continue;
        const opts=[]; for(const [nx,ny] of orth(x,y)){
          if(!inBounds(cols,rows,nx,ny)) continue; if(nx===base.wizard[0]&&ny===base.wizard[1]) continue; const t=grid[ny][nx]; if(!t||!t.revealed) continue;
          const ar=m.rank, br=t.rank; const ok = (t.mtype===MType.BRUTE) ? (br===ar||br===ar+1) : (br===ar||br===ar+1||br===ar+2); if(ok) opts.push({nx,ny,br});
        }
        if(opts.length){ opts.sort((a,b)=>a.br-b.br); const {nx,ny}=opts[0]; const prey=grid[ny][nx]; m.killCount=(m.killCount||0)+1+(prey.killCount||0); m.eatenStack=[...(m.eatenStack||[]),{rank:prey.rank,mtype:prey.mtype,eatenStack:prey.eatenStack||[]}]; grid[ny][nx]=m; grid[y][x]=null; }
      }
      const [wx,wy]=base.wizard;
      for(let y=0;y<rows;y++) for(let x=0;x<cols;x++){
        const m=grid[y][x]; if(!m||m.frozen>0) continue; const dx=Math.abs(x-wx), dy=Math.abs(y-wy); const adj=dx+dy===1;
        if(m.mtype===MType.BRUTE){ if(adj) pendingDamage+=2; }
        else if(m.mtype===MType.GUARD){ if(adj) pendingDamage+=1; }
        else if(m.mtype===MType.HUNGRY){ if(adj) pendingDamage+=1; }
      }
      let damage=pendingDamage, health=base.health;
      if(damage>0){ if(inv.armor>0){ const absorb=Math.min(inv.armor,damage); inv.armor-=absorb; damage-=absorb; } health=Math.max(0,health - Math.max(0,damage)); }
      const ns={...base, grid, inventory:inv, health, rankOverrides, turn:base.turn+1, selectedA:null, selectedB:null}; ns.grid=pathReveal(ns);
      if(ns.health<=0){ const newHigh=Math.max(ns.highScore||0, ns.score||0); const fresh=makeLevel(specs[level], level); fresh.highScore=newHigh; setState(fresh); return; }
      setState(ns);
    }

    // --------------- Input handlers ---------------
    function handleTileClick(x,y){
      const ns={...state};
      if(ns.mode==="usePower"){
        const ok = applyPowerUpOne(ns, ns.powerType, x, y);
        if(ok){ ns.mode="normal"; ns.powerType=null; ns.powerFirst=null; ns.selectedA=null; ns.selectedB=null; endTurnFrom(ns); }
        else setState(ns);
        return;
      }
      if(ns.mode==="usePowerPick2"){
        if(!ns.powerFirst){ ns.powerFirst=[x,y]; setState(ns); return; }
        const ok = applyPowerUpTwo(ns, ns.powerType, ns.powerFirst, [x,y]);
        ns.mode="normal"; ns.powerType=null; ns.powerFirst=null;
        if(ok){ ns.selectedA=null; ns.selectedB=null; endTurnFrom(ns); }
        else setState(ns);
        return;
      }
      if(!ns.selectedA){ ns.selectedA=[x,y]; ns.selectedB=null; }
      else if(!ns.selectedB){ ns.selectedB=[x,y]; }
      else { ns.selectedA=[x,y]; ns.selectedB=null; }
      setState(ns);
    }

    function handlePowerUpClick(type){
      if(!(state.inventory[type]>0)) return;
      if(type==="swapTwo"){
        setState({...state, mode:"usePowerPick2", powerType:type, powerFirst:null, message:"Pick two monsters to swap."});
      } else if (type==="teleport"){
        setState({...state, mode:"usePower", powerType:type, message:"Teleport: click an empty tile."});
      } else if (type==="freeze"){
        setState({...state, mode:"usePower", powerType:type, message:"Freeze: click a center tile (3x3)"});
      } else if (type==="revealRandom" || type==="gainMana3"){
        const ns={...state};
        if(type==="gainMana3"){
          ns.mana += 3;
          ns.inventory.gainMana3--;
          endTurnFrom(ns);
          return;
        }
        if(type==="revealRandom"){
          const hidden=[];
          for(let y=0;y<ns.rows;y++) for(let x=0;x<ns.cols;x++){
            const m=ns.grid[y][x]; if(m && !m.revealed) hidden.push([x,y]);
          }
          if(hidden.length){
            const [rx,ry]=hidden[(Math.random()*hidden.length)|0];
            ns.grid=deepCloneGrid(ns.grid);
            ns.grid[ry][rx].revealed=true;
            ns.grid=pathReveal(ns);
          }
          ns.inventory.revealRandom--;
          endTurnFrom(ns);
          return;
        }
      } else {
        setState({...state, mode:"usePower", powerType:type, message:"Click a monster to use."});
      }
    }

    function performSmartAction(){
      if(doEat()) return; if(doSwap()) return; if(moveMonster()) return;
      mergeState({message:"Select two tiles: eat > swap > move."});
    }

    // --------------- Render ---------------
    function render(){
      recomputeTile();
      const app = document.getElementById('app');
      app.innerHTML = '';
      const title = h('h1',{},'SevenAteNine');
      const topl = h('div',{class:'statline'},`Level ${level+1}/${levelSpecs().length} · Health `,b(state.health),' · Armor ',b(state.inventory.armor||0),' · Turn ',b(state.turn));
      const top2 = h('div',{class:'statline'},'Score ',b(state.score||0),' · High Score ',b(state.highScore||0),' · Best Chain (level) ',b(state.bestChainThisLevel||0),' · Eaten (level) ',b(state.totalEatenThisLevel||0));
      app.appendChild(title); app.appendChild(topl); app.appendChild(top2);

      const gridWrap = h('div',{class:'grid-wrap'});
      const grid = h('div',{class:'grid'});
      grid.style.gridTemplateColumns = `repeat(${state.cols}, ${tile}px)`;
      grid.style.gridTemplateRows = `repeat(${state.rows}, ${tile}px)`;
      grid.style.gap = GAP+'px';

      for(let y=0;y<state.rows;y++) for(let x=0;x<state.cols;x++){
        const k=`${x},${y}`;
        const isExit=(x===state.exit[0]&&y===state.exit[1]);
        const isWizard=(x===state.wizard[0]&&y===state.wizard[1]);
        const m=state.grid[y][x];
        const rock=state.obstacles[k];
        const hidden=m? !m.revealed:false;
        const eff = m ? ((state.rankOverrides && state.rankOverrides[k]!=null) ? state.rankOverrides[k] : m.rank) : null;
        const borderDanger = m && (m.mtype===MType.GUARD || m.mtype===MType.BRUTE || m.mtype===MType.HUNGRY || m.mtype===MType.STALWART);
        const selA = state.selectedA && state.selectedA[0]===x && state.selectedA[1]===y;
        const selB = state.selectedB && state.selectedB[0]===x && state.selectedB[1]===y;

        const tileEl = h('div',{class:`tile ${isExit?'exit':''} ${borderDanger?'danger':''} ${selA?'selA':''} ${selB?'selB':''}`});
        tileEl.dataset.x = x; tileEl.dataset.y = y;
        if(rock){ tileEl.appendChild(h('div',{class:'rock'},'🪨')); }
        if(m){
          const mon = h('div',{class:`monster ${mtypeColorClass(m.mtype)}`});
          const label = h('div',{}, hidden? '?' : String(m.rank));
          if(!hidden && eff!==m.rank){ label.appendChild(h('span',{style:'font-size:10px; opacity:.8; margin-left:2px;'},`(${eff})`)); }
          mon.appendChild(label);
          if(m.frozen>0) mon.appendChild(h('div',{class:'frozen'},'❄'));
          tileEl.appendChild(mon);
        }
        if(state.loot[k]){
          const sym = state.loot[k]==='armor'?'🛡': state.loot[k]==='scroll'?'📜': state.loot[k]==='freeze'?'❄': state.loot[k]==='teleport'?'✦': state.loot[k]==='gem'?'💎':'★';
          tileEl.appendChild(h('div',{class:'loot'}, sym));
        }
        if(isWizard) tileEl.appendChild(h('div',{class:'wizard'}));
        grid.appendChild(tileEl);
      }
      grid.addEventListener('click', (e)=>{
        const el = e.target.closest('.tile'); if(!el) return;
        const x = +el.dataset.x, y=+el.dataset.y;
        handleTileClick(x,y);
      });
      gridWrap.appendChild(grid);
      app.appendChild(gridWrap);

      // Buttons row
      const btnRow = h('div',{class:'btnrow', style:`max-width:${gridW}px;`},
        button('Eat (Space)','green', doEat),
        button('Swap (S)','amber', doSwap),
        button('Move (M)','cyan', moveMonster),
        button('Finish (N)','slate', ()=>{ if(atExit()) finishLevel(state); else mergeState({message:'Reach the exit first.'}); }),
        button('Restart (R)','slate', restart),
      );
      app.appendChild(wrapCentered(btnRow));

      // Mana icons row
      const manaRow = h('div',{class:'mana', style:`max-width:${gridW}px;`}, ...renderManaIcons(state.mana));
      app.appendChild(wrapCentered(manaRow));

      // Floating mobile buttons
      app.appendChild(h('div',{class:'floating-right'}, h('button',{class:'thumbMain', onclick:performSmartAction, 'aria-label':'Action (Space)'}, 'Action')));
      app.appendChild(h('div',{class:'floating-left'}, h('button',{class:'thumbAlt', onclick:moveMonster, 'aria-label':'Move'}, 'Move')));

      // Inventory panel
      const invPanel = h('div',{class:'panel'});
      invPanel.appendChild(h('div',{class:'statline', style:'text-align:left; margin-bottom:6px; opacity:.8;'}, 'Inventory (click to use)'));
      const invBar = h('div',{class:'inv'});
      for(const p of invItems){
        const have = state.inventory[p.key]||0;
        const btn = h('button',{title:`${p.tip} (have: ${have})`, disabled: !have, onclick: ()=>handlePowerUpClick(p.key)},
          h('span',{},p.label), h('span', {style:'font-size:12px;'}, 'x'+have)
        );
        invBar.appendChild(btn);
      }
      invPanel.appendChild(invBar);
      if(state.mode!=="normal"){
        invPanel.appendChild(h('div',{class:'statline', style:'margin-top:6px;'}, `Targeting: ${state.powerType} — click ${state.mode==="usePowerPick2" && !state.powerFirst ? "first" : state.mode==="usePowerPick2" && state.powerFirst ? "second" : "a"} tile…`));
      }
      app.appendChild(wrapCentered(invPanel));

      // Message
      app.appendChild(wrapCentered(h('div',{class:'hud-msg', style:`max-width:${gridW}px;`}, state.message||'')));

      // Footer
      app.appendChild(wrapCentered(h('div',{class:'footer', style:`max-width:${gridW}px;`},
        'Space: context action (Eat > Swap > Move). Brutes require equal/+1. Finish on the exit to bank ', b('mana'),' + ', b('best chain'),' + ', b('eaten'),' this level. Monsters never share a tile with the wizard.'
      )));
    }

    // --- Small DOM helpers ---
    function h(tag, props, ...children){
      const el = document.createElement(tag);
      if(props){
        for(const [k,v] of Object.entries(props)){
          if(k==='class') el.className = v;
          else if(k==='style') el.setAttribute('style', v);
          else if(k.startsWith('on')) el[k] = v;
          else el.setAttribute(k, v);
        }
      }
      for(const c of children){ el.appendChild(typeof c==='string' ? document.createTextNode(c) : c); }
      return el;
    }
    const b = (txt)=> h('b',{}, String(txt));
    const button = (label, kind, onclick)=> h('button',{class:'btn '+kind, onclick}, label);
    const wrapCentered = (el)=>{ const w = h('div',{style:'width:100%; display:flex; justify-content:center; padding: 0 8px;'}); w.appendChild(el); return w; };
    function renderManaIcons(m){
      const tens=Math.floor(m/10), rem=m%10, fives=Math.floor(rem/5), ones=rem%5;
      const out=[];
      for(let i=0;i<tens;i++) out.push(h('span',{class:'chip'},'10'));
      for(let i=0;i<fives;i++) out.push(h('span',{class:'chip'},'5'));
      for(let i=0;i<ones;i++) out.push(h('span',{class:'chip'},'•'));
      return out;
    }

    // --------------- Keyboard ---------------
    window.addEventListener('keydown', (e)=>{
      const k=e.key; const tag = (document.activeElement && document.activeElement.tagName) || '';
      if(tag==='INPUT'||tag==='TEXTAREA') return;
      if(k===' '){ e.preventDefault(); performSmartAction(); }
      else if(k.toLowerCase()==='s') doSwap();
      else if(k.toLowerCase()==='m') moveMonster();
      else if(k==='ArrowUp') moveWizard(0,-1);
      else if(k==='ArrowDown') moveWizard(0,1);
      else if(k==='ArrowLeft') moveWizard(-1,0);
      else if(k==='ArrowRight') moveWizard(1,0);
      else if(k.toLowerCase()==='r') restart();
    });

    // --------------- Initialize ---------------
    window.addEventListener('resize', recomputeTile);
    window.addEventListener('orientationchange', recomputeTile);
    recomputeTile();
    render();

    // ------------------ Sanity tests (console) ------------------
    (function __hw_dev_tests(){
      try {
        function testGroupMana(m){ const tens=Math.floor(m/10); const rem=m%10; const fives=Math.floor(rem/5); const ones=rem%5; return {tens,fives,ones}; }
        let g;
        g=testGroupMana(0); console.assert(g.tens===0&&g.fives===0&&g.ones===0,'groupMana(0)');
        g=testGroupMana(5); console.assert(g.tens===0&&g.fives===1&&g.ones===0,'groupMana(5)');
        g=testGroupMana(7); console.assert(g.fives===1&&g.ones===2,'groupMana(7)');
        g=testGroupMana(12); console.assert(g.tens===1&&g.fives===0&&g.ones===2,'groupMana(12)');
        g=testGroupMana(19); console.assert(g.tens===1&&g.fives===1&&g.ones===4,'groupMana(19)');
        g=testGroupMana(27); console.assert(g.tens===2&&g.fives===1&&g.ones===2,'groupMana(27)');
        const base = {cols:2, rows:1, rankOverrides:{}, grid:[[ {rank:5,mtype:MType.NORMAL, revealed:true, frozen:0}, {rank:5,mtype:MType.NORMAL, revealed:true, frozen:0} ]]};
        console.assert(hasAnyEatablePair(base), 'Normals equal should be eatable');
        const bruteAdj = {cols:2, rows:1, rankOverrides:{}, grid:[[ {rank:5,mtype:MType.NORMAL, revealed:true, frozen:0}, {rank:6,mtype:MType.BRUTE, revealed:true, frozen:0} ]]};
        console.assert(hasAnyEatablePair(bruteAdj), 'Brute (+1) adjacency should be eatable in some direction');
        const notAdj = {cols:3, rows:1, rankOverrides:{}, grid:[[ {rank:5,mtype:MType.NORMAL, revealed:true, frozen:0}, null, {rank:5,mtype:MType.NORMAL, revealed:true, frozen:0} ]]};
        console.assert(!hasAnyEatablePair(notAdj), 'Non-adjacent equal ranks should not be considered');
        const hiddenBlock = {cols:2, rows:1, rankOverrides:{}, grid:[[ {rank:5,mtype:MType.NORMAL, revealed:false, frozen:0}, {rank:5,mtype:MType.NORMAL, revealed:true, frozen:0} ]]};
        console.assert(!hasAnyEatablePair(hiddenBlock), 'Hidden monsters should block eatability');
        const bruteTooHigh = {cols:2, rows:1, rankOverrides:{}, grid:[[ {rank:5,mtype:MType.NORMAL, revealed:true, frozen:0}, {rank:7,mtype:MType.BRUTE, revealed:true, frozen:0} ]]};
        console.assert(!hasAnyEatablePair(bruteTooHigh), 'Brute +2 should not be eatable');
      } catch(e){ console.warn('HW dev tests skipped:', e); }
    })();
  })();
  </script>
</body>
</html>
