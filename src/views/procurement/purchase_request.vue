<template>
  <div>
    <div class="grid justify-end mr-60">
      <div class="relative">
        <div class="absolute z-0">
          <Button
            icon="heroicons:arrow-up-tray"
            text="Upload an Excel File"
            btnClass=" btn-success font-normal btn-md"
            iconClass="text-md"
            link="upload-excel-file"
          />
        </div>
      </div>
    </div>
    <div class="mt-10 relative z-10">
      <Card noborder>
        <div class="grid grid-cols-4 grid-rows-3 gap-3">
          <div class="col-span-3">
            <p class="text-xl text-slate-800 font-bold">LGU: Sorsogon City</p>
            <p class="text-sm text-slate-800">Department City</p>
            <p class="text-sm text-slate-800 italic">Administrative Office</p>
            <div class="mt-6">
              <vue-good-table
                :columns="columns"
                :rows="basicTableData"
                styleClass=" vgt-table striped bordered"
                :sort-options="{
                  enabled: false,
                }"
              >
                <template v-slot:table-row="props">
                  <span v-if="props.column.field == 'item'">
                    <p class="text-sm text-slate-800 font-bold">{{ props.row.id }}</p>
                    <p class="text-xl text-slate-800">{{ props.row.name }}</p>
                    <p class="text-xs text-slate-800 italic">{{ props.row.desc }}</p>
                  </span>
                  <span v-if="props.column.field == 'quantity'">
                    <p class="text-sm text-slate-800">{{ props.row.quantity }}</p>
                  </span>
                  <span v-if="props.column.field == 'unit_cost'">
                    <p class="text-sm text-slate-800">₱ {{ props.row.unit_cost }}</p>
                  </span>
                  <span v-if="props.column.field == 'total_cost'">
                    <p class="text-sm text-slate-800">₱ {{ props.row.total_cost }}</p>
                  </span>
                </template>
              </vue-good-table>
              <div class="grid justify-center">
                <Button
                  text="Add More"
                  btnClass=" block-btn btn-outline-white font-normal btn-md"
                  iconClass="text-md"
                />
              </div>
            </div>
          </div>

          <div class="col-span-1">
            <div class="md:flex md:items-center mb-6">
              <div class="md:w-1/3">
                <label
                  class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
                  for="inline-full-name"
                >
                  Fund
                </label>
              </div>
              <div class="md:w-2/3">
                <input
                  class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-success-500"
                  id="fund-input"
                  type="text"
                  value=""
                />
              </div>
            </div>
            <div class="md:flex md:items-center mb-6">
              <div class="md:w-1/3">
                <label
                  class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
                  for="inline-full-name"
                >
                  PR No.
                </label>
              </div>
              <div class="md:w-2/3">
                <input
                  class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-success-500"
                  id="fund-input"
                  type="text"
                  value=""
                />
              </div>
            </div>
            <div class="md:flex md:items-center mb-6">
              <div class="md:w-1/3">
                <label
                  class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
                  for="inline-full-name"
                >
                  Section
                </label>
              </div>
              <div class="md:w-2/3">
                <input
                  class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-success-500"
                  id="fund-input"
                  type="text"
                  value=""
                />
              </div>
            </div>
            <div class="md:flex md:items-center mb-6">
              <div class="md:w-1/3">
                <label
                  class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
                  for="inline-full-name"
                >
                  FPP
                </label>
              </div>
              <div class="md:w-2/3">
                <input
                  class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-success-500"
                  id="fund-input"
                  type="text"
                  value=""
                />
              </div>
            </div>
            <div class="md:flex md:items-center mb-6">
              <div class="md:w-1/3">
                <label
                  class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
                  for="inline-full-name"
                >
                  Date
                </label>
              </div>
              <div class="md:w-2/3">
                <input
                  class="bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-2 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-success-500"
                  id="fund-input"
                  type="date"
                  value=""
                />
              </div>
            </div>
            <Button text="Submit Request" btnClass="btn-dark py-2 px-4" />
          </div>
        </div>
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

import { MenuItem } from "@headlessui/vue";
import { basiTableData } from "../../constant/basic-tablle-data";
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
      basicTableData: [
        {
          id: "Item No. 1",
          name: "Item Name One",
          desc: "Lorem ipsum dolor sit amet, consectetur adipiscing elit.",
          quantity: "10",
          unit_cost: "80.00",
          total_cost: "800.00",
        },
        {
          id: "Item No. 2",
          name: "Item Name Two",
          desc:
            "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea.",
          quantity: "15",
          unit_cost: "50.00",
          total_cost: "750.00",
        },
      ],
      columns: [
        {
          label: "ITEM",
          field: "item",
        },
        {
          label: "QUANITY",
          field: "quantity",
        },
        {
          label: "UNIT COST",
          field: "unit_cost",
        },
        {
          label: "TOTAL COST",
          field: "total_cost",
        },
      ],
    };
  },
};
</script>
<style lang="scss" scoped></style>
