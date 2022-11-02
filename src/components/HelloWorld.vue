<template>
  <div>
    <input class="border border-black" type="text" v-model="work" ref="input" />
    <button class="border border-black" @click="addTodo">ADD</button>
    <!-- <p>{{work}}</p> -->
    <div>
      <b>Todo:</b>
      <div v-for="(todo, id) in notCompletedTodoList" :key="todo.id">
        {{ todo }}
        <button
          class="border border-black"
          @click="setTodoCompleted(id)"
        >
          Complete
        </button>
      </div>
    </div>

    <div>
      <b>Done:</b>
      <div v-for="todo in completedTodoList" :key="todo.id">
        {{ todo }}
        <button class="border border-black" @click="removeTodo(todo)">
          Delete
        </button>
        <button
          class="border border-black"
          @click="setTodoCompleted(todo, false)"
        >
          Restore
        </button>
      </div>
    </div>
  </div>
</template>

<script>
// import { ref, computed } from "vue";
import { observable } from "mobx";
import { observer } from "mobx-vue";

class Store {
  @observable
  work;
}

export default observer({
  name: "HelloWorld",
  data() {
    return {
      Store,
      notCompletedTodoList: [],
      completedTodoList: [],
      work: "",
    };
  },
  computed: {
    
  },
  methods: {
    addTodo() {
      if (this.work) {
        this.notCompletedTodoList.push(this.work)
        this.work = ''
      }
    },
    setTodoCompleted(todo) {
      this.notCompletedTodoList.filters((t) => t !== todo)
      console.log(this.notCompletedTodoList);
    },
    removeTodo() {
      console.log("remove");
    },
    handleKeyboardEvent(e) {
      switch (e.keyCode) {
        case 13:
          this.addTodo()
          e.preventDefault()
          break;
        default:
          break;
      }
    }
  },
  mounted() {
    this.$refs.input.focus();
    window.addEventListener('keyup', this.handleKeyboardEvent)
  },
  beforeUnmount() {
    window.removeEventListener('keydown', this.handleKeyboardEvent)
  },

});

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
