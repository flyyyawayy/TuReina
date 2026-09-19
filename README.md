<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Una carta pixelada para ti</title>
<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=VT323&display=swap" rel="stylesheet">
<style>
  :root{
    box-sizing:border-box;
    padding-top:env(safe-area-inset-top,0px);
    padding-bottom:env(safe-area-inset-bottom,0px);
    --px:'Press Start 2P','Courier New',monospace;
    --body:'VT323','Courier New',monospace;
    --sky1:#ffb8d6; --sky2:#fff1f7;
    --ink:#3b1030; --muted:#7a3f66;
    --panel:#fff8fc; --panel-hi:#ffffff; --panel-sh:#f3c6da;
    --edge:#3b1030; --shadow:rgba(59,16,48,.28);
    --accent:#e0245e; --accent-d:#9c123f; --gold:#ffcf3d;
    --ol:#3b1030; --spark:#ff8fb8;
    --g1:#ff8fb8; --g2:#ff7aa9; --g3:#c93c72;
  }
  @media (prefers-color-scheme: dark){
    :root:not([data-theme="light"]){
      --sky1:#150a33; --sky2:#5b1b63;
      --ink:#ffeaf4; --muted:#e6bcd6;
      --panel:#2b1653; --panel-hi:#3d2472; --panel-sh:#1d0e3c;
      --edge:#ffeaf4; --shadow:rgba(0,0,0,.45);
      --ol:#0c0420; --spark:#fff6c9;
      --g1:#5a1f6e; --g2:#4d1a5e; --g3:#2e0f3f;
    }
  }
  :root[data-theme="dark"]{
    --sky1:#150a33; --sky2:#5b1b63;
    --ink:#ffeaf4; --muted:#e6bcd6;
    --panel:#2b1653; --panel-hi:#3d2472; --panel-sh:#1d0e3c;
    --edge:#ffeaf4; --shadow:rgba(0,0,0,.45);
    --ol:#0c0420; --spark:#fff6c9;
    --g1:#5a1f6e; --g2:#4d1a5e; --g3:#2e0f3f;
  }

  html{height:100%;scroll-padding-top:env(safe-area-inset-top,0px)}
  body{
    height:100%;margin:0;background:var(--sky2);color:var(--ink);
    font-family:var(--body);font-size:26px;line-height:1.25;
    overflow-x:hidden;-webkit-tap-highlight-color:transparent;
  }
  *,*::before,*::after{box-sizing:border-box}
  svg{display:block;image-rendering:pixelated;max-width:100%;height:auto}
  :focus-visible{outline:4px dashed var(--accent);outline-offset:4px}

  /* Paleta de los sprites */
  .cO,.cK{fill:var(--ol)} .cR{fill:#ff3d7f} .cH{fill:#ffb3d1} .cW{fill:#fff4e0}
  .cY{fill:#ffcf3d} .cN{fill:#b5651d} .cS{fill:#ffd2b0} .cB{fill:#5ec8ff}
  .cD{fill:#232a4d} .cG{fill:#4b5670} .cT{fill:#b07a4a} .cE{fill:#f4f4f4}
  .cZ{fill:#20202e} .cA{fill:#3a1f1a} .cU{fill:#d69a6c} .cM{fill:#d9668a}

  /* Fondo */
  .bg{position:fixed;inset:0;background:linear-gradient(var(--sky1),var(--sky2));z-index:0}
  .fx{position:fixed;inset:0;overflow:hidden;pointer-events:none;z-index:0}
  .st{position:absolute;background:var(--spark);animation:blink 2.4s steps(1) infinite}
  .bh{position:absolute;bottom:-60px;opacity:.5;animation:rise linear infinite}
  .ground{
    position:fixed;left:0;right:0;bottom:0;z-index:0;
    height:calc(56px + env(safe-area-inset-bottom,0px));
    padding-bottom:env(safe-area-inset-bottom,0px);
    background:repeating-linear-gradient(90deg,var(--g1) 0 24px,var(--g2) 24px 48px);
    border-top:8px solid var(--g3);
  }

  #stage{
    position:relative;z-index:1;min-height:100%;
    display:flex;align-items:center;justify-content:center;
    padding:60px 16px 100px;
  }
  .scene{display:none;width:100%;max-width:640px;text-align:center}
  .scene.on{display:flex;flex-direction:column;align-items:center;gap:22px}

  h1,h2{font-family:var(--px);margin:0;line-height:1.7;color:var(--ink);text-shadow:4px 4px 0 var(--accent)}
  h1{font-size:clamp(18px,5.4vw,32px)}
  h2{font-size:clamp(16px,4.6vw,26px)}
  .sub{margin:0;font-size:28px;color:var(--ink)}
  .sub b{color:var(--accent)}
  .coin{font-family:var(--px);font-size:11px;line-height:1.8;margin:0;color:var(--muted)}

  /* Paneles y botones */
  .panel{
    background:var(--panel);color:var(--ink);border:4px solid var(--edge);width:100%;padding:18px 20px;
    box-shadow:inset -4px -4px 0 var(--panel-sh),inset 4px 4px 0 var(--panel-hi),6px 6px 0 var(--shadow);
  }
  .btn{
    font-family:var(--px);font-size:14px;line-height:1.4;color:#fff;background:var(--accent);
    border:4px solid var(--edge);padding:16px 24px;cursor:pointer;
    box-shadow:inset -4px -4px 0 var(--accent-d),inset 4px 4px 0 rgba(255,255,255,.28),0 6px 0 var(--shadow);
  }
  .btn:active{transform:translateY(4px);box-shadow:inset -4px -4px 0 var(--accent-d),inset 4px 4px 0 rgba(255,255,255,.28),0 2px 0 var(--shadow)}
  .btn.ghost{background:var(--panel);color:var(--ink);box-shadow:inset -4px -4px 0 var(--panel-sh),inset 4px 4px 0 var(--panel-hi),0 6px 0 var(--shadow)}
  .btn.small{font-size:11px;padding:10px 14px}
  .snd{position:fixed;top:calc(env(safe-area-inset-top,0px) + 10px);right:12px;z-index:5}

  /* Título */
  .bob{animation:bob .8s steps(2,end) infinite alternate}
  .blink{animation:blink 1s steps(1) infinite}

  /* Sobre */
  .env{position:relative;background:none;border:0;padding:0;cursor:pointer;animation:wig 1.2s steps(2) infinite}
  .env .seal{position:absolute;left:50%;top:52%;transform:translate(-50%,-50%)}
  .env.open{animation:openEnv .65s steps(6) forwards}

  /* Diálogo */
  .hud{display:flex;align-items:center;justify-content:center;gap:12px;flex-wrap:wrap}
  .hud span{font-family:var(--px);font-size:11px}
  .bar{display:flex;gap:3px}
  .bar i{width:20px;height:16px;background:var(--panel-sh);border:2px solid var(--edge)}
  .bar i.on{background:var(--accent)}
  .who{display:flex;align-items:flex-end;gap:14px;align-self:flex-start;margin-bottom:-8px}
  .npc{animation:bob .7s steps(2,end) infinite alternate}
  .tag{
    font-family:var(--px);font-size:12px;background:var(--accent);color:#fff;
    border:4px solid var(--edge);padding:8px 12px;margin-bottom:6px;
  }
  .dialog{position:relative;text-align:left;min-height:170px;padding:20px 22px 40px;cursor:pointer}
  .dialog p{margin:0;font-size:30px;line-height:1.2}
  .next{
    position:absolute;right:16px;bottom:12px;width:44px;height:36px;background:none;border:0;cursor:pointer;
  }
  .next::after{
    content:'';position:absolute;left:12px;top:12px;
    border-left:10px solid transparent;border-right:10px solid transparent;border-top:13px solid var(--ink);
    animation:blink 1s steps(1) infinite;
  }
  .choices{display:flex;flex-direction:column;gap:12px;margin-top:16px}
  .choices[hidden]{display:none}
  .opt{
    position:relative;font-family:var(--px);font-size:12px;line-height:1.5;text-align:left;
    background:var(--panel-hi);color:var(--ink);border:4px solid var(--edge);padding:14px 14px 14px 38px;cursor:pointer;
  }
  .opt::before{
    content:'';position:absolute;left:14px;top:50%;margin-top:-7px;
    border-top:7px solid transparent;border-bottom:7px solid transparent;border-left:10px solid var(--accent);
  }
  .opt:hover,.opt:focus-visible{background:var(--accent);color:#fff}
  .hint{margin:0;font-size:22px;color:var(--muted)}

  /* Minijuego */
  .score{font-family:var(--px);font-size:16px;color:var(--ink)}
  .score span:first-child{color:var(--accent)}
  .arena{
    position:relative;height:clamp(260px,46vh,380px);padding:0;overflow:hidden;
    display:flex;align-items:center;justify-content:center;
    background-image:
      linear-gradient(var(--panel-sh) 2px,transparent 2px),
      linear-gradient(90deg,var(--panel-sh) 2px,transparent 2px);
    background-size:32px 32px;
  }
  .gh{position:absolute;background:none;border:0;padding:6px;cursor:pointer;animation:pop .25s steps(3)}
  .gh.warn{animation:blink .3s steps(1) infinite}
  .plus{
    position:absolute;font-family:var(--px);font-size:14px;color:var(--accent);
    text-shadow:2px 2px 0 var(--panel);pointer-events:none;animation:up .6s steps(6) forwards;
  }
  .chest{background:none;border:0;cursor:pointer;padding:8px;animation:shake .5s steps(2) infinite}

  /* Final */
  .win{font-size:clamp(18px,5.6vw,30px)}
  .letter{text-align:left}
  .letter p{margin:0}
  .cierre{font-size:30px;line-height:1.2}
  .firma{margin-top:10px !important;font-family:var(--px);font-size:12px;line-height:1.6;color:var(--accent);text-align:left}
  .stats{text-align:left;font-family:var(--px);font-size:12px;line-height:2.2}
  .row{display:flex;align-items:baseline;gap:8px}
  .row .dots{flex:1;border-bottom:4px dotted var(--muted);transform:translateY(-4px)}
  .row b{color:var(--accent);font-weight:400}
  .prize{background:var(--gold);color:#3b1030;text-align:center;
    box-shadow:inset -4px -4px 0 #d8a800,inset 4px 4px 0 #ffe98a,6px 6px 0 var(--shadow)}
  .prize .ptag{font-family:var(--px);font-size:12px;display:block;margin-bottom:8px}
  .prize p{margin:0;font-size:30px;line-height:1.15}
  .cont{font-family:var(--px);font-size:11px;color:var(--muted);margin:0}
  .confetti{position:fixed;inset:0;pointer-events:none;overflow:hidden;z-index:6}
  .confetti span{position:absolute;top:-50px;animation:fall linear 1 both}

  .duo{display:flex;align-items:flex-end;justify-content:center;gap:10px}
  .rim{filter:drop-shadow(4px 0 0 var(--edge)) drop-shadow(-4px 0 0 var(--edge)) drop-shadow(0 4px 0 var(--edge)) drop-shadow(0 -4px 0 var(--edge))}

  .sign{margin-top:18px;padding-top:16px;border-top:4px dotted var(--muted)}
  .signTitle{font-family:var(--px);font-size:11px;line-height:1.6;color:var(--muted);margin:0 0 10px !important}
  .cells{display:flex;gap:4px}
  .cell{
    flex:1 1 0;min-width:0;max-width:36px;height:38px;display:flex;align-items:center;justify-content:center;
    font-family:var(--px);font-size:13px;border:4px solid var(--edge);background:var(--panel-hi);color:var(--muted);
  }
  .cell.hint{background:var(--panel-sh);color:var(--ink)}
  .cell.ok{background:var(--gold);color:#3b1030;animation:pop .25s steps(3)}
  .cells.no{animation:shake .4s steps(2) 1}
  .guess{display:flex;flex-direction:column;gap:10px;margin-top:14px}
  .guess input{
    font-family:var(--px);font-size:16px;padding:12px;width:100%;
    border:4px solid var(--edge);background:var(--panel-hi);color:var(--ink);
  }
  .guess input::placeholder{color:var(--muted)}
  .guess .btn{align-self:flex-start}
  #signMsg{font-size:26px;margin-top:10px !important;min-height:1.2em}
  .mem{display:flex;gap:16px;justify-content:center;align-items:flex-end;flex-wrap:wrap}
  .mem figure{margin:0;flex:1 1 130px;max-width:260px}
  .mem img{width:100%;height:auto;image-rendering:pixelated;border:4px solid var(--edge);display:block}
  .mem figcaption{font-family:var(--px);font-size:10px;line-height:1.6;margin-top:8px;text-align:center}

  .sr{position:absolute;width:1px;height:1px;overflow:hidden;clip:rect(0 0 0 0);white-space:nowrap}

  @keyframes blink{50%{opacity:0}}
  @keyframes bob{to{transform:translateY(-10px)}}
  @keyframes rise{to{transform:translateY(-125vh)}}
  @keyframes wig{0%,100%{transform:rotate(0)}25%{transform:rotate(-4deg)}75%{transform:rotate(4deg)}}
  @keyframes openEnv{0%{transform:scale(1)}40%{transform:scale(1.15) rotate(-3deg)}100%{transform:scale(3);opacity:0}}
  @keyframes pop{from{transform:scale(.2)}to{transform:scale(1)}}
  @keyframes up{to{transform:translateY(-40px);opacity:0}}
  @keyframes shake{0%,100%{transform:translateX(0)}25%{transform:translateX(-6px)}75%{transform:translateX(6px)}}
  @keyframes fall{to{transform:translateY(118vh)}}

  @media (prefers-reduced-motion: reduce){
    *,*::before,*::after{animation-duration:.001s !important;animation-iteration-count:1 !important;animation-delay:0s !important}
  }
  @media (max-width:420px){
    body{font-size:24px}
    .dialog p{font-size:27px}
    .bar i{width:16px}
  }
</style>
</head>
<body>
<div class="bg"></div>
<div class="fx" id="fx"></div>
<div class="ground"></div>

<button class="btn small ghost snd" id="sndBtn" aria-pressed="true">SONIDO: SÍ</button>

<main id="stage">

  <!-- 1. Título -->
  <section class="scene on" id="s-title">
    <div class="duo bob"><span class="rim" id="tBat"></span><span id="titleHeart"></span><span class="rim" id="tCat"></span></div>
    <h1>DÍA DEL AMOR<br>Y LA AMISTAD</h1>
    <p class="coin">EN COLOMBIA · TERCER SÁBADO DE SEPTIEMBRE</p>
    <p class="sub">Para: <b data-para></b> &nbsp; De: <b data-de></b></p>
    <p class="coin blink">- INSERTA CORAZÓN PARA JUGAR -</p>
    <button class="btn" id="startBtn">START</button>
  </section>

  <!-- 2. Sobre -->
  <section class="scene" id="s-env">
    <h2>UNA LECHUZA<br>TRAJO UNA CARTA</h2>
    <div class="bob rim" id="owl"></div>
    <button class="env" id="envBtn" aria-label="Abrir la carta">
      <span id="envSprite"></span>
      <span class="seal" id="seal"></span>
    </button>
    <p class="coin blink">TOCA EL SOBRE PARA ABRIRLO</p>
  </section>

  <!-- 3. Diálogo -->
  <section class="scene" id="s-dialog">
    <div class="hud"><span>CARIÑO</span><div class="bar" id="bar" role="progressbar" aria-label="Progreso de la carta" aria-valuemin="0" aria-valuemax="100" aria-valuenow="0"></div></div>
    <div class="who"><div class="npc rim" id="npc"></div><span class="tag" id="speaker"></span></div>
    <div class="panel dialog" id="dialog">
      <span class="sr" id="srText" aria-live="polite"></span>
      <p id="text" aria-hidden="true"></p>
      <div class="choices" id="choices" hidden></div>
      <button class="next" id="nextBtn" aria-label="Continuar"></button>
    </div>
    <p class="hint">Toca la caja, o presiona Espacio, para seguir</p>
  </section>

  <!-- 4. Minijuego -->
  <section class="scene" id="s-game">
    <h2>NIVEL BONUS</h2>
    <p class="sub" id="gameHint"></p>
    <div class="score"><span id="count">0</span>/<span id="goal">7</span></div>
    <div class="panel arena" id="arena" aria-label="Zona de juego"></div>
    <button class="btn small ghost" id="skipBtn">Saltar bonus</button>
  </section>

  <!-- 5. Final -->
  <section class="scene" id="s-end">
    <h2 class="win">¡NIVEL<br>COMPLETADO!</h2>
    <div class="duo bob"><span class="rim" id="eBat"></span><span id="endHeart"></span><span class="rim" id="eCat"></span></div>
    <div class="panel letter">
      <p class="cierre" id="cierre"></p>
      <div class="sign">
        <p class="signTitle">FIRMADO POR</p>
        <div class="cells" id="cells" role="img" aria-label="Nombre oculto"></div>
        <p class="firma" id="firmaRol"></p>
        <div class="guess">
          <input id="guess" type="text" autocomplete="off" autocapitalize="words" spellcheck="false" maxlength="24" placeholder="¿Quién soy?" aria-label="Escribe el nombre oculto de quien firma">
          <button class="btn small" id="guessBtn">DESCIFRAR</button>
        </div>
        <p id="signMsg" aria-live="polite"></p>
      </div>
    </div>
    <div class="panel">
      <p class="signTitle">RECUERDOS DESBLOQUEADOS</p>
      <div class="mem">
        <figure><img src="data:image/png;base64,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" width="84" height="119" alt="Ilustración de los dos haciendo un corazón con las manos"><figcaption>RECUERDO 1</figcaption></figure>
        <figure><img src="data:image/png;base64,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" width="112" height="96" alt="Ilustración de los dos como Batman y Catwoman bajo la luna"><figcaption>RECUERDO 2</figcaption></figure>
      </div>
    </div>
    <div class="panel stats" id="stats"></div>
    <div class="panel prize"><span class="ptag">PREMIO</span><p id="vale"></p></div>
    <p class="cont blink">CONTINUARÁ...</p>
    <button class="btn ghost" id="replay">JUGAR DE NUEVO</button>
  </section>

</main>

<script>
/* ============================================================
   PERSONALIZA AQUÍ: nombres, mensajes, premio
   Usa {para} y {de} dentro de los textos para insertar los nombres.
   ============================================================ */
const CONFIG = {
  para: "Piero",
  de: "TU REINA",
  emisor: "MENSAJERO MÁGICO",
  metaCorazones: 10,
  paginas: [
    { quien: "MENSAJERO MÁGICO", t: "¡Alto ahí, {para}! Una lechuza me trajo una carta sellada con magia, y es solo para ti." },
    { t: "Aquí en Colombia, el Día del Amor y la Amistad se celebra el tercer sábado de septiembre. Este año cayó el 19." },
    { t: "Y aunque estés lejos, una reina decidió que su rey merece un nivel bonus." },
    { quien: "{de}", t: "Hola, mi amor hermoso. Quise hacerte una carta distinta, de esas que se juegan en vez de solo leerse." },
    { t: "Eres mi hombre, mi rey, mi sol. Contigo todo se siente más fácil, como si me hubieran activado las vidas infinitas." },
    { t: "Gracias por cuidarme, por hacerme reír y por estar en mi equipo en cada nivel y seguir apostándole a este proyecto que queremos construir juntos." },
    { choice: true,
      t: "Pregunta seria, {para}: ¿Quieres seguir viviendo aventuras como mi dúo de la historia, seguir siendo mi consentido y seguir subiendo muchos niveles más?",
      opciones: ["Sí, mi reina", "Obvio que sí, mi amor"],
      respuesta: "¡Logro desbloqueado! Pareja de nivel legendario." },
    { t: "Antes del final hay un bonus: atrapa los corazones y abre el cofre. Ojo, se escapan rápido." }
  ],
  cierre: "Mi amor hermoso, mi hombre, mi rey, mi sol: gracias por ser mi lugar favorito en este mundo, aunque nos separen kilómetros. Feliz Día del Amor y la Amistad. Aquí tienes vidas infinitas.",

  /* Firma encriptada: las letras conocidas van en la pista y "_" es una letra oculta.
     Los códigos son huellas del nombre, así que el nombre real no aparece escrito en la página. */
  firmaPista: "M_R__L_T_",
  firmaRol: "tu Reina, tu Amor",
  firmaHashes: [3600387820150492, 7479633593139257],
  pistas: [
    "Pista: es el nombre cariñoso que solo tú me dices.",
    "Pista: fíjate bien en las letras que ya están, mi rey."
  ],
  firmaOk: "¡Sabía que lo descifrarías, {para}! Sí, soy yo: tu Reina, tu Amor.",
  firmaMal: "Mmm, esa no es. Piensa bien, mi rey.",

  stats: [
    ["AMOR", "MAX"],
    ["RISAS JUNTOS", "∞"],
    ["BESOS PENDIENTES", "99+"],
    ["VIDAS EXTRA", "∞"]
  ],
  vale: "Vale por 1 abrazo eterno, 1 plan a tu elección y todos los besos que quepan."
};

/* ============================================================
   Sprites en pixel art (cada letra es un color, el punto es vacío)
   ============================================================ */
const HEART = [
  "..OO...OO..",
  ".ORRO.ORRO.",
  "ORHRRORRRRO",
  "ORHRRRRRRRO",
  "ORRRRRRRRRO",
  ".ORRRRRRRO.",
  "..ORRRRRO..",
  "...ORRRO...",
  "....ORO....",
  ".....O....."
];
const ENVELOPE = [
  "OOOOOOOOOOOOOOOO",
  "OWOWWWWWWWWWWOWO",
  "OWWOWWWWWWWWOWWO",
  "OWWWOWWWWWWOWWWO",
  "OWWWWOWWWWOWWWWO",
  "OWWWWWOWWOWWWWWO",
  "OWWWWWWOOWWWWWWO",
  "OWWWWWWWWWWWWWWO",
  "OWWWWWWWWWWWWWWO",
  "OWWWWWWWWWWWWWWO",
  "OOOOOOOOOOOOOOOO"
];
const WIZ = [
  "..AA.AA.AA..",
  ".AAAAAAAAAA.",
  ".AAAAAAAAAA.",
  ".ASSSSSSSSA.",
  ".SKKKSSKKKS.",
  ".SKEKSSKEKS.",
  ".SKKKSSKKKS.",
  ".SSSSRRSSSSY",
  "..SSSSSSSS.N",
  ".ZRYRYRYRYZN",
  "ZZZZZRYZZZSN",
  "ZZZZZZZZZZZZ",
  ".ZZZZZZZZZZ.",
  "..ZZZZZZZZ.."
];
const OWL = [
  ".OO......OO.",
  ".ONNOOOONNO.",
  "ONNNNNNNNNNO",
  "ONEEENNEEENO",
  "ONEKENNEKENO",
  "ONEEENNEEENO",
  "ONNNNYYNNNNO",
  "ONWWWWWWWWNO",
  "ONWWNWWNWWNO",
  ".ONWWWWWWNO.",
  "..OONNNNOO..",
  "...YY..YY..."
];
const CHEST = [
  "..OOOOOOOOOO..",
  ".ONNNNNNNNNNO.",
  "ONNNNNNNNNNNNO",
  "ONNNNNNNNNNNNO",
  "OOOOOOYYOOOOOO",
  "OYYYYYYYYYYYYO",
  "ONNNNNYYNNNNNO",
  "ONNNNNNNNNNNNO",
  "ONNNNNNNNNNNNO",
  "OOOOOOOOOOOOOO"
];

const BAT = [
  "..D......D..",
  "..DD....DD..",
  "..DDDDDDDD..",
  ".DDDDDDDDDD.",
  ".DEEDDDDEED.",
  ".DDDDDDDDDD.",
  ".DTTTTTTTTD.",
  ".DTTDDDDTTD.",
  "..DTTDDTTD..",
  "DGGGGGGGGGGD",
  "DGGGDDDDGGGD",
  "DGGGGDDGGGGD",
  "DGGGYYYYGGGD",
  ".DDGGGGGGDD."
];
const CAT = [
  "..Z......Z..",
  "..ZZ....ZZ..",
  "..ZZAAAAZZ..",
  ".AAAAAAAAAA.",
  ".AAUUUUUUAA.",
  ".AUKUUUUKUA.",
  ".AUUUUUUUUA.",
  ".AUUUMMUUUA.",
  ".AAUUUUUUAA.",
  "AAZZZZZZZZAA",
  "AAZZZZZZZZAA",
  "AZZZZZZZZZZA",
  ".ZZZZZZZZZZ.",
  "..ZZZZZZZZ.."
];

function sprite(map, px){
  const h = map.length, w = map[0].length;
  let rects = "";
  map.forEach((row, y) => {
    let x = 0;
    while (x < w){
      const c = row[x];
      if (c === "."){ x++; continue; }
      let e = x;
      while (e < w && row[e] === c) e++;
      rects += `<rect class="c${c}" x="${x}" y="${y}" width="${e - x}" height="1"/>`;
      x = e;
    }
  });
  return `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 ${w} ${h}" width="${w * px}" height="${h * px}" shape-rendering="crispEdges" aria-hidden="true">${rects}</svg>`;
}

/* ============================================================
   Utilidades
   ============================================================ */
const $ = s => document.querySelector(s);
const fill = s => s.split("{para}").join(CONFIG.para).split("{de}").join(CONFIG.de);
const reduceMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches;

document.querySelectorAll("[data-para]").forEach(e => e.textContent = CONFIG.para);
document.querySelectorAll("[data-de]").forEach(e => e.textContent = CONFIG.de);
document.title = "Una carta pixelada para " + CONFIG.para;

/* Sonido 8-bit */
let audioOn = true, actx = null;
function beep(freq, dur = .08, type = "square", vol = .04){
  if (!audioOn) return;
  try {
    actx = actx || new (window.AudioContext || window.webkitAudioContext)();
    const o = actx.createOscillator(), g = actx.createGain();
    o.type = type; o.frequency.value = freq; g.gain.value = vol;
    o.connect(g); g.connect(actx.destination);
    o.start();
    g.gain.exponentialRampToValueAtTime(.0001, actx.currentTime + dur);
    o.stop(actx.currentTime + dur);
  } catch (e) {}
}
function jingle(notes){ notes.forEach((n, i) => setTimeout(() => beep(n, .14), i * 120)); }

const sndBtn = $("#sndBtn");
sndBtn.addEventListener("click", () => {
  audioOn = !audioOn;
  sndBtn.textContent = audioOn ? "SONIDO: SÍ" : "SONIDO: NO";
  sndBtn.setAttribute("aria-pressed", String(audioOn));
  beep(600);
});

/* Fondo: estrellitas y corazones que suben */
(function buildBackground(){
  const fx = $("#fx");
  for (let i = 0; i < 36; i++){
    const s = document.createElement("span");
    s.className = "st";
    const z = [3, 4, 6][Math.random() * 3 | 0];
    s.style.cssText = `left:${Math.random() * 100}%;top:${Math.random() * 85}%;width:${z}px;height:${z}px;animation-delay:${Math.random() * 3}s`;
    fx.appendChild(s);
  }
  for (let i = 0; i < 9; i++){
    const h = document.createElement("span");
    h.className = "bh";
    h.innerHTML = sprite(HEART, 2 + (i % 3));
    h.style.cssText = `left:${5 + Math.random() * 90}%;animation-duration:${14 + Math.random() * 12}s;animation-delay:${-Math.random() * 20}s`;
    fx.appendChild(h);
  }
})();

/* Sprites fijos */
$("#titleHeart").innerHTML = sprite(HEART, 7);
$("#tBat").innerHTML       = sprite(BAT, 7);
$("#tCat").innerHTML       = sprite(CAT, 7);
$("#eBat").innerHTML       = sprite(BAT, 7);
$("#eCat").innerHTML       = sprite(CAT, 7);
$("#owl").innerHTML        = sprite(OWL, 6);
$("#envSprite").innerHTML  = sprite(ENVELOPE, 14);
$("#seal").innerHTML       = sprite(HEART, 3);
$("#endHeart").innerHTML   = sprite(HEART, 7);

/* Escenas */
let current = "s-title";
function go(id){
  document.querySelectorAll(".scene").forEach(s => s.classList.toggle("on", s.id === id));
  current = id;
  window.scrollTo(0, 0);
}

/* ============================================================
   Escena 1 y 2: inicio y sobre
   ============================================================ */
$("#startBtn").addEventListener("click", () => {
  jingle([392, 523, 659]);
  go("s-env");
});

let opening = false;
$("#envBtn").addEventListener("click", () => {
  if (opening) return;
  opening = true;
  const env = $("#envBtn");
  env.classList.add("open");
  jingle([523, 659, 784, 1047]);
  setTimeout(() => {
    go("s-dialog");
    env.classList.remove("open");
    opening = false;
    pageIdx = 0; lastWho = CONFIG.emisor;
    showPage();
  }, 650);
});

/* ============================================================
   Escena 3: diálogo estilo RPG
   ============================================================ */
const pages = CONFIG.paginas;
let pageIdx = 0, lastWho = CONFIG.emisor, typing = null, full = "", pos = 0, onDone = null, waitingChoice = false;
const textEl = $("#text"), srText = $("#srText"), choicesEl = $("#choices"), nextBtn = $("#nextBtn");

const bar = $("#bar");
for (let i = 0; i < 10; i++) bar.appendChild(document.createElement("i"));
function setBar(frac){
  const on = Math.round(frac * 10);
  [...bar.children].forEach((c, i) => c.classList.toggle("on", i < on));
  bar.setAttribute("aria-valuenow", String(Math.round(frac * 100)));
}

function typeText(str, done){
  clearInterval(typing);
  full = str; pos = 0; onDone = done;
  textEl.textContent = "";
  srText.textContent = str;
  typing = setInterval(() => {
    pos++;
    textEl.textContent = full.slice(0, pos);
    if (pos % 3 === 0) beep(500 + Math.random() * 90, .03, "square", .02);
    if (pos >= full.length) finishTyping();
  }, 28);
}
function finishTyping(){
  clearInterval(typing); typing = null;
  textEl.textContent = full;
  if (onDone){ const d = onDone; onDone = null; d(); }
}

function showPage(){
  const p = pages[pageIdx];
  if (p.quien) lastWho = p.quien;
  $("#speaker").textContent = fill(lastWho);
  $("#npc").innerHTML = lastWho === "{de}" ? sprite(CAT, 9) : sprite(WIZ, 9);
  choicesEl.hidden = true; choicesEl.innerHTML = "";
  nextBtn.style.visibility = "hidden";
  setBar((pageIdx + 1) / pages.length);
  typeText(fill(p.t), () => {
    if (p.choice){
      waitingChoice = true;
      choicesEl.hidden = false;
      p.opciones.forEach(o => {
        const b = document.createElement("button");
        b.className = "opt"; b.textContent = o;
        b.addEventListener("click", () => pick(p));
        choicesEl.appendChild(b);
      });
      choicesEl.querySelector("button").focus({ preventScroll: true });
    } else {
      nextBtn.style.visibility = "visible";
    }
  });
}

function pick(p){
  waitingChoice = false;
  choicesEl.hidden = true; choicesEl.innerHTML = "";
  jingle([659, 784, 988]);
  typeText(fill(p.respuesta), () => { nextBtn.style.visibility = "visible"; });
}

function advance(){
  if (current !== "s-dialog") return;
  if (typing){ finishTyping(); return; }
  if (waitingChoice) return;
  beep(440, .05);
  if (pageIdx < pages.length - 1){ pageIdx++; showPage(); }
  else startGame();
}

$("#dialog").addEventListener("click", e => {
  if (e.target.closest(".choices")) return;
  advance();
});
document.addEventListener("keydown", e => {
  if (e.key !== " " && e.key !== "Enter") return;
  if (e.target.tagName === "BUTTON") return;
  if (current === "s-dialog"){ e.preventDefault(); advance(); }
});

/* ============================================================
   Escena 4: minijuego de corazones
   ============================================================ */
const arena = $("#arena");
let caught = 0, active = false, spawnLoop = null;
const goal = CONFIG.metaCorazones;
$("#goal").textContent = goal;

function updateCount(){ $("#count").textContent = caught; }

function startGame(){
  go("s-game");
  caught = 0; updateCount();
  arena.innerHTML = "";
  $("#gameHint").innerHTML = `Atrapa <b>${goal}</b> corazones antes de que se escapen`;
  active = true;
  clearInterval(spawnLoop);
  spawnLoop = setInterval(spawn, 650);
  spawn();
}

function spawn(){
  if (!active) return;
  if (arena.querySelectorAll(".gh").length >= 3) return;
  const b = document.createElement("button");
  b.className = "gh";
  b.setAttribute("aria-label", "Corazón: tócalo para atraparlo");
  b.innerHTML = sprite(HEART, 5);
  const w = arena.clientWidth, h = arena.clientHeight;
  b.style.left = Math.max(0, Math.random() * (w - 76)) + "px";
  b.style.top  = Math.max(0, Math.random() * (h - 76)) + "px";
  const life = reduceMotion ? 4000 : 2200;
  const t1 = setTimeout(() => b.classList.add("warn"), life - 600);
  const t2 = setTimeout(() => b.remove(), life);
  b.addEventListener("click", () => {
    clearTimeout(t1); clearTimeout(t2);
    catchHeart(b);
  });
  arena.appendChild(b);
}

function catchHeart(b){
  const plus = document.createElement("span");
  plus.className = "plus"; plus.textContent = "+1";
  plus.style.left = b.style.left; plus.style.top = b.style.top;
  arena.appendChild(plus);
  setTimeout(() => plus.remove(), 650);
  b.remove();
  caught++; updateCount();
  beep(520 + caught * 70, .09);
  if (caught >= goal) winGame();
}

function winGame(){
  active = false; clearInterval(spawnLoop);
  arena.innerHTML = "";
  jingle([523, 659, 784, 1047]);
  $("#gameHint").textContent = "¡Cofre desbloqueado! Tócalo para abrirlo";
  const chest = document.createElement("button");
  chest.className = "chest";
  chest.setAttribute("aria-label", "Abrir el cofre");
  chest.innerHTML = sprite(CHEST, 12);
  chest.addEventListener("click", openEnd);
  arena.appendChild(chest);
  chest.focus({ preventScroll: true });
}

$("#skipBtn").addEventListener("click", () => {
  active = false; clearInterval(spawnLoop);
  openEnd();
});

/* ============================================================
   Escena 5: final
   ============================================================ */
const cyrb53 = (str, seed = 0) => {
  let h1 = 0xdeadbeef ^ seed, h2 = 0x41c6ce57 ^ seed;
  for (let i = 0, ch; i < str.length; i++){
    ch = str.charCodeAt(i);
    h1 = Math.imul(h1 ^ ch, 2654435761);
    h2 = Math.imul(h2 ^ ch, 1597334677);
  }
  h1 = Math.imul(h1 ^ (h1 >>> 16), 2246822507);
  h1 ^= Math.imul(h2 ^ (h2 >>> 13), 3266489909);
  h2 = Math.imul(h2 ^ (h2 >>> 16), 2246822507);
  h2 ^= Math.imul(h1 ^ (h1 >>> 13), 3266489909);
  return 4294967296 * (2097151 & h2) + (h1 >>> 0);
};
const norm = t => t.normalize("NFD").replace(/[\u0300-\u036f]/g, "").toLowerCase().replace(/[^a-z]/g, "");
let fails = 0, signed = false;

function initSign(){
  fails = 0; signed = false;
  const cells = $("#cells");
  cells.innerHTML = "";
  [...CONFIG.firmaPista].forEach(ch => {
    const c = document.createElement("span");
    c.className = "cell" + (ch === "_" ? "" : " hint");
    c.textContent = ch === "_" ? "?" : ch;
    cells.appendChild(c);
  });
  $("#firmaRol").textContent = CONFIG.firmaRol;
  $("#guess").value = ""; $("#guess").disabled = false; $("#guessBtn").disabled = false;
  $("#signMsg").textContent = "Solo tú sabes quién firma. Escribe mi nombre para descifrarlo.";
}

function tryGuess(){
  if (signed) return;
  const raw = $("#guess").value.trim();
  if (!raw) return;
  const cells = $("#cells");
  if (CONFIG.firmaHashes.includes(cyrb53(norm(raw)))){
    signed = true;
    cells.innerHTML = "";
    [...raw.replace(/[^A-Za-zÀ-ÿ]/g, "").toUpperCase()].forEach(ch => {
      const c = document.createElement("span");
      c.className = "cell ok"; c.textContent = ch;
      cells.appendChild(c);
    });
    $("#guess").disabled = true; $("#guessBtn").disabled = true;
    $("#signMsg").textContent = fill(CONFIG.firmaOk);
    jingle([523, 659, 784, 1047, 1319]);
    confetti();
  } else {
    fails++;
    cells.classList.remove("no"); void cells.offsetWidth; cells.classList.add("no");
    beep(180, .2, "sawtooth", .03);
    const extra = fails >= 2 ? " " + CONFIG.pistas[Math.min(fails - 2, CONFIG.pistas.length - 1)] : "";
    $("#signMsg").textContent = CONFIG.firmaMal + extra;
  }
}
$("#guessBtn").addEventListener("click", tryGuess);
$("#guess").addEventListener("keydown", e => { if (e.key === "Enter"){ e.preventDefault(); tryGuess(); } });

function openEnd(){
  active = false; clearInterval(spawnLoop);
  go("s-end");
  initSign();
  $("#cierre").textContent = fill(CONFIG.cierre);
  $("#vale").textContent = fill(CONFIG.vale);
  const st = $("#stats");
  st.innerHTML = "";
  CONFIG.stats.forEach(([k, v]) => {
    const r = document.createElement("div");
    r.className = "row";
    const a = document.createElement("span"); a.textContent = k;
    const d = document.createElement("i"); d.className = "dots";
    const b = document.createElement("b"); b.textContent = v;
    r.append(a, d, b);
    st.appendChild(r);
  });
  jingle([523, 659, 784, 659, 784, 1047]);
  confetti();
}

function confetti(){
  if (reduceMotion) return;
  const c = document.createElement("div");
  c.className = "confetti";
  const filters = ["none", "hue-rotate(35deg)", "hue-rotate(-25deg)", "saturate(.6) brightness(1.2)"];
  for (let i = 0; i < 34; i++){
    const s = document.createElement("span");
    s.innerHTML = sprite(HEART, 2 + (i % 3));
    s.style.cssText = `left:${Math.random() * 100}%;filter:${filters[i % 4]};animation-duration:${3.5 + Math.random() * 3.5}s;animation-delay:${Math.random() * 2.5}s`;
    c.appendChild(s);
  }
  document.body.appendChild(c);
  setTimeout(() => c.remove(), 11000);
}

$("#replay").addEventListener("click", () => {
  pageIdx = 0; lastWho = CONFIG.emisor; waitingChoice = false;
  beep(392, .1);
  go("s-title");
});
</script>
</body>
</html>
