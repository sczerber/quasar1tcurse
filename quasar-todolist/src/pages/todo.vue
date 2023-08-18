<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input @keyup.enter="addTask" filled v-model="newTask" bg-color="white" class="col" square placeholder="Add task" dense>
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list
    separator
    bordered
    >


      <q-item
        @click="task.done = !task.done"
        clickable
        :class="{'done bg-green-2': task.done}"
        v-for="(task, index) in tasks" :key="task.title"
        tag="label" v-ripple
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" val="teal" color="orange"/>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section
        v-if="task.done"
        side
        >
          <q-btn
            @click.stop="deleteTask(index)"
            round color="amber" glossy text-color="black" icon="delete" />
        </q-item-section>
      </q-item>


    </q-list>
    <div
      v-if="!tasks.length"
      class="no-task absolute-center">
      <div class="text-purple-4 text-primary text-h3 text-center">There is no tasks!</div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [
        // {
        //   title: 'task1',
        //   done: true,
        // },
        // {
        //   title: 'task2',
        //   done: false,
        // },
        // {
        //   title: 'task3',
        //   done: false,
        // }
      ]
    };
  },
  methods: {
    deleteTask(index){
      this.$q.dialog({
        dark: true,
        title: 'Are u sure?',
        message: 'Are u really want to delete this?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1);
        this.$q.notify({
          message: 'Task deleted!',
          icon: 'announcement'
        })
      });



    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false
      });
      this.newTask = '';
    }
  }
}

</script>
<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: green;
  }
}
</style>
