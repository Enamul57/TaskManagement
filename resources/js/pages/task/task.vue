<template>
  <div>
   <div class=' col-md-12' style='text-align:right'>

        <router-link :to="{ name: 'all_task' }" class="btn btn-primary"
          >All Tasks</router-link
        >

   </div>
    <div class="row">

      <div class="col-md-12 mx-auto">
        <form @submit.prevent="addAccountType()">
          <div class="form-group mx-sm-3 mb-2 col-md-4">
            <label for="projectName" class="test_text">Select Client</label>
            <select
                  class="form-control status"
                  id="status"
                  v-model="forms.client_name"
                  required
                >
                  <option
                    v-for="(project, index) in clients"
                    :key="index"
                    :value="project.username"
                  >
                    {{ project.username }}
                  </option>
                </select>
          </div>
          <div class="form-group mx-sm-3 mb-2 col-md-4">
            <label for="projectName" class="test_text">Title</label>
             <input
          type="search"
          v-model="forms.title"
          class="form-control rounded"
          placeholder="Task Title.."

          aria-describedby="search-addon"
        />
          </div>
  <div class="form-group mx-sm-3 mb-2 col-md-4">
            <label for="projectName" class="test_text">Description</label>
             <textarea rows="" type="search"
          v-model="forms.description"
          class="form-control rounded"
          placeholder="Description .. "
          aria-label="Search"
          aria-describedby="search-addon" cols=""></textarea>


          </div>  <div class="form-group mx-sm-3 mb-2 col-md-4">
            <label for="projectName" class="test_text">Deadline</label>
             <input
          type="date"
          v-model="forms.deadline"
          class="form-control rounded"
          placeholder="Search by Type"
          aria-label="Search"
          aria-describedby="search-addon"
        />
          </div>
          <div class="form-group mx-sm-3 mb-2 col-md-4 btn_holder">
                     <button type="submit" class="btn btn-primary mb-2">Save</button>

          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "add_reference",
  beforeRouteEnter(to, from, next) {
    next((vm) => {
      // Access the $router property here
      if (!User.hasLoggedIn()) {
        vm.$router.push({ name: "home" });
      } else {
        vm.$router.push({ name: "task" });
      }
    });
  },
  data() {
    return {
        forms:{
            client_name:"",
            title:"",
            description:"",
            deadline:"",
            username:"",
        },

      clients:[],
      errors: {},
    };
  },
  created() {
    this.forms.username = User.getUserName();

    axios
      .post("/api/checkClient", this.forms)
      .then((res) => {
        this.clients = res.data;
        console.log(res.data);
      })
      .catch((err) => {
        console.log(err.res);
      });
  },
  methods: {
    addAccountType() {
        console.log(this.forms);
      axios
        .post("/api/task_approve", this.forms, {
          headers: {
            "Content-Type": "application/json",
            Accept: "application/json",
          },
        })
        .then((res) => {
           this.$router.push({name:'all_task'});
        })
        .catch((error) => {
          this.errors = JSON.parse(error.response.data);
        });
    },
  },
};
</script>
<style>
    .test_text{
        font-size:1.2em;
        color:#4e3089;
        font-weight:bold;
    }
    .btn_holder{
        align-items:right;
    }
    .test_title{
        color:#605ca8!important;
    }
</style>
