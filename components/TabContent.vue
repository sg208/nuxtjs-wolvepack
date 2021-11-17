<template>
  <b-tab :title="tabdetail.text.heading">
    <RandomAlert v-if="$fetchState.pending" :errortext="`Fetching ${tabdetail.text.plural}`" />
    <RandomAlert v-else-if="$fetchState.error" :errortext="`An error occurred, while fetching ${tabdetail.text.plural} data :(`" />
    <div v-else>
      <b-row class="mini-gap">
        <b-col cols="9">
          <h2>List of {{ tabdetail.text.heading }}</h2>
        </b-col>
        <b-col class="text-right">
          <b-button variant="success" @click="showAlert">
            + Add new {{ tabdetail.text.singular }}
          </b-button>
        </b-col>
      </b-row>
      <RandomAlert v-if="show" :errortext="`Thanks for clicking but unfortunately we're still working on this feature, please stay tune :)`" />
      <RandomAlert v-if="showDeletedRecordMessage" :errortext="`Record has been deleted, thx :)`" />
      <RandomAlert v-if="!tabcontent.length" :errortext="`There are no ${tabdetail.text.plural} data available at this time. Maybe you should add a new one :)`" />
      <ul v-else class="list">
        <li v-for="item in tabcontent" :key="item.id">
          <b-row>
            <b-col cols="9">
              <b>{{ item.name }}</b>
              <div v-if="item.gender">
                Gender: {{ item.gender }} | Birthday: {{ item.birthday }} | Wolve ID: {{ item.id }}
              </div>
              <div v-else>
                Pack ID: {{ item.id }} | Lat: {{ item.lat }}, Lng: {{ item.lng }}
              </div>
            </b-col>
            <b-col cols="3" class="text-right">
              <b-button-group>
                <b-button variant="outline-success" :aria-label="`Update ${item.name}`">
                  Update
                </b-button>
                <b-button variant="outline-danger" :aria-label="`Remove ${item.name}`" @click="removeRecord(item.id, 'delete')">
                  Remove
                </b-button>
              </b-button-group>
            </b-col>
          </b-row>
        </li>
      </ul>
    </div>
  </b-tab>
</template>

<script>
export default {
  // eslint-disable-next-line vue/require-prop-types
  props: ['tabdetail'],
  data () {
    return {
      endpoint: `https://join.wolfpackit.nl/api/v1/${this.tabdetail.text.plural}`,
      endpointOptions: {
        method: 'GET',
        headers: {
          'content-type': 'application/json',
          Authorization: 'Bearer 9bAqXRPplyiGfF6n81NVUGpAqeLI1QHw46aqICVL1BLaGI6'
        }
      },
      tabcontent: [],
      show: false,
      showDeletedRecordMessage: false
    }
  },
  async fetch () {
    this.tabcontent = await fetch(this.endpoint, this.endpointOptions).then(res => res.json())
  },
  methods: {
    async removeRecord (id, method = 'get') {
      await fetch(`${this.endpoint}/${id}`, {
        method: method.toUpperCase(),
        headers: this.endpointOptions.headers
      })
      await this.$nuxt.refresh()
      this.show = false
      this.showDeletedRecordMessage = true
    },
    showAlert () {
      this.show = true
      this.showDeletedRecordMessage = false
    }
  }
}
</script>

<style lang="scss" scoped>
.list {
    list-style-type: none;
    padding: 0;
    margin: 0;

    li {
        padding: 1.5rem 0;
        margin: 0;
        border-top: 1px dotted #cccccc;
    }
}
.mini-gap {
    padding: 1rem 0;
}
</style>
