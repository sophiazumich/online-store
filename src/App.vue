<template>
  <div id="app">
    <div id="nav" class="wrapper flex-col flex-col--align-center">
      <h1 class="flex-col--2">My Store</h1>
      <div class="flex-col--2 nav-items">
        <router-link :to="{name: 'home'}" class="nav-items__item">Home</router-link>
        <router-link
          :to="{ name: 'gender-overview', params: { gender: 'women'}}"
          class="nav-items__item">Women</router-link>
        <router-link
          :to="{ name: 'gender-overview', params: { gender: 'men'}}"
          class="nav-items__item">Men</router-link>
          <router-link to="/about" class="nav-items__item">About</router-link>
          <router-link :to="{name: 'cart'}" class="nav-items__item">
            Cart
            <counter-badge :count="cartCount"></counter-badge>
          </router-link>
      </div>
    </div>
    <router-view/>
  </div>
</template>

<script>
import CounterBadge from '@/components/CounterBadge';

export default {
  name: 'app',
  components: { CounterBadge },
  computed: {
    cartCount() {
      return this.$store.state.cart.length
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
#nav {
  padding: 30px;
  text-align: center;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
h2{
  text-align: center;
}
.nav-items {
  justify-content: flex-end;
  display: flex;
}
.nav-items__item {
  margin-left: 2rem;
  position: relative;
}
ul {
  padding-left: 0;
  list-style: none;
}
</style>
