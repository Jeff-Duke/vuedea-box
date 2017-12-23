<template>
  <section class="idea-inputs">

    <h1>idea
      <span class="title-gray">box</span>
    </h1>

    <label
      for="title-input"
    >Title
      <input
        v-model="title"
        id="title-input"
        ref="titleInput"
        placeholder="Title"
        type="text"
      />
    </label>

    <label
      for="body-input">Body
      <input
        v-model="body"
        id="body-input"
        @keyup.enter="createIdea"
        placeholder="Body"
        type="text"
      />
    </label>

    <button
    @click="createIdea"
    :disabled="!isEnabled"
    >
      Save
    </button>

  </section>
</template>
<script>
export default {
  name: 'IdeaHeader',

  data() {
    return {
      title: '',
      body: '',
    };
  },
  computed: {
    isEnabled() {
      return Boolean(this.title) && Boolean(this.body);
    },
  },

  mounted() {
    this.$refs.titleInput.focus();
  },

  methods: {
    createIdea() {
      const { title, body } = this;
      if (this.isEnabled) {
        const idea = {
          id: Date.now(),
          title,
          body,
          quality: 1,
          created: Date.now(),
        };

        this.$refs.titleInput.focus();
        this.clearInputs();
        this.$emit('addIdea', idea);
      }
      return null;
    },

    clearInputs() {
      this.title = '';
      this.body = '';
    },
  },
};
</script>
