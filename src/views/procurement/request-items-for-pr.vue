<template>
  <div>
    <div class="grid lg:grid-cols-122 grid-cols-1 gap-5">
      <Card>
        <div class="md:flex justify-between pb-6 md:space-y-0 space-y-3 items-center">
          <InputGroup
            v-model="searchTerm"
            placeholder="Search"
            type="text"
            prependIcon="heroicons-outline:search"
            merged
          />
          <Button
            icon="heroicons-outline:plus-sm"
            text="Add Item"
            btnClass=" btn-dark font-normal btn-sm"
            iconClass="text-lg"
            @click="$refs.modaladd.openModal()"
          />
          <Modal
            title="Request Item from PPMP"
            label="Request Item from PPMP"
            labelClass="btn-outline-success"
            ref="modaladd"
          >
            <Select label="Item" :options="this.approved_ppmp_items_selection" v-model="this.selected.id" />
            <!-- <Textinput
              label="Quantity"
              name="quantity"
              type="number"
              placeholder="Enter Quantity"
            /> -->
            <template v-slot:footer>
              <Button
                text="Close"
                btnClass="btn-outline-dark "
                @click="$refs.modaladd.closeModal()"
              />
              <Button
                text="Add Item to Purchase Request"
                btnClass="btn-success"
                @click="$refs.modaladd.closeModal(); this.submitItem()"
              />
            </template>
          </Modal>
        </div>

        <vue-good-table
          :columns="columns"
          styleClass=" vgt-table bordered centered"
          :rows="pr_items"
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
            <span v-if="props.column.field == 'item_num'" class="block w-full">
              <span class="text-sm text-slate-600 dark:text-slate-300 capitalize">{{
                props.row.item_num
              }}</span>
            </span>
            <span v-if="props.column.field == 'item_unit'" class="block w-full">
              <span class="text-sm text-slate-600 dark:text-slate-300 capitalize">{{
                props.row.item_unit
              }}</span>
            </span>

            <span v-if="props.column.field == 'item_desc'" class="dark:text-slate-300">
              <div class="flex-1 text-start">
                <h4 class="text-sm font-medium text-slate-600 whitespace-nowrap">
                  {{ props.row.item_desc }}
                </h4>
              </div>
            </span>
            <span v-if="props.column.field == 'unit_cost'" class="dark:text-slate-300">
              <div class="flex-1 text-start">
                <h4 class="text-sm font-medium text-slate-600 whitespace-nowrap">
                  {{ props.row.unit_cost }}
                </h4>
              </div>
            </span>
            <span v-if="props.column.field == 'total_cost'" class="dark:text-slate-300">
              <div class="flex-1 text-start">
                <h4 class="text-sm font-medium text-slate-600 whitespace-nowrap">
                  {{ (props.row.unit_cost * props.row.qty).toFixed(2) }}
                </h4>
              </div>
            </span>

            <span v-if="props.column.field == 'action'">
              <div class="flex space-x-3 rtl:space-x-reverse">
                <Tooltip placement="top" arrow theme="dark-500">
                  <template #button>
                    <div class="action-btn" @click="$refs.removeItemModal.openModal(); selectDelete(props.row.item_id)">
                      <Icon icon="heroicons:trash" />
                    </div>
                  </template>
                  <span>Remove</span>
                </Tooltip>

                <Modal
                  title="Remove Item"
                  label="Item Remove"
                  labelClass="btn-outline-success"
                  ref="removeItemModal"
                >
                  <p><b>Are you sure you want to remove item?</b></p>

                  <template v-slot:footer>
                    <Button
                      text="Close"
                      btnClass="btn-outline-dark "
                      @click="$refs.removeItemModal.closeModal()"
                    />
                    <Button
                      text="Yes, remove"
                      btnClass="btn-success "
                      @click="$refs.removeItemModal.closeModal();  submitDelete()"
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
//import fullname1 from "@/assets/images/all-img/customer_1.png";

import { useRouter } from "vue-router";
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
    Select,
    MenuItem,
  },

  data() {
    return {
      email: "",
      password: "",
      emailError: "",
      passwordError: "",
      pr_no: this.$route.query.id,
      pr_items: [],
      approved_ppmp_items: [],
      approved_ppmp_items_selection: [],
      selectedPrItem: {
        id: ""
      },
      advancedTable: [
        {
          id: 1,
          code: "05-02-04-010",
          generaldescription: "Maintenance and Other Operating Expenses (MOOE)",
          unit: "container",
          quantity: "133",
          proc_mode: "Bidding",
          manageitems: "PHP 4,000.00",
        },
      ],
      current: 1,
      perpage: 10,
      pageRange: 5,
      searchTerm: "",

      columns: [
        {
          label: "Item No.",
          field: "item_id",
        },
        {
          label: "Unit",
          field: "unit",
        },

        {
          label: "Item Description",
          field: "item_description",
        },
        {
          label: "Quantity",
          field: "qty",
        },
        {
          label: "Unit Cost",
          field: "unit_cost",
        },
        {
          label: "Total Cost",
          field: "total_cost",
        },
        {
          label: "Action",
          field: "action",
        },
      ],
      selected: {
        id: "",
        pr_no: this.$route.query.id
      },
      options: [
        {
          value: "option1",
          label: "Option 1",
        },
        {
          value: "option2",
          label: "Option 2",
        },
        {
          value: "option3",
          label: "Option 3",
        },
      ],
    };
  },
  methods: {
    useToken: function () {
      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }
    },

    getItems: function() {
      this.useToken();

      axios.get(apiEndPoint + "/api/pr_items/" + this.pr_no)
        .then((response) => {
          this.pr_items = response.data
        })
        .catch((error) => {

        });
    },

    getApprovedPpmp: function() {
      const user = JSON.parse(localStorage.activeUser);
      this.useToken();

      axios.get(apiEndPoint + "/api/approved_ppmp/" + user.department_name)
        .then((response) => {
          this.approved_ppmp_items = response.data;
          this.approved_ppmp_items.forEach((approved_items) => {
            let setItem = { label: approved_items.code + ' | ' +approved_items.general_desc + ' | ' + approved_items.quantity + ' ' +  approved_items.unit, value: approved_items.id }
            
            this.approved_ppmp_items_selection.push(setItem)
          });
        })
        .catch((error) => {

        });
    },

    selectDelete: function(id){
      this.selectedPrItem.id = id
    },

    submitItem: function() {
      const toast = useToast();
      this.useToken()

      axios.post(apiEndPoint + '/api/submit_pr_items', this.selected)
        .then((response) => {
          // router.push("/app/ppmprecords");
          // router.back();
          // router.push("/app/request-items-for-pr?id=" + response.data.id);
          toast.success("Data saved.", {
            timeout: 2000,
          });
          this.getItems();
        })
        .catch((error) => {
          toast.error("Operation failed.", {
            timeout: 2000,
          });
          console.log(error)
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

      axios.delete(apiEndPoint + '/api/remove_pr_items/' + this.selectedPrItem.id)
        .then((response) => {
          // router.push("/app/system-users");
          this.getItems();
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
    this.getItems()
    this.getApprovedPpmp()
  }
};
</script>
<style lang="scss" scoped></style>
