<template>
  <div :class="[{flexStart: step === 1}, 'wrapper']">
    <transition name="slide">
      <img src="../assets/download.svg" class="logo" v-if="step === 1">
    </transition>
    <transition name="fade">
      <HeroImage v-if="step === 0"/>
    </transition>
    <Claim v-if="step === 0"/>
    <SearchInput v-model="searchValue" @input="handleInput" :dark="step === 1"/>
    <div class="results" v-if="results && !loading && step === 1">
      <Item v-for="item in results" :item="item" :key="item.data[0].nasa_id"/>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';
import Claim from '@/components/Claim.vue';
import SearchInput from '@/components/SearchInput.vue';
import HeroImage from '@/components/HeroImage.vue';
import Item from '@/components/Item.vue';

// @ is an alias to /src
const API = 'https://images-api.nasa.gov/search';
export default {
  name: 'App',
  components: {
    Claim, SearchInput, HeroImage, Item,
  },
  data() {
    return {
      loading: false,
      step: 0,
      searchValue: '',
      results: [],
    };
  },
  methods: {
    handleInput: debounce(function () {
      this.loading = true;
      axios.get(`${API}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = (response.data.collection.items);
          this.loading = false;
          this.step = 1;
        })
        .catch((error) => {
          console.log(error);
        });
    }, 500),
  },
};
</script>

<style lang="scss" scoped>
  .wrapper{
    display: flex;
    position: relative;
    margin: 0;
    width: 100%;
    height: 100vh;
    padding: 30px;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    &.flexStart{
      justify-content: flex-start;
    }
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s ease;
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }

  .logo{
    position: absolute;
    top: 20px;
  }

  .slide-enter-active, .slide-leave-active {
    transition: margin-top .5s ease;
  }
  .slide-enter, .slide-leave-to /* .fade-leave-active below version 2.1.8 */ {
    margin-top: -50px;
  }

  .results {
  margin-top: 50px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 20px;
  @media (min-width: 768px) {
    width: 90%;
    grid-template-columns: 1fr 1fr 1fr;
  }
}
</style>
