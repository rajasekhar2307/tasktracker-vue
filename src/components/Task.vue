<template>
  <div
    @dblclick="$emit('toggle-reminder', task.id)"
    :class="[task.reminder ? 'reminder' : '']"
  >
    <h3>
      {{ task.text }}
      <i @click="onDelete(task.id)" class="fas fa-times"></i>
    </h3>
    <p>
      {{ task.day }}

      <i
        @click="$emit('edit-task', task.id)"
        class="fas fa-pencil-alt black"
      ></i>
    </p>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: Object,
  },
  methods: {
    onDelete(id) {
      this.$emit("delete-task", id);
    },
  },
};
</script>

<style scoped>
.fas {
  color: red;
}

.black {
  color: #36454f;
}

.reminder {
  border-left: 5px solid green;
}

div {
  background-color: #f3f3f3;
  border-radius: 10px;
  padding: 1rem;
  margin-top: 1rem;
  cursor: pointer;
}

h3 {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 1.2rem;
}
p {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
p > i {
  font-size: 1.2rem;
}
</style>
