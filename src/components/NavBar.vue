<script setup lang="ts">
import { langIconMap } from "@/i18n";
import { useI18n } from "vue-i18n";
const lange = useStorage("lang", "en-US");
const { locale } = useI18n(); // 获取当前语言
const langIcon = computed(() => {
  return langIconMap[locale.value];
});

function toTop() {
  window.scrollTo({
    top: 0,
    behavior: "smooth",
  });
}

const handleLangChange = () => {
  let lang;
  if (locale.value === "zh-CN") {
    lang = "en-US";
  } else {
    lang = "zh-CN";
  }
  locale.value = lang;
  lange.value = lang;
  // // 强制刷新页面
  window.location.reload();
};

const { y: scroll } = useWindowScroll();
</script>

<template>
  <header class="header z-40">
    <RouterLink
      class="w-8 h-8 absolute xl:fixed m-7 mb-0 select-none outline-none flex flex-items-center"
      to="/"
      focusable="false"
    >
      <Logo />
    </RouterLink>

    <button
      title="Scroll to top"
      fixed
      right-3
      bottom-3
      w-10
      h-10
      hover:op100
      rounded-full
      hover-bg-hex-8883
      transition
      duration-300
      z-100
      print:hidden
      :class="scroll > 300 ? 'op30' : 'op0! pointer-events-none'"
      @click="toTop()"
    >
      <div i-ri-arrow-up-line />
    </button>
    <nav class="nav">
      <div class="spacer" />
      <div class="right" print:op0>
        <RouterLink to="/navs" :title="$t('nav.navs', 'Navs')">
          <span class="lt-md:hidden">{{ $t("nav.navs", "Navs") }}</span>
          <div i-material-symbols-bottom-navigation-outline class="md:hidden" />
        </RouterLink>
        <RouterLink to="/posts" :title="$t('nav.Blog', 'Blog')">
          <span class="lt-md:hidden">{{ $t("nav.Blog", "Blog") }}</span>
          <div i-ri-article-line class="md:hidden" />
        </RouterLink>
        <!-- <VMenuWrap
          :title="$t('nav.Blog', 'Blog')"
          icon="i-ri-article-line"
          :menus="[
            {
              link: '/posts',
              name: `${$t('nav.article', 'Notes')}`,
            },
            {
              link: 'https://m.ziliao88.top',
              name: $t('nav.blog.old', 'Blog [deprecated]'),
            },
          ]"
        /> -->
        <RouterLink to="/projects" :title="$t('nav.Projects', 'Projects')">
          <span class="lt-md:hidden">{{ $t("nav.Projects", "Projects") }}</span>
          <div i-ri-lightbulb-line class="md:hidden" />
        </RouterLink>
        <RouterLink to="/demos" :title="$t('nav.Demos', 'Demos')">
          <span class="lt-md:hidden">{{ $t("nav.Demos", "Demos") }}</span>
          <div i-ri-screenshot-line class="md:hidden" />
        </RouterLink>
        <!-- 学习 -->
        <!-- <VMenuWrap
          :title="$t('nav.learning', 'Learning')"
          icon="i-fluent-microscope-20-regular"
          :menus="[
            {
              link: '/interview',
              name: $t('nav.learn.interview', 'Interview tips'),
            },
          ]"
        /> -->

        <a href="javascript:void(0)" class="relative">
          <div class="i-mynaui-search-square w-1.3em h-1.3em"></div>
          <AlgoliaSearchBox
            class="absolute top-0 left-0 w-full h-full op0!"
            :algolia="{
              apiKey: 'db0e9b82d77e75c9fc8aee05b1e14334',
              indexName: 'ryan',
              appId: 'X0NE0GCGVB',
            }"
          />
        </a>
        <a
          href="https://github.com/rr210"
          target="_blank"
          title="GitHub"
          class="lt-md:hidden"
        >
          <div i-uil-github-alt />
        </a>
        <a class="flex items-center" href="javascript:void(0)">
          <div
            @click="handleLangChange"
            :class="langIcon"
            v-tooltip="$t('lang.change', 'English To Chinese')"
          ></div>
        </a>
        <ToggleTheme />
      </div>
    </nav>
  </header>
</template>

<style scoped>
.header h1 {
  margin-bottom: 0;
}

.logo {
  position: absolute;
  top: 1.5rem;
  left: 1.5rem;
}

.nav {
  padding: 2rem;
  width: 100%;
  display: grid;
  grid-template-columns: auto max-content;
  box-sizing: border-box;
}

.nav > * {
  margin: auto;
}

.nav img {
  margin-bottom: 0;
}

.nav :deep(a) {
  cursor: pointer;
  text-decoration: none;
  color: inherit;
  transition: opacity 0.2s ease;
  opacity: 0.6;
  outline: none;
}

.nav :deep(a):hover {
  opacity: 1;
  text-decoration-color: inherit;
}

.nav .right {
  display: grid;
  grid-gap: 1.2rem;
  grid-auto-flow: column;
}

.nav .right > * {
  margin: auto;
}
</style>
