<template>
  <div
    class="main-wrapper w-fit-full vph-full pt20 pb24 pr7 pl7"
    :class="{ dark, light: !dark }"
  >
    <table-list
      :is-loading="loading"
      :columns="columns"
      :config="tableConfig"
      :table-data="tableData"
      @edit-item="handleEditCLS"
      @delete-item="handleDeleteCLS"
      @alert-message="handleSendMessage"
      :row-classes="['state', 'light-yellow-bg', 'false']"
    />
  </div>
</template>

<script>
import { clsColumns } from '../../../config/columns'
import TableList from '../../../components/TableList.vue'
import { getClss, deleteCls } from '../../../services/api/cls'
import { TOGGLE_MESSAGE_DIALOG } from '../../../store/actionTypes'
import { MAP_FOCUS_ON } from '../../../store/actionTypes/map'

export default {
  name: 'CablesSection',
  components: {
    TableList
  },
  data: () => ({
    tableData: [],
    loading: false,
    tableConfig: {
      title: 'CLS',
      creation_link: '/user/section/create?id=cls',
      btn_label: 'Create cls'
    },
    columns: [...clsColumns].filter(col => col.showTable)
  }),
  async mounted() {
    await this.getClssList()
  },
  computed: {
    dark() {
      return this.$store.state.isDark
    }
  },
  methods: {
    handleSendMessage(data) {
      this.$store.commit(`${MAP_FOCUS_ON}`, {
        id: data._id,
        name: data.name,
        type: 'cls'
      })
      return this.$store.commit(`${TOGGLE_MESSAGE_DIALOG}`, true)
    },
    async getClssList() {
      this.loading = true
      const res = await getClss({ user_id: this.$auth.user.sub })
      if (res.t !== 'error' && res.data) {
        this.tableData = res.data.r
      }
      this.loading = false
    },
    handleEditCLS(_id) {
      this.$router.push({
        path: '/user/section/create',
        query: { id: 'cls', item: _id }
      })
    },
    handleDeleteCLS(_id) {
      this.$confirm(
        'Are you sure you want to delete this CLS. This action is irreversible',
        'Please confirm to continue'
      )
        .then(async () => {
          await deleteCls({
            user_id: this.$auth.user.sub,
            _id
          }).then(() => this.getClssList())
        })
        .catch(() => {})
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../../../assets/scss/pages/user/section-styles.scss';
</style>
