<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        filled
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Add task"
        bg-color="white"
        class="col"
        square
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list separator bordered class="bg-white">
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            @click="checkTask()"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>

    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary">No tasks</div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import { useQuasar } from "quasar";

export default defineComponent({
  name: "Todo",
  data() {
    const $q = useQuasar();
    let tasks;

    if ($q.localStorage.has("tasks")) {
      tasks = $q.localStorage.getItem("tasks");
      console.log(`Ya hay tareas:`, tasks);
    } else {
      console.log(`No hay tareas, creando...`);

      tasks = [];
      $q.localStorage.set("tasks", tasks);
      console.log(`Tareas creadas.`);
    }

    return {
      newTask: "",
      tasks,
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Do don't have more weed?",
          message: "Are you really shure about that?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          console.log(this.tasks);
          this.$q.localStorage.set("tasks", this.tasks);
          this.$q.notify("Task deleted");
        });
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.$q.localStorage.set("tasks", this.tasks);
      console.log(this.tasks);
      this.newTask = "";
    },
    checkTask() {
      this.$q.localStorage.set("tasks", this.tasks);
      console.log(this.tasks);
    },
  },
});
</script>
<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-task {
  opacity: 0.5;
}
</style>
