<template>
  <div
    class="shell"
    :class="[
      `lang-${language}`,
      `theme-${theme}`,
      { menu: menuOpen, scrolled, ready: !loading },
    ]"
    :dir="dir"
    :lang="language"
  >
    <a class="skip" href="#main">{{
      language === "fa" ? "پرش به محتوای اصلی" : "Skip to content"
    }}</a>

    <!-- LOADER: Lottie buildings -->
    <Transition name="fade">
      <div v-if="loading" class="loader" role="status" aria-live="polite">
        <div ref="lottieRef" class="ld-lottie" aria-hidden="true"></div>

        <strong class="ld-brand">مشاورین املاک صدف</strong>

        <p>{{ t("loader") }}</p>

        <div class="ld-bar">
          <span :style="{ width: progress + '%' }" />
        </div>

        <em>{{ num(progress) }}٪</em>
      </div>
    </Transition>

    <!-- HEADER -->
    <header class="header">
      <div class="container header-in">
        <a class="brand" href="#about" @click="closeMenu">
          <svg
            class="logo"
            viewBox="0 0 40 40"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
          >
            <rect class="lg-bg" x="0.5" y="0.5" width="39" height="39" rx="9" />
            <path
              class="lg-l"
              pathLength="1"
              d="M15 32V9H25V32M9 32V21H15M25 25H31V32M6 32H34"
            />
            <rect
              v-for="(w, i) in logoWins"
              :key="i"
              class="lg-w"
              :class="{ on: w.on }"
              :style="{ '--i': i }"
              :x="w.x"
              :y="w.y"
              width="2.6"
              height="2.6"
            />
          </svg>

          <span>{{ brand }}</span>
        </a>

        <nav ref="navRef" class="nav" aria-label="Primary">
          <span class="nav-pill" :style="pillStyle" />

          <a
            v-for="n in navItems"
            :key="n.id"
            :href="`#${n.id}`"
            :class="{ active: activeSection === n.id }"
            :aria-current="activeSection === n.id ? 'true' : undefined"
            @click="activeSection = n.id"
          >
            {{ n.label[language] }}
          </a>
        </nav>

        <div class="actions">
          <button
            class="chip desk-lang"
            type="button"
            :aria-label="language === 'en' ? 'فارسی' : 'English'"
            @click="toggleLanguage"
          >
            {{ language === "en" ? "FA" : "EN" }}
          </button>

          <button
            class="tsw"
            type="button"
            role="switch"
            :aria-checked="theme === 'dark'"
            :aria-label="theme === 'dark' ? 'Light' : 'Dark'"
            @click="toggleTheme"
          >
            <svg class="ti ti-sun" viewBox="0 0 24 24" aria-hidden="true">
              <circle cx="12" cy="12" r="4" />
              <path
                d="M12 2.5V5M12 19V21.5M2.5 12H5M19 12H21.5M5.3 5.3l1.8 1.8M16.9 16.9l1.8 1.8M5.3 18.7l1.8-1.8M16.9 7.1l1.8-1.8"
              />
            </svg>
            <svg class="ti ti-moon" viewBox="0 0 24 24" aria-hidden="true">
              <path d="M20 14.2A8.5 8.5 0 0 1 9.8 4a8.6 8.6 0 1 0 10.2 10.2z" />
            </svg>
            <span class="tk" />
          </button>

          <a class="btn btn-sm desk" href="#contact">
            {{ t("header.talk") }}
          </a>

          <button
            class="burger"
            type="button"
            :aria-expanded="menuOpen"
            aria-label="Menu"
            aria-controls="mobile-menu"
            @click="menuOpen = !menuOpen"
          >
            <span /><span />
          </button>
        </div>
      </div>
    </header>

    <!-- MOBILE MENU -->
    <Transition name="fade">
      <div
        v-if="menuOpen"
        id="mobile-menu"
        class="mmenu"
        role="dialog"
        aria-modal="true"
        @click.self="closeMenu"
      >
        <div class="mpanel">
          <a
            v-for="n in navItems"
            :key="n.id"
            :href="`#${n.id}`"
            @click="closeMenu"
          >
            {{ n.label[language] }}
            <span>↗</span>
          </a>

          <button class="mlang" type="button" @click="toggleLanguage">
            <span>{{ language === "en" ? "فارسی" : "English" }}</span>
            <b>{{ language === "en" ? "FA" : "EN" }}</b>
          </button>

          <a class="btn" href="#contact" @click="closeMenu">
            {{ t("header.talk") }}
          </a>
        </div>
      </div>
    </Transition>

    <main id="main">
      <!-- HERO -->
      <section
        id="about"
        class="hero"
        @pointermove="onPointer"
        @pointerleave="resetPointer"
      >
        <div class="container hero-grid">
          <div class="hero-copy">
            <span class="eyebrow">{{ t("hero.eyebrow") }}</span>

            <h1>
              <span class="ln" style="--l: 0"
                ><span
                  >{{ t("hero.line1") }}
                  <span class="grad">{{ t("hero.line2") }}</span></span
                ></span
              >
              <span class="ln" style="--l: 1"
                ><span>{{ t("hero.line3") }} {{ t("hero.line4") }}</span></span
              >
            </h1>

            <p class="lead">{{ t("hero.description") }}</p>

            <div class="hero-cta">
              <a class="btn" href="#contact">{{ t("hero.cta") }}</a>

              <a class="btn ghost" href="#services">
                {{ t("services.caption") }}
              </a>
            </div>

            <div class="stats">
              <div v-for="s in stats" :key="s.key">
                <strong> {{ num(animated[s.key]) }}+ </strong>

                <span>
                  {{ s.label[language] }}
                </span>
              </div>
            </div>
          </div>

          <!-- HERO ART: architectural elevation -->
          <div class="art-wrap" aria-hidden="true">
            <div class="art">
              <svg
                class="arc"
                viewBox="0 0 600 540"
                preserveAspectRatio="xMidYMid meet"
                xmlns="http://www.w3.org/2000/svg"
              >
                <defs>
                  <pattern
                    id="arc-grid"
                    width="40"
                    height="40"
                    patternUnits="userSpaceOnUse"
                  >
                    <path
                      d="M40 0H0V40"
                      fill="none"
                      style="stroke: var(--a-grid)"
                      stroke-width="1"
                    />
                  </pattern>
                  <radialGradient id="arc-fade" cx="50%" cy="46%" r="62%">
                    <stop offset="0" stop-color="#fff" />
                    <stop offset="1" stop-color="#000" />
                  </radialGradient>
                  <mask id="arc-grid-mask">
                    <rect width="600" height="540" fill="url(#arc-fade)" />
                  </mask>
                  <pattern
                    id="arc-far"
                    width="6"
                    height="6"
                    patternUnits="userSpaceOnUse"
                  >
                    <rect width="6" height="6" style="fill: var(--a-far)" />
                    <path
                      d="M0 0V6"
                      style="stroke: var(--a-far-line)"
                      stroke-width="1"
                    />
                  </pattern>
                  <pattern
                    id="arc-near"
                    width="6"
                    height="6"
                    patternUnits="userSpaceOnUse"
                  >
                    <rect width="6" height="6" style="fill: var(--a-near)" />
                    <path
                      d="M0 0V6"
                      style="stroke: var(--a-near-line)"
                      stroke-width="1"
                    />
                  </pattern>
                  <linearGradient id="arc-sky" x1="0" x2="0" y1="0" y2="1">
                    <stop offset="0" style="stop-color: var(--sky-a)" />
                    <stop offset="0.8" style="stop-color: var(--sky-b)" />
                  </linearGradient>
                  <linearGradient id="arc-tower" x1="0" x2="1" y1="0" y2="0">
                    <stop offset="0" style="stop-color: var(--a-paper)" />
                    <stop offset="1" style="stop-color: var(--a-paper2)" />
                  </linearGradient>
                  <linearGradient id="arc-ground" x1="0" x2="0" y1="0" y2="1">
                    <stop offset="0" style="stop-color: var(--a-ground)" />
                    <stop offset="1" style="stop-color: var(--a-ground2)" />
                  </linearGradient>
                  <linearGradient id="arc-refl" x1="0" x2="0" y1="0" y2="1">
                    <stop
                      offset="0"
                      style="stop-color: var(--a-ink)"
                      stop-opacity="0.22"
                    />
                    <stop
                      offset="1"
                      style="stop-color: var(--a-ink)"
                      stop-opacity="0"
                    />
                  </linearGradient>
                  <linearGradient id="arc-mist" x1="0" x2="1" y1="0" y2="0">
                    <stop
                      offset="0"
                      style="stop-color: var(--a-cloud, #fff)"
                      stop-opacity="0"
                    />
                    <stop
                      offset="0.5"
                      style="stop-color: var(--a-cloud, #fff)"
                      stop-opacity="0.75"
                    />
                    <stop
                      offset="1"
                      style="stop-color: var(--a-cloud, #fff)"
                      stop-opacity="0"
                    />
                  </linearGradient>
                  <linearGradient id="arc-glint" x1="0" x2="1" y1="0" y2="0">
                    <stop offset="0" stop-color="#fff" stop-opacity="0" />
                    <stop offset="0.5" stop-color="#fff" stop-opacity="0.5" />
                    <stop offset="1" stop-color="#fff" stop-opacity="0" />
                  </linearGradient>
                  <radialGradient id="arc-glare" cx="50%" cy="50%" r="50%">
                    <stop offset="0" stop-color="#fff" stop-opacity="0.28" />
                    <stop offset="1" stop-color="#fff" stop-opacity="0" />
                  </radialGradient>
                  <linearGradient id="arc-haze" x1="0" x2="0" y1="0" y2="1">
                    <stop
                      offset="0"
                      style="stop-color: var(--sky-b)"
                      stop-opacity="0"
                    />
                    <stop
                      offset="1"
                      style="stop-color: var(--sky-b)"
                      stop-opacity="0.7"
                    />
                  </linearGradient>
                  <mask id="arc-moon-mask">
                    <rect x="400" y="50" width="110" height="110" fill="#fff" />
                    <circle cx="464" cy="95" r="25" fill="#000" />
                  </mask>
                  <clipPath id="arc-tower-clip">
                    <rect x="210" y="142" width="180" height="288" />
                  </clipPath>
                </defs>

                <!-- panel -->
                <rect class="arc-panel" width="600" height="540" rx="10" />
                <rect
                  class="arc-gridfill"
                  width="600"
                  height="430"
                  fill="url(#arc-grid)"
                  mask="url(#arc-grid-mask)"
                />

                <!-- sun / moon: swapped by theme -->
                <g class="orb sun">
                  <g class="arc-sun">
                    <circle cx="452" cy="104" r="52" class="ring-o" />
                    <circle cx="452" cy="104" r="34" class="ring-i" />
                    <circle cx="452" cy="104" r="34" class="disc" />
                  </g>
                </g>
                <g class="orb moon">
                  <g class="arc-sun">
                    <circle cx="452" cy="104" r="52" class="ring-o" />
                    <circle
                      cx="452"
                      cy="104"
                      r="30"
                      class="moon-body"
                      mask="url(#arc-moon-mask)"
                    />
                  </g>
                </g>

                <!-- clouds -->
                <g class="arc-clouds">
                  <rect
                    class="cloud cl1"
                    x="60"
                    y="76"
                    width="118"
                    height="3"
                    rx="1.5"
                  />
                  <rect
                    class="cloud cl2"
                    x="96"
                    y="92"
                    width="64"
                    height="3"
                    rx="1.5"
                  />
                  <rect
                    class="cloud cl3"
                    x="300"
                    y="58"
                    width="92"
                    height="3"
                    rx="1.5"
                  />
                </g>

                <g class="birds">
                  <path class="bird b1" d="M0 0q5 -5 10 0q5 -5 10 0" />
                  <path class="bird b2" d="M0 0q4 -4 8 0q4 -4 8 0" />
                  <path class="bird b3" d="M0 0q3 -3 6 0q3 -3 6 0" />
                </g>

                <!-- skyline (parallax) -->
                <g class="px px-far">
                  <rect
                    v-for="(b, i) in farSky"
                    :key="'f' + i"
                    class="rise"
                    :style="{ '--d': (0.5 + i * 0.09).toFixed(2) + 's' }"
                    :x="b.x"
                    :y="430 - b.h"
                    :width="b.w"
                    :height="b.h"
                    fill="url(#arc-far)"
                  />
                </g>
                <g class="px px-near">
                  <rect
                    v-for="(b, i) in nearSky"
                    :key="'n' + i"
                    class="rise"
                    :style="{ '--d': (0.8 + i * 0.1).toFixed(2) + 's' }"
                    :x="b.x"
                    :y="430 - b.h"
                    :width="b.w"
                    :height="b.h"
                    fill="url(#arc-near)"
                  />
                </g>

                <rect
                  class="mist"
                  x="0"
                  y="366"
                  width="380"
                  height="72"
                  rx="36"
                  fill="url(#arc-mist)"
                />

                <rect
                  class="haze"
                  x="0"
                  y="330"
                  width="600"
                  height="100"
                  fill="url(#arc-haze)"
                />

                <!-- ground plane + reflection -->
                <rect
                  class="fade"
                  x="0"
                  y="430"
                  width="600"
                  height="110"
                  fill="url(#arc-ground)"
                  style="--d: 0.2s"
                />
                <g class="fade" style="--d: 2.4s">
                  <rect
                    x="210"
                    y="430"
                    width="180"
                    height="96"
                    fill="url(#arc-refl)"
                  />
                  <rect
                    x="96"
                    y="430"
                    width="114"
                    height="60"
                    fill="url(#arc-refl)"
                    opacity="0.7"
                  />
                  <rect
                    x="390"
                    y="430"
                    width="110"
                    height="52"
                    fill="url(#arc-refl)"
                    opacity="0.7"
                  />
                </g>
                <path
                  class="paving fade"
                  style="--d: 2.2s"
                  d="M0 452H600M0 476H600M0 506H600"
                />

                <g class="fade" style="--d: 2.6s">
                  <path class="cast" d="M210 430H390L520 472H340Z" />
                </g>

                <!-- building -->
                <g class="px px-mid">
                  <!-- wings -->
                  <rect
                    class="rise"
                    style="--d: 1s"
                    x="96"
                    y="322"
                    width="114"
                    height="108"
                    fill="var(--a-paper2)"
                  />
                  <rect
                    class="rise"
                    style="--d: 1.1s"
                    x="390"
                    y="358"
                    width="110"
                    height="72"
                    fill="var(--a-paper2)"
                  />
                  <path
                    class="line draw"
                    style="--d: 0.4s; --t: 1.3s"
                    pathLength="1"
                    d="M210 322H96V430"
                  />
                  <path
                    class="line draw"
                    style="--d: 0.5s; --t: 1.3s"
                    pathLength="1"
                    d="M390 358H500V430"
                  />
                  <path
                    class="line-thin draw"
                    style="--d: 1.3s; --t: 1s"
                    pathLength="1"
                    d="M90 394H210M90 358H210M390 394H506"
                  />
                  <path
                    class="line-thin fade"
                    style="--d: 2.1s"
                    d="M112 322V302M194 322V302M104 302H202M112 302v-4M128 302v-4M144 302v-4M160 302v-4M176 302v-4M192 302v-4"
                  />
                  <path
                    class="line-thin fade"
                    style="--d: 2.1s"
                    d="M392 350H498M400 350v8M416 350v8M432 350v8M448 350v8M464 350v8M480 350v8M496 350v8"
                  />

                  <!-- tower -->
                  <rect
                    class="rise"
                    style="--d: 1.1s; --t: 1.5s"
                    x="210"
                    y="142"
                    width="180"
                    height="288"
                    fill="url(#arc-tower)"
                  />
                  <rect
                    class="fade"
                    style="--d: 2s"
                    x="374"
                    y="142"
                    width="16"
                    height="288"
                    fill="var(--a-ink)"
                    opacity="0.06"
                  />
                  <rect
                    class="fade"
                    style="--d: 1.9s"
                    x="246"
                    y="124"
                    width="108"
                    height="18"
                    fill="var(--a-paper2)"
                  />
                  <path
                    class="line draw"
                    style="--d: 0.3s; --t: 1.7s"
                    pathLength="1"
                    d="M210 430V142H390V430"
                  />
                  <path
                    class="line draw"
                    style="--d: 0.9s; --t: 0.9s"
                    pathLength="1"
                    d="M246 142V124H354V142"
                  />
                  <path
                    class="line-thin fade"
                    style="--d: 2.1s"
                    d="M262 124V142M278 124V142M294 124V142M310 124V142M326 124V142M342 124V142"
                  />
                  <path
                    class="line-thin draw"
                    style="--d: 0.9s; --t: 1.5s"
                    pathLength="1"
                    :d="slabs"
                  />
                  <path
                    class="crown draw"
                    style="--d: 1.2s; --t: 0.9s"
                    pathLength="1"
                    d="M204 142H396"
                  />

                  <!-- glazing -->
                  <g class="glass fade" style="--d: 1.8s">
                    <rect
                      v-for="w in windows"
                      :key="'g' + w.k"
                      :x="w.x"
                      :y="w.y"
                      width="30"
                      height="20"
                    />
                  </g>
                  <g clip-path="url(#arc-tower-clip)">
                    <rect
                      v-for="w in windows"
                      :key="'l' + w.k"
                      class="lit"
                      :class="{
                        on: w.lit,
                        live: w.lit && w.live,
                        'live-on': !w.lit && w.live,
                      }"
                      :style="w.style"
                      :x="w.x"
                      :y="w.y"
                      width="30"
                      height="20"
                    />
                  </g>
                  <path class="mullion fade" style="--d: 1.9s" :d="mullions" />

                  <!-- entrance -->
                  <rect
                    class="fade"
                    style="--d: 1.8s"
                    x="282"
                    y="400"
                    width="36"
                    height="30"
                    fill="var(--a-glass)"
                  />
                  <rect
                    class="lit on"
                    style="--d: 3.3s"
                    x="282"
                    y="400"
                    width="36"
                    height="30"
                  />
                  <path
                    class="mullion fade"
                    style="--d: 1.9s"
                    d="M300 400V430"
                  />
                  <path
                    class="crown draw"
                    style="--d: 2.6s; --t: 0.7s"
                    pathLength="1"
                    d="M270 394H330"
                  />

                  <!-- glint + pointer glare -->
                  <g clip-path="url(#arc-tower-clip)">
                    <rect
                      class="glint"
                      x="250"
                      y="142"
                      width="46"
                      height="288"
                      fill="url(#arc-glint)"
                      transform="skewX(-14)"
                    />
                    <g class="glare">
                      <ellipse
                        cx="300"
                        cy="270"
                        rx="120"
                        ry="150"
                        fill="url(#arc-glare)"
                      />
                    </g>
                  </g>

                  <!-- service tour -->
                  <g class="tour">
                    <g class="ti0" :style="{ '--o': tour[0].o }">
                      <rect
                        class="hl"
                        x="210"
                        y="142"
                        width="180"
                        height="288"
                      />
                      <path class="ld" d="M172 196H222" />
                      <circle class="dot" cx="222" cy="196" r="2.6" />
                    </g>
                    <g class="ti1" :style="{ '--o': tour[1].o }">
                      <rect
                        class="hl"
                        x="96"
                        y="322"
                        width="114"
                        height="108"
                      />
                      <path class="ld" d="M108 268V336" />
                      <circle class="dot" cx="108" cy="336" r="2.6" />
                    </g>
                    <g class="ti2" :style="{ '--o': tour[2].o }">
                      <rect
                        class="hl"
                        x="390"
                        y="358"
                        width="110"
                        height="72"
                      />
                      <path class="ld" d="M445 318V376" />
                      <circle class="dot" cx="445" cy="376" r="2.6" />
                    </g>
                  </g>
                </g>

                <!-- ground line -->
                <path
                  class="ground draw"
                  style="--d: 0.1s; --t: 1.2s"
                  pathLength="1"
                  d="M0 430H600"
                />

                <!-- cypress -->
                <g class="cypress">
                  <ellipse
                    class="rise"
                    style="--d: 2.3s"
                    cx="64"
                    cy="396"
                    rx="7"
                    ry="34"
                  />
                  <ellipse
                    class="rise"
                    style="--d: 2.45s"
                    cx="80"
                    cy="404"
                    rx="6"
                    ry="26"
                  />
                  <ellipse
                    class="rise"
                    style="--d: 2.6s"
                    cx="512"
                    cy="400"
                    rx="7"
                    ry="30"
                  />
                </g>
                <!-- dimension -->
                <g class="dim">
                  <path
                    class="draw"
                    style="--d: 2.5s; --t: 1s"
                    pathLength="1"
                    d="M528 430V142"
                  />
                  <path
                    class="fade"
                    style="--d: 3.2s"
                    d="M522 436L534 424M522 148L534 136M396 142H536"
                  />
                  <text
                    class="fade"
                    style="--d: 3.3s"
                    x="528"
                    y="128"
                    text-anchor="middle"
                  >
                    +31.50
                  </text>
                  <text
                    class="fade"
                    style="--d: 3.3s"
                    x="528"
                    y="452"
                    text-anchor="middle"
                  >
                    ±0.00
                  </text>
                </g>
              </svg>

              <div
                v-for="(t, i) in tour"
                :key="t.k"
                class="tag"
                :class="`tg${i}`"
                :style="{ '--o': t.o }"
              >
                <i />
                {{ t.label[language] }}
              </div>

              <div class="tblock">
                <span class="tb-mark" />
                <p>
                  <b>{{ t("hero.product.title") }}</b>
                  <small>
                    {{
                      language === "fa"
                        ? "املاک صدف · تهران"
                        : "Sadaf Estate · Tehran"
                    }}
                  </small>
                </p>
              </div>
            </div>
          </div>
        </div>
        <a
          class="scroll-cue"
          href="#about-2"
          :aria-label="language === 'fa' ? 'ادامه' : 'Scroll'"
          ><i
        /></a>
      </section>

      <!-- ABOUT -->
      <section id="about-2" class="sec">
        <div class="container split">
          <span class="cap">{{ t("about.caption") }}</span>

          <div>
            <h2>
              {{ t("about.title.line1") }}
              <em>{{ t("about.title.line2") }}</em>
              {{ t("about.title.line3") }}
            </h2>

            <p class="body">
              {{ t("about.description") }}
            </p>
          </div>
        </div>
      </section>

      <!-- SERVICES -->
      <section id="services" class="sec alt">
        <div class="container">
          <span class="cap">{{ t("services.caption") }}</span>

          <div class="cards">
            <article v-for="s in services" :key="s.icon" class="card reveal">
              <span class="ico">{{ s.icon }}</span>

              <h3>{{ s.title[language] }}</h3>

              <p>{{ s.description[language] }}</p>
            </article>
          </div>
        </div>
      </section>

      <!-- EXPERTISE -->
      <section id="expertise" class="sec">
        <div class="container split">
          <span class="cap">
            {{ t("expertise.caption") }}
          </span>

          <div>
            <h2>
              {{ t("expertise.title.line1") }}
              <em>{{ t("expertise.title.line2") }}</em>
            </h2>

            <p class="body">
              {{ t("expertise.description") }}
            </p>

            <div
              class="tags"
              @mouseenter="tagsPaused = true"
              @mouseleave="tagsPaused = false"
            >
              <span
                v-for="tag in tags"
                :key="tag"
                :class="{ on: activeTags.includes(tag) }"
              >
                {{ t(`expertise.tags.${tag}`) }}
              </span>
            </div>
          </div>
        </div>
      </section>

      <!-- CTA -->
      <section class="sec">
        <div class="container">
          <div class="cta reveal">
            <div>
              <h2>
                {{ t("cta.title.line1") }}
                <em>{{ t("cta.title.line2") }}</em>
              </h2>

              <p class="body">
                {{ t("cta.description") }}
              </p>
            </div>

            <a class="btn" href="#contact">
              {{ t("cta.button") }}
            </a>
          </div>
        </div>
      </section>

      <!-- CONTACT -->
      <section id="contact" class="sec">
        <div class="container contact">
          <div>
            <span class="cap">
              {{ t("office.caption") }}
            </span>

            <h2>
              {{ t("office.title.line1") }}
              <em>{{ t("office.title.line2") }}</em>
            </h2>

            <div class="offices">
              <article v-for="o in offices" :key="o.k" class="card">
                <small>
                  {{ t(`office.${o.label}`) }}
                </small>

                <h3>
                  {{ t(`office.${o.name}`) }}
                </h3>

                <p>
                  {{ t(`office.${o.addr}`) }}
                </p>

                <a :href="`tel:${o.raw}`">
                  {{ t(`office.${o.phone}`) }}
                </a>

                <a :href="`mailto:${email}`">
                  {{ email }}
                </a>
              </article>
            </div>
          </div>

          <!-- OFFICE LOTTIE -->
          <div class="cvis">
            <div
              ref="officeLottieRef"
              class="office-lottie"
              aria-hidden="true"
            ></div>

            <a :href="`mailto:${email}`">
              {{ email }}
            </a>
          </div>
        </div>
      </section>
    </main>

    <!-- FOOTER -->
    <footer class="footer">
      <div class="container">
        <div class="f-top">
          <div class="f-brand">
            <a class="brand" href="#about">
              <svg
                class="logo"
                viewBox="0 0 40 40"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
              >
                <rect
                  class="lg-bg"
                  x="0.5"
                  y="0.5"
                  width="39"
                  height="39"
                  rx="9"
                />
                <path
                  class="lg-l"
                  pathLength="1"
                  d="M15 32V9H25V32M9 32V21H15M25 25H31V32M6 32H34"
                />
                <rect
                  v-for="(w, i) in logoWins"
                  :key="i"
                  class="lg-w"
                  :class="{ on: w.on }"
                  :style="{ '--i': i }"
                  :x="w.x"
                  :y="w.y"
                  width="2.6"
                  height="2.6"
                />
              </svg>

              <span>{{ brand }}</span>
            </a>

            <p>
              {{ t("footer.description") }}
            </p>

            <small>
              {{ t("footer.copyright") }}
            </small>
          </div>

          <div class="f-cols">
            <div
              v-for="c in footerColumns"
              :key="c.key"
              class="f-col"
              :class="{ open: openFooter === c.key }"
            >
              <button
                type="button"
                :aria-expanded="openFooter === c.key"
                @click="openFooter = openFooter === c.key ? null : c.key"
              >
                {{ t(c.title) }}
                <span>⌄</span>
              </button>

              <div class="f-links">
                <a v-for="l in c.links" :key="l.label" :href="l.href">
                  {{ t(l.label) }}
                </a>
              </div>
            </div>
          </div>
        </div>

        <div class="f-bottom">
          <span>{{ t("footer.bottom") }}</span>
        </div>
      </div>
    </footer>

    <button
      class="to-top"
      :class="{ show: scrolled }"
      type="button"
      aria-label="Back to top"
      @click="scrollTop"
    >
      ↑
    </button>
  </div>
