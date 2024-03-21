<template>
  <div>
    <Card title="Update System User">
      <form @submit.prevent="onSubmit">
        <div class="grid lg:grid-cols-1 md:grid-cols-1 grid-cols-1 gap-5">
          <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
            <div
              class="lg:col-span-2 col-span-1 text-slate-900 dark:text-slate-300 text-base font-medium"
            >
              Personal Information
            </div>
            <Textinput
              label="Name"
              type="text"
              placeholder="Name"
              v-model="name"
              :error="nameError"
            />

            <Textinput
              label="Email Address"
              type="email"
              placeholder="example@gmail.com"
              v-model="email"
              :error="emailError"
            />

            <Textinput
              label="Username"
              type="text"
              placeholder="Username"
              v-model="username"
              :error="usernameError"
            />

            
            <!-- <Textinput
              label="Password"
              type="text"
              placeholder="Password"
              v-model="password"
              :error="passwordError"
            /> -->
            <Select label="Department" :options="departments" v-model="selectedDepartment" :error="departmentError" />
          </div>

          <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
            <div
              class="lg:col-span-2 col-span-1 text-slate-900 dark:text-slate-300 text-base font-medium"
            >
              Account
            </div>

            <Select label="System Role" :options="roles" v-model="selectedRole" :error="roleError" />
            <Select label="Status" :options="status" v-model="selectedStatus" :error="statusError" />
            <!-- <select>
              <option v-for="(key, value) in departments" :value="key.id">{{ key.department_name }}</option>
            </select> -->
          </div>
        </div>

        <div class="ltr:text-right rtl:text-left space-x-3 rtl:space-x-reverse mt-4">
          <Button text="Save" btnClass="btn-success" />
        </div>
      </form>
    </Card>
  </div>
</template>
<script>
import Button from "@/components/Button";
import Card from "@/components/Card";
import FromGroup from "@/components/FromGroup";
import Textarea from "@/components/Textarea";
import Textinput from "@/components/Textinput";
import Select from "@/components/Select";

import { useField, useForm } from "vee-validate";
import * as yup from "yup";

import { useRouter } from "vue-router";
import { useRoute } from "vue-router";
import { useToast } from "vue-toastification";

import axios from 'axios';

import { apiEndPoint } from "../../constant/data";

export default {
  components: {
    Button,
    Card,
    Textinput,
    FromGroup,
    Textarea,
    Select,
  },
  data() {
    return {
      status: [
        {
          value: 'Active',
          label: 'Active', 
        },
        {
          value: 'Inactive',
          label: 'Inactive', 
        }
      ],

      roles: [],
      departments: [],
      user_id: this.$route.query.id,
    };
  },

  methods: {
    useToken: function () {
      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }
    },

    getDepartments: function() {
      this.useToken();

      axios.get(apiEndPoint + "/api/departments")
        .then((response) => {
          for (var i = 0; i < response.data.length; i++){
            this.departments.push({
              value: response.data[i].id,
              label: response.data[i].department_name,
            });
          }
        })
        .catch((error) => {

        });
    },

    getRoles: function() {
      this.useToken();

      axios.get(apiEndPoint + "/api/roles")
        .then((response) => {
          for (var i = 0; i < response.data.length; i++){
            this.roles.push({
              value: response.data[i].id,
              label: response.data[i].role_name,
            });
          }
        })
        .catch((error) => {

        });
    },

    getUser: function() {
      this.useToken();

      axios.get(apiEndPoint + "/api/users/" + this.user_id)
        .then((response) => {
          this.name = response.data["name"];
          this.email = response.data["email"];
          this.username = response.data["username"];
          // this.password = response.data["password"];
          this.selectedDepartment = response.data["department_no"];
          this.selectedRole = response.data["role_id"];
          this.selectedStatus = response.data["status"];
          // this.user = response.data
        })
        .catch((error) => {

        });
    }
    
    // onSubmit: function() {
    //   alert('hello')
    // }
  },

  setup() {
    const schema = yup.object({
      name: yup.string().required("Name is required"),
      email: yup.string().required("Email is required"),
      username: yup.string().required("Username is required"),
      // password: yup.string().required("Password is required").min(5),
      selectedDepartment: yup.string().required("Department is required"),
      selectedRole: yup.string().required("Role is required"),
      selectedStatus: yup.string().required("Status is required"),
    });

    const toast = useToast();
    const router = useRouter();
    const route = useRoute();

    const formValues = {
      name: "",
      email: "",
      username: "",
      // password: "",
      selectedDepartment: "",
      selectedRole: "",
      selectedStatus: "",
    };

    const { handleSubmit } = useForm({
      validationSchema: schema,
      initialValues: formValues,
    });

    const { value: name, errorMessage: nameError } = useField("name");
    const { value: email, errorMessage: emailError } = useField("email");
    const { value: username, errorMessage: usernameError } = useField("username");
    // const { value: password, errorMessage: passwordError } = useField("password");
    const { value: selectedDepartment, errorMessage: departmentError } = useField("selectedDepartment");
    const { value: selectedRole, errorMessage: roleError } = useField("selectedRole");
    const { value: selectedStatus, errorMessage: statusError } = useField("selectedStatus");

    const onSubmit = handleSubmit((values) => {
      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.put(apiEndPoint + '/api/update_user/' + route.query.id, values)
        .then((response) => {
          router.back();
          // router.push("/app/system-users");
          toast.success(" Data saved.", {
            timeout: 2000,
          });
        })
        .catch((error) => {
          toast.error(" Operation failed.", {
            timeout: 2000,
          });
        });
    });

    return {
      name,
      nameError,
      email,
      emailError,
      username,
      usernameError,
      // password,
      // passwordError,
      selectedDepartment,
      departmentError,
      selectedRole,
      roleError,
      selectedStatus,
      statusError,
      onSubmit
    }
  },

  mounted () {
    this.getUser();
    this.getDepartments();
    this.getRoles();
  }
};
</script>
<style lang=""></style>
