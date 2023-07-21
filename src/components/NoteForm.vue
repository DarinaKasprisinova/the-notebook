<template>
  <form @submit.prevent="handleSubmit">
    <div class="input-container">
      <label class="required">Note:</label>
      <input v-model="note" required />

      <label class="required">Date:</label>
      <input type="date" v-model="date" required />
    </div>
    <div v-if="noteError" class="error">{{ noteError }}</div>
    <div class="submit">
      <button
        class="mt-4 bg-gray-800 hover:bg-blue-800 text-blue-50 font-bold py-2 px-4 rounded"
      >
        Add note
      </button>
    </div>
  </form>

  <h2 class="text-2xl font-bold mt-8 dark:text-gray-800">Your notes</h2>

  <ul>
    <li v-for="(not, index) in notes" :key="not.id">
      <div class="note-info">
        <h4 class="text-xl font-medium font-sans">{{ not.note }}</h4>
        <div class="note-date-action">
          <p>{{ not.date }}</p>
          <p class="removeNote" @click="deleteNote(index)">remove note</p>
        </div>
      </div>
    </li>
  </ul>
</template>

<script>
import { handleError } from "vue";

export default {
  data() {
    return {
      note: "",
      notes: [],
      noteError: "",
      date: this.formattedDate(),
    };
  },
  computed: {
    similarNotes() {
      if (this.note && this.date) {
        return this.notes.filter(
          (item) =>
            this.similarity(this.note, item.note) >= 80 &&
            item.date === this.formatDate(this.date)
        );
      }
      return [];
    },
  },
  methods: {
    formattedDate() {
      const currentDate = new Date();
      const day = currentDate.getDate().toString().padStart(2, "0");
      const month = (currentDate.getMonth() + 1).toString().padStart(2, "0");
      const year = currentDate.getFullYear();
      const formattedDate = `${year}-${month}-${day}`;
      return formattedDate;
    },
    formatDate(date) {
      const options = { year: "numeric", month: "long", day: "numeric" };
      return new Date(date).toLocaleDateString("sk-SK", options);
    },
    similarity(a, b) {
      const setA = new Set(a.toLowerCase());
      const setB = new Set(b.toLowerCase());
      const intersection = new Set([...setA].filter((x) => setB.has(x)));
      return (intersection.size / setA.size) * 100;
    },
    deleteNote(index) {
      this.notes.splice(index, 1);
    },
    handleSubmit() {
      if (this.similarNotes.length > 0) {
        const similarNotesText = this.similarNotes
          .map((item) => item.note)
          .join(", ");
        this.noteError = `Poznámka s podobným textom a dátumom už existuje: ${similarNotesText}`;
      } else {
        this.notes.push({
          id: Date.now(),
          note: this.note,
          date: this.formatDate(this.date),
        });
        this.noteError = "";
        this.note = "";
      }
    },
  },
};
</script>

<style>
form {
  max-width: 700px;
  margin: 30px auto;
  background: white;
  color: #444;
  text-align: left;
  padding: 50px;
  border-radius: 10px;
}

label {
  color: #444;
  display: inline-block;
  margin: 25px 15px 15px 0;
  font-size: 0.6em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}

.input-container {
  display: flex;
  flex-direction: row;
  align-items: center;
}

input {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  color: #555;
}
.submit {
  text-align: center;
}
.required::after {
  content: "*";
  color: red;
  margin-left: 2px;
}

p,
h3,
ul {
  margin: 0;
  padding: 0;
}
li {
  max-width: 700px;
  list-style-type: none;
  background: #fff;
  color: rgb(77, 80, 94);
  margin: 20px auto;
  padding: 10px 20px;
  border-radius: 10px;
  align-items: flex-start;
}

.note-info {
  display: flex;
  flex-direction: column;

  display: flex;

  justify-content: space-between;
}

.note-date-action {
  display: flex;

  display: flex;

  justify-content: space-between;
}

.note-date-action p {
  margin-right: 10px;
  display: flex;

  justify-content: space-between;
}

li.fav {
  background: #ff9ed2;
  color: white;
}
.box {
  padding: 100px 0;
  width: 400px;
  text-align: center;
  background: #ddd;
  margin: 20px;
  display: inline-block;
}
.error {
  color: crimson;
}
.removeNote {
  color: rgb(226, 43, 76);
  cursor: pointer;
  text-decoration: underline;
}
.removeNote:hover {
  font-size: 1.1em;
}
</style>