</template>

<script setup>
import {
  computed,
  nextTick,
  onBeforeUnmount,
  onMounted,
  reactive,
  ref,
  watch,
} from "vue";
import lottie from "lottie-web";
import buildingsAnimation from "./assets/animations/buildings.json";
import solarPoweredHouseAnimation from "./assets/animations/Solar Powered House.json";

const language = ref("fa");
const theme = ref("light");
const loading = ref(true);
const progress = ref(0);
const menuOpen = ref(false);
const scrolled = ref(false);
const activeSection = ref("about");
const openFooter = ref(null);

const animated = reactive({
  experience: 0,
  projects: 0,
  clients: 0,
});

const lottieRef = ref(null);
const officeLottieRef = ref(null);

let lottieInstance = null;
let officeLottieInstance = null;
let revealIO, sectionIO, raf, loadRaf;

const dir = computed(() => (language.value === "fa" ? "rtl" : "ltr"));

const brand = computed(() =>
  language.value === "fa" ? "املاک صدف" : "Sadaf Estate",
);

const num = (n) =>
  Number(n).toLocaleString(language.value === "fa" ? "fa-IR" : "en-US");

/* ---------- DATA ---------- */

const logoWins = [
  { x: 16.6, y: 12, on: true },
  { x: 20.8, y: 12, on: false },
  { x: 16.6, y: 16.5, on: false },
  { x: 20.8, y: 16.5, on: true },
  { x: 16.6, y: 21, on: true },
  { x: 20.8, y: 21, on: false },
  { x: 16.6, y: 25.5, on: false },
  { x: 20.8, y: 25.5, on: true },
  { x: 10.7, y: 24.5, on: true },
  { x: 26.7, y: 27.5, on: false },
];

