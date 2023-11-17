<template>
  <div>
    <div class="card">
      <div class="card-header">
        <h4>
          Students
          <button @click="showModal" class="btn btn-primary float-end">
            Add student
          </button>
        </h4>
      </div>
      <div class="card-body">
        <table class="table table-bordered">
          <thead>
            <tr>
              <th>Id</th>
              <th>Name</th>
              <th>Email</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="student in sortedStudents" :key="student.id">
              <td>{{ student.id }}</td>
              <td>{{ student.name }}</td>
              <td>{{ student.email }}</td>
              <td>
                <button
                  @click="deleteStudent(student.id)"
                  class="btn btn-danger"
                >
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div class="modal" :class="{ 'modal-visible': isModalVisible }">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">Add Student</h5>
          <button @click="hideModal" class="close">&times;</button>
        </div>
        <div class="modal-body">
          <!-- Form fields for adding a new student -->
          <form @submit.prevent="addStudent">
            <div class="mb-3">
              <label for="newStudentName" class="form-label">Name</label>
              <input
                v-model="newStudent.name"
                type="text"
                class="form-control"
                id="newStudentName"
                required
              />
            </div>
            <div class="mb-3">
              <label for="newStudentEmail" class="form-label">Email</label>
              <input
                v-model="newStudent.email"
                type="email"
                class="form-control"
                id="newStudentEmail"
                required
              />
            </div>
            <button type="submit" class="btn btn-primary">Save</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
  position: relative;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-visible {
  display: block;
}
</style>

<script>
export default {
  data() {
    return {
      listStudents: [
        { id: 1, name: "John Doe", email: "john@example.com" },
        { id: 2, name: "Jane Doe", email: "jane@example.com" },
      ],
      isModalVisible: false,
      newStudent: {
        name: "",
        email: "",
      },
      sortKey: "id",
      sortOrders: {
        id: 1,
        name: 1,
        email: 1,
      },
    };
  },
  computed: {
    sortedStudents() {
      return this.listStudents.slice().sort((a, b) => {
        const order = this.sortOrders[this.sortKey];
        return order * (a[this.sortKey] > b[this.sortKey] ? 1 : -1);
      });
    },
  },
  methods: {
    sort(key) {
      this.sortKey = key;
      this.sortOrders[key] = this.sortOrders[key] * -1;
    },
    deleteStudent(studentId) {
      const index = this.listStudents.findIndex(
        (student) => student.id === studentId
      );
      if (index !== -1) {
        this.listStudents.splice(index, 1);
      }
    },
    showModal() {
      this.isModalVisible = true;
    },
    hideModal() {
      this.isModalVisible = false;

      this.newStudent = {
        name: "",
        email: "",
      };
    },
    addStudent() {
      this.listStudents.push({
        id: this.listStudents.length + 1,
        name: this.newStudent.name,
        email: this.newStudent.email,
      });

      this.hideModal();
    },
  },
};
</script>
