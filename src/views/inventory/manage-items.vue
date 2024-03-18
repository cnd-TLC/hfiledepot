<template>
  <div>
    <div class="md:w-1/2 w-full">
    <Card noborder>
      <div class="md:flex justify-between  md:space-y-0 space-y-3 items-center">
        
            <!-- Your content goes here -->
          </div>
        <h5 class="mb-5">Manage Items</h5>
        <InputGroup
      type="text"
      prepend="Search Items from Inventory"
      placeholder=""
      class="mb-3"
    />
    <div class="space-y-2 mb-5">
        <Select label="Mark Item as:" :options="options" v-model="selected" />
        <div class="text-base">
        </div>
      </div>
      <div class="mb-4">
        <label for="myTextarea" class="block text-sm font-medium text-gray-700">Remarks</label>
        <textarea id="myTextarea" class="resize-y select-all w-full h-15 p-2 border rounded-md focus:outline-none focus:ring focus:border-blue-300"></textarea>
      </div>
      <Button text="Submit" btnClass="btn-info" />
    </Card>
</div>
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
import Select from "@/components/Select";
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
    Select,
  },

  data() {
    return {
      selected: null,
      options: [
        {
          value: "option1",
          label: "Returned",
        },
        {
          value: "option2",
          label: "Serviceable",
        },
        {
          value: "option3",
          label: "Unserviceable",
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