const navItems = [
  {
    id: "about",
    label: { en: "About", fa: "درباره ما" },
  },
  {
    id: "services",
    label: { en: "Services", fa: "خدمات" },
  },
  {
    id: "expertise",
    label: { en: "Expertise", fa: "تخصص" },
  },
  {
    id: "contact",
    label: { en: "Contact", fa: "تماس" },
  },
];

const services = [
  {
    icon: "🏡",
    title: {
      en: "Buying & Selling",
      fa: "خرید و فروش ملک",
    },
    description: {
      en: "Complete support for buying and selling residential, commercial and office properties.",
      fa: "پشتیبانی کامل برای خرید و فروش املاک مسکونی، تجاری و اداری.",
    },
  },
  {
    icon: "📈",
    title: {
      en: "Investment Consulting",
      fa: "مشاوره سرمایه‌گذاری",
    },
    description: {
      en: "Identifying opportunities and analyzing return on real estate investments.",
      fa: "شناسایی فرصت‌های سرمایه‌گذاری و تحلیل بازده در بازار املاک.",
    },
  },
  {
    icon: "🔑",
    title: {
      en: "Rent & Lease",
      fa: "اجاره و رهن",
    },
    description: {
      en: "The right rental for tenants and reliable tenants for owners.",
      fa: "یافتن ملک مناسب برای مستأجرین و مستأجر مطمئن برای مالکین.",
    },
  },
  {
    icon: "📐",
    title: {
      en: "Valuation & Appraisal",
      fa: "کارشناسی و ارزیابی",
    },
    description: {
      en: "Accurate and fair valuation based on real market data.",
      fa: "ارزیابی دقیق و منصفانه املاک بر اساس داده‌های واقعی بازار.",
    },
  },
  {
    icon: "⚖️",
    title: {
      en: "Legal Consulting",
      fa: "مشاوره حقوقی املاک",
    },
    description: {
      en: "Legal advice on contracts, documentation and transactions.",
      fa: "مشاوره حقوقی در مورد قراردادها، اسناد و معاملات ملکی.",
    },
  },
  {
    icon: "🏢",
    title: {
      en: "Property Management",
      fa: "مدیریت املاک",
    },
    description: {
      en: "Full management of residential and commercial properties for owners.",
      fa: "مدیریت کامل املاک مسکونی و تجاری به نمایندگی از مالکین.",
    },
  },
];

const tags = [
  "Residential",
  "Commercial",
  "Luxury",
  "Investment",
  "Rentals",
  "Land",
  "Villas",
  "Offices",
  "Legal",
];

const tagShift = ref(0);
const tagsPaused = ref(false);
const activeTags = computed(() =>
  [0, 2, 3].map((i) => tags[(i + tagShift.value) % tags.length]),
);
let tagTimer;

const stats = [
  {
    key: "experience",
    target: 12,
    label: {
      en: "Years experience",
      fa: "سال تجربه",
    },
  },
  {
    key: "projects",
    target: 500,
    label: {
      en: "Successful deals",
      fa: "معامله موفق",
    },
  },
  {
    key: "clients",
    target: 350,
    label: {
      en: "Happy clients",
      fa: "مشتری راضی",
    },
  },
];

const email = "info@sadaf-estate.com";

const offices = [
  {
    k: 1,
    label: "headquarters",
    name: "tehran",
    addr: "address1",
    phone: "phone1",
    raw: "+982122345678",
  },
  {
    k: 2,
    label: "branch",
    name: "tehranBranch",
    addr: "address2",
    phone: "phone2",
    raw: "+982122678901",
  },
];

const footerColumns = [
  {
    key: "c",
    title: "footer.COMPANY",
    links: [
      {
        label: "aboutLink",
        href: "#about-2",
      },
      {
        label: "serviceLink1",
        href: "#services",
      },
    ],
  },
  {
    key: "s",
    title: "footer.SERVICES",
    links: [1, 2, 3, 4].map((n) => ({
      label: `serviceLink${n}`,
      href: "#services",
    })),
  },
  {
    key: "r",
    title: "footer.RESOURCES",
    links: [
      {
        label: "resourceLink1",
        href: "#expertise",
      },
      {
        label: "resourceLink2",
        href: "#expertise",
      },
      {
        label: "resourceLink3",
        href: "#contact",
      },
    ],
  },
];

const translations = {
  en: {
    loader: "Getting your new home ready…",

    header: {
      talk: "Contact us",
    },

    hero: {
      eyebrow: "Professional real estate consultants",
      line1: "Find your",
      line2: "dream property",
      line3: "with",
      line4: "Sadaf Estate.",
      description:
        "With years of experience in the housing market, we give expert advice on buying, selling, renting and investing in real estate.",
      cta: "Free consultation",
      product: {
        title: "Specialized real estate consulting",
      },
    },

    about: {
      caption: "About us",
      title: {
        line1: "Professional",
        line2: "real estate expertise",
        line3: "for your next move.",
      },
      description:
        "With years of experience in the Tehran housing market, we are your trusted partner in property transactions — precise market knowledge, transparent pricing and expert consultation.",
    },

    services: {
      caption: "Our services",
    },

    expertise: {
      caption: "Our expertise",
      title: {
        line1: "Expertise",
        line2: "that delivers.",
      },
      description:
        "Deep knowledge of Tehran's neighborhoods and property types means the right solution for your purchase, sale or investment.",
      tags: {
        Residential: "Residential",
        Commercial: "Commercial",
        Luxury: "Luxury",
        Investment: "Investment",
        Rentals: "Rentals",
        Land: "Land",
        Villas: "Villas",
        Offices: "Offices",
        Legal: "Legal",
      },
    },

    cta: {
      title: {
        line1: "Looking for",
        line2: "the right property?",
      },
      description:
        "Contact us for a free consultation about buying, selling or investing.",
      button: "Contact us",
    },

    office: {
      caption: "Our office",
      title: {
        line1: "Let's find",
        line2: "your perfect property.",
      },
      headquarters: "Main office",
      branch: "Branch",
      tehran: "Tehran office",
      tehranBranch: "Sadaf branch",
      address1: "Tehran, Saadat Abad, Darya Blvd., Motahari St., No. 12",
      address2: "Tehran, Zafaraniyeh, Moghaddas Ardabili St., No. 45, Floor 2",
      phone1: "+98-21-22345678",
      phone2: "+98-21-22678901",
    },

    footer: {
      description:
        "Sadaf Real Estate Consultants — your trusted partner in buying, selling, renting and investing.",
      copyright: "© Sadaf Estate 2025",
      bottom: "Specialized real estate consulting & investment",
      COMPANY: "Company",
      SERVICES: "Services",
      RESOURCES: "Resources",
    },

    aboutLink: "About us",
    serviceLink1: "Buy consulting",
    serviceLink2: "Sell consulting",
    serviceLink3: "Property valuation",
    serviceLink4: "Rent & lease",
    resourceLink1: "Blog",
    resourceLink2: "FAQ",
    resourceLink3: "Contact us",
  },

  fa: {
    loader: "در حال آماده‌سازی خانه‌ی جدید شما…",

    header: {
      talk: "تماس با ما",
    },

    hero: {
      eyebrow: "مشاورین املاک حرفه‌ای",
      line1: "با مشاورین",
      line2: "املاک صدف",
      line3: "ملک رویایی",
      line4: "خود را بیابید.",
      description:
        "مشاورین املاک صدف با سال‌ها تجربه در بازار مسکن، مشاوره تخصصی برای خرید، فروش، اجاره و سرمایه‌گذاری در املاک ارائه می‌دهند.",
      cta: "مشاوره رایگان",
      product: {
        title: "مشاوره تخصصی املاک",
      },
    },

    about: {
      caption: "درباره ما",
      title: {
        line1: "تخصص",
        line2: "حرفه‌ای املاک",
        line3: "برای معامله بعدی شما.",
      },
      description:
        "مشاورین املاک صدف با سال‌ها تجربه در بازار مسکن تهران، همراه مطمئن شما در معاملات ملکی است. ما با شناخت دقیق بازار، قیمت‌گذاری شفاف و مشاوره تخصصی، بهترین گزینه‌ها را پیشنهاد می‌دهیم.",
    },

    services: {
      caption: "خدمات ما",
    },

    expertise: {
      caption: "تخصص ما",
      title: {
        line1: "تخصصی",
        line2: "که نتیجه می‌دهد.",
      },
      description:
        "با شناخت دقیق مناطق مختلف تهران و تخصص در انواع ملک، راهکار مناسب برای خرید، فروش یا سرمایه‌گذاری شما ارائه می‌کنیم.",
      tags: {
        Residential: "مسکونی",
        Commercial: "تجاری",
        Luxury: "لوکس",
        Investment: "سرمایه‌گذاری",
        Rentals: "اجاره",
        Land: "زمین",
        Villas: "ویلا",
        Offices: "اداری",
        Legal: "حقوقی",
      },
    },

    cta: {
      title: {
        line1: "به دنبال ملک",
        line2: "مناسب هستید؟",
      },
      description:
        "برای دریافت مشاوره رایگان در مورد خرید، فروش یا سرمایه‌گذاری با ما تماس بگیرید.",
      button: "تماس با ما",
    },

    office: {
      caption: "دفتر ما",
      title: {
        line1: "بیایید ملک",
        line2: "مناسب شما را پیدا کنیم.",
      },
      headquarters: "دفتر مرکزی",
      branch: "شعبه",
      tehran: "دفتر تهران",
      tehranBranch: "شعبه صدف",
      address1: "تهران، سعادت‌آباد، بلوار دریا، خیابان مطهری، پلاک ۱۲",
      address2: "تهران، زعفرانیه، خیابان مقدس اردبیلی، پلاک ۴۵، طبقه ۲",
      phone1: "۰۲۱-۲۲۳۴۵۶۷۸",
      phone2: "۰۲۱-۲۲۶۷۸۹۰۱",
    },

    footer: {
      description:
        "مشاورین املاک صدف؛ همراه مطمئن شما در خرید، فروش، اجاره و سرمایه‌گذاری املاک.",
      copyright: "© املاک صدف ۱۴۰۴",
      bottom: "مشاوره تخصصی خرید، فروش و سرمایه‌گذاری املاک",
      COMPANY: "شرکت",
      SERVICES: "خدمات",
      RESOURCES: "منابع",
    },

    aboutLink: "درباره ما",
    serviceLink1: "مشاوره خرید",
    serviceLink2: "مشاوره فروش",
    serviceLink3: "کارشناسی و ارزیابی ملک",
    serviceLink4: "اجاره و رهن",
    resourceLink1: "وبلاگ",
    resourceLink2: "سوالات متداول",
    resourceLink3: "تماس با ما",
  },
};

const t = (path) =>
  path.split(".").reduce((v, p) => v?.[p], translations[language.value]) ?? "";

