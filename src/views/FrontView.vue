<template>
  <FrontNavbar></FrontNavbar> <!--封裝元件-->
  <router-view></router-view>
</template>

<script>
import FrontNavbar from '@/components/FrontNavbar.vue';

export default {
  components: {
    FrontNavbar,
  },
};
</script>
