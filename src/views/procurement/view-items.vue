<template>
  <div>
    <Card>
      <div class="grid grid-cols-6 gap-4">
        <div class="col-start-1 col-end-3 ...">
          <td class="text-slate-700 dark:text-slate-300 text-base font-normal">
            <div>Calendar Year (2024)</div>
            <div class="text-md text-slate-500 dark:text-slate-400 mt-1">
              Project Title:
            </div>
            <div class="text-md text-slate-500 dark:text-slate-400 mt-1">
              PMO/End-User/Department:
            </div>
            <div class="text-md text-slate-500 dark:text-slate-400 mt-1">
              Source of Funds:
            </div>
            <div class="text-md text-slate-500 dark:text-slate-400 mt-1">
              Received by:
            </div>
          </td>
        </div>
        <div class="col-end-8 col-span-1 ...">
          <Tooltip placement="top" arrow theme="dark">
            <template #button>
              <div class="action-btn" @click="$refs.modal2.openModal()">
                <Button text="Mark as Received" btnClass="btn-dark btn-sm " />
              </div>
            </template>
            <span>Receive</span>
          </Tooltip>
        </div>
      </div>
    </Card>
    <Card>
      <div class="md:flex justify-between pb-6 md:space-y-0 space-y-3 items-center">
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
                {{ props.row.generaldescription }}
              </h4>
              <div class="text-xs font-normal text-info-500 dark:text-slate-400">
                <i>Unit: {{ props.row.unit }}</i>
              </div>
              <div class="text-xs font-normal text-warning-500 dark:text-slate-400">
                <i>Quantity: {{ props.row.quantity }}</i>
              </div>
            </div>
          </span>

          <span v-if="props.column.field == 'action'">
            <div class="flex space-x-3 rtl:space-x-reverse">
              <Tooltip placement="top" arrow theme="dark">
                <template #button>
                  <div class="action-btn" @click="$refs.modal1.openModal()">
                    <Button text="Schedule/Milestone" btnClass="btn-secondary btn-sm" />
                  </div>
                </template>
                <span> Schedule/Milestone</span>
              </Tooltip>

              <Modal
                title="Schedule/Milestone of Activities"
                label="Schedule"
                labelClass="btn-outline-success"
                ref="modal1"
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
                          v-model="january"
                          :error="emailError"
                        />
                      </div>
                      <div>
                        <Textinput
                          label="February"
                          type="text"
                          placeholder=""
                          name="february"
                          v-model="february"
                          :error="emailError"
                        />
                      </div>
                      <div>
                        <Textinput
                          label="March"
                          type="text"
                          placeholder=""
                          name="march"
                          v-model="march"
                          :error="emailError"
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
                          v-model="april"
                          :error="emailError"
                        />
                      </div>
                      <div>
                        <Textinput
                          label="May"
                          type="text"
                          placeholder=""
                          name="may"
                          v-model="may"
                          :error="emailError"
                        />
                      </div>
                      <div>
                        <Textinput
                          label="June"
                          type="text"
                          placeholder=""
                          name="june"
                          v-model="june"
                          :error="emailError"
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
                          v-model="july"
                          :error="emailError"
                        />
                      </div>
                      <div>
                        <Textinput
                          label="August"
                          type="text"
                          placeholder=""
                          name="august"
                          v-model="august"
                          :error="emailError"
                        />
                      </div>
                      <div>
                        <Textinput
                          label="September"
                          type="text"
                          placeholder=""
                          name="september"
                          v-model="september"
                          :error="emailError"
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
                          v-model="october"
                          :error="emailError"
                        />
                      </div>
                      <div>
                        <Textinput
                          label="November"
                          type="text"
                          placeholder=""
                          name="november"
                          v-model="november"
                          :error="emailError"
                        />
                      </div>
                      <div>
                        <Textinput
                          label="December"
                          type="text"
                          placeholder=""
                          name="december"
                          v-model="december"
                          :error="emailError"
                        />
                      </div>
                    </div>
                  </div>
                </div>

                <template v-slot:footer>
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.modal1.closeModal()"
                  />
                </template>
              </Modal>
              <Modal
                title="Mark as Received"
                label="Received"
                labelClass="btn-outline-success"
                ref="modal2"
              >
                <p><b>Mark this PPMP record as RECEIVED?</b></p>

                <template v-slot:footer>
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.modal2.closeModal()"
                  />
                  <Button
                    text="Yes, mark as received"
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
//import fullname1 from "@/assets/images/all-img/customer_1.png";

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
      selectedPpmp: {
        calendar_year: "",
        project_title: "",
        pmo_end_user_dept: "",
        source_of_fund: "",
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
          field: "generaldescription",
        },

        {
          label: "Mode",
          field: "proc_mode",
        },
        {
          label: "Est. Budget",
          field: "manageitems",
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

      axios.get(apiEndPoint + "/api/ppmps/" + this.$route.query.id)
        .then((response) => {
          this.selectedPpmp.calendar_year = response.data["calendar_year"];
          this.selectedPpmp.project_title = response.data["project_title"];
          this.selectedPpmp.pmo_end_user_dept = response.data["pmo_end_user_dept"];
          this.selectedPpmp.source_of_fund = response.data["source_of_fund"];
        })
        .catch((error) => {

        });
      }
    
  },

  mounted () {
    // this.getPpmp();
  }
};
</script>
<style lang="scss" scoped></style>
