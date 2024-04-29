<template>
    <div>
      <h1>Daftar Kegiatan</h1>
      <input type="text" v-model="newActivity.name" placeholder="Tambahkan kegiatan baru">
      <div class="datetime-container">
        <label for="datetime">Tanggal & Jam:</label>
        <input id="datetime" type="datetime-local" v-model="newActivity.dateTime">
      </div>
      <button @click="addActivity">Tambah</button>
      <table>
        <thead>
          <tr>
            <th>Kegiatan</th>
            <th>Tanggal & Jam</th>
            <th>Status</th>
            <th>Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(activity, index) in activities" :key="index">
            <td>{{ activity.name }}</td>
            <td>{{ formatDate(activity.dateTime) }}</td>
            <td>
              <input type="checkbox" v-model="activity.completed">
              <span :class="{ 'completed': activity.completed }">{{ activity.completed ? 'Selesai' : 'Belum Selesai' }}</span>
            </td>
            <td><button @click="removeActivity(index)">Hapus</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        newActivity: {
          name: '',
          dateTime: ''
        },
        activities: []
      };
    },
    methods: {
      addActivity() {
        if (this.newActivity.name.trim() !== '' && this.newActivity.dateTime.trim() !== '') {
          this.activities.push({ 
            name: this.newActivity.name, 
            dateTime: this.newActivity.dateTime, 
            completed: false 
          });
          this.newActivity.name = '';
          this.newActivity.dateTime = '';
        }
      },
      removeActivity(index) {
        this.activities.splice(index, 1);
      },
      formatDate(dateTime) {
        const options = { year: 'numeric', month: 'short', day: 'numeric', hour: 'numeric', minute: 'numeric' };
        return new Date(dateTime).toLocaleDateString('en-US', options);
      }
    }
  };
  </script>
  
  <style>
  .completed {
    text-decoration: line-through;
  }
  .datetime-container {
    margin-top: 10px;
  }
  .datetime-container label {
    display: block;
    margin-bottom: 5px;
  }
  </style>