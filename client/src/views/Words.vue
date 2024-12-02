<template>
  <div>
    <div>
      <div class="ui large icon input" style="display: flex; justify-content: center; padding-left: 80px; width: 90%;">
        <input type="text" v-model="searchTerm" placeholder="Search for word...">
        <i class="inverted circular search link icon" @click="filterVocabs"></i>
      </div>

      <div style="text-align: left; margin-top: 10px; padding-left: 120px">
              <h3>Search by: </h3>
              <select v-model="searchBy" class="styled-dropdown">
                  <option value="english">English</option>
                  <option value="german">German</option>
                  <option value="japan">Japan</option>
              </select>
              <i class="dropdown icon"></i>
      </div>
    </div>

    <div>
      <h1>Words</h1>
      <table id="words" class="ui celled compact table">
        <thead>
          <tr>
            <th>English</th>
            <th>German</th>
            <th>Japan</th>
            <th colspan="3">Menu</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(word, i) in filteredWords" :key="i">
            <td>{{ word.english }}</td>
            <td>{{ word.german }}</td>
            <td>{{ word.japan }}</td>
            <td width="75" class="center aligned">
              <router-link :to="{ name: 'show', params: { id: word._id } }">
                Show
              </router-link>
            </td>
            <td width="75" class="center aligned">
              <router-link :to="{ name: 'edit', params: { id: word._id } }">
                Edit
              </router-link>
            </td>
            <td
              width="75"
              class="center aligned"
              @click.prevent="onDelete(word._id)"
            >
              <a href="`/words/${word._id}`"> Delete</a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

  </div>
</template>

<script>
import { api } from "../helpers/helper"

export default {
  name: "words",
  data() {
    return {
      words: [],
      searchTerm: '',
      filteredWords: [],
      searchBy: 'english',  // Default search option
    };
  },
  methods: {
    async onDelete(id) {
      const sure = window.confirm("Are you sure ?");
      if (!sure) return;
      await api.deleteWord(id);
      this.flash("Word deleted successfully", "success");
      const newWords = this.words.filter((word) => word._id !== id);
      this.words = newWords;
    },
    filterVocabs() {
            let languageToFilter = this.words;

            // Filtering based on the search term
            if (this.searchTerm) {
                if (this.searchBy === 'english') {
                    languageToFilter = languageToFilter.filter(word =>
                        word.english.toLowerCase().includes(this.searchTerm.toLowerCase())
                    );
                } else if (this.searchBy === 'german') {
                  languageToFilter = languageToFilter.filter(word =>
                        word.german.toLowerCase().includes(this.searchTerm.toLowerCase())
                    );
                } else if (this.searchBy === 'japan') {
                  languageToFilter = languageToFilter.filter(word =>
                        word.japan.toLowerCase().includes(this.searchTerm.toLowerCase())
                    );    
                }
            }
            // Assigning the filtered and sorted tasks to filteredTasks
            this.filteredWords = languageToFilter;
          }
  },
  async mounted() {
    this.words = await api.getWords();
    this.filteredWords = this.words;        // Show all words by default
  },
};
</script>
