<template>
  <div>
    <Card title="Add New User Role">
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

          <div class="grid lg:grid-cols-3 grid-cols-1 gap-5">
            <div
              class="lg:col-span-3 col-span-1 text-slate-900 dark:text-slate-300 text-base"
            >
              Module Permissions
            </div>
            
            </div>
            <div class="grid lg:grid-cols-1 md:grid-cols-1 grid-cols-1 gap-5">

              <div class="grid lg:grid-cols-3 grid-cols-1 gap-5">
                <!-- col-span-3 is the division of columns -->
                <div
                  class="lg:col-span-12 col-span-1 text-slate-900 dark:text-slate-300 text-base font-medium"
                >
                  Dashboard
                </div>
    
                <Card>
                  <div class="flex space-x-5 flex-wrap"></div>
                  <label>Purchased Orders</label>
                  <div v-for="(item, i) in dashboardpurchased" :key="i">
                    <Checkbox
                      :label="item.label"
                      name="module1"
                      v-model="dashboardpurchasedoption"
                      :value="item.value"
                    />
                  </div>
                  <!-- {{ dashboardpurchasedoption }} -->
                </Card>
                <Card>
                  <div class="flex space-x-5 flex-wrap"></div>
                  <label>Total Spend on Procurement</label>
                  <div v-for="(item, i) in dashboardtotalspend" :key="i">
                    <Checkbox
                      :label="item.label"
                      name="module1"
                      v-model="dashboardtotalspendoption"
                      :value="item.value"
                    />
                  </div>
                  <!-- {{ dashboardtotalspendoption }} -->
                </Card>
                <div
                  class="lg:col-span-12 col-span-1 text-slate-900 dark:text-slate-300 text-base font-medium"
                >
                  Procurement
                </div>
    
                <Card>
                  <div class="flex space-x-5 flex-wrap"></div>
                  <label>Purchase Request Form</label>
                  <div v-for="(item, i) in procurementrequest" :key="i">
                    <Checkbox
                      :label="item.label"
                      name="module1"
                      v-model="procurementrequestoption"
                      :value="item.value"
                    />
                  </div>
                  
                  <!-- {{ procurementrequestoption }} -->
                </Card>
                <Card>
                  <div class="flex space-x-5 flex-wrap"></div>
                  <label>List of Requests</label>
                  <div v-for="(item, i) in procurementlist" :key="i">
                    <Checkbox
                      :label="item.label"
                      name="module1"
                      v-model="procurementlistrequestoption"
                      :value="item.value"
                    />
                  </div>
                
                  <!-- {{ procurementlistrequestoption }} -->
                </Card>
                <Card>
                  <div class="flex space-x-5 flex-wrap"></div>
                  <label>PPMP</label>
                  <div v-for="(item, i) in ppmpform" :key="i">
                    <Checkbox
                      :label="item.label"
                      name="module1"
                      v-model="ppmpformoption"
                      :value="item.value"
                    />
                  </div>
                
                  <!-- {{ ppmpformoption }} -->
                </Card>
    
    
    
                <div
                class="lg:col-span-12 col-span-1 text-slate-900 dark:text-slate-300 text-base font-medium"
              >
                Inventory
              </div>
    
              <Card>
                <div class="flex space-x-5 flex-wrap"></div>
                <label>Procured Items</label>
                <div v-for="(item, i) in procureditems" :key="i">
                  <Checkbox
                    :label="item.label"
                    name="module1"
                    v-model="procureditemsoption"
                    :value="item.value"
                  />
                </div>
                
                  <!-- {{ procureditemsoption }} -->
              </Card>
              <Card>
                <div class="flex space-x-5 flex-wrap"></div>
                <label>Manage Items</label>
                <div v-for="(item, i) in manageitems" :key="i">
                  <Checkbox
                    :label="item.label"
                    name="module1"
                    v-model="manageitemsoption"
                    :value="item.value"
                  />
                </div>
              
                  <!-- {{ manageitemsoption }} -->
              </Card>
              <Card>
                <div class="flex space-x-5 flex-wrap"></div>
                <label>Items for Inspection</label>
                <div v-for="(item, i) in inspection" :key="i">
                  <Checkbox
                    :label="item.label"
                    name="module1"
                    v-model="inspectionoption"
                    :value="item.value"
                  />
                </div>
              
                  <!-- {{ inspectionoption }} -->
              </Card>
    
              <div
              class="lg:col-span-12 col-span-1 text-slate-900 dark:text-slate-300 text-base font-medium"
            >
              Account Management
            </div>
    
            <Card>
              <div class="flex space-x-5 flex-wrap"></div>
              <label>System Users</label>
              <div v-for="(item, i) in systemusers" :key="i">
                <Checkbox
                  :label="item.label"
                  name="module1"
                  v-model="systemusersoption"
                  :value="item.value"
                />
              </div>
              
                  <!-- {{ systemusersoption }} -->
            </Card>
            <Card>
              <div class="flex space-x-5 flex-wrap"></div>
              <label>Roles and Permissions </label>
              <div v-for="(item, i) in rolespermissions" :key="i">
                <Checkbox
                  :label="item.label"
                  name="module1"
                  v-model="rolespermissionsoption"
                  :value="item.value"
                />
              </div>
            
                  <!-- {{ rolespermissionsoption }} -->
            </Card>
            <!-- <Card>
              <div class="flex space-x-5 flex-wrap"></div>
              <label>System Modules</label>
              <div v-for="(item, i) in systemmodules" :key="i">
                <Checkbox
                  :label="item.label"
                  name="module1"
                  v-model="systemmodulesoption"
                  :value="item.value"
                />
              </div>
            
            </Card> -->
               
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
import Checkbox from "@/components/Checkbox";

