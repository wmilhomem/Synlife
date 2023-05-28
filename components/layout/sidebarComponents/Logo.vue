<template>
  <div class="logo">
    <router-link :to="dashboardPath" replace>
      <img :src="source" :alt="name" />
    </router-link>
  </div>
</template>
<script>
import { frontendURL } from 'dashboard/helper/URLHelper';

export default {
  props: {
    source: {
      type: String,
      default: '',
    },
    name: {
      type: String,
      default: '',
    },
    accountId: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    dashboardPath() {
      return frontendURL(`accounts/${this.accountId}/dashboard`);
    },
  },
};
</script>
<style lang="scss" scoped>
$logo-size-width: 4.4rem;
$logo-size-height: 1.7rem;

.logo {
  width: 6.4rem;
  height: 6.4rem;
  display: grid;
  place-items: center;
  border-bottom: 2px solid #F4F4F4;
  img {
    width: $logo-size-width;
    height: $logo-size-height;
  }
}
</style>
