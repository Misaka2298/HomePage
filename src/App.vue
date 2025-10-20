<template>
  <!-- 全屏背景图片和毛玻璃遮罩 -->
  <div
    class="app-bg"
    :style="{ backgroundImage: backgroundStyle }"
    aria-hidden="true"
  ></div>
  <div class="app-frost" aria-hidden="true"></div>
  <Transition name="fade" appear>
    <Loading v-if="showLoading" />
  </Transition>
  <main>
    <main-card></main-card>
  </main>
  <div class="reThemeBtn" @click="changeTheme">
    {{ theme == "light" ? "🔆" : "🌙" }}
  </div>
</template>

<script setup>
import cfg from "./config/config.json";
import { computed } from "vue";
import MainCard from "./views/MainCard.vue";
import Loading from "./components/Loading.vue";
import { onMounted, ref } from "vue";

let theme = ref(localStorage.getItem("theme") || "light");
let showLoading = ref(true);
document.body.style.overflow = "hidden";

const backgroundStyle = computed(() => {
  const url = cfg.backgroundImageUrl || "";
  return url ? `url('${url}')` : "none";
});

onMounted(() => {
  document.body.setAttribute("theme", theme.value);
  showLoading.value = false;
  setTimeout(() => {
    document.body.style.overflow = "";
  }, 300);
});

const changeTheme = () => {
  if (theme.value == "light") {
    theme.value = "dark";
    onTheme("dark");
  } else {
    theme.value = "light";
    onTheme("light");
  }

  console.log(theme.value);
};

const onTheme = (theme) => {
  document.body.setAttribute("theme", theme);
  localStorage.setItem("theme", theme);
};
</script>

<style>
@import url(assets/css/App.css);

/* 添加渐隐效果的CSS */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
