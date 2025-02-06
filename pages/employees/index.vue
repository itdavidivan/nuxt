<template>
  <div>
    <div class="employees-container">
      <h1>ALL EMPLOYEES</h1>
      <div
        v-for="employee in allEmployees"
        :key="employee.id"
        class="employee-card"
      >
        <nuxt-link :to="`/employees/${employee.id}`">
          <h2>Name: {{ employee.name }}</h2>
          <h3>Age: {{ employee.age }}</h3></nuxt-link
        >
        <span class="removeButton" @click="removeEmployee(employee.id)"
          >❌</span
        >
      </div>
    </div>

    <form v-on:submit.prevent="addNewEmployee">
      <label for="newName">Name: </label>
      <input required v-model="newName" type="text" id="newName" />
      <label for="newAge">Age: </label>
      <input required v-model="newAge" type="number" id="newAge" />
      <button>Add new employee</button>
    </form>
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
      allEmployees: [] as Employee[],
      newName: "",
      newAge: "",
      error: null,
    };
  },
  methods: {
    async removeEmployee(id: string) {
      try {
        const response = await axios.delete(
          `http://localhost:4000/employees/${id}`
        );
        this.allEmployees = this.allEmployees.filter((employee) => {
          return employee.id !== id;
        });
      } catch {}
    },
    async addNewEmployee() {
      try {
        const response = await axios.post("http://localhost:4000/employees", {
          name: this.newName,
          age: +this.newAge,
        });
        this.allEmployees = [...this.allEmployees, response.data];
      } catch {}
      this.newName = "";
      this.newAge = "";
    },
  },
  mounted() {
    fetch("http://localhost:4000/employees").then((httpString) =>
      httpString.json().then((employees) => {
        this.allEmployees = employees as Employee[];
      })
    );
  },
};
</script>

<style scoped>
.employees-container {
  display: grid;
  grid-template-columns: repeat(
    auto-fill,
    minmax(250px, 1fr)
  ); /* Dynamické stĺpce podľa veľkosti obrazovky */
  gap: 20px; /* Medzera medzi kartami */
  padding: 20px;
}

.employee-card {
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 20px;
  text-align: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.employee-card:hover {
  transform: translateY(-5px); /* Mierny posun pri hoveri */
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

h2 {
  font-size: 1.5rem;
  color: #333;
}

h3 {
  font-size: 1.25rem;
  color: #666;
}
.removeButton {
  cursor: pointer;
}
</style>
