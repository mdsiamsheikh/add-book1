<template>
  <div class="siam">
    <h1>Add Favorite Book</h1>
    <section class="container">
      <form id="book-form">
        <div>
          <label for="title">Book</label>
          <input
            type="text"
            id="title"
            placeholder="Book"
            value="{data.title}"
            v-model="title"
          />
        </div>
        <div>
          <label for="author"> Author</label>
          <input
            type="text"
            id="author"
            value="data.author"
            v-model="author"
            placeholder="Author"
          />
        </div>
        <div>
          <label for="Released">Released</label>
          <input
            type="text"
            id="Released"
            value="Released"
            v-model="released"
            placeholder="Released"
          />
        </div>

        <div v-if="!awawesome">
          <button type="submit" @click.prevent="postData()" class="btn">
            Add In Your List
          </button>
        </div>

        <div v-else>
          <button type="submit" @click.prevent="updateItem()" class="btn">
            Update Your List
          </button>
        </div>
      </form>

      <section class="table">
        <section class="table-section">
          <table>
            <tr class="header">
              <th>Book</th>
              <th>Author</th>
              <th>Released</th>
              <th>Update</th>
              <th>Delete</th>
            </tr>

            <tr class="siam2" v-for="item in items" :key="item.id">
              <td>{{ item.bookName }}</td>
              <td>{{ item.author }}</td>
              <td>{{ item.released }}</td>

              <td class="siam4" @click="putData(item.id, item)">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 30 30"
                  width="30px"
                  height="30px"
                >
                  <path
                    d="M 22.828125 3 C 22.316375 3 21.804562 3.1954375 21.414062 3.5859375 L 19 6 L 24 11 L 26.414062 8.5859375 C 27.195062 7.8049375 27.195062 6.5388125 26.414062 5.7578125 L 24.242188 3.5859375 C 23.851688 3.1954375 23.339875 3 22.828125 3 z M 17 8 L 5.2597656 19.740234 C 5.2597656 19.740234 6.1775313 19.658 6.5195312 20 C 6.8615312 20.342 6.58 22.58 7 23 C 7.42 23.42 9.6438906 23.124359 9.9628906 23.443359 C 10.281891 23.762359 10.259766 24.740234 10.259766 24.740234 L 22 13 L 17 8 z M 4 23 L 3.0566406 25.671875 A 1 1 0 0 0 3 26 A 1 1 0 0 0 4 27 A 1 1 0 0 0 4.328125 26.943359 A 1 1 0 0 0 4.3378906 26.939453 L 4.3632812 26.931641 A 1 1 0 0 0 4.3691406 26.927734 L 7 26 L 5.5 24.5 L 4 23 z"
                  />
                </svg>
              </td>

              <td class="siam3" @click="deleteData(item.id)">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 30 30"
                  width="30px"
                  height="30px"
                >
                  <path
                    d="M 14.984375 2.4863281 A 1.0001 1.0001 0 0 0 14 3.5 L 14 4 L 8.5 4 A 1.0001 1.0001 0 0 0 7.4863281 5 L 6 5 A 1.0001 1.0001 0 1 0 6 7 L 24 7 A 1.0001 1.0001 0 1 0 24 5 L 22.513672 5 A 1.0001 1.0001 0 0 0 21.5 4 L 16 4 L 16 3.5 A 1.0001 1.0001 0 0 0 14.984375 2.4863281 z M 6 9 L 7.7929688 24.234375 C 7.9109687 25.241375 8.7633438 26 9.7773438 26 L 20.222656 26 C 21.236656 26 22.088031 25.241375 22.207031 24.234375 L 24 9 L 6 9 z"
                  />
                </svg>
              </td>
            </tr>

            <tr></tr>
          </table>
        </section>

        <section id="book-list"></section>
      </section>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      items: [],
      title: "",
      author: "",
      released: "",
      scletindex: null,
      awawesome: false,
      response: null,
    };
  },
  methods: {
    async fetch() {
      const { data } = await this.$axios.get("http://localhost:3001/todo/all");
      this.items = data.todos;
      console.log(data.todos);
    },

    async postData() {
      console.log("hellop");
      try {
        const data = {
          bookName: this.title,
          author: this.author,
          released: this.released,
        };
        await this.$axios.post("http://localhost:3001/todo", data);
        this.items.push({
          bookName: this.title,
          author: this.author,
          released: this.released,
        });
        this.title = "";
        this.author = "";
        this.released = "";
        console.log(response);
      } catch (error) {
        console.log(error);
      }
    },
    async putData(id, item) {
      this.awawesome = true;
      this.title = item.bookName;
      this.author = item.author;
      this.released = item.released;
      this.scletindex = id;
    },
    async updateItem() {
      try {
        await this.$axios.put(`http://localhost:3001/todo/${this.scletindex}`, {
          bookName: this.title,
          author: this.author,
          released: this.released,
        });
      } catch (error) {
        console.log(error);
      } finally {
        this.fetch();
      }
    },
    async deleteData(id) {
      console.log("siam");
      // alert("are you sure!!");
      await this.$axios.delete(`http://localhost:3001/todo/${id}`);
      const item = this.items.map((item) => item.id).indexOf(id);
      this.items.splice(item, 1);
      this.items;

      // this.items = this.items.splice(id, 1);

      console.log(item);
    },
    catch(error) {
      console.log(error);
    },
  },

  mounted() {
    this.fetch();
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  background-color: #080809;
  background: linear-gradient(blueviolet, rgb(82, 5, 154));
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100vh;
  font-family: sans-serif;
}

.siam h1 {
  position: relative;
  color: #fff;
  margin-top: 20px;
  left: 22px;
}

.container {
  width: 100%;
  height: 600px;
  /* max-width: 1200px;
  margin: auto; */
  padding: 20px;
  color: white;
  position: relative;
}

input {
  width: 100%;
  padding: 15px;
  margin-top: 4px;
  margin-bottom: 20px;
  outline: none;
  box-sizing: border-box;
  border-radius: 5px;
  margin-top: 10px;
}

#book-list {
  display: flex;
  flex-direction: column;
  list-style: none;
  overflow: hidden;
  word-wrap: break-word;
  background: #fff;
  color: #000;
}

#book-list > section {
  display: flex;
  align-items: center;
  text-align: center;
  justify-content: space-around;
  border-bottom: 1px solid #ccc;
  padding: 10px;
}

.btn {
  background: deeppink;
  border: none;
  color: #fff;
  width: 100%;
  margin-bottom: 20px;
  font-size: 17px;
  padding: 15px;
  cursor: pointer;
  border-radius: 5px;
}

.table-section {
  width: 100%;
  display: flex;
  justify-content: space-around;
  margin-top: 8px;
}

table {
  width: 100%;
  text-align: left;
  /* height: 35vh; */
  padding: 10px;

  border-radius: 5px;
}

.header {
  width: 100%;
  height: 5vh;
  background: crimson !important;
}

tr:nth-child(odd) {
  background: rgb(214, 96, 96);
}
</style>
