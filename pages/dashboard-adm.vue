<template>
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
      sort-by="date"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Mr. Booking</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <v-card>
              <v-card-text>
                <v-container>
                  <v-row>
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
                            v-model="editedItem.tgl"
                            label="Pilih Tanggal"
                            prepend-icon="mdi-calendar"
                            readonly
                            v-bind="attrs"
                            v-on="on"
                          ></v-text-field>
                        </template>
                        <v-date-picker
                          v-model="editedItem.tgl"
                          no-title
                          scrollable
                        >
                          <v-spacer></v-spacer>
                          <v-btn text color="primary" @click="menu = false">
                            Cancel
                          </v-btn>
                          <v-btn
                            text
                            color="primary"
                            @click="$refs.menu.save(date)"
                          >
                            OK
                          </v-btn>
                        </v-date-picker>
                      </v-menu>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-select
                        v-model="editedItem.nm_ruang"
                        label="Pilih Ruang"
                        :items="items"
                        required
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.display_nm"
                        label="Pengguna"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.activity"
                        label="Aktivitas"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.nm_snack"
                        label="Snack"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                  Cancel
                </v-btn>
                <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="headline"
                >Are you sure you want to delete this item?</v-card-title
              >
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete"
                  >Cancel</v-btn
                >
                <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                  >OK</v-btn
                >
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:[`item.actions`]="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    tangal: new Date().toISOString().substr(0, 10),
    menu: false,
    tgl: '',
    search: '',
    items: [
      'SGM Explor',
      'SGM Bunda',
      'Bebelac',
      'SGM Ananda',
      'SGM Soya',
      'Lactamil',
      'LLM',
      'Nutrilon Royal',
      'RWS',
      'Themis',
    ],
    headers: [
      { text: 'Tanggal', value: 'tgl' },
      { text: 'Ruang', value: 'nm_ruang' },
      { text: 'Pengguna', value: 'display_nm' },
      { text: 'Aktivitas', value: 'activity' },
      { text: 'Snack', value: 'nm_snack' },
      { text: 'Keterangan', value: 'actions', sortable: false },
    ],
    transaksi: [],
    editedIndex: -1,
    editedItem: {
      tgl: '',
      nm_ruang: 0,
      display_nm: 0,
      activity: 0,
      nm_snack: 0,
    },
    defaultItem: {
      tgl: '',
      nm_ruang: 0,
      display_nm: 0,
      activity: 0,
      nm_snack: 0,
    },
  }),

  watch: {
    dialog(val) {
      val || this.close()
    },
    dialogDelete(val) {
      val || this.closeDelete()
    },
  },

  async mounted() {
    const apitransaksi = await this.$axios.get('/api/transaksi')
    this.transaksi = apitransaksi.data.values
  },

  created() {
    this.initialize()
  },

  methods: {
    initialize() {
      this.transaksi = [
        {
          tgl: '2020-10-01',
          nm_ruang: 'SGM Explor',
          display_nm: 'Arvian',
          activity: 'Audit FSI',
          nm_snack: 'Snack 1',
        },
        {
          tgl: '2020-10-04',
          nm_ruang: 'SGM Bunda',
          display_nm: 'Eka',
          activity: 'Audit Dango',
          nm_snack: 'Snack 2',
        },
        {
          tgl: '2020-10-07',
          nm_ruang: 'Bebelac',
          display_nm: 'Saputra',
          activity: 'Audit ISO',
          nm_snack: 'Snack 3',
        },
        {
          tgl: '2020-10-10',
          nm_ruang: 'SGM Ananda',
          display_nm: 'Faridhotul',
          activity: 'Safety Update',
          nm_snack: 'Snack 4',
        },
        {
          tgl: '2020-10-13',
          nm_ruang: 'SGM Soya',
          display_nm: 'Khasanah',
          activity: 'Meeting Vendor',
          nm_snack: 'Snack 5',
        },
        {
          tgl: '2020-10-16',
          nm_ruang: 'Lactamil',
          display_nm: 'Putri',
          activity: 'Meeting Internal',
          nm_snack: 'Snack 1',
        },
        {
          tgl: '2020-10-19',
          nm_ruang: 'LLM',
          display_nm: 'Meyra',
          activity: 'Pinjam ruang',
          nm_snack: 'Snack 2',
        },
        {
          tgl: '2020-10-22',
          nm_ruang: 'Nutrilon Royal',
          display_nm: 'Afiyana',
          activity: 'Interview',
          nm_snack: 'Snack 3',
        },
        {
          tgl: '2020-10-25',
          nm_ruang: 'RWS',
          display_nm: 'Delta',
          activity: 'Safety Talk',
          nm_snack: 'Snack 4',
        },
        {
          tgl: '2020-10-28',
          nm_ruang: 'Themis',
          display_nm: 'Rizky',
          activity: 'Plant Talk',
          nm_snack: 'Snack 5',
        },
        {
          tgl: '',
          nm_ruang: '',
          display_nm: '',
          activity: '',
          nm_snack: '',
        },
      ]
    },

    editItem(item) {
      this.editedIndex = this.transaksi.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem(item) {
      this.editedIndex = this.transaksi.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteItemConfirm() {
      this.transaksi.splice(this.editedIndex, 1)
      this.closeDelete()
    },

    close() {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete() {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.transaksi[this.editedIndex], this.editedItem)
      } else {
        this.transaksi.push(this.editedItem)
      }
      this.close()
    },
  },
}
</script>
