<template>
  <main id="todolist">
    <h1>You have {{ items.length }} Todos</h1>
    <div class="items" v-if="items.length">
      <ul>
        <li v-for="(item, index) in items" :key="index">
          <span class="label">{{item.title}}</span>
          <div class="actions">
            <button class="btn-picto" type="button" aria-label="Delete" @click="deleteItem(index)">
              <i aria-hidden="true" class="material-icons">delete</i>
            </button>
          </div>
        </li>
      </ul>
    </div>

    <form @submit.prevent="addItem">
      <!-- yonlendirmeyi engellemek icin submit.prevent kullaniyoruz -->
      <input
        type="text"
        name="newitem"
        id="newitem"
        placeholder="Enter Item"
        v-model="newItemTitle"
      />
      <button type="submit">Submit</button>
    </form>
  </main>
</template>

<script>
export default {
  data() {
    return {
      newItemTitle: "",
      items: [],
    };
  },
  methods: {
    addItem() {
      if (this.newItemTitle === "") {
        return;
      }
      this.items.push({
        title: this.newItemTitle,
      });
      this.newItemTitle = "";
    },
    deleteItem(index) {
      // index'e karsılık gelen elemanı sil
      this.items.splice(index, 1);
    },
  },
  mounted() {
    // sayfa yuklendiginde local storage'dan gelen verilerle items' i guncelle
    const items = localStorage.getItem("todos") || "[]";
    this.items = JSON.parse(items);
  },
  watch: {
    items(items) {
      // json objesini text haline getirmeliyiz
      localStorage.setItem("todos", JSON.stringify(items));
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#todolist {
  margin: 3rem auto;
  padding: 2rem 3rem 3rem;
  max-width: 500px;
  background: #1979a9;
  color: #fff;
}
#todolist h1 {
  text-align: center;
  font-size: 2rem;
  border-bottom: 1px solid white;
}
#todolist ul {
  margin-top: 2.6rem;
  list-style: none;
}

#todolist li {
  display: flex;
  font-size: 1.2rem;
  font-weight: 500;
  margin: 0 -3rem 4px;
  padding: 1.1rem 3rem;
  justify-content: space-between;
  align-items: center;
  background: rgba(255, 255, 255, 0.2);
}

#todolist .btn-picto {
  border: none;
  background: none;
  cursor: pointer;
  color: #fff;
}

form {
  margin-top: 3rem;
  display: flex;
  flex-wrap: wrap;
}
form label {
  min-width: 100%;
  margin-bottom: 0.5rem;
  font-size: 1.3rem;
}
form input {
  flex-grow: 1;
  border: none;
  background: #f7f1f1;
  padding: 0 1.5em;
  font-size: initial;
  border-radius: 5px;
}
form button {
  padding: 0 1.3rem;
  border: none;
  background: #1979a9;
  color: white;
  text-transform: uppercase;
  font-weight: bold;
  border: 1px solid rgba(255, 255, 255, 0.3);
  margin-left: 5px;
  cursor: pointer;
  border-radius: 5px;
}
form input,
form button {
  height: 3rem;
}
</style>
