<template>
  <div>
    <Card noborder>
      <div class="md:flex justify-between pb-6 md:space-y-0 space-y-3 items-center">
        <h5>List of Purchase Requests</h5>

        <InputGroup
          v-model="searchTerm"
          placeholder="Search"
          type="text"
          prependIcon="heroicons-outline:search"
          merged
        />
        <Button
          icon="heroicons-outline:plus-sm"
          text="Submit New Purchase Request"
          btnClass=" btn-dark font-normal btn-sm"
          iconClass="text-lg"
          link="submit-pr"
        />
      </div>

      <vue-good-table
        :columns="columns"
        styleClass=" vgt-table bordered centered align-middle"
        :rows="purchase_requests"
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
          <span v-if="props.column.field == 'pr_num'" class="flex">
            <span class="text-sm text-slate-600 dark:text-slate-300 capitalize">
              {{ props.row.pr_no }}
              <br/>
              {{ props.row.date_of_request }}
            </span>
          </span>
          <span v-if="props.column.field == 'dept'" class="dark:text-slate-300">
            <div class="flex-1 text-start">
              <h4 class="text-sm font-medium text-slate-600 whitespace-nowrap">
                {{ props.row.department_name }}
              </h4>
              <div class="text-xs font-normal text-info-600 dark:text-slate-400">
                <i>Section: {{ props.row.section }}</i>
              </div>
            </div>
          </span>
          <span v-if="props.column.field == 'requested_by'" class="flex">
            {{ props.row.users_requested_by_name }}
          </span>
          <span v-if="props.column.field == 'cash_availability'" class="flex">
            {{ props.row.users_cash_availability_name }}
          </span>
          <span v-if="props.column.field == 'status'" class="block w-full">
            <span
              class="inline-block px-3 min-w-[90px] text-center mx-auto py-1 rounded-[999px] bg-opacity-25 text-xs"
              :class="`${props.row.approved_by != null ? 'text-success-500 bg-success-500' : 'text-warning-500 bg-warning-500'}`"
            >
              <span v-if="props.row.approved_by == null"> Pending </span>
              <span v-else> Approved by: {{ props.row.users_approved_by_name }} </span> 
            </span>
          </span>
          <span v-if="props.column.field == 'action'">
            <div class="space-y-1 rtl:space-x-reverse">
              <Button
                text="Preview"
                btnClass=" btn-light font-normal btn-sm block-btn"
                @click="$refs.printPreview.openModal(); setModalData(props.row.department_name, props.row.pr_no, props.row.date_of_request, props.row.section, props.row.fpp, props.row.fund)"
              />
              <Button
                text="Request Items"
                btnClass=" btn-light font-normal btn-sm block-btn"
                :link="'request-items-for-pr?id=' + props.row.pr_no"
              />
              <Button
                text="Update"
                btnClass=" btn-light font-normal btn-sm block-btn"
                :link="'update-pr?id=' + props.row.pr_no"
              />
              <Button
                text="Remove"
                btnClass=" btn-danger font-normal btn-sm block-btn"
                @click="$refs.deleteModal.openModal(); selectDelete(props.row.pr_no)"
              />
              
              <Modal
                title="Purchase Request"
                label="Print Preview"
                labelClass="btn-outline-success"
                ref="printPreview"
                sizeClass=""
              >
                <div class="p-6 bg-white">
                  <!-- Department and PR No -->
                  <div class="flex justify-between mb-4">
                    <div class="w-1/3">
                      <label class="block text-sm font-medium text-gray-700"
                        >LGU: Sorsogon City</label
                      >
                      <div class="mt-1 p-2 bg-gray-100 rounded">
                        {{ this.selected_purchase_request.dep }}
                      </div>
                    </div>
                    <div class="w-1/3">
                      <label class="block text-sm font-medium text-gray-700"
                        >PR No.:</label
                      >
                      <div class="mt-1 p-2 bg-gray-100 rounded"> {{ this.selected_purchase_request.pr_no }} </div>
                    </div>
                    <div class="w-1/3">
                      <label class="block text-sm font-medium text-gray-700">Date:</label>
                      <div class="mt-1 p-2 bg-gray-100 rounded"> {{ this.selected_purchase_request.date }} </div>
                    </div>
                  </div>
                  <!-- FPP and Fund -->
                  <div class="flex justify-between mb-4">
                    <div class="w-1/3">
                      <label class="block text-sm font-medium text-gray-700"
                        >Section:</label
                      >
                      <div class="mt-1 p-2 bg-gray-100 rounded">{{ this.selected_purchase_request.section }}</div>
                    </div>
                    <div class="w-1/3">
                      <label class="block text-sm font-medium text-gray-700">FPP:</label>
                      <div class="mt-1 p-2 bg-gray-100 rounded">{{ this.selected_purchase_request.fpp }}</div>
                    </div>
                    <div class="w-1/3">
                      <label class="block text-sm font-medium text-gray-700">Fund:</label>
                      <div class="mt-1 p-2 bg-gray-100 rounded">{{ this.selected_purchase_request.fund }}</div>
                    </div>
                  </div>

                  <!-- Itemized Requests Table -->
                  <div class="mt-6 border-t border-gray-200 pt-4">
                    <h3 class="text-lg font-bold">Itemized Requests</h3>
                    <div class="overflow-x-auto mt-2">
                      <table class="min-w-full divide-y divide-gray-200">
                        <thead class="bg-gray-50">
                          <tr>
                            <th
                              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                            >
                              Item No.
                            </th>
                            <th
                              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                            >
                              Item Description
                            </th>
                            <th
                              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                            >
                              Quantity
                            </th>
                            <th
                              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                            >
                              Unit
                            </th>
                            <th
                              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                            >
                              Unit Cost
                            </th>
                            <th
                              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                            >
                              Total Cost
                            </th>
                          </tr>
                        </thead>
                        <tbody class="bg-white divide-y divide-gray-200">
                          <tr v-for="item in this.requested_items">
                            <td class="px-6 py-4 whitespace-nowrap">{{ item.item_id }}</td>
                            <td class="px-6 py-4 whitespace-nowrap">{{ item.item_description }}</td>
                            <td class="px-6 py-4 whitespace-nowrap">{{ item.qty }}</td>
                            <td class="px-6 py-4 whitespace-nowrap">{{ item.unit }}</td>
                            <td class="px-6 py-4 whitespace-nowrap">{{ item.unit_cost }}</td>
                            <td class="px-6 py-4 whitespace-nowrap">{{ item.unit_cost }}</td>
                          </tr>
                        </tbody>
                      </table>
                      <div class="footer-content">
                        <div class="dummy-qr-code mr-6">
                          <img
                            src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d0/QR_code_for_mobile_English_Wikipedia.svg/1200px-QR_code_for_mobile_English_Wikipedia.svg.png"
                            alt="Dummy QR Code"
                            style="
                            max-width: 100px; /* Maximum width */
                            max-height: 100px; /* Maximum height */
                            align
                          "
                          />
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <template v-slot:footer class="print-hide">
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.printPreview.closeModal()"
                  />
                  <Button
                    text="Print"
                    btnClass="btn-success "
                    @click="printModalContent(props.row.department_name, props.row.pr_no, props.row.date_of_request, props.row.section, props.row.fpp, props.row.fund)"
                  />
                </template>
              </Modal>

              <Modal
                title="Cancel Request"
                label="Item Remove"
                labelClass="btn-outline-success"
                ref="deleteModal"
              >
                <p><b>Are you sure you want to cancel request?</b></p>

                <template v-slot:footer>
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.deleteModal.closeModal()"
                  />
                  <Button
                    text="Yes, cancel"
                    btnClass="btn-dark "
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
</template>
<script>
import { PDFDocument, rgb } from 'pdf-lib';
import { saveAs } from 'file-saver';

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

