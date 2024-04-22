<template>
  <div>
    <div class="grid lg:grid-cols-122 grid-cols-1 gap-5">
      <Card>
        <td class="text-slate-700 dark:text-slate-300 text-base font-normal">
          <div>Calendar Year ({{ this.selectedPpmp.calendar_year }})</div>
          <div class="text-md text-slate-500 dark:text-slate-400 mt-1">Project Title: {{ this.selectedPpmp.project_title }}</div>
          <div class="text-md text-slate-500 dark:text-slate-400 mt-1">
            PMO/End-User/Department: {{ this.selectedPpmp.pmo_end_user_dept }}
          </div>
          <div class="text-md text-slate-500 dark:text-slate-400 mt-1">
            Source of Funds: {{ this.selectedPpmp.source_of_fund }}
          </div>
          <div class="text-md text-slate-500 dark:text-slate-400 mt-1">Received by:</div>
        </td>
      </Card>
      <Card noborder>
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
            text="List New Item"
            btnClass=" btn-dark font-normal btn-sm"
            iconClass="text-lg"
            :link="`/app/list-new-item?id=` + this.$route.query.id"
          />
        </div>

        <vue-good-table
          :columns="columns"
          styleClass=" vgt-table bordered centered"
          :rows="ppmpItems"
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
            <span v-if="props.column.field == 'code'" class="block w-full">
              <span class="text-sm text-slate-600 dark:text-slate-300 capitalize">{{
                props.row.code
              }}</span>
            </span>

            <span
              v-if="props.column.field == 'generaldescription'"
              class="dark:text-slate-300"
            >
              <div class="flex-1 text-start">
                <h4 class="text-sm font-medium text-slate-600 whitespace-nowrap">
                  {{ props.row.general_desc }}
                </h4>
                <div class="text-xs font-normal text-info-500 dark:text-slate-400">
                  <i>Unit: {{ props.row.unit }}</i>
                </div>
                <div class="text-xs font-normal text-warning-500 dark:text-slate-400">
                  <i>Quantity: {{ props.row.quantity }}</i>
                </div>
              </div>
            </span>
            <span v-if="props.column.field == 'quantity'" class="block w-full">
              <span class="text-sm text-slate-600 dark:text-slate-300 capitalize" v-if="props.row.quantity != null">
                {{ props.row.quantity }} {{ props.row.unit }}
              </span>
              <span class="text-sm text-slate-600 dark:text-slate-300 capitalize" v-else>
                N/A
              </span>
            </span>
            <span v-if="props.column.field == 'est_budget'" class="block w-full">
              <span class="text-sm text-slate-600 dark:text-slate-300 capitalize">
                ₱ {{ props.row.est_budget }}
              </span>
            </span>

            <span v-if="props.column.field == 'action'">
              <div class="flex space-x-3 rtl:space-x-reverse">
                <Tooltip placement="top" arrow theme="dark">
                  <template #button>
                    <div class="action-btn" @click="$refs.calendarModal.openModal(); selectCalendar(props.row.jan, props.row.feb, props.row.mar, props.row.apr, props.row.may, props.row.jun, props.row.jul, props.row.aug, props.row.sept, props.row.oct, props.row.nov, props.row.dec)">
                      <Icon icon="heroicons:calendar" />
                    </div>
                  </template>
                  <span> Schedule/Milestone</span>
                </Tooltip>

                <Tooltip placement="top" arrow theme="dark">
                  <template #button>
                    <div class="action-btn">
                      <RouterLink :to="'edit-item?id=' + props.row.id">
                        <Icon icon="heroicons:pencil-square" />
                      </RouterLink>
                    </div>
                  </template>
                  <span>Edit</span>
                </Tooltip>
                <Tooltip placement="top" arrow theme="dark-500">
                  <template #button>
                    <div class="action-btn" @click="$refs.deleteModal.openModal(); selectDelete(props.row.id)">
                      <Icon icon="heroicons:trash" />
                    </div>
                  </template>
                  <span>Remove</span>
                </Tooltip>

                <Modal
                  title="Schedule/Milestone of Activities"
                  label="Schedule"
                  labelClass="btn-outline-success"
                  ref="calendarModal"
                  sizeClass="max-w-5xl"
                >
                  <div class="grid grid-cols-2 gap-4">
                    <div>
                      <p class="mt-2 font-medium text-green-600">1st Quarter</p>
                      <div class="grid grid-cols-3 gap-4">
                        <div>
                          <Textinput
                            label="January"
                            type="text"
                            placeholder=""
                            name="january"
                            v-model="this.month.jan"
                            disabled
                          />
                        </div>
                        <div>
                          <Textinput
                            label="February"
                            type="text"
                            placeholder=""
                            name="february"
                            v-model="this.month.feb"
                            disabled
                          />
                        </div>
                        <div>
                          <Textinput
                            label="March"
                            type="text"
                            placeholder=""
                            name="march"
                            v-model="this.month.mar"
                            disabled
                          />
                        </div>
                      </div>
                    </div>
                    <div>
                      <p class="mt-2 font-medium text-green-600">2nd Quarter</p>
                      <div class="grid grid-cols-3 gap-4">
                        <div>
                          <Textinput
                            label="April"
                            type="text"
                            placeholder=""
                            name="april"
                            v-model="this.month.apr"
                            disabled
                          />
                        </div>
                        <div>
                          <Textinput
                            label="May"
                            type="text"
                            placeholder=""
                            name="may"
                            v-model="this.month.may"
                            disabled
                          />
                        </div>
                        <div>
                          <Textinput
                            label="June"
                            type="text"
                            placeholder=""
                            name="june"
                            v-model="this.month.jun"
                            disabled
                          />
                        </div>
                      </div>
                    </div>
                    <div>
                      <p class="mt-2 font-medium text-green-600">3rd Quarter</p>
                      <div class="grid grid-cols-3 gap-4">
                        <div>
                          <Textinput
                            label="July"
                            type="text"
                            placeholder=""
                            name="july"
                            v-model="this.month.jul"
                            disabled
                          />
                        </div>
                        <div>
                          <Textinput
                            label="August"
                            type="text"
                            placeholder=""
                            name="august"
                            v-model="this.month.aug"
                            disabled
                          />
                        </div>
                        <div>
                          <Textinput
                            label="September"
                            type="text"
                            placeholder=""
                            name="september"
                            v-model="this.month.sept"
                            disabled
                          />
                        </div>
                      </div>
                    </div>
                    <div>
                      <p class="mt-2 font-medium text-green-600">4th Quarter</p>
                      <div class="grid grid-cols-3 gap-4">
                        <div>
                          <Textinput
                            label="October"
                            type="text"
                            placeholder=""
                            name="october"
                            v-model="this.month.oct"
                            disabled
                          />
                        </div>
                        <div>
                          <Textinput
                            label="November"
                            type="text"
                            placeholder=""
                            name="november"
                            v-model="this.month.nov"
                            disabled
                          />
                        </div>
                        <div>
                          <Textinput
                            label="December"
                            type="text"
                            placeholder=""
                            name="december"
                            v-model="this.month.dec"
                            disabled
                          />
                        </div>
                      </div>
                    </div>
                  </div>

                  <template v-slot:footer>
                    <Button
                      text="Close"
                      btnClass="btn-outline-dark "
                      @click="$refs.calendarModal.closeModal()"
                    />
                  </template>
                </Modal>
                <Modal
                  title="Remove Item"
                  label="Item Remove"
                  labelClass="btn-outline-success"
                  ref="deleteModal"
                >
                  <p><b>Are you sure you want to remove item?</b></p>

                  <template v-slot:footer>
                    <Button
                      text="Close"
                      btnClass="btn-outline-dark "
                      @click="$refs.deleteModal.closeModal()"
                    />
                    <Button
                      text="Yes, remove"
                      btnClass="btn-success "
                      @click="$refs.deleteModal.closeModal(); submitDelete()"
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
    MenuItem,
  },

  data() {
    return {
      selectedPpmpItem: {
        id: "",
      },
      ppmpItems: [],
      selectedPpmp: {
        calendar_year: "",
        project_title: "",
        pmo_end_user_dept: "",
        source_of_fund: "",
      },
      current: 1,
      perpage: 10,
      pageRange: 5,
      searchTerm: "",
      month: {
        jan: "",
        feb: "",
        mar: "",
        apr: "",
        may: "",
        jun: "",
        jul: "",
        aug: "",
        sept: "",
        oct: "",
        nov: "",
        dec: "",
      },
// code
// general_desc
// unit
// quantity
// mode_of_procurement
// est_budget
        
// ppmp_id
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
          label: "Code",
          field: "code",
        },
        {
          label: "Request Item",
          field: "general_desc",
        },

        {
          label: "Mode",
          field: "mode_of_procurement",
        },{
          label: "Quantity",
          field: "quantity",
        },
        {
          label: "Est. Budget",
          field: "est_budget",
        },
        {
          label: "Action",
          field: "action",
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

    getPpmp: function() {
      this.useToken();

      axios.get(apiEndPoint + "/api/specificPpmps/" + this.$route.query.id)
        .then((response) => {
          this.selectedPpmp.calendar_year = response.data["calendar_year"];
          this.selectedPpmp.project_title = response.data["project_title"];
          this.selectedPpmp.pmo_end_user_dept = response.data["pmo_end_user_dept"];
          this.selectedPpmp.source_of_fund = response.data["source_of_fund"];
        })
        .catch((error) => {

        });
    },

    getPpmpItems: function() {
      this.useToken();

      axios.get(apiEndPoint + "/api/ppmp_items/" + this.$route.query.id)
        .then((response) => {
          this.ppmpItems = response.data
        })
        .catch((error) => {

        });
    },

    selectCalendar: function(jan, feb, mar, apr,may, jun, jul, aug, sept, oct, nov, dec) {
      this.month.jan = jan;
      this.month.feb = feb;
      this.month.mar = mar;
      this.month.apr = apr;
      this.month.may = may;
      this.month.jun = jun;
      this.month.jul = jul;
      this.month.aug = aug;
      this.month.sept = sept;
      this.month.oct = oct;
      this.month.nov = nov;
      this.month.dec = dec;
    },

    selectDelete: function(id){
      this.selectedPpmpItem.id = id
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

      axios.delete(apiEndPoint + '/api/remove_ppmp_item/' + this.selectedPpmpItem.id)
        .then((response) => {
          // router.push("/app/system-users");
          this.getPpmpItems();
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
    this.getPpmp();
    this.getPpmpItems();
  }
};
</script>
<style lang="scss" scoped></style>
