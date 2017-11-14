<template>
  <main>
    <section class="idea-inputs">
      <h1>idea<span class="title-gray">box</span></h1>
      <input v-model="title.value" placeholder="Title" type="text" />
      <input v-model="body.value" placeholder="Body" type="text" />
      <button @click="addIdea">Save</button>
    </section>
    <section class="ideas">
      <article v-for="idea in ideas" class="idea-card" :key="idea.id">
        <h3 @dblclick="idea.title.editing = true" v-show="idea.title.editing == false">{{idea.title.value}}</h3>
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
          <p>{{idea.quality}}</p>
          <button @click="upvote(idea)">upvote</button>
          <button @click="downvote(idea)">downvote</button>
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
    };
  },
  mounted: function loadIdeasOnMount() {
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
    doneEdit() {
      this.storeIdeas();
    },
    upvote(idea) {
      if (idea.quality === 'swill') {
        idea.quality = 'plausible';
      } else if (idea.quality === 'plausible') {
        idea.quality = 'genius';
      }
      this.storeIdeas();
    },
    downvote(idea) {
      if (idea.quality === 'genius') {
        idea.quality = 'plausible';
      } else if (idea.quality === 'plausible') {
        idea.quality = 'swill';
      }
      this.storeIdeas();
    }
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
