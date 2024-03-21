<template>
  <div>
    <Card title="List New Item">
      <form @submit.prevent="onSubmit">
        <div class="grid lg:grid-cols-1 md:grid-cols-1 grid-cols-1 gap-5">
          <div class="grid grid-cols-3 gap-4">
            <div class="...">
              <Textinput
                label="Code"
                type="text"
                placeholder=""
                name="code"
                v-model="code"
                :error="codeError"
              />
            </div>
            <div class="col-span-2 ...">
              <Textinput
                label="General Description"
                type="gen_desc"
                placeholder=""
                name="gen_desc"
                v-model="general_desc"
                :error="general_descError"
              />
            </div>
            <div class="...">
              <Textinput
                label="Unit"
                type="text"
                placeholder=""
                name="unit"
                v-model="unit"
                :error="unitError"
              />
            </div>

            <div class="...">
              <Textinput
                label="Quantity"
                type="number"
                placeholder=""
                name="quantity"
                v-model="quantity"
                :error="quantityError"
              />
            </div>

            <div class="...">
              <Textinput
                label="Custom"
                type="text"
                placeholder="(lumpsum)"
                name="custom"
                
              />
            </div>
            <div class="col-span-2...">
              <Select label="Mode of Procurement" :options="options" v-model="mode_of_procurement" :error="mode_of_procurementError" />
            </div>
            <div class="col-span-2...">
              <Textinput
                label="Estimated Budget"
                type="text"
                placeholder=""
                name="budget"
                v-model="est_budget"
                :error="est_budgetError"
              />
            </div>
          </div>
        </div>
        <div
          class="lg:col-span-2 col-span-1 text-slate-900 dark:text-slate-300 text-base font-medium mt-5 mb-5"
        >
          Schedule/Milestone of Activities
        </div>
        <div class="grid grid-cols-2 gap-4">
          <div>
            <p class="mt-2 font-medium text-green-600">1st Quarter</p>
            <div class="grid grid-cols-3 gap-4">
              <div>
                <Textinput
                  label="January"
                  type="number"
                  placeholder=""
                  name="january"
                  v-model="jan"
                  :error="janError"
                />
              </div>
              <div>
                <Textinput
                  label="February"
                  type="number"
                  placeholder=""
                  name="february"
                  v-model="feb"
                  :error="febError"
                />
              </div>
              <div>
                <Textinput
                  label="March"
                  type="number"
                  placeholder=""
                  name="march"
                  v-model="mar"
                  :error="marError"
                />
              </div>
            </div>
          </div>
          <div>
            <p class="mt-2 font-medium text-green-600">2nd Quarter</p>
            <div class="grid grid-cols-3 gap-4">
              <div>
                <Textinput
                  label="April"
                  type="number"
                  placeholder=""
                  name="april"
                  v-model="apr"
                  :error="aprError"
                />
              </div>
              <div>
                <Textinput
                  label="May"
                  type="number"
                  placeholder=""
                  name="may"
                  v-model="may"
                  :error="mayError"
                />
              </div>
              <div>
                <Textinput
                  label="June"
                  type="number"
                  placeholder=""
                  name="june"
                  v-model="jun"
                  :error="junError"
                />
              </div>
            </div>
          </div>
          <div>
            <p class="mt-2 font-medium text-green-600">3rd Quarter</p>
            <div class="grid grid-cols-3 gap-4">
              <div>
                <Textinput
                  label="July"
                  type="number"
                  placeholder=""
                  name="july"
                  v-model="jul"
                  :error="julError"
                />
              </div>
              <div>
                <Textinput
                  label="August"
                  type="number"
                  placeholder=""
                  name="august"
                  v-model="aug"
                  :error="augError"
                />
              </div>
              <div>
                <Textinput
                  label="September"
                  type="number"
                  placeholder=""
                  name="september"
                  v-model="sept"
                  :error="septError"
                />
              </div>
            </div>
          </div>
          <div>
            <p class="mt-2 font-medium text-green-600">4th Quarter</p>
            <div class="grid grid-cols-3 gap-4">
              <div>
                <Textinput
                  label="October"
                  type="number"
                  placeholder=""
                  name="october"
                  v-model="oct"
                  :error="octError"
                />
              </div>
              <div>
                <Textinput
                  label="November"
                  type="number"
                  placeholder=""
                  name="november"
                  v-model="nov"
                  :error="novError"
                />
              </div>
              <div>
                <Textinput
                  label="December"
                  type="number"
                  placeholder=""
                  name="december"
                  v-model="dec"
                  :error="decError"
                />
              </div>
            </div>
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
          value: "bidding",
          label: "Bidding",
        },
        {
          value: "shopping",
          label: "Shopping",
        },
      ],
    };
  },

  setup() {
    const schema = yup.object({
      code: yup.string().required("Code is required"),
      general_desc: yup.string().required("General Description is required"),
      unit: yup.string().required("Unit is required"),
      quantity: yup.string().required("Quantity is required"),
      mode_of_procurement: yup.string().required("Mode of Procurement is required"),
      est_budget: yup.string().required("Estimated Budget is required"),
    });

    const toast = useToast();
    const router = useRouter();
    const route = useRoute();

    const formValues = {
      code: "",
      general_desc: "",
      unit: "",
      quantity: "",
      mode_of_procurement: "",
      est_budget: "",
      jan: "",
      feb: "",
      mar: "",
      apr: "",
      may: "",
      jun: "",
      jul: "",
      aug: "",
      sept: "",
      oct: "",
      nov: "",
      dec: "",
    };

    const { handleSubmit } = useForm({
      validationSchema: schema,
      initialValues: formValues,
    });

    const { value: code, errorMessage: codeError } = useField("code");
    const { value: general_desc, errorMessage: general_descError } = useField("general_desc");
    const { value: unit, errorMessage: unitError } = useField("unit");
    const { value: quantity, errorMessage: quantityError } = useField("quantity");
    const { value: mode_of_procurement, errorMessage: mode_of_procurementError } = useField("mode_of_procurement");
    const { value: est_budget, errorMessage: est_budgetError } = useField("est_budget");
    const { value: jan, errorMessage: janError } = useField("jan");
    const { value: feb, errorMessage: febError } = useField("feb");
    const { value: mar, errorMessage: marError } = useField("mar");
    const { value: apr, errorMessage: aprError } = useField("apr");
    const { value: may, errorMessage: mayError } = useField("may");
    const { value: jun, errorMessage: junError } = useField("jun");
    const { value: jul, errorMessage: julError } = useField("jul");
    const { value: aug, errorMessage: augError } = useField("aug");
    const { value: sept, errorMessage: septError } = useField("sept");
    const { value: oct, errorMessage: octError } = useField("oct");
    const { value: nov, errorMessage: novError } = useField("nov");
    const { value: dec, errorMessage: decError } = useField("dec");

    const onSubmit = handleSubmit((values) => {
      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.post(apiEndPoint + "/api/add_ppmp_item/" + route.query.id, values)
        .then((response) => {
          // router.push("/app/ppmp-items?id=" + route.query.id);
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
      code,
      general_desc,
      unit,
      quantity,
      mode_of_procurement,
      est_budget,
      jan,
      feb,
      mar,
      apr,
      may,
      jun,
      jul,
      aug,
      sept,
      oct,
      nov,
      dec,
      codeError,
      general_descError,
      unitError,
      quantityError,
      mode_of_procurementError,
      est_budgetError,
      janError,
      febError,
      marError,
      aprError,
      mayError,
      junError,
      julError,
      augError,
      septError,
      octError,
      novError,
      decError,
      onSubmit
    }
  },
};
</script>
<style lang=""></style>
