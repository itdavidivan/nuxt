<template>
  <div>
    <div>
      <h1>Name: {{ employee.name }}</h1>
      <h2>Age: {{ employee.age }}</h2>
      {{ employeeId }}
    </div>
    <form v-on:submit.prevent="editEmployee">
      <label for="newName">Name: </label>
      <input required v-model="newName" type="text" id="newName" />
      <label for="newAge">Age: </label>
      <input required v-model="newAge" type="number" id="newAge" />
      <button>Edit new employee</button>
    </form>
    <div>
      <form v-on:submit.prevent="editAge">
        <label for="newEditAge">Age: </label>
        <input required v-model="newEditAge" type="number" id="newEditAge" />
        <button>Edit new employee</button>
      </form>
    </div>
  </div>
</template>

<script lang="ts">
type Employee = {
  name: string;
  age: number;
  id: string;
};
import axios from "axios";
export default {
  data() {
    return {
      employeeId: this.$route.params.id,
      employee: {} as Employee,
      newName: "",
      newAge: "",
      newEditAge: "",
    };
  },
  methods: {
    async editAge() {
      await axios.patch(`http://localhost:4000/employees/${this.employeeId}`, {
        age: +this.newEditAge,
      });
      this.employee = {
        ...this.employee,
        age: +this.newEditAge,
      };
      this.newEditAge = "";
    },
    async editEmployee() {
      await axios.put(`http://localhost:4000/employees/${this.employeeId}`, {
        name: this.newName,
        age: +this.newAge,
      });
      this.employee = {
        name: this.newName,
        age: +this.newAge,
        id: this.employee.id,
      };
      this.newName = "";
      this.newAge = "";
    },
  },
  mounted() {
    fetch(`http://localhost:4000/employees/${this.employeeId}`).then(
      (httpString) =>
        httpString.json().then((employee) => {
          this.employee = employee as Employee;
        })
    );
  },
};
</script>

<style lang="scss" scoped></style>
