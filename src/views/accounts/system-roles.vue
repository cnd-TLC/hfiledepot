<template>
  <div>
    <Card noborder>
      <div class="md:flex justify-between pb-6 md:space-y-0 space-y-3 items-center">
        <h5>User Roles</h5>

        <InputGroup
          v-model="searchTerm"
          placeholder="Search"
          type="text"
          prependIcon="heroicons-outline:search"
          merged
        />
        <Button
          icon="heroicons-outline:plus-sm"
          text="Add Role"
          btnClass=" btn-dark font-normal btn-sm "
          iconClass="text-lg"
          link="system-roles-add"
        />
      </div>

      <vue-good-table
        :columns="columns"
        styleClass=" vgt-table bordered "
        :rows="roles"
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
          <span v-if="props.column.field == 'rolename'" class="flex">
            <span class="text-sm text-slate-600 dark:text-slate-300">{{
              props.row.rolename
            }}</span>
          </span>

          <span v-if="props.column.field == 'Description'">
            <a
              href="#"
              class="text-slate-500 dark:text-slate-300 hover:text-warning-500 hover:underline cursor-pointer"
            >
              {{ props.row.desc }}
            </a>
          </span>

          <!-- <span v-if="props.column.field == 'status'" class="block w-full text-center">
            <span
              class="inline-block px-3 min-w-[90px] mx-auto py-1 rounded-[999px] bg-opacity-25"
              :class="`${
                props.row.status === 'active' ? 'text-success-500 bg-success-500' : ''
              } 
            ${props.row.status === 'suspended' ? 'text-warning-500 bg-warning-500' : ''}
            ${props.row.status === 'inactive' ? 'text-danger-500 bg-danger-500' : ''}
            
             `"
            >
              {{ props.row.status }}
            </span>
          </span> -->
          <span v-if="props.column.field == 'permissions'"> 
            <router-link
              :to="`manage-permissions?id=` + props.row.id"
              class="text-info-500 hover:text-warning-500"
            >
              Manage Permissions
            </router-link>
          </span>

          <span v-if="props.column.field == 'action'">
            <div class="flex space-x-3 rtl:space-x-reverse">
             

              <Tooltip placement="top" arrow theme="dark">
                <template #button>
                  <div class="action-btn">
                    <RouterLink :to="'system-roles-edit?id=' + props.row.id">
                      <Icon icon="heroicons:pencil-square" />
                    </RouterLink>
                  </div>
                </template>
                <span>Edit</span>
              </Tooltip>
              <Tooltip placement="top" arrow theme="danger-500">
                <template #button>
                  <div
                  class="action-btn"
                  @click="
                    $refs.deleteModal.openModal(); selectDelete(props.row.id);
                  "
                >
                  <Icon icon="heroicons:trash" />
                </div>
                </template>
                <span>Delete</span>
              </Tooltip>
              
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
            <Modal
                title="Delete User"  
                label="User Delete"
                labelClass="btn-outline-success"
                ref="deleteModal"
              >
              <p  ><b>Are you sure you want to delete selected user role? </b></p>
                
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
        </template>
      </vue-good-table>
      <!-- {{ this.permissions }} -->
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
import rolename1 from "@/assets/images/all-img/customer_1.png";

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
      selectedRole: {
        id: "",
      },
      roles: [],
      current: 1,
      perpage: 10,
      pageRange: 5,
      searchTerm: "",
      permissions: [],

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
          label: "ID",
          field: "id",
        },
        {
          label: "Role Name",
          field: "role_name",
        },
        {
          label: "Description",
          field: "description",
        },

        {
          label: "Permissions",
          field: "permissions",
        },
        {
          label: "Action",
          field: "action",
        }
      ],
    };
  },

  methods: {

    getRoles: function () {
      const token = JSON.parse(localStorage.jwt);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.get(apiEndPoint + "/api/roles")
        .then((response) => {
          this.roles = response.data;
        })
        .catch((error) => {

        });
    },

    getIndividualPermission: function(){
      const token = JSON.parse(localStorage.jwt);
      const user = JSON.parse(localStorage.activeUser);
      if(token){
        axios.defaults.headers = {
          accept: "application/json",
          Authorization: `Bearer ${token}`
        }  
      }

      axios.get(apiEndPoint + "/api/user_permission/rolespermissionsoption/" + user.role_id)
        .then((response) => {
          this.permissions = response.data;
        })
        .catch((error) => {

        });
    },

    selectDelete: function(id){
      this.selectedRole.id = id
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

      axios.delete(apiEndPoint + '/api/remove_role/' + this.selectedRole.id)
        .then((response) => {
          // router.push("/app/system-users");
          this.getRoles();
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
    this.getRoles();
    this.getIndividualPermission();
  },
};
</script>
<style lang="scss" scoped></style>
