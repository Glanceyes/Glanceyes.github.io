---
title: Seonho Lee
layout: home
permalink: /
description: Griffin Sunho (Seonho) Lee – Applied AI Researcher at KRAFTON AI
---

<section>
  <div class="section-title">News</div>
  <ul class="news-list">
    <li class="news-item">
      <span class="news-date">Jun. 2026</span>
      <span class="news-content">Our <a href="/publications#c8"><strong>Dense Reward for Multi-View 3D Reasoning</strong></a> paper accepted to <strong><a href="https://eccv.ecva.net/Conferences/2026" target="_blank">ECCV 2026</a></strong>.</span>
      <span class="news-tag paper">Paper</span>
    </li>
    <li class="news-item">
      <span class="news-date">May. 2026</span>
      <span class="news-content">Our <a href="/publications#c7"><strong>Perceptual Judgment Bias</strong></a> paper accepted to <strong><a href="https://icml.cc/Conferences/2026" target="_blank">ICML 2026</a></strong>.</span>
      <span class="news-tag paper">Paper</span>
    </li>
    <li class="news-item">
      <span class="news-date">Mar. 2026</span>
      <span class="news-content">I begin to work as an Applied AI Researcher at <a href="https://www.krafton.com/en/" target="_blank">KRAFTON</a> <a href="https://www.krafton.ai/en/" target="_blank">AI</a>.</span>
      <span class="news-tag career">Career</span>
    </li>
    <li class="news-item">
      <span class="news-date">Jan. 2026</span>
      <span class="news-content">Our <a href="/publications#c6"><strong>What "Not" to Detect</strong></a> paper got accepted to <strong><a href="https://iclr.cc/Conferences/2026" target="_blank">ICLR 2026</a></strong>.</span>
      <span class="news-tag paper">Paper</span>
    </li>
    <li class="news-item">
      <span class="news-date">Oct. 2025</span>
      <span class="news-content">Two papers <a href="/publications#c4"><strong>PartCATSeg</strong></a> and <a href="/publications#c5"><strong>3D-Aware VLM Finetuning</strong></a> selected as <strong><a href="https://www.qualcomm.com/research/university-relations/innovation-fellowship/2025-south-korea" target="_blank">QIFK 2025 Finalist</a></strong>.</span>
      <span class="news-tag award">Award</span>
    </li>
    <li class="news-item">
      <span class="news-date">Aug. 2025</span>
      <span class="news-content">Our <a href="/publications#c5"><strong>3D-Aware VLM Finetuning</strong></a> paper accepted to <strong><a href="https://2025.emnlp.org/" target="_blank">EMNLP 2025 Findings</a></strong>.</span>
      <span class="news-tag paper">Paper</span>
    </li>
    <li class="news-item">
      <span class="news-date">Jun. 2025</span>
      <span class="news-content">I got ML engineering internship at <a href="https://www.snap.com" target="_blank">Snap Inc.</a> in Santa Monica, CA.</span>
      <span class="news-tag career">Career</span>
    </li>
    <li class="news-item">
      <span class="news-date">Jun. 2025</span>
      <span class="news-content">I got awarded <strong><a href="https://www.msit.go.kr/bbs/view.do?sCode=user&mPid=121&mId=311&bbsSeqNo=100&nttSeqNo=3179541" target="_blank">Korean Presidential Science Scholarship for Graduate Students</a></strong>.</span>
      <span class="news-tag award">Award</span>
    </li>
    <li class="news-item">
      <span class="news-date">May. 2025</span>
      <span class="news-content">Our <a href="/publications#c3"><strong>ScribbleDiff</strong></a> paper accepted to <strong><a href="https://2025.ieeeicip.org/" target="_blank">ICIP 2025</a></strong>.</span>
      <span class="news-tag paper">Paper</span>
    </li>
    <li class="news-item">
      <span class="news-date">Feb. 2025</span>
      <span class="news-content">Our <a href="/publications#c4"><strong>PartCATSeg</strong></a> paper accepted to <strong><a href="https://cvpr.thecvf.com/Conferences/2025" target="_blank">CVPR 2025</a></strong>.</span>
      <span class="news-tag paper">Paper</span>
    </li>
    <li class="news-item">
      <span class="news-date">Jan. 2025</span>
      <span class="news-content">Our <a href="/publications#c2"><strong>DreamCatalyst</strong></a> paper accepted to <strong><a href="https://iclr.cc/Conferences/2025" target="_blank">ICLR 2025</a></strong>.</span>
      <span class="news-tag paper">Paper</span>
    </li>
  </ul>
  <div style="margin-top:12px;">
    <button class="more-news-btn" id="moreNewsBtn" onclick="toggleMoreNews()">
      <span class="more-news-arrow">▸</span> More News
    </button>
    <div class="more-news-body" id="moreNewsBody">
      <ul class="news-list" style="margin-top:12px;">
        <li class="news-item">
          <span class="news-date">Sep. 2024</span>
          <span class="news-content">Our <a href="/publications#c1"><strong>PartCLIPSeg</strong></a> paper accepted to <strong><a href="https://neurips.cc/Conferences/2024" target="_blank">NeurIPS 2024</a></strong>.</span>
          <span class="news-tag paper">Paper</span>
        </li>
        <li class="news-item">
          <span class="news-date">Sep. 2023</span>
          <span class="news-content">Joined <strong><a href="https://kaist-cvml.github.io" target="_blank">CVML Lab</a></strong> as an M.S. graduate student at <a href="https://gsai.kaist.ac.kr/" target="_blank">KAIST AI</a>.</span>
          <span class="news-tag career">Career</span>
        </li>
      </ul>
    </div>
  </div>
