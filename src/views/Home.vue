<template>
  <div>
    <div class="title">
      <h1>Dogs Breed Browser</h1><i>-Made by <a href="https://github.com/NourdineMazali">Nourdine Mazali</a></i>
    </div>
    <div class="container">
      <input
        v-model="breedComputed"
        type="text"
        name="breed-search"
        placeholder="Search Dog Breed..."
        id="breed-search"
      >
    </div>
    <div class="container--flex">
      <breed-card
        v-for="breed in filterBreeds"
        :key="breed.id"
        :breed="breed.type"
        :sub-breeds="breed.subBreeds"
      >
      </breed-card>
    </div>
  </div>
</template>
<script>
import BreedCard from '@/components/BreedCard'
import { mapActions, mapState } from 'vuex'
import _ from 'lodash'
export default {
  name: 'Home',
  components: {
    BreedCard
  },
  data () {
    return {
      breedInfo: '',
      dbBreedInfo: ''
    }
  },
  computed: {
    ...mapState(['breeds']),
    filterBreeds () {
      if (this.breeds.length) {
        return this.breeds.filter(b =>
          b.type.includes(this.breedComputed) ||
          b.subBreeds.find(sb => sb.includes(this.breedComputed))
        )
      }
    },
    breedComputed: {
      get() {
        return this.breedInfo
      },
      set:_.debounce(function(val) {
        this.breedInfo = val
      }, 300)
    }
  },
  async mounted () {
    await this.fetchBreeds()
  },
  methods: {
    ...mapActions(['fetchBreeds']),
  }
}
</script>

<style scoped>
  .container {
    padding: 2em;
  }
  input[type="text"] {
    padding:1em 1em;
    width: 20%;
    border: 2px solid#E7F1E9;
    font-size:.8em;
    border-radius:5px;
  }
  .container--flex {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  .title {
    margin-left: 1em;
  }

  @media all and (max-width: 743px) {
    .container {
      padding: .5em;
    }
  }
</style>
