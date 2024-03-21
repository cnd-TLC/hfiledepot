<template>
  <div>
    <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
      <Card>
        <div class="grid lg:grid-cols-1 md:grid-cols-1 grid-cols-1 gap-5">
          <h6 class="mb-4 text-center">PURCHASE REQUEST ITEMS</h6>
          <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
            <Textinput label="Item Number" name="item_num" type="text" placeholder="" />
            <Textinput label="Unit" name="unit" type="text" placeholder="" />
          </div>
          <div class="grid lg:grid-cols-1 grid-cols-1 gap-5">
            <Textinput
              label="Item Description"
              name="item_desc"
              type="text"
              placeholder=""
            />
          </div>
          <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
            <Textinput label="Quantity" name="pr_num" type="text" placeholder="" />
            <Textinput label="Unit Cost" name="fpp" type="text" placeholder="" />
          </div>
          <div class="grid lg:grid-cols-3 grid-cols-1 gap-5">
            <Textinput
              label="Total Cost"
              name="total_cost"
              type="text"
              placeholder=""
              readonly
            />
          </div>
        </div>

        <div class="space-x-3 rtl:space-x-reverse mt-4">
          <Button
            text="Add Item to Request"
            btnClass="btn-success"
            link="request-items-for-pr"
          />
        </div>
      </Card>
    </div>
  </div>
</template>
<script>
import Button from "@/components/Button";
import Card from "@/components/Card";
import FromGroup from "@/components/FromGroup";
import Textarea from "@/components/Textarea";
import Textinput from "@/components/Textinput";
import Select from "@/components/Select";
import Modal from "@/components/Modal/Modal";
export default {
  components: {
    Modal,
    Button,
    Card,
    Textinput,
    FromGroup,
    Textarea,
    Select,
  },
  data() {
    return {
      selected: null,
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
};
</script>
<style lang=""></style>
