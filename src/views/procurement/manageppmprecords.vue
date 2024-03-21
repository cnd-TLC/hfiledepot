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
        :rows="advancedTable"
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
              {{ props.row.manageitems }}
            </router-link>
          </span>
          <span v-if="props.column.field == 'action'">
            <div class="flex space-x-3 rtl:space-x-reverse">
              <!--Tooltip placement="top" arrow theme="dark">
                <template #button>
                  <div class="action-btn" @click="$refs.modal1.openModal()">
                    <Icon icon="heroicons:eye" />
                  </div>
                </template>
                <span> View</span>
              </Tooltip-->

              <Tooltip placement="top" arrow theme="dark">
                <template #button>
                  <div class="action-btn" @click="$refs.modal2.openModal()">
                    <Button text="Mark as Received" btnClass="btn-dark btn-sm " />
                  </div>
                </template>
                <span>Receive</span>
              </Tooltip>

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
                title="Mark as Received"
                label="Received"
                labelClass="btn-outline-success"
                ref="modal2"
              >
                <p><b>Mark this PPMP record as RECEIVED?</b></p>

                <template v-slot:footer>
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.modal2.closeModal()"
                  />
                  <Button
                    text="Yes, mark as received"
                    btnClass="btn-success "
                    @click="$refs.modal2.closeModal()"
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
import { advancedTable } from "../../constant/basic-tablle-data";
//import fullname1 from "@/assets/images/all-img/customer_1.png";

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
          field: "calendaryear",
        },
        {
          label: "Project Title",
          field: "projectTitle",
        },

        {
          label: "PMO/End-User/Department",
          field: "endUser",
        },

        {
          label: "Source of Fund",
          field: "fundsource",
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
    fillFields: function (value1, value2) {
      this.email = value1;
      this.password = value2;
    },

    save: function () {
      this.emailError = "Email doesn't exist";
    },
  },
};
</script>
<style lang="scss" scoped></style>
