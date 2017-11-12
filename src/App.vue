<template>
  <main id="app">
    <IdeaBox />
  </main>
</template>

<script>
import IdeaBox from './components/IdeaBox';

export default {
  name: 'app',
  components: {
    IdeaBox,
  },
};
</script>

<style>
html {
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

body {
  font-family: "Open Sans", sans-serif;
  margin: 0;
}
</style>
