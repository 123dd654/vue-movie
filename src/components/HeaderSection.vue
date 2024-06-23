<template>
  <header id="header" role="banner" @click="goToHome">
    <div class="header__inner container">
      <h1><a href="/">Film Chart</a></h1>
      <div class="search">
        <input type="search" v-model="searchQuery" @keyup.enter="goToSearch" />
        <span class="icon"></span>
        <button @click="goToSearch">검색</button>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: ''
    }
  },
  methods: {
    goToHome() {
      this.$router.push({ name: 'HomeView' })
    },
    goToSearch() {
      this.$router.push({ name: 'search', query: { q: this.searchQuery } })
    }
  }
}
</script>

<style lang="scss">
#header {
  width: 100%;
  height: 60px;
  background-color: #16182f;

  .header__inner {
    background-color: #16182f;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 100%; /* Ensure it takes the full height of the header */

    h1 {
      font-weight: 900;
      font-size: 2rem;
      cursor: pointer;

      span {
        color: aquamarine;
      }
    }

    .search {
      display: flex;
      align-items: center; /* Center the items vertically */

      input {
        width: 360px;
        border-radius: 10px;
        background-color: var(--mainBg);
        padding: 8px;
        color: var(--white);
        border: 1px solid var(--white);
      }

      button {
        padding: 6px 20px;
        margin-left: 10px;
        border-radius: 6px;
        border: 1px solid var(--white);
        background-color: #16182f;
        color: var(--white);
        font-size: 1.1rem;
        cursor: pointer;

        &:hover {
          background-color: var(--white);
          color: #16182f;
        }
      }
    }
  }
}
</style>
