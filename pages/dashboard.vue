<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="6" md="4">
        <v-date-picker v-model="picker"></v-date-picker>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
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
                <td><a href="#">Edit</a> , <a href="#">Delete</a></td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
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
      ],
    }
  },
}
</script>
