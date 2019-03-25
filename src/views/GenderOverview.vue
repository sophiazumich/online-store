<template>
  <div class="wrapper">
    <h1>{{ gender }}</h1>

    <select class="btn--grey" v-model="sortDirection">
      <option
        v-for="option in sortOrderArray"
        :key="option.text"
        :value="option.value">
        {{ option.text }}
      </option>
    </select>
    <ul class="wrapper item-grid">
        <li v-for="product in productsByGender" :key="product.id" class="item-grid__item">
          <router-link :to="{ name: 'product', params: { id: product.id}}">
            <img class="product-image" :src="makeImagePath(product)" alt="">
            <p class="product-title">{{ product.name }}</p>
            <p><em>${{ product.price }}</em></p>
          </router-link>
        </li>
    </ul>
    <div class="wrapper random-items-wrapper">
      <h2>Our Recommendations</h2>
      <p>Try these on for size!</p>
      <section class="random-items">
        <router-link :to="{ name: 'product', params: { id: randomTop.id}}" class="random-items__item">
          <img class="product-image" :src="makeImagePath(randomTop)" alt="">
          <p class="product-title">{{ randomTop.name }}</p>
          <p><em>${{ randomTop.price }}</em></p>
        </router-link>
        <router-link :to="{ name: 'product', params: { id: randomBottom.id}}" class="random-items__item">
          <img class="product-image" :src="makeImagePath(randomBottom)" alt="">
          <p class="product-title">{{ randomBottom.name }}</p>
          <p><em>${{ randomBottom.price }}</em></p>
        </router-link>
        <router-link :to="{ name: 'product', params: { id: randomFootwear.id}}" class="random-items__item">
          <img class="product-image" :src="makeImagePath(randomFootwear)" alt="">
          <p class="product-title">{{ randomFootwear.name }}</p>
          <p><em>${{ randomFootwear.price }}</em></p>
        </router-link>
      </section>
      <button class="btn btn--grey" @click="recommendRandomOutfit">Shuffle</button>
    </div>
  </div>
</template>

<script>
import { imagePath } from '@/mixins/imagePath';

export default {
  name: "genderOverview",
  mixins: [imagePath],
  created() {
    this.recommendRandomOutfit()
  },
  data() {
    return {
      sortDirection: "unsorted",
      sortOrderArray: [
        {
          text: 'Unsorted',
          value: "unsorted"
        },
        {
          text: 'Low to High Price',
          value: "lowToHigh"
        },
        {
          text: 'High to Low Price',
          value: "highToLow"
        }
      ],
      randomTopId: null,
      randomBottomId: null,
      randomFootwearId: null
    }
  },
  computed: {
    gender() {
      return this.$route.params.gender
    },
    productsByGender() {
      let genderProducts = this.$store.getters.productsByGender(this.gender)
      if (this.sortDirection === "lowToHigh"){
        return genderProducts.sort((a,b) => a.price - b.price)
      }
      if (this.sortDirection ==="highToLow"){
        return genderProducts.sort((a,b) => b.price - a.price)
      }
      else {
        return genderProducts
      }
    },
    randomTop() {
      return this.$store.getters.product(this.randomTopId)
    },
    randomBottom() {
      return this.$store.getters.product(this.randomBottomId)
    },
    randomFootwear() {
      return this.$store.getters.product(this.randomFootwearId)
    }
  },
  methods: {
    randomProductIdByCategory(category) {
      let allProductsInCategory = this.productsByGender.filter(p =>  p.category === category);
      let randomIndex = Math.floor(Math.random() * allProductsInCategory.length);
      return allProductsInCategory[randomIndex].id;
    },
    recommendRandomOutfit() {
      this.randomTopId = this.randomProductIdByCategory('Shirts');
      this.randomBottomId = this.randomProductIdByCategory('Pants');
      this.randomFootwearId = this.randomProductIdByCategory('Shoes');
    }
  }
}
</script>

<style lang="scss">
h1{
  text-transform: capitalize;
}

.random-items-wrapper {
  background: #fafafa;
  text-align: center;
  padding: 3rem;
}
.random-items {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.random-items__item {
  flex: 0 0 33%;
}

.item-grid {
  list-style: none;
  padding-left: 0;
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  flex-wrap: wrap;
}
.item-grid__item {
  box-sizing: border-box;
  text-align: center;
  padding: 1rem;
  flex: 0 0 33.3%;
  @media only screen and (max-width: 832px) {
    flex: 0 0 50%;
  }
  @media only screen and (max-width: 475px) {
    flex: 0 0 100%;
  }
}

.btn, select {
  padding: .5rem .75rem;
  border-radius: 3px;
  border: none;
  background-color: transparent;
  font-size: .9rem;
  font-weight: bold;
  cursor: pointer;
  transition: all .15s ease;
}
select{
  height: 2rem;
}
.btn--grey {
  background-color: #2c3e50;
  color: #FFF;
  &:hover, &:focus {
    background-color: #42b983;
  }
}
</style>
