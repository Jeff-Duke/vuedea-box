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
        <h3 @dblclick="editIdea(idea,title)">{{idea.title}}</h3>
        <input class="edit" type="text"
          v-model="idea.title"
          v-todoFocus="title == editedTitle"
          @blur="doneEdit(todo)"
          @keyup.enter="doneEdit(todo)"
          @keyup.esc="cancelEdit(todo)">
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
      editedTitle: null,
      body: '',
      editedBody: null,
      backup: '',
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
    storeIdeas() {
      localStorage.setItem('ideas', JSON.stringify(this.ideas));
    },
    loadStoredIdeas() {
      this.ideas = JSON.parse(localStorage.getItem('ideas')) || [];
    },
    addIdea() {
      const { title, body } = this;
      const idea = { id: Date.now(), title, body, quality: 'swill' };

      this.ideas.unshift(idea);
      this.clearInputs();

      this.storeIdeas();
    },
    deleteIdea(ideaID) {
      const id = parseInt(ideaID, 10);

      this.ideas = this.ideas.filter(idea => idea.id !== id);
      this.storeIdeas();
    },
    editIdea(idea, el) {
      debugger;
      this.backup = idea.el;
      this.editedIdea = idea;
    },
  },
  directives: {
    todoFocus(el, binding) {
      if (binding.value) {
        el.focus();
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
