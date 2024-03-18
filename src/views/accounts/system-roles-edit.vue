<template>
  <div>
    <Card title="Edit User Role">
      <form @submit.prevent="onSubmit">
        <div class="grid lg:grid-cols-1 md:grid-cols-1 grid-cols-1 gap-5">
          <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
            <div
              class="lg:col-span-3 col-span-1 text-slate-900 dark:text-slate-300 text-base font-medium"
            >
              Role Information
            </div>
            <Textinput
              label="Role Name"
              type="text"
              placeholder="Enter a unique role name"
              v-model="roleName"
              :error="roleNameError"
            />
            <Textinput
              label="Role Description"
              type="text"
              placeholder="Describe the main tasks associated with this role"
              v-model="description"
              :error="descriptionError"
            />
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
import Checkbox from "@/components/Checkbox";

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
    Checkbox,
  },

  data() {
    return {
      role_id: this.$route.query.id,
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

    getRole: function() {
      this.useToken();

      axios.get(apiEndPoint + "/api/roles/" + this.role_id)
        .then((response) => {
          this.roleName = response.data["role_name"]
          this.description = response.data["description"]
          // this.user = response.data
        })
        .catch((error) => {

        });
    }
  },

  setup() {
    const schema = yup.object({
      roleName: yup.string().required("Role Name is required"),
      description: yup.string().required("Description is required"),
    });

    const toast = useToast();
    const router = useRouter();
    const route = useRoute();

    const formValues = {
      roleName: "",
      description: "",
    };

    const { handleSubmit } = useForm({
      validationSchema: schema,
      initialValues: formValues,
    });

    const { value: roleName, errorMessage: roleNameError } = useField("roleName");
    const { value: description, errorMessage: descriptionError } = useField("description");

    const onSubmit = handleSubmit((values) => {
      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.put(apiEndPoint + '/api/update_role/' + route.query.id, values)
        .then((response) => {
          router.push("/app/system-roles");
          toast.success("Data saved.", {
            timeout: 2000,
          });
        })
        .catch((error) => {
          toast.error("Operation failed.", {
            timeout: 2000,
          });
        });
    });

    return {
      roleName,
      roleNameError,
      description,
      descriptionError,
      onSubmit
    }
  },

  mounted() {
    this.getRole();
  }
};
</script>
<style lang=""></style>
 