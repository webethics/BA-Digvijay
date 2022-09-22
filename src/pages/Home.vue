<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";
const showPrevios = ref(true);
const showNext = ref(false);
const users = ref([]);
const amount = ref(0);
const jobOwner = ref('-');

onMounted(() => {
  fetchUsers();
});
function fetchUsers() {
  axios.get("https://demo-api.bettercommissions.com/interview-data/users").then((response) => {
    users.value = response.data.users;
  });
}
function setShowPre() {
  showPrevios.value = true;
  showNext.value = false;
}

function setShowNext() {
  showPrevios.value = false;
  showNext.value = true;
}

defineProps({
  msg: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <h2 class="title">Project</h2>
        <div class="app-bar">
          <div class="blurb-holder" :class="{ active: showNext }">
            <div class="blurb active"><span class="counter">1</span> Configuration</div>
            <div class="blurb" :class="{ active: showNext }">
              <span class="counter">2</span> Summary
            </div>
          </div>
        </div>
      </div>

      <div class="col-12" v-if="showPrevios">
        <div class="table-responsive">
          <table class="table prj-table">
            <thead>
              <tr>
                <th scope="col">Add plan name</th>
                <th scope="col">$ Amount</th>
                <th scope="col">Job Owner</th>
                <th scope="col"></th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Product name"
                    aria-describedby="emailHelp"
                  />
                </td>
                <td>
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Dollar amount"
                    v-model="amount"
                    aria-describedby="emailHelp"
                  />
                </td>
                <td>
                  <select
                    class="form-select"
                    aria-label="Default select example"
                    v-model="jobOwner"
                  >
                    <option selected disabled>Open this select menu</option>
                    <option v-for="(user, index) in users" :key="index">{{ user.name }}</option>
                    <option value="1">One</option>
                    <option value="2">Two</option>
                    <option value="3">Three</option>
                  </select>
                </td>
                <td>
                  <svg
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <path
                      d="M6 19C6 20.1 6.9 21 8 21H16C17.1 21 18 20.1 18 19V7H6V19ZM19 4H15.5L14.5 3H9.5L8.5 4H5V6H19V4Z"
                      fill="#3A3541"
                      fill-opacity="0.54"
                    />
                  </svg>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="app-foo">
          <a href="#" class="link">+ add row</a>
          <button class="btn btn-primary" type="button" @click.prevent="setShowNext()">Next</button>
        </div>
      </div>
    </div>
  </div>

  <div class="project-details" v-if="showNext">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <table class="table mb-0">
            <thead>
              <tr>
                <th scope="col fw-bold">$ Sum</th>
                <th scope="col fw-bold">Job Owner</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>{{ amount }}</td>
                <td>{{ jobOwner }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-12">
          <a
            href="#"
            class="back-link d-inline-block mt-4 text-decoration-none text-uppercase"
            @click.prevent="setShowPre()"
            >&lt; back
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.title {
  font-weight: 600;
  font-size: 24px;
  color: #000000;
  margin-bottom: 24px;
}
.app-bar {
  background: #ffffff;
  box-shadow: 0px 3px 1px -2px rgba(0, 0, 0, 0.2), 0px 2px 2px rgba(0, 0, 0, 0.14),
    0px 1px 5px rgba(0, 0, 0, 0.12);
  padding: 24px;
  margin-bottom: 48px;
}
.blurb-holder {
  display: flex;
  justify-content: space-between;
  position: relative;
  margin: 0 -8px;
}
.blurb {
  font-size: 16px;
  color: rgba(0, 0, 0, 0.38);
  display: flex;
  align-items: center;
  background-color: #ffffff;
  padding: 0 8px;
  position: relative;
  z-index: 2;
}
.blurb.active {
  color: #000000;
}
.blurb-holder::after {
  content: "";
  height: 1px;
  background-color: rgba(0, 0, 0, 0.12);
  display: block;
  position: absolute;
  left: 0;
  width: 100%;
  top: 50%;
  transform: translateY(-50%);
}
.blurb-holder.active::after {
  background-color: #1976d2;
}
.counter {
  width: 24px;
  height: 24px;
  background: rgba(0, 0, 0, 0.38);
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  color: #fff;
  margin-right: 8px;
}
.active .counter {
  background-color: #1976d2;
}
.table > :not(caption) > * > * {
  border-color: rgba(0, 0, 0, 0.12);
  box-shadow: none;
  padding: 16px;
  padding: 16px;
  vertical-align: middle;
}
.table th {
  font-size: 12px;
  font-weight: 700;
  color: rgba(0, 0, 0, 0.6);
}
.form-control,
.form-select {
  border: 1px solid rgba(0, 0, 0, 0.42);
  border-radius: 4px;
  min-height: 40px;
}
.form-control::placeholder {
  font-size: 16px;
  color: rgba(0, 0, 0, 0.87);
}
.prj-table .form-control,
.prj-table .form-select {
  max-width: 234px;
}
.app-foo {
  display: flex;
  justify-content: space-between;
  margin-top: 40px;
}
.link {
  color: #1976d2;
  font-size: 14px;
  text-transform: uppercase;
  font-weight: 500;
  text-decoration: none;
}
.btn {
  font-size: 16px;
  font-weight: 500;
  border-radius: 36px;
  padding: 5px 18px;
}

.back-link {
  color: #1976d2;
  letter-spacing: 1.25px;
  font-weight: 500;
  font-size: 14px;
  line-height: 36px;
}

@media (max-width: 640px) {
  .table > :not(caption) > * > [data-v-933e9cdf] {
    padding: 10px;
  }
}

@media screen and (min-width: 768px) {
  .project-details table {
    max-width: 575px;
  }
}
</style>
