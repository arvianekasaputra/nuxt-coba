<template>
  <v-container>
    <v-row>
      <v-col col-md-3 class="text-center"
        ><v-card height="200">Snack 1</v-card></v-col
      >
      <v-col col-md-3 class="text-center"
        ><v-card height="200">Snack 2</v-card></v-col
      >
      <v-col col-md-3 class="text-center"
        ><v-card height="200">Snack 3</v-card></v-col
      >
    </v-row>
    <v-row>
      <v-col col-md-3 class="text-center"
        ><v-card height="200">Snack 4</v-card></v-col
      >
      <v-col col-md-3 class="text-center"
        ><v-card height="200">Snack 5</v-card></v-col
      >
      <v-col col-md-3 class="text-center"
        ><v-card height="200">Snack 6</v-card></v-col
      >
    </v-row>
    <v-row>
      <v-col col-md-3 class="text-center"
        ><v-card height="200">Snack 7</v-card></v-col
      >
      <v-col col-md-3 class="text-center"
        ><v-card height="200">Snack 8</v-card></v-col
      >
      <v-col col-md-3 class="text-center"
        ><v-card height="200">Snack 9</v-card></v-col
      >
    </v-row>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    items: [
      {
        color: '#1F7087',
        src: 'https://cdn.vuetifyjs.com/images/cards/foster.jpg',
        title: 'Supermodel',
        artist: 'Foster the People',
      },
    ],
  }),
}
</script>

<style>
.farid {
  height: 200px;
}
</style>
