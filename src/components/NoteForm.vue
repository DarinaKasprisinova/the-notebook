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
        <button>Add note</button>
      </div>
    </form>
  
    <h2>Your notes</h2>
  
    <div>
      <ul>
        <li v-for="not in notes" :key="not.id">
            <h4>{{ not.note }}</h4>
            <p>{{ not.date }}</p>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import { handleError } from "vue";
  
  export default {
    data() {
      return {
        note: "",
        notes: [],
        noteError: '',
        date: this.formattedDate(),
      };
    },
    computed: {
      similarNotes() {
        if (this.note && this.date) {
          return this.notes.filter(
            (item) => this.similarity(this.note, item.note) >= 80 && item.date === this.formatDate(this.date)
          );
        }
        return [];
      }
  
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
      handleSubmit() {
        if (this.similarNotes.length > 0) {
          const similarNotesText = this.similarNotes.map((item) => item.note).join(', ');
          this.noteError = (`Poznámka s podobným textom a dátumom už existuje: ${similarNotesText}`);
        } else {
          this.notes.push({
          id: Date.now(),
          note: this.note,
          date: this.formatDate(this.date),
        }
        );
        this.noteError = '';
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
    text-align: left;
    padding: 50px;
    border-radius: 10px;
  }
  
  label {
    color: #aaa;
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
  button {
    background: #444;
    border: 0;
    font-size: 15px;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
  }
  button:hover {
    background: #222;
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
    display: flex;
    align-items: center;
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
  </style>
  