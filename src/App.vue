<template>
  <div class="app">
    <h1>To Do List Saya</h1>
    <input type="text" v-model="newItem" placeholder="Telusuri To do List Saya" @keyup.enter="addItem">
    <ul>
      <li v-for="(item, index) in items" :key="index">
        <span :class="{ 'completed': item.completed }">{{ item.text }}</span>
        <button @click="editItem(index)">Edit</button>
        <button @click="toggleCompleted(index)">{{ item.completed ? 'Undo' : 'Done' }}</button>
        <button @click="deleteItem(index)">Delete</button>
        <!-- Tambahkan input untuk pengeditan -->
        <input v-if="item.editing" type="text" v-model="item.updatedText" @keyup.enter="saveEdit(index)" @blur="cancelEdit(index)">
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newItem: '',
      items: []
    };
  },
  methods: {
    addItem() {
      if (this.newItem.trim() !== '') {
        this.items.push({ text: this.newItem, completed: false });
        this.newItem = '';
      }
    },
    editItem(index) {
      // Tandai item yang diedit
      this.items[index].editing = true;
      // Simpan teks asli untuk pengeditan
      this.items[index].updatedText = this.items[index].text;
    },
    saveEdit(index) {
      // Perbarui teks item dan nonaktifkan mode pengeditan
      if (this.items[index].updatedText.trim() !== '') {
        this.items[index].text = this.items[index].updatedText;
        this.items[index].editing = false;
      }
    },
    cancelEdit(index) {
      // Batal mode pengeditan dan kembalikan teks ke teks asli
      this.items[index].editing = false;
      this.items[index].updatedText = this.items[index].text;
    },
    deleteItem(index) {
      this.items.splice(index, 1);
    },
    toggleCompleted(index) {
      this.items[index].completed = !this.items[index].completed;
    }
  }
};
</script>

<style scoped>
.app {
  font-family: Arial, sans-serif;
  max-width: 400px;
  margin: 0 auto;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 50px;
  border-radius: 10px;
  box-shadow: 0px 0px 10px 0px rgba(0,0,0,0.2);
}
input[type="text"] {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin-bottom: 5px;
}
button {
  margin-left: 5px;
  background-color: #f44336;
  border: none;
  color: white;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #ca0e0e;
}
.completed {
  text-decoration: line-through;
}
</style>
