<script>
  import garmentHtml from '$lib/garment.html?raw';
  import eigengrasHtml from '$lib/eigengrasp_session.html?raw';

  let egFrame = $state();
  let activeSession = $state('metagrasp');

  function switchSession(key) {
    activeSession = key;
    egFrame?.contentWindow.postMessage({ type: 'switch', key }, '*');
  }

  const SECTIONS = [
    { id: 's-garment',     name: 'garment_gen',    tag: 'Case Study', year: '2026' },
    { id: 's-eigengrasp',  name: 'eigengrasp',     tag: 'Tool',       year: '2026' },
    { id: 's-pill',        name: 'pill organizer', tag: 'Project',    year: '2023' },
    { id: 's-work',        name: '@3ddump',        tag: 'Work',       year: '2021–present' },
  ];

  let activeSection = $state(null);
  let showBar = $state(false);

  $effect(() => {
    const observers = [];

    const headerEl = document.getElementById('s-header');
    if (headerEl) {
      const obs = new IntersectionObserver(([entry]) => {
        showBar = !entry.isIntersecting;
      }, { threshold: 0 });
      obs.observe(headerEl);
      observers.push(obs);
    }

    const sectionEls = SECTIONS.map(s => document.getElementById(s.id)).filter(Boolean);
    if (sectionEls.length) {
      const obs = new IntersectionObserver((entries) => {
        for (const entry of entries) {
          if (entry.isIntersecting) {
            const found = SECTIONS.find(s => s.id === entry.target.id);
            if (found) activeSection = found;
          }
        }
      }, { rootMargin: '-15% 0px -75% 0px' });
      sectionEls.forEach(el => obs.observe(el));
      observers.push(obs);
    }

    return () => observers.forEach(o => o.disconnect());
  });
</script>

<!-- sticky header bar -->
<div
  class="fixed top-0 inset-x-0 z-50 transition-transform duration-200 ease-out"
  class:translate-y-0={showBar && activeSection}
  class:-translate-y-full={!showBar || !activeSection}
>
  <div class="bg-white/80 backdrop-blur-md border-b border-zinc-100">
    <div class="mx-auto max-w-[640px] px-6 h-10 grid grid-cols-3 items-center">
      <span class="text-[12px] font-medium text-zinc-900">{activeSection?.name}</span>
      <span class="text-[10px] uppercase tracking-[0.12em] text-zinc-400 text-center">{activeSection?.tag}</span>
      <span class="text-[11px] text-zinc-400 text-right">{activeSection?.year}</span>
    </div>
  </div>
</div>