</section>

<section>
  <div class="section-title">Research Areas</div>
  <div class="ra-grid" id="ra-grid"></div>
</section>

<style>
.ra-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:18px;}
@media(max-width:640px){.ra-grid{grid-template-columns:1fr;}}
.ra-card{position:relative;border:1px solid var(--border);border-radius:18px;padding:16px 16px 14px;background:var(--bg);box-shadow:0 1px 2px rgba(16,24,40,.03),0 12px 30px var(--ra-glow,rgba(29,78,216,.06));transition:box-shadow .25s ease,transform .25s ease;}
.ra-card:hover{transform:translateY(-3px);box-shadow:0 0 0 1px var(--ra-ring,rgba(29,78,216,.14)),0 18px 40px var(--ra-glow,rgba(29,78,216,.13));}
.ra-head{display:flex;align-items:center;gap:9px;text-decoration:none;color:var(--text);margin:2px 2px 13px;}
.ra-ic{display:inline-flex;color:var(--ra,var(--accent));flex-shrink:0;}
.ra-ic svg{width:20px;height:20px;display:block;}
.ra-name{font-size:15px;font-weight:800;letter-spacing:-.01em;}
.ra-go{margin-left:auto;color:var(--ra,var(--accent));opacity:0;transform:translateX(-4px);transition:opacity .2s,transform .2s;font-weight:700;}
.ra-card:hover .ra-go{opacity:1;transform:translateX(0);}
.ra-slider{position:relative;border-radius:12px;overflow:hidden;border:1px solid var(--border);background:#fff;}
.ra-track{display:flex;transition:transform .55s cubic-bezier(.4,0,.2,1);}
.ra-slide{min-width:100%;display:block;position:relative;text-decoration:none;}
.ra-slide img{width:100%;height:152px;object-fit:contain;background:#fff;display:block;padding:8px;}
.ra-cap{position:absolute;left:0;right:0;bottom:0;padding:16px 12px 8px;font-size:11.5px;font-weight:600;color:#fff;background:linear-gradient(transparent,rgba(15,27,45,.75));}
.ra-dots{position:absolute;right:9px;bottom:9px;display:flex;gap:5px;z-index:2;}
.ra-dot{width:6px;height:6px;border-radius:50%;border:0;padding:0;cursor:pointer;background:rgba(255,255,255,.55);transition:background .2s,transform .2s;}
.ra-dot.on{background:#fff;transform:scale(1.3);}
.ra-note{border-radius:12px;border:1px dashed var(--border);background:var(--bg-alt);padding:34px 16px;text-align:center;font-size:13px;color:var(--text-mid);line-height:1.6;}
.ra-note b{color:var(--ra);font-weight:800;}
.ra-topics{font-size:11.5px;color:var(--text-light);margin:-6px 2px 13px;line-height:1.55;}
.ra-nav{position:absolute;top:50%;transform:translateY(-50%);z-index:3;width:30px;height:30px;border-radius:50%;border:1px solid var(--border);cursor:pointer;background:rgba(255,255,255,.9);color:var(--text-mid);display:inline-flex;align-items:center;justify-content:center;font-size:17px;line-height:1;box-shadow:0 2px 8px rgba(16,24,40,.12);opacity:0;transition:opacity .2s,background .2s,color .2s;}
.ra-slider:hover .ra-nav{opacity:1;}
.ra-nav:hover{background:#fff;color:var(--text);}
.ra-nav.prev{left:8px;}
.ra-nav.next{right:8px;}
</style>

<script>
(function(){
  var IMG='/images/about/publications/', PUB='/publications';
  var ICON={
    reasoning:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/></svg>',
    perception:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><path d="M12 2 2 7l10 5 10-5-10-5Z"/><path d="m2 17 10 5 10-5"/><path d="m2 12 10 5 10-5"/></svg>',
    generation:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><path d="M9.94 15.5A2 2 0 0 0 8.5 14.06l-6.14-1.58a.5.5 0 0 1 0-.96L8.5 9.94A2 2 0 0 0 9.94 8.5l1.58-6.14a.5.5 0 0 1 .96 0L14.06 8.5A2 2 0 0 0 15.5 9.94l6.14 1.58a.5.5 0 0 1 0 .96L15.5 14.06a2 2 0 0 0-1.44 1.44l-1.58 6.14a.5.5 0 0 1-.96 0z"/></svg>',
    agentic:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.9" stroke-linecap="round" stroke-linejoin="round"><line x1="6" x2="10" y1="11" y2="11"/><line x1="8" x2="8" y1="9" y2="13"/><line x1="15" x2="15.01" y1="12" y2="12"/><line x1="18" x2="18.01" y1="10" y2="10"/><path d="M17.32 5H6.68a4 4 0 0 0-3.978 3.59c-.006.052-.01.101-.017.152C2.604 9.416 2 14.456 2 16a3 3 0 0 0 3 3c1 0 1.5-.5 2-1l1.414-1.414A2 2 0 0 1 9.828 16h4.344a2 2 0 0 1 1.414.586L17 18c.5.5 1 1 2 1a3 3 0 0 0 3-3c0-1.544-.604-6.584-.685-7.258-.007-.05-.011-.1-.017-.152A4 4 0 0 0 17.32 5z"/></svg>'
  };
  var TOPICS={
    reasoning:'MLLM-as-a-Judge · 3D Reasoning · Negation Understanding · Visual Question Answering',
    perception:'Open-Vocabulary Part Segmentation · Detection',
    generation:'3D Editing · Text-to-Image · Video Generation',
    agentic:'Game Agents · Benchmark Construction'
  };
  var AREAS=[
    {key:'reasoning',name:'Multimodal Reasoning',color:'#1d4ed8',glow:'rgba(29,78,216,.10)',ring:'rgba(29,78,216,.20)',slides:[
      {img:'dr3d.png',href:'c8',cap:'Dense Reward &middot; ECCV 2026'},
      {img:'perceptual_judge.png',href:'c7',cap:'Perceptual Judgment &middot; ICML 2026'},
      {img:'negtome.png',href:'c6',cap:'What &ldquo;Not&rdquo; to Detect &middot; ICLR 2026'},
      {img:'3d_aware_vlm.png',href:'c5',cap:'3D-Aware VLM &middot; EMNLP 2025'},
      {img:'waymoqa.png',href:'p1',cap:'WaymoQA &middot; Preprint'}
    ]},
    {key:'perception',name:'Visual Perception',color:'#0e9384',glow:'rgba(14,147,132,.11)',ring:'rgba(14,147,132,.22)',slides:[
      {img:'partcatseg.png',href:'c4',cap:'PartCATSeg &middot; CVPR 2025'},
      {img:'partclipseg.png',href:'c1',cap:'PartCLIPSeg &middot; NeurIPS 2024'}
    ]},
    {key:'generation',name:'Generation &amp; Editing',color:'#7c3aed',glow:'rgba(124,58,237,.11)',ring:'rgba(124,58,237,.22)',slides:[
      {img:'dreamcatalyst.png',href:'c2',cap:'DreamCatalyst &middot; ICLR 2025'},
      {img:'scribblediff.png',href:'c3',cap:'Scribble-Guided Diffusion &middot; ICIP 2025'}
    ]},
    {key:'agentic',name:'Agentic AI',color:'#d97706',glow:'rgba(217,119,6,.10)',ring:'rgba(217,119,6,.20)',note:'Game agents &amp; benchmarks<br>Ongoing at <b>KRAFTON&nbsp;AI</b>'}
  ];
  var grid=document.getElementById('ra-grid');
  if(!grid) return;
  AREAS.forEach(function(a){
    var card=document.createElement('article');
    card.className='ra-card';
    card.style.setProperty('--ra',a.color);
    card.style.setProperty('--ra-glow',a.glow);
    card.style.setProperty('--ra-ring',a.ring);

    var head=document.createElement('a'); head.className='ra-head'; head.href=PUB+'#area-'+a.key;
    head.innerHTML='<span class="ra-ic">'+(ICON[a.key]||'')+'</span><span class="ra-name">'+a.name+'</span><span class="ra-go">&rarr;</span>';
    card.appendChild(head);

    var tp=TOPICS[a.key];
    if(tp){ var topicsEl=document.createElement('div'); topicsEl.className='ra-topics'; topicsEl.textContent=tp; card.appendChild(topicsEl); }

    if(a.slides && a.slides.length){
      var slider=document.createElement('div'); slider.className='ra-slider';
      var track=document.createElement('div'); track.className='ra-track';
      a.slides.forEach(function(s){
        var slide=document.createElement('a'); slide.className='ra-slide'; slide.href=PUB+'#'+s.href;
        slide.innerHTML='<img src="'+IMG+s.img+'" alt="" loading="lazy"><span class="ra-cap">'+s.cap+'</span>';
        track.appendChild(slide);
      });
      slider.appendChild(track);
      var dots=document.createElement('div'); dots.className='ra-dots'; slider.appendChild(dots);
      card.appendChild(slider);

      var idx=0,n=a.slides.length,timer=null;
      function render(){ track.style.transform='translateX(-'+(idx*100)+'%)'; var ds=dots.children; for(var i=0;i<ds.length;i++){ ds[i].className='ra-dot'+(i===idx?' on':''); } }
      function go(i){ idx=(i+n)%n; render(); restart(); }
      function next(){ go(idx+1); }
      function restart(){ if(timer){clearInterval(timer);} timer=setInterval(next,4200); }
      a.slides.forEach(function(s,i){
        var d=document.createElement('button'); d.type='button'; d.className='ra-dot'+(i===0?' on':'');
        d.addEventListener('click',function(e){ e.preventDefault(); go(i); });
        dots.appendChild(d);
      });
      var prevBtn=document.createElement('button'); prevBtn.type='button'; prevBtn.className='ra-nav prev'; prevBtn.setAttribute('aria-label','Previous'); prevBtn.innerHTML='&lsaquo;';
      prevBtn.addEventListener('click',function(e){ e.preventDefault(); e.stopPropagation(); go(idx-1); });
      var nextBtn=document.createElement('button'); nextBtn.type='button'; nextBtn.className='ra-nav next'; nextBtn.setAttribute('aria-label','Next'); nextBtn.innerHTML='&rsaquo;';
      nextBtn.addEventListener('click',function(e){ e.preventDefault(); e.stopPropagation(); go(idx+1); });
      slider.appendChild(prevBtn); slider.appendChild(nextBtn);
      slider.addEventListener('mouseenter',function(){ if(timer){clearInterval(timer);} });
      slider.addEventListener('mouseleave',restart);
      render(); restart();
    } else if(a.note){
      var note=document.createElement('div'); note.className='ra-note'; note.innerHTML=a.note;
      card.appendChild(note);
    }
    grid.appendChild(card);
  });
})();
</script>

<section>
  <div style="display:flex; align-items:baseline; justify-content:space-between;">
    <div class="section-title" style="flex:1; margin-bottom:0; border-bottom:none; padding-bottom:0;">Selected Publications</div>
    <a href="/publications" class="section-link">All Publications →</a>
  </div>
  <div style="border-top:1px solid var(--border); margin:10px 0 16px;"></div>
  <div class="pub-legend">* equal contribution · † corresponding author</div>
  <div class="pub-list">

    <div class="pub-entry">
      <img class="pub-thumb" src="/images/about/publications/dr3d.png" alt="DR3D">
      <div class="pub-body">
        <div class="pub-title">Dense Reward for Multi-View 3D Reasoning with Global Maps and Local Views</div>
        <div class="pub-authors">Jiho Choi*, <strong>Seonho Lee</strong>*, Seojeong Park, Hyunjung Shim†</div>
        <div class="pub-note"><a href="https://huggingface.co/papers/2606.23557" target="_blank">🤗 HuggingFace Daily Paper</a></div>
        <div class="pub-meta">
          <a class="venue-badge conf" href="https://eccv.ecva.net/Conferences/2026" target="_blank">ECCV 2026</a>
          <div class="pub-links">
            <a href="https://arxiv.org/abs/2606.23557" target="_blank" class="pub-link">Paper</a>
            <a href="https://dr-mv3d.github.io/" target="_blank" class="pub-link">Project</a>
            <a href="https://github.com/kaist-cvml/DR-MV3D" target="_blank" class="pub-link">Code</a>
            <a href="https://github.com/kaist-cvml/DR-MV3D" target="_blank" style="text-decoration:none; border:none;"><img src="https://img.shields.io/github/stars/kaist-cvml/DR-MV3D?style=social" alt="GitHub Stars" style="display:inline; height:18px; vertical-align:middle;"></a>
          </div>
        </div>
      </div>
    </div>

    <div class="pub-entry">
      <img class="pub-thumb" src="/images/about/publications/3d_aware_vlm.png" alt="3D-Aware VLM">
      <div class="pub-body">
        <div class="pub-title">3D-Aware Vision-Language Models Fine-Tuning with Geometric Distillation</div>
        <div class="pub-authors"><strong>Seonho Lee</strong>*, Jiho Choi*, Inha Kang, Jiwook Kim, Junsung Park, Hyunjung Shim†</div>
        <div class="pub-meta">
          <a class="venue-badge conf" href="https://2025.emnlp.org/" target="_blank">EMNLP 2025 Findings</a>
          <div class="pub-links">
            <a href="http://arxiv.org/abs/2506.09883" target="_blank" class="pub-link">Paper</a>
            <a href="https://github.com/kaist-cvml/3d-vlm-gd" target="_blank" class="pub-link">Code</a>
            <a href="https://github.com/kaist-cvml/3d-vlm-gd" target="_blank" style="text-decoration:none; border:none;"><img src="https://img.shields.io/github/stars/kaist-cvml/3d-vlm-gd?style=social" alt="GitHub Stars" style="display:inline; height:18px; vertical-align:middle;"></a>
          </div>
        </div>
      </div>
    </div>

    <div class="pub-entry">
      <img class="pub-thumb" src="/images/about/publications/partcatseg.png" alt="PartCATSeg">
      <div class="pub-body">
        <div class="pub-title">Fine-Grained Image-Text Correspondence with Cost Aggregation for Open-Vocabulary Part Segmentation</div>
        <div class="pub-authors">Jiho Choi*, <strong>Seonho Lee</strong>*, Seungho Lee, Minhyun Lee, Hyunjung Shim†</div>
        <div class="pub-meta">
          <a class="venue-badge conf" href="https://cvpr.thecvf.com/Conferences/2025" target="_blank">CVPR 2025</a>
          <div class="pub-links">
            <a href="http://arxiv.org/abs/2501.09688" target="_blank" class="pub-link">Paper</a>
            <a href="https://github.com/kaist-cvml/part-catseg" target="_blank" class="pub-link">Code</a>
            <a href="https://github.com/kaist-cvml/part-catseg" target="_blank" style="text-decoration:none; border:none;"><img src="https://img.shields.io/github/stars/kaist-cvml/part-catseg?style=social" alt="GitHub Stars" style="display:inline; height:18px; vertical-align:middle;"></a>
          </div>
        </div>
      </div>
    </div>

    <div class="pub-entry">
      <img class="pub-thumb" src="/images/about/publications/scribblediff.png" alt="ScribbleDiff">
      <div class="pub-body">
        <div class="pub-title">Scribble-Guided Diffusion for Training-free Text-to-Image Generation</div>
        <div class="pub-authors"><strong>Seonho Lee</strong>*, Jiho Choi*, Seohyun Lim, Jiwook Kim, Hyunjung Shim†</div>
        <div class="pub-meta">
          <a class="venue-badge conf" href="https://2025.ieeeicip.org/" target="_blank">ICIP 2025</a>
          <div class="pub-links">
            <a href="https://arxiv.org/abs/2409.08026" target="_blank" class="pub-link">Paper</a>
            <a href="https://github.com/kaist-cvml-lab/scribble-diffusion" target="_blank" class="pub-link">Code</a>
            <a href="https://github.com/kaist-cvml-lab/scribble-diffusion" target="_blank" style="text-decoration:none; border:none;"><img src="https://img.shields.io/github/stars/kaist-cvml-lab/scribble-diffusion?style=social" alt="GitHub Stars" style="display:inline; height:18px; vertical-align:middle;"></a>
          </div>
        </div>
      </div>
    </div>

    <div class="pub-entry">
      <img class="pub-thumb" src="/images/about/publications/dreamcatalyst.png" alt="DreamCatalyst">
      <div class="pub-body">
        <div class="pub-title">DreamCatalyst: Fast and High-Quality 3D Editing via Controlling Editability and Identity Preservation</div>
        <div class="pub-authors">Jiwook Kim*, <strong>Seonho Lee</strong>*, Jaeyo Shin, Jiho Choi, Hyunjung Shim†</div>
        <div class="pub-note"><a href="https://huggingface.co/papers/2407.11394" target="_blank">🤗 HuggingFace Daily Paper</a></div>
        <div class="pub-meta">
          <a class="venue-badge conf" href="https://iclr.cc/Conferences/2025" target="_blank">ICLR 2025</a>
          <div class="pub-links">
            <a href="https://arxiv.org/abs/2407.11394" target="_blank" class="pub-link">Paper</a>
            <a href="https://dream-catalyst.github.io/" target="_blank" class="pub-link">Project</a>
            <a href="https://github.com/kaist-cvml-lab/DreamCatalyst" target="_blank" class="pub-link">Code</a>
            <a href="https://github.com/kaist-cvml-lab/DreamCatalyst" target="_blank" style="text-decoration:none; border:none;"><img src="https://img.shields.io/github/stars/kaist-cvml-lab/DreamCatalyst?style=social" alt="GitHub Stars" style="display:inline; height:18px; vertical-align:middle;"></a>
          </div>
        </div>
      </div>
    </div>

    <div class="pub-entry">
      <img class="pub-thumb" src="/images/about/publications/partclipseg.png" alt="PartCLIPSeg">
      <div class="pub-body">
        <div class="pub-title">Understanding Multi-Granularity for Open-Vocabulary Part Segmentation</div>
        <div class="pub-authors">Jiho Choi*, <strong>Seonho Lee</strong>*, Seungho Lee, Minhyun Lee, Hyunjung Shim†</div>
        <div class="pub-meta">
          <a class="venue-badge conf" href="https://neurips.cc/Conferences/2024" target="_blank">NeurIPS 2024</a>
          <div class="pub-links">
            <a href="https://arxiv.org/abs/2406.11384" target="_blank" class="pub-link">Paper</a>
            <a href="https://github.com/kaist-cvml-lab/part-clipseg" target="_blank" class="pub-link">Code</a>
            <a href="https://github.com/kaist-cvml-lab/part-clipseg" target="_blank" style="text-decoration:none; border:none;"><img src="https://img.shields.io/github/stars/kaist-cvml-lab/part-clipseg?style=social" alt="GitHub Stars" style="display:inline; height:18px; vertical-align:middle;"></a>
          </div>
        </div>
      </div>
    </div>

  </div>
  <div style="text-align:center; margin-top:8px;">
    <a href="/publications" class="btn-all">View All Publications →</a>
  </div>
</section>

<section>
  <div class="section-title">Work Experience</div>
  <div class="exp-list">

    <div class="exp-entry">
      <a href="https://www.krafton.com/en/" target="_blank"><img class="exp-logo" src="/images/about/companies/krafton.png" alt="KRAFTON AI"></a>
      <div class="exp-body">
        <div class="exp-company">KRAFTON AI</div>
        <div class="exp-role">Applied AI Researcher</div>
        <div class="exp-meta">Mar. 2026 – Present · Seoul, Republic of Korea</div>
        <ul class="exp-bullets">
          <li>Working on agentic game development and benchmark construction</li>
          <li>Worked on training and VQA evaluation of <a href="/projects#raon">Raon-VisionEncoder</a></li>
        </ul>
      </div>
    </div>

    <div class="exp-entry">
      <a href="https://www.snap.com" target="_blank"><img class="exp-logo" src="/images/about/companies/snap.png" alt="Snap Inc."></a>
      <div class="exp-body">
        <div class="exp-company">Snap Inc.</div>
        <div class="exp-role">ML Engineer Intern <span class="exp-team">· Generative ML (VideoCraft)</span></div>
        <div class="exp-meta">Jun. 2025 – Sep. 2025 · Santa Monica, CA, USA</div>
        <ul class="exp-bullets">
          <li>Led cross-reference dataset preprocessing pipeline for personalized video generation</li>
          <li>Developed cross-reference dataset pipeline and multi-subject adapter architecture for <a href="/projects#videoalchemist">VideoAlchemist 2.0</a></li>
        </ul>
      </div>
    </div>

  </div>
</section>

<section>
  <div class="section-header-row">
    <div class="section-title">Selected Projects</div>
    <a href="/projects" class="section-link">All Projects →</a>
  </div>
  <hr class="section-divider">
  <div class="projects-grid">

    <div class="project-card has-thumb" id="raon">
      <div class="project-card-inner">
        <img class="project-thumb" src="/images/projects/raon_visionencoder.jpg" alt="Raon-VisionEncoder">
        <div style="flex:1; min-width:0;">
          <div class="project-header">
            <div class="project-title">Raon-VisionEncoder</div>
            <span class="project-org">KRAFTON AI</span>
          </div>
          <div class="project-subtitle">A Fully Open SigLIP2-class Vision Encoder</div>
          <ul class="project-bullets">
            <li>Developed a fully-open vision encoder with comparable performance to SigLIP2</li>
            <li>Contributed to VLM training pipeline integrating the vision encoder with a language model for downstream VQA evaluation and training optimization</li>
          </ul>
          <div class="project-links">
            <a href="https://krafton.ai/blog/posts/2026-04-02-raon_vision_encoder/raon-visionencoder-en.html" target="_blank" class="project-link">Blog Post</a>
            <a href="https://huggingface.co/KRAFTON/Raon-VisionEncoder" target="_blank" class="project-link">HuggingFace</a>
          </div>
        </div>
      </div>
    </div>

    <div class="project-card has-thumb" id="videoalchemist">
      <div class="project-card-inner">
        <img class="project-thumb" src="/images/projects/videoalchemist.png" alt="VideoAlchemist 2.0" style="object-fit:contain; background:#fff;">
        <div style="flex:1; min-width:0;">
          <div class="project-header">
            <div class="project-title">VideoAlchemist 2.0</div>
            <span class="project-org">Snap Inc.</span>
          </div>
          <div class="project-subtitle">Multi-Subject Personalized Video Generation</div>
          <ul class="project-bullets">
            <li>Developed a personalized video generation model with multiple subjects</li>
            <li>Built cross-reference dataset pipeline and adapter architecture for personalized video generation</li>
            <li>Contributed to foundation of dataset generation pipeline and adapter design of <a href="https://arxiv.org/abs/2512.10943" target="_blank">AlcheMinT</a></li>
          </ul>
        </div>
      </div>
    </div>

    <div class="project-card has-thumb">
      <div class="project-card-inner">
        <img class="project-thumb" src="/images/projects/3d_aware_vlm.png" alt="3D-Aware VLM Finetuning" style="object-fit:contain; background:#fff;">
        <div style="flex:1; min-width:0;">
          <div class="project-header">
            <div class="project-title">3D-Aware VLM Finetuning</div>
            <span class="project-org">Samsung Research</span>
          </div>
          <ul class="project-bullets">
            <li>Electronic Device and Method for Operating Thereof and Storage Medium</li>
            <li>Paper: <a href="/publications#c5">3D-Aware Vision-Language Models Fine-Tuning with Geometric Distillation</a></li>
          </ul>
          <div class="project-links">
            <span class="project-link" style="cursor:default; pointer-events:none; color:var(--text-light);">Korean Patent: 10-2025-0109574</span>
          </div>
        </div>
      </div>
    </div>

  </div>
  <div style="text-align:center; margin-top:8px;">
    <a href="/projects" class="btn-all">View All Projects →</a>
  </div>
</section>

<section>
  <div class="section-title">Education</div>
  <div class="edu-list">
    <div class="edu-entry">
      <div class="edu-school"><a href="https://www.kaist.ac.kr/en/" target="_blank">KAIST</a></div>
      <div class="edu-degree">M.S. in Artificial Intelligence</div>
      <div class="edu-meta">Mar. 2024 – Feb. 2026 · GPA 4.20 / 4.3</div>
    </div>
    <div class="edu-entry">
      <div class="edu-school"><a href="https://www.sogang.ac.kr/en/home" target="_blank">Sogang University</a></div>
      <div class="edu-degree">B.S. in Computer Science and Engineering</div>
      <div class="edu-meta">Mar. 2017 – Feb. 2024 · GPA 4.12 / 4.3 (<em>Summa Cum Laude</em>)</div>
    </div>
    <div class="edu-entry">
      <div class="edu-school"><a href="https://school.jbedu.kr/sangsan" target="_blank">Sangsan High School</a></div>
      <div class="edu-meta">Mar. 2014 – Feb. 2017</div>
    </div>
  </div>
</section>

<section>
  <div class="section-title">Honors &amp; Awards</div>
  <div class="honor-list">
    <div class="honor-entry">
      <div>
        <div class="honor-title">Grand Prize, <a href="http://www.ipiu.or.kr/" target="_blank">IPIU 2026</a></div>
        <div class="honor-sub">Paper: <a href="/publications#c6">What "Not" to Detect</a></div>
      </div>
      <div class="honor-date">Feb. 2026</div>
    </div>
    <div class="honor-entry">
      <div>
        <div class="honor-title"><a href="https://www.qualcomm.com/research/university-relations/innovation-fellowship/2025-south-korea" target="_blank">Qualcomm Innovation Fellowship Korea (QIFK) 2025</a> Finalist</div>
        <div class="honor-sub">Selected by Qualcomm AI Research · <a href="/publications#c4">PartCATSeg</a> &amp; <a href="/publications#c5">3D-Aware VLM Finetuning</a></div>
      </div>
      <div class="honor-date">Oct. 2025</div>
    </div>
    <div class="honor-entry">
      <div>
        <div class="honor-title">Korean Presidential Science Scholarship for <a href="https://www.msit.go.kr/bbs/view.do?sCode=user&mPid=121&mId=311&bbsSeqNo=100&nttSeqNo=3179541" target="_blank">Graduate Students</a></div>
        <div class="honor-sub">Awarded by the President of Korea</div>
      </div>
      <div class="honor-date">Jun. 2025</div>
    </div>
    <div class="honor-entry">
      <div>
        <div class="honor-title">2nd Place, <a href="https://vplow.github.io/vplow_4th.html" target="_blank">Open Vocabulary Part Segmentation Challenge</a> at CVPR 2024</div>
        <div class="honor-sub">2nd Place on both Track 1 &amp; 2 · 4th Workshop on Open World Vision (VPLOW) at CVPR 2024</div>
      </div>
      <div class="honor-date">Jun. 2024</div>
    </div>
    <div class="honor-entry">
      <div>
        <div class="honor-title">Excellence Award, 2023 <a href="https://dataen.ai/challenge/history" target="_blank">POSTECH OIBC Challenge</a></div>
        <div class="honor-sub">3rd Place (3/120) · AI Competition of Solar Power Generation Forecasting</div>
      </div>
      <div class="honor-date">Dec. 2023</div>
    </div>
    <div class="honor-entry">
      <div>
        <div class="honor-title">2022 ICPC Asia Korea Regional Contest</div>
        <div class="honor-sub">48th in Korea, 62nd in Preliminary</div>
      </div>
      <div class="honor-date">2022</div>
    </div>
    <div class="honor-entry">
      <div>
        <div class="honor-title">Dean's List, Sogang University</div>
        <div class="honor-sub">Top 1%: Spring 2018, Spring 2019, Fall 2022 · Top 5%: Fall 2018</div>
      </div>
      <div class="honor-date">2018 – 2022</div>
    </div>
    <div class="honor-entry">
      <div>
        <div class="honor-title">Korea National Science and Technology Scholarship</div>
        <div class="honor-sub">Spring 2019, Fall 2022, Spring 2023, Fall 2023 (4 Semesters)</div>
      </div>
      <div class="honor-date">2019 – 2023</div>
    </div>
  </div>
</section>

<section>
  <div class="section-title">Academic Activities</div>
  <div class="honor-list">
    <div class="honor-entry">
      <div>
        <div class="honor-title">Reviewer</div>
        <div class="honor-sub">CVPRW 2026, 3DV 2026</div>
      </div>
      <div class="honor-date">2026</div>
    </div>
  </div>
</section>
