<template>
  <header class="idea-header">

    <h1 class="header">idea
      <span class="title-gray">box</span>
    </h1>

    <section class="inputs">
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

.idea-header {
  background-color: $color-background;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  margin-bottom: 1rem;

  .inputs {
    display: flex;
    flex-direction: column;
    width: 65vw;

    @media screen and(max-width: 480px) {
      width: 90vw;
    }

    .input {
      border: 2px solid $color-border-gray;
      margin-bottom: 1rem;

      &--body {
        height: 2.8125rem;
        resize: none;

        @media screen and(max-width: 480px) {
          height: 5.625rem;
        }
      }
    }

    .input,
    .btn {
      font-family: $primary-font;
      font-size: 1.125rem;
      padding: 0.5rem;
    }

    .btn--primary {
      align-self: center;
      background-color: $color-primary-blue;
      color: $color-white;
      border: 2px solid transparent;
      cursor: pointer;
      width: 100%;

      &:hover,
      &:focus {
        background-color: $color-hover-green;
        box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
      }

      &:active,
      &:focus:active {
        box-shadow: none;
      }

      &:disabled {
        pointer-events: none;
        background-color: $color-white;
        color: $color-text-dark-gray;
        border: 2px solid $color-border-gray;
      }
    }
  }
}
</style>

