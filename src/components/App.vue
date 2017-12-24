<template>

  <main>
    <IdeaHeader
      @addIdea="addIdea"
    />

    <section class="filter-sort-panel">
      <label for="search">Search
        <input
          v-model="searchTerm"
          id="search"
        />
      </label>

      <label for="sort">Sort By:
        <select
        v-model="sortBy"
        id="sort"
        >
          <option value="newest" selected>Newest</option>
          <option value="oldest">Oldest</option>
          <option value="highest">Highest Quality</option>
          <option value="lowest">Lowest Quality</option>
        </select>
      </label>
    </section>

    <section class="idea-list">
      <Idea
        v-for="idea in filteredIdeas"
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

const sorters = {
  newest: ideas => ideas.sort((a, b) => a.created < b.created),
  oldest: ideas => ideas.sort((a, b) => a.created > b.created),
  highest: ideas => ideas.sort((a, b) => a.quality < b.quality),
  lowest: ideas => ideas.sort((a, b) => a.quality > b.quality),
};

export default {
  name: 'App',
  components: {
    IdeaHeader,
    Idea,
  },

  data() {
    return {
      ideas: [],
      searchTerm: '',
      sortBy: 'newest',
    };
  },

  mounted() {
    this.loadStoredIdeas();
  },

  computed: {
    sortedIdeas() {
      return sorters[this.sortBy](this.ideas);
    },

    filteredIdeas() {
      return this.sortedIdeas.filter(
        idea =>
          idea.title.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
          idea.body.toLowerCase().includes(this.searchTerm.toLowerCase()),
      );
    },
  },

  methods: {
    addIdea(idea) {
      this.ideas.unshift(idea);
      this.storeIdeas();
    },

    updateIdea(idea) {
      const index = this.ideas.findIndex(storedIdea => storedIdea.id === idea.id);

      this.ideas[index] = idea;
      this.storeIdeas();
    },

    deleteIdea(ideaID) {
      const id = parseInt(ideaID, 10);

      this.ideas = this.ideas.filter(idea => idea.id !== id);
      this.storeIdeas();
    },

    storeIdeas() {
      localStorage.setItem('ideas', JSON.stringify(this.ideas));
    },

    loadStoredIdeas() {
      this.ideas = JSON.parse(localStorage.getItem('ideas')) || [];
    },
  },
};
</script>

<style lang='scss'>
@import '../styles/_mixins_vars.scss';

html {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

body {
  background-color: white;
  font-family: 'Open Sans', sans-serif;
  margin: 0;
  padding: 0;
}

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
