<template>
  <div class="bg-teal-300">
    <div class="mx-auto max-w-4xl py-10">
      <div class="relative border-b-slate-300 py-3 px-5">
        <!-- 标签项 -->
        <div class="flex gap-10" ref="tabs">
          <div
            class="item text-xl cursor-pointer"
            @click="() => updateSlider(0)"
          >
            在线玩家
          </div>
          <!-- <div class="item text-xl cursor-pointer" @click="() => updateSlider(1)">
          金币玩家
        </div> -->
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
        <div class="flex flex-wrap gap-5  py-3 px-5" v-if="pending">
          <div
            v-for="i in 6"
            :key="i"
            class="flex items-center w-full md:w-1/4 bg-white border p-4 gap-4"
          >
            <div class="rounded-full bg-gray-300 h-16 w-16"></div>
            <div class="flex-1 space-y-6 py-1">
              <div class="h-4 bg-gray-300 rounded"></div>
              <div class="h-4 bg-gray-300 rounded w-5/6"></div>
            </div>
          </div>
        </div>
        <div class="flex flex-wrap gap-7 py-5 px-5" v-else-if="!pending && posts && posts.players && posts.players.list">
          <div
            v-for="(player, index) in posts.players.list"
            :key="index"
            class="flex items-center w-[100%] md:w-[30%] border p-4 gap-4 bg-slate-50 rounded-xl"
          >
            <img
              :src="`https://api.mineatar.io/face/${player.uuid}?scale=16`"
              alt="Your Avatar"
              class="rounded-full border h-16 w-16"
            />
            <div class="text-xl break-all">{{ player.name_raw }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const sliderStyle = ref({ width: 0, left: 0 });
const tabs = ref(null);
const formData = ref({
  page: 1,
  per_page: 10,
  condition: {
    search_type: "LastLoginTime",
  },
});

const { pending, data: posts } = await useLazyFetch(
  "https://api.mcstatus.io/v2/status/java/52mc.top:911",
  {
    lazy: true,
    server: false,
    transform: ({ version, players, motd }) => {
      return {
        version,
        players,
        motd,
      };
    },
  }
);

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

<style scoped>
body {
  /* display: flex; */
}
</style>
