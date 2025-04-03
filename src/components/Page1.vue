<template>
  <div class="page" id="page1">
    <h1>Profile</h1>
    <p>Chayakorn Phuttharaksa</p>
    <a href="#" class="button" @click.prevent="$emit('navigate', 3)">เข้าสู่เนื้อหา</a>
  </div>
</template>

<script>
export default {
  name: 'Page1'
};
</script>