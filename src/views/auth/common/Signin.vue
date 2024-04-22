<template>
  <form @submit.prevent="onSubmit" class="space-y-4">
    <Textinput
      label="Username"
      type="text"
      placeholder="Type your username"
      name="username"
      v-model="username"
      :error="usernameError"
      classInput="h-[48px]"
    />
    <Textinput
      label="Password"
      type="password"
      placeholder="8+ characters, 1 capital letter "
      name="password"
      v-model="password"
      :error="passwordError"
      hasicon
      classInput="h-[48px]"
    />

    <div class="flex justify-between">
      <label class="cursor-pointer flex items-start">
        <input
          type="checkbox"
          class="hidden"
          @change="() => (checkbox = !checkbox)"
        />
        <span
          class="h-4 w-4 border rounded flex-none inline-flex mr-3 relative top-1 transition-all duration-150"
          :class="
            checkbox
              ? 'ring-2 ring-black-500 dark:ring-offset-slate-600 dark:ring-slate-900  dark:bg-slate-900 ring-offset-2 bg-slate-900'
              : 'bg-slate-100 dark:bg-slate-600 border-slate-100 dark:border-slate-600 '
          "
        >
          <img
            src="@/assets/images/icon/ck-white.svg"
            alt=""
            class="h-[10px] w-[10px] block m-auto"
            v-if="checkbox"
          />
        </span>
        <span class="text-slate-500 dark:text-slate-400 text-sm leading-6"
          >Keep me signed in</span
        >
      </label>
      <router-link
        to="/forgot-password"
        class="text-sm text-slate-800 dark:text-slate-400 leading-6 font-medium"
        >Forgot Password?</router-link
      >
    </div>

    <button type="submit" class="btn btn-dark block w-full text-center">
      Sign in
    </button>
  </form>
</template>
<script>
import Textinput from "@/components/Textinput";
import { useField, useForm } from "vee-validate";
import * as yup from "yup";

import { useRouter } from "vue-router";
import { useToast } from "vue-toastification";

import axios from 'axios';

import { apiEndPoint } from "../../../constant/data";

export default {
  components: {
    Textinput,
  },
  data() {
    return {
      checkbox: false,
      apiEndPoint,
      user: [],
    };
  },
  methods: {
    checkLoggedIn: function(){
      try{
        this.user = JSON.parse(localStorage.activeUser);
        this.$router.push("/app/home");
      }
      catch(err){
        this.$router.push("/");
      }
    }
  },
  setup() {
    // Define a validation schema
    const schema = yup.object({
      username: yup.string().required("Username is required"),
      password: yup.string().required("Password is required").min(5),
    });

    const toast = useToast();
    const router = useRouter();

    const formValues = {
      username: "",
      password: "",
    };

    const { handleSubmit } = useForm({
      validationSchema: schema,
      initialValues: formValues,
    });
    // No need to define rules for fields

    const { value: username, errorMessage: usernameError } = useField("username");
    const { value: password, errorMessage: passwordError } = useField("password");

    const onSubmit = handleSubmit((values) => {
      axios.get(apiEndPoint + "/sanctum/csrf-cookie").then(async() => {
        axios.post(apiEndPoint + "/api/login", values)
          .then((response) => {
            let activeUser = response.data.user;
            let token = response.data.token;
            // console.log(token)
            localStorage.setItem("activeUser", JSON.stringify(activeUser));
            localStorage.setItem("jwt", JSON.stringify(token));
              if(token){
                axios.defaults.headers = {
                  accept: "application/json",
                  Authorization: `Bearer ${token}`
                }  
              }
              axios.get(apiEndPoint + "/api/user_permissions/"+activeUser.id)
                .then((perm) => {
                  // console.log(response.data[0]);
                  localStorage.setItem("permissions", JSON.stringify(perm.data));
                  router.push("/app/home");
                  toast.success(" Logged in successfully!", {
                    timeout: 2000,
                  });
                })
                .catch((error) => {
                  toast.error(" Operation failed.", {
                    timeout: 2000,
                  });
                });
          })
          .catch((error) => {
            toast.error("Credentials doesn't match any records.", {
              timeout: 2000,
            });
          });
        });
    });

    return {
      username,

      usernameError,
      password,
      passwordError,
      onSubmit,
    };
  },

  mounted() {
    this.checkLoggedIn();
  }
};
</script>
<style lang="scss"></style>
