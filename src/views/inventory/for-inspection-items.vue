<template>
  <div>
    <Card noborder>
      <div class="md:flex justify-between pb-6 md:space-y-0 space-y-3 items-center">
        <h5>For Inspection</h5>
        

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
                props.row.status === 'returned' ? 'text-info-500 bg-info-500' : ''
              } 
            ${props.row.status === 'serviceable' ? 'text-warning-500 bg-warning-500' : ''}
            ${props.row.status === 'unserviceable' ? 'text-danger-500 bg-danger-500' : ''}
            
             `"
            >
              {{ props.row.status }}
            </span>
          </span>
          <span v-if="props.column.field == 'action'">
            <div class="flex space-x-3 rtl:space-x-reverse">
              <Tooltip placement="top" arrow theme="success-500">
                <template #button>
                  <div
                    class="action-btn"
                    @click="
                      $refs.modal1.openModal();
                      fillFields('Joey@sample', 'JoeyPass');
                    "
                  >
                    <Icon icon="heroicons:hand-thumb-up" />
                  </div>
                </template>
                <span> Approve</span>
              </Tooltip>

               
              <Tooltip placement="top" arrow theme="danger-500">
                <template #button>
                  <div class="action-btn">
                    <Icon icon="heroicons:hand-thumb-down" />
                  </div>
                </template>
                <span>Decline</span>
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

          status: "serviceable",
        },
        {
          id: 2,
          fullname: "CLARK NEILSEN DIZA",
          image: fullname1,
          role: "ROLE2",
          dept: "BAC",

          status: "returned",
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
          label: "Item #",
          field: "Item #",
        },
        {
          label: "PR #",
          field: "PR #",
        },
        {
          label: "Unit",
          field: "unit",
        },

        {
          label: "Item Description",
          field: "item-description",
        },

        {
          label: "Quantity",
          field: "quantity",
        },
        {
          label: "Unit Cost",
          field: "unit-cost",
        },
        {
          label: "Total Cost",
          field: "total-cost",
        },
        {
          label: "Department",
          field: "department",
        },
        {
          label: "Status",
          field: "status",
        },
        {
          label: "",
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
