<template>
  <v-row justify="center">
    <v-col class="text-center">
      <v-form ref="form" lazy-validation>
        <v-select label="Pilih Ruang" :items="items" required></v-select>

        <v-text-field label="Name" required></v-text-field>

        <v-text-field label="Email" required></v-text-field>

        <v-text-field label="Cost Center"></v-text-field>

        <v-btn color="success" class="mr-4">Submit</v-btn>

        <v-btn color="error" class="mr-4" @click="reset">Reset Form</v-btn>
      </v-form>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data: () => ({
    items: ['Ruang 1', 'Ruang 2', 'Ruang 3', 'Ruang 4'],
  }),

  methods: {
    reset() {
      this.$refs.form.reset()
    },
  },
}
</script>

<style type="css">
.col {
  max-width: 45%;
}
</style>
