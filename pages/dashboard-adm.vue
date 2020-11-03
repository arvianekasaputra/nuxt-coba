<template>
  <v-data-table :headers="headers" :items="dashboard" sort-by="date">
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Mr. Booking</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

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
                          v-model="editedItem.tanggal"
                          label="Pilih tanggal"
                          prepend-icon="mdi-calendar"
                          readonly
                          v-bind="attrs"
                          v-on="on"
                        ></v-text-field>
                      </template>
                      <v-date-picker
                        v-model="editedItem.tanggal"
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
                      v-model="editedItem.ruang"
                      label="Pilih Ruang"
                      :items="items"
                      required
                    ></v-select>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.pengguna"
                      label="Pengguna"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.aktivitas"
                      label="Aktivitas"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.protein"
                      label="Protein (g)"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close"> Cancel </v-btn>
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
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    tangal: new Date().toISOString().substr(0, 10),
    menu: false,
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
      { text: 'Tanggal', value: 'tanggal' },
      { text: 'Ruang', value: 'ruang' },
      { text: 'Pengguna', value: 'pengguna' },
      { text: 'Aktivitas', value: 'aktivitas' },
      { text: 'Snack', value: 'protein' },
      { text: 'Keterangan', value: 'actions', sortable: false },
    ],
    dashboard: [],
    editedIndex: -1,
    editedItem: {
      date: '',
      ruang: 0,
      pengguna: 0,
      aktivitas: 0,
      protein: 0,
    },
    defaultItem: {
      date: '',
      ruang: 0,
      pengguna: 0,
      aktivitas: 0,
      protein: 0,
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    },
  },

  watch: {
    dialog(val) {
      val || this.close()
    },
    dialogDelete(val) {
      val || this.closeDelete()
    },
  },

  created() {
    this.initialize()
  },

  methods: {
    initialize() {
      this.dashboard = [
        {
          tanggal: '2020-10-01',
          ruang: 'SGM Explor',
          pengguna: 'Arvian',
          aktivitas: 'Audit FSI',
          protein: 'Snack 1',
        },
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
      ]
    },

    editItem(item) {
      this.editedIndex = this.dashboard.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem(item) {
      this.editedIndex = this.dashboard.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteItemConfirm() {
      this.dashboard.splice(this.editedIndex, 1)
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
        Object.assign(this.dashboard[this.editedIndex], this.editedItem)
      } else {
        this.dashboard.push(this.editedItem)
      }
      this.close()
    },
  },
}
</script>
