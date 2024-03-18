<template>
  <div>
    <Card title="Manage Permissions">
      <form @submit.prevent="onSubmit">
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
                  checked
                  v-if="dashboardpurchasedoption.includes(item.value)"
                />
                <Checkbox
                  :label="item.label"
                  name="module1"
                  v-model="dashboardpurchasedoption"
                  :value="item.value"
                  v-else
                />
              </div>
              <!-- {{ this.dashboardpurchasedoption }} -->
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
                  checked
                  v-if="dashboardtotalspendoption.includes(item.value)"
                />
                <Checkbox
                  :label="item.label"
                  name="module1"
                  v-model="dashboardtotalspendoption"
                  :value="item.value"
                  v-else
                />
              </div>
              <!-- {{ this.dashboardtotalspendoption }} -->
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
                  checked
                  v-if="procurementrequestoption.includes(item.value)"
                />
                <Checkbox
                  :label="item.label"
                  name="module1"
                  v-model="procurementrequestoption"
                  :value="item.value"
                  v-else
                />
              </div>
              
              <!-- {{ this.procurementrequestoption }} -->
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
                  checked
                  v-if="procurementlistrequestoption.includes(item.value)"
                />
                <Checkbox
                  :label="item.label"
                  name="module1"
                  v-model="procurementlistrequestoption"
                  :value="item.value"
                  v-else
                />
              </div>
            
              <!-- {{ this.procurementlistrequestoption }} -->
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
                  checked
                  v-if="ppmpformoption.includes(item.value)"
                />
                <Checkbox
                  :label="item.label"
                  name="module1"
                  v-model="ppmpformoption"
                  :value="item.value"
                  v-else
                />
              </div>
            
              <!-- {{ this.ppmpformoption }} -->
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
                checked
                v-if="procureditemsoption.includes(item.value)"
              />
              <Checkbox
                :label="item.label"
                name="module1"
                v-model="procureditemsoption"
                :value="item.value"
                v-else
              />
            </div>
            
              <!-- {{ this.procureditemsoption }} -->
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
                checked
                v-if="manageitemsoption.includes(item.value)"
              />
              <Checkbox
                :label="item.label"
                name="module1"
                v-model="manageitemsoption"
                :value="item.value"
                v-else
              />
            </div>
          
              <!-- {{ this.manageitemsoption }} -->
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
                checked
                v-if="inspectionoption.includes(item.value)"
              />
              <Checkbox
                :label="item.label"
                name="module1"
                v-model="inspectionoption"
                :value="item.value"
                v-else
              />
            </div>
          
              <!-- {{ this.inspectionoption }} -->
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
              checked
              v-if="systemusersoption.includes(item.value)"
            />
            <Checkbox
              :label="item.label"
              name="module1"
              v-model="systemusersoption"
              :value="item.value"
              v-else
            />
          </div>
          
              <!-- {{ this.systemusersoption }} -->
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
              checked
              v-if="rolespermissionsoption.includes(item.value)"
            />
            <Checkbox
              :label="item.label"
              name="module1"
              v-model="rolespermissionsoption"
              :value="item.value"
              v-else
            />
          </div>
        
              <!-- {{ this.rolespermissionsoption }} -->
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

    checkPermissions: function(mod, create, update, del, view){
      if(mod == 'dashboardpurchasedoption'){
        if(create)
          this.dashboardpurchasedoption.push("create")
        if(update)
          this.dashboardpurchasedoption.push("update")
        if(del)
          this.dashboardpurchasedoption.push("delete")
        if(view)
          this.dashboardpurchasedoption.push("view")
      }

      if(mod == 'dashboardtotalspendoption'){
        if(create)
          this.dashboardtotalspendoption.push("create")
        if(update)
          this.dashboardtotalspendoption.push("update")
        if(del)
          this.dashboardtotalspendoption.push("delete")
        if(view)
          this.dashboardtotalspendoption.push("view")
      }

      if(mod == 'procurementrequestoption'){
        if(create)
          this.procurementrequestoption.push("create")
        if(update)
          this.procurementrequestoption.push("update")
        if(del)
          this.procurementrequestoption.push("delete")
        if(view)
          this.procurementrequestoption.push("view")
      }

      if(mod == 'procurementlistrequestoption'){
        if(create)
          this.procurementlistrequestoption.push("create")
        if(update)
          this.procurementlistrequestoption.push("update")
        if(del)
          this.procurementlistrequestoption.push("delete")
        if(view)
          this.procurementlistrequestoption.push("view")
      }

      if(mod == 'ppmpformoption'){
        if(create)
          this.ppmpformoption.push("create")
        if(update)
          this.ppmpformoption.push("update")
        if(del)
          this.ppmpformoption.push("delete")
        if(view)
          this.ppmpformoption.push("view")
      }

      if(mod == 'procureditemsoption'){
        if(create)
          this.procureditemsoption.push("create")
        if(update)
          this.procureditemsoption.push("update")
        if(del)
          this.procureditemsoption.push("delete")
        if(view)
          this.procureditemsoption.push("view")
      }

      if(mod == 'manageitemsoption'){
        if(create)
          this.manageitemsoption.push("create")
        if(update)
          this.manageitemsoption.push("update")
        if(del)
          this.manageitemsoption.push("delete")
        if(view)
          this.manageitemsoption.push("view")
      }

      if(mod == 'inspectionoption'){
        if(create)
          this.inspectionoption.push("create")
        if(update)
          this.inspectionoption.push("update")
        if(del)
          this.inspectionoption.push("delete")
        if(view)
          this.inspectionoption.push("view")
      }

      if(mod == 'systemusersoption'){
        if(create)
          this.systemusersoption.push("create")
        if(update)
          this.systemusersoption.push("update")
        if(del)
          this.systemusersoption.push("delete")
        if(view)
          this.systemusersoption.push("view")
      }

      if(mod == 'rolespermissionsoption'){
        if(create)
          this.rolespermissionsoption.push("create")
        if(update)
          this.rolespermissionsoption.push("update")
        if(del)
          this.rolespermissionsoption.push("delete")
        if(view)
          this.rolespermissionsoption.push("view")
      }
    },

    getPermission: function() {
      this.useToken();

      axios.get(apiEndPoint + "/api/permissions/" + this.role_id)
        .then((response) => {
          for(var i = 0; i < response.data.length; i++){
            this.checkPermissions(response.data[i].module, response.data[i].create, response.data[i].update, response.data[i].delete, response.data[i].view)
          }
        })
        .catch((error) => {

        });
    }
  },

  setup() {

    const toast = useToast();
    const router = useRouter();
    const route = useRoute();

    const formValues = {
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
      initialValues: formValues,
    });

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

      axios.put(apiEndPoint + '/api/update_permission/' + route.query.id, values)
        .then((response) => {
          router.push("/app/system-roles");
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

  mounted() {
    this.getPermission();
  }
};
</script>
<style lang=""></style>
