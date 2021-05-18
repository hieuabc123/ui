<template>
  <div class="content">
      <router-view></router-view>
  </div>
</template>

<script>
export default {

}
</script>

<style scoped>
.content{
    position: absolute;
    top: 48px;
    left: 180px;
    right: 0;
    bottom: 0;
    box-sizing: border-box;
    background: #f8f8f8;
    overflow: hidden;
    
}
</style>