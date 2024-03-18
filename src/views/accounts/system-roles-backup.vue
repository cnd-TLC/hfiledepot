<template>
  <div>
    <Card title="Roles and Permissions">
      <div class="md:flex justify-between pb-6 md:space-y-0 space-y-3 items-center">
        <div class="space-y-5">
          <InputGroup type="text" placeholder="Enter Role Name">
            <template v-slot:append>
              <Button text="Add New Role" btnClass="btn-dark " />
            </template>
          </InputGroup>
          <div class="flex space-x-rb flex-wrap">
            <Switch label="Module 1" active class="mb-5" />
            <Switch label="Module 2" class="mb-5" />
            <Switch label="Module 3" active class="mb-5" />
            <Switch label="Module 4" class="mb-5" />
          </div>
          <div class="flex space-x-rb flex-wrap">
            <Switch label="Module 5" active class="mb-5" />
            <Switch label="Module 6" class="mb-5" />
            <Switch label="Module 7" active class="mb-5" />
            <Switch label="Module 8" class="mb-5" />
          </div>
        </div>
      </div>
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
import Switch from "@/components/Switch";

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
    Switch,
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
