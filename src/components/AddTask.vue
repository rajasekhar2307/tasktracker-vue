<template>
  <form @submit="onSubmit" class="add-form">
    <div class="form-control">
      <label for="task">Task</label>
      <input
        type="text"
        v-model="editdata.text"
        name="text"
        placeholder="Add Task"
      />
    </div>
    <div class="form-control">
      <label for="date">Date and Time</label>
      <input
        type="text"
        v-model="editdata.day"
        name="day"
        placeholder="Add Day & time"
      />
    </div>
    <div class="form-control form-control-check">
      <label for="reminder">Would you like to remind</label>
      <input type="checkbox" v-model="editdata.reminder" name="reminder" />
    </div>
    <input type="submit" class="btn btn-block" />
    <input type="text" v-model="editdata.id" hidden />
  </form>
</template>

<script>
export default {
  name: "AddTask",
  data() {
    return {
      text: "",
      day: "",
      reminder: false,
      id: "",
    };
  },
  props: {
    editdata: Object,
  },
  methods: {
    onSubmit(e) {
      e.preventDefault();

      console.log(this.editdata);

      if (!this.editdata.text) {
        alert("Please enter a task");
        return;
      }
      console.log("id is", this.editdata.id);
      if (this.editdata.id == "") {
        const newTask = {
          // id: Math.floor(Math.random() * 10000),
          text: this.editdata.text,
          day: this.editdata.day,
          reminder: this.editdata.reminder,
        };

        console.log(newTask);

        this.$emit("add-task", newTask);
      }

      this.$emit("edit-task-db", this.editdata);

      this.editdata.text = "";
      this.editdata.day = "";
      this.editdata.reminder = "";
    },
  },
};
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>
