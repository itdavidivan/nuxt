<template>
  <div class="container">
    <div class="employee-details">
      <h1 class="employee-name">Name: {{ employee.name }}</h1>
      <h2 class="employee-age">Age: {{ employee.age }}</h2>
      <p class="employee-id">Employee ID: {{ employeeId }}</p>
    </div>

    <form v-on:submit.prevent="editEmployee" class="edit-form">
      <label for="newName" class="label">Name: </label>
      <input
        required
        v-model="newName"
        type="text"
        id="newName"
        class="input"
        placeholder="Enter name"
      />

      <label for="newAge" class="label">Age: </label>
      <input
        required
        v-model="newAge"
        type="number"
        id="newAge"
        class="input"
        placeholder="Enter age"
      />

      <button type="submit" class="button">Update Employee</button>
    </form>

    <form v-on:submit.prevent="editAge" class="edit-form">
      <label for="newEditAge" class="label">New Age: </label>
      <input
        required
        v-model="newEditAge"
        type="number"
        id="newEditAge"
        class="input"
        placeholder="Enter new age"
      />

      <button type="submit" class="button">Update Age</button>
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

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 20px;
  background-color: #f7f9fc;
  font-family: "Arial", sans-serif;
  color: #333;
  min-height: 100vh;
}

.employee-details {
  text-align: center;
  margin-bottom: 30px;
}

.employee-name,
.employee-age {
  font-size: 2rem;
  color: #2c3e50;
  font-weight: 700;
}

.employee-id {
  font-size: 1rem;
  color: #7f8c8d;
  margin-top: 5px;
}

.edit-form {
  width: 100%;
  max-width: 400px;
  background-color: #ffffff;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

.label {
  font-size: 1rem;
  font-weight: 500;
  color: #2c3e50;
  margin-bottom: 6px;
  display: block;
}

.input {
  width: 100%;
  padding: 12px;
  margin: 8px 0 16px 0;
  font-size: 1rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  color: #333;
  transition: border-color 0.3s ease;
}

.input:focus {
  outline: none;
  border-color: #3498db;
  box-shadow: 0 0 5px rgba(52, 152, 219, 0.5);
}

.button {
  width: 100%;
  padding: 12px;
  background-color: #3498db;
  color: white;
  font-weight: 600;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1.1rem;
  transition: background-color 0.3s ease;
}

.button:hover {
  background-color: #2980b9;
}

@media (max-width: 768px) {
  .employee-name,
  .employee-age {
    font-size: 1.5rem;
  }

  .input,
  .button {
    font-size: 1rem;
    padding: 10px;
  }

  .container {
    padding: 10px;
  }
}
</style>
