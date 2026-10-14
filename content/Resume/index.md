+++
title = "Resume"
[extra]
+++

<style>
/* These styles are loaded only on the resume page. */
#main-content {
  width: min(960px, calc(100% - 2rem));
  margin: 8rem auto 4rem;
}
#main-content > h1 {
  margin: 0 0 0.5rem;
  font-size: clamp(2.25rem, 6vw, 3.5rem);
  line-height: 1.1;
}
.resume-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 1rem;
  margin: 0 0 1.75rem;
}
.resume-header p { margin: 0; }
.resume-actions { display: flex; flex-wrap: wrap; gap: 0.75rem; }
.resume-actions a {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 44px;
  padding: 0.65rem 1.1rem;
  border: 1px solid currentColor;
  border-radius: 999px;
  font-size: 0.9rem;
  font-weight: 600;
  text-decoration: none;
}
.resume-actions a:hover { background: var(--fg-muted-1); }
.resume-frame {
  display: block;
  width: 100%;
  height: auto;
  aspect-ratio: 8.5 / 11;
  margin: 0;
  border: 0;
  border-radius: 8px;
  background: white;
  box-shadow: 0 12px 40px rgba(0, 0, 0, 0.12);
}
.resume-help { margin: 1rem 0 0; font-size: 0.85rem; opacity: 0.75; }
@media (max-width: 600px) {
  #main-content { margin-top: 7rem; }
  .resume-header { align-items: flex-start; }
  .resume-frame { height: 70vh; aspect-ratio: auto; }
}
</style>

<div class="resume-header">
  <p>Experience, education &amp; technical skills.</p>
  <div class="resume-actions">
    <a href="/Docs/Lucca%20Correia%20Resume.pdf" target="_blank" rel="noopener">Open PDF ↗</a>
    <a href="/Docs/Lucca%20Correia%20Resume.pdf" download>Download resume ↓</a>
  </div>
</div>
<iframe class="resume-frame" title="Lucca Correia’s resume" src="/Docs/Lucca%20Correia%20Resume.pdf#toolbar=0&navpanes=0&view=FitH"></iframe>
<p class="resume-help">Having trouble with the preview? Open the PDF above to view the full resume.</p>
