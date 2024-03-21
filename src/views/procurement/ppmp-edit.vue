<template>
  <div>
   <Card title="Create New Project Procurement Management Plan">
      <form @submit.prevent="onSubmit">
        <div class="grid lg:grid-cols-1 md:grid-cols-1 grid-cols-1 gap-5">
          <div class="grid lg:grid-cols-4 grid-cols-1 gap-5">
            <Textinput
              label="Calendar Year"
              v-model="calendar_year"
              type="number"
              placeholder="(ex. CY 2022-2023)"
              :error="calendar_yearError"
            />
            <Textinput
              label="Project Title"
              v-model="project_title"
              type="text"
              placeholder=""
              :error="project_titleError"
            />

            <Textinput
              label="PMO/End User/Department"
              v-model="pmo_end_user_dept"
              type="text"
              placeholder=""
              :error="pmo_end_user_deptError"
            />
            <Textinput
              label="Source of Funds"
              v-model="source_of_fund"
              type="text"
              placeholder=""
              :error="source_of_fundError"
            />
            <!--Select label="Department" :options="options" v-model="selected" />
            <Select label="Assigned Office" :options="options" v-model="selected" /-->
          </div>
        </div>

        <div class="ltr:text-right rtl:text-left space-x-3 rtl:space-x-reverse mt-4">
          <!-- <Button text="Print" btnClass="btn-secondary" /> -->
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
import Modal from "@/components/Modal/Modal";

import { useField, useForm } from "vee-validate";
import * as yup from "yup";

import { useRouter } from "vue-router";
import { useRoute } from "vue-router";
import { useToast } from "vue-toastification";

import axios from 'axios';

import { apiEndPoint } from "../../constant/data";

export default {
  components: {
    Modal,
    Button,
    Card,
    Textinput,
    FromGroup,
    Textarea,
    Select,
  },
  data() {
    return {
      selected: null,
      options: [
        {
          value: "option1",
          label: "Option 1",
        },
        {
          value: "option2",
          label: "Option 2",
        },
        {
          value: "option3",
          label: "Option 3",
        },
      ],
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

    getPpmp: function() {
      this.useToken();

      axios.get(apiEndPoint + "/api/ppmps/" + this.$route.query.id)
        .then((response) => {
          this.calendar_year = response.data["calendar_year"];
          this.project_title = response.data["project_title"];
          this.pmo_end_user_dept = response.data["pmo_end_user_dept"];
          this.source_of_fund = response.data["source_of_fund"];
        })
        .catch((error) => {

        });
      }
  },

  setup() {
    const schema = yup.object({
      calendar_year: yup.string().required("Calendar Year is required"),
      project_title: yup.string().required("Project Title is required"),
      pmo_end_user_dept: yup.string().required("PMP/End User/Department is required"),
      source_of_fund: yup.string().required("Source of Funds is required"),
    });

    const toast = useToast();
    const router = useRouter();
    const route = useRoute();

    const formValues = {
      calendar_year: "",
      project_title: "",
      pmo_end_user_dept: "",
      source_of_fund: "",
    };

    const { handleSubmit } = useForm({
      validationSchema: schema,
      initialValues: formValues,
    });

    const { value: calendar_year, errorMessage: calendar_yearError } = useField("calendar_year");
    const { value: project_title, errorMessage: project_titleError } = useField("project_title");
    const { value: pmo_end_user_dept, errorMessage: pmo_end_user_deptError } = useField("pmo_end_user_dept");
    const { value: source_of_fund, errorMessage: source_of_fundError } = useField("source_of_fund");

    const onSubmit = handleSubmit((values) => {
      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.put(apiEndPoint + "/api/update_ppmp/" + route.query.id, values)
        .then((response) => {
          // router.push("/app/ppmprecords");
          router.back();
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
      calendar_year,
      project_title,
      pmo_end_user_dept,
      source_of_fund,
      calendar_yearError,
      project_titleError,
      pmo_end_user_deptError,
      source_of_fundError,
      onSubmit
    }
  },

  mounted () {
    this.getPpmp();
  }
};
</script>
<style lang=""></style>
