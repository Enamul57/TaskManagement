<template>
  <div>
    <h2 class="test_title text-center mb-4 ">All Tasks</h2>
    <div class="row">
      <div class="input-group col-md-6 page_to_page">
        <router-link :to="{ name: 'task' }" class="btn btn-primary"
          >Add Task</router-link
        >
      </div>
      <div class="input-group col-md-6">
        <input
          type="search"
          v-model="searchTerm"
          class="form-control rounded"
          placeholder="Search by title"
          aria-label="Search"
          aria-describedby="search-addon"
        />
        <button type="button" class="btn btn-primary" style="height: 38px">Search</button>
      </div>
    </div>
    <table class="table">
      <thead>
        <tr>
          <th>Task Title <i class="fa fa-sort" @click="sorting_asc"></i></th>
          <th>Client Name</th>
          <th>Deadline</th>
          <th>Task Sender</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="project in filterSearch">
          <td>{{ project.title }}</td>
          <td>{{ project.client_name }}</td>
          <td>{{ project.deadline }}</td>
          <td>{{ project.task_sender }}</td>

          <td class="remove_bg_td">

            <button class="btn" @click="deleteTask(project.id)">
              <i class="fa fa-trash"></i>
            </button>
          </td>
        </tr>

        <!-- add more rows as needed -->
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  name: "all_project",
  data() {
    return {
      projects: [],
      sorting: false,
      searchTerm: "",
    };
  },

  methods: {
    deleteTask($id){
        axios.delete('/api/task_approve/'+$id).then((res)=>{
            location.reload();
        }).catch((err)=>{
            console.log(err.response);
        })
    },
    sorting_asc() {
      this.sorting = !this.sorting;
      if (this.sorting == true) {
        return this.projects.sort((a, b) => {
          const nameA = a.deadline.toUpperCase();
          const nameB = b.deadline.toUpperCase();
          if (nameA < nameB) {
            return -1;
          }
          if (nameA > nameB) {
            return 1;
          }
          return 0;
        });
      } else {
        return this.projects.sort((a, b) => {
          const nameA = a.deadline.toUpperCase();
          const nameB = b.deadline.toUpperCase();
          if (nameA > nameB) {
            return -1;
          }
          if (nameA < nameB) {
            return 1;
          }
          return 0;
        });
      }
    },
    fetchProjects(url) {
      axios
        .get(url)
        .then((res) => {
          this.projects = res.data;
          console.log(res.data);
        })
        .catch((error) => {
        });
    },
  },
  created() {

    this.fetchProjects("/api/task_approve");
  },
  computed: {
    filterSearch() {
      return this.projects.filter((project) => {
        const search_Term = this.searchTerm.toLowerCase();
        const typeName = project.title.toLowerCase();
        return typeName.match(search_Term);
      });
    },
  },
  beforeRouteEnter(to, from, next) {
    next((vm) => {
      // Access the $router property here
      if (!User.hasLoggedIn()) {
        vm.$router.push({ name: "home" });
      }  else {
        vm.$router.push({ name: "all_task" });
      }
    });
  },
};
</script>
<style>

th {
  font-weight: bold;
  color: #473b5edb;
}
table tr:nth-child(even) {
  background-color: #605ca8 !important;
  color: white;
}
.action {
  color: white;
  background: #5d57c3;
  padding: 5px;
  width: 60px;
}
table tr:nth-child(odd) {
  color: #473b5edb;
  font-weight: bold;
}
.status_project {
  font-weight: bold;
  font-size: 1.2rem;
}
.text-warning {
  color: #35354c !important;
}
.text-info {
  color: #36becc !important;
}
.text-success {
  color: #66a756  !important;
}
.page_to_page {
  margin-bottom: 1rem;
}
table tr:nth-child(even) .remove_bg_td {
  background: white;
}
</style>
