<template>
  <div class=" py-10">
    <div class="relative border-b-slate-300 py-3 px-5">
      <!-- 标签项 -->
      <div class="flex gap-10" ref="tabs">
        <div class="item text-xl cursor-pointer" @click="() => updateSlider(0)">
          在线玩家
        </div>
      </div>
      <!-- 滑块 -->
      <div
        class="absolute bottom-0 h-1 bg-rose-600 transition-all duration-200"
        :style="{
          width: sliderStyle.width + 'px',
          left: sliderStyle.left + 'px',
        }"
      ></div>
    </div>
    <div>
      <div class="flex flex-wrap gap-7 py-5 px-5" v-if="pending">
        <div
          v-for="i in 6"
          :key="i"
          class="flex items-center w-[100%] md:w-[30%] border p-4 gap-4 bg-slate-50 rounded-xl"
        >
          <div class="rounded-full bg-gray-300 h-16 w-16"></div>
          <div class="flex-1 space-y-6 py-1">
            <div class="h-4 bg-gray-300 rounded"></div>
            <div class="h-4 bg-gray-300 rounded w-5/6"></div>
          </div>
        </div>
      </div>

      <div
        v-if="!pending && posts && posts.players && posts.players.list"
        class="flex flex-wrap gap-7 py-5 px-5"
      >
        <div
          v-for="(player, index) in posts.players.list"
          :key="index"
          class="flex items-center w-[100%] md:w-[30%] border p-4 gap-4 bg-slate-50 rounded-xl transition-all duration-300"
        >
          <img
            :src="`https://mc-heads.net/avatar/${player.name_raw}`"
            alt="Your Avatar"
            class="rounded-full border h-16 w-16"
          />
          <div class="text-xl break-all">{{ player.name_raw }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const sliderStyle = ref({ width: 0, left: 0 });
const tabs = ref(null);
const props = defineProps({
  pending: Boolean,
  posts: Array
});


const updateSlider = (index) => {
  if (tabs.value && tabs.value.children[index]) {
    const tab = tabs.value.children[index];
    sliderStyle.value = {
      width: tab.offsetWidth - 20,
      left: tab.offsetLeft + 10,
    };
  }
};

onMounted(() => {
  updateSlider(0); // 初始设置滑块位置
});
</script>

<style lang="scss" scoped>
</style>