/* ---------- HELPERS ---------- */

const setMeta = (name, content) => {
  const attr = /^og:/.test(name) ? "property" : "name";

  let el = document.head.querySelector(`meta[${attr}="${name}"]`);

  if (!el) {
    el = document.createElement("meta");
    el.setAttribute(attr, name);
    document.head.appendChild(el);
  }

  el.setAttribute("content", content);
};

const setLink = (rel, href) => {
  let el = document.head.querySelector(`link[rel="${rel}"]`);

  if (!el) {
    el = document.createElement("link");
    el.setAttribute("rel", rel);
    document.head.appendChild(el);
  }

  el.setAttribute("href", href);
};

const setJsonLd = (data) => {
  let el = document.getElementById("ld-json");

  if (!el) {
    el = document.createElement("script");
    el.id = "ld-json";
    el.type = "application/ld+json";
    document.head.appendChild(el);
  }

  el.textContent = JSON.stringify(data);
};

const updateSeo = () => {
  const fa = language.value === "fa";

  document.documentElement.lang = fa ? "fa" : "en";
  document.documentElement.dir = dir.value;

  document.title = fa
    ? "املاک صدف | مشاوره تخصصی خرید، فروش و سرمایه‌گذاری ملک"
    : "Sadaf Estate | Specialized Real Estate Consulting";

  const desc = fa
    ? "مشاورین املاک صدف با سال‌ها تجربه در بازار مسکن تهران، خدمات مشاوره خرید، فروش، اجاره، کارشناسی و سرمایه‌گذاری املاک ارائه می‌دهند."
    : "Sadaf Real Estate Consultants: buying, selling, renting, valuation and investment in the Tehran housing market.";

  setMeta("description", desc);
  setMeta("og:title", document.title);
  setMeta("og:description", desc);
  setMeta("og:locale", fa ? "fa_IR" : "en_US");

  const url = location.origin + location.pathname;
  const img = `${location.origin}/og-cover.jpg`;
  const name = fa ? "املاک صدف" : "Sadaf Estate";

  setMeta("robots", "index, follow, max-image-preview:large, max-snippet:-1");
  setMeta("og:type", "website");
  setMeta("og:site_name", name);
  setMeta("og:url", url);
  setMeta("og:image", img);
  setMeta("og:image:alt", name);
  setMeta("twitter:card", "summary_large_image");
  setMeta("twitter:title", document.title);
  setMeta("twitter:description", desc);
  setMeta("twitter:image", img);
  setLink("canonical", url);

  setJsonLd({
    "@context": "https://schema.org",
    "@graph": [
      {
        "@type": "RealEstateAgent",
        "@id": `${url}#org`,
        name,
        url,
        image: img,
        description: desc,
        areaServed: { "@type": "City", name: "Tehran" },
        address: {
          "@type": "PostalAddress",
          addressLocality: "Tehran",
          addressCountry: "IR",
        },
      },
      {
        "@type": "WebSite",
        "@id": `${url}#site`,
        url,
        name,
        inLanguage: fa ? "fa-IR" : "en",
      },
    ],
  });
};

const applyTheme = () => {
  document.documentElement.dataset.theme = theme.value;
  document.documentElement.style.colorScheme = theme.value;

  setMeta("theme-color", theme.value === "dark" ? "#0b1220" : "#ffffff");
};

const save = (k, v) => {
  try {
    localStorage.setItem(k, v);
  } catch {}
};

const toggleTheme = () => {
  theme.value = theme.value === "dark" ? "light" : "dark";

  save("sadaf-theme", theme.value);
  applyTheme();
};

const toggleLanguage = async () => {
  language.value = language.value === "en" ? "fa" : "en";

  save("sadaf-language", language.value);

  await nextTick();

  updateSeo();
};

const closeMenu = () => {
  menuOpen.value = false;
};

const scrollTop = () => {
  window.scrollTo({
    top: 0,
    behavior: "smooth",
  });
};

/* ---------- LOTTIE LOADER ---------- */

const initLottie = () => {
  if (!lottieRef.value) return;

  lottieInstance?.destroy();

  lottieInstance = lottie.loadAnimation({
    container: lottieRef.value,
    renderer: "svg",
    loop: true,
    autoplay: true,
    animationData: buildingsAnimation,
    rendererSettings: {
      preserveAspectRatio: "xMidYMid meet",
    },
  });
};

/* ---------- OFFICE LOTTIE ---------- */

const initOfficeLottie = () => {
  if (!officeLottieRef.value) return;

  officeLottieInstance?.destroy();

  officeLottieInstance = lottie.loadAnimation({
    container: officeLottieRef.value,
    renderer: "svg",
    loop: true,
    autoplay: true,
    animationData: solarPoweredHouseAnimation,
    rendererSettings: {
      preserveAspectRatio: "xMidYMid meet",
    },
  });
};

/* ---------- NAV PILL ---------- */

const navRef = ref(null);

const pill = reactive({
  x: 0,
  w: 0,
  on: false,
});

const pillStyle = computed(() => ({
  width: pill.w + "px",
  transform: `translateX(${pill.x}px)`,
  opacity: pill.on ? 1 : 0,
}));

const updatePill = () => {
  const a = navRef.value?.querySelector("a.active");

  if (!a || !a.offsetWidth) {
    pill.on = false;
    return;
  }

  pill.x = a.offsetLeft;
  pill.w = a.offsetWidth;
  pill.on = true;
};

watch([activeSection, language], () => nextTick(updatePill));

/* ---------- POINTER PARALLAX ---------- */

const onPointer = (e) => {
  if (window.innerWidth < 900) return;

  const r = e.currentTarget.getBoundingClientRect();

  e.currentTarget.style.setProperty(
    "--mx",
    (((e.clientX - r.left) / r.width - 0.5) * 2).toFixed(2),
  );

  e.currentTarget.style.setProperty(
    "--my",
    (((e.clientY - r.top) / r.height - 0.5) * 2).toFixed(2),
  );
};

const resetPointer = (e) => {
  e.currentTarget.style.setProperty("--mx", 0);
  e.currentTarget.style.setProperty("--my", 0);
};

/* ---------- HERO ARCHITECTURE (SVG elevation) ---------- */

const GROUND = 430;
const FLOOR_H = 36;
const WIN_W = 30;
const WIN_H = 20;

// deterministic pseudo-random: the same "occupancy" pattern on every load
const hash = (n) => {
  const x = Math.sin(n * 91.7 + 13.1) * 43758.5453;
  return x - Math.floor(x);
};

const buildWindows = ({ x0, w, floors, bays, seed, skip }) => {
  const gap = (w - bays * WIN_W) / (bays + 1);
  const out = [];

  for (let f = 0; f < floors; f++) {
    for (let b = 0; b < bays; b++) {
      if (skip?.(f, b)) continue;

      const r = hash(seed + f * 7 + b * 3);
      const lit = r > 0.52;

      out.push({
        k: `${seed}-${f}-${b}`,
        x: +(x0 + gap + b * (WIN_W + gap)).toFixed(1),
        y: GROUND - (f + 1) * FLOOR_H + 8,
        lit,
        live: lit ? hash(seed + f + b * 9) > 0.86 : r < 0.08,
        style: {
          "--d":
            (2 + f * 0.17 + b * 0.05 + hash(seed + f * 3 + b) * 0.45).toFixed(
              2,
            ) + "s",
          "--p": (9 + hash(seed + b + f * 2) * 6).toFixed(1) + "s",
        },
      });
    }
  }

  return out;
};

const windows = [
  ...buildWindows({
    x0: 210,
    w: 180,
    floors: 8,
    bays: 4,
    seed: 3,
    skip: (f, b) => f === 0 && (b === 1 || b === 2),
  }),
  ...buildWindows({ x0: 96, w: 114, floors: 3, bays: 2, seed: 11 }),
  ...buildWindows({ x0: 390, w: 110, floors: 2, bays: 2, seed: 19 }),
];

const mullions = windows
  .map((w) => `M${w.x + WIN_W / 2} ${w.y}v${WIN_H}`)
  .join("");

const slabs = Array.from({ length: 7 }, (_, i) => {
  const y = GROUND - (i + 1) * FLOOR_H;

  return `M204 ${y}H396`;
}).join("");

const tour = [
  { k: "invest", o: "5.5s", label: { fa: "سرمایه‌گذاری", en: "Investment" } },
  { k: "rent", o: "9.5s", label: { fa: "اجاره و رهن", en: "Rent & lease" } },
  { k: "sale", o: "13.5s", label: { fa: "خرید و فروش", en: "Buy & sell" } },
];

const farSky = [
  { x: 18, w: 44, h: 176 },
  { x: 66, w: 36, h: 244 },
  { x: 106, w: 50, h: 150 },
  { x: 160, w: 40, h: 214 },
  { x: 396, w: 40, h: 196 },
  { x: 440, w: 34, h: 262 },
  { x: 478, w: 48, h: 168 },
  { x: 534, w: 38, h: 226 },
  { x: 574, w: 30, h: 150 },
];

const nearSky = [
  { x: 0, w: 58, h: 118 },
  { x: 60, w: 34, h: 82 },
  { x: 552, w: 48, h: 128 },
];

/* ---------- COUNTERS ---------- */

const animateCounters = () => {
  const start = performance.now();

  const tick = (now) => {
    const p = Math.min((now - start) / 1500, 1);

    const e = 1 - Math.pow(1 - p, 3);

    stats.forEach((s) => (animated[s.key] = Math.round(s.target * e)));

    if (p < 1) {
      raf = requestAnimationFrame(tick);
    }
  };

  raf = requestAnimationFrame(tick);
};

/* ---------- LOADER PROGRESS ---------- */

const runLoader = () => {
  const start = performance.now();
  const dur = 3000;

  const tick = (now) => {
    const p = Math.min((now - start) / dur, 1);

    progress.value = Math.round((1 - Math.pow(1 - p, 2)) * 100);

    if (p < 1) {
      loadRaf = requestAnimationFrame(tick);

      return;
    }

    setTimeout(() => {
      lottieInstance?.destroy();
      lottieInstance = null;

      loading.value = false;

      document.body.classList.remove("lock");

      nextTick(observe);
    }, 250);
  };

  loadRaf = requestAnimationFrame(tick);
};

/* ---------- REVEAL ---------- */

const observe = () => {
  revealIO?.disconnect();

  revealIO = new IntersectionObserver(
    (es) =>
      es.forEach((e) => {
        if (e.isIntersecting) {
          e.target.classList.add("in");
          revealIO.unobserve(e.target);
        }
      }),
    {
      threshold: 0.12,
    },
  );

  document
    .querySelectorAll(".reveal:not(.in)")
    .forEach((el) => revealIO.observe(el));
};

/* ---------- MOUNT ---------- */

const onKey = (e) => {
  if (e.key === "Escape") closeMenu();
};

onMounted(async () => {
  try {
    const l = localStorage.getItem("sadaf-language");

    const th = localStorage.getItem("sadaf-theme");

    if (l === "fa" || l === "en") {
      language.value = l;
    }

    theme.value =
      th === "dark" || th === "light"
        ? th
        : window.matchMedia("(prefers-color-scheme: dark)").matches
          ? "dark"
          : "light";
  } catch {}

  applyTheme();
  updateSeo();

  document.body.classList.add("lock");

  const onScroll = () => {
    scrolled.value = window.scrollY > 18;

    const h = document.documentElement;

    h.style.setProperty(
      "--sp",
      (h.scrollTop / (h.scrollHeight - h.clientHeight || 1)).toFixed(4),
    );

    h.style.setProperty(
      "--hs",
      Math.min(window.scrollY / (window.innerHeight * 0.9), 1).toFixed(3),
    );
  };

  window.addEventListener("scroll", onScroll, {
    passive: true,
  });

  onScroll();

  window._sadafScroll = onScroll;

  await nextTick();

  initLottie();
  initOfficeLottie();

  sectionIO = new IntersectionObserver(
    (es) => {
      const v = es
        .filter((e) => e.isIntersecting)
        .sort((a, b) => b.intersectionRatio - a.intersectionRatio)[0];

      if (v) {
        activeSection.value = v.target.id === "about-2" ? "about" : v.target.id;
      }
    },
    {
      threshold: [0.15, 0.4],
      rootMargin: "-20% 0px -50% 0px",
    },
  );

  ["about-2", "services", "expertise", "contact"].forEach((id) => {
    const el = document.getElementById(id);

    el && sectionIO.observe(el);
  });

  observe();

  setTimeout(animateCounters, 2300);

  runLoader();

  window.addEventListener("resize", updatePill);

  window.addEventListener("keydown", onKey);

  if (!window.matchMedia("(prefers-reduced-motion: reduce)").matches) {
    tagTimer = setInterval(() => {
      if (!document.hidden && !tagsPaused.value) {
        tagShift.value = (tagShift.value + 1) % tags.length;
      }
    }, 2600);
  }

  document.fonts?.ready.then(updatePill);

  nextTick(updatePill);
});

/* ---------- UNMOUNT ---------- */

