+++
title = "About Me"
+++

<style>
/* --- knobs --- */
:root{
  --COL: 520px;            /* width of each column */
  --GAP: 8px;             /* gap between columns (was 24px) */
  --PAD: clamp(16px, 4vw, 48px); /* side padding to match site feel */
  --RIGHTPAD: 30px;        /* extra room on the RIGHT to nudge ski pic */
}

/* break out of the centered article container and use viewport width */
.full-bleed{
  width: 100vw;
  position: relative;
  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;
  padding-left: var(--PAD);
  padding-right: var(--PAD);
  overflow-x: auto;           /* if window is narrower than 3*COL + 2*GAP */
}

/* fixed-width stage aligned left (with extra right pad) */
.stage{
  width: calc(3 * var(--COL) + 2 * var(--GAP) + var(--RIGHTPAD));
  margin: 0;                  /* left-aligned (not centered) */
  box-sizing: border-box;
}

.header{ margin:0 0 8px 0; text-align:left; }
.subtitle{
  margin: 10px 0 24px;
  color:#7f7165;
  font-weight:600;
  font-size:20px;
  line-height:1.3;
}

.row{ display:flex; gap:var(--GAP); align-items:flex-start; justify-content:flex-start; }
.col{ flex:0 0 var(--COL); }
.col-right{ margin-left:auto; }   /* push the ski column to the right edge */

img{ display:block; width:100%; height:auto; border-radius:12px; }

.col p{ margin:0 0 14px 0; line-height:1.55; }

.pair{ display:grid; grid-template-columns:1fr 1fr; column-gap:36px; row-gap:8px; margin-top:8px; }
.pair img{ width:100%; height:auto; border-radius:10px; }
</style>

<div class="full-bleed">
<div class="stage">

<h1 class="header" style="margin:0; display:inline-flex; align-items:center; gap:10px;">
  Lucca Correia
  <a href="https://www.linkedin.com/in/luccaec/" target="_blank" aria-label="LinkedIn">
    <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 32 32" fill="#7f7165"><path d="M29.637 0H2.363C1.057 0 0 1.057 0 2.363v27.274C0 30.944 1.057 32 2.363 32h27.274c1.305 0 2.363-1.056 2.363-2.363V2.363C32 1.057 30.944 0 29.637 0zM9.764 25.452H5.86V12.763h3.904v12.689zM7.812 11.265c-1.186 0-2.147-.951-2.147-2.12 0-1.171.961-2.122 2.147-2.122 1.171 0 2.12.951 2.12 2.122 0 1.169-.949 2.12-2.12 2.12zm17.679 14.187h-3.902v-6.293c0-1.497-.026-3.418-2.074-3.418-2.078 0-2.397 1.617-2.397 3.288v6.423h-3.903v-12.689h3.744v1.728h.05c.522-.981 1.798-2.015 3.698-2.015 3.957 0 4.684 2.604 4.684 5.98v6.996z"/></svg>
  </a>
  <a href="mailto:lec254@cornell.edu" target="_blank" aria-label="Email" style="display:inline-flex; align-items:center; transform:translateY(4px);">
    <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" fill="#7f7165"><path d="M20 4H4C2.895 4 2 4.895 2 6v12c0 1.105.895 2 2 2h16c1.105 0 2-.895 2-2V6c0-1.105-.895-2-2-2zm-1.35 4.693l-5.823 4.506a2 2 0 0 1-2.654 0L5.35 8.693a.75.75 0 0 1 .9-1.186L12 11l5.75-3.493a.75.75 0 1 1 .9 1.186z"/></svg>
  </a>
</h1>

<div class="subtitle">Senior at Cornell University pursuing a B.S. in Mechanical Engineering and M.Eng in Systems Engineering</div>

<div class="row">
  <div class="col">
    <img src="/FoundationBot.jpeg#no-hover#start" alt="Foundation Bot"
         style="width:480px; height:auto; border-radius:12px; flex-shrink:0;">
  </div>

  <div class="col">
    <p>At Cornell, I work in the <a href="https://helbling-lab.github.io/" target="_blank">Helbling Robotics Research Lab</a>, where I contribute to the design of autonomous insect-scale robots capable of sustained operation in real-world environments. I also served as the Mechanical Subteam Lead for Nexus, a Cornell Project Team developing an autonomous beach-cleaning rover to remove microplastics from sand. These experiences have enhanced my ability to fuse mechanical design with system-level integration and management through close collaboration with multidisciplinary engineering teams.</p>
    <p>This past summer, I interned at Foundation Robotics, a humanoid robotics company in San Francisco, where I contributed to the design, build, and scaling of next-generation humanoid systems. I also previously worked at The MITRE Corporation, where I modeled and analyzed defense aircraft components to enhance cooling performance and system reliability.</p>
    <p>Outside the lab, I am an active member of the Cornell United Soccer Club Team and a lifelong competitive USSA and FIS ski racer.</p>
    <div class="pair">
      <img src="/ProfilePicture.jpg#no-hover#start" alt="Profile Picture">
      <img src="/TeamPic.png#no-hover#start"   alt="Team Picture">
    </div>
  </div>

  <div class="col col-right">
    <img src="/Jackson.jpeg#no-hover#start" alt="Ski Picture"
         style="width:480px; height:auto; border-radius:12px; flex-shrink:0;">
  </div>
</div>

</div>
</div>
