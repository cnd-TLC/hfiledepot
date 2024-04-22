<template>
  <div>
    <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
      <form @submit.prevent="onSubmit">
        <Card>
          <div class="grid lg:grid-cols-1 md:grid-cols-1 grid-cols-1 gap-5">
            <h6 class="mb-4 text-center">PURCHASE REQUEST</h6>

            <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
              <Textinput label="PR Number" name="pr_num" type="text" placeholder="" v-model="pr_no" :error="pr_noError"/>
              <Textinput label="Section" name="section" type="text" placeholder="" v-model="section" :error="sectionError"/>
            </div>

            <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
              <Textinput label="Fund" name="fund" type="text" placeholder="" v-model="fund" :error="fundError"/>
              <Textinput label="FPP" name="fpp" type="text" placeholder="" v-model="fpp" :error="fppError"/>
            </div>
            <div class="grid lg:grid-cols-1 grid-cols-1 gap-5">
              <Textinput
                label="Purpose of Request"
                name="purpose"
                type="text"
                placeholder=""
                v-model="purpose" 
                :error="purposeError"
              />
            </div>

            <!-- Upload Attachments Field -->
            <div class="flex flex-col gap-2">
              <label
                for="attachments"
                class="mb-2 text-sm font-medium text-gray-900 dark:text-gray-300"
                >Upload Attachments</label
              >
              <Fileinput
                name="basic"
                @change="handleFileChange"
                v-model="attachments"
                multiple
              />
              {{ this.attachments }}
            </div>
          </div>

          <div class="space-x-3 rtl:space-x-reverse mt-4">
            <Button
              text="Proceed to Request Items"
              btnClass="btn-success"
              
            />
          </div>
        </Card>
      </form>
    </div>
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

import Fileinput from "@/components/Fileinput";


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
    Fileinput,
    FromGroup,
    Textarea,
    Select,
  },
  data() {
    return {
      attachments: null,
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
    handleFileChange(event) {
      this.attachments = event.target.files;
      console.log(this.attachments);
    },
  },

  setup() {
    const user = JSON.parse(localStorage.activeUser);

    const schema = yup.object({
      pr_no: yup.string().required("PR No is required"),
      fund: yup.string().required("Fund is required"),
      fpp: yup.string().required("FPP is required"),
      purpose: yup.string().required("Purpose is required"),
      section: yup.string().required("Section is required"),
    });

    const toast = useToast();
    const router = useRouter();

    const formValues = {
      pr_no: "",
      fund: "",
      fpp: "",
      purpose: "",
      section: "",
    };

    const { handleSubmit } = useForm({
      validationSchema: schema,
      initialValues: formValues,
    });

    const { value: pr_no, errorMessage: pr_noError } = useField("pr_no");
    const { value: fund, errorMessage: fundError } = useField("fund");
    const { value: fpp, errorMessage: fppError } = useField("fpp");
    const { value: purpose, errorMessage: purposeError } = useField("purpose");
    const { value: section, errorMessage: sectionError } = useField("section");

    const onSubmit = handleSubmit((values) => {
      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.post(apiEndPoint + "/api/submit_purchase_request", values)
        .then((response) => {
          // router.push("/app/ppmprecords");
          // router.back();
          console.log(response.data)
          router.push("/app/request-items-for-pr?id=" + response.data.id);
          toast.success("Data saved.", {
            timeout: 2000,
          });
        })
        .catch((error) => {
          toast.error("Operation failed.", {
            timeout: 2000,
          });
          console.log(error)
        });
    });

    // ppmp-items

    return {
      pr_no,
      section,
      fund,
      fpp,
      purpose,
      pr_noError,
      sectionError,
      fundError,
      fppError,
      purposeError,
      onSubmit
    }
  },
};
</script>
<style lang=""></style>
