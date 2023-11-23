<template>
  <input type="file" @change="loadFromFile" />
  <button @click="saveToFile">Save to File</button>
  <br>
  <br>
  <div>
    <input v-model="newTodo" type="text" placeholder="Add a new task" />
    <button @click="addTodo">Add</button>
  </div>
  <br>
  <div style="display: flex; justify-content: space-around; border: solid black 2px; height: 100%;">
    <div style="background-color: lightcoral; width: 100%;">
      <div style="border: solid black 2px;">
        <h1>Todo</h1>
      </div>
      <div style="border: solid black 2px; height: 19.05rem; padding-top: 0.5rem;">
        <draggable 
        v-model="todoArray" 
        group="kanban" 
        @start="drag=true" 
        @end="drag=false" 
        item-key="id">
          <template #item="{element}">
            <div style="height: 1.5rem; padding-top: 0.4rem;">
              <span style="margin-left: 1.5rem;">
                {{element.name}}
              </span>
              <button @click="deleteTask('todoArray', element.id)" style="float: right; background: none; border: none;">
                <img src="../assets/x-symbol.svg" alt="x-symbol" style="height: 1rem; width: 1rem;">
              </button>
            </div>
          </template> 
        </draggable>
      </div>
    </div>
    <div style="background-color: lightblue; width: 100%;">
      <div style="border: solid black 2px;">
        <h1>In progress</h1>
      </div>
      <div style="border: solid black 2px; height: 19.05rem; padding-top: 0.5rem;">
        <draggable 
        v-model="inProgressArray" 
        group="kanban" 
        @start="drag=true" 
        @end="drag=false" 
        item-key="id">
          <template #item="{element}">
            <div style="height: 1.5rem; padding-top: 0.4rem;">
              <span style="margin-left: 1.5rem;">
                {{element.name}}
              </span>
              <button @click="deleteTask('inProgressArray', element.id)" style="float: right; background: none; border: none;">
                <img src="../assets/x-symbol.svg" alt="x-symbol" style="height: 1rem; width: 1rem;">
              </button>
            </div>
          </template>
        </draggable>
      </div>
    </div>
    <div style="background-color: lightgreen; width: 100%;">
      <div style="border: solid black 2px;">
        <h1>Finished</h1>
      </div>
      <div style="border: solid black 2px; height: 19.05rem; padding-top: 0.5rem;">
        <draggable 
        v-model="finishedArray" 
        group="kanban" 
        @start="drag=true" 
        @end="drag=false" 
        item-key="id">
          <template #item="{element}">
            <div style="height: 1.5rem; padding-top: 0.4rem;">
              <span style="margin-left: 1.5rem;">
                {{element.name}}
              </span>
              <button @click="deleteTask('finishedArray', element.id)" style="float: right; background: none; border: none;">
                <img src="../assets/x-symbol.svg" alt="x-symbol" style="height: 1rem; width: 1rem;">
              </button>
            </div>
          </template>
        </draggable>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from 'vuedraggable';

export default {
  components: {
    draggable,
  },
  data() {
    return {
      drag: false,
      todoArray: [
        { id: 1, name: "Extras" },
        { id: 2, name: "Fix button" },
        { id: 3, name: "Fix frontpage bug" },
        { id: 4, name: "Add extra spiff" },
        { id: 5, name: "Add css" }
      ],
      inProgressArray: [
        { id: 6, name: "Hotfixes" },
        { id: 7, name: "Increase efficiency of current functionality" },
        { id: 8, name: "Allow bulk updates" },
      ],
      finishedArray: [
        { id: 9, name: "Integrate components" },
        { id: 10, name: "Add core functionality" }
      ],
    };
  },
  methods: {
    async saveToFile() {
      const content = JSON.stringify({
        todoArray: this.todoArray,
        inProgressArray: this.inProgressArray,
        finishedArray: this.finishedArray,
      });

      const blob = new Blob([content], { type: 'text/plain' });
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = 'todos.txt';
      a.click();
    },
    async loadFromFile(event) {
      const file = event.target.files[0];

      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          try {
            const data = JSON.parse(e.target.result);
            this.todoArray = data.todoArray;
            this.inProgressArray = data.inProgressArray;
            this.finishedArray = data.finishedArray;
          } catch (error) {
            console.error('Error reading file:', error);
          }
        };
        reader.readAsText(file);
      }
    },
    addTodo() {
      if (this.newTodo.trim() !== "") {
        const newTask = {
          id: this.todoArray.length + 1,
          name: this.newTodo.trim(),
        };
        this.todoArray.push(newTask);
        this.newTodo = ""; // Clear the input field after adding the task
      }
    },
    deleteTask(arrayName, taskId) {
      const targetArray = this[arrayName];
      const index = targetArray.findIndex((task) => task.id === taskId);

      if (index !== -1) {
        targetArray.splice(index, 1);
      }
    }
  },
};
</script>

<style scoped>
.column {
  margin: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
  display: inline-block;
}

.task {
  margin: 5px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #fff;
  cursor: grab;
}
</style>
