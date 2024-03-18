<template>
  <div
    class="loginwrapper bg-cover bg-no-repeat bg-center"
    :style="{
      backgroundImage:
        'url(' + loginBg + ')',
    }"
  >
    <div class="lg-inner-column">
      <div class="left-columns lg:w-1/2 lg:block hidden">
       
      </div>
      <div class="lg:w-1/2 w-full flex flex-col items-center justify-center">
        <div class="auth-box-3">
          <div class="mobile-logo text-center mb-6 lg:hidden block">
            <router-link to="/"
              ><img :src="logo" alt="" class="mx-auto"
            /></router-link>
          </div>
          <div class="text-center 2xl:mb-10 mb-5">
            <h4 class="font-medium">Sign In</h4>
            <div class="text-slate-500 dark:text-slate-400 text-base">
              Sign in to your account to start using HFILE Depot
            </div>
          </div>
          <Signin />
          <div
            className=" relative border-b-[#9AA2AF] border-opacity-[16%] border-b pt-6"
          >
           
          </div>
          <div class="max-w-[242px] mx-auto mt-8 w-full">
            <Social />
          </div>
          
        </div>
      </div>
      <div class="auth-footer3 text-white py-5 px-5 text-md w-full">
        COPYRIGHT © 2022 HFILE Depot, All rights Reserved
      </div>
    </div>
  </div>
</template>
<script>
import Signin from "../common/Signin";
import Social from "../common/Social";
// Image Import
import loginBg from "@/assets/images/all-img/city.jpg"
import logoWhite from "@/assets/images/logo/logo-white.svg"
import logo from "@/assets/images/logo/logo.svg"

export default {
  data () {
    return {
      logoWhite,logo,loginBg
    }
  },
  components: {
    Social,
    Signin,
  },
};
</script>
<style lang="scss"></style>
