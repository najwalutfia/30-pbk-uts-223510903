<template>
 <div class="wrapper">
  <nav class="navbar">
    <ul class="nav nav-pills">
      <li><button class="nav-link" :class="{ active: currentView === 'todos' }" @click="currentView = 'todos'" aria-current="page">Todos</button></li>
      <li><button class="nav-link" :class="{ active: currentView === 'posts' }" @click="currentView = 'posts'">Posts</button></li>
    </ul>
  </nav>
  <slot>Content Post Todos Najwa Lutfia Nisya</slot>
</div>


  <div v-if="currentView === 'todos'">
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
          <th>Aktivitas</th>
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

  <div v-if="currentView === 'posts'">
    <h1>Data Postingan</h1>
    <label for="user-select">Pilih User:</label>
    <select id="user-select" v-model="selectedUser" @change="fetchPosts">
      <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
    </select>
    <table>
      <thead>
        <tr>
          <th>Judul</th>
          <th>Isi</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="post in posts" :key="post.id">
          <td>{{ post.title }}</td>
          <td>{{ post.body }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  data() {
    return {
      currentView: 'todos', // Set the default view to 'todos'
      newActivity: {
        name: '',
        dateTime: ''
      },
      activities: [],
      users: [],
      selectedUser: null,
      posts: []
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
    },
    async fetchUsers() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        this.users = await response.json();
      } catch (error) {
        console.error('Failed to fetch users:', error);
      }
    },
    async fetchPosts() {
      if (this.selectedUser) {
        try {
          const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`);
          this.posts = await response.json();
        } catch (error) {
          console.error('Failed to fetch posts:', error);
        }
      }
    }
  },
  created() {
    this.fetchUsers();
  }
};
</script>
<style>
.wrapper {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  font-family: Arial, sans-serif;
}

nav.navbar {
  background-color: rgb(58, 56, 179);
  color: white;
  width: 100%;
  padding: 0.5rem 1rem;
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: space-around;
  align-items: center;
  z-index: 1000;
}

nav ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  gap: 1rem;
}

nav ul li {
  display: inline;
}

nav ul li .nav-link {
  color: white;
  text-decoration: none;
  background: none;
  border: none;
  padding: 0.5rem 1rem;
  cursor: pointer;
}

nav ul li .nav-link.active {
  background-color: rgb(255, 255, 255);
  color: rgb(56, 154, 179);
  border-radius: 0.25rem;
}

h1 {
  margin-top: 4rem;
}

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
