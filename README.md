<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, viewport-fit=cover">
<meta name="theme-color" content="#efe3d0">
<title>Vocabulary Trainer for Dushi</title>
<style>
:root{
  --bg:#efe3d0;
  --bg2:#e5d6bc;
  --panel:#f9f1e2;
  --line:#d2bd9c;
  --ink:#3a2e23;
  --dim:#8a7862;
  --tile:#fffaf0;
  --tileink:#33291f;
  --accent:#4b3a2b;
  --gold:#c9962f;
  --ok:#4e8c5b;
  --bad:#b4523f;
  --shadow:rgba(96,72,44,.20);
  --r:16px;
  --tileH:66px;
  --font:system-ui,-apple-system,"Segoe UI Variable Text","Segoe UI",Roboto,"Helvetica Neue",Arial,sans-serif;
}
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{height:100%;margin:0;overflow:hidden;background:var(--bg);}
body{
  font-family:var(--font);color:var(--ink);
  background:radial-gradient(130% 85% at 50% -15%,#faf1e2 0%,var(--bg) 62%);
  overscroll-behavior:none;
}
button{font:inherit;color:inherit;border:0;background:none;cursor:pointer}
#app{
  position:relative;height:100dvh;max-width:540px;margin:0 auto;
  padding:env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
}
@media (min-width:600px) and (min-height:640px){
  #app{height:min(100dvh,860px);margin-top:calc((100dvh - min(100dvh,860px))/2);
       border:1px solid var(--line);border-radius:26px;overflow:hidden;
       box-shadow:0 24px 60px rgba(96,72,44,.28)}
  :root{--tileH:74px}
}
.screen{position:absolute;inset:0;display:none;flex-direction:column;gap:14px;padding:18px}
.screen.on{display:flex}
.scroll{overflow-y:auto;scrollbar-width:none;-ms-overflow-style:none;min-height:0}
.scroll::-webkit-scrollbar{display:none}
.screen.scroll>*{flex:0 0 auto}

/* ---------- typography ---------- */
h1{font-size:clamp(24px,7.2vw,32px);line-height:1.08;letter-spacing:-.02em;font-weight:800;margin:0;text-wrap:balance}
h2{font-size:20px;font-weight:700;letter-spacing:-.01em;margin:0}
.sub{color:var(--dim);font-size:14px;margin:0}
.label{font-size:13px;color:var(--dim);font-weight:600;margin:0 0 6px 2px}

