<script>
  import QuantumWaveOrbital from "./QuantumWaveOrbital.svelte";
  import { onMount } from "svelte";
  import "./styles.css";
  import Particles, { particlesInit } from "@tsparticles/svelte";
  import { loadSlim } from "@tsparticles/slim";
  import particlesConfig from "./particles-config.js";
  import {config} from "./config";


  let { name, title, linkedinUrl, githubUrl, calendarUrl, photo } = config;

  let particlesContainer;
  onMount(async () => {
    const randomHex = () => {
      let n = (Math.random() * 0xfffff * 1000000).toString(16);
      return "#" + n.slice(0, 6);
    };
    const color1 = randomHex();
    const color2 = randomHex();

    document.body.style.backgroundImage = `linear-gradient(to bottom, ${color1}, ${color2})`;

    // Initialize particles
    try {
      await particlesInit(particlesConfig);
    } catch (e) {}
  });

  let onParticlesLoaded = (event) => {
    particlesContainer = event.detail.particles;
  };

  void particlesInit(async (engine) => {
    try {
      await loadSlim(engine);
    } catch (e) {}
  });
  function handleMouseMove(event) {
    try {
      if (particlesContainer && particlesContainer.addParticle) {
        particlesContainer.addParticle({
          x: event.clientX,
          y: event.clientY,
        });
      }
    } catch (e) {}
  }

  function handleContact() {
    window.open(calendarUrl, "_blank");
  }
</script>

<svelte:head>
  <title>{name}</title>

  <link rel="icon" type="image/png" href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8AAAAP0lEQVR42mNgYGD4Twz6jAwMDP8xiYOAkRUXd4yM7IAak/FpfBqfxqfxaXwak/FpfHICH+FT/yQqKyqAKiahCwAA2TwMqx7NKRUAAAAASUVORK5CYII=">

</svelte:head>

<div class="background">
  <Particles
    style="z-index: -1;"
    on:mousemove={handleMouseMove}
    id="tsparticles"
    options={particlesConfig}
    on:particlesLoaded={onParticlesLoaded}
    class="absolute inset-0"
  />
  <div class="card">
    <div class="flex items-center justify-center h-full">
      <img src={photo} alt="photo" class="rounded-full mx-auto h-[120px] w-[120px]" />
    </div>
    <h1>{name}</h1>
    <p>{title}</p>

    <button
      on:click={handleContact}
      class="group relative mx-auto grid place-items-center overflow-hidden rounded-xl px-4 py-2 shadow-[0_1000px_0_0_hsl(0_0%_20%)_inset] transition-colors duration-200"
    >
      <span>
        <span
          class="spark mask-gradient animate-flip before:animate-kitrotate absolute inset-0 h-[100%] w-[100%] overflow-hidden rounded-xl [mask:linear-gradient(white,_transparent_50%)] before:absolute before:aspect-square before:w-[200%] before:rotate-[-90deg] before:bg-[conic-gradient(from_0deg,transparent_0_340deg,white_360deg)] before:content-[''] before:[inset:0_auto_auto_50%] before:[translate:-50%_-15%]"
        />
      </span>
      <span
        class="backdrop absolute inset-px rounded-[11px] bg-neutral-950 transition-colors duration-200 group-hover:bg-neutral-900"
      />
      <span class="z-10 text-neutral-400 text-sm font-medium">
        <slot>
          <svg
            class="w-6 h-6 mr-2 inline-block"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
            xmlns="http://www.w3.org/2000/svg"
            ><path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M15 10l4.553-2.276A1 1 0 0121 8.618v6.764a1 1 0 01-1.447.894L15 14M5 18h8a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v8a2 2 0 002 2z"
            ></path></svg
          >
          Schedule Call
        </slot>
      </span>
    </button>
    <div class="social-links">
      <a href={linkedinUrl} class="social-icon linkedin group">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
          <rect x="0" y="0" width="24" height="24" fill="#0077B5" />
          <g fill="#FFFFFF">
            <path
              d="M5 9h3v11H5zM6.5 7.5a1.75 1.75 0 1 1 0-3.5 1.75 1.75 0 0 1 0 3.5zM13 9h3v1.5c.4-.8 1.6-1.5 2.5-1.5 2.7 0 3.5 1.7 3.5 4.5V20h-3v-6c0-1.2-.2-2.5-1.5-2.5S15 12.7 15 14v6h-3V9z"
            />
          </g>
        </svg>
        <span
          class="highlight group-hover:bg-white group-hover:shadow-lg rounded-full"
        ></span>
      </a>
      <a href={githubUrl} class="social-icon github group">
        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"
          ><path
            d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"
          /></svg
        >
        <span
          class="highlight group-hover:bg-white group-hover:shadow-lg rounded-full"
        ></span>
      </a>
    </div>

    <QuantumWaveOrbital />
  </div>
</div>
