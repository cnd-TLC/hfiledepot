<template>
  <div>
    <div class="mt-10 relative z-10">
      <div class="flex z-[5] items-center relative justify-center md:mx-8 mb-16">
        <div
          class="relative z-[1] items-center item flex flex-start flex-1 last:flex-none group"
          v-for="(item, i) in steps"
          :key="i"
        >
          <div
            :class="`   ${
              stepNumber >= i
                ? 'bg-green-500 text-white ring-green-700 ring-offset-2 dark:ring-offset-green-500 dark:bg-green-700 dark:ring-green-700'
                : 'bg-white ring-orange-500 ring-opacity-70  text-orange-700 dark:text-orange-500 dark:bg-orange-500 dark:ring-orange-500 text-opacity-70'
            }`"
            class="transition duration-150 icon-box md:h-12 md:w-12 h-7 w-7 rounded-full flex flex-col items-center justify-center relative z-[66] ring-1 md:text-lg text-base font-medium"
          >
            <span v-if="stepNumber <= i"> {{ i + 1 }}</span>
            <span v-else class="text-3xl">
              <Icon icon="bx:check-double" />
            </span>
          </div>

          <div
            class="absolute top-1/2 h-[2px] w-full"
            :class="
              stepNumber >= i
                ? 'bg-green-700 dark:bg-green-700'
                : 'bg-[#E0EAFF] dark:bg-green-500'
            "
          ></div>
          <div
            class="absolute top-full text-base md:leading-6 mt-3 transition duration-150 md:opacity-100 opacity-0 group-hover:opacity-100"
            :class="
              stepNumber >= i
                ? ' text-slate-900 dark:text-slate-300'
                : 'text-slate-500 dark:text-slate-300 dark:text-opacity-40'
            "
          >
            <span class="w-max">{{ item.title }}</span>
          </div>
        </div>
      </div>
      <div class="mb-3">
        
      </div>
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
            <div class="md:flex md:items-center mb-6">
              <div class="md:w-1/3">
                <label
                  class="block text-gray-500 font-bold md:text-right mb-1 md:mb-0 pr-4"
                  for="inline-full-name"
                >
                  Notes
                </label>
              </div>
              <div class="md:w-2/3">
                <textarea v-model="message"></textarea>
              </div>
            </div>
            <div class="md:flex md:items-right mb-6">
              <div class="md:w-3/3">
            <Button text="Approve" btnClass="btn-success" />
            <Button text="Deny" btnClass="btn-danger " />
            </div>
            </div>
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
import { computed, ref } from "vue";

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

  setup() {
    let steps = [
      {
        id: 1,
        title: "Treasury",
      },
      {
        id: 2,
        title: "Mayor's Office",
      },
      {
        id: 3,
        title: "Budget",
      },
      {
        id: 4,
        title: "BAC",
      },
      {
        id: 5,
        title: "CGSO",
      },
      {
        id: 6,
        title: "Accounting",
      },
    ];

    let stepNumber = ref(1);

    return {
      steps,
      stepNumber,
    };
  },
};
</script>
<style lang="scss" scoped></style>
