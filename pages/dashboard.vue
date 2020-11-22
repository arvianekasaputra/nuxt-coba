<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" sm="6" md="4">
        <v-menu
          ref="menu"
          v-model="menu"
          :close-on-content-click="false"
          :return-value.sync="date"
          transition="scale-transition"
          offset-y
          min-width="290px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-text-field
              v-model="date"
              label="Picker in menu"
              prepend-icon="mdi-calendar"
              readonly
              v-bind="attrs"
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker v-model="date" no-title scrollable>
            <v-spacer></v-spacer>
            <v-btn text color="primary" @click="menu = false"> Cancel </v-btn>
            <v-btn text color="primary" @click="$refs.menu.save(date)">
              OK
            </v-btn>
          </v-date-picker>
        </v-menu>
      </v-col>
    </v-row>
    <v-card>
      <v-card-title>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="transaksi"
        :search="search"
      ></v-data-table>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      tangal: new Date().toISOString().substr(0, 10),
      dashboard: [],
      menu: false,
      search: '',
      transaksi: [
        {
          tangal: '',
          nm_ruang: '',
          display_nm: '',
          activity: '',
          nm_snack: '',
        },
      ],
      date: [],
      headers: [
        { text: 'Tanggal', value: 'tanggal' },
        { text: 'Ruang', value: 'ruang' },
        { text: 'Pengguna', value: 'pengguna' },
        { text: 'Aktivitas', value: 'aktivitas' },
        { text: 'Snack', value: 'protein' },
      ],
      desserts: [
        {
          tanggal: '2020-10-04',
          ruang: 'SGM Bunda',
          pengguna: 'Eka',
          aktivitas: 'Audit Dango',
          protein: 'Snack 2',
        },
        {
          tanggal: '2020-10-07',
          ruang: 'Bebelac',
          pengguna: 'Saputra',
          aktivitas: 'Audit ISO',
          protein: 'Snack 3',
        },
        {
          tanggal: '2020-10-10',
          ruang: 'SGM Ananda',
          pengguna: 'Faridhotul',
          aktivitas: 'Safety Update',
          protein: 'Snack 4',
        },
        {
          tanggal: '2020-10-13',
          ruang: 'SGM Soya',
          pengguna: 'Khasanah',
          aktivitas: 'Meeting Vendor',
          protein: 'Snack 5',
        },
        {
          tanggal: '2020-10-16',
          ruang: 'Lactamil',
          pengguna: 'Putri',
          aktivitas: 'Meeting Internal',
          protein: 'Snack 1',
        },
        {
          tanggal: '2020-10-19',
          ruang: 'LLM',
          pengguna: 'Meyra',
          aktivitas: 'Pinjam ruang',
          protein: 'Snack 2',
        },
        {
          tanggal: '2020-10-22',
          ruang: 'Nutrilon Royal',
          pengguna: 'Afiyana',
          aktivitas: 'Interview',
          protein: 'Snack 3',
        },
        {
          tanggal: '2020-10-25',
          ruang: 'RWS',
          pengguna: 'Delta',
          aktivitas: 'Safety Talk',
          protein: 'Snack 4',
        },
        {
          tanggal: '2020-10-28',
          ruang: 'Themis',
          pengguna: 'Rizky',
          aktivitas: 'Plant Talk',
          protein: 'Snack 5',
        },
      ],
    }
  },
  async mounted() {
    const apitransaksi = await this.$axios.get('/api/transaksi')
    this.transaksi = apitransaksi.data.values
  },
}
</script>
