<template>

  <main>
    <IdeaHeader
      @addIdea="addIdea"
    />

    <section class="filter-sort-panel container">
        <input
          v-model="searchTerm"
          class="input input--search"
          placeholder="Search"
          aria-label="Search"
        />

      <label for="sort">Sort By:
        <select
        v-model="sortBy"
        class="select select--sort"
        @change="sortIdeas"
        >
          <option value="newest" selected>Newest</option>
          <option value="oldest">Oldest</option>
          <option value="highest">Highest Quality</option>
          <option value="lowest">Lowest Quality</option>
        </select>
      </label>
    </section>

    <section class="idea-list container">
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
      sortedIdeas: [],
    };
  },

  mounted() {
    this.loadStoredIdeas();
    this.sortIdeas();
  },

  computed: {
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
      this.sortIdeas();
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

    sortIdeas() {
      const sortedIdeas = sorters[this.sortBy](this.ideas);
      this.sortedIdeas = sortedIdeas;
    },
  },
};
</script>

<style lang='scss'>
@import '../styles/_reset.scss';
@import '../styles/_mixins_vars.scss';
@import '../styles/_buttons_inputs.scss';

body {
  background-color: $color-white;
  font-family: $primary-font;
  margin: 0;
}

.container {
  margin: 0 auto;
  width: 65vw;

  @media screen and(max-width: 480px) {
    width: 90vw;
  }
}

.filter-sort-panel {
  display: flex;
  flex-direction: column-reverse;
  margin-top: 2rem;

  .select--sort {
    appearance: none;
    background: url('../assets/chevron-down.png') no-repeat 95%;
    background-size: 1.25rem;
    border: 2px solid $color-border-gray;
    font-size: 1rem;
    margin: 1rem 0 1.5rem 0.75rem;
    padding: 0.25rem;
    padding-right: 2rem;
  }
}
</style>
