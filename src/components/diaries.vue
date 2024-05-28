<script>
export default {
  data() {
    return {
      entries: [],
      newEntry: {
        date: '',
        title: '',
        content: ''
      },
      selectedEntry: null
    };
  },
  mounted() {
    this.loadEntries();
  },
  computed: {
    weeklyEntries() {
      const today = new Date();
      const firstDayOfWeek = new Date(today.getFullYear(), today.getMonth(), today.getDate() - today.getDay());
      const lastDayOfWeek = new Date(today.getFullYear(), today.getMonth(), today.getDate() - today.getDay() + 6);

      return this.entries.filter(entry => {
        const entryDate = new Date(entry.date);
        return entryDate >= firstDayOfWeek && entryDate <= lastDayOfWeek;
      });
    }
  },
  methods: {
    addEntry() {
      if (this.newEntry.date && this.newEntry.title && this.newEntry.content) {
        this.entries.push({
          date: this.newEntry.date,
          title: this.newEntry.title,
          content: this.newEntry.content
        });
        this.saveEntries();
        this.newEntry.date = '';
        this.newEntry.title = '';
        this.newEntry.content = '';
      } else {
        alert('Please fill in all fields.');
      }
    },
    viewEntry(index) {
      this.selectedEntry = this.weeklyEntries[index];
    },
    deleteEntry(index) {
      this.entries.splice(index, 1);
      this.saveEntries();
    },
    loadEntries() {
      const savedEntries = JSON.parse(localStorage.getItem('diaryEntries'));
      if (savedEntries) {
        this.entries = savedEntries;
      }
    },
    saveEntries() {
      localStorage.setItem('diaryEntries', JSON.stringify(this.entries));
    }
  }
};
</script>

<template>
  <div class="container">
    <div class="header">
      <h1>My Diary 🦖</h1>
    </div>
    <div class="entries">
      <h2>Diary</h2>
      <ul>
        <li v-for="(entry, index) in weeklyEntries" :key="index" @click="viewEntry(index)">
          <div class="entry-info">
            <span>{{ entry.date }} - {{ entry.title }}</span>
            <button @click.stop="deleteEntry(index)" class="delete-button">Delete</button>
          </div>
        </li>
      </ul>
    </div>
    <div class="add-entry">
      <h2>Add New Diary</h2>
      <input type="date" v-model="newEntry.date" class="input-field">
      <input type="text" v-model="newEntry.title" placeholder="Title" class="input-field">
      <textarea v-model="newEntry.content" placeholder="Write your entry here" class="input-field"></textarea>
      <button @click="addEntry" class="add-button">Add Entry</button>
    </div>
    <div v-if="selectedEntry !== null" class="entry-details">
      <h2>Selected Entry</h2>
      <p><strong>Date:</strong> {{ selectedEntry.date }}</p>
      <p><strong>Title:</strong> {{ selectedEntry.title }}</p>
      <p><strong>Content:</strong> {{ selectedEntry.content }}</p>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 850px;
  margin: 20px auto;
  padding: 20px;
  background-color: #f4e1d2; /* Warna krem */
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.header {
  text-align: center;
  margin-bottom: 20px;
}

.entries {
  margin-bottom: 20px;
}

.entries h2 {
  margin-top: 0;
}

.add-entry {
  background-color: #fff; /* Warna putih */
  padding: 20px;
  border-radius: 10px;
}

.input-field {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-sizing: border-box;
}

.add-button {
  background-color: #805539; /* Warna kopi */
  color: #fff; /* Warna putih */
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

.add-button:hover {
  background-color: #64442e; /* Warna kopi yang lebih gelap saat hover */
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  padding: 10px;
  background-color: #fff; /* Warna putih */
  border-radius: 5px;
  margin-bottom: 5px;
  transition: background-color 0.3s ease;
}

li:hover {
  background-color: #f4e1d2; /* Warna krem saat hover */
}

.entry-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.delete-button {
  background-color: #f44336; /* Warna merah */
  color: #fff; /* Warna putih */
  border: none;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
}
</style>
