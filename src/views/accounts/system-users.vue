<template>
  <div>
    <Card noborder>
      <div class="md:flex justify-between pb-6 md:space-y-0 space-y-3 items-center">
        <h5>System Users</h5>

        <InputGroup
          v-model="searchTerm"
          placeholder="Search"
          type="text"
          prependIcon="heroicons-outline:search"
          merged
        />
        <Button
          icon="heroicons-outline:plus-sm"
          text="Add System User"
          btnClass=" btn-dark font-normal btn-sm "
          iconClass="text-lg"
          link="system-users-add"
        />
      </div>

      <vue-good-table
        :columns="columns"
        styleClass=" vgt-table bordered centered"
        :rows="users"
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
          <span v-if="props.column.field == 'fullname'" class="flex">
            <span class="w-7 h-7 rounded-full ltr:mr-3 rtl:ml-3 flex-none">
              <img
                :src="props.row.image"
                :alt="props.row.name"
                class="object-cover w-full h-full rounded-full"
              />
            </span>
            <span class="text-sm text-slate-600 dark:text-slate-300 capitalize">{{
              props.row.name
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
                props.row.status === 'Active' ? 'text-success-500 bg-success-500' : ''
              } 
            ${props.row.status === 'Suspended' ? 'text-warning-500 bg-warning-500' : ''}
            ${props.row.status === 'Inactive' ? 'text-danger-500 bg-danger-500' : ''}
            
             `"
            >
              {{ props.row.status }}
            </span>
          </span>
          <span v-if="props.column.field == 'action'">
            <div class=" space-y-1 rtl:space-x-reverse">
              <Button
                text="View"
                btnClass=" btn-light font-normal btn-sm block-btn"
                @click="
                  $refs.viewModal.openModal(); selectView(props.row.id, props.row.name, props.row.email, props.row.department_name, props.row.role_name, props.row.status);
                "
              />
              <Button
                text="Update"
                btnClass=" btn-light font-normal btn-sm block-btn"
                :link="'system-users-edit?id=' + props.row.id"
              />
              <Button
                text="Remove"
                btnClass="btn-danger font-normal btn-sm block-btn"
                @click="
                    $refs.deleteModal.openModal(); selectDelete(props.row.id);
                  "
              />

              <Modal
                title="View User Details"
                label="User Details"
                labelClass="btn-outline-success"
                ref="viewModal"
              >
              <div class="flex flex-row">
                <div class="w-1/2">
                  <!-- Personal info goes here -->
                  <p><b>Name:</b></p>
                  <p><b>Email Address:</b></p>
                  <!-- Other personal info fields -->
                  <div class="border-b border-gray-300 my-4"></div>
                  <p><b>Department:</b></p>
                  <p><b>System User Role:</b></p> 
                  <p><b>Status:</b></p> 
                </div>
                <div class="w-1/2">
                  <!-- Values go here -->
                  <p>{{ selectedUser.name }}</p>
                  <p>{{ selectedUser.email }}</p>
                  <!-- Other value fields -->
                  <div class="border-b border-gray-300 my-4"></div>
                  <p>{{ selectedUser.department_name }}</p>
                  <p>{{ selectedUser.role_name }}</p>
                  <p>{{ selectedUser.status }}</p>
                </div>
              </div>
                
                <template v-slot:footer>
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.viewModal.closeModal()"
                  />
                  
                </template>
              </Modal>
              <Modal
                title="Delete User"  
                label="User Delete"
                labelClass="btn-outline-success"
                ref="deleteModal"
              >
              <p  ><b>Are you sure you want to delete this user?</b></p>
                <template v-slot:footer>
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.deleteModal.closeModal()"
                  />
                  <Button
                  text="Yes, delete"
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
import fullname1 from "@/assets/images/all-img/customer_1.png";

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
      selectedUser: {
        id: "",
        name: "",
        email: "",
        department_name: "",
        role_name: "",
        status: ""
      },
      users: [],
      current: 1,
      perpage: 10,
      pageRange: 5,
      searchTerm: "",
      permissions: JSON.parse(localStorage.permissions),
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
          field: "name",
        },
        {
          label: "Role",
          field: "role_name",
        },

        {
          label: "Department",
          field: "department_name",
        },

        {
          label: "Status",
          field: "status",
        },

        // {
        //   label: "Status",
        //   field: "status",
        // },
        {
          label: "Action",
          field: "action",
        },
      ],
    };
  },

  methods: {
    showPerms: function() {
      console.log(this.permissions)
    },
    getUsers: function () {
      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.get(apiEndPoint + "/api/users")
        .then((response) => {
          this.users = response.data;
        })
        .catch((error) => {

        });
    },

    selectDelete: function(id){
      this.selectedUser.id = id
    },

    selectView: function(id, name, email, department_name, role_name, status){
      this.selectedUser.id = id
      this.selectedUser.name = name
      this.selectedUser.email = email
      this.selectedUser.department_name = department_name
      this.selectedUser.role_name = role_name
      this.selectedUser.status = status
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

      axios.delete(apiEndPoint + '/api/remove_user/' + this.selectedUser.id)
        .then((response) => {
          // router.push("/app/system-users");
          this.getUsers();
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
    this.getUsers();
    this.showPerms();
  }
};
</script>
<style lang="scss" scoped></style>
