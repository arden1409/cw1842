<template>
  <div>
    <h1>Show Word</h1>

    <div class="ui labeled input fluid">
      <div class="ui label"><i class="united kingdom flag"></i> English</div>
      <input type="text" disabled :value="word.english" />
    </div>

    <div class="ui labeled input fluid">
      <div class="ui label"><i class="germany flag"></i> German</div>
      <input type="text" disabled :value="word.german" />
    </div>

    <div class="ui labeled input fluid">
      <div class="ui label"><i class="japan flag"></i> Japan</div>
      <input type="text" disabled :value="word.japan" />
    </div>

    <div class="actions">
      <router-link
        :to="{ name: 'edit', params: { id: this.$route.params.id } }"
      >
        Edit word
      </router-link>
    </div>
  </div>
</template>

<script>
import { api } from "../helpers/helper"

export default {
  name: "show",
  data() {
    return {
      word: "",
    };
  },
  async mounted() {
    this.word = await api.getWord(this.$route.params.id);
  },
};
</script>

<style scoped>
.actions a {
  display: block;
  text-decoration: underline;
  margin: 20px 10px;
}
</style>
