<template>

  <main>
    <IdeaHeader
      @addIdea="addIdea"
    />

    <section class="ideas">

      <label for="search">Search</label>
      <input v-model="searchTerm" id="search" @keyup="filterIdeas()" />

      <label for="sort">Sort By:</label>
      <select v-model="sortBy" id="sort" @change="sortIdeas">
        <option value="newest" selected>Newest</option>
        <option value="oldest">Oldest</option>
        <option value="highest">Highest Quality</option>
        <option value="lowest">Lowest Quality</option>
      </select>

      <Idea
        v-for="idea in visibleIdeas"
        :key="idea.id"
        :idea="idea"
        @deleteIdea="deleteIdea"
        @updateIdea="updateIdea"
      />

    </section>

  </main>

</template>

<script>
import IdeaHeader from './IdeaHeader';
import Idea from './Idea';

export default {
  name: 'IdeaBox',
  components: {
    IdeaHeader,
    Idea,
  },

  data() {
    return {
      searchTerm: '',
      sortBy: 'newest',
      ideas: [],
      visibleIdeas: [],
      title: '',
      body: '',
    };
  },

  mounted() {
    this.loadStoredIdeas();
  },

  methods: {
    addIdea(idea) {
      this.ideas.unshift(idea);
      this.storeIdeas();
    },

    storeIdeas() {
      localStorage.setItem('ideas', JSON.stringify(this.ideas));
      this.sortIdeas();
    },

    loadStoredIdeas() {
      this.ideas = JSON.parse(localStorage.getItem('ideas')) || [];
      this.sortIdeas();
    },

    deleteIdea(ideaID) {
      const id = parseInt(ideaID, 10);

      this.ideas = this.ideas.filter(idea => idea.id !== id);
      this.storeIdeas();
    },

    filterIdeas() {
      this.visibleIdeas = this.ideas.filter(
        idea =>
          idea.title.value.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
          idea.body.value.toLowerCase().includes(this.searchTerm.toLowerCase()),
      );
    },

    sortIdeas() {
      if (this.sortBy === 'newest') {
        return this.sortNewest();
      }
      if (this.sortBy === 'oldest') {
        return this.sortOldest();
      }
      if (this.sortBy === 'highest') {
        return this.sortHighestQuality();
      }
      if (this.sortBy === 'lowest') {
        return this.sortLowestQuality();
      }
      return null;
    },

    sortHighestQuality() {
      this.visibleIdeas = this.ideas.sort((a, b) => a.quality < b.quality);
    },

    sortLowestQuality() {
      this.visibleIdeas = this.ideas.sort((a, b) => a.quality > b.quality);
    },

    sortNewest() {
      this.visibleIdeas = this.ideas.sort((a, b) => a.id < b.id);
    },

    sortOldest() {
      this.visibleIdeas = this.ideas.sort((a, b) => a.id > b.id);
    },

    updateIdea(idea) {
      const index = this.ideas.findIndex(storedIdea => storedIdea.id === idea.id);

      this.ideas[index] = idea;
      this.storeIdeas();
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
