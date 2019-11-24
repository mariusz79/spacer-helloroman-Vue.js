<template>
  <div class="wrapper">
    <HeroImage/>
    <Claim/>
    <SearchInput v-model="searchValue" @input="handleInput"/>
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';
import Claim from '@/components/Claim.vue';
import SearchInput from '@/components/SearchInput.vue';
import HeroImage from '@/components/HeroImage.vue';

// @ is an alias to /src
const API = 'https://images-api.nasa.gov/search';
export default {
  name: 'App',
  components: { Claim, SearchInput, HeroImage },
  data() {
    return {
      searchValue: '',
      results: [],
    };
  },
  methods: {
    handleInput: debounce(function () {
      axios.get(`${API}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = (response.data.collection.items);
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
    margin: 0;
    width: 100%;
    height: 100vh;
    padding: 30px;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
</style>
