<template>
  <div
    class="container-fluid py-5"
    :style="{
      'background-image':
        'url(https://i.pinimg.com/originals/5d/27/cf/5d27cf2f04ff38b851c3701bc2682683.jpg)',
      'background-size': 'cover',
      'background-repeat': 'no-repeat',
      'background-position': 'center',
      'background-attachment': 'fixed',
      'z-index': 1,
      'min-height': '100vh',
    }"
  >
    <div class="container">
      <div
        class="fs-3 fw-bolder p-3 mb-3 text-center customCard text-white primaryColor"
      >
        Todo List
      </div>

      <div class="row justify-content-center align-items-start">
        <div class="col-12 col-md-6 mb-3">
          <div class="customCard pt-3 secondaryColor">
            <div class="fs-4 fw-bolder pb-3 text-center borderBottom">
              Công việc
            </div>
            <form
              @submit.prevent="addTask"
              class="d-flex flex-column p-3 contentBgColor"
            >
              <div class="form-group mb-3">
                <label class="fs-5 fw-bolder mb-2">Tiêu đề:</label>
                <input
                  class="form-control customInput fs-6 fw-bolder"
                  placeholder="Nhập tiêu đề"
                  v-model="newTask.title"
                  id="newTaskTitle"
                  required
                />
              </div>

              <div class="form-group mb-3">
                <label class="fs-5 fw-bolder mb-2">Thời hạn:</label>
                <input
                  type="date"
                  class="form-control customInput fs-6"
                  v-model="newTask.time"
                  id="newTaskTime"
                  required
                />
              </div>

              <div class="form-group mb-3">
                <label class="fs-5 fw-bolder mb-2">Chi tiết:</label>
                <textarea
                  class="form-control customInput fs-6"
                  :style="{ minHeight: '100px' }"
                  placeholder="Chi tiết công việc"
                  v-model="newTask.des"
                  id="newTaskDes"
                />
              </div>

              <button
                type="submit"
                :class="'btn btn-primary flex-fill mt-3 fs-5 fw-bolder customInput buttonBgColor'"
                :style="{ border: 'none' }"
                @mouseover="$event.target.style.opacity = 0.5"
                @mouseout="$event.target.style.opacity = 1"
              >
                {{ selectedId === -1 ? "Thêm" : "Chỉnh sửa" }}
              </button>
            </form>
          </div>
        </div>

        <div class="col-12 col-md-6">
          <div class="pt-3 customCard tertiaryColor">
            <div class="fs-4 fw-bolder pb-3 text-center">
              Danh sách công việc
            </div>
            <div class="px-3 borderBottom">
              <div class="input-group mb-3 customInput">
                <div class="input-group-text bg-white">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="16"
                    height="16"
                    fill="currentColor"
                    class="bi bi-search"
                    viewBox="0 0 16 16"
                  >
                    <path
                      d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"
                    />
                  </svg>
                </div>
                <input
                  type="text"
                  v-model="searchValue"
                  class="form-control fs-5 fw-bolder"
                  placeholder="Tìm kiếm"
                />
              </div>
            </div>

            <div class="row px-0 mx-0">
              <div
                v-for="task in filteredTasks"
                :key="task.id"
                class="col-12 p-3 contentBgColor borderBottom contentHover"
                :style="{
                  cursor: 'pointer',
                  backgroundColor:
                    selectedId === task.id
                      ? 'rgba(255, 255, 255, 1)'
                      : new Date(date) < new Date()
                      ? 'rgba(254, 161, 161, 0.07)'
                      : '',
                  boxShadow:
                    selectedId === id
                      ? 'rgba(17, 17, 26, 0.1) 0px 8px 24px,rgba(17, 17, 26, 0.1) 0px 16px 56px, rgba(17, 17, 26, 0.1) 0px 24px 80px'
                      : '',
                }"
              >
                <div class="row align-items-center justify-content-between">
                  <div class="col" v-on:click="selectedId = task.id">
                    <div
                      class="mb-0 fs-5 fw-bolder"
                      :style="{
                        color: selectedId === task.id ? 'black' : '',
                      }"
                    >
                      {{ task.title }}
                    </div>

                    <div
                      class="mb-1 fs-6"
                      :style="{
                        color: selectedId === task.id ? 'black' : '',
                      }"
                    >
                      {{ task.time.split("-").reverse().join("/") }}
                    </div>

                    <div
                      class="text-break fs-6"
                      :style="{
                        color: selectedId === task.id ? 'black' : '',
                      }"
                    >
                      {{ task.des }}
                    </div>
                  </div>

                  <div class="col-auto">
                    <button
                      @click="deleteTask(task.id)"
                      :style="{
                        color: 'grey',
                        backgroundColor: 'transparent',
                        border: 'none',
                      }"
                      onMouseOver="(e) => { e.target.style.opacity = '0.5'; }"
                      onMouseOut="(e) => { e.target.style.opacity = '1'; }"
                      type="button"
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="32"
                        height="32"
                        fill="currentColor"
                        class="bi bi-trash-fill"
                        viewBox="0 0 16 16"
                      >
                        <path
                          d="M2.5 1a1 1 0 0 0-1 1v1a1 1 0 0 0 1 1H3v9a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V4h.5a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H10a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1H2.5zm3 4a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 .5-.5zM8 5a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7A.5.5 0 0 1 8 5zm3 .5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 1 0z"
                        />
                      </svg>
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      listTasks: [
        {
          id: 1,
          title: "Hên",
          des: "Seminar VueJS",
          done: false,
          time: "2023-11-18",
        },
        {
          id: 2,
          title: "Hay",
          des: "Cũng hay",
          done: false,
          time: "2023-11-19",
        },
        {
          id: 3,
          title: "Dữ",
          des: "Cũng dữ",
          done: false,
          time: "2023-11-17",
        },
      ],
      newTask: {
        title: "",
        des: "",
        done: false,
        time: new Date(),
      },
      selectedId: -1,
      searchValue: "",
      sortKey: "time",
      sortOrders: {
        id: 1,
        title: 1,
        des: 1,
        done: 1,
        time: 1,
      },
    };
  },
  watch: {
    selectedId(newValue) {
      if (newValue !== -1) {
        const selectedTask = this.listTasks.find(
          (task) => task.id === newValue
        );
        if (selectedTask) {
          this.newTask.title = selectedTask.title;
          this.newTask.des = selectedTask.des;
          this.newTask.done = selectedTask.done;
          this.newTask.time = selectedTask.time;
        }
      } else {
        this.resetNewTask();
      }
    },
  },
  computed: {
    sortedTasks() {
      return this.listTasks.slice().sort((a, b) => {
        const dateA = new Date(a.time);
        const dateB = new Date(b.time);

        return dateB.getTime() - dateA.getTime();
      });
    },
    filteredTasks() {
      if (this.searchValue === "") {
        return this.sortedTasks;
      }
      return this.sortedTasks.filter(
        (task) =>
          task.title.toLowerCase().includes(this.searchValue.toLowerCase()) ||
          task.des.toLowerCase().includes(this.searchValue.toLowerCase()) ||
          task.time.includes(this.searchValue.toLowerCase())
      );
    },
  },
  methods: {
    sort(key) {
      this.sortKey = key;
      this.sortOrders[key] = this.sortOrders[key] * -1;
    },
    deleteTask(taskId) {
      const index = this.listTasks.findIndex((task) => task.id === taskId);
      if (index !== -1) {
        this.listTasks.splice(index, 1);
      }
    },

    resetNewTask() {
      this.newTask = {
        title: "",
        des: "",
        done: false,
        time: new Date().toDateString(),
      };
    },
    addTask() {
      if (this.selectedId !== -1) {
        const index = this.listTasks.findIndex(
          (task) => task.id === this.selectedId
        );
        if (index !== -1) {
          this.listTasks[index].title = this.newTask.title;
          this.listTasks[index].des = this.newTask.des;
          this.listTasks[index].done = this.newTask.done;
          this.listTasks[index].time = this.newTask.time;
          this.resetNewTask();
          this.selectedId = -1;
        }
        return;
      }

      let id = 1;

      if (this.listTasks.length > 0) {
        id =
          this.listTasks.sort((a, b) => a.id - b.id)[this.listTasks.length - 1]
            .id + 1;
      }

      this.listTasks.push({
        id: id,
        title: this.newTask.title,
        des: this.newTask.des,
        done: false,
        time: this.newTask.time,
      });
      this.resetNewTask();
      console.log(this.listTasks);
    },
  },
};
</script>

<style>
.customCard {
  border-radius: 16px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  backdrop-filter: blur(15px);
  overflow: hidden;
}

.borderBottom {
  border-bottom: 1px solid rgba(0, 0, 0, 0.2);
}

.customInput {
  border-radius: 8px;
  border: none;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.primaryColor {
  background-color: rgba(255, 255, 255, 0.6);
  color: rgb(27, 50, 45) !important;
}

.secondaryColor {
  background-color: rgba(255, 255, 255, 0.6);
  color: rgb(27, 50, 45) !important;
}

.tertiaryColor {
  background-color: rgba(255, 255, 255, 0.6);
  color: rgb(27, 50, 45) !important;
}

.buttonBgColor {
  background-color: rgba(2, 119, 107, 0.9) !important;
}

.contentBgColor {
  background-color: rgba(255, 255, 255, 0);
}

.contentHover:hover {
  box-shadow: rgba(17, 17, 26, 0.1) 0px 8px 24px,
    rgba(17, 17, 26, 0.1) 0px 16px 56px, rgba(17, 17, 26, 0.1) 0px 24px 80px;
  background-color: rgba(255, 255, 255, 0.5);
}

.form-control:focus {
  border-color: transparent;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
</style>
