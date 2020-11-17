<template>
  <section>
    <div class="container">
      <div class="button-list">
        <div class="btn btnPrimary" @click="prevPage">&#8592;</div>
        <div class="btn btnPrimary" @click="nextPage">&#8594;</div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
    props: {
        repos: {
            type: Array,
        },
        page: {
            type: Object
        }
    },

    methods: {
         nextPage() {
      if (this.page.current * this.page.length < this.repos.length)
        this.page.current += 1;
    },
    prevPage() {
      if (this.page.current > 1) this.page.current -= 1;
    }
    }
}
</script>

<style lang="scss" scoped>
.btn {
  &.btnPrimary {
    background-color: rgba(122, 122, 122, 0.5);
    border-color: rgba(3, 3, 3, 0.89);
  }
}
</style>