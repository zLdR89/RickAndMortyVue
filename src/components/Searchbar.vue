<script setup>
import ResultItem from './ResultItem.vue'
import SuggestionsListItem from './SuggestionsListItem.vue'
import IconSearch from './icons/IconSearch.vue'
import IconClose from './icons/IconClose.vue'
</script>

<template>
  <div class="searchform">
    <IconSearch />
    <!-- Search Input -->
    <input v-model="searchInput" list="charcater-list" v-on:input="onInput" v-on:keydown="onKeydown"
      name="search-input-selection" id="search-input-selection" class="form-control" :placeholder="placeholder" />
    <!-- HTML5 Autocomplete list by datalist -->
    <datalist id="charcater-list" v-if="data && !showResults && !selectedResult">
      <option v-for="item in autocomplete" v-bind:key="item" v-bind:value="item" v-bind:label="item">
      </option>
    </datalist>
  </div>
  <!-- Error Message if there are no entries -->
  <div v-if="errorMsg" class="errorMsg">{{ errorMsg }}</div>
  <!-- Resultlist -->
  <div v-if="showResults && data && data.results && !selectedResult" class="suggestions">
    <ul>
      <li v-for="result in data.results" :item="result" :key="result" :data-id="result.id"
        v-on:click="onSuggestionClick(result.id)">
        <SuggestionsListItem :item="result" />
      </li>
    </ul>
  </div>
  <!-- Resultitem -->
  <div class="selectedResult" v-if="selectedResult">
    <IconClose class="closeIcon" v-on:click="closeResult" />
    <ResultItem :item="selectedResult" />
  </div>
  <!-- degub only  -->
  <!-- <pre v-if="showResults && data"><code>{{ JSON.stringify(data.info, undefined, 4) }}</code></pre> -->
  <!-- <pre v-if="showResults && data"><code>{{ JSON.stringify(data.results, undefined, 4) }}</code></pre> -->
</template>

<script>
import axios from "axios";
const ENTER_KEYCODE = 13;
const KEYCODE_UNDEFINED = undefined;
export default {
  name: 'searchform',
  data() {
    return {
      searchInput: '',
      placeholder: 'Find yout favourite Charakter!',
      charactersUrl: "https://rickandmortyapi.com/api/character",
      debounceMilliseconds: 150,
      data: null,
      selectedResult: null,
      showResults: false,
      errorMsg: ""
    }
  },
  methods: {
    // loads data from rickandmorty api with axios
    async getData(name) {
      clearTimeout(this.timeout);
      this.timeout = setTimeout(() => {
        this.data = null;
        this.autocomplete = null;
        this.errorMsg = "";
        const charactersPromise = axios.get(this.charactersUrl, {
          params: {
            name: name
          }
        });
        Promise.all([charactersPromise]).then(response => {
          this.data = response[0].data;
          this.autocomplete = this.data.results.map(item => item.name).filter((x, i, a) => a.indexOf(x) == i);
        }).catch(error => {
          this.data = null;
          this.errorMsg = "Could not find any suggestions!";
          this.closeResult();
        })
      }, this.debounceMilliseconds);
    },
    // get local result from suggestions
    getResultByID(id) {
      return this.data.results.find(result => result.id == id);
    },
    // click on suggestion item
    onSuggestionClick(id) {
      this.selectedResult = this.getResultByID(id);
    },
    // toggles the result view of one item
    toggleShowResults(show) {
      this.showResults = show === false || show === true ? show : !this.showResults;
      if (this.showResults)
        this.closeResult();
    },
    closeResult() {
      this.selectedResult = null;
    },
    async onInput(e) {

      await this.getData(this.searchInput);
    },
    // on key down in search input
    async onKeydown(e) {
      await this.getData(this.searchInput);
      // toggles show result on enter keycode and undefined (mouseclick on auto suggestion)
      this.toggleShowResults(e.which === ENTER_KEYCODE || e.which === KEYCODE_UNDEFINED);
    },
  },
  // created() {
  //   this.getData();
  // }
}
</script>

<style scoped>
.searchform {
  display: flex;
  height: 48px;
  background: #fff;
  border: 1px solid transparent;
  box-shadow: 0 2px 5px 1px rgb(64 60 67 / 16%);
  border-radius: 24px;
  /* flex: 100%; */
  padding: 2px 0 0 20px;
  place-items: top;
  margin: 20px 0;
}

.closeIcon {
  position: absolute;
  right: 0;
  top: 0;
  z-index: 3;
  display: block;
  cursor: pointer;
  opacity: 0.5;
}

.closeIcon:hover {
  opacity: 1;
}

.errorMsg {
  color: red;
  text-align: center;
}

.suggestions {
  max-height: 400px;
  overflow-y: auto;
  padding: 10px;
}

ul {
  padding: 0;
}

li {
  list-style: none;
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 24px;
  height: 100px;
  margin-bottom: 5px;
  cursor: pointer;
  display: block;
}

li:nth-child(odd) {
  background-color: rgba(255, 255, 255, 0.05);
}

li:focus,
li:hover {
  background-color: hsla(160, 100%, 37%, 1) !important;
  color: black;
}

li:focus a,
li:hover a {
  color: white;
}

.searchform svg {
  position: absolute;
  left: 15px;
  top: 7.5px;
  z-index: 3;

}


.searchform input {
  background-color: transparent;
  -webkit-tap-highlight-color: transparent;
  border: none;
  margin: 0 30px;
  padding: 10px 15x 10px 15px;
  display: flex;
  width: 100%;
  font-size: 1.25rem;
}

.searchform input:focus {
  outline: none !important;
}
</style>