import { useField, useForm } from "vee-validate";
import * as yup from "yup";

import { useRouter } from "vue-router";
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
      apiEndPoint,
      ss: "",
      dashboardpurchased: [
        {
          value: "view",
          label: "View",
        },
       
      ],
      dashboardtotalspend: [
        {
          value: "view",
          label: "View",
        },
       
      ],
      procurementrequest: [
        {
          value: "view",
          label: "View",
        },
        {
          value: "create",
          label: "Create",
        },
       
      ],
      procurementlist: [
        {
          value: "view",
          label: "View",
        },
        {
          value: "create",
          label: "Create",
        },
        {
          value: "update",
          label: "Update",
        },
        {
          value: "delete",
          label: "Delete",
        },
      ],
      ppmpform: [
        {
          value: "view",
          label: "View",
        },
        {
          value: "create",
          label: "Create",
        },
        {
          value: "update",
          label: "Update",
        },
        {
          value: "delete",
          label: "Delete",
        },
      ],

      procureditems: [
        {
          value: "view",
          label: "View",
        },
        
      ],
      manageitems: [
        {
          value: "view",
          label: "View",
        },
        {
          value: "create",
          label: "Create",
        },
        {
          value: "update",
          label: "Update",
        },
        {
          value: "delete",
          label: "Delete",
        },
      ],
      inspection: [
        {
          value: "view",
          label: "View",
        },
        {
          value: "create",
          label: "Create",
        },
        {
          value: "update",
          label: "Update",
        },
        {
          value: "delete",
          label: "Delete",
        },
      ],
      systemusers: [
        {
          value: "view",
          label: "View",
        },
        {
          value: "create",
          label: "Create",
        },
        {
          value: "update",
          label: "Update",
        },
        {
          value: "delete",
          label: "Delete",
        },
      ],
      rolespermissions: [
        {
          value: "view",
          label: "View",
        },
        {
          value: "create",
          label: "Create",
        },
        {
          value: "update",
          label: "Update",
        },
        {
          value: "delete",
          label: "Delete",
        },
      ],
      systemmodules: [
        {
          value: "view",
          label: "View",
        },
        {
          value: "create",
          label: "Create",
        },
        {
          value: "update",
          label: "Update",
        },
        {
          value: "delete",
          label: "Delete",
        },
      ],
    };
  },


  setup() {
    const schema = yup.object({
      roleName: yup.string().required("Role Name is required"),
      description: yup.string().required("Description is required"),
    });

    const toast = useToast();
    const router = useRouter();

    const formValues = {
      roleName: "",
      description: "",

      dashboardpurchasedoption: [],
      dashboardtotalspendoption: [],
      procurementrequestoption: [],
      procurementlistrequestoption: [],
      ppmpformoption: [],
      procureditemsoption: [],
      manageitemsoption: [],
      inspectionoption: [],
      systemusersoption: [],
      rolespermissionsoption: [],
      systemmodulesoption: [],
    };

    const { handleSubmit } = useForm({
      validationSchema: schema,
      initialValues: formValues,
    });

    const { value: roleName, errorMessage: roleNameError } = useField("roleName");
    const { value: description, errorMessage: descriptionError } = useField("description");

    const { value: dashboardpurchasedoption } = useField("dashboardpurchasedoption");
    const { value: dashboardtotalspendoption } = useField("dashboardtotalspendoption");
    const { value: procurementrequestoption } = useField("procurementrequestoption");
    const { value: procurementlistrequestoption } = useField("procurementlistrequestoption");
    const { value: ppmpformoption } = useField("ppmpformoption");
    const { value: procureditemsoption } = useField("procureditemsoption");
    const { value: manageitemsoption } = useField("manageitemsoption");
    const { value: inspectionoption } = useField("inspectionoption");
    const { value: systemusersoption } = useField("systemusersoption");
    const { value: rolespermissionsoption } = useField("rolespermissionsoption");
    const { value: systemmodulesoption } = useField("systemmodulesoption");

    const onSubmit = handleSubmit((values) => {
      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.post(apiEndPoint + "/api/add_role", values)
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

      dashboardpurchasedoption,
      dashboardtotalspendoption,
      procurementrequestoption,
      procurementlistrequestoption,
      ppmpformoption,
      procureditemsoption,
      manageitemsoption,
      inspectionoption,
      systemusersoption,
      rolespermissionsoption,
      systemmodulesoption,

      onSubmit
    }
  },
};
</script>
<style lang=""></style>
