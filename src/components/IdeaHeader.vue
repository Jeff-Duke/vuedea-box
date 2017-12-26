<template>
  <header class="idea-header">

    <h1 class="header">idea
      <span class="title-gray">box</span>
    </h1>

    <section class="inputs container">
        <input
          v-model="title"
          class="input input--title"
          ref="titleInput"
          placeholder="Title"
          type="text"
          aria-label="title"
        />

        <textarea
          v-model="body"
          class="input input--body"
          @keyup.enter="createIdea"
          placeholder="Body"
          type="text"
          aria-label="body"
        />

      <button
      @click="createIdea"
      :disabled="!isEnabled"
      class="btn btn--primary"
      >
        Save
      </button>
    </section>

  </header>
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

  mounted() {
    this.$refs.titleInput.focus();
  },

  computed: {
    isEnabled() {
      return Boolean(this.title) && Boolean(this.body);
    },
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
<style lang="scss">
@import '../styles/_mixins_vars.scss';
@import '../styles/_buttons_inputs.scss';

.idea-header {
  background-color: $color-background;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  margin-bottom: 1rem;

  .header {
    color: $color-primary-blue;
    font-family: $secondary-font;
    font-size: 3rem;
    margin-bottom: 2rem;

    .title-gray {
      color: $color-text-dark-gray;
      margin-left: -0.75rem;
    }
  }

  .inputs {
    display: flex;
    flex-direction: column;
  }

  @media screen and (max-width: 480px) {
    padding: 2rem 0.5rem;
  }
}
</style>