<main class="mx-auto max-w-[640px] px-6 py-16 space-y-3">

  <section id="s-header" class="border border-zinc-200 rounded-xl px-5 py-4">
    <h1 class="text-xl font-medium tracking-tight text-zinc-900 mb-1.5">Samuel Tan</h1>
    <p class="text-[13px] text-zinc-900 leading-relaxed">I build tools to make things I can feel but can't yet say. Most of them end up being useful to someone else too.</p>
    <p class="text-[13px] text-zinc-400 mt-3">sam@samtan.design</p>
  </section>

  <section id="s-garment" class="border border-zinc-200 rounded-xl px-5 py-4" style="margin-top: 1.5rem;">
    <p class="text-[10px] uppercase tracking-[0.12em] text-zinc-400 mb-2">case study</p>
    <h2 class="text-[15px] font-medium text-zinc-900 mb-1">garment_gen</h2>
    <p class="text-[12px] text-zinc-400 mb-3">Built in Houdini</p>

    <div class="mb-4">
      <iframe srcdoc={garmentHtml} class="w-full rounded-lg border border-zinc-200" style="height: 480px;" title="garment_gen demo"></iframe>
    </div>

    <div class="mt-4">
      <h3 class="text-[15px] font-medium text-zinc-900 mb-1">Overview</h3>
    </div>
    <p class="text-[13px] text-zinc-900 leading-relaxed mb-4">A friend studying fashion and I both used pattern making software and hit the same wall. Powerful tools, but they ask you to think like a patternmaker before you can think like a designer. He introduced me to the sloper, the foundational pattern every garment starts from, derived from body measurements with everything else following from that. Garments already have a procedural system underneath them. I built a tool that applies the same logic anyone can use that's stable enough to build further on.</p>

    <div class="mt-4">
      <h3 class="text-[15px] font-medium text-zinc-900 mb-1">Method A</h3>
      <p class="text-[13px] text-zinc-900 leading-relaxed mb-2">Ray SOP panel projection worked, but it had a ceiling. The garment lived in world space, so any change to the avatar broke it. And placing the inputs correctly required garment knowledge the tool was meant to remove.</p>
      <video src="/videos/method_a.mp4" autoplay muted playsinline loop class="w-full rounded-lg"></video>
    </div>

    <div class="mt-4">
      <h3 class="text-[15px] font-medium text-zinc-900 mb-1">Method B</h3>
      <p class="text-[13px] text-zinc-900 leading-relaxed mb-2">Geodesic body encoding solved both. Landmarks anyone already knows, and a garment that adapts to whatever it's placed on.</p>
      <video src="/videos/method_b.mp4" autoplay muted playsinline loop class="w-full rounded-lg"></video>
    </div>

    <p class="text-[12px] text-zinc-400 text-center">Drop in your avatar, place intuitive landmarks, start designing</p>

    <div class="mt-4">
      <h3 class="text-[15px] font-medium text-zinc-900 mb-1">Reflection</h3>
      <p class="text-[13px] text-zinc-900 leading-relaxed">Method A required knowing where garments are constructed before you could design one. Method B replaced that with what designers already know about the body. Both methods informed what the tool needed to be, technically robust, but intuitive enough to stay out of the way. By grounding the system in what people already know, the simpler input and the most structurally sound approach were always the same decision.</p>
    </div>
  </section>

  <section id="s-eigengrasp" class="border border-zinc-200 rounded-xl px-5 py-4">
    <p class="text-[10px] uppercase tracking-[0.12em] text-zinc-400 mb-2">tool</p>
    <h2 class="text-[15px] font-medium text-zinc-900 mb-1">eigengrasp</h2>
    <p class="text-[13px] text-zinc-900 leading-relaxed mb-4">Most creative tools assume you know what you want before you start. Eigengrasp surfaces it first, finding the load-bearing structure of any domain before building begins. Built to establish shared ground truth between human and AI collaborators, before prompting, before anything.</p>
    <div class="flex gap-2 mb-2">
      <button
        onclick={() => switchSession('metagrasp')}
        class="text-[10px] uppercase tracking-[0.1em] px-3 py-1 rounded border transition-colors cursor-pointer bg-transparent"
        class:border-zinc-800={activeSession === 'metagrasp'}
        class:text-zinc-800={activeSession === 'metagrasp'}
        class:border-zinc-300={activeSession !== 'metagrasp'}
        class:text-zinc-400={activeSession !== 'metagrasp'}
      >metagrasp</button>
      <button
        onclick={() => switchSession('human_signals')}
        class="text-[10px] uppercase tracking-[0.1em] px-3 py-1 rounded border transition-colors cursor-pointer bg-transparent"
        class:border-zinc-800={activeSession === 'human_signals'}
        class:text-zinc-800={activeSession === 'human_signals'}
        class:border-zinc-300={activeSession !== 'human_signals'}
        class:text-zinc-400={activeSession !== 'human_signals'}
      >human signals</button>
    </div>
    <iframe
      bind:this={egFrame}
      srcdoc={eigengrasHtml}
      class="w-full rounded-lg"
      style="height: 600px;"
      title="eigengrasp session replay"
    ></iframe>
  </section>

  <section id="s-pill" class="border border-zinc-200 rounded-xl px-5 py-4">
    <p class="text-[10px] uppercase tracking-[0.12em] text-zinc-400 mb-2">project</p>
    <h2 class="text-[15px] font-medium text-zinc-900 mb-1">pill organizer</h2>
    <p class="text-[13px] text-zinc-900 leading-relaxed mb-4">A self-directed brief for a pill organizer in the $20–30 range, designed for people who take their health seriously but want objects that reflect their taste. Explored bi-stable hinges, magnetic lids, removable containers, and slide-out forms across multiple sketch iterations. Material studies in wood, metal, and plastic. Two directions, one minimal, one modular.</p>
    <img src="/pill_sketch.webp" alt="pill organizer ideation sketches" class="w-full rounded-lg mb-2 object-cover" />
    <div class="grid grid-cols-2 gap-2">
      <img src="/pill_black.webp" alt="pill organizer black render" class="w-full rounded-lg object-cover" />
      <img src="/pill_white.webp" alt="pill organizer white render" class="w-full rounded-lg object-cover" />
    </div>
  </section>

  <section id="s-work" class="border border-zinc-200 rounded-xl px-5 py-4" style="margin-top: 1.5rem;">
    <p class="text-[13px] text-zinc-900 leading-relaxed mb-3">
      <a href="https://instagram.com/3ddump" target="_blank" rel="noopener" class="text-zinc-400 hover:text-zinc-600 transition-colors">@3ddump</a>
    </p>
    <div class="grid grid-cols-3 gap-2">
      {#each ['/work1.jpg','/work2.jpg','/work3.jpg','/work4.jpg','/work5.jpg','/work6.png'] as src}
        <div class="aspect-square">
          <img {src} alt="" class="w-full h-full rounded-lg object-cover" />
        </div>
      {/each}
    </div>
    <p class="mt-4 text-[13px] text-zinc-900 leading-relaxed">
      <span class="text-[10px] uppercase tracking-[0.12em] text-zinc-400 mr-3">Recognition</span><span class="text-zinc-400">Antoni Tudisco · Aus Taylor</span>
    </p>
  </section>

</main>
