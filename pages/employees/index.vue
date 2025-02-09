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
        console.log(response);
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
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 30px;
  padding: 30px;
  background-color: #f4f6f9;
  text-decoration: none;
}

.employee-card {
  background-color: #ffffff;
  border-radius: 15px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
  padding: 25px;
  width: 100%;
  max-width: 320px;
  text-align: center;
  position: relative;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
  overflow: hidden;
  border: 1px solid #ddd;
}

.employee-card:hover {
  transform: translateY(-12px);
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.15);
}

h2 {
  font-size: 1.7rem;
  color: #2c3e50;
  font-weight: 700;
  margin-bottom: 10px;
  text-decoration: none;
}

h3 {
  font-size: 1.3rem;
  color: #7f8c8d;
  font-weight: 400;
  margin-top: 10px;
  text-decoration: none;
}

.removeButton {
  position: absolute;
  top: 15px;
  right: 15px;
  color: #e74c3c;
  font-size: 1.6rem;
  cursor: pointer;
  transition: color 0.3s ease;
  z-index: 1;
}

.removeButton:hover {
  color: #c0392b;
}

@media (max-width: 768px) {
  .employees-container {
    flex-direction: column;
    align-items: center;
  }

  .employee-card {
    max-width: 100%;
    margin-bottom: 20px;
    padding: 20px;
  }

  h2 {
    font-size: 1.5rem;
  }

  h3 {
    font-size: 1.2rem;
  }
}

form {
  background-color: #ffffff;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  margin-top: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
a {
  text-decoration: none;
}

form label {
  font-size: 1rem;
  color: #2c3e50;
  margin-bottom: 6px;
  font-weight: 500;
}

form input {
  padding: 10px;
  margin: 8px 0;
  width: 100%;
  max-width: 300px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 1rem;
  color: #333;
}

form input:focus {
  outline: none;
  border-color: #3498db;
  box-shadow: 0 0 5px rgba(52, 152, 219, 0.5);
}

form button {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 8px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  margin-top: 15px;
  transition: background-color 0.3s ease;
}

form button:hover {
  background-color: #2980b9;
}

@media (max-width: 768px) {
  .employees-container {
    grid-template-columns: 1fr;
  }

  .employee-card {
    padding: 15px;
  }

  h2 {
    font-size: 1.4rem;
  }

  h3 {
    font-size: 1.1rem;
  }

  form input,
  form button {
    max-width: 100%;
  }
}
</style>
