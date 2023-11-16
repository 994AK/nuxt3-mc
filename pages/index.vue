<template>
  <div class="">
    <div class="container w-screen h-screen bg-white md:px-20 flex flex-col  md:flex-row gap-5">
      <div class="flex-1 md:flex-[2] pt-12 pl-4  md:pt-44 md:pl-32 ">
        <div class="flex flex-col gap-2 md:gap-5 md:w-1/2">
          <span class="text-zinc-950 font-serif text-3xl md:text-7xl">Yuhua</span> 
          <span class="font-serif text-xl md:text-5xl text-sky-400">服务器</span> 
          <span class="font-serif text-lg md:text-3xl text-zinc-500 leading-10">
            ⛱️我希望我的服务器不存在熊孩子，可以互相友爱的，共同进步，共同发展
          </span>

          <div class="w-28 flex justify-center items-center p-2 text-xl text-white bg-cyan-700 rounded-lg cursor-pointer active:text-slate-200" @click="navigateToPlayer">查询玩家</div>
        </div> 
      </div>      
      <div class="flex-[1] flex justify-center items-center ">
        <img class="object-cover w-[300px] h-[300px] md:w-[400px] md:h-[400px] rounded-3xl " src="https://common.mentorsc.com/jsy-common-attachment/2023-11-16/1725090800095858688-mc-bg.png"  />  
      </div>   
    </div>
    <div id="player-details" class="container w-screen h-screen md:px-20 md:pl-32 bg-teal-300">
      <players-list :posts="posts" :pending="pending" />
    </div>
  </div>
</template>

<script setup>
const formData = ref({
  page: 1,
  per_page: 10,
  condition: {
    search_type: "LastLoginTime",
  },
});

const router = useRouter();

const { pending, data: posts } = await useLazyFetch(
  "https://api.mcstatus.io/v2/status/java/66aserver.zhongbai233.top",
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


function navigateToPlayer() {
  router.push({ hash: '#player-details' });
}


</script>

<style scoped>
body {
  /* display: flex; */
}
</style>