import { useToast } from "vue-toastification";

import axios from 'axios';

import { apiEndPoint } from "../../constant/data";

import pdf from "@/assets/templates/pr.pdf";

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
        id: ""
      },
      email: "",
      password: "",
      emailError: "",
      passwordError: "",
      current: 1,
      perpage: 10,
      pageRange: 5,
      searchTerm: "",
      purchase_requests: [],
      requested_items: [],
      selected_purchase_request: {
        dep: "",
        pr_no: "",
        date: "",
        section: "",
        fpp: "",
        fund: ""
      },

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
          label: "PR Number",
          field: "pr_num",
        },
        {
          label: "Department/Section",
          field: "dept",
        },
        {
          label: "FPP",
          field: "fpp",
        },

        {
          label: "Fund",
          field: "fund",
        },
        {
          label: "Requested By",
          field: "requested_by",
        },
        {
          label: "Cash Availability",
          field: "cash_availability",
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
    async printModalContent(department_name, pr_no, date_of_request, section, fpp, fund) {
      try {
        const response = await axios.get(pdf, { responseType: 'blob' });

        if (response.status !== 200) {
          throw new Error('Failed to fetch PDF');
        }

        const existingPdfBytes = await response.data.arrayBuffer(); 

        const pdfDoc = await PDFDocument.load(existingPdfBytes);
        
        let fields = pdfDoc.getForm().getFields();
        fields = fields.map((f) => f.getName());
        const form = pdfDoc.getForm();
        form.getTextField(fields[0]).setText(department_name);
        form.getTextField(fields[1]).setText(pr_no.toString());
        form.getTextField(fields[2]).setText(section);
        form.getTextField(fields[3]).setText(fpp);
        form.getTextField(fields[4]).setText(date_of_request);
        form.getTextField(fields[5]).setText('Php ' + fund.toString());
        // form.getTextField(fields[6]).setText(cash-avail);
        // form.getTextField(fields[7]).setText(cash-avail-desig);
        // form.getTextField(fields[8]).setText(mayor-name);
        form.getTextField(fields[9]).setText('Lorem Ipsum');

        pdfDoc.getForm().flatten();
        const pdfBytesWithFieldsFilled = await pdfDoc.save();
        let pdfBytes = pdfBytesWithFieldsFilled;

        const blob = new Blob([pdfBytes], { type: 'application/pdf' });
        saveAs(blob, pr_no + '-pr.pdf');

      } catch (error) {
        console.error('Error filling PDF:', error);
      }
    },

    selectDelete: function(id){
      this.selectedPpmp.id = id
      console.log(id)
      console.log(this.selectedPpmp.id)
    },

    setModalData(department_name, pr_no, date_of_request, section, fpp, fund){
      this.selected_purchase_request.dep = department_name
      this.selected_purchase_request.pr_no = pr_no
      this.selected_purchase_request.date = date_of_request
      this.selected_purchase_request.section = section
      this.selected_purchase_request.fpp = fpp
      this.selected_purchase_request.fund = fund

      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.get(apiEndPoint + "/api/pr_items/" + this.selected_purchase_request.pr_no)
        .then((response) => {
          this.requested_items = response.data;
        })
        .catch((error) => {

        });
    },

    getPr: function () {
      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.get(apiEndPoint + "/api/purchase_request")
        .then((response) => {
          this.purchase_requests = response.data;
        })
        .catch((error) => {

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

      axios.delete(apiEndPoint + '/api/remove_purchase_request/' + this.selectedPpmp.id)
        .then((response) => {
          // router.push("/app/system-users");
          this.getPr();
          toast.success(" Data removed.", {
            timeout: 2000,
          });
        })
        .catch((error) => {
          console.log(error.data)
          toast.error(" Operation failed.", {
            timeout: 2000,
          });
        });
    }
  },

  mounted () {
    this.getPr();
  }
};
</script>
<style lang="scss" scoped></style>