onBeforeUnmount(() => {
  window.removeEventListener("scroll", window._sadafScroll);

  window.removeEventListener("resize", updatePill);

  window.removeEventListener("keydown", onKey);

  clearInterval(tagTimer);

  revealIO?.disconnect();
  sectionIO?.disconnect();

  cancelAnimationFrame(raf);
  cancelAnimationFrame(loadRaf);

  lottieInstance?.destroy();
  lottieInstance = null;

  officeLottieInstance?.destroy();
  officeLottieInstance = null;

  document.body.classList.remove("lock");
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,700&family=Inter:wght@400;500;600&family=Vazirmatn:wght@400;500;700;800&display=swap");

/* ============ TOKENS ============ */

:root {
  --container: 1200px;
  --r-sm: 12px;
  --r-md: 18px;
  --r-lg: 26px;
  --font: "Inter", system-ui, sans-serif;
  --font-d: "Fraunces", Georgia, serif;
  --font-fa: "Vazirmatn", "Inter", sans-serif;
  --ease: cubic-bezier(0.22, 1, 0.36, 1);

  font-family: var(--font);
  -webkit-font-smoothing: antialiased;
  text-rendering: optimizeLegibility;
}

/* روشن */

[data-theme="light"] {
  --bg: #ffffff;
  --bg2: #f6f6f3;
  --panel: #ffffff;
  --text: #14213d;
  --text2: #465066;
  --text3: #667085;
  --gold: #c9971c;
  --gold-hi: #e5b640;
  --gold-lo: #9c7010;
  --gold-t: #85600a;
  --mint: #3a9d8f;
  --line: rgba(133, 96, 10, 0.22);
  --soft: rgba(20, 33, 61, 0.08);
  --shadow: 0 18px 46px rgba(20, 33, 61, 0.1);
  --btn-bg: #14213d;
  --btn-fg: #fff;
  --btn-hover: #22345c;
  --btn-shadow: rgba(20, 33, 61, 0.22);

  --sky-a: #dbe8f4;
  --sky-b: #fbf5e4;
  --hill-a: #b8d3a5;
  --hill-b: #a2c48e;
  --wall: #fffdf7;
  --wall-line: #d9c48f;
  --roof-a: #e3b548;
  --roof-b: #b98615;
  --win: #bfe0f2;
  --win-glow: rgba(120, 180, 220, 0.35);
  --door: #2c3e68;
  --door-d: #14213d;
  --road: #59627a;
  --cloud: #fff;
  --ground: rgba(40, 60, 30, 0.28);
  --tree: #6fae7c;
  --tree-d: #4f9663;
  --star: #e8b23b;
}

/* تاریک */

[data-theme="dark"] {
  --bg: #0b1220;
  --bg2: #0f1a2e;
  --panel: #142038;
  --text: #f3efe6;
  --text2: #c3c9d6;
  --text3: #8d97ab;
  --gold: #e4b955;
  --gold-hi: #ffdc85;
  --gold-lo: #b5851f;
  --gold-t: #ecc978;
  --mint: #6fe0cd;
  --line: rgba(228, 185, 85, 0.22);
  --soft: rgba(255, 255, 255, 0.08);
  --shadow: 0 20px 60px rgba(0, 0, 0, 0.5);

  --btn-bg: linear-gradient(135deg, #ffdc85, #e4b955);

  --btn-fg: #1a1405;

  --btn-hover: linear-gradient(135deg, #ffe6a1, #efc45e);

  --btn-shadow: rgba(228, 185, 85, 0.28);

  --sky-a: #0f1a33;
  --sky-b: #26365f;
  --hill-a: #1f3d48;
  --hill-b: #275058;
  --wall: #27324f;
  --wall-line: #4b5a80;
  --roof-a: #f0c65f;
  --roof-b: #b8851e;
  --win: #ffe7a3;
  --win-glow: rgba(255, 214, 120, 0.6);
  --door: #efc45e;
  --door-d: #b5851f;
  --road: #0a0f1c;
  --cloud: #34406b;
  --ground: rgba(0, 0, 0, 0.5);
  --tree: #2f7a6b;
  --tree-d: #22604f;
  --star: #ffe28a;
}

/* ============ BASE ============ */

*,
*::before,
*::after {
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
  scroll-padding-top: 84px;
  background: var(--bg);
}

body {
  margin: 0;
  min-width: 320px;
  background: var(--bg);
  color: var(--text);
  font-family: var(--font);
}

body.lock {
  overflow: hidden;
}

button {
  font: inherit;
  border: 0;
  background: none;
  color: inherit;
  cursor: pointer;
}

a {
  color: inherit;
  text-decoration: none;
}

::selection {
  background: var(--gold-hi);
  color: #1f1b16;
}

:where(a, button):focus-visible {
  outline: 2px solid var(--gold);
  outline-offset: 3px;
  border-radius: 8px;
}

.shell {
  position: relative;
  min-height: 100vh;
  overflow-x: clip;

  background:
    radial-gradient(
      circle at 85% 8%,
      color-mix(in srgb, var(--gold) 7%, transparent),
      transparent 32%
    ),
    radial-gradient(
      circle at 5% 60%,
      color-mix(in srgb, var(--mint) 4%, transparent),
      transparent 30%
    ),
    var(--bg);

  transition:
    background 0.4s,
    color 0.4s;
}

.lang-fa {
  font-family: var(--font-fa);
}

.container {
  width: min(100% - 48px, var(--container));
  margin-inline: auto;
}

.sec {
  padding: 110px 0;
}

.sec.alt {
  background: var(--bg2);
}

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 14px 28px;
  border-radius: 8px;
  font-weight: 600;
  font-size: 14px;
  color: var(--btn-fg);
  background: var(--btn-bg);

  box-shadow: 0 10px 26px var(--btn-shadow);

  transition:
    transform 0.25s var(--ease),
    box-shadow 0.25s,
    background 0.25s;
}

.btn:hover {
  transform: translateY(-2px);
  background: var(--btn-hover);
}

.btn:active {
  transform: scale(0.97);
}

.btn.ghost {
  background: transparent;
  color: var(--text);
  box-shadow: none;
  border: 1px solid var(--line);
}

.btn.ghost:hover {
  border-color: var(--gold);

  background: color-mix(in srgb, var(--gold) 10%, transparent);
}

.btn-sm {
  padding: 9px 20px;
  font-size: 13px;
  border-radius: 7px;
}

/* ============ LOTTIE LOADER ============ */

.loader {
  position: fixed;
  inset: 0;
  z-index: 5000;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  gap: 10px;

  background:
    radial-gradient(
      circle at 50% 40%,
      color-mix(in srgb, var(--gold) 16%, transparent),
      transparent 40%
    ),
    var(--bg);

  font-size: 14px;
}

.ld-lottie {
  width: min(390px, 78vw);
  height: min(280px, 48vh);

  display: flex;
  align-items: center;
  justify-content: center;

  margin-bottom: 4px;
}

.ld-lottie svg {
  width: 100% !important;
  height: 100% !important;
  display: block;
}

.ld-brand {
  font-family: var(--font-fa);
  font-size: 28px;
  font-weight: 800;
  line-height: 1.5;
  letter-spacing: 0;
  color: var(--text);
  text-align: center;
}

.loader p {
  margin: 0;
  color: var(--text3);
  font-size: 13px;
  text-align: center;
}

.ld-bar {
  width: 190px;
  height: 6px;
  border-radius: 99px;
  background: var(--soft);
  overflow: hidden;
}

.ld-bar span {
  display: block;
  height: 100%;
  border-radius: inherit;
  background: linear-gradient(90deg, var(--gold), var(--gold-hi));
}

.loader em {
  font-style: normal;
  font-size: 12px;
  color: var(--gold-t);
  font-variant-numeric: tabular-nums;
}

.fade-enter-active,
.fade-leave-active {
  transition:
    opacity 0.6s ease,
    transform 0.6s var(--ease);
}

.fade-leave-to,
.fade-enter-from {
  opacity: 0;
}

.loader.fade-leave-to {
  transform: scale(1.04);
}

/* ============ HEADER ============ */

.header {
  position: fixed;
  z-index: 1000;
  inset: 0 0 auto;
  padding: 12px 0;
  background: transparent;
}

.header::after {
  content: "";
  position: absolute;
  inset: 0;
  z-index: -1;
  pointer-events: none;
  opacity: 0;
  background: color-mix(in srgb, var(--panel) 84%, transparent);
  backdrop-filter: blur(18px) saturate(1.4);
  -webkit-backdrop-filter: blur(18px) saturate(1.4);
  border-bottom: 1px solid var(--soft);
  box-shadow: 0 10px 34px rgba(20, 33, 61, 0.08);
  transition: opacity 0.45s var(--ease);
}

.scrolled .header::after,
.menu .header::after {
  opacity: 1;
}

.header::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  background: linear-gradient(90deg, var(--gold), var(--gold-hi));

  transform: scaleX(var(--sp, 0));

  transform-origin: left;
}

.lang-fa .header::before {
  transform-origin: right;
}

.header-in {
  min-height: 52px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;

  border: 1px solid transparent;
  border-radius: 12px;

  transition:
    width 0.45s var(--ease),
    padding 0.35s var(--ease),
    background 0.35s,
    border-color 0.35s,
    box-shadow 0.35s;
}

.brand {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  font-weight: 700;
  font-size: 17px;
}

.logo {
  display: block;
  flex: 0 0 auto;
  width: 40px;
  height: 40px;
  overflow: visible;
}

.lg-bg {
  fill: #14213d;
  stroke: var(--line);
  stroke-width: 1;
}

.lg-l {
  fill: none;
  stroke: var(--gold-hi);
  stroke-width: 1.4;
  stroke-linecap: square;
  stroke-linejoin: miter;
}

.lg-w {
  fill: var(--gold-hi);
  opacity: 0.32;
  transition: opacity 0.35s calc(var(--i, 0) * 45ms);
}

.lg-w.on,
.brand:hover .lg-w,
.brand:focus-visible .lg-w {
  opacity: 1;
}

@media (prefers-reduced-motion: no-preference) {
  .shell.ready .lg-l {
    stroke-dasharray: 1;
    animation: arcDraw 1.1s 0.3s var(--ease) both;
  }

  .shell.ready .lg-w {
    animation: arcFade 0.6s calc(1.1s + var(--i, 0) * 60ms) ease both;
  }
}

.nav {
  position: relative;
  display: flex;
  gap: 2px;
  padding: 4px;
  border-radius: 12px;

  background: color-mix(in srgb, var(--panel) 60%, transparent);
  border: 1px solid var(--soft);
}

.nav a {
  position: relative;
  z-index: 1;
  padding: 8px 18px;
  border-radius: 8px;
  font-size: 13px;
  color: var(--text2);
  transition: color 0.25s;
}

.nav a:hover {
  color: var(--text);
}

.nav a.active {
  color: var(--text);
  font-weight: 600;
}

/* active indicator: a thin gold rule that slides between links */
.nav-pill {
  position: absolute;
  left: 0;
  bottom: 3px;
  height: 2px;
  background: var(--gold);
  clip-path: inset(0 16px);

  transition:
    transform 0.5s var(--ease),
    width 0.5s var(--ease),
    opacity 0.3s;
}

/* theme switch: sun and moon icons, a thumb slides over the active one */
.tsw {
  position: relative;
  display: block;
  flex: 0 0 auto;
  width: 58px;
  height: 32px;
  direction: ltr;
  border: 1px solid var(--line);
  border-radius: 99px;
  background: var(--bg2);

  transition:
    background 0.4s,
    border-color 0.3s;
}

.tsw:hover {
  border-color: var(--gold);
}

.ti {
  position: absolute;
  z-index: 2;
  top: 7px;
  width: 16px;
  height: 16px;
  fill: none;
  stroke: currentColor;
  stroke-width: 1.8;
  stroke-linecap: round;
  stroke-linejoin: round;
  pointer-events: none;
  transition: color 0.35s;
}

.ti-sun {
  left: 7px;
  color: var(--gold-t);
}

.ti-moon {
  right: 7px;
  color: var(--text3);
}

.tk {
  position: absolute;
  z-index: 1;
  top: 3px;
  left: 3px;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: var(--panel);
  border: 1px solid var(--line);
  box-shadow: 0 1px 4px rgba(20, 33, 61, 0.14);

  transition: transform 0.45s var(--ease);
}

.theme-dark .tk {
  transform: translateX(26px);
}

.theme-dark .ti-sun {
  color: var(--text3);
}

.theme-dark .ti-moon {
  color: var(--gold-t);
}

.mpanel > * {
  animation: dropIn 0.5s var(--ease) both;
}

.mpanel > :nth-child(2) {
  animation-delay: 0.06s;
}

.mpanel > :nth-child(3) {
  animation-delay: 0.12s;
}

.mpanel > :nth-child(4) {
  animation-delay: 0.18s;
}

.mpanel > :nth-child(5) {
  animation-delay: 0.24s;
}

.actions {
  display: flex;
  align-items: center;
  gap: 10px;
}

.chip {
  display: grid;
  place-items: center;
  min-width: 36px;
  height: 36px;
  padding: 0 8px;
  border-radius: 10px;
  border: 1px solid var(--line);
  background: transparent;
  font-size: 12px;
  font-weight: 600;
  color: var(--text2);
  transition:
    border-color 0.25s,
    color 0.25s;
}

.chip:hover {
  border-color: var(--gold);
  color: var(--gold-t);
}

.burger {
  display: none;
  position: relative;
  width: 38px;
  height: 38px;
}

.burger span {
  position: absolute;
  left: 9px;
  width: 20px;
  height: 2px;
  border-radius: 2px;
  background: var(--text);

  transition:
    transform 0.3s var(--ease),
    top 0.3s;
}

.burger span:first-child {
  top: 14px;
}

.burger span:last-child {
  top: 22px;
}

.menu .burger span:first-child {
  top: 18px;
  transform: rotate(45deg);
}

.menu .burger span:last-child {
  top: 18px;
  transform: rotate(-45deg);
}

.mmenu {
  position: fixed;
  z-index: 950;
  inset: 0;
  background: rgba(15, 12, 8, 0.45);

  backdrop-filter: blur(8px);
}

.mpanel {
  position: absolute;
  inset: 84px 16px auto;

  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 14px;

  border-radius: 14px;

  background: var(--panel);
  border: 1px solid var(--line);
  box-shadow: var(--shadow);

  animation: dropIn 0.4s var(--ease);
}

.mpanel a:not(.btn) {
  display: flex;
  justify-content: space-between;
  padding: 16px 14px;
  border-radius: 8px;
  font-size: 17px;
}

.mpanel a:not(.btn):hover {
  background: color-mix(in srgb, var(--gold) 12%, transparent);
}

.mpanel a span {
  color: var(--gold-t);
}

.mpanel .btn {
  margin-top: 8px;
}

@media (prefers-reduced-motion: no-preference) {
  .shell.ready .header-in {
    animation: hdrIn 0.9s 0.1s var(--ease) both;
  }
}

@keyframes hdrIn {
  from {
    opacity: 0;
    transform: translateY(-14px);
  }
}

/* ============ HERO ============ */

.hero {
  padding: 132px 0 80px;
}

.hero-grid {
  display: grid;
  grid-template-columns:
    1.05fr
    0.95fr;
  gap: 32px;
  align-items: center;
}

.eyebrow {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  color: var(--gold-t);
  font-size: 13px;
  font-weight: 600;
}

.eyebrow::before {
  content: "";
  width: 34px;
  height: 2px;
  background: var(--gold);
}

.hero h1 {
  margin: 20px 0 22px;
  font-family: var(--font-d);
  font-size: clamp(38px, 4.4vw, 58px);
  line-height: 1.14;
  letter-spacing: -0.015em;
  font-weight: 600;
  color: var(--text);
}

.lang-fa .hero h1,
.lang-fa h2 {
  font-family: var(--font-fa);
  font-weight: 800;
  letter-spacing: -0.02em;
  line-height: 1.32;
}

.lang-fa .hero h1 {
  font-weight: 700;
}

.grad {
  color: var(--gold-t);
}

.lead {
  max-width: 480px;
  margin: 0;
  color: var(--text2);
  font-size: 16px;
  line-height: 1.9;
}

.hero-cta {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  margin-top: 30px;
}

.stats {
  display: inline-grid;
  grid-template-columns: repeat(3, auto);

  margin-top: 40px;

  border: 1px solid var(--line);
  border-radius: 6px;
  background: var(--panel);
  overflow: hidden;
}

.stats > div {
  padding: 16px 26px;
}

.stats > div + div {
  border-inline-start: 1px solid var(--soft);
}

.stats strong {
  display: block;
  font-size: 26px;
  color: var(--text);
  font-family: var(--font-d);
}

.lang-fa .stats strong {
  font-family: var(--font-fa);
}

.stats span {
  font-size: 12px;
  color: var(--text3);
}

/* --- ART: architectural elevation --- */

.art-wrap {
  width: 100%;
  max-width: 600px;
  margin-inline: auto;
}

.art {
  --a-ink: #14213d;
  --a-paper: #fbf8f0;
  --a-paper2: #ece5d2;
  --a-glass: #3a4c74;
  --a-lit: #efd58f;
  --a-mull: rgba(20, 33, 61, 0.6);
  --a-grid: rgba(20, 33, 61, 0.09);
  --a-far: #cdd9e6;
  --a-far-line: #c3d0df;
  --a-near: #b9c7d9;
  --a-near-line: #adbcd0;
  --a-ground: #e9e3d2;
  --a-ground2: #f4f0e4;
  --a-tree: #6f8f76;
  --a-cloud: rgba(255, 255, 255, 0.85);

  position: relative;
  width: 100%;
  aspect-ratio: 600 / 540;
}

.theme-dark .art {
  --a-ink: #e9d9a8;
  --a-paper: #1a2846;
  --a-paper2: #131e38;
  --a-glass: #0a1122;
  --a-lit: #ffdc85;
  --a-mull: rgba(8, 14, 28, 0.85);
  --a-grid: rgba(233, 217, 168, 0.07);
  --a-far: #101a30;
  --a-far-line: #14203a;
  --a-near: #0c1527;
  --a-near-line: #101b31;
  --a-ground: #0c1427;
  --a-ground2: #0a1121;
  --a-tree: #1f4a44;
  --a-cloud: rgba(120, 140, 190, 0.3);
}

.arc {
  display: block;
  width: 100%;
  height: 100%;
  overflow: hidden;
  border-radius: 10px;
  direction: ltr;
  box-shadow: var(--shadow);
}

.arc-panel {
  fill: url(#arc-sky);
  stroke: var(--line);
  stroke-width: 1;
}

.arc .line,
.arc .line-thin,
.arc .crown,
.arc .ground,
.arc .dim path,
.arc .mullion,
.arc .paving {
  fill: none;
  stroke-linecap: butt;
  stroke-linejoin: miter;
}

.arc .line {
  stroke: var(--a-ink);
  stroke-width: 1.5;
}

.arc .line-thin {
  stroke: var(--a-ink);
  stroke-width: 1;
  opacity: 0.7;
}

.arc .crown {
  stroke: var(--gold);
  stroke-width: 2.5;
}

.arc .ground {
  stroke: var(--a-ink);
  stroke-width: 1.6;
}

.arc .paving {
  stroke: var(--a-ink);
  stroke-width: 0.6;
  opacity: 0.12;
}

.arc .mullion {
  stroke: var(--a-mull);
  stroke-width: 1;
}

.arc .glass rect {
  fill: var(--a-glass);
}

.arc .lit {
  fill: var(--a-lit);
  opacity: 0;
}

.arc .lit.on {
  opacity: 0.9;
}

.arc .cypress ellipse {
  fill: var(--a-tree);
}

.arc .dim path {
  stroke: var(--gold);
  stroke-width: 0.9;
}

.arc .dim text {
  fill: var(--gold-t);
  font:
    500 10px/1 ui-monospace,
    "SF Mono",
    Menlo,
    Consolas,
    monospace;
  letter-spacing: 0.04em;
}

.arc .ring-o,
.arc .ring-i {
  fill: none;
  stroke: var(--gold);
  stroke-width: 0.9;
}

.arc .ring-o {
  opacity: 0.4;
}

.arc .disc {
  fill: var(--gold-hi);
  opacity: 0.32;
}

.arc .cloud {
  fill: var(--a-cloud);
}

.arc .glint {
  opacity: 0;
}

.arc .px {
  translate: 0 0;
  transition: translate 0.7s var(--ease);
}

.arc .px-far {
  translate: calc(var(--mx, 0) * -9px) calc(var(--my, 0) * -3px);
}

.arc .px-near {
  translate: calc(var(--mx, 0) * -5px) calc(var(--my, 0) * -2px);
}

.arc .px-mid {
  translate: calc(var(--mx, 0) * -1.5px) 0;
}

/* sun <-> moon */

.arc .orb {
  transition:
    transform 1.3s var(--ease),
    opacity 0.9s ease;
}

.arc .orb.moon {
  opacity: 0;
  transform: translateY(96px);
}

.theme-dark .arc .orb.sun {
  opacity: 0;
  transform: translateY(96px);
}

.theme-dark .arc .orb.moon {
  opacity: 1;
  transform: none;
}

.arc .moon-body {
  fill: var(--a-ink);
  opacity: 0.85;
}

.arc .haze {
  pointer-events: none;
}

.theme-dark .arc .haze {
  opacity: 0.5;
}

.arc .glare {
  translate: calc(var(--mx, 0) * 70px) calc(var(--my, 0) * 40px);
  transition: translate 0.7s var(--ease);
  pointer-events: none;
}

/* service tour: one part of the building at a time */

.arc .tour > g {
  opacity: 0;
}

.arc .hl {
  fill: color-mix(in srgb, var(--gold) 9%, transparent);
  stroke: var(--gold);
  stroke-width: 1.4;
}

.arc .ld {
  fill: none;
  stroke: var(--gold);
  stroke-width: 1;
}

.arc .dot {
  fill: var(--gold);
}

.tag {
  position: absolute;
  z-index: 2;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 6px 12px;

  font-size: 12px;
  font-weight: 600;
  white-space: nowrap;
  color: var(--text);

  border: 1px solid var(--line);
  border-radius: 4px;
  background: color-mix(in srgb, var(--panel) 90%, transparent);
  backdrop-filter: blur(6px);
  -webkit-backdrop-filter: blur(6px);

  opacity: 0;
  pointer-events: none;
}

.tag i {
  width: 6px;
  height: 6px;
  background: var(--gold);
  transform: rotate(45deg);
}

.tg0 {
  left: 28.67%;
  top: 36.3%;
  translate: -100% -50%;
}

.tg1 {
  left: 18%;
  top: 49.6%;
  translate: -50% -100%;
}

.tg2 {
  left: 74.17%;
  top: 58.9%;
  translate: -50% -100%;
}

@media (max-width: 640px) {
  .tag {
    display: none;
  }
}

/* title block */

.tblock {
  position: absolute;
  inset-inline-start: 4.5%;
  bottom: 5%;

  display: flex;
  align-items: stretch;
  gap: 12px;
  padding: 11px 18px 11px 14px;

  border: 1px solid var(--line);
  border-radius: 4px;
  background: color-mix(in srgb, var(--panel) 86%, transparent);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
}

.tb-mark {
  width: 2px;
  background: var(--gold);
}

.tblock p {
  display: grid;
  gap: 3px;
  margin: 0;
}

.tblock b {
  font-size: 13px;
  font-weight: 600;
  color: var(--text);
}

.tblock small {
  font-size: 11.5px;
  color: var(--text3);
}

/* motion: one orchestrated build-up after the loader */

@media (prefers-reduced-motion: no-preference) {
  .shell.ready .arc .draw {
    stroke-dasharray: 1;
    animation: arcDraw var(--t, 1.2s) var(--d, 0s) var(--ease) both;
  }

  .shell.ready .arc .rise {
    transform-box: fill-box;
    transform-origin: 50% 100%;
    animation: arcRise var(--t, 1.2s) var(--d, 0s) var(--ease) both;
  }

  .shell.ready .arc .fade {
    animation: arcFade 0.9s var(--d, 0s) ease both;
  }

  .shell.ready .arc .arc-sun {
    animation: arcSun 2.2s 0.9s var(--ease) both;
  }

  .shell.ready .arc .arc-gridfill {
    animation: arcFade 1.4s ease both;
  }

  .shell.ready .arc .lit.on {
    animation: arcWinOn 1.1s var(--d, 2s) ease both;
  }

  .shell.ready .arc .lit.live {
    animation:
      arcWinOn 1.1s var(--d, 2s) ease both,
      arcLive var(--p, 11s) calc(var(--d, 2s) + 5s) ease-in-out infinite;
  }

  .shell.ready .arc .lit.live-on {
    animation: arcLiveOn var(--p, 11s) calc(var(--d, 2s) + 5s) ease-in-out
      infinite;
  }

  .shell.ready .arc .glint {
    animation: arcGlint 12s 5.2s ease-in-out infinite;
  }

  .shell.ready .arc .cloud {
    animation: arcCloud 46s ease-in-out infinite alternate;
  }

  .shell.ready .arc .cl2 {
    animation-duration: 58s;
  }

  .shell.ready .arc .cl3 {
    animation-duration: 52s;
    animation-direction: alternate-reverse;
  }

  .shell.ready .arc .tour > g,
  .shell.ready .tag {
    animation: arcTour 12s var(--o, 5.5s) ease infinite both;
  }

  .shell.ready .tblock {
    animation: arcTitle 0.9s 3.1s var(--ease) both;
  }
}

@keyframes heroIn {
  from {
    opacity: 0;
    transform: translateY(14px);
  }
}

@keyframes arcDraw {
  from {
    stroke-dashoffset: 1;
  }

  to {
    stroke-dashoffset: 0;
  }
}

@keyframes arcTour {
  0% {
    opacity: 0;
  }

  6%,
  29% {
    opacity: 1;
  }

  35%,
  100% {
    opacity: 0;
  }
}

@keyframes arcRise {
  from {
    transform: scaleY(0);
  }
}

@keyframes arcFade {
  from {
    opacity: 0;
  }
}

@keyframes arcSun {
  from {
    opacity: 0;
    transform: translateY(44px);
  }
}

@keyframes arcWinOn {
  from {
    opacity: 0;
  }
}

@keyframes arcLive {
  0%,
  54%,
  100% {
    opacity: 0.9;
  }

  62%,
  86% {
    opacity: 0.08;
  }
}

@keyframes arcLiveOn {
  0%,
  48%,
  100% {
    opacity: 0;
  }

  58%,
  84% {
    opacity: 0.9;
  }
}

@keyframes arcGlint {
  0%,
  100% {
    opacity: 0;
    transform: skewX(-14deg) translateX(-120px);
  }

  4% {
    opacity: 0.55;
  }

  22% {
    opacity: 0.55;
    transform: skewX(-14deg) translateX(280px);
  }

  26% {
    opacity: 0;
    transform: skewX(-14deg) translateX(280px);
  }
}

@keyframes arcCloud {
  from {
    transform: translateX(-24px);
  }

  to {
    transform: translateX(24px);
  }
}

@keyframes arcTitle {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
}

/* ============ SECTIONS ============ */

.cap {
  display: inline-block;
  margin-bottom: 16px;
  padding-inline-start: 14px;

  border-inline-start: 3px solid var(--gold);

  color: var(--gold-t);
  font-size: 14px;
  font-weight: 600;
}

h2 {
  margin: 0;
  font-family: var(--font-d);
  font-size: clamp(34px, 4.4vw, 58px);
  line-height: 1.12;
  letter-spacing: -0.02em;
  font-weight: 700;
}

h2 em {
  font-style: normal;
  color: var(--gold-t);
}

.body {
  max-width: 620px;
  margin: 24px 0 0;
  color: var(--text2);
  font-size: 15.5px;
  line-height: 1.95;
}

.split {
  display: grid;
  grid-template-columns:
    0.5fr
    1.5fr;
  gap: 40px;
}

.cards {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  margin-top: 20px;
}

.card {
  padding: 28px;

  border: 1px solid var(--soft);
  border-radius: var(--r-md);
  background: var(--panel);

  transition:
    transform 0.35s var(--ease),
    border-color 0.3s,
    box-shadow 0.35s;
}

.card:hover {
  transform: translateY(-6px);
  border-color: var(--line);
  box-shadow: var(--shadow);
}

.ico {
  display: grid;
  place-items: center;
  width: 52px;
  height: 52px;
  margin-bottom: 18px;
  border-radius: 16px;

  background: color-mix(in srgb, var(--gold) 16%, transparent);

  font-size: 26px;

  transition: transform 0.4s var(--ease);
}

.card:hover .ico {
  transform: rotate(-8deg) scale(1.1);
}

.card h3 {
  margin: 0 0 8px;
  font-size: 17px;
}

.card p {
  margin: 0;
  color: var(--text2);
  font-size: 14px;
  line-height: 1.85;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 30px;
}

.tags span {
  padding: 10px 18px;
  border: 1px solid var(--soft);
  border-radius: 99px;
  background: var(--panel);
  color: var(--text2);
  font-size: 13px;
  transition: all 0.25s;
}

.tags span:hover {
  border-color: var(--gold);
  transform: translateY(-2px);
}

.tags .on {
  color: #2a1c05;
  border-color: transparent;

  background: linear-gradient(135deg, var(--gold-hi), var(--gold));

  font-weight: 600;
}

.cta {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  gap: 30px;
  padding: 56px;

  border: 1px solid var(--line);
  border-radius: var(--r-lg);

  background:
    radial-gradient(
      circle at 85% 30%,
      color-mix(in srgb, var(--gold) 22%, transparent),
      transparent 45%
    ),
    var(--panel);

  box-shadow: var(--shadow);
}

.contact {
  display: grid;
  grid-template-columns:
    1.1fr
    0.9fr;
  gap: 56px;
  align-items: center;
}

.offices {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
  margin-top: 36px;
}

.offices small {
  color: var(--gold-t);
  font-weight: 600;
  font-size: 12px;
}

.offices h3 {
  margin: 10px 0 0;
}

.offices p {
  margin: 10px 0 16px;
  font-size: 13px;
}

.offices a {
  display: block;
  padding: 3px 0;
  color: var(--text2);
  font-size: 13px;
  direction: ltr;
  text-align: start;
  transition: color 0.2s;
}

.offices a:hover {
  color: var(--gold-t);
}

.cvis {
  position: relative;
  display: grid;
  place-items: center;
  min-height: 420px;

  border: 1px solid var(--line);
  border-radius: 32px;
  overflow: hidden;

  background:
    radial-gradient(
      circle at 50% 45%,
      color-mix(in srgb, var(--gold) 22%, transparent),
      transparent 55%
    ),
    var(--panel);

  box-shadow: var(--shadow);
}

.office-lottie {
  width: min(360px, 78%);
  height: 360px;

  display: flex;
  align-items: center;
  justify-content: center;
}

.office-lottie svg {
  width: 100% !important;
  height: 100% !important;
  display: block;
}

.cvis a {
  position: absolute;
  bottom: 30px;
  padding-bottom: 6px;

  border-bottom: 1px solid var(--line);

  color: var(--gold-t);
  font-size: 13px;
  direction: ltr;
}

/* ============ FOOTER ============ */

.footer {
  padding: 60px 0 30px;
  border-top: 1px solid var(--soft);
  background: var(--bg2);
}

.f-top {
  display: grid;
  grid-template-columns:
    0.9fr
    1.1fr;

  gap: 60px;
  padding-bottom: 44px;

  border-bottom: 1px solid var(--soft);
}

.f-brand p {
  max-width: 340px;
  margin: 18px 0;
  color: var(--text2);
  font-size: 13px;
  line-height: 1.9;
}

.f-brand small {
  color: var(--text3);
}

.f-cols {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}

.f-col button {
  display: flex;
  justify-content: space-between;
  width: 100%;
  font-weight: 600;
  font-size: 14px;
  cursor: default;
  padding: 0;
}

.f-col button span {
  display: none;
}

.f-links {
  display: flex;
  flex-direction: column;
  gap: 12px;
  margin-top: 16px;
}

.f-links a {
  color: var(--text2);
  font-size: 13px;

  transition:
    color 0.2s,
    transform 0.2s;
}

.f-links a:hover {
  color: var(--gold-t);
  transform: translateX(3px);
}

.lang-fa .f-links a:hover {
  transform: translateX(-3px);
}

.f-bottom {
  padding-top: 22px;
  color: var(--text3);
  font-size: 12px;
}

.to-top {
  position: fixed;
  z-index: 800;
  inset-inline-end: 22px;
  bottom: 22px;

  display: grid;
  place-items: center;

  width: 44px;
  height: 44px;
  border-radius: 50%;

  color: #2a1c05;

  background: linear-gradient(135deg, var(--gold-hi), var(--gold));

  box-shadow: var(--shadow);

  opacity: 0;
  visibility: hidden;

  transform: translateY(14px) scale(0.9);

  transition: all 0.3s var(--ease);
}

.to-top.show {
  opacity: 1;
  visibility: visible;
  transform: none;
}

/* ============ REVEAL ============ */

.reveal {
  opacity: 0;
  transform: translateY(28px) scale(0.985);

  transition:
    opacity 0.9s var(--ease),
    transform 0.9s var(--ease);
}

.reveal.in {
  opacity: 1;
  transform: none;
}

.cards .reveal:nth-child(3n + 2) {
  transition-delay: 0.08s;
}

.cards .reveal:nth-child(3n) {
  transition-delay: 0.16s;
}

.shell:not(.ready) main,
.shell:not(.ready) .header {
  opacity: 0;
}

.shell.ready .hero-copy > * {
  animation: heroIn 0.9s var(--ease) both;
}

.shell.ready .hero-copy > :nth-child(2) {
  animation-delay: 0.08s;
}

.shell.ready .hero-copy > :nth-child(3) {
  animation-delay: 0.16s;
}

.shell.ready .hero-copy > :nth-child(4) {
  animation-delay: 0.24s;
}

.shell.ready .hero-copy > :nth-child(5) {
  animation-delay: 0.32s;
}

/* ============ KEYFRAMES ============ */

@keyframes mdoor {
  0%,
  58%,
  92%,
  100% {
    transform: scaleX(1);
  }

  66%,
  86% {
    transform: scaleX(0.12);
  }
}

@keyframes mlit {
  0%,
  58%,
  92%,
  100% {
    opacity: 0;
  }

  66%,
  86% {
    opacity: 1;
  }
}

@keyframes flagUp {
  0%,
  52%,
  96%,
  100% {
    transform: rotate(80deg);
  }

  58%,
  88% {
    transform: rotate(0);
  }
}

@keyframes envFly {
  0% {
    opacity: 0;

    transform: translate(3em, -9em) rotate(30deg) scale(0.8);
  }

  8% {
    opacity: 1;
  }

  20% {
    transform: translate(2em, -6em) rotate(-14deg);
  }

  32% {
    transform: translate(-0.4em, -3.4em) rotate(12deg);
  }

  44% {
    transform: translate(0, -1.2em) rotate(0);
  }

  52% {
    opacity: 1;

    transform: translate(0, 0.5em) scale(0.8);
  }

  56%,
  100% {
    opacity: 0;

    transform: translate(0, 1.4em) scale(0.5);
  }
}

@keyframes sealPulse {
  0% {
    transform: scale(0.94);

    opacity: 0.9;
  }

  100% {
    transform: scale(1.18);

    opacity: 0;
  }
}

@keyframes dropIn {
  from {
    transform: translateY(-14px) scale(0.97);

    opacity: 0;
  }
}

/* ============ RESPONSIVE ============ */

@media (max-width: 1024px) {
  .cards {
    grid-template-columns: repeat(2, 1fr);
  }

  .split {
    grid-template-columns: 1fr;
    gap: 20px;
  }

  .nav {
    display: none;
  }

  .burger {
    display: block;
  }
}

@media (max-width: 900px) {
  .hero {
    padding: 108px 0 50px;
  }

  .hero-grid {
    grid-template-columns: 1fr;
    gap: 10px;
    text-align: start;
  }

  .art-wrap {
    max-width: 480px;
    margin-top: 10px;
  }

  .contact,
  .f-top {
    grid-template-columns: 1fr;
    gap: 36px;
  }

  .sec {
    padding: 80px 0;
  }

  .desk {
    display: none;
  }
}

@media (max-width: 640px) {
  .container {
    width: calc(100% - 32px);
  }

  .cards,
  .offices {
    grid-template-columns: 1fr;
  }

  .stats {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
  }

  .stats > div {
    padding: 14px 10px;
  }

  .stats strong {
    font-size: 21px;
  }

  .stats span {
    font-size: 11px;
  }

  .cta {
    padding: 32px 24px;
  }

  .cvis {
    min-height: 340px;
  }

  .office-lottie {
    width: min(300px, 82vw);
    height: 300px;
  }

  .seal {
    font-size: 10px;
  }

  .r1 {
    width: 260px;
    height: 260px;
  }

  .r2 {
    width: 200px;
    height: 200px;
  }

  .f-cols {
    grid-template-columns: 1fr;
    gap: 0;
  }

  .f-col {
    border-bottom: 1px solid var(--soft);
  }

  .f-col button {
    padding: 15px 0;
    cursor: pointer;
  }

  .f-col button span {
    display: block;
    transition: transform 0.3s;
  }

  .f-col.open button span {
    transform: rotate(180deg);
  }

  .f-links {
    max-height: 0;
    margin: 0;
    overflow: hidden;
    opacity: 0;

    transition: all 0.4s var(--ease);
  }

  .f-col.open .f-links {
    max-height: 260px;
    padding-bottom: 16px;
    opacity: 1;
  }

  .ld-lottie {
    width: min(330px, 82vw);

    height: 220px;
  }

  .ld-brand {
    font-size: 24px;
  }

  .to-top {
    inset-inline-end: 16px;
    bottom: 16px;
  }
}

/* ============ UPGRADE: header, menu, hero ============ */

@property --mx {
  syntax: "<number>";
  inherits: true;
  initial-value: 0;
}
@property --my {
  syntax: "<number>";
  inherits: true;
  initial-value: 0;
}

:where(a, button):focus-visible {
  outline: 2px solid var(--gold);
  outline-offset: 3px;
}

.skip {
  position: fixed;
  z-index: 2000;
  inset-block-start: 8px;
  inset-inline-start: 8px;
  padding: 10px 16px;
  border-radius: 8px;
  background: var(--btn-bg);
  color: var(--btn-fg);
  font-weight: 600;
  transform: translateY(-160%);
  transition: transform 0.25s var(--ease);
}

.skip:focus-visible {
  transform: none;
}

.burger::after {
  content: "";
  position: absolute;
  inset: -4px;
}

.mlang {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  margin-top: 6px;
  padding: 14px;
  border: 0;
  border-top: 1px solid var(--soft);
  border-radius: 0 0 8px 8px;
  background: transparent;
  color: var(--text);
  font: inherit;
  font-size: 17px;
  cursor: pointer;
}

.mlang b {
  display: grid;
  place-items: center;
  min-width: 36px;
  height: 32px;
  padding: 0 8px;
  border: 1px solid var(--line);
  border-radius: 10px;
  font-size: 12px;
  color: var(--gold-t);
}

.mlang:hover {
  background: color-mix(in srgb, var(--gold) 12%, transparent);
}

.mpanel > * {
  animation: mIn 0.55s var(--ease) both;
}
.mpanel > :nth-child(2) {
  animation-delay: 0.05s;
}
.mpanel > :nth-child(3) {
  animation-delay: 0.1s;
}
.mpanel > :nth-child(4) {
  animation-delay: 0.15s;
}
.mpanel > :nth-child(5) {
  animation-delay: 0.2s;
}
.mpanel > :nth-child(6) {
  animation-delay: 0.25s;
}
.mpanel > :nth-child(7) {
  animation-delay: 0.3s;
}

@keyframes mIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
}

@media (max-width: 1024px) {
  .desk-lang {
    display: none;
  }
}

/* hero: pointer-follow light, smoothed parallax */
.hero {
  position: relative;
  isolation: isolate;
  transition:
    --mx 0.8s var(--ease),
    --my 0.8s var(--ease);
}

.hero::before {
  content: "";
  position: absolute;
  inset: 0;
  z-index: -1;
  pointer-events: none;
  opacity: 0;
  background: radial-gradient(
    560px circle at calc(72% + var(--mx) * 7%) calc(44% + var(--my) * 9%),
    color-mix(in srgb, var(--gold) 5%, transparent),
    transparent 70%
  );
}

.lang-fa .hero::before {
  background: radial-gradient(
    560px circle at calc(28% + var(--mx) * 7%) calc(44% + var(--my) * 9%),
    color-mix(in srgb, var(--gold) 5%, transparent),
    transparent 70%
  );
}

.shell.ready .hero::before {
  animation: auroraIn 2.2s 0.4s ease forwards;
}
@keyframes auroraIn {
  to {
    opacity: 1;
  }
}

/* headline: masked line reveal */
.hero h1 .ln {
  display: block;
  overflow: hidden;
  padding-block: 0.14em;
  margin-block: -0.14em;
}
.hero h1 .ln > span {
  display: inline-block;
}
.shell.ready .hero-copy > h1 {
  animation: none;
}
.shell.ready .hero h1 .ln > span {
  animation: lineUp 1.15s calc(0.25s + var(--l) * 0.16s) var(--ease) both;
}
@keyframes lineUp {
  from {
    transform: translateY(112%);
    opacity: 0;
    filter: blur(6px);
  }
}

.eyebrow::before {
  transform-origin: left center;
}
.lang-fa .eyebrow::before {
  transform-origin: right center;
}
.shell.ready .eyebrow::before {
  animation: lineGrow 0.9s 0.3s var(--ease) both;
}
@keyframes lineGrow {
  from {
    transform: scaleX(0);
  }
}

@supports (background-clip: text) or (-webkit-background-clip: text) {
  .hero .grad {
    background: linear-gradient(
        100deg,
        var(--gold-t) 0 42%,
        var(--gold-hi) 50%,
        var(--gold-t) 58% 100%
      )
      100% 0 / 260% 100% no-repeat;
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  .shell.ready .hero .grad {
    animation: sheen 2.6s 1.5s var(--ease) both;
  }
}
@keyframes sheen {
  to {
    background-position: 0 0;
  }
}

.btn {
  position: relative;
  overflow: hidden;
}
.btn::after {
  content: "";
  position: absolute;
  inset: 0;
  pointer-events: none;
  background: linear-gradient(
    100deg,
    transparent 30%,
    rgba(255, 255, 255, 0.32) 50%,
    transparent 70%
  );
  transform: translateX(-130%);
}
.btn:hover::after {
  transform: translateX(130%);
  transition: transform 0.9s var(--ease);
}
.btn.ghost::after {
  display: none;
}

.stats strong {
  font-variant-numeric: tabular-nums;
}
.shell.ready .stats > div {
  animation: statIn 0.8s var(--ease) both;
}
.shell.ready .stats > :nth-child(1) {
  animation-delay: 0.9s;
}
.shell.ready .stats > :nth-child(2) {
  animation-delay: 1s;
}
.shell.ready .stats > :nth-child(3) {
  animation-delay: 1.1s;
}
@keyframes statIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
}

/* art: 3D tilt, float, blueprint scan */
.art-wrap {
  perspective: 1400px;
}
.art {
  transform: rotateY(calc(var(--mx) * -3.2deg))
    rotateX(calc(var(--my) * 2.4deg));
  transform-style: preserve-3d;
  will-change: transform;
}

@media (prefers-reduced-motion: no-preference) {
  .hero-copy {
    transform: translateY(calc(var(--hs, 0) * 34px));
    opacity: calc(1 - var(--hs, 0) * 0.6);
  }
  .art-wrap {
    translate: 0 calc(var(--hs, 0) * -26px);
  }
}

/* ============ REFINEMENT: calm, natural motion ============ */

:root {
  --calm: cubic-bezier(0.16, 1, 0.3, 1);
}

/* gentler, flatter parallax: an elevation drawing, not a toy */
.art {
  transform: rotateY(calc(var(--mx) * -1.4deg)) rotateX(calc(var(--my) * 1deg));
}

/* copy: slower, softer entrance, no blur */
.shell.ready .hero-copy > * {
  animation: heroIn 1.3s var(--calm) both;
}
.shell.ready .hero-copy > :nth-child(2) {
  animation-delay: 0.12s;
}
.shell.ready .hero-copy > :nth-child(3) {
  animation-delay: 0.3s;
}
.shell.ready .hero-copy > :nth-child(4) {
  animation-delay: 0.42s;
}
.shell.ready .hero-copy > :nth-child(5) {
  animation-delay: 0.54s;
}
@keyframes heroIn {
  from {
    opacity: 0;
    transform: translateY(22px);
  }
}

.shell.ready .hero h1 .ln > span {
  animation: lineUp 1.5s calc(0.2s + var(--l) * 0.18s) var(--calm) both;
}
@keyframes lineUp {
  from {
    transform: translateY(105%);
    opacity: 0;
  }
}

.shell.ready .hero .grad {
  animation: sheen 3.2s 1.9s cubic-bezier(0.45, 0, 0.2, 1) both;
}

/* art settles in like a camera coming to rest */
@media (prefers-reduced-motion: no-preference) {
  .shell.ready .art {
    animation: artSettle 2.4s 0.1s var(--calm) both;
  }

  .arc .cypress {
    transform-box: fill-box;
    transform-origin: 50% 100%;
  }
  .shell.ready .arc .cypress {
    animation: sway 7s 3.2s ease-in-out infinite alternate;
  }

  .shell.ready .arc .ring-o {
    transform-box: fill-box;
    transform-origin: 50% 50%;
    animation: breathe 9s 3s ease-in-out infinite alternate;
  }

  .arc .bird {
    fill: none;
    stroke: var(--a-ink);
    stroke-width: 1;
    stroke-linecap: round;
    opacity: 0;
  }
  .shell.ready .arc .b1 {
    animation: fly 38s 5s linear infinite;
    --y: 118px;
    --dy: -22px;
  }
  .shell.ready .arc .b2 {
    animation: fly 46s 12s linear infinite;
    --y: 150px;
    --dy: -16px;
  }
  .shell.ready .arc .b3 {
    animation: fly 52s 21s linear infinite;
    --y: 96px;
    --dy: -12px;
  }
}

[data-theme="dark"] .arc .birds {
  display: none;
}

@keyframes artSettle {
  from {
    opacity: 0;
    scale: 1.045;
  }
}
@keyframes sway {
  from {
    transform: rotate(-0.7deg);
  }
  to {
    transform: rotate(0.9deg);
  }
}
@keyframes breathe {
  from {
    opacity: 0.25;
    transform: scale(0.96);
  }
  to {
    opacity: 0.5;
    transform: scale(1.05);
  }
}
@keyframes fly {
  0% {
    opacity: 0;
    transform: translate(-40px, var(--y));
  }
  6% {
    opacity: 0.55;
  }
  50% {
    transform: translate(300px, calc(var(--y) + var(--dy)));
  }
  94% {
    opacity: 0.55;
  }
  100% {
    opacity: 0;
    transform: translate(650px, var(--y));
  }
}

/* ============ REFINEMENT 3: quieter backdrop, hero polish, expertise tags ============ */

/* neutral (not cream) sky and ground in light theme */
.theme-light .art {
  --sky-b: #eef3f8;
  --a-ground: #e4e8ec;
  --a-ground2: #f2f4f7;
}

/* drifting mist over the skyline, slow sun drift */
.arc .mist {
  opacity: 0;
  pointer-events: none;
}

@media (prefers-reduced-motion: no-preference) {
  .shell.ready .arc .mist {
    animation: mist 52s 3.5s linear infinite;
  }
  .shell.ready .arc .orb {
    animation: orbDrift 44s 3.4s ease-in-out infinite alternate;
  }

  .shell.ready .hero .grad {
    animation:
      sheen 3.2s 1.9s cubic-bezier(0.45, 0, 0.2, 1) both,
      sheenLoop 11s 10s cubic-bezier(0.45, 0, 0.2, 1) infinite;
  }

  .shell.ready .scroll-cue {
    animation: cueIn 1.2s 3.6s var(--calm) forwards;
  }
  .shell.ready .scroll-cue i {
    animation: cueDot 2.8s 4.2s cubic-bezier(0.45, 0, 0.2, 1) infinite;
  }
}

@keyframes mist {
  0% {
    transform: translateX(-280px);
    opacity: 0;
  }
  14%,
  86% {
    opacity: 0.6;
  }
  100% {
    transform: translateX(720px);
    opacity: 0;
  }
}
@keyframes orbDrift {
  to {
    translate: -16px 7px;
  }
}
@keyframes cueIn {
  to {
    opacity: 1;
  }
}
@keyframes cueDot {
  0% {
    opacity: 0;
    transform: translateY(0);
  }
  25% {
    opacity: 1;
  }
  80%,
  100% {
    opacity: 0;
    transform: translateY(15px);
  }
}

.scroll-cue {
  position: absolute;
  left: 50%;
  bottom: 20px;
  width: 22px;
  height: 38px;
  translate: -50% 0;
  border: 1px solid var(--line);
  border-radius: 12px;
  opacity: 0;
}
.scroll-cue i {
  position: absolute;
  top: 8px;
  left: 50%;
  width: 3px;
  height: 3px;
  margin-left: -1.5px;
  border-radius: 50%;
  background: var(--gold);
  opacity: 0;
}
.scrolled .scroll-cue {
  animation: none !important;
  opacity: 0;
  transition: opacity 0.4s;
}
@media (max-width: 900px) {
  .scroll-cue {
    display: none;
  }
}

/* expertise: gold fill lives on a pseudo layer so it can fade between tags */
.tags span {
  position: relative;
  isolation: isolate;
  overflow: hidden;
  transition:
    color 0.9s var(--calm),
    border-color 0.9s var(--calm),
    transform 0.4s var(--calm);
}
.tags span::before {
  content: "";
  position: absolute;
  inset: 0;
  z-index: -1;
  border-radius: inherit;
  background: linear-gradient(135deg, var(--gold-hi), var(--gold));
  opacity: 0;
  transform: scale(0.86);
  transition:
    opacity 0.9s var(--calm),
    transform 0.9s var(--calm);
}
.tags span.on {
  background: var(--panel);
}
.tags span.on::before {
  opacity: 1;
  transform: none;
}

/* ============ REFINEMENT 4: depth, light and shadow ============ */

.arc .cast {
  fill: #14213d;
  opacity: 0.07;
  transform-box: fill-box;
  transform-origin: 50% 0;
}
[data-theme="dark"] .arc .cast {
  fill: #000;
  opacity: 0.28;
}

@media (prefers-reduced-motion: no-preference) {
  /* the sun moves, so the shadow leans slowly */
  .shell.ready .arc .cast {
    animation: castLean 56s 3s ease-in-out infinite alternate;
  }

  /* more depth layers reacting to the pointer */
  .arc .arc-clouds {
    translate: calc(var(--mx) * -15px) calc(var(--my) * -4px);
  }
  .arc .cypress {
    translate: calc(var(--mx) * 6px) 0;
  }

  /* scroll pulls the illustration back slightly */
  .art-wrap {
    scale: calc(1 - var(--hs, 0) * 0.035);
  }

  /* one quiet ring on the primary button */
  .shell.ready .hero-cta .btn:not(.ghost) {
    animation: ctaRing 2.6s 5.5s ease-out 2;
  }
}

@keyframes sheenLoop {
  0% {
    background-position: 100% 0;
  }
  32%,
  100% {
    background-position: 0 0;
  }
}
@keyframes castLean {
  from {
    transform: skewX(-10deg);
  }
  to {
    transform: skewX(8deg);
  }
}
@keyframes ctaRing {
  from {
    box-shadow:
      0 10px 26px var(--btn-shadow),
      0 0 0 0 color-mix(in srgb, var(--gold) 50%, transparent);
  }
  to {
    box-shadow:
      0 10px 26px var(--btn-shadow),
      0 0 0 16px transparent;
  }
}

/* ============ REDUCED MOTION ============ */

@media (prefers-reduced-motion: reduce) {
  html {
    scroll-behavior: auto;
  }

  *,
  *::before,
  *::after {
    animation: none !important;
    transition-duration: 0.01ms !important;
  }

  .reveal {
    opacity: 1;
    transform: none;
  }

  .mflag {
    transform: rotate(0);
  }

  .sh-lit {
    opacity: 1;
  }
}
</style>
