<template>
  <div>
    <Card noborder>
      <div class="md:flex justify-between pb-6 md:space-y-0 space-y-3 items-center">
        <h5>List of Purchase Requests</h5>

        <InputGroup
          v-model="searchTerm"
          placeholder="Search"
          type="text"
          prependIcon="heroicons-outline:search"
          merged
        />
        <Button
          icon="heroicons-outline:plus-sm"
          text="Submit New Purchase Request"
          btnClass=" btn-dark font-normal btn-sm"
          iconClass="text-lg"
          link="submit-pr"
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
          <span v-if="props.column.field == 'pr_num'" class="flex">
            <span class="text-sm text-slate-600 dark:text-slate-300 capitalize">{{
              props.row.pr_num
            }}</span>
          </span>
          <span v-if="props.column.field == 'dept'" class="dark:text-slate-300">
            <div class="flex-1 text-start">
              <h4 class="text-sm font-medium text-slate-600 whitespace-nowrap">
                {{ props.row.dept }}
              </h4>
              <div class="text-xs font-normal text-info-600 dark:text-slate-400">
                <i>Section: {{ props.row.section }}</i>
              </div>
            </div>
          </span>
          <span v-if="props.column.field == 'req-items'" class="space-x-3">
            <Button
              icon="heroicons-outline:queue-list"
              text="Request Items"
              btnClass=" btn-primary font-normal btn-sm"
              iconClass="text-lg"
              link="request-items-for-pr"
            />
          </span>
          <span v-if="props.column.field == 'status'" class="block w-full">
            <span
              class="inline-block px-3 min-w-[90px] text-center mx-auto py-1 rounded-[999px] bg-opacity-25 text-xs"
              :class="`${
                props.row.status === 'Approved by'
                  ? 'text-success-500 bg-success-500'
                  : ''
              } 
          ${props.row.status === 'Rejected' ? 'text-warning-500 bg-warning-500' : ''} 
          
           `"
            >
              {{ props.row.status }}
            </span>
          </span>
          <span v-if="props.column.field == 'action'">
            <div class="flex space-x-3 rtl:space-x-reverse justify-center">
              <Tooltip placement="top" arrow theme="dark">
                <template #button>
                  <RouterLink :to="'update-pr/' + props.row.id">
                    <Icon icon="heroicons:pencil-square" />
                  </RouterLink>
                </template>
                <span> Edit</span>
              </Tooltip>
              <Tooltip placement="top" arrow theme="dark">
                <template #button>
                  <div class="action-btn" @click="$refs.modal2.openModal()">
                    <Icon icon="heroicons:trash" />
                  </div>
                </template>
                <span>Cancel</span>
              </Tooltip>

              <Modal
                title="Cancel Request"
                label="Item Remove"
                labelClass="btn-outline-success"
                ref="modal2"
              >
                <p><b>Are you sure you want to cancel request?</b></p>

                <template v-slot:footer>
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.modal2.closeModal()"
                  />
                  <Button
                    text="Yes, cancel"
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
import fullname1 from "@/assets/images/all-img/customer_1.png";

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
          pr_num: "1",
          fund: "-",
          dept: "City Administrator's Office",
          section: "A",
          fpp: "-",
          status: "Approved by",
        },
        {
          id: 2,
          pr_num: "2",
          fund: "-",
          dept: "City Administrator's Office",
          section: "B",
          fpp: "-",
          status: "Approved by",
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
        {
          label: "PR Number",
          field: "pr_num",
        },
        {
          label: "FPP",
          field: "fpp",
        },

        {
          label: "Fund",
          field: "fund",
        },
        {
          label: "Date Requested",
          field: "fund",
        },
        {
          label: "Department/Section",
          field: "dept",
        },
        {
          label: "ITEM REQUESTS",
          field: "req-items",
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
