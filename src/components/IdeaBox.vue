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

      <article v-for="idea in visibleIdeas" class="idea-card" :key="idea.id">
        <h3>
          {{idea.title}}
        </h3>

        <input class="edit" type="text"
          v-model="idea.title.value"
          v-show="idea.title.editing == true"
          @blur="idea.title.editing = false; doneEdit(idea);"
          @keyup.enter="idea.title.editing = false; doneEdit(idea);"
        >

        <p>
          {{idea.body}}
        </p`>

        <input class="edit" type="text"
          v-model="idea.body.value"
          v-show="idea.body.editing"
          @blur="idea.body.editing = false; doneEdit(idea);"
          @keyup.enter="idea.body.editing = false; doneEdit(idea);"
        >

        <div>
          <p>{{getIdeaQuality(idea.quality)}}</p>

          <button
            @click="upvote(idea)"
            :disabled="idea.quality === 3"
          >
            upvote
          </button>

          <button
            @click="downvote(idea)"
            :disabled="idea.quality === 1"
          >
            downvote
          </button>

        </div>

        <button @click="deleteIdea(idea.id)">Delete</button>

      </article>

    </section>

  </main>

</template>

<script>
import IdeaHeader from './IdeaHeader';

export default {
  name: 'IdeaBox',
  components: {
    IdeaHeader,
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

  mounted: function loadIdeasOnMount() {
    this.loadStoredIdeas();
  },

  computed: {},

  methods: {
    clearInputs() {
      this.title = '';
      this.body = '';
    },

    storeIdeas() {
      localStorage.setItem('ideas', JSON.stringify(this.ideas));
      this.sortIdeas();
    },

    loadStoredIdeas() {
      this.ideas = JSON.parse(localStorage.getItem('ideas')) || [];
      this.sortIdeas();
    },

    addIdea(idea) {
      this.ideas.unshift(idea);
      this.storeIdeas();
    },

    deleteIdea(ideaID) {
      const id = parseInt(ideaID, 10);

      this.ideas = this.ideas.filter(idea => idea.id !== id);
      this.storeIdeas();
    },

    doneEdit() {
      this.storeIdeas();
    },

    getIdeaQuality(quality) {
      if (quality === 1) {
        return 'swill';
      }
      if (quality === 2) {
        return 'plausible';
      }
      if (quality === 3) {
        return 'genius';
      }
      return null;
    },

    findIdea(id) {
      return this.ideas.find(idea => idea.id === id);
    },

    upvote(idea) {
      const ideaObject = this.findIdea(idea.id);
      if (ideaObject.quality < 3) {
        ideaObject.quality += 1;
      }
      this.storeIdeas();
    },

    downvote(idea) {
      const ideaObject = this.findIdea(idea.id);
      if (ideaObject.quality > 1) {
        ideaObject.quality -= 1;
      }
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
