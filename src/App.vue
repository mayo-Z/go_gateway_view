<template>
  <div id="app">
    <router-view />
    <div
      style="position:fixed;text-align:center;bottom:0;margin:0 auto;width:100%;color: #5c6b77"
    >
      <a
        target="_blank"
        style="color: #5c6b77"
        href="https://beian.miit.gov.cn/"
        >XICP备XXXXXXXXX号</a
      >
      &nbsp; ©2021-2022 秋叶原の黑猫
    </div>
  </div>
</template>

<script>
export default {
  name: "App"
};
</script>
