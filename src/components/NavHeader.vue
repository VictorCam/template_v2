<script setup>
import { ref, onMounted } from "vue";
import utils from "../utils";
const navBtnRef = ref(null);
const navSvgRef = ref(null);
const themeBtnRef = ref(null);
const themeSvgRef = ref(null);

let links = [
  {
    name: "Link1",
    href: "#Link1",
  },
  {
    name: "Link2",
    href: "#Link2",
  },
];

onMounted(() => {
  utils.loadSavedTheme(themeSvgRef)
})

const toggleTheme = () => {
  // get default theme and set the theme value
  const prefersDark = window.matchMedia && window.matchMedia("(prefers-color-scheme: dark)").matches;
  let theme = prefersDark ? 'light' : 'dark';

  //override the theme value if theres a saved theme
  let savedTheme = localStorage.getItem('theme')
  if (savedTheme) {
    theme = savedTheme === 'light' ? 'dark' : 'light';
  }

  // save the theme and override the color-scheme
  localStorage.setItem('theme', theme);
  document.documentElement.style.setProperty("color-scheme", theme);
};

</script>

<template>
  <header>
    <div class="svg-bw tmp"></div>
    <button ref="themeBtnRef" @click="
      utils.switchButtonIcons(
        { buttonRef: themeBtnRef, svgRef: themeSvgRef },
        { icon1: 'sun', icon2: 'moon' }
      );
    toggleTheme();
    " aria-pressed="false" class="svg-button margin-left-auto" aria-label="Toggle Dark/Light Theme">
      <div ref="themeSvgRef" class="svg-bw "></div>
    </button>
    <button ref="navBtnRef" @click="
      utils.switchButtonIcons(
        { buttonRef: navBtnRef, svgRef: navSvgRef },
        { icon1: 'burger', icon2: 'close' }
      );
    " aria-pressed="false" class="svg-button hide-on-desktop" aria-label="Hamburger Menu">
      <div ref="navSvgRef" class="svg-bw burger"></div>
    </button>
    <nav class="">
      <ul>
        <li v-for="link in links">
          <a :href="link.href">{{ link.name }}</a>
        </li>
      </ul>
    </nav>
  </header>
</template>

<style module>
header {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 5px;
}

button {
  background: none;
  border: none;
}

ul {
  display: flex;
  flex-direction: row;
  gap: 10px;
}

nav {
  margin-right: 10px;
}

@media (max-width: 30em) {
  ul {
    flex-direction: column;
  }
}
</style>
