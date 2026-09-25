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
          <span class="logo" aria-hidden="true">
            <i class="lg-roof" />
            <i class="lg-chim"><s /></i>
            <i class="lg-wall">
              <b class="lg-win a" />
              <b class="lg-win b" />
              <b class="lg-door" />
            </i>
          </span>

          <span>{{ brand }}</span>
        </a>

        <nav ref="navRef" class="nav" aria-label="Primary">
          <span class="nav-pill" :style="pillStyle" />

          <a
            v-for="n in navItems"
            :key="n.id"
            :href="`#${n.id}`"
            :class="{ active: activeSection === n.id }"
            @click="activeSection = n.id"
          >
            {{ n.label[language] }}
          </a>
        </nav>

        <div class="actions">
          <button
            class="chip"
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
            @click="menuOpen = !menuOpen"
          >
            <span /><span />
          </button>
        </div>
      </div>
    </header>

    <!-- MOBILE MENU -->
    <Transition name="fade">
      <div v-if="menuOpen" class="mmenu" @click.self="closeMenu">
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

          <a class="btn" href="#contact" @click="closeMenu">
            {{ t("header.talk") }}
          </a>
        </div>
      </div>
    </Transition>

    <main>
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
              {{ t("hero.line1") }}
              <span class="grad">{{ t("hero.line2") }}</span
              ><br />
              {{ t("hero.line3") }} {{ t("hero.line4") }}
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

          <!-- HERO ART -->
          <div class="art-wrap" aria-hidden="true">
            <div class="art">
              <div class="sky">
                <span class="orb" />

                <span class="star s1">✦</span>
                <span class="star s2">✦</span>
                <span class="star s3">✦</span>

                <span class="cloud c1" />
                <span class="cloud c2" />

                <span class="bird b1" />
                <span class="bird b2" />

                <span class="shoot" />

                <span class="hill hl-a" />
                <span class="hill hl-b" />

                <span class="road">
                  <i />
                </span>

                <div class="car">
                  <i class="cw c-a" />
                  <i class="cw c-b" />
                </div>

                <div class="tree">
                  <i />
                  <b />
                </div>

                <div class="bush">
                  <i />
                  <i />
                </div>

                <span class="shadow" />

                <div class="sign">
                  <span class="post" />

                  <div class="board">
                    <span class="face front">
                      {{ language === "fa" ? "برای فروش" : "For sale" }}
                    </span>

                    <span class="face back">
                      {{ language === "fa" ? "فروخته شد" : "Sold" }}
                    </span>
                  </div>
                </div>

                <div class="house-wrap">
                  <div class="house">
                    <span class="chimney">
                      <s />
                      <s />
                      <s />
                    </span>

                    <span class="roof" />
                    <span class="attic" />

                    <div class="wall">
                      <span class="win w1" />
                      <span class="win w2" />

                      <span class="lamp l1" />
                      <span class="lamp l2" />

                      <span class="pot p1">
                        <i />
                      </span>

                      <span class="pot p2">
                        <i />
                      </span>

                      <div class="doorway">
                        <div class="room">
                          <span class="glow" />

                          <span class="fig f1">
                            <i />
                          </span>

                          <span class="fig f2">
                            <i />
                          </span>

                          <span class="fig f3">
                            <i />
                          </span>
                        </div>

                        <span class="door">
                          <i />
                        </span>
                      </div>
                    </div>
                  </div>
                </div>

                <span class="sp sp1">✦</span>
                <span class="sp sp2">✦</span>
                <span class="sp sp3">✦</span>
                <span class="sp sp4">✦</span>

                <span
                  v-for="(c, i) in confetti"
                  :key="i"
                  class="cf"
                  :style="c"
                />

                <div class="key">
                  <i class="bow" />
                  <i class="shaft" />
                  <i class="teeth" />
                </div>
              </div>

              <div class="badge">
                {{ t("hero.product.title") }}
              </div>

              <div class="mini-card">
                <span>⌂</span>

                <p>
                  <small>
                    {{ language === "fa" ? "برای خانواده‌ها" : "For families" }}
                  </small>

                  <b>
                    {{
                      language === "fa"
                        ? "خانه‌ای برای زندگی"
                        : "A place to live"
                    }}
                  </b>
                </p>
              </div>
            </div>
          </div>
        </div>
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

            <div class="tags">
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
              <span class="logo" aria-hidden="true">
                <i class="lg-roof" />
                <i class="lg-chim">
                  <s />
                </i>

                <i class="lg-wall">
                  <b class="lg-win a" />
                  <b class="lg-win b" />
                  <b class="lg-door" />
                </i>
              </span>

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

const city = [
  { h: 5, w: 3.2 },
  { h: 8, w: 3.6 },
  { h: 5.6, w: 4.6, home: true },
  { h: 10, w: 3.4 },
  { h: 6.4, w: 3.2 },
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

const activeTags = ["Residential", "Luxury", "Investment"];

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
  const attr = /^(og|twitter):/.test(name) ? "property" : "name";

  let el = document.head.querySelector(`meta[${attr}="${name}"]`);

  if (!el) {
    el = document.createElement("meta");
    el.setAttribute(attr, name);
    document.head.appendChild(el);
  }

  el.setAttribute("content", content);
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

const palette = ["var(--gold-hi)", "#e5684f", "var(--mint)", "#5b9dff"];

const confetti = Array.from({ length: 12 }, (_, i) => ({
  "--x": ((i - 5.5) * 1.6).toFixed(1) + "em",
  "--y": (-(6 + (i % 3) * 2.4)).toFixed(1) + "em",
  "--r": 200 + i * 47 + "deg",
  "--c": palette[i % 4],
  animationDelay: (i % 4) * 0.05 + "s",
}));

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

  document.fonts?.ready.then(updatePill);

  nextTick(updatePill);
});

