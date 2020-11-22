<template>
  <v-container>
    <v-row>
      <v-col
        v-for="item in ruang"
        :key="item.id_ruang"
        col-md-3
        class="text-center"
        ><v-card height="200">{{ item.nm_ruang }}</v-card></v-col
      >
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      ruang: [],
    }
  },
  async mounted() {
    const apiruang = await this.$axios.get('/api/ruang')
    this.ruang = apiruang.data.values
  },
}
</script>
