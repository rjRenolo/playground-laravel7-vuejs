<template>
  <div class="w-50">
    <form @submit.prevent="submitData">
      <div class="input-group mb-3 w-100">
        <input
          type="text"
          v-model="form.title"
          :class="{'is-invalid' : form.errors.has('title')}"
          class="form-control form-control-lg"
          aria-label="Recipient's username"
          aria-describedby="button-addon2"
          @keydown="form.errors.clear('title')"
        />
        <div class="input-group-append">
          <button class="btn btn-success" type="button" id="button-addon2">Add Todo</button>
        </div>
      </div>

      <span
        class="text-danger pt-3"
        style="font-size: 20px; padding-bottom:3px;"
        v-if="form.errors.has('title')"
        v-text="form.errors.get('title')"
      ></span>
    </form>
    <!-- <div class="w-25" style="border: 1px solid red">
      <div class="w-100" v-for="todo in todos" :key="todo.id">{{todo.title}}</div>
    </div>-->
    <div class="card-hover-shadow-2x mb-3 card">
      <div class="card-header-tab card-header">
        <div class="card-header-title font-size-lg text-capitalize font-weight-normal">
          <i class="fa fa-tasks"></i>&nbsp;Task Lists
        </div>
      </div>
      <div class="scroll-area-sm">
        <perfect-scrollbar class="ps-show-limits">
          <div style="position: static;" class="ps ps--active-y">
            <div class="ps-content">
              <ul class="list-group list-group-flush">
                <li class="list-group-item">
                  <div class="todo-indicator bg-warning"></div>
                  <div class="widget-content p-0">
                    <div class="widget-content-wrapper">
                      <div class="widget-content-left mr-2">
                        <div class="custom-checkbox custom-control">
                          <input
                            class="custom-control-input"
                            id="exampleCustomCheckbox12"
                            type="checkbox"
                          />
                          <label class="custom-control-label" for="exampleCustomCheckbox12">&nbsp;</label>
                        </div>
                      </div>
                      <div class="widget-content-left">
                        <div class="widget-heading">
                          Call Sam For payments
                          <div class="badge badge-danger ml-2">Rejected</div>
                        </div>
                        <div class="widget-subheading">
                          <i>By Bob</i>
                        </div>
                      </div>
                      <div class="widget-content-right">
                        <button class="border-0 btn-transition btn btn-outline-success">
                          <i class="fa fa-check"></i>
                        </button>
                        <button class="border-0 btn-transition btn btn-outline-danger">
                          <i class="fa fa-trash"></i>
                        </button>
                      </div>
                    </div>
                  </div>
                </li>
                <li class="list-group-item">
                  <div class="todo-indicator bg-focus"></div>
                  <div class="widget-content p-0">
                    <div class="widget-content-wrapper">
                      <div class="widget-content-left mr-2">
                        <div class="custom-checkbox custom-control">
                          <input
                            class="custom-control-input"
                            id="exampleCustomCheckbox1"
                            type="checkbox"
                          />
                          <label class="custom-control-label" for="exampleCustomCheckbox1">&nbsp;</label>
                        </div>
                      </div>
                      <div class="widget-content-left">
                        <div class="widget-heading">Make payment to Bluedart</div>
                        <div class="widget-subheading">
                          <div>
                            By Johnny
                            <div class="badge badge-pill badge-info ml-2">NEW</div>
                          </div>
                        </div>
                      </div>
                      <div class="widget-content-right">
                        <button class="border-0 btn-transition btn btn-outline-success">
                          <i class="fa fa-check"></i>
                        </button>
                        <button class="border-0 btn-transition btn btn-outline-danger">
                          <i class="fa fa-trash"></i>
                        </button>
                      </div>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </perfect-scrollbar>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: new Form({
        title: "",
      }),
      todos: null,
    };
  },
  methods: {
    submitData() {
      let data = new FormData();
      data.append("title", this.form.title);
      axios
        .post("/api/todo", data)
        .then((res) => {
          this.form.reset();
          this.getAllTodo();
        })
        .catch((err) => {
          this.form.errors.record(err.response.data.errors);
        });
    },
    getAllTodo() {
      axios
        .get("/api/todo")
        .then((response) => (this.todos = response.data))
        .catch((err) => console.log(err));
    },
  },

  mounted() {
    console.log("Component mounted.");
    this.getAllTodo();
  },
};
</script>

