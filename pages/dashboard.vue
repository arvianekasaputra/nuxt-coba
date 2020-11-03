<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="6" md="4">
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
    <v-row justify="center">
      <v-simple-table>
        <template v-slot:default>
          <thead>
            <tr>
              <th class="text-left">Date</th>
              <th class="text-left">Nama</th>
              <th class="text-left">Pengguna</th>
              <th class="text-left">Waktu</th>
              <th class="text-left">Aktivitas</th>
              <th class="text-left">Aksi</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="room in desserts" :key="room.name">
              <td>{{ room.date }}</td>
              <td>{{ room.name }}</td>
              <td>{{ room.user }}</td>
              <td>{{ room.time }}</td>
              <td>{{ room.activity }}</td>
              <td><a href=".\form-edit">Edit</a> , <a href="#">Delete</a></td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      date: new Date().toISOString().substr(0, 10),
      menu: false,
      desserts: [
        {
          date: '10 Oktober 2020',
          name: 'SGM Explor',
          user: 'Arvian',
          time: '8 AM - 5 PM',
          activity: 'Audit',
        },
        {
          date: '20 Oktober 2020',
          name: 'Lactamil',
          user: 'Eka',
          time: '1 PM - 5 PM',
          activity: 'Internal Meeting',
        },
        {
          date: '30 Oktober 2020',
          name: 'SGM Ananda',
          user: 'Saputra',
          time: '8 AM - 10 AM',
          activity: 'Safety Update',
        },
        {
          date: '1 November 2020',
          name: 'SGM Bunda',
          user: 'Modus',
          time: '8 AM - 11:30 AM',
          activity: 'Meeting Vendor',
        },
      ],
    }
  },
}
</script>
