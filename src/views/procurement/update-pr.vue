<template>
  <div>
    <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
      <Card>
        <div class="grid lg:grid-cols-1 md:grid-cols-1 grid-cols-1 gap-5">
          <h6 class="mb-4 text-center">PURCHASE REQUEST</h6>
          <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
            <Textinput
              label="LGU"
              name="lgu_name"
              type="text"
              placeholder="Sorsogon City"
            />
            <Textinput label="Fund" name="fund" type="text" placeholder="" />
          </div>
          <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
            <Select label="Department" :options="options" v-model="selected" />
            <Select label="Section" :options="options" v-model="selected" />
          </div>
          <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
            <Textinput label="PR Number" name="pr_num" type="text" placeholder="" />
            <Textinput label="FPP" name="fpp" type="text" placeholder="" />
          </div>
          <div class="grid lg:grid-cols-1 grid-cols-1 gap-5">
            <Textinput
              label="Purpose of Request"
              name="purpose"
              type="text"
              placeholder=""
            />
          </div>
        </div>

        <div class="space-x-3 rtl:space-x-reverse mt-4">
          <Button text="Update PR Details" btnClass="btn-success" link="ppmp-items" />
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