/* ---------- UNMOUNT ---------- */

onBeforeUnmount(() => {
  window.removeEventListener("scroll", window._sadafScroll);

  window.removeEventListener("resize", updatePill);

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
  border-radius: 12px;
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
  border-radius: 10px;
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
  padding: 18px 0;
  transition: padding 0.35s var(--ease);
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

.scrolled .header {
  padding: 10px 0;
}

.header-in {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;

  border: 1px solid transparent;
  border-radius: 18px;

  transition:
    width 0.45s var(--ease),
    padding 0.35s var(--ease),
    background 0.35s,
    border-color 0.35s,
    box-shadow 0.35s;
}

.scrolled .header-in {
  width: min(100% - 32px, 1060px);

  padding: 8px 12px;

  background: color-mix(in srgb, var(--panel) 78%, transparent);

  backdrop-filter: blur(18px) saturate(1.4);

  -webkit-backdrop-filter: blur(18px) saturate(1.4);

  border-color: var(--soft);
  box-shadow: var(--shadow);
}

.brand {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  font-weight: 700;
  font-size: 17px;
}

.logo {
  position: relative;
  flex: 0 0 auto;
  width: 40px;
  height: 40px;
  font-size: 10px;
  border-radius: 12px;

  background: linear-gradient(135deg, var(--gold-hi), var(--gold));

  box-shadow: 0 6px 16px color-mix(in srgb, var(--gold) 40%, transparent);

  animation: logoBob 4s ease-in-out infinite;
}

.logo i,
.logo b,
.logo s {
  position: absolute;
  display: block;
}

.lg-roof {
  left: 0.5em;
  top: 1em;
  width: 3em;
  height: 1.4em;

  clip-path: polygon(50% 0, 100% 100%, 0 100%);

  background: #14213d;
}

.lg-chim {
  left: 2.6em;
  top: 0.6em;
  width: 0.42em;
  height: 0.9em;
  border-radius: 0.1em 0.1em 0 0;
  background: #14213d;
}

.lg-chim s {
  left: -0.05em;
  bottom: 100%;
  width: 0.5em;
  height: 0.5em;
  border-radius: 50%;
  background: rgba(20, 33, 61, 0.45);
  animation: smokeSm 2.8s ease-out infinite;
}

.lg-wall {
  left: 0.8em;
  top: 2.3em;
  width: 2.4em;
  height: 1.35em;
  border-radius: 0.1em 0.1em 0.25em 0.25em;
  background: #fffaf0;
}

.lg-win {
  top: 0.25em;
  width: 0.5em;
  height: 0.5em;
  border-radius: 0.1em;
  background: var(--gold-lo);

  animation: twinkle 3s ease-in-out infinite;
}

.lg-win.a {
  left: 0.25em;
}

.lg-win.b {
  right: 0.25em;
  animation-delay: 1.2s;
}

.lg-door {
  bottom: 0;
  left: 50%;
  margin-left: -0.28em;
  width: 0.56em;
  height: 0.8em;
  border-radius: 0.3em 0.3em 0 0;
  background: #14213d;
  animation: logoDoor 5s ease-in-out infinite;
}

.brand:hover .logo {
  animation: logoHop 0.6s var(--ease);
}

.nav {
  position: relative;
  display: flex;
  gap: 6px;
  padding: 5px;
  border-radius: 999px;

  background: color-mix(in srgb, var(--panel) 70%, transparent);

  border: 1px solid var(--soft);
}

.nav a {
  position: relative;
  z-index: 1;
  padding: 8px 18px;
  border-radius: 999px;
  font-size: 13px;
  color: var(--text2);
  transition: all 0.25s;
}

.nav a:hover {
  color: var(--text);
}

.nav a.active {
  color: #2a1c05;
  font-weight: 600;
}

.nav-pill {
  position: absolute;
  left: 0;
  top: 5px;
  bottom: 5px;

  border-radius: 999px;

  background: linear-gradient(135deg, var(--gold-hi), var(--gold));

  box-shadow: 0 4px 14px color-mix(in srgb, var(--gold) 40%, transparent);

  transition:
    transform 0.5s var(--ease),
    width 0.5s var(--ease),
    opacity 0.3s;
}

.tsw {
  position: relative;
  flex: 0 0 auto;
  width: 58px;
  height: 30px;
  border: 1px solid var(--line);
  border-radius: 99px;
  overflow: hidden;

  background: linear-gradient(180deg, #bfe0f2, #fbf5e4);

  transition: background 0.5s;
}

.tsw::before {
  content: "";
  position: absolute;
  bottom: 5px;
  inset-inline-end: 9px;
  width: 14px;
  height: 6px;
  border-radius: 9px;
  background: #fff;

  box-shadow: -6px -3px 0 -1px #fff;

  transition:
    opacity 0.4s,
    transform 0.5s var(--ease);
}

.tsw::after {
  content: "";
  position: absolute;
  top: 7px;
  inset-inline-start: 11px;
  width: 2px;
  height: 2px;
  border-radius: 50%;
  background: #fff;

  box-shadow:
    8px 6px 0 #fff,
    3px 12px 0 #fff,
    15px 1px 0 #fff;

  opacity: 0;

  transition: opacity 0.4s;
}

.tk {
  position: absolute;
  top: 3px;
  inset-inline-start: 3px;
  width: 22px;
  height: 22px;
  border-radius: 50%;

  background: radial-gradient(
    circle at 35% 30%,
    #fff8cf,
    var(--gold-hi) 55%,
    var(--gold)
  );

  box-shadow:
    0 0 0 3px rgba(255, 220, 120, 0.3),
    0 2px 8px rgba(0, 0, 0, 0.2);

  transition:
    inset-inline-start 0.5s var(--ease),
    background 0.4s,
    box-shadow 0.4s;
}

.tsw:hover .tk {
  box-shadow:
    0 0 0 5px rgba(255, 220, 120, 0.3),
    0 2px 8px rgba(0, 0, 0, 0.2);
}

.theme-dark .tsw {
  background: linear-gradient(180deg, #0f1a33, #26365f);
}

.theme-dark .tsw::before {
  opacity: 0;
  transform: translateY(6px);
}

.theme-dark .tsw::after {
  opacity: 1;
}

.theme-dark .tk {
  inset-inline-start: 31px;

  background: radial-gradient(circle at 65% 35%, #fffdf0, #e8dfb8 70%);

  box-shadow:
    inset -4px -3px 0 rgba(150, 140, 100, 0.35),
    0 0 0 3px rgba(255, 250, 220, 0.12);
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
  min-width: 38px;
  height: 38px;
  padding: 0 10px;
  border-radius: 50%;
  border: 1px solid var(--line);
  background: var(--panel);
  font-size: 12px;
  font-weight: 600;
  color: var(--gold-t);
  transition: transform 0.25s var(--ease);
}

.chip:hover {
  transform: translateY(-2px) rotate(-6deg);
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
  background: var(--gold-t);

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
  inset: 78px 16px auto;

  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 14px;

  border-radius: var(--r-lg);

  background: var(--panel);
  border: 1px solid var(--line);
  box-shadow: var(--shadow);

  animation: dropIn 0.4s var(--ease);
}

.mpanel a:not(.btn) {
  display: flex;
  justify-content: space-between;
  padding: 16px 14px;
  border-radius: 14px;
  font-size: 18px;
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
  font-size: clamp(40px, 4.8vw, 64px);
  line-height: 1.12;
  letter-spacing: -0.02em;
  font-weight: 700;
  color: var(--text);
}

.lang-fa .hero h1,
.lang-fa h2 {
  font-family: var(--font-fa);
  font-weight: 800;
  letter-spacing: -0.02em;
  line-height: 1.32;
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
  border-radius: 14px;
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

/* --- ART --- */

.art-wrap {
  container-type: inline-size;
  width: 100%;
  max-width: 600px;
  margin-inline: auto;
}

.art {
  position: relative;
  width: 40em;
  height: 36em;
  font-size: calc(100cqw / 40);
}

.art > *,
.sky > *,
.house-wrap > *,
.house > *,
.wall > *,
.doorway > *,
.room > *,
.tree > *,
.bush > *,
.key > *,
.chimney > *,
.fig > i,
.sign > *,
.board > *,
.pot > i,
.road > i,
.car > i {
  position: absolute;
}

.sky {
  inset: 1em 2em 2em;
  border-radius: 3em;
  overflow: hidden;

  background: linear-gradient(180deg, var(--sky-a), var(--sky-b) 78%);

  border: 1px solid var(--line);
  box-shadow: var(--shadow);
}

.orb {
  top: 3em;
  inset-inline-end: 5em;
  width: 4.6em;
  height: 4.6em;
  border-radius: 50%;

  background: radial-gradient(
    circle at 35% 30%,
    #fff8cf,
    var(--gold-hi) 55%,
    var(--gold)
  );

  box-shadow:
    0 0 0 1em color-mix(in srgb, var(--gold-hi) 25%, transparent),
    0 0 3em color-mix(in srgb, var(--gold-hi) 55%, transparent);

  animation: floaty 7s ease-in-out infinite;
}

[data-theme="dark"] .orb {
  background: radial-gradient(circle at 65% 35%, #fffdf0, #f0e6c0 60%, #cbbf90);

  box-shadow:
    0 0 0 1em rgba(255, 250, 220, 0.07),
    0 0 3em rgba(255, 244, 200, 0.3);
}

.star {
  color: var(--star);
  font-size: 1.4em;
  opacity: 0;
  animation: twinkle 3s ease-in-out infinite;
}

[data-theme="light"] .star {
  display: none;
}

.s1 {
  top: 4em;
  left: 5em;
}

.s2 {
  top: 9em;
  left: 11em;
  animation-delay: 0.9s;
}

.s3 {
  top: 3em;
  left: 15em;
  animation-delay: 1.7s;
}

.cloud {
  height: 2.4em;
  border-radius: 99px;
  background: var(--cloud);
  opacity: 0.92;
}

.cloud::before,
.cloud::after {
  content: "";
  position: absolute;
  border-radius: 50%;
  background: inherit;
}

.cloud::before {
  width: 1.7em;
  height: 1.7em;
  top: -0.9em;
  left: 0.8em;
}

.cloud::after {
  width: 2.3em;
  height: 2.3em;
  top: -1.2em;
  left: 2.3em;
}

.c1 {
  top: 9em;
  inset-inline-start: 3em;
  width: 6em;

  animation: drift 14s ease-in-out infinite alternate;
}

.c2 {
  top: 14em;
  inset-inline-end: 2.5em;
  width: 5em;

  animation: drift 11s ease-in-out infinite alternate-reverse;
}

.hill {
  border-radius: 50%;
}

.hl-a {
  left: -8em;
  bottom: -9em;
  width: 32em;
  height: 15em;
  background: var(--hill-b);
}

.hl-b {
  right: -9em;
  bottom: -10em;
  width: 34em;
  height: 15em;
  background: var(--hill-a);
}

.tree {
  left: 3em;
  bottom: 5em;
  width: 5em;
  height: 9em;
  z-index: 2;

  transform-origin: bottom center;

  animation: sway 4s ease-in-out infinite alternate;
}

.tree i {
  bottom: 0;
  left: 2em;
  width: 1em;
  height: 3.4em;
  border-radius: 0.5em;
  background: var(--gold-lo);
}

.tree b {
  top: 0;
  left: 0;
  width: 5em;
  height: 5.6em;
  border-radius: 50%;

  background: radial-gradient(circle at 35% 30%, var(--tree), var(--tree-d));
}

.bush {
  left: 6.4em;
  bottom: 4em;
  width: 5em;
  height: 2.6em;
  z-index: 2;
}

.bush i {
  bottom: 0;
  width: 2.8em;
  height: 2.4em;
  border-radius: 50%;
  background: var(--tree-d);
}

.bush i:first-child {
  left: 0;
}

.bush i:last-child {
  left: 1.8em;
  background: var(--tree);
}

.shadow {
  left: 50%;
  bottom: 2.4em;
  width: 24em;
  height: 3em;
  margin-left: -12em;
  border-radius: 50%;

  background: radial-gradient(ellipse, var(--ground), transparent 70%);
}

/* --- HOUSE --- */

.house-wrap {
  left: 50%;
  bottom: 4em;
  width: 18em;
  height: 18em;
  margin-left: -9em;
  z-index: 3;
}

.house {
  inset: 0;

  transform-origin: bottom center;

  animation: houseHop 7s ease-in-out infinite;
}

.roof {
  top: 1em;
  left: -1.6em;
  width: 21.2em;
  height: 7.6em;
  z-index: 2;

  clip-path: polygon(50% 0, 100% 100%, 0 100%);

  background: linear-gradient(150deg, var(--roof-a), var(--roof-b));
}

.attic {
  top: 4.3em;
  left: 50%;
  margin-left: -0.9em;
  width: 1.8em;
  height: 1.8em;
  z-index: 4;
  border-radius: 50%;
  background: var(--win);
  border: 0.22em solid var(--roof-b);

  box-shadow: 0 0 1em var(--win-glow);
}

.chimney {
  top: 1.4em;
  inset-inline-end: 2.4em;
  width: 2em;
  height: 4.4em;
  z-index: 1;
  border-radius: 0.3em 0.3em 0 0;

  background: linear-gradient(var(--roof-b), var(--gold-lo));
}

.chimney s {
  left: 0.3em;
  bottom: 100%;
  width: 1.4em;
  height: 1.4em;
  border-radius: 50%;
  background: var(--cloud);
  opacity: 0;

  animation: smoke 3.8s ease-out infinite;
}

.chimney s:nth-child(2) {
  animation-delay: 1.3s;
}

.chimney s:nth-child(3) {
  animation-delay: 2.6s;
}

.wall {
  bottom: 0;
  left: 0;
  width: 100%;
  height: 10em;
  z-index: 3;

  border-radius: 0.5em 0.5em 1em 1em;

  background: var(--wall);
  border: 0.18em solid var(--wall-line);

  box-shadow: inset 0 -1em 1.4em rgba(0, 0, 0, 0.05);
}

.win {
  top: 2em;
  width: 3.6em;
  height: 3.8em;
  border-radius: 0.4em;
  border: 0.25em solid var(--wall-line);

  box-shadow: 0 0 1.4em var(--win-glow);

  background:
    linear-gradient(
      90deg,
      transparent calc(50% - 0.1em),
      var(--wall-line) 0,
      var(--wall-line) calc(50% + 0.1em),
      transparent 0
    ),
    linear-gradient(
      transparent calc(50% - 0.1em),
      var(--wall-line) 0,
      var(--wall-line) calc(50% + 0.1em),
      transparent 0
    ),
    var(--win);
}

.w1 {
  left: 2.2em;
}

.w2 {
  right: 2.2em;
}

.win {
  animation: winPulse 7s ease-in-out infinite;
}

.doorway {
  bottom: 0;
  left: 50%;
  width: 4.8em;
  height: 6.4em;
  margin-left: -2.4em;
  border-radius: 2.4em 2.4em 0 0;
  perspective: 36em;

  box-shadow: 0 0 0 0.3em var(--wall-line);
}

.room {
  inset: 0;
  border-radius: 2.4em 2.4em 0 0;
  overflow: hidden;
  background: #2a2113;
}

.glow {
  inset: 0;

  background: radial-gradient(
    ellipse at 50% 100%,
    #fff3b8,
    var(--gold-hi) 55%,
    var(--gold)
  );

  opacity: 0;

  animation: doorLight 7s ease-in-out infinite;
}

.fig {
  bottom: 0;
  width: 1.5em;
  height: 4em;

  transform: translateY(110%);

  animation: figIn 7s ease-in-out infinite;

  --c: var(--mint);
}

.fig i {
  top: 0;
  left: 0.15em;
  width: 1.2em;
  height: 1.2em;
  border-radius: 50%;
  background: #f4c9a0;
}

.fig::after {
  content: "";
  position: absolute;
  top: 1.15em;
  left: 0;
  width: 100%;
  height: 2.85em;
  border-radius: 0.75em 0.75em 0 0;
  background: var(--c);
}

.f1 {
  left: 0.5em;
}

.f2 {
  left: 1.9em;
  --c: #e5684f;
  animation-delay: 0.1s;
}

.f3 {
  right: 0.5em;
  font-size: 0.78em;
  --c: var(--gold-hi);
  animation-delay: 0.2s;
}

.door {
  inset: 0;
  border-radius: 2.4em 2.4em 0 0;

  background: linear-gradient(180deg, var(--door), var(--door-d));

  transform-origin: left center;

  animation: doorOpen 7s ease-in-out infinite;

  box-shadow: inset 0 0 0 0.3em rgba(255, 255, 255, 0.1);
}

.door::before {
  content: "";
  position: absolute;
  inset: 1em 0.9em 45% 0.9em;
  border: 1px solid rgba(255, 255, 255, 0.28);

  border-radius: 1.3em 1.3em 0 0;
}

.door i {
  right: 0.7em;
  top: 55%;
  width: 0.6em;
  height: 0.6em;
  border-radius: 50%;
  background: #fff3c4;
}

.sp {
  z-index: 9;
  left: 50%;
  font-size: 1.6em;
  color: var(--gold-hi);
  opacity: 0;

  animation: sparkle 7s ease-out infinite;

  filter: drop-shadow(0 0 0.4em var(--gold));
}

.sp1 {
  bottom: 9em;
  margin-left: -6em;
}

.sp2 {
  bottom: 12em;
  margin-left: 4em;
  animation-delay: 0.08s;
}

.sp3 {
  bottom: 5em;
  margin-left: 5em;
  animation-delay: 0.16s;
}

.sp4 {
  bottom: 11em;
  margin-left: -3em;
  animation-delay: 0.24s;
}

/* --- UPGRADES --- */

.orb::before {
  content: "";
  position: absolute;
  inset: -2.6em;
  border-radius: 50%;

  background: repeating-conic-gradient(
    color-mix(in srgb, var(--gold-hi) 45%, transparent) 0 5deg,
    transparent 5deg 22deg
  );

  -webkit-mask: radial-gradient(
    circle,
    transparent 42%,
    #000 44%,
    transparent 74%
  );

  mask: radial-gradient(circle, transparent 42%, #000 44%, transparent 74%);

  animation: spin 40s linear infinite;
}

[data-theme="dark"] .orb::before {
  display: none;
}

.bird {
  top: 6em;
  left: -3em;
  width: 1.8em;
  height: 0.7em;
  color: var(--text2);
  opacity: 0.7;

  animation: fly 18s linear infinite;
}

.bird::before,
.bird::after {
  content: "";
  position: absolute;
  bottom: 0;
  width: 0.95em;
  height: 0.55em;

  border-top: 0.16em solid currentColor;

  border-radius: 50% 50% 0 0;
}

.bird::before {
  left: 0;
  transform-origin: right bottom;

  animation: flapL 0.6s ease-in-out infinite alternate;
}

.bird::after {
  right: 0;
  transform-origin: left bottom;

  animation: flapR 0.6s ease-in-out infinite alternate;
}

.b2 {
  top: 11em;
  scale: 0.7;
  animation-duration: 25s;
  animation-delay: -10s;
}

.sign {
  right: 1.4em;
  bottom: 4.6em;
  width: 7.4em;
  height: 8em;
  z-index: 4;
  perspective: 30em;
}

.post {
  left: 50%;
  bottom: 0;
  width: 0.45em;
  height: 5.6em;
  margin-left: -0.22em;
  border-radius: 0.2em;
  background: var(--gold-lo);
}

.board {
  left: 0;
  top: 0;
  width: 100%;
  height: 3em;

  transform-style: preserve-3d;

  animation: signFlip 7s ease-in-out infinite;

  filter: drop-shadow(0 0.3em 0.4em rgba(0, 0, 0, 0.18));
}

.face {
  inset: 0;
  display: grid;
  place-items: center;
  border-radius: 0.5em;
  font-size: 1.15em;
  font-weight: 700;

  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
}

.front {
  background: var(--wall);
  border: 0.18em solid var(--wall-line);

  color: var(--text);
}

.back {
  background: linear-gradient(135deg, var(--gold-hi), var(--gold));

  color: #2a1c05;

  transform: rotateY(180deg);
}

.cf {
  z-index: 9;
  left: 50%;
  bottom: 8em;
  width: 0.6em;
  height: 1em;
  border-radius: 0.15em;
  background: var(--c);
  opacity: 0;

  animation: confetti 7s ease-out infinite;
}

.hl-a {
  translate: calc(var(--mx, 0) * 0.6em) 0;
}

.hl-b {
  translate: calc(var(--mx, 0) * -0.4em) 0;
}

.orb {
  translate: calc(var(--mx, 0) * -0.9em) calc(var(--my, 0) * -0.4em);
}

.cloud {
  translate: calc(var(--mx, 0) * 1.3em) 0;
}

.tree,
.bush,
.sign {
  translate: calc(var(--mx, 0) * 0.5em) 0;
}

.house-wrap,
.key,
.shadow {
  translate: calc(var(--mx, 0) * 0.25em) calc(var(--my, 0) * 0.15em);
}

.hl-a,
.hl-b,
.orb,
.cloud,
.tree,
.bush,
.sign,
.house-wrap,
.key,
.shadow {
  transition: translate 0.9s var(--ease);
}

/* ROAD */

.road {
  left: 0;
  right: 0;
  bottom: 0;
  height: 2.6em;
  z-index: 1;
  background: var(--road);
}

.road i {
  left: 0;
  right: 0;
  top: 50%;
  height: 0.2em;

  background: repeating-linear-gradient(
    90deg,
    #fff 0 1.4em,
    transparent 1.4em 2.8em
  );

  opacity: 0.65;

  animation: dash 1s linear infinite;
}

.car {
  left: -7em;
  bottom: 0.3em;
  width: 5.4em;
  height: 2.4em;
  z-index: 2;

  animation:
    drive 16s linear infinite,
    bob 0.45s ease-in-out infinite alternate;

  filter: drop-shadow(0 0.3em 0.3em rgba(0, 0, 0, 0.3));
}

.car::before {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0.5em;
  height: 1.15em;
  border-radius: 0.6em;

  background: linear-gradient(var(--gold-hi), var(--gold));

  box-shadow:
    inset -0.3em 0 0 rgba(255, 90, 60, 0.55),
    inset 0.3em 0 0 rgba(255, 255, 255, 0.5);
}

.car::after {
  content: "";
  position: absolute;
  left: 1.1em;
  width: 2.9em;
  bottom: 1.55em;
  height: 0.85em;
  border-radius: 0.8em 0.9em 0 0;

  background: var(--win);

  border: 0.15em solid var(--gold-lo);

  border-bottom: 0;
}

.cw {
  bottom: 0;
  width: 1em;
  height: 1em;
  border-radius: 50%;
  background: #1b1f2a;
  border: 0.2em solid #cfd3dc;

  animation: spin 0.5s linear infinite;
}

.cw::after {
  content: "";
  position: absolute;
  inset: 0.15em 0.3em;
  background: #cfd3dc;
}

.c-a {
  left: 0.7em;
}

.c-b {
  right: 0.7em;
}

.pot {
  bottom: 0;
  width: 1.7em;
  height: 1.3em;
}

.pot::before {
  content: "";
  position: absolute;
  inset: 0;

  clip-path: polygon(6% 0, 94% 0, 80% 100%, 20% 100%);

  background: #cf7d45;
}

.pot i {
  left: 0.1em;
  bottom: 1.1em;
  width: 1.5em;
  height: 1.3em;
  border-radius: 50%;

  transform-origin: bottom center;

  animation: sway 3s ease-in-out infinite alternate;

  background:
    radial-gradient(circle at 28% 38%, #ff8fa3 0 0.33em, transparent 0.34em),
    radial-gradient(circle at 72% 28%, #ffd166 0 0.33em, transparent 0.34em),
    radial-gradient(circle at 55% 78%, #ff6f91 0 0.3em, transparent 0.31em),
    var(--tree);
}

.p1 {
  left: 3.6em;
}

.p2 {
  right: 3.6em;
}

.p2 i {
  animation-delay: -1.4s;
}

.lamp {
  bottom: 4.9em;
  width: 0.7em;
  height: 1em;
  border-radius: 0.2em 0.2em 0.45em 0.45em;

  background: var(--win);

  border: 0.12em solid var(--wall-line);

  box-shadow: 0 0 0.6em var(--win-glow);

  animation: lampOn 7s ease-in-out infinite;
}

.l1 {
  left: 5.2em;
}

.l2 {
  right: 5.2em;
}

.shoot {
  top: 4em;
  left: 22em;
  width: 6em;
  height: 0.15em;
  border-radius: 1em;

  background: linear-gradient(90deg, transparent, #fff);

  opacity: 0;
  rotate: -25deg;

  animation: shoot 9s ease-out infinite;
}

[data-theme="light"] .shoot {
  display: none;
}

.key::before {
  content: "";
  position: absolute;
  left: -0.9em;
  top: -0.9em;
  width: 4.4em;
  height: 4.4em;
  border-radius: 50%;

  background: radial-gradient(
    circle,
    color-mix(in srgb, var(--gold-hi) 55%, transparent),
    transparent 65%
  );

  animation: halo 1.4s ease-in-out infinite alternate;
}

.shell.ready .house-wrap {
  animation: houseIn 1.1s 0.3s var(--ease) both;
}

.shell.ready .sign {
  animation: signIn 0.9s 0.9s var(--ease) both;
}

/* KEY */

.key {
  z-index: 10;
  left: 50%;
  bottom: 6.3em;
  width: 6.4em;
  height: 2.6em;
  margin-left: -12em;

  transform-origin: 90% 50%;

  animation: keyTrip 7s var(--ease) infinite;

  filter: drop-shadow(0 0.5em 0.5em rgba(0, 0, 0, 0.25));
}

.key i {
  display: block;
}

.bow {
  left: 0;
  top: 0;
  width: 2.6em;
  height: 2.6em;
  border-radius: 50%;

  background: radial-gradient(circle at 35% 30%, var(--gold-hi), var(--gold));

  border: 0.3em solid var(--gold-lo);
}

.bow::after {
  content: "";
  position: absolute;
  inset: 0.55em;
  border-radius: 50%;
  background: var(--bg);
}

.shaft {
  left: 2.3em;
  top: 1.05em;
  width: 3.6em;
  height: 0.55em;
  border-radius: 0.3em;

  background: linear-gradient(var(--gold-hi), var(--gold-lo));
}

.teeth {
  right: 0.4em;
  top: 1.05em;
  width: 0.7em;
  height: 1.4em;
  border-radius: 0 0 0.2em 0.2em;

  background: var(--gold-lo);

  box-shadow: -1em 0 0 -0.05em var(--gold-lo);
}

/* FLOATING LABELS */

.badge {
  z-index: 12;
  top: 0.2em;
  inset-inline-start: 0.5em;

  padding: 0.7em 1.2em;

  border: 1px solid var(--line);
  border-radius: 10px;

  background: color-mix(in srgb, var(--panel) 90%, transparent);

  backdrop-filter: blur(10px);

  color: var(--gold-t);
  font-size: 1.15em;
  font-weight: 600;

  animation: floaty 6s ease-in-out infinite;
}

.mini-card {
  z-index: 12;
  bottom: 0.3em;
  inset-inline-end: 0.5em;

  display: flex;
  align-items: center;
  gap: 0.9em;

  padding: 0.9em 1.3em;

  border: 1px solid var(--line);
  border-radius: 14px;

  background: color-mix(in srgb, var(--panel) 92%, transparent);

  backdrop-filter: blur(10px);

  box-shadow: var(--shadow);

  animation: floaty 7s ease-in-out 1s infinite;
}

.mini-card > span {
  display: grid;
  place-items: center;
  width: 2.6em;
  height: 2.6em;
  border-radius: 0.8em;

  color: #2a1c05;

  background: linear-gradient(135deg, var(--gold-hi), var(--gold));

  font-size: 1.5em;
}

.mini-card p {
  margin: 0;
  display: flex;
  flex-direction: column;
}

.mini-card small {
  font-size: 1em;
  color: var(--text3);
}

.mini-card b {
  font-size: 1.25em;
  color: var(--text);
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
  transform: translateY(18px);

  transition:
    opacity 0.7s,
    transform 0.7s var(--ease);
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

.shell.ready .hero-copy {
  animation: slideIn 0.8s var(--ease) both;
}

.shell.ready .art-wrap {
  animation: popIn 0.9s 0.1s var(--ease) both;
}

/* ============ KEYFRAMES ============ */

@keyframes twinkle {
  0%,
  100% {
    opacity: 0.35;
  }

  50% {
    opacity: 1;
  }
}

@keyframes floaty {
  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-0.6em);
  }
}

@keyframes drift {
  from {
    transform: translateX(-1.5em);
  }

  to {
    transform: translateX(1.5em);
  }
}

@keyframes sway {
  from {
    transform: rotate(-2deg);
  }

  to {
    transform: rotate(2deg);
  }
}

@keyframes houseHop {
  0%,
  54%,
  72%,
  100% {
    transform: none;
  }

  58% {
    transform: scale(1.015, 0.985);
  }

  62% {
    transform: translateY(-0.5em);
  }

  66% {
    transform: scale(0.99, 1.01);
  }
}

@keyframes smoke {
  0% {
    opacity: 0;
    transform: translateY(0) scale(0.4);
  }

  25% {
    opacity: 0.8;
  }

  100% {
    opacity: 0;

    transform: translateY(-4.5em) translateX(1em) scale(1.7);
  }
}

@keyframes doorOpen {
  0%,
  60%,
  92%,
  100% {
    transform: rotateY(0);
  }

  70%,
  86% {
    transform: rotateY(-80deg);
  }
}

@keyframes doorLight {
  0%,
  60%,
  92%,
  100% {
    opacity: 0;
  }

  70%,
  86% {
    opacity: 1;
  }
}

@keyframes figIn {
  0%,
  68%,
  92%,
  100% {
    transform: translateY(110%);
  }

  76%,
  83%,
  88% {
    transform: translateY(0);
  }

  80%,
  85% {
    transform: translateY(-0.3em);
  }
}

@keyframes sparkle {
  0%,
  58% {
    opacity: 0;

    transform: scale(0.2) rotate(0);
  }

  64% {
    opacity: 1;

    transform: scale(1.3) rotate(90deg);
  }

  80%,
  100% {
    opacity: 0;

    transform: scale(0.3) rotate(200deg);
  }
}

@keyframes keyTrip {
  0% {
    transform: translate(18em, -16em) rotate(30deg) scale(0.6);

    opacity: 0;
  }

  8% {
    opacity: 1;
  }

  22% {
    transform: translate(10em, -9em) rotate(-8deg) scale(1);
  }

  38% {
    transform: translate(1em, -0.6em) rotate(0);
  }

  46%,
  52% {
    transform: translate(4.6em, 0) rotate(0);
  }

  57% {
    transform: translate(4.6em, 0) rotate(-45deg);
  }

  62%,
  74% {
    transform: translate(4.6em, 0) rotate(0);

    opacity: 1;
  }

  90%,
  100% {
    transform: translate(18em, -16em) rotate(30deg) scale(0.6);

    opacity: 0;
  }
}

@keyframes signFlip {
  0%,
  62% {
    transform: rotateY(0);
  }

  72%,
  92% {
    transform: rotateY(180deg);
  }

  100% {
    transform: rotateY(360deg);
  }
}

@keyframes confetti {
  0%,
  62% {
    opacity: 0;

    transform: translate(0, 0) rotate(0);
  }

  66% {
    opacity: 1;
  }

  76% {
    opacity: 1;

    transform: translate(var(--x), var(--y)) rotate(var(--r));
  }

  92%,
  100% {
    opacity: 0;

    transform: translate(calc(var(--x) * 1.15), calc(var(--y) + 10em))
      rotate(calc(var(--r) * 1.6));
  }
}

@keyframes fly {
  0% {
    transform: translate(0, 0);
  }

  25% {
    transform: translate(11em, -1.4em);
  }

  50% {
    transform: translate(23em, 0.6em);
  }

  75% {
    transform: translate(34em, -0.9em);
  }

  100% {
    transform: translate(42em, 0);
  }
}

@keyframes flapL {
  from {
    transform: rotate(20deg);
  }

  to {
    transform: rotate(-16deg);
  }
}

@keyframes flapR {
  from {
    transform: rotate(-20deg);
  }

  to {
    transform: rotate(16deg);
  }
}

@keyframes winPulse {
  0%,
  56%,
  94%,
  100% {
    filter: none;
  }

  66%,
  88% {
    filter: brightness(1.15);

    box-shadow: 0 0 2.6em var(--win-glow);
  }
}

@keyframes logoBob {
  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-2px);
  }
}

@keyframes logoHop {
  0% {
    transform: none;
  }

  35% {
    transform: translateY(-5px) scale(1.06, 0.94);
  }

  60% {
    transform: scale(0.96, 1.05);
  }

  100% {
    transform: none;
  }
}

@keyframes logoDoor {
  0%,
  68%,
  100% {
    background: #14213d;
  }

  76%,
  92% {
    background: #fff3c4;
  }
}

@keyframes smokeSm {
  0% {
    opacity: 0;

    transform: translateY(0) scale(0.4);
  }

  30% {
    opacity: 0.8;
  }

  100% {
    opacity: 0;

    transform: translateY(-1.6em) translateX(0.4em) scale(1.5);
  }
}

@keyframes dash {
  to {
    background-position-x: 2.8em;
  }
}

@keyframes drive {
  from {
    transform: translateX(0);
  }

  to {
    transform: translateX(50em);
  }
}

@keyframes bob {
  from {
    translate: 0 0;
  }

  to {
    translate: 0 -0.12em;
  }
}

@keyframes lampOn {
  0%,
  60%,
  94%,
  100% {
    filter: none;

    box-shadow: 0 0 0.6em var(--win-glow);
  }

  70%,
  88% {
    filter: brightness(1.25);

    box-shadow: 0 0 1.8em 0.4em var(--win-glow);
  }
}

@keyframes shoot {
  0%,
  78% {
    opacity: 0;
    transform: translateX(0);
  }

  80% {
    opacity: 1;
  }

  90%,
  100% {
    opacity: 0;

    transform: translateX(-16em);
  }
}

@keyframes halo {
  from {
    opacity: 0.5;
    transform: scale(0.9);
  }

  to {
    opacity: 1;
    transform: scale(1.15);
  }
}

@keyframes houseIn {
  from {
    opacity: 0;

    transform: translateY(3em) scale(0.92);
  }
}

@keyframes signIn {
  from {
    opacity: 0;

    transform: translateY(-2.5em);
  }
}

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

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

@keyframes dropIn {
  from {
    transform: translateY(-14px) scale(0.97);

    opacity: 0;
  }
}

@keyframes slideIn {
  from {
    opacity: 0;

    transform: translateY(22px);
  }
}

@keyframes popIn {
  from {
    opacity: 0;

    transform: scale(0.94) translateY(16px);
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

  .door {
    transform: rotateY(-80deg);
  }

  .glow {
    opacity: 1;
  }

  .fig {
    transform: none;
  }

  .key {
    opacity: 0;
  }

  .star {
    opacity: 0.8;
  }

  .board {
    transform: none;
  }

  .logo {
    animation: none;
  }

  .car {
    display: none;
  }

  .mflag {
    transform: rotate(0);
  }

  .sh-lit {
    opacity: 1;
  }
}
</style>
