<template>

  <main>

    <section class="idea-inputs">

      <h1>idea<span class="title-gray">box</span></h1>

      <input v-model="title.value" placeholder="Title" type="text" />

      <input v-model="body.value" placeholder="Body" type="text" />

      <button @click="addIdea">Save</button>

    </section>

    <section class="ideas">
      <input v-model="searchTerm" @keyup="filterIdeas()" />

      <article v-for="idea in visibleIdeas" class="idea-card" :key="idea.id">
        <h3
          @dblclick="idea.title.editing = true"
          v-show="idea.title.editing == false"
        >
          {{idea.title.value}}
        </h3>

        <input class="edit" type="text"
          v-model="idea.title.value"
          v-show="idea.title.editing == true"
          @blur="idea.title.editing = false; doneEdit(idea);"
          @keyup.enter="doneEdit(idea)"
        >

        <h4 @dblclick="idea.body.editing = !idea.body.editing"
          v-show="!idea.body.editing"
        >
          {{idea.body.value}}
        </h4>

        <input class="edit" type="text"
          v-model="idea.body.value"
          v-show="idea.body.editing"
          @blur="idea.body.editing = false; doneEdit(idea);"
          @keyup.enter="doneEdit(idea)"
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
export default {
  name: 'IdeaBox',

  data() {
    return {
      title: {
        value: '',
        editing: false,
      },
      body: {
        value: '',
        editing: false,
      },
      backup: '',
      ideas: [],
      visibleIdeas: [],
      searchTerm: '',
    };
  },

  mounted: function loadIdeasOnMount() {
    this.loadStoredIdeas();
  },

  computed: {

  },

  methods: {
    clearInputs() {
      this.title = '';
      this.body = '';
    },

    storeIdeas() {
      localStorage.setItem('ideas', JSON.stringify(this.ideas));
    },

    loadStoredIdeas() {
      this.ideas = JSON.parse(localStorage.getItem('ideas')) || [];
      this.visibleIdeas = this.ideas;
    },

    addIdea() {
      const { title, body } = this;
      const idea = { id: Date.now(), title, body, quality: 1 };

      this.ideas.unshift(idea);
      this.clearInputs();

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
      this.visibleIdeas = this.ideas.filter(idea =>
        idea.title.value.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
        idea.body.value.toLowerCase().includes(this.searchTerm.toLowerCase()),
      );
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
