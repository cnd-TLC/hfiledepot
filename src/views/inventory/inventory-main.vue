<template>
  <div>
    <Card noborder>
      <div class="md:flex justify-between pb-6 md:space-y-0 space-y-3 items-center">
        <h5>System Users</h5>

        <InputGroup
          v-model="searchTerm"
          placeholder="Search"
          type="text"
          prependIcon="heroicons-outline:search"
          merged
        />
        <Button
          icon="heroicons-outline:plus-sm"
          text="Add System User"
          btnClass=" btn-dark font-normal btn-sm "
          iconClass="text-lg"
          link="system-users-add"
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
          <span v-if="props.column.field == 'fullname'" class="flex">
            <span class="w-7 h-7 rounded-full ltr:mr-3 rtl:ml-3 flex-none">
              <img
                :src="props.row.image"
                :alt="props.row.fullname"
                class="object-cover w-full h-full rounded-full"
              />
            </span>
            <span class="text-sm text-slate-600 dark:text-slate-300 capitalize">{{
              props.row.fullname
            }}</span>
          </span>

          <span
            v-if="props.column.field == 'role'"
            class="text-slate-500 dark:text-slate-300"
          >
            {{ props.row.role }}
          </span>

          <span v-if="props.column.field == 'status'" class="block w-full">
            <span
              class="inline-block px-3 min-w-[90px] text-center mx-auto py-1 rounded-[999px] bg-opacity-25"
              :class="`${
                props.row.status === 'active' ? 'text-success-500 bg-success-500' : ''
              } 
            ${props.row.status === 'suspended' ? 'text-warning-500 bg-warning-500' : ''}
            ${props.row.status === 'inactive' ? 'text-danger-500 bg-danger-500' : ''}
            
             `"
            >
              {{ props.row.status }}
            </span>
          </span>
          <span v-if="props.column.field == 'action'">
            <div class="flex space-x-3 rtl:space-x-reverse">
              <Tooltip placement="top" arrow theme="dark">
                <template #button>
                  <div
                    class="action-btn"
                    @click="
                      $refs.modal1.openModal();
                      fillFields('Joey@sample', 'JoeyPass');
                    "
                  >
                    <Icon icon="heroicons:eye" />
                  </div>
                </template>
                <span> View</span>
              </Tooltip>

              <Tooltip placement="top" arrow theme="dark">
                <template #button>
                  <div class="action-btn">
                    <Icon icon="heroicons:pencil-square" />
                  </div>
                </template>
                <span> Edit</span>
              </Tooltip>
              <Tooltip placement="top" arrow theme="danger-500">
                <template #button>
                  <div class="action-btn">
                    <Icon icon="heroicons:trash" />
                  </div>
                </template>
                <span>Delete</span>
              </Tooltip>

              <Modal
                title="View User Details"
                label="Login Form"
                labelClass="btn-outline-dark"
                ref="modal1"
              >
                <div class="text-base text-slate-600 dark:text-slate-300">
                  <Textinput
                    label="Email"
                    type="email"
                    placeholder="Type your email"
                    name="emil"
                    v-model="email"
                    :error="emailError"
                  />
                  <Textinput
                    label="Password"
                    type="password"
                    placeholder="8+ characters, 1 capitat letter "
                    name="password"
                    v-model="password"
                    :error="passwordError"
                    hasicon
                  />
                </div>
                <template v-slot:footer>
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.modal1.closeModal()"
                  />
                  <Button text="Save" btnClass="btn-dark " @click="save()" />
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
          fullname: "JOEY LORRAINE APDAL",
          image: fullname1,
          role: "ROLE1",
          dept: "TREASURY",
          permissions: "Permissions",

          status: "inactive",
        },
        {
          id: 2,
          fullname: "CLARK NEILSEN DIZA",
          image: fullname1,
          role: "ROLE2",
          dept: "BAC",

          status: "active",
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
          label: "Id",
          field: "id",
        },
        {
          label: "Full Name",
          field: "fullname",
        },
        {
          label: "Role",
          field: "role",
        },

        {
          label: "Department",
          field: "dept",
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
