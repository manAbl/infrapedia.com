<template>
  <div
    class="el-card is-hover-shadow cursor-pointer mb4"
    :class="cardData.popular ? 'popular' : ''"
  >
    <div class="card-inner-wrapper flex row wrap p4">
      <h2 class="m0 inline-block text-bold truncate info-wrapper">
        <span
          v-if="cardData.popular"
          class="text-bold mr2 popular-tag p1 vertical-align fs-small"
          :class="{ dark }"
        >
          Popular
        </span>
        {{ cardData.name }}
      </h2>
      <div class="align-self-end">
        <p class="m0 mb2 light-tag">Expires in {{ cardData.expireTime }}</p>
        <p class="light-tag">
          <fa :icon="['fas', 'eye']" />
          {{ cardData.views > 10 ? cardData.views : 'less than 10' }}
        </p>
      </div>
      <div class="mt2 info-wrapper">
        <strong class="mr2">
          <fa :icon="['fas', 'map-marker-alt']" class="icon" />
          <p class="inline-block ml1 m0 light-tag">
            Markets
          </p>
        </strong>
        <el-tag type="info" v-for="(tag, i) in markets" :key="i" class="mr2">
          {{ tag }}
        </el-tag>
        <span type="info" v-if="cardData.markets.length > 1" class="light-tag">
          +1 more ...
        </span>
      </div>
      <div class="mt2 info-wrapper">
        <strong class="mr2 ml-1">
          <fa :icon="['fas', 'wrench']" class="icon" />
          <p class="inline-block ml1 m0 light-tag">
            Services
          </p>
        </strong>
        <el-tag type="info" v-for="(tag, i) in services" :key="i" class="mr2">
          {{ tag }}
        </el-tag>
        <span type="info" v-if="cardData.services.length > 1" class="light-tag">
          +1 more ...
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cardData: {
      type: Object,
      required: true
    }
  },
  computed: {
    dark() {
      return this.$store.state.isDark
    },
    markets() {
      const d = [...Array.from(this.cardData.markets)]
      return d.splice(0, 2)
    },
    services() {
      const d = [...Array.from(this.cardData.services)]
      return d.splice(0, 2)
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../../assets/scss/components/marketplace/card-view-styles.scss';
</style>
