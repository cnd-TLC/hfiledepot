<template>
  <div>
    <Card noborder>
      <div class="md:flex justify-between pb-6 md:space-y-0 space-y-3 items-center">
        <h5>Procurement Project Management Plans</h5>

        <InputGroup
          v-model="searchTerm"
          placeholder="Search"
          type="text"
          prependIcon="heroicons-outline:search"
          merged
        />
      </div>
      <vue-good-table
        :columns="columns"
        styleClass=" vgt-table bordered centered"
        :rows="ppmp"
        :pagination-options="{
          enabled: true,
          perPage: perpage,
        }"
        :search-options="{
          enabled: true,
          externalQuery: searchTerm,
        }"
        :select-options="{
          enabled: true,
          selectOnCheckboxOnly: true, // only select when checkbox is clicked instead of the row
          selectioninfoClass: 'custom-class',
          selectionText: 'rows selected',
          clearSelectionText: 'clear',
          disableSelectinfo: true, // disable the select info-500 panel on top
          selectAllByGroup: true, // when used in combination with a grouped table, add a checkbox in the header row to check/uncheck the entire group
        }"
      >
        <template v-slot:table-row="props">
          <span v-if="props.column.field == 'calendaryear'" class="block w-full">
            <span class="text-sm text-slate-600 dark:text-slate-300 capitalize">{{
              props.row.calendaryear
            }}</span>
          </span>

          <span
            v-if="props.column.field == 'projectTitle'"
            class="text-slate-500 dark:text-slate-300 block w-full"
          >
            {{ props.row.projectTitle }}
          </span>

          <span v-if="props.column.field == 'fundsource'" class="block w-full">
            {{ props.row.fundsource }}
          </span>
          <span v-if="props.column.field == 'manageitems'">
            <router-link
              :to="`view-items?id=` + props.row.id"
              class="text-info-500 hover:text-warning-500"
            >
              Check Items
            </router-link>
          </span>
          <span v-if="props.column.field == 'action'">
            <div class="flex space-x-3 rtl:space-x-reverse">
              

              
              <Button 
                text="Mark as Received" 
                btnClass="btn-dark font-normal btn-sm " 
                @click="$refs.markReceiveModal.openModal(); setReceiveId(props.row.id)"
              />

              <Modal
                title="Mark as Received"
                label="Received"
                labelClass="btn-outline-success"
                ref="markReceiveModal"
              >
                <p><b>Mark this PPMP record as RECEIVED?</b></p>

                <template v-slot:footer>
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.markReceiveModal.closeModal()"
                  />
                  <form @submit.prevent="submitReceive">
                      <Textinput
                        type="hidden"
                        placeholder=""
                        name="december"
                        disabled
                        v-model="selectedPpmp.ppmp_id"
                      />
                      <Button
                        text="Yes, mark as received"
                        btnClass="btn-success "
                        @click="$refs.markReceiveModal.closeModal(); markReceived()"
                      />
                    </form>
                </template>
              </Modal>
            </div>
          </span>
        </template>
        <template #pagination-bottom="props">
          <div class="py-4 px-3">
            <Pagination
              :total="50"
              :current="current"
              :per-page="perpage"
              :pageRange="pageRange"
              @page-changed="current = $event"
              :pageChanged="props.pageChanged"
              :perPageChanged="props.perPageChanged"
              enableSearch
              enableSelect
              :options="options"
            >
              >
            </Pagination>
          </div>
        </template>
      </vue-good-table>
    </Card>
  </div>
</template>
<script>
import Dropdown from "@/components/Dropdown";
import Card from "@/components/Card";
import Icon from "@/components/Icon";
import Tooltip from "@/components/Tooltip";
import InputGroup from "@/components/InputGroup";
import Pagination from "@/components/Pagination";
import Button from "@/components/Button";
import Modal from "@/components/Modal/Modal";
import Textinput from "@/components/Textinput";

import { MenuItem } from "@headlessui/vue";
import { advancedTable } from "../../constant/basic-tablle-data";

import { useField, useForm } from "vee-validate";
import * as yup from "yup";

import { useRoute } from "vue-router";
import { useToast } from "vue-toastification";

import axios from 'axios';

import { apiEndPoint } from "../../constant/data";

import { ref } from 'vue';

export default {
  components: {
    Pagination,
    InputGroup,
    Dropdown,
    Icon,
    Tooltip,
    Card,
    Modal,
    Button,
    Textinput,
    MenuItem,
  },

  data() {
    return {
      email: "",
      password: "",
      emailError: "",
      passwordError: "",
      advancedTable: [
        {
          id: 1,
          calendaryear: "2023",
          projectTitle: "Maintenance and Other Operating Expenses (MOOE)",
          endUser: "City Information and Communications Technology (CICTO)",
          fundsource: "General Fund",
          manageitems: "Check Items",
        },
      ],
      current: 1,
      perpage: 10,
      pageRange: 5,
      searchTerm: "",
      ppmp: [],
      selectedPpmp: {
        ppmp_id: ""
      },

      options: [
        {
          value: "1",
          label: "1",
        },
        {
          value: "3",
          label: "3",
        },
        {
          value: "5",
          label: "5",
        },
      ],
      columns: [
        // {
        //  label: "Id",
        //    field: "id",
        //  },
        {
          label: "Calendar Year",
          field: "calendar_year",
        },
        {
          label: "Project Title",
          field: "project_title",
        },

        {
          label: "PMO/End-User/Department",
          field: "pmo_end_user_dept",
        },

        {
          label: "Source of Fund",
          field: "source_of_fund",
        },
        {
          label: "Items",
          field: "manageitems",
        },
        {
          label: "Action",
          field: "action",
        },
      ],
    };
  },

  methods: {
    getPpmp: function () {
      const user = JSON.parse(localStorage.activeUser);
      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.get(apiEndPoint + "/api/ppmps/" + user.department_name)
        .then((response) => {
          this.ppmp = response.data;
          // console.log(this.ppmp)
        })
        .catch((error) => {

        });
    },

    setReceiveId: function (id) {
      this.selectedPpmp.ppmp_id = String(id)
    },

    markReceived: function () {
      const token = JSON.parse(localStorage.jwt);
      const toast = useToast();
      
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.post(apiEndPoint + "/api/ppmp_approve", this.selectedPpmp)
        .then((response) => {
          // router.push("/app/ppmprecords");
          // router.back();
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
    }
  },

  mounted() {
    this.getPpmp();
  }
};
</script>
<style lang="scss" scoped></style>
