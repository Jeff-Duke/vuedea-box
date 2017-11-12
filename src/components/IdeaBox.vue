<template>
  <main>
    <section class="idea-inputs">
      <h1>idea<span class="title-gray">box</span></h1>
      <input v-model="title" placeholder="Title" type="text" />
      <input v-model="body" placeholder="Body" type="text" />
      <button @click="addIdea">Save</button>
    </section>
    <section class="ideas">
      <article v-for="idea in ideas" class="idea-card" :key="idea.id">
        <h3>{{idea.title}}</h3>
        <h4>{{idea.body}}</h4>
        <p>{{idea.quality}}</p>
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
      title: '',
      body: '',
      ideas: [],
    };
  },
  mounted: function getRandomOnMount() {
    this.loadStoredIdeas();
  },
  methods: {
    clearInputs() {
      this.title = '';
      this.body = '';
    },
    addIdea() {
      const { title, body } = this;
      const idea = { id: Date.now(), title, body, quality: 'swill' };

      this.ideas.push(idea);
      this.clearInputs();

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
      if (localStorage.getItem('ideas')) {
        this.ideas = JSON.parse(localStorage.getItem('ideas'));
      }
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
