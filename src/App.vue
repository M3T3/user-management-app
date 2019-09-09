<template>
  <div>
    <header class="header d-flex">
      <div class="container">
        <div class="row">
          <div class="col-12">
            <p class="header-title">User Management System</p>
          </div>
        </div>
      </div>
    </header>
    <Home></Home>
  </div>
</template>

<script>
import Home from '@/components/Home.vue'
export default {
  name: "App",
  components: {
    Home
  }
};
</script>

<style lang="scss">
@import 'assets/mixins.scss';
html, body {
  overflow-x: hidden;
}

.header {
  padding: 10px 0;
  background: $header-bg;

  &-title {
    color: $white;
    font-size: 14px;
    margin: 0;
    font-weight: 400;
    text-align: center;
  }
}
</style>