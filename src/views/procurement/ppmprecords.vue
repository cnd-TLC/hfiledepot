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
        <Button
          icon="heroicons-outline:plus-sm"
          text="Add New PPMP"
          btnClass=" btn-dark font-normal btn-sm"
          iconClass="text-lg"
          link="ppmp"
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
              :to="`ppmp-items?id=` + props.row.id"
              class="text-info-500 hover:text-warning-500"
            >
              Manage Items
            </router-link>
          </span>
          <span v-if="props.column.field == 'status'" class="block w-full">
            <span
              class="inline-block px-3 min-w-[90px] text-center mx-auto py-1 rounded-[999px] bg-opacity-25 text-xs text-warning-500 bg-warning-500"
              v-if="props.row.status == 'pending'"
            >
              Pending
            </span>
            <span
              class="inline-block px-3 min-w-[90px] text-center mx-auto py-1 rounded-[999px] bg-opacity-25 text-xs text-success-500 bg-success-500"
              v-if="props.row.status == 'approved'"
            >
              Approved
            </span>
            <span
              class="inline-block px-3 min-w-[90px] text-center mx-auto py-1 rounded-[999px] bg-opacity-25 text-xs text-danger-500 bg-danger-500"
              v-if="props.row.status == 'rejected'"
            >
              Rejected
            </span>
          </span>
          <span v-if="props.column.field == 'action'">
            <div class=" space-y-1 rtl:space-x-reverse">
              <!--Tooltip placement="top" arrow theme="dark">
                <template #button>
                  <div class="action-btn" @click="$refs.modal1.openModal()">
                    <Icon icon="heroicons:eye" />
                  </div>
                </template>
                <span> View</span>
              </Tooltip-->
              <Button
                text="Update"
                btnClass=" btn-light font-normal btn-sm block-btn"
                :link="'ppmp-edit?id=' + props.row.id"
              />
              <Button
                text="Remove"
                btnClass=" btn-danger font-normal btn-sm block-btn"
                @click="$refs.deleteModal.openModal(); selectDelete(props.row.id)"
              />

              <Modal
                title="View User Details"
                label="User Details"
                labelClass="btn-outline-success"
                ref="modal1"
              >
                <div class="flex flex-row">
                  <div class="w-1/2">
                    <!-- Personal info goes here -->
                    <p><b>Name:</b></p>
                    <p><b>Email Address:</b></p>
                    <p><b>Phone:</b></p>
                    <!-- Other personal info fields -->
                    <div class="border-b border-gray-300 my-4"></div>
                    <p><b>Department:</b></p>
                    <p><b>Assigned Office:</b></p>
                    <div class="border-b border-gray-300 my-4"></div>
                    <p><b>System User Role:</b></p>
                    <p><b>Status:</b></p>
                  </div>
                  <div class="w-1/2">
                    <!-- Values go here -->
                    <p>John Doe</p>
                    <p>john.doe@example.com</p>
                    <p>123-456-7890</p>
                    <!-- Other value fields -->
                    <div class="border-b border-gray-300 my-4"></div>
                    <p>----</p>
                    <p>----</p>
                    <div class="border-b border-gray-300 my-4"></div>
                    <p>----</p>
                    <p>----</p>
                  </div>
                </div>

                <template v-slot:footer>
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.modal1.closeModal()"
                  />
                </template>
              </Modal>
              <Modal
                title="Delete User"
                label="User Delete"
                labelClass="btn-outline-success"
                ref="deleteModal"
              >
                <p><b>Are you sure you want to delete this PPMP record?</b></p>

                <template v-slot:footer>
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.deleteModal.closeModal()"
                  />
                  <Button
                    text="Yes, delete"
                    btnClass="btn-success "
                    @click="$refs.deleteModal.closeModal(); submitDelete()"
                  />
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
//import fullname1 from "@/assets/images/all-img/customer_1.png";

import { useToast } from "vue-toastification";

import axios from 'axios';

import { apiEndPoint } from "../../constant/data";

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
      selectedPpmp: {
        id: ""
      },
      current: 1,
      perpage: 10,
      pageRange: 5,
      searchTerm: "",
      ppmp: [],

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
        //   label: "Id",
        //   field: "id",
        // },
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
          label: "Status",
          field: "status",
        },
        {
          label: "Action",
          field: "action",
        },
      ],
    };
  },

  methods: {
    selectDelete: function(id){
      this.selectedPpmp.id = id
    },

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
        })
        .catch((error) => {

        });
    },

    submitDelete: function() {
      const toast = useToast();

      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.delete(apiEndPoint + '/api/remove_ppmp/' + this.selectedPpmp.id)
        .then((response) => {
          // router.push("/app/system-users");
          this.getPpmp();
          toast.success(" Data removed.", {
            timeout: 2000,
          });
        })
        .catch((error) => {
          toast.error(" Operation failed.", {
            timeout: 2000,
          });
        });
    }
  },

  mounted () {
    this.getPpmp();
  }
};
</script>
<style lang="scss" scoped></style>
