<template>
    <div>
      <Card title="Project Procurement Management Plan">
        <div class="grid lg:grid-cols-1 md:grid-cols-1 grid-cols-1 gap-5">
          <div class="grid lg:grid-cols-4 grid-cols-1 gap-5">
         
            <Textinput
              label="Calendar Year"
              name="calendar_year"
              type="text"
              placeholder="(ex. CY 2022-2023)"
            />
            <Textinput
              label="Project Title"
              name="project_title"
              type="text"
              placeholder=""
            />
  
            <Textinput
              label="PMO/End User/Department"
              name="end_user"
              type="text"
              placeholder=""
            />
            <Textinput
              label="Source of Funds"
              name="source_fund"
              type="text"
              placeholder=""
            />
            <!--Select label="Department" :options="options" v-model="selected" />
            <Select label="Assigned Office" :options="options" v-model="selected" /-->
          </div>
  
          <div class="grid lg:grid-cols-2 grid-cols-1 gap-5">
            <div
              class="lg:col-span-2 col-span-1 text-slate-900 dark:text-slate-300 text-base  "
            >
            
            <Modal
                title="List Items"
                label="+ List New Items"
                labelClass="btn-dark"
                ref="modal1"
                sizeClass="max-w-5xl"
              >
              

                <template v-slot:footer>
                  <Button
                    text="Close"
                    btnClass="btn-outline-dark "
                    @click="$refs.modal1.closeModal()"
                  />
                  <Button
                    text="Submit"
                    btnClass="btn-dark "
                    @click="$refs.modal1.closeModal()"
                  />
                </template>
                <div class="grid grid-cols-3 gap-4">
                  <div class="...">
                    <Textinput
                    label="Code"
                    type="text"
                    placeholder=""
                    name="code"
                    v-model="code"
                    :error="emailError"
                  />
                  </div>
                  <div class="col-span-2 ...">
                    <Textinput
                    label="General Description"
                    type="gen_desc"
                    placeholder=""
                    name="gen_desc"
                    v-model="gen_desc"
                    :error="passwordError" 
                  />
                  </div>
                  <div class="...">
                    <Textinput
                    label="Unit"
                    type="text"
                    placeholder=""
                    name="unit"
                    v-model="unit"
                    :error="emailError"
                  />
                  </div>
                  
                    <div class="...">
                      <Textinput
                      label="Quantity"
                      type="number"
                      placeholder=""
                      name="quantity"
                      v-model="quantity"
                      :error="emailError"
                    />
                    </div> 
                
                    <div class="...">
                      <Textinput
                      label="Custom"
                      type="text"
                      placeholder="(lumpsum)"
                      name="custom"
                      v-model="custom"
                      :error="emailError"
                    />
                    </div> 
                    <div class="col-span-2...">
                      <Select label="Mode of Procurement" :options="options" v-model="selected" />
                    </div> 
                    <div class="col-span-2...">
                      <Textinput
                      label="Estimated Budget"
                      type="text"
                      placeholder=""
                      name="budget"
                      v-model="budget"
                      :error="emailError"
                    />
                    </div> 
                  
                </div>
                <div
                class="lg:col-span-2 col-span-1 text-slate-900 dark:text-slate-300 text-base font-medium mt-5 mb-5"
              >
                Schedule/Milestone of Activities
              </div>
              <div class="grid grid-cols-2 gap-4">
                <div>
                  <p class="mt-2 font-medium text-green-600 ">1st Quarter</p>
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
                  <p class="mt-2 font-medium text-green-600" >2nd Quarter</p>
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
              </Modal>
              
    
            </div>
  
          </div>
        </div>
       
  
        <div class="ltr:text-right rtl:text-left space-x-3 rtl:space-x-reverse mt-4">
          <Button text="Print" btnClass="btn-secondary" />
          <Button text="Save" btnClass="btn-success" />
        </div>
      </Card>
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
  