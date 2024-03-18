<template>
  <div>
    <div class="grid justify-end mr-60"></div>
    <div class="mt-10 relative z-10">
      <Card title="File upload">
        <DropZoneVue />
        <div class="ltr:text-right rtl:text-left space-x-3 rtl:space-x-reverse mt-4">
          <Button text="Import" btnClass="btn-success" />
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
import DropZoneVue from "@/components/Fileinput/index.vue";
import { useDropzone } from "vue3-dropzone";
import { ref } from "vue";
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
    DropZoneVue,
  },
  data() {
    return {};
  },
};
</script>

<style lang="scss" scoped></style>
