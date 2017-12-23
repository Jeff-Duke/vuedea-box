<template>
<article class="idea-card">
        <h3
          @dblclick="editing = 'title'"
          v-show="editing !==
          'title'"
        >
          {{idea.title}}
        </h3>

        <input class="edit" type="text"
          v-model="title"
          v-show="editing === 'title'"
          @blur="editing = false; updateIdea();"
          @keyup.enter="editing = false; updateIdea();"
        >

        <p
          @dblclick="editing = 'body'"
          v-show="editing !== 'body'"
        >
          {{idea.body}}
        </p>

        <input class="edit" type="text"
          v-model="body"
          v-show="editing === 'body'"
          @blur="editing = false; updateIdea();"
          @keyup.enter="editing = false; updateIdea();"
        >

        <div>
          <p>{{ ideaQuality }}</p>

          <button
            @click="upVote(idea)"
            :disabled="quality === 3"
          >
            upvote
          </button>

          <button
            @click="downVote(idea)"
            :disabled="quality === 1"
          >
            downvote
          </button>

        </div>

        <button @click="deleteIdea(idea.id)">Delete</button>

      </article>
</template>
<script>
export default {
  name: 'Idea',

  props: ['idea'],

  data() {
    return {
      title: this.idea.title,
      body: this.idea.body,
      quality: this.idea.quality,
      editing: false,
    };
  },

  computed: {
    ideaQuality() {
      const quality = this.quality;
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
  },

  methods: {
    upVote() {
      this.quality += 1;
      this.updateIdea();
    },

    downVote() {
      this.quality -= 1;
      this.updateIdea();
    },

    deleteIdea() {
      return this.$emit('deleteIdea', this.idea.id);
    },

    updateIdea() {
      const { id, created } = this.idea;
      const { title, body, quality } = this;
      const idea = {
        id,
        title,
        body,
        quality,
        created,
      };

      return this.$emit('updateIdea', idea);
    },
  },
};
</script>
