<template>
  <div class="main-wrapper overflow-y-scroll">
    <!------------- HERO WRAPPER ------------->
    <div
      class="flex mt20 justify-content-center align-items-center text-center column wrap w-fit-full mt17 h120"
    >
      <div
        class="h-fit-content flex justify-content-center column no-wrap title-wrapper"
      >
        <h1 class="title p4 mb12">
          Something really important here
        </h1>
        <p class="p4">Something else that is important</p>
        <span class="p4">
          A searchbox here
        </span>
      </div>
    </div>
    <!------------- HERO WRAPPER ENDS ------------->
    <el-divider />

    <!------------- NEWS SECTION ------------->
    <div
      class="mb24 news-wrapper flex align-items-center justify-content-center row wrap"
    >
      <el-divider content-position="left">
        <h3>
          Latest news
        </h3>
      </el-divider>
      <div class="flex row  wrap p4">
        <div
          v-for="i in 4"
          class="box w60 h90 mr3 flex align-items-center justify-content-center"
          :key="i"
        >
          {{ i }}
        </div>
      </div>
      <el-button type="primary" plain class="w80 ml40">
        View more
      </el-button>
    </div>
    <!------------- NEWS SECTION ENDS ------------->
    <el-divider />

    <!------------- JOIN SECTION ------------->
    <div
      class="join-section p10 mb20 flex align-items-center column justify-content-center"
    >
      <h2 class="title">
        Join us today. Create a free Infrapedia account.
      </h2>
      <el-button class="mt4 w80" type="primary" @click="joinNow">
        Open an account
      </el-button>
    </div>
    <!------------- JOIN SECTION ENDS ------------->
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
@import '../assets/scss/pages/dashboard-styles.scss';
</style>
