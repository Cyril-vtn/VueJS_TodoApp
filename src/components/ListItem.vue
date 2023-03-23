<script>
export default {
  name: "ListItem",
  data() {
    return {
      editingIndex: null,
    };
  },
  props: { items: { type: Array, required: true } },
  methods: {
    deleteTodoItem(index) {
      // delete the right todo item from the list array
      this.items.splice(index, 1);
      // save the list array on the local storage
      localStorage.setItem("todo", JSON.stringify(this.items));
    },
    editTodoItem(index) {
      this.editingIndex = index;
    },
    saveEditedItem(index, event) {
      // save the edited item in the list array on the local storage
      localStorage.setItem("todo", JSON.stringify(this.items));
      // save the edited item
      this.items[index] = event.target.value;
      this.editingIndex = null;
    },
  },
};
</script>

<template>
  <!-- render the list on props -->
  <div v-for="(item, index) in items">
    <div class="flex justify-between gap-5 py-2 px-5 bg-slate-700 rounded">
      <div class="text-white p-2" :key="item">
        <!-- Afficher le texte ou l'input en mode édition selon la valeur de editingIndex -->
        <template v-if="editingIndex === index">
          <input
            :value="item"
            type="text"
            class="bg-slate-700 border-zinc-300 text-slate-300 rounded active:bordernone focus:outline-none"
            @blur="saveEditedItem(index, $event)"
            @keyup.enter="saveEditedItem(index, $event)"
          />
        </template>
        <template v-else>
          <p>{{ index + 1 }} - {{ item }}</p>
        </template>
      </div>
      <!-- create a trash for deleting the todo -->
      <div class="flex flex-row justify-center gap-4">
        <button
          type="button"
          @click="deleteTodoItem(index)"
          class="text-slate-300 rounded-2xl active:bordernone focus:outline-none"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M6 18L18 6M6 6l12 12"
            />
          </svg>
        </button>
        <!-- create a editsvg for edit the text -->
        <button
          type="button"
          @click="editTodoItem(index)"
          class="text-slate-300 rounded-2xl bg-slate-400active:bordernone focus:outline-none"
        >
          <!-- add the edit svg -->
          <i class="fi fi-rr-edit align-middle"></i>
        </button>
      </div>
    </div>
  </div>
</template>