<style scoped>
i {
  font-style: italic;
}

.container {
  margin-top: 100px;
}

.card {
  box-shadow: 0 0.46875rem 2.1875rem rgba(4, 9, 20, 0.03),
    0 0.9375rem 1.40625rem rgba(4, 9, 20, 0.03),
    0 0.25rem 0.53125rem rgba(4, 9, 20, 0.05),
    0 0.125rem 0.1875rem rgba(4, 9, 20, 0.03);
  border-width: 0;
  transition: all 0.2s;
}

.card-header {
  display: flex;
  align-items: center;
  border-bottom-width: 1px;
  padding-top: 0;
  padding-bottom: 0;
  padding-right: 0.625rem;
  height: 3.5rem;
  background-color: #fff;
}

.widget-subheading {
  color: #858a8e;
  font-size: 10px;
}

.card-header.card-header-tab .card-header-title {
  display: flex;
  align-items: center;
  white-space: nowrap;
}

.card-header .header-icon {
  font-size: 1.65rem;
  margin-right: 0.625rem;
}

.card-header.card-header-tab .card-header-title {
  display: flex;
  align-items: center;
  white-space: nowrap;
}

.btn-actions-pane-right {
  margin-left: auto;
  white-space: nowrap;
}

.text-capitalize {
  text-transform: capitalize !important;
}

.scroll-area-sm {
  height: 288px;
  overflow-x: hidden;
}

.list-group-item {
  position: relative;
  display: block;
  padding: 0.75rem 1.25rem;
  margin-bottom: -1px;
  background-color: #fff;
  border: 1px solid rgba(0, 0, 0, 0.125);
}

.list-group {
  display: flex;
  flex-direction: column;
  padding-left: 0;
  margin-bottom: 0;
}

.todo-indicator {
  position: absolute;
  width: 4px;
  height: 60%;
  border-radius: 0.3rem;
  left: 0.625rem;
  top: 20%;
  opacity: 0.6;
  transition: opacity 0.2s;
}

.bg-warning {
  background-color: #f7b924 !important;
}

.widget-content {
  padding: 1rem;
  flex-direction: row;
  align-items: center;
}

.widget-content .widget-content-wrapper {
  display: flex;
  flex: 1;
  position: relative;
  align-items: center;
}

.widget-content .widget-content-right.widget-content-actions {
  visibility: hidden;
  opacity: 0;
  transition: opacity 0.2s;
}

.widget-content .widget-content-right {
  margin-left: auto;
}

.btn:not(:disabled):not(.disabled) {
  cursor: pointer;
}

.btn {
  position: relative;
  transition: color 0.15s, background-color 0.15s, border-color 0.15s,
    box-shadow 0.15s;
}

.btn-outline-success {
  color: #3ac47d;
  border-color: #3ac47d;
}

.btn-outline-success:hover {
  color: #fff;
  background-color: #3ac47d;
  border-color: #3ac47d;
}

.btn-outline-success:hover {
  color: #fff;
  background-color: #3ac47d;
  border-color: #3ac47d;
}

.btn-primary {
  color: #fff;
  background-color: #3f6ad8;
  border-color: #3f6ad8;
}

.btn {
  position: relative;
  transition: color 0.15s, background-color 0.15s, border-color 0.15s,
    box-shadow 0.15s;
  outline: none !important;
}

.card-footer {
  background-color: #fff;
}
</style>