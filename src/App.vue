<template>
  <v-app>
    <v-main>
      <List/>
    </v-main>
  </v-app>
</template>

<script>
import List from './components/List';

export default {
  name: 'App',
  components: {
    List,
  },
};
</script>
