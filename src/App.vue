<template>
  <div id="app" class="small-container">
    <h1>Employee Details</h1>
    <employee-form @add:employee="addEmployee" />

    <employee-details
      v-bind:employees="employees"
      @edit:employee="editEmployee"
      @delete:employee="deleteEmployee"
    />
  </div>
</template>

<script>
import EmployeeForm from "./components/EmployeeForm.vue";
import EmployeeDetails from "./components/EmployeeDetails.vue";

export default {
  name: "app",
  components: {
    EmployeeDetails,
    EmployeeForm,
  },

  data() {
    return {
      employees: [],
    };
  },

  methods: {
    async addEmployee(employee) {
      try {
        const response = await fetch(
          "https://ap-south-1.aws.data.mongodb-api.com/app/rage-api-ewhwq/endpoint/vue/note",

          {
            method: "POST",
            body: JSON.stringify([employee]),
            headers: { "Content-type": "application/json; charset=UTF-8" },
          }
        );
        console.log(employee);
        const data = await response.json();
        this.employees = [...this.employees, data];
      } catch (error) {
        console.error("Error occured while adding employee: " + error);
      }
    },

    async getEmployees() {
      try {
        const response = await fetch(
          `https://ap-south-1.aws.data.mongodb-api.com/app/rage-api-ewhwq/endpoint/vue/note`
        );
        const data = await response.json();
        this.employees = data;
        console.log(data);
      } catch (error) {
        console.error("Error occured while retrieving employees: " + error);
      }
    },

    async editEmployee(_id, updatedEmployee) {
      try {
        const response = await fetch(
          `https://ap-south-1.aws.data.mongodb-api.com/app/rage-api-ewhwq/endpoint/vue/note?_id=${_id}`,
          {
            method: "PUT",
            body: JSON.stringify(updatedEmployee),
            headers: { "Content-type": "application/json; charset=UTF-8" },
          }
        );

        const data = await response.json();
        this.employees = this.employees.map((employee) =>
          employee._id === _id ? data : employee
        );
        console.log(data);
      } catch (error) {
        console.error("Error while editing: ", +error);
      }
    },

    async deleteEmployee(_id) {
      try {
        await fetch(
          `https://ap-south-1.aws.data.mongodb-api.com/app/rage-api-ewhwq/endpoint/vue/note?_id=${_id}`,
          {
            method: "DELETE",
          }
        );
        const data = await response.json();
        console.log(data);
        this.employees = this.employees.filter(
          (employee) => employee._id !== _id
        );
      } catch (error) {
        console.error("Error while deleting: ", +error);
      }
    },
  },
  mounted() {
    this.getEmployees();
  },
};
</script>

<style>
#employee-details {
  width: 100%;
}

button {
  background: #41b883;
  border: 1px solid #41b883;
}

.delete-button {
  background: #d11a2a;
  border: 1px solid #d11a2a;
}

.small-container {
  max-width: 720px;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 10px;
  margin-left: 10px;
}
</style>
