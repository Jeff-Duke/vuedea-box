<template>
  <article class="idea-card">
    <h3
      @dblclick="editIdea('ideaTitle')"
      v-if="editing !==
      'ideaTitle'"
      class="idea__title"
    >
      {{idea.title}}
    </h3>

    <input class="edit edit__title" type="text"
      v-model="title"
      v-if="editing === 'ideaTitle'"
      @blur="doneEditing"
      @keyup.enter="doneEditing"
      ref="ideaTitle"
    />

    <button
      @click="deleteIdea"
      class="btn__idea btn__idea--delete"
    />

    <p
      @dblclick="editIdea('ideaBody')"
      v-if="editing !== 'ideaBody'"
      class="idea__body"
    >
      {{idea.body}}
    </p>

    <textarea class="edit edit__body" type="text"
      v-model="body"
      v-if="editing === 'ideaBody'"
      @blur="doneEditing"
      @keyup.enter="doneEditing"
      ref="ideaBody"
    />

    <div>
      <p class="idea__quality">Quality: <span class="idea__quality--text">{{ ideaQuality }}</span></p>

      <button
        @click="upVote(idea)"
        :disabled="quality === 3"
        class="btn btn__idea btn__idea--upvote"
      />

      <button
        @click="downVote(idea)"
        :disabled="quality === 1"
        class="btn btn__idea btn__idea--downvote"
      />
    </div>

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
      const qualityGate = {
        1: 'swill',
        2: 'plausible',
        3: 'genius',
      };

      return qualityGate[this.quality];
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

    editIdea(ref) {
      this.editing = ref;
      this.$nextTick(() => {
        this.$refs[ref].focus();
      });
    },

    doneEditing() {
      this.editing = false;
      this.updateIdea();
    },
  },
};
</script>

<style lang="scss">
@import '../styles/_mixins_vars.scss';

.idea-card {
  border-bottom: 1px solid $color-border-gray;
  color: $color-text-dark-gray;
  font-family: $primary-font;
  font-size: 1rem;
  margin: 1rem 0;
  padding: 1rem 0 3rem 0;

  .idea__title,
  .idea__quality {
    font-family: $secondary-font;
    font-weight: 700;
    color: $color-text-dark-gray;
  }

  .idea__title,
  .idea__body {
    margin-bottom: 1rem;
  }

  .idea__title {
    font-size: 1.5rem;
  }

  .idea__body {
    line-height: 1.5rem;
    width: 85%;
  }

  .edit {
    display: block;
    border: 2px solid $color-border-gray;
    font-family: $primary-font;
    font-size: 1.125rem;
    margin-bottom: 1rem;
    padding: 0.5rem;

    &__title,
    &__body {
      width: 85%;
    }

    &__body {
      resize: none;
      height: 6rem;
    }
  }

  .btn__idea {
    border: none;
    background-color: transparent;
    cursor: pointer;
    height: 1.5rem;
    width: 1.5rem;

    &--delete {
      float: right;
      background: url('../assets/delete.svg') center no-repeat;

      &:hover {
        background: url('../assets/delete-hover.svg') center no-repeat;
      }
    }

    &--upvote {
      float: left;
      background: url('../assets/upvote.svg') center no-repeat;
      margin-right: 0.5rem;

      &:hover {
        background: url('../assets/upvote-hover.svg') center no-repeat;
      }
    }

    &--downvote {
      float: left;
      background: url('../assets/downvote.svg') center no-repeat;
      margin-right: 1rem;

      &:hover {
        background: url('../assets/downvote-hover.svg') center no-repeat;
      }
    }
  }

  .idea__quality {
    margin: 1rem 0;
  }
}
</style>