/* ---------- buttons ---------- */
.btn{
  display:flex;align-items:center;justify-content:center;gap:8px;
  width:100%;min-height:56px;padding:14px 16px;border-radius:var(--r);
  background:var(--panel);border:1px solid var(--line);
  font-weight:650;font-size:17px;transition:transform .08s ease,background .15s ease;
}
.btn:active{transform:scale(.985)}
.btn.primary{background:var(--accent);color:#fdf6ea;border-color:transparent;font-weight:750}
.btn.ghost{background:transparent}
.btn:disabled{opacity:.38;cursor:default}
.btn.small{min-height:44px;font-size:15px;padding:10px 14px;width:auto}
.row{display:flex;gap:10px}
.row>*{flex:1}
.seg{display:flex;gap:8px}
.seg button{
  flex:1;min-height:50px;border-radius:13px;background:var(--bg2);border:1.5px solid var(--line);
  font-weight:650;font-size:15px;transition:.15s
}
.seg button[aria-pressed="true"]{background:var(--ink);color:#fbf4e9;border-color:var(--ink)}
.seg button:disabled{opacity:.3}
:focus-visible{outline:3px solid var(--accent);outline-offset:2px}

/* ---------- start screen ---------- */
.hearts{display:flex;justify-content:center;gap:14px;margin:2px 0 4px}
.heart{width:62px;height:62px;filter:drop-shadow(0 6px 12px rgba(96,72,44,.30));animation:breathe 3s ease-in-out infinite}
.heart:nth-child(2){animation-delay:-1s}
.heart:nth-child(3){animation-delay:-2s}
@keyframes breathe{0%,100%{transform:translateY(0) scale(1)}50%{transform:translateY(-4px) scale(1.06)}}
@media (prefers-reduced-motion:reduce){.heart{animation:none}}
.head{text-align:center;display:flex;flex-direction:column;gap:8px;align-items:center}
.topline{display:flex;align-items:center;justify-content:space-between;gap:10px}
.chip{
  display:inline-flex;align-items:center;gap:7px;padding:7px 12px;border-radius:999px;
  background:var(--bg2);border:1px solid var(--line);font-size:13px;font-weight:650;color:var(--dim)
}
.chip b{color:var(--ink);font-weight:750}
.icobtn{width:44px;height:44px;border-radius:12px;background:var(--bg2);border:1px solid var(--line);
  display:grid;place-items:center;flex:0 0 auto}
.stack{display:flex;flex-direction:column;gap:10px}

/* ---------- level grid ---------- */
.grid{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.lvl{
  border-radius:var(--r);background:var(--panel);border:1px solid var(--line);
  padding:11px;display:flex;flex-direction:column;gap:5px;align-items:flex-start;min-height:84px
}
.lvl .n{font-size:19px;font-weight:750}
.lvl .meta{font-size:12px;color:var(--dim)}
.lvl[disabled]{opacity:.4}
.stars{display:flex;gap:3px}
.star{width:17px;height:17px}
.star.off{opacity:.4}

/* ---------- game ---------- */
#scr-game{padding:12px 12px 14px;gap:10px}
.hud{display:flex;align-items:center;gap:8px}
.hud .pill{background:var(--bg2);border:1px solid var(--line);border-radius:999px;padding:6px 11px;
  font-size:13px;font-weight:700;letter-spacing:.01em;white-space:nowrap}
.lives{display:flex;gap:5px;align-items:center}
.pip{width:11px;height:11px;border-radius:50%;background:var(--bad);box-shadow:0 0 0 1px rgba(0,0,0,.25) inset}
.pip.gone{background:transparent;box-shadow:0 0 0 1.5px var(--line) inset}
.board{
  position:relative;flex:1;min-height:0;display:flex;gap:10px;overflow:hidden;
  border-radius:18px;background:var(--bg2);box-shadow:inset 0 2px 8px rgba(96,72,44,.14);
  border:1px solid var(--line);padding:8px
}
.col{position:relative;flex:1;min-width:0}
.tile{
  position:absolute;left:0;right:0;top:0;height:var(--tileH);
  display:grid;place-items:center;text-align:center;padding:6px 8px;
  border-radius:14px;background:var(--tile);color:var(--tileink);
  font-weight:700;font-size:clamp(14px,4.3vw,20px);line-height:1.15;
  box-shadow:0 3px 0 var(--shadow),0 1px 0 rgba(255,255,255,.7) inset;border:1px solid #e2d2b8;will-change:transform;word-break:break-word;hyphens:auto
}
.tile{overflow:hidden}
.cols3 .tile{font-size:clamp(11px,3.1vw,16px);line-height:1.1}
.tile.hit{background:var(--ok);color:#f4faf2;border-color:var(--ok);animation:pop .32s ease}
.tile.solution{outline:3px solid var(--ok);outline-offset:-3px}
.tile.miss{background:var(--bad);color:#fff2ee;border-color:var(--bad);animation:shake .32s ease}
.tile.dim{opacity:.35}
@keyframes pop{0%{transform:var(--ty) scale(1)}45%{transform:var(--ty) scale(1.09)}100%{transform:var(--ty) scale(1)}}
@keyframes shake{0%,100%{margin-left:0}25%{margin-left:-7px}75%{margin-left:7px}}
.floor{position:absolute;left:8px;right:8px;bottom:6px;height:3px;border-radius:2px;background:var(--line)}
.flash{position:absolute;inset:0;border-radius:18px;pointer-events:none;opacity:0;transition:opacity .18s}
.flash.ok{background:rgba(78,140,91,.22);opacity:1}
.flash.bad{background:rgba(180,82,63,.20);opacity:1}
.prompt{
  display:flex;align-items:center;justify-content:center;gap:12px;
  min-height:96px;padding:12px 16px;border-radius:18px;
  background:var(--panel);border:1px solid var(--line);text-align:center;cursor:pointer
}
.prompt .w{font-size:clamp(24px,7.4vw,34px);font-weight:800;letter-spacing:-.02em;line-height:1.1}
.prompt .hint{font-size:12px;color:var(--dim);font-weight:600;margin-top:4px}
.prompt:active{background:var(--bg2)}
.overlay{
  position:absolute;inset:0;background:rgba(243,233,216,.94);backdrop-filter:blur(3px);
  display:none;flex-direction:column;justify-content:center;gap:14px;padding:26px;z-index:5;text-align:center
}
.overlay.on{display:flex}
.count{font-size:64px;font-weight:800;letter-spacing:-.03em}
.result-line{display:flex;justify-content:space-between;align-items:center;gap:10px;
  padding:12px 14px;border-radius:13px;background:var(--panel);border:1px solid var(--line);font-size:15px}
.result-line b{font-weight:750}
.big{font-size:clamp(34px,11vw,52px);font-weight:800;letter-spacing:-.03em}
.badge{align-self:center;padding:6px 12px;border-radius:999px;background:var(--gold);color:#2b1e06;font-weight:750;font-size:13px}
.center{display:flex;flex-direction:column;justify-content:center;flex:1;gap:14px;min-height:0}
.dlg{position:absolute;inset:0;display:none;place-items:center;padding:22px;background:rgba(74,58,43,.45);z-index:9}
.dlg.on{display:grid}
.dlg .box{background:var(--panel);box-shadow:0 20px 40px rgba(74,58,43,.3);border:1px solid var(--line);border-radius:20px;padding:20px;display:flex;flex-direction:column;gap:14px;max-width:340px}
.spacer{flex:1;min-height:0}
</style>
</head>
<body>
<div id="app">

  <!-- ============ menu language (first run) ============ -->
  <section id="scr-lang" class="screen">
    <div class="center">
      <div class="hearts" id="hearts-a"></div>
      <h1 style="text-align:center">Vocabulary Trainer for Dushi</h1>
      <p class="sub" style="text-align:center">Menüsprache · menu language · idioma del menú</p>
      <div class="stack">
        <button class="btn" data-ml="de">Deutsch</button>
        <button class="btn" data-ml="en">English</button>
        <button class="btn" data-ml="es">Español</button>
      </div>
    </div>
  </section>

  <!-- ============ home ============ -->
  <section id="scr-home" class="screen scroll">
    <div class="head">
      <h1>Vocabulary Trainer for Dushi</h1>
      <div class="hearts" id="hearts-b"></div>
    </div>

    <div>
      <p class="label" id="lb-speak"></p>
      <div class="seg" id="seg-from"></div>
    </div>
    <div>
      <p class="label" id="lb-learn"></p>
      <div class="seg" id="seg-to"></div>
    </div>

    <div class="stack">
      <button class="btn primary" id="b-play"></button>
      <div class="row">
        <button class="btn" id="b-levels"></button>
        <button class="btn" id="b-endless"></button>
      </div>
      <p class="sub" id="endless-hint" style="text-align:center;margin:-4px 0 0"></p>
    </div>

    <div class="topline">
      <span class="chip" id="chip-diff"></span>
      <div class="row" style="flex:0 0 auto;gap:8px">
        <button class="icobtn" id="b-sound" title="Sound"></button>
        <button class="btn small" id="b-settings"></button>
      </div>
    </div>
  </section>

  <!-- ============ level select ============ -->
  <section id="scr-levels" class="screen">
    <div class="topline">
      <h2 id="lv-title"></h2>
      <button class="btn small" data-back="home"></button>
    </div>
    <div class="grid scroll" id="lv-grid"></div>
  </section>

  <!-- ============ settings ============ -->
  <section id="scr-settings" class="screen">
    <div class="topline">
      <h2 id="st-title"></h2>
      <button class="btn small" data-back="home"></button>
    </div>
    <div class="scroll stack" style="gap:18px;padding-bottom:8px">
      <div>
        <p class="label" id="st-lang"></p>
        <div class="seg" id="seg-ml"></div>
      </div>
      <div>
        <p class="label" id="st-diff"></p>
        <div class="seg" id="seg-diff"></div>
        <p class="sub" id="st-fall" style="margin-top:8px"></p>
      </div>
      <div>
        <p class="label" id="st-audio"></p>
        <div class="stack">
          <button class="btn" id="t-sound"></button>
          <button class="btn" id="t-music"></button>
          <button class="btn" id="t-speech"></button>
        </div>
        <p class="sub" id="st-voice" style="margin:8px 0 0"></p>
      </div>
      <button class="btn ghost" id="b-reset" style="color:var(--bad);border-color:var(--bad)"></button>
    </div>
  </section>

  <!-- ============ game ============ -->
  <section id="scr-game" class="screen">
    <div class="hud">
      <span class="pill" id="g-level"></span>
      <span class="pill" id="g-prog"></span>
      <div class="lives" id="g-lives"></div>
      <div class="spacer"></div>
      <span class="pill" id="g-score">0</span>
      <button class="icobtn" id="b-pause"></button>
    </div>
    <div class="board" id="board">
      <div class="floor"></div>
      <div class="flash" id="flash"></div>
    </div>
    <div class="prompt" id="prompt">
      <div>
        <div class="w" id="p-word"></div>
        <div class="hint" id="p-hint"></div>
      </div>
      <span id="p-spk" aria-hidden="true"></span>
    </div>

    <div class="overlay" id="ov-count"><div class="count" id="count-n"></div><div class="sub" id="count-lv"></div></div>
    <div class="overlay" id="ov-pause">
      <h2 id="pz-title" style="text-align:center"></h2>
      <div class="stack">
        <button class="btn primary" id="pz-resume"></button>
        <button class="btn" id="pz-restart"></button>
        <button class="btn ghost" id="pz-home"></button>
      </div>
    </div>
  </section>

  <!-- ============ result ============ -->
  <section id="scr-result" class="screen">
    <div class="center scroll">
      <h2 id="r-title" style="text-align:center"></h2>
      <div class="stars" id="r-stars" style="justify-content:center;transform:scale(1.7);margin:10px 0 18px"></div>
      <div class="big" id="r-total" style="text-align:center"></div>
      <div id="r-badge"></div>
      <div class="stack" style="margin-top:6px">
        <div class="result-line"><span id="r-calc-l"></span><b id="r-calc"></b></div>
        <div class="result-line"><span id="r-best-l"></span><b id="r-best"></b></div>
      </div>
    </div>
    <div class="stack">
      <button class="btn primary" id="r-next"></button>
      <div class="row">
        <button class="btn" id="r-retry"></button>
        <button class="btn ghost" id="r-home"></button>
      </div>
    </div>
  </section>

  <!-- confirm dialog -->
  <div class="dlg" id="dlg">
    <div class="box">
      <p id="dlg-text" style="margin:0;font-size:16px;line-height:1.4"></p>
      <div class="row">
        <button class="btn" id="dlg-no"></button>
        <button class="btn primary" id="dlg-yes" style="background:var(--bad);color:#fff2ee"></button>
      </div>
    </div>
  </div>
</div>

<script>
/* ==================================================================
   Vocabulary Trainer for Dushi — single file, offline, no libraries
   ================================================================== */
const W=[["the","el, la","der, die, das"],["a, an","un, una","ein, eine"],["I","yo","ich"],["you","tú, usted","du, Sie"],["he","él","er"],["she","ella","sie"],["it","ello, lo","es"],["we","nosotros","wir"],["they","ellos, ellas","sie"],["me","me, mí","mich, mir"],["him","lo, le","ihn, ihm"],["her","la, le","sie, ihr"],["us","nos","uns"],["them","los, les","sie, ihnen"],["my","mi","mein"],["your","tu, su","dein, Ihr"],["his","su","sein"],["her","su","ihr"],["our","nuestro","unser"],["their","su","ihr"],["this","este, esta","dieser, diese, dieses"],["that","ese, aquel","jener, das"],["these","estos, estas","diese"],["those","esos, aquellos","jene"],["who","quién","wer"],["what","qué","was"],["which","cuál","welcher"],["where","dónde","wo"],["when","cuándo","wann"],["why","por qué","warum"],["how","cómo","wie"],["whose","de quién","wessen"],["someone","alguien","jemand"],["something","algo","etwas"],["nothing","nada","nichts"],["nobody","nadie","niemand"],["everyone","todos","jeder, alle"],["everything","todo","alles"],["here","aquí","hier"],["there","allí, ahí","dort, da"],["in","en","in"],["on","en, sobre","auf"],["at","en, a","an, bei"],["to","a","zu, nach"],["from","de, desde","von, aus"],["of","de","von"],["with","con","mit"],["without","sin","ohne"],["for","para, por","für"],["by","por","von, durch"],["about","sobre, acerca de","über"],["over","encima de","über"],["under","debajo de","unter"],["between","entre","zwischen"],["through","a través de","durch"],["into","en, dentro de","in (hinein)"],["after","después de","nach"],["before","antes de","vor"],["during","durante","während"],["until","hasta","bis"],["since","desde","seit"],["against","contra","gegen"],["near","cerca de","nahe, bei"],["behind","detrás de","hinter"],["next to","al lado de","neben"],["and","y","und"],["or","o","oder"],["but","pero","aber"],["because","porque","weil"],["if","si","wenn, falls"],["that","que","dass"],["than","que","als"],["so","así que, tan","so, also"],["while","mientras","während"],["although","aunque","obwohl"],["be","ser, estar","sein"],["have","tener","haben"],["do","hacer","tun, machen"],["can","poder","können"],["will","ir a (Futur)","werden"],["would","-ría (Konditional)","würde"],["should","debería","sollte"],["must","deber, tener que","müssen"],["may","poder","dürfen"],["go","ir","gehen"],["come","venir","kommen"],["get","conseguir, obtener","bekommen"],["make","hacer","machen"],["say","decir","sagen"],["tell","contar","erzählen"],["ask","preguntar","fragen"],["answer","responder","antworten"],["know","saber, conocer","wissen, kennen"],["think","pensar","denken"],["believe","creer","glauben"],["want","querer","wollen"],["need","necesitar","brauchen"],["like","gustar","mögen"],["love","amar, querer","lieben"],["hate","odiar","hassen"],["see","ver","sehen"],["look","mirar","schauen"],["watch","ver, observar","anschauen"],["hear","oír","hören"],["listen","escuchar","zuhören"],["speak","hablar","sprechen"],["talk","charlar","reden"],["read","leer","lesen"],["write","escribir","schreiben"],["learn","aprender","lernen"],["understand","entender","verstehen"],["teach","enseñar","unterrichten"],["work","trabajar","arbeiten"],["play","jugar","spielen"],["eat","comer","essen"],["drink","beber","trinken"],["sleep","dormir","schlafen"],["live","vivir","leben"],["die","morir","sterben"],["give","dar","geben"],["take","tomar, coger","nehmen"],["put","poner","setzen, legen"],["find","encontrar","finden"],["lose","perder","verlieren"],["keep","mantener","behalten"],["leave","dejar, salir","verlassen"],["stay","quedarse","bleiben"],["bring","traer","bringen"],["send","enviar","schicken"],["buy","comprar","kaufen"],["sell","vender","verkaufen"],["pay","pagar","bezahlen"],["cost","costar","kosten"],["open","abrir","öffnen"],["close","cerrar","schließen"],["start","empezar","anfangen"],["stop","parar","aufhören"],["finish","terminar","beenden"],["help","ayudar","helfen"],["use","usar","benutzen"],["try","intentar","versuchen"],["change","cambiar","ändern"],["move","mover","bewegen"],["walk","caminar","gehen, laufen"],["run","correr","rennen"],["drive","conducir","fahren"],["fly","volar","fliegen"],["sit","sentarse","sitzen"],["stand","estar de pie","stehen"],["wait","esperar","warten"],["meet","conocer, encontrar","treffen"],["call","llamar","anrufen"],["show","mostrar","zeigen"],["feel","sentir","fühlen"],["seem","parecer","scheinen"],["become","convertirse en","werden"],["happen","pasar, ocurrir","passieren"],["remember","recordar","sich erinnern"],["forget","olvidar","vergessen"],["win","ganar","gewinnen"],["time","tiempo","Zeit"],["year","año","Jahr"],["month","mes","Monat"],["week","semana","Woche"],["day","día","Tag"],["hour","hora","Stunde"],["minute","minuto","Minute"],["morning","mañana","Morgen"],["evening","tarde, noche","Abend"],["night","noche","Nacht"],["man","hombre","Mann"],["woman","mujer","Frau"],["child","niño","Kind"],["people","gente","Leute"],["person","persona","Person"],["friend","amigo","Freund"],["family","familia","Familie"],["mother","madre","Mutter"],["father","padre","Vater"],["name","nombre","Name"],["house","casa","Haus"],["home","hogar","Zuhause"],["room","habitación","Zimmer"],["door","puerta","Tür"],["table","mesa","Tisch"],["city","ciudad","Stadt"],["country","país","Land"],["street","calle","Straße"],["place","lugar","Ort"],["world","mundo","Welt"],["school","escuela","Schule"],["student","estudiante","Schüler"],["teacher","profesor","Lehrer"],["book","libro","Buch"],["word","palabra","Wort"],["language","idioma","Sprache"],["question","pregunta","Frage"],["problem","problema","Problem"],["idea","idea","Idee"],["story","historia","Geschichte"],["work","trabajo","Arbeit"],["job","empleo","Stelle"],["company","empresa","Firma"],["money","dinero","Geld"],["price","precio","Preis"],["shop","tienda","Laden"],["food","comida","Essen"],["water","agua","Wasser"],["car","coche","Auto"],["train","tren","Zug"],["way","camino, manera","Weg"],["hand","mano","Hand"],["head","cabeza","Kopf"],["eye","ojo","Auge"],["body","cuerpo","Körper"],["life","vida","Leben"],["love","amor","Liebe"],["game","juego","Spiel"],["music","música","Musik"],["picture","imagen","Bild"],["film","película","Film"],["phone","teléfono","Handy"],["number","número","Zahl"],["part","parte","Teil"],["thing","cosa","Ding"],["kind","tipo","Art"],["end","fin","Ende"],["group","grupo","Gruppe"],["point","punto","Punkt"],["side","lado","Seite"],["sun","sol","Sonne"],["rain","lluvia","Regen"],["tree","árbol","Baum"],["animal","animal","Tier"],["dog","perro","Hund"],["good","bueno","gut"],["bad","malo","schlecht"],["big","grande","groß"],["small","pequeño","klein"],["new","nuevo","neu"],["old","viejo","alt"],["young","joven","jung"],["long","largo","lang"],["short","corto","kurz"],["high","alto","hoch"],["low","bajo","niedrig"],["easy","fácil","einfach"],["difficult","difícil","schwierig"],["important","importante","wichtig"],["different","diferente","anders"],["same","mismo","gleich"],["other","otro","andere"],["first","primero","erste"],["last","último","letzte"],["next","próximo","nächste"],["right","correcto, derecho","richtig, rechts"],["wrong","incorrecto","falsch"],["happy","feliz","glücklich"],["sad","triste","traurig"],["beautiful","hermoso, bonito","schön"],["free","libre, gratis","frei, kostenlos"],["true","verdadero","wahr"],["own","propio","eigen"],["full","lleno","voll"],["empty","vacío","leer"],["hot","caliente","heiß"],["cold","frío","kalt"],["warm","cálido","warm"],["strong","fuerte","stark"],["weak","débil","schwach"],["fast","rápido","schnell"],["slow","lento","langsam"],["early","temprano","früh"],["late","tarde","spät"],["sure","seguro","sicher"],["not","no","nicht"],["no","no","kein, nein"],["yes","sí","ja"],["very","muy","sehr"],["too","demasiado, también","zu, auch"],["also","también","auch"],["only","solo","nur"],["more","más","mehr"],["most","la mayoría","die meisten"],["less","menos","weniger"],["much","mucho","viel"],["many","muchos","viele"],["few","pocos","wenige"],["all","todo","alle"],["some","algunos","einige"],["any","cualquier","irgendein"],["every","cada","jeder"],["again","otra vez","wieder"],["always","siempre","immer"],["never","nunca","nie"],["often","a menudo","oft"],["sometimes","a veces","manchmal"],["now","ahora","jetzt"],["today","hoy","heute"],["tomorrow","mañana","morgen"]];
const LI = {en:0, es:1, de:2};
const LEVELS = 20;          // Anzahl Level
const PER = 15;             // Vokabeln pro Level
const TILES3 = 9;           // ab diesem Level fallen drei Kacheln

/* ---------------- i18n ---------------- */
const T = {
 de:{speak:"Angesagte Sprache",learn:"Fallende Vokabeln",play:"Weiterspielen",levels:"Level auswählen",
  endless:"Endlos-Modus",settings:"Einstellungen",back:"Zurück",home:"Startseite",retry:"Nochmal",
  next:"Nächstes Level",resume:"Weiter",paused:"Pause",difficulty:"Schwierigkeit",
  easy:"Leicht",med:"Mittel",hard:"Schwierig",menuLang:"Menüsprache",audio:"Ton & Aussprache",
  sound:"Töne",music:"Menü-Musik",speech:"Sprachausgabe",on:"an",off:"aus",fall:"Fallzeit in Level 1",
  reset:"Fortschritt zurücksetzen",resetAsk:"Alle Level, Sterne und Rekorde löschen?",
  cancel:"Abbrechen",del:"Löschen",level:"Level",locked:"Ab Level 20",record:"Rekord",
  none:"—",levelDone:"Level geschafft!",gameOver:"Keine Leben mehr",endlessOver:"Lauf beendet",
  review:"Fehlerrunde",reviewHint:"Noch einmal — ohne Lebensverlust",
  calc:"Punkte × Faktor",best:"Rekord",newBest:"Neuer Rekord!",words:"Wörter",
  tapHear:"Antippen zum Anhören",sec:"Sek.",go:"Los!",
  voice:"Stimme",noVoice:"keine Stimme installiert – Ausgabe bleibt stumm"},
 en:{speak:"Spoken language",learn:"Falling words",play:"Continue",levels:"Choose level",
  endless:"Endless mode",settings:"Settings",back:"Back",home:"Home",retry:"Try again",
  next:"Next level",resume:"Resume",paused:"Paused",difficulty:"Difficulty",
  easy:"Easy",med:"Medium",hard:"Hard",menuLang:"Menu language",audio:"Sound & speech",
  sound:"Sounds",music:"Menu music",speech:"Speech",on:"on",off:"off",fall:"Fall time in level 1",
  reset:"Reset progress",resetAsk:"Delete all levels, stars and records?",
  cancel:"Cancel",del:"Delete",level:"Level",locked:"After level 20",record:"Best",
  none:"—",levelDone:"Level cleared!",gameOver:"Out of lives",endlessOver:"Run over",
  review:"Review round",reviewHint:"Once more — no lives lost",
  calc:"Points × factor",best:"Best",newBest:"New record!",words:"words",
  tapHear:"Tap to hear it",sec:"sec",go:"Go!",
  voice:"Voice",noVoice:"no voice installed – stays silent"},
 es:{speak:"Idioma hablado",learn:"Palabras que caen",play:"Continuar",levels:"Elegir nivel",
  endless:"Modo infinito",settings:"Ajustes",back:"Atrás",home:"Inicio",retry:"Otra vez",
  next:"Siguiente nivel",resume:"Seguir",paused:"Pausa",difficulty:"Dificultad",
  easy:"Fácil",med:"Media",hard:"Difícil",menuLang:"Idioma del menú",audio:"Sonido y voz",
  sound:"Sonidos",music:"Música del menú",speech:"Voz",on:"sí",off:"no",fall:"Tiempo de caída en el nivel 1",
  reset:"Borrar progreso",resetAsk:"¿Borrar todos los niveles, estrellas y récords?",
  cancel:"Cancelar",del:"Borrar",level:"Nivel",locked:"Tras el nivel 20",record:"Récord",
  none:"—",levelDone:"¡Nivel superado!",gameOver:"Sin vidas",endlessOver:"Partida terminada",
  review:"Ronda de repaso",reviewHint:"Otra vez — sin perder vidas",
  calc:"Puntos × factor",best:"Récord",newBest:"¡Nuevo récord!",words:"palabras",
  tapHear:"Toca para escuchar",sec:"seg",go:"¡Ya!",
  voice:"Voz",noVoice:"sin voz instalada – se queda en silencio"}
};
const LANGNAME = {de:"Deutsch", en:"English", es:"Español"};
const LOCALE = {de:"de-DE", en:"en-US", es:"es-ES"};
const DIFF = {easy:{f:1.35, m:1.0}, med:{f:1.0, m:1.5}, hard:{f:0.75, m:2.0}};

/* ---------------- storage (falls back to memory) ---------------- */
const store = (()=>{
  let ok = true, mem = {};
  try{ localStorage.setItem("__t","1"); localStorage.removeItem("__t"); }catch(e){ ok = false; }
  return {
    get(k){ try{ return ok ? localStorage.getItem(k) : (mem[k] ?? null); }catch(e){ return mem[k] ?? null; } },
    set(k,v){ try{ ok ? localStorage.setItem(k,v) : (mem[k]=v); }catch(e){ mem[k]=v; } },
    del(k){ try{ ok ? localStorage.removeItem(k) : delete mem[k]; }catch(e){ delete mem[k]; } }
  };
})();
const KEY = "vtd.v2";
const DEF = {ml:null, from:"en", to:"de", diff:"med", sound:true, music:true, speech:true,
             level:1, unlocked:1, stars:{}, best:{}, ebest:0, ebestD:null};
let S = Object.assign({}, DEF);
try{ const raw = store.get(KEY); if(raw) S = Object.assign({}, DEF, JSON.parse(raw)); }catch(e){}
const save = ()=> store.set(KEY, JSON.stringify(S));
let t = T[S.ml || "de"];

/* ---------------- helpers ---------------- */
const $ = s => document.querySelector(s);
const norm = s => s.toLowerCase().replace(/[.,!?()]/g,"").trim();
const shuffle = a => { for(let i=a.length-1;i>0;i--){ const j=(Math.random()*(i+1))|0; [a[i],a[j]]=[a[j],a[i]]; } return a; };
const playLevel = () => Math.min(S.unlocked, LEVELS);
const nf = n => n.toLocaleString(LOCALE[S.ml||"de"]);
const ff = n => n.toLocaleString(LOCALE[S.ml||"de"], {minimumFractionDigits:1, maximumFractionDigits:2});

const SVG_STAR = c => `<svg class="star ${c}" viewBox="0 0 24 24" aria-hidden="true"><path fill="${c==='off'?'#bda98a':'#c9962f'}" d="M12 2.6l2.9 5.9 6.5.9-4.7 4.6 1.1 6.5L12 17.4l-5.8 3.1 1.1-6.5L2.6 9.4l6.5-.9z"/></svg>`;
const SVG_SOUND = on => on
 ? `<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="#3a2e23" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 9v6h4l5 4V5L8 9H4z"/><path d="M16.5 8.5a5 5 0 010 7"/><path d="M19 6a8.5 8.5 0 010 12"/></svg>`
 : `<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="#8a7862" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 9v6h4l5 4V5L8 9H4z"/><path d="M17 9l5 6M22 9l-5 6"/></svg>`;
const SVG_PAUSE = `<svg width="20" height="20" viewBox="0 0 24 24" fill="#3a2e23"><rect x="6" y="4" width="4" height="16" rx="1.4"/><rect x="14" y="4" width="4" height="16" rx="1.4"/></svg>`;
const SVG_SPEAK = `<svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="#8a7862" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 9v6h4l5 4V5L8 9H4z"/><path d="M16.5 8.5a5 5 0 010 7"/></svg>`;

/* ---------------- flag hearts ---------------- */
const HEART_D = "M50 88C20 66 4 49 4 32.5 4 18.5 15 8.5 28 8.5c9 0 17 5 22 12.5 5-7.5 13-12.5 22-12.5 13 0 24 10 24 24 0 16.5-16 33.5-46 55.5z";
function heartSVG(kind, uid){
  let inner = "";
  if(kind === "us"){
    for(let i=0;i<7;i++) inner += `<rect x="0" y="${i*14.3}" width="100" height="14.3" fill="${i%2?'#ffffff':'#b22234'}"/>`;
    inner += `<rect x="0" y="0" width="52" height="43" fill="#3c3b6e"/>`;
    for(let r=0;r<4;r++) for(let c=0;c<5;c++)
      inner += `<circle cx="${6+c*10}" cy="${6+r*10.5}" r="1.9" fill="#fff"/>`;
  } else if(kind === "de"){
    inner = `<rect width="100" height="33.4" fill="#111"/><rect y="33.4" width="100" height="33.3" fill="#dd0000"/><rect y="66.7" width="100" height="33.3" fill="#ffce00"/>`;
  } else {
    inner = `<rect width="100" height="50" fill="#fcd116"/><rect y="50" width="100" height="25" fill="#003893"/><rect y="75" width="100" height="25" fill="#ce1126"/>`;
  }
  return `<svg class="heart" viewBox="0 0 100 100" role="img" aria-label="${kind}">
    <defs><clipPath id="hc${uid}"><path d="${HEART_D}"/></clipPath></defs>
    <g clip-path="url(#hc${uid})">${inner}</g>
    <path d="${HEART_D}" fill="none" stroke="rgba(255,252,245,.9)" stroke-width="4" stroke-linejoin="round"/>
  </svg>`;
}
function paintHearts(){
  ["hearts-a","hearts-b"].forEach((id,k)=>{
    const el = document.getElementById(id); if(!el) return;
    el.innerHTML = heartSVG("us","u"+k) + heartSVG("de","d"+k) + heartSVG("co","c"+k);
  });
}

/* ---------------- audio ---------------- */
let AC = null;
function ac(){
  if(!AC){ try{ AC = new (window.AudioContext||window.webkitAudioContext)(); }catch(e){ AC = false; } }
  if(AC && AC.state === "suspended") AC.resume().catch(()=>{});
  return AC || null;
}
function tone(f1, f2, dur, type, vol, delay){
  if(!S.sound) return;
  const c = ac(); if(!c) return;
  try{
    const t0 = c.currentTime + (delay||0);
    const o = c.createOscillator(), g = c.createGain();
    o.type = type || "sine";
    o.frequency.setValueAtTime(f1, t0);
    if(f2) o.frequency.exponentialRampToValueAtTime(f2, t0 + dur);
    g.gain.setValueAtTime(0.0001, t0);
    g.gain.exponentialRampToValueAtTime(vol || 0.16, t0 + 0.015);
    g.gain.exponentialRampToValueAtTime(0.0001, t0 + dur);
    o.connect(g).connect(c.destination);
    o.start(t0); o.stop(t0 + dur + 0.03);
  }catch(e){}
}
const sfxOK   = ()=>{ tone(660,0,0.11,"triangle",0.16,0); tone(990,0,0.16,"triangle",0.13,0.09); };
const sfxBad  = ()=>{ tone(200,110,0.32,"sawtooth",0.13,0); };
const sfxWin  = ()=>{ [523,659,784,1046].forEach((f,i)=>tone(f,0,0.14,"triangle",0.13,i*0.11)); };
const sfxTick = ()=>{ tone(420,0,0.06,"sine",0.09,0); };

/* ---------------- Menü-Musik: Bachata-Instrumental, synthetisiert ----------------
   Nur im Menü, nie im Spiel. Requinto-Arpeggio, weicher Bass, Güira und Bongó –
   alles aus Oszillatoren und Rauschen, keine Audiodateien. */
const Music = (()=>{
  const BPM = 96, STEP = (60 / BPM) / 4;           // Sechzehntel, ruhiges Bachata-Tempo
  const CH = [                                     // Am – F – C – E
    {b:110.00, n:[220.00,261.63,329.63,440.00]},
    {b: 87.31, n:[174.61,220.00,261.63,349.23]},
    {b:130.81, n:[196.00,261.63,329.63,392.00]},
    {b: 82.41, n:[164.81,207.65,246.94,329.63]}
  ];
  const GTR  = [0,null,null,2,null,1,null,null,2,null,null,3,null,2,null,null];
  const BASS = {0:"r", 6:"f", 8:"r", 14:"a"};      // Wurzel, Quinte, Wurzel, Vorgriff

  let ctx=null, bus=null, master=null, gEnv=null, bEnv=null, bFil=null,
      srcs=[], nbuf=null, timer=0, next=0, pos=0, on=false;

  function noise(c){
    if(nbuf) return nbuf;
    const len = c.sampleRate * 2;
    nbuf = c.createBuffer(1, len, c.sampleRate);
    const d = nbuf.getChannelData(0);
    for(let i=0;i<len;i++) d[i] = Math.random() * 2 - 1;
    return nbuf;
  }
  function build(c){
    master = c.createGain(); master.gain.value = 0.0001;
    const lp = c.createBiquadFilter(); lp.type = "lowpass"; lp.frequency.value = 3200;
    bus = c.createGain(); bus.gain.value = 1;
    bus.connect(lp); lp.connect(master); master.connect(c.destination);
    const dl = c.createDelay(0.9); dl.delayTime.value = 0.375;
    const fb = c.createGain(); fb.gain.value = 0.26;
    const wet = c.createGain(); wet.gain.value = 0.13;
    bus.connect(dl); dl.connect(fb); fb.connect(dl); dl.connect(wet); wet.connect(lp);
    const g = c.createBufferSource(); g.buffer = noise(c); g.loop = true;
    const gf = c.createBiquadFilter(); gf.type = "bandpass"; gf.frequency.value = 6200; gf.Q.value = 0.8;
    gEnv = c.createGain(); gEnv.gain.value = 0;
    g.connect(gf); gf.connect(gEnv); gEnv.connect(bus); g.start();
    const b = c.createBufferSource(); b.buffer = noise(c); b.loop = true;
    bFil = c.createBiquadFilter(); bFil.type = "bandpass"; bFil.Q.value = 4;
    bEnv = c.createGain(); bEnv.gain.value = 0;
    b.connect(bFil); bFil.connect(bEnv); bEnv.connect(bus); b.start();
    srcs = [g, b];
  }
  function pluck(t, f, v, dur){
    const o1 = ctx.createOscillator(), o2 = ctx.createOscillator();
    o1.type = "triangle"; o2.type = "sawtooth";
    o1.frequency.value = f; o2.frequency.value = f * 1.004;
    const fl = ctx.createBiquadFilter(); fl.type = "lowpass";
    fl.frequency.setValueAtTime(2100, t);
    fl.frequency.exponentialRampToValueAtTime(520, t + dur);
    const g = ctx.createGain();
    g.gain.setValueAtTime(0.0001, t);
    g.gain.exponentialRampToValueAtTime(v, t + 0.02);
    g.gain.exponentialRampToValueAtTime(0.0001, t + dur);
    o1.connect(fl); o2.connect(fl); fl.connect(g); g.connect(bus);
    o1.start(t); o2.start(t); o1.stop(t + dur + 0.02); o2.stop(t + dur + 0.02);
  }
  function bass(t, f, v){
    const o = ctx.createOscillator(); o.type = "sine"; o.frequency.value = f;
    const g = ctx.createGain();
    g.gain.setValueAtTime(0.0001, t);
    g.gain.exponentialRampToValueAtTime(v, t + 0.035);
    g.gain.exponentialRampToValueAtTime(0.0001, t + 0.75);
    o.connect(g); g.connect(bus); o.start(t); o.stop(t + 0.8);
  }
  function hit(env, t, v, dec){
    env.gain.cancelScheduledValues(t);
    env.gain.setValueAtTime(0.0001, t);
    env.gain.linearRampToValueAtTime(v, t + 0.004);
    env.gain.exponentialRampToValueAtTime(0.0001, t + dec);
  }
  function schedule(i, t){
    const bar = i >> 4, s = i & 15, ch = CH[bar];
    const gi = GTR[s];
    if(gi !== null) pluck(t, ch.n[gi], s % 4 === 0 ? 0.050 : 0.034, 0.85);
    const bp = BASS[s];
    if(bp){
      const nx = CH[(bar + 1) % 4];
      bass(t, bp === "r" ? ch.b : bp === "f" ? ch.b * 1.5 : nx.b, bp === "a" ? 0.075 : 0.11);
    }
    hit(gEnv, t, s % 4 === 0 ? 0.018 : s % 2 === 0 ? 0.011 : 0.006, 0.05);    // Güira
    if(s % 2 === 0){                                                          // Bongó
      const open = (s === 12);
      bFil.frequency.setValueAtTime(open ? 480 : (s % 4 === 0 ? 340 : 1150), t);
      hit(bEnv, t, open ? 0.050 : 0.024, open ? 0.16 : 0.07);
    }
  }
  function tick(){
    if(!on) return;
    const now = ctx.currentTime;
    if(next < now) next = now + 0.06;                 // nach Tab-Wechsel neu einhängen
    while(next < now + 0.18){
      schedule(pos, next);
      next += STEP; pos = (pos + 1) % 64;
    }
    timer = setTimeout(tick, 45);
  }
  function start(tries){
    if(on || !S.sound || !S.music) return;
    const c = ac();
    if(!c) return;
    if(c.state !== "running"){                        // Kontext startet asynchron
      if((tries || 0) < 6) setTimeout(()=> start((tries || 0) + 1), 350);
      return;
    }
    ctx = c; build(c); on = true; pos = 0; next = c.currentTime + 0.1;
    master.gain.setValueAtTime(0.0001, c.currentTime);
    master.gain.exponentialRampToValueAtTime(0.30, c.currentTime + 2.4);
    tick();
  }
  function stop(){
    if(!on) return;
    on = false; clearTimeout(timer);
    const c = ctx, m = master, old = srcs;
    srcs = [];
    try{
      m.gain.cancelScheduledValues(c.currentTime);
      m.gain.setValueAtTime(Math.max(0.0001, m.gain.value), c.currentTime);
      m.gain.exponentialRampToValueAtTime(0.0001, c.currentTime + 0.6);
    }catch(e){}
    setTimeout(()=> old.forEach(x => { try{ x.stop(); }catch(e){} }), 900);
  }
  /* Musik läuft in allen Menüs, im Spiel ist Ruhe. */
  function sync(){
    const inGame = document.getElementById("scr-game").classList.contains("on");
    if(inGame || !S.sound || !S.music || document.hidden) stop(); else start(0);
  }
  return {start, stop, sync};
})();

/* ---------------- speech ---------------- */
/* Regionen in Wunschreihenfolge – die erste vorhandene Stimme gewinnt. */
const VPREF = {
  en:["en-us","en-ca","en-gb","en-au","en"],
  es:["es-co","es-mx","es-us","es-419","es-la","es-es","es"],
  de:["de-de","de-at","de-ch","de"]
};
let VOICES = [];
const nl = l => (l || "").toLowerCase().replace(/_/g, "-");
function loadVoices(){
  try{ VOICES = window.speechSynthesis ? (speechSynthesis.getVoices() || []) : []; }
  catch(e){ VOICES = []; }
  return VOICES;
}
/* Sucht eine Stimme, die wirklich zur Sprache gehört. Offline-Stimmen zuerst. */
function pickVoice(lang){
  const pool = VOICES.filter(v => nl(v.lang).split("-")[0] === lang);
  if(!pool.length) return null;
  const rank = list => list.find(v => v.localService && v.default)
                    || list.find(v => v.localService)
                    || list.find(v => v.default)
                    || list[0];
  for(const p of (VPREF[lang] || [lang])){
    const hit = pool.filter(v => nl(v.lang) === p || nl(v.lang).startsWith(p + "-"));
    if(hit.length) return rank(hit);
  }
  return rank(pool);
}
/* Ein einziger Sprech-Slot. Jede neue Ansage verwirft die alte samt Timern,
   damit sich nichts überlagert oder verspätet nachfeuert. */
const Voice = (()=>{
  let cur = null;      // Referenz halten: Chrome verschluckt sonst die Ausgabe
  let tId = 0, wId = 0, gen = 0, primed = false;

  function stop(){
    gen++;
    clearTimeout(tId); clearTimeout(wId);
    cur = null;
    try{ if(window.speechSynthesis) speechSynthesis.cancel(); }catch(e){}
  }

  function prime(){                       // erster Kontakt braucht eine Geste (iOS)
    if(primed || !("speechSynthesis" in window)) return;
    primed = true;
    try{
      const u = new SpeechSynthesisUtterance(" ");
      u.volume = 0; speechSynthesis.speak(u);
    }catch(e){}
  }

  function run(text, lang, opt, myGen, tries){
    if(myGen !== gen) return;
    if(!VOICES.length) loadVoices();
    const v = pickVoice(lang);
    if(!v){
      if(tries < 3 && !VOICES.length){    // Stimmenliste lädt noch
        tId = setTimeout(()=> run(text, lang, opt, myGen, tries + 1), 250);
        return;
      }
      if(opt.onend) opt.onend();          // keine Stimme -> Spielfluss nicht bremsen
      return;
    }
    const u = new SpeechSynthesisUtterance(text.replace(/,\s*/g, ", "));
    u.voice  = v;
    u.lang   = v.lang || LOCALE[lang];
    u.rate   = opt.rate || 0.92; u.pitch = 1; u.volume = 1;
    let done = false;
    const finish = ()=>{
      if(done || myGen !== gen) return;
      done = true; clearTimeout(wId);
      if(opt.onend) opt.onend();
    };
    u.onend = finish; u.onerror = finish;
    cur = u;
    try{
      speechSynthesis.resume();           // falls die Engine hängen geblieben ist
      speechSynthesis.speak(u);
    }catch(e){ finish(); return; }
    // Notbremse, falls onend ausbleibt (kommt bei manchen Engines vor)
    wId = setTimeout(finish, Math.min(2600, 700 + text.length * 70));
  }

  return {
    stop, prime,
    /* true = Sprachausgabe übernimmt das Timing und ruft opt.onend auf */
    say(text, lang, opt){
      opt = opt || {};
      if(!S.speech || !("speechSynthesis" in window)) return false;
      stop();
      const myGen = gen;
      tId = setTimeout(()=> run(text, lang, opt, myGen, 0), 80);  // cancel() braucht einen Tick
      return true;
    }
  };
})();
const speak = (text, lang, opt) => Voice.say(text, lang, opt);
if("speechSynthesis" in window){
  loadVoices();
  speechSynthesis.addEventListener("voiceschanged", ()=>{
    loadVoices();
    if(document.getElementById("scr-settings").classList.contains("on")) renderSettings();
  });
  setTimeout(loadVoices, 800);
}

/* ---------------- screens ---------------- */
function show(id){
  document.querySelectorAll(".screen").forEach(s => s.classList.toggle("on", s.id === "scr-"+id));
  Music.sync();
}

/* ---------------- render: home ---------------- */
function segLang(host, current, onPick){
  host.innerHTML = "";
  ["de","en","es"].forEach(l=>{
    const b = document.createElement("button");
    b.textContent = LANGNAME[l];
    b.setAttribute("aria-pressed", String(l === current));
    b.onclick = ()=>{ onPick(l); };
    host.appendChild(b);
  });
}
function renderHome(){
  t = T[S.ml];
  $("#lb-speak").textContent = t.speak;
  $("#lb-learn").textContent = t.learn;
  $("#b-play").textContent   = t.play + " · " + t.level + " " + playLevel();
  $("#b-levels").textContent = t.levels;
  $("#b-endless").textContent = t.endless;
  $("#b-settings").textContent = t.settings;
  $("#b-sound").innerHTML = SVG_SOUND(S.sound);
  $("#chip-diff").innerHTML = `${t.difficulty}: <b>${t[S.diff]}</b>`;
  const locked = S.unlocked <= LEVELS;
  $("#b-endless").disabled = locked;
  $("#endless-hint").textContent = locked ? (t.endless + ": " + t.locked) : "";
  // Jede Sprache ist immer wählbar – bei Kollision tauschen die beiden Zeilen einfach.
  segLang($("#seg-from"), S.from, l=>{
    if(l === S.to) S.to = S.from;
    S.from = l; save(); renderHome();
  });
  segLang($("#seg-to"), S.to, l=>{
    if(l === S.from) S.from = S.to;
    S.to = l; save(); renderHome();
  });
}

/* ---------------- render: levels ---------------- */
function renderLevels(){
  $("#lv-title").textContent = t.levels;
  document.querySelectorAll("[data-back]").forEach(b => b.textContent = t.back);
  const g = $("#lv-grid"); g.innerHTML = "";
  for(let i=1;i<=LEVELS;i++){
    const b = document.createElement("button");
    b.className = "lvl";
    const st = S.stars[i] || 0, best = S.best[i];
    const stars = [1,2,3].map(k => SVG_STAR(k <= st ? "" : "off")).join("");
    b.innerHTML = `<span class="n">${t.level} ${i}</span>
      <span class="stars">${stars}</span>
      <span class="meta">${t.record}: ${best ? nf(best.s) + " · " + t[best.d] : t.none}</span>`;
    if(i > S.unlocked) b.disabled = true;
    else b.onclick = ()=> startLevel(i);
    g.appendChild(b);
  }
}

/* ---------------- render: settings ---------------- */
function renderSettings(){
  $("#st-title").textContent = t.settings;
  $("#st-lang").textContent  = t.menuLang;
  $("#st-diff").textContent  = t.difficulty;
  $("#st-audio").textContent = t.audio;
  $("#b-reset").textContent  = t.reset;
  document.querySelectorAll("[data-back]").forEach(b => b.textContent = t.back);
  $("#st-fall").textContent = `${t.fall}: ${ff(6 * DIFF[S.diff].f)} ${t.sec}`;
  $("#t-sound").textContent  = `${t.sound}: ${S.sound ? t.on : t.off}`;
  $("#t-music").textContent  = `${t.music}: ${S.music ? t.on : t.off}`;
  $("#t-music").disabled = !S.sound;
  $("#t-speech").textContent = `${t.speech}: ${S.speech ? t.on : t.off}`;
  const vline = l => {
    const v = pickVoice(l);
    return `${t.voice} ${LANGNAME[l]}: ` + (v ? `${v.name} (${v.lang})` : t.noVoice);
  };
  $("#st-voice").innerHTML = [S.from, S.to].map(vline).join("<br>");

  const ml = $("#seg-ml"); ml.innerHTML = "";
  ["de","en","es"].forEach(l=>{
    const b = document.createElement("button");
    b.textContent = LANGNAME[l];
    b.setAttribute("aria-pressed", String(l === S.ml));
    b.onclick = ()=>{ S.ml = l; t = T[l]; document.documentElement.lang = l; save(); renderSettings(); renderHome(); };
    ml.appendChild(b);
  });
  const dg = $("#seg-diff"); dg.innerHTML = "";
  ["easy","med","hard"].forEach(d=>{
    const b = document.createElement("button");
    b.textContent = t[d];
    b.setAttribute("aria-pressed", String(d === S.diff));
    b.onclick = ()=>{ S.diff = d; save(); renderSettings(); renderHome(); };
    dg.appendChild(b);
  });
}

/* ================= GAME ================= */
const G = {
  mode:"level", level:1, queue:[], idx:0, lives:3, score:0, errors:0,
  wrong:[], review:false, tiles:[], raf:0, last:0, prog:0, fallMs:6000,
  running:false, locked:true, boardH:0, count:0, current:null, cdIV:0, ses:0
};
const board = $("#board"), flash = $("#flash");

function fallTimeLevel(lv){ return Math.max(2.5, 6 - 0.2*(lv-1)) * DIFF[S.diff].f * 1000; }
function fallTimeEndless(n){ return Math.max(1.0, 5 * Math.pow(0.97, Math.floor(n/10))) * DIFF[S.diff].f * 1000; }

function startLevel(lv){
  G.mode = "level"; G.level = lv;
  G.queue = W.slice((lv-1)*PER, lv*PER).map((w,i)=>({w, i}));
  G.idx = 0; G.lives = 3; G.score = 0; G.errors = 0; G.wrong = []; G.review = false; G.count = 0;
  S.level = lv; save();
  beginGame();
}
function startEndless(){
  G.mode = "endless";
  G.queue = shuffle(W.slice()).map(w => ({w}));
  G.idx = 0; G.lives = 3; G.score = 0; G.errors = 0; G.wrong = []; G.review = false; G.count = 0;
  beginGame();
}
function beginGame(){
  G.ses++;
  show("game");
  $("#b-pause").innerHTML = SVG_PAUSE;
  $("#g-level").textContent = G.mode === "endless" ? t.endless : (t.level + " " + G.level);
  updateHUD();
  clearTiles();
  countdown(()=> nextWord());
}
function countdown(done){
  const ov = $("#ov-count"); ov.classList.add("on");
  $("#count-lv").textContent = G.mode === "endless" ? t.endless : (t.level + " " + G.level);
  let n = 3, ses = G.ses;
  $("#count-n").textContent = n; sfxTick();
  clearInterval(G.cdIV);
  const iv = G.cdIV = setInterval(()=>{
    if(ses !== G.ses){ clearInterval(iv); return; }
    n--;
    if(n > 0){ $("#count-n").textContent = n; sfxTick(); }
    else{
      clearInterval(iv);
      $("#count-n").textContent = t.go;
      setTimeout(()=>{ if(ses !== G.ses) return; ov.classList.remove("on"); done(); }, 380);
    }
  }, 620);
}
function updateHUD(){
  const lv = $("#g-lives"); lv.innerHTML = "";
  for(let i=0;i<3;i++){ const d = document.createElement("i"); d.className = "pip" + (i < G.lives ? "" : " gone"); lv.appendChild(d); }
  $("#g-score").textContent = nf(G.score);
  if(G.review) $("#g-prog").textContent = "↻ " + G.queue.length;
  else if(G.mode === "endless") $("#g-prog").textContent = G.count + " " + t.words;
  else $("#g-prog").textContent = Math.min(G.idx + 1, PER) + "/" + PER;
}
function clearTiles(){
  G.tiles.forEach(x => x.el.remove());
  G.tiles = [];
  board.querySelectorAll(".col").forEach(c => c.remove());
}
function pickDistractors(word, n){
  const from = S.from, to = S.to;
  const aT = norm(word[LI[to]]), aF = norm(word[LI[from]]);
  const used = new Set([aT]);
  const out = [];
  let guard = 0;
  while(out.length < n && guard++ < 900){
    const c = W[(Math.random()*W.length)|0];
    const cT = norm(c[LI[to]]);
    if(used.has(cT)) continue;
    if(norm(c[LI[from]]) === aF) continue;   // avoid a second valid answer
    used.add(cT); out.push(c);
  }
  return out;
}
function nextWord(){
  if(!document.getElementById("scr-game").classList.contains("on")) return;
  if(G.queue.length === 0 || G.idx >= G.queue.length){
    if(G.mode === "endless"){ G.queue = shuffle(W.slice()).map(w=>({w})); G.idx = 0; }
    else if(!G.review && G.wrong.length){ startReview(); return; }
    else { finish(true); return; }
  }
  const item = G.queue[G.idx];
  const word = item.w;
  G.current = word;
  const cols = (G.mode === "endless" || G.level >= TILES3) ? 3 : 2;
  board.classList.toggle("cols3", cols === 3);

  clearTiles();
  const opts = shuffle([{w:word, ok:true}, ...pickDistractors(word, cols-1).map(w=>({w, ok:false}))]);
  for(let i=0;i<cols;i++){
    const col = document.createElement("div"); col.className = "col";
    const el = document.createElement("div"); el.className = "tile";
    el.textContent = opts[i].w[LI[S.to]];
    el.style.setProperty("--ty","translateY(-200px)");
    el.style.transform = "translateY(-200px)";
    col.appendChild(el); board.appendChild(col);
    const rec = {el, ok:opts[i].ok};
    el.addEventListener("pointerdown", e => { e.preventDefault(); answer(rec); }, {passive:false});
    G.tiles.push(rec);
  }
  $("#p-word").textContent = word[LI[S.from]];
  $("#p-hint").innerHTML = `${LANGNAME[S.from]} · ${t.tapHear}`;
  speak(word[LI[S.from]], S.from);

  G.fallMs = G.mode === "endless" ? fallTimeEndless(G.count) : fallTimeLevel(G.level);
  G.prog = 0; G.locked = false; G.running = true; G.last = 0;
  G.boardH = board.clientHeight - 16;
  updateHUD();
  cancelAnimationFrame(G.raf);
  G.raf = requestAnimationFrame(step);
}
function step(ts){
  if(!G.running) return;
  if(!G.last) G.last = ts;
  const dt = ts - G.last; G.last = ts;
  G.prog += dt / G.fallMs;
  const tileH = G.tiles[0] ? G.tiles[0].el.offsetHeight : 66;
  const y = -tileH + Math.min(G.prog, 1) * G.boardH;
  G.tiles.forEach(x => { x.el.style.setProperty("--ty", `translateY(${y}px)`); x.el.style.transform = `translateY(${y}px)`; });
  if(G.prog >= 1){ G.running = false; timeUp(); return; }
  G.raf = requestAnimationFrame(step);
}
function flashFX(cls){
  flash.className = "flash " + cls;
  setTimeout(()=>{ flash.className = "flash"; }, 220);
}
function answer(rec){
  if(G.locked || !G.running) return;
  G.locked = true; G.running = false;
  cancelAnimationFrame(G.raf);
  if(rec.ok) good(rec); else bad(rec);
}
function timeUp(){
  G.locked = true;
  bad(null);
}
function good(rec){
  sfxOK(); flashFX("ok");
  rec.el.classList.add("hit");
  G.tiles.forEach(x => { if(x !== rec) x.el.classList.add("dim"); });
  if(!G.review){
    const bonus = Math.round(100 * (1 - G.prog));
    G.score += 100 + Math.max(0, bonus);
    if(G.mode === "endless") G.count++;
  }
  updateHUD();
  let moved = false;
  const advance = ()=>{
    if(moved) return; moved = true;
    if(G.review) G.queue.shift();
    else G.idx++;
    nextWord();
  };
  // Lösung in der Lernsprache vorlesen, danach zum nächsten Wort
  const ses = G.ses;
  const talking = Voice.say(G.current[LI[S.to]], S.to,
    {rate:0.85, onend:()=> setTimeout(()=>{ if(ses === G.ses) advance(); }, 180)});
  setTimeout(()=>{ if(ses === G.ses) advance(); }, talking ? 2600 : 420);
}
function bad(rec){
  sfxBad(); flashFX("bad");
  if(rec) rec.el.classList.add("miss");
  const sol = G.tiles.find(x => x.ok);
  if(sol) sol.el.classList.add("solution");
  if(!G.review){
    G.lives--; G.errors++;
    G.wrong.push(G.queue[G.idx]);
    updateHUD();
  }
  setTimeout(()=>{
    if(G.review){
      G.queue.push(G.queue.shift());
      nextWord();
    } else if(G.lives <= 0){
      finish(false);
    } else {
      G.idx++;
      nextWord();
    }
  }, 850);
}
function startReview(){
  G.review = true;
  G.queue = G.wrong.slice();
  G.wrong = [];
  G.idx = 0;
  const ov = $("#ov-count"); ov.classList.add("on");
  $("#count-n").textContent = "↻";
  $("#count-lv").textContent = t.review + " · " + t.reviewHint;
  const ses = G.ses;
  setTimeout(()=>{ if(ses !== G.ses) return; ov.classList.remove("on"); nextWord(); }, 1500);
}

/* ---------------- pause ---------------- */
function pause(){
  if(!G.running) return;
  G.running = false; cancelAnimationFrame(G.raf);
  Voice.stop();
  $("#pz-title").textContent = t.paused;
  $("#pz-resume").textContent = t.resume;
  $("#pz-restart").textContent = t.retry;
  $("#pz-home").textContent = t.home;
  $("#ov-pause").classList.add("on");
}
function unpause(){
  $("#ov-pause").classList.remove("on");
  if(G.prog >= 1) return;
  G.running = true; G.last = 0; G.locked = false;
  G.raf = requestAnimationFrame(step);
}
function quitGame(){
  G.ses++; G.running = false; cancelAnimationFrame(G.raf); clearInterval(G.cdIV);
  $("#ov-pause").classList.remove("on"); $("#ov-count").classList.remove("on");
  clearTiles();
  Voice.stop();
  renderHome(); show("home");
}

/* ---------------- result ---------------- */
function finish(passed){
  G.ses++; G.running = false; cancelAnimationFrame(G.raf); clearInterval(G.cdIV);
  Voice.stop();
  clearTiles();
  const mult = DIFF[S.diff].m;
  const total = Math.round(G.score * mult);
  let stars = 0, rec = false;

  if(G.mode === "level"){
    if(passed){
      stars = Math.max(0, 3 - G.errors);
      if(stars > (S.stars[G.level]||0)) S.stars[G.level] = stars;
      if(G.level + 1 > S.unlocked) S.unlocked = Math.min(LEVELS + 1, G.level + 1);
      if(G.level < LEVELS) S.level = G.level + 1;
      const b = S.best[G.level];
      if(total > 0 && (!b || total > b.s)){ S.best[G.level] = {s: total, d: S.diff}; rec = true; }
    } else {
      const b = S.best[G.level];
      if(total > 0 && (!b || total > b.s)){ S.best[G.level] = {s: total, d: S.diff}; rec = true; }
    }
  } else {
    if(total > S.ebest){ S.ebest = total; S.ebestD = S.diff; rec = true; }
  }
  save();

  $("#r-title").textContent = G.mode === "endless" ? t.endlessOver : (passed ? t.levelDone : t.gameOver);
  $("#r-stars").innerHTML = G.mode === "endless" ? "" :
    [1,2,3].map(k => SVG_STAR(k <= stars ? "" : "off")).join("");
  $("#r-total").textContent = nf(total);
  $("#r-badge").innerHTML = rec ? `<span class="badge">${t.newBest}</span>` : "";
  $("#r-calc-l").textContent = t.calc;
  $("#r-calc").textContent = `${nf(G.score)} × ${ff(mult)} = ${nf(total)}`;
  $("#r-best-l").textContent = t.best;
  const b = G.mode === "endless" ? (S.ebest ? {s:S.ebest, d:S.ebestD} : null) : S.best[G.level];
  $("#r-best").textContent = b ? `${nf(b.s)} · ${t[b.d]}` : t.none;

  const nx = $("#r-next");
  const canNext = G.mode === "level" && passed && G.level < LEVELS;
  nx.style.display = canNext ? "flex" : "none";
  nx.textContent = t.next;
  nx.onclick = ()=> startLevel(G.level + 1);
  $("#r-retry").textContent = t.retry;
  $("#r-retry").onclick = ()=> G.mode === "endless" ? startEndless() : startLevel(G.level);
  $("#r-home").textContent = t.home;
  $("#r-home").onclick = ()=>{ renderHome(); show("home"); };

  if(passed && G.mode === "level") sfxWin(); else sfxBad();
  show("result");
}

/* ---------------- wiring ---------------- */
document.querySelectorAll("[data-ml]").forEach(b => b.onclick = ()=>{
  S.ml = b.dataset.ml; t = T[S.ml]; document.documentElement.lang = S.ml; save();
  ac(); Voice.prime(); renderHome(); show("home");
});
$("#b-play").onclick     = ()=>{ ac(); Voice.prime(); startLevel(playLevel()); };
$("#b-levels").onclick   = ()=>{ renderLevels(); show("levels"); };
$("#b-endless").onclick  = ()=>{ ac(); Voice.prime(); startEndless(); };
$("#b-settings").onclick = ()=>{ renderSettings(); show("settings"); };
$("#b-sound").onclick    = ()=>{ S.sound = !S.sound; save(); renderHome(); if(S.sound){ ac(); sfxTick(); } Music.sync(); };
document.querySelectorAll("[data-back]").forEach(b => b.onclick = ()=>{ renderHome(); show("home"); });
$("#t-sound").onclick  = ()=>{ S.sound = !S.sound; save(); renderSettings(); renderHome(); Music.sync(); if(S.sound) sfxTick(); };
$("#t-music").onclick  = ()=>{ S.music = !S.music; save(); renderSettings(); Music.sync(); };
$("#t-speech").onclick = ()=>{ S.speech = !S.speech; save(); renderSettings(); };
$("#b-reset").onclick  = ()=>{
  $("#dlg-text").textContent = t.resetAsk;
  $("#dlg-no").textContent = t.cancel;
  $("#dlg-yes").textContent = t.del;
  $("#dlg").classList.add("on");
};
$("#dlg-no").onclick  = ()=> $("#dlg").classList.remove("on");
$("#dlg-yes").onclick = ()=>{
  const ml = S.ml, from = S.from, to = S.to, diff = S.diff, sound = S.sound, music = S.music, speech = S.speech;
  S = Object.assign({}, DEF, {ml, from, to, diff, sound, music, speech});
  save(); $("#dlg").classList.remove("on"); renderSettings(); renderHome();
};
$("#b-pause").onclick   = pause;
$("#pz-resume").onclick = unpause;
$("#pz-restart").onclick= ()=>{ $("#ov-pause").classList.remove("on"); G.mode === "endless" ? startEndless() : startLevel(G.level); };
$("#pz-home").onclick   = quitGame;
$("#prompt").addEventListener("pointerdown", e =>{ e.stopPropagation(); if(G.current) speak(G.current[LI[S.from]], S.from); });
$("#p-spk").innerHTML = SVG_SPEAK;

document.addEventListener("keydown", e=>{
  if(!$("#scr-game").classList.contains("on")) return;
  if(e.key === "Escape" || e.key.toLowerCase() === "p"){ e.preventDefault(); $("#ov-pause").classList.contains("on") ? unpause() : pause(); return; }
  if(e.key === " "){ e.preventDefault(); if(G.current) speak(G.current[LI[S.from]], S.from); return; }
  const map = {"1":0,"2":1,"3":2,"ArrowLeft":0,"ArrowDown":1,"ArrowRight":2};
  const i = map[e.key];
  if(i === undefined) return;
  const idx = (G.tiles.length === 2 && e.key === "ArrowRight") ? 1 : i;
  if(G.tiles[idx]) { e.preventDefault(); answer(G.tiles[idx]); }
});
window.addEventListener("resize", ()=>{ G.boardH = board.clientHeight - 16; });
document.addEventListener("visibilitychange", ()=>{ if(document.hidden && G.running) pause(); Music.sync(); });

/* Autoplay-Sperre: bei der ersten Berührung Audio freischalten */
["pointerdown","keydown"].forEach(ev =>
  document.addEventListener(ev, function once(){
    document.removeEventListener(ev, once);
    ac(); Music.sync();
  }, {passive:true}));

/* ---------------- boot ---------------- */
paintHearts();
if(S.ml){ t = T[S.ml]; document.documentElement.lang = S.ml; renderHome(); show("home"); }
else show("lang");
</script>
</body>
</html>

