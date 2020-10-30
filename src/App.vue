<template>
  <div class="wrapper">
    <!-- <header></header> -->
    <header>
      <div class="navbar">
        <div class="container">
          <div class="navbar-content">
            <div class="logo">GitHub Finder</div>
          </div>
        </div>
      </div>
    </header>

    <router-view></router-view>

    <!-- <footer></footer> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      links: [
        {title: 'Home',url: '/'},
      ]
    }
  }
};
</script>

<style lang="scss">
.navbar-link {
  &.router-link-exact-active {
    color:rgb(134, 117, 228)
  }
}
</style>
