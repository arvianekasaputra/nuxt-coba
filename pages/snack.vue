<template>
  <v-container>
    <v-row>
      <v-col v-for="n in 9" :key="n" cols="4">
        <v-card height="200"></v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
