<template>
  <div>
    <Card noborder>
      <div class="md:flex justify-between pb-6 md:space-y-0 space-y-3 items-center">
        <h5>List of Modules</h5>

        <InputGroup
          v-model="searchTerm"
          placeholder="Search"
          type="text"
          prependIcon="heroicons-outline:search"
          merged
        />
        <!--Button
          icon="heroicons-outline:plus-sm"
          text="Add Role"
          btnClass=" btn-dark font-normal btn-sm "
          iconClass="text-lg"
          link="system-roles-add"
        /-->
      </div>

      <!--vue-good-table
        :columns="columns"
        styleClass=" vgt-table bordered "
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
          <span v-if="props.column.field == 'rolename'" class="flex">
            <span class="text-sm text-slate-600 dark:text-slate-300">{{
              props.row.rolename
            }}</span>
          </span>

          <span v-if="props.column.field == 'Description'">
            <a
              href="#"
              class="text-slate-500 dark:text-slate-300 hover:text-warning-500 hover:underline cursor-pointer"
            >
              {{ props.row.desc }}
            </a>
          </span>

          <span v-if="props.column.field == 'status'" class="block w-full text-center">
            <span
              class="inline-block px-3 min-w-[90px] mx-auto py-1 rounded-[999px] bg-opacity-25"
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
          <span v-if="props.column.field == 'permissions'">
            <a href="#" class="text-info-500 hover:text-warning-500">{{
              props.row.permissions
            }}</a>
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
      </vue-good-table-->
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
import rolename1 from "@/assets/images/all-img/customer_1.png";

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
          rolename: "Admin",
          desc:
            "The top authority with access to all privileges and control over the system.",
          permissions: "Manage Permissions",

          status: "inactive",
        },
        {
          id: 2,
          rolename: "Department",
          desc:
            " The organizational pillars that manage and specialize in various functions, collaborating to keep the wheels of the institution turning smoothly",
          permissions: "Manage Permissions",
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
          label: "Role Name",
          field: "rolename",
        },
        {
          label: "Description",
          field: "desc",
        },

        {
          label: "Status",
          field: "status",
        },

        {
          label: "Permissions",
          field: "permissions",
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
