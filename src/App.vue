<script setup>
// import mounted
import { ref, onMounted } from "vue";
// import { RouterLink, RouterView } from "vue-router";
import ListItem from "./components/ListItem.vue";
// récupérér les donnée de l'input
const search = ref("");
const list = ref([]);
// fonction qui permet de récupérer les données de l'input et les les store dans une state
const AddTodo = (e) => {
  e.preventDefault();
  // récupérer les données de l'input
  const searchValue = search.value;
  list.value.push(searchValue);
  // reset l'input
  search.value = "";
  // stocker les données dans le local storage
  localStorage.setItem("todo", JSON.stringify(list.value));
};

// recuperer les item dans le local storage quand la page est rechargé
const getTodo = () => {
  const todo = localStorage.getItem("todo");
  if (todo) {
    list.value = JSON.parse(todo);
  }
};

// executer la fonction getTodo quand la page est rechargé
onMounted(getTodo);
</script>

<template>
  <div
    class="flex flex-col gap-5 justify-center items-center h-screen bg-gray-800"
  >
    <!-- afficher le titre -->
    <div class="text-white text-4xl font-semibold">
      <h1>TODO</h1>
    </div>
    <div class="flex flex-col gap-5 w-1/3">
      <form class="flex gap-5 justify-between">
        <input
          type="text"
          class="bg-gray-700 text-slate-300 p-2 rounded active:bordernone focus:outline-none w-full font-semibold"
          placeholder="TODO"
          v-model="search"
        />

        <div
          class="w-40 bg-gray-700 text-slate-300 rounded px-2 flex justify-center center hover:bg-slate-300 transition-all hover:text-gray-700 duration-300 cursor-pointer"
        >
          <button
            type="submit"
            @click="AddTodo"
            class="active:bordernone focus:outline-none font-semibold"
          >
            ADD TODO
          </button>
        </div>
      </form>
      <div class="flex flex-col gap-5">
        <!-- rendre le composant ListItem pour chaque input -->
        <ListItem :items="list" />
      </div>
    </div>
  </div>
</template>
