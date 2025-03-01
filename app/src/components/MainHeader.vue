<template>
 <!--title-->
 <main class="text-center">
 <h1 class="font-bold text-6xl">ホステル日記</h1>
 <p>オークランド、シドニー、チェンマイ編</p>
 </main>

 <!--お知らせバー-->
 <div class="overflow-hidden w-full bg-blue-500">
  <div class="whitespace-nowrap animate-marquee text-white text-xl font-normal">
   前の章の内容が前提で書かれている内容もありますー。
  </div>
 </div>
</template>

<script>
export default {
 name: 'MainHeader',
 props: {
   //msg: String
 }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@keyframes marquee {
 0% {
   transform: translateX(100%);
 }
 100% {
   transform: translateX(-100%);
 }
}
.animate-marquee {
 animation: marquee 10s linear infinite;
}
</style>
