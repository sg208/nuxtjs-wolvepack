<template>
  <b-tab :title="tabdetail.text.heading">
    <RandomAlert v-if="$fetchState.pending" :errortext="`Fetching ${tabdetail.text.plural}`" />
    <RandomAlert v-else-if="$fetchState.error" :errortext="`An error occurred, while fetching ${tabdetail.text.plural} data :(`" />
    <div v-else>
      <b-row class="mini-gap">
        <b-col sm="12" md="9">
          <h1 class="display-4">
            List of {{ tabdetail.text.heading }}
          </h1>
        </b-col>
        <b-col class="d-flex align-items-center" sm="12" md="3">
          <b-button
            v-b-modal="`modal-addnew-${tabdetail.id}`"
            block
            variant="success"
            size="lg"
          >
            + Add new {{ tabdetail.text.singular }}
          </b-button>
        </b-col>
        <ModalAddNewRecord
          :content="{
            id: `modal-addnew-${tabdetail.id}`,
            title: tabdetail.text.singular,
            endpoint: {
              url: endpoint,
              headers: endpointOptions.headers
            }
          }"
        />
      </b-row>

      <RandomAlert v-if="showDeletedRecordMessage" :errortext="`Record has been deleted, thx :)`" />

      <RandomAlert v-if="!tabcontent.length" :errortext="`There are no ${tabdetail.text.plural} data available at this time. Maybe you should add a new one :)`" />
      <ul v-else class="list">
        <li v-for="item in tabcontent" :key="item.id">
          <b-row>
            <b-col sm="12" md="10">
              <h2 class="h5">
                <b-badge variant="secondary">
                  {{ item.id }}
                </b-badge> {{ item.name }}
              </h2>
              <div v-if="item.gender" class="padding-bottom-1">
                Gender: {{ item.gender }} | Birthday: {{ item.birthday }}
              </div>
              <div v-else class="padding-bottom-1">
                Lat: {{ item.lat }}, Lng: {{ item.lng }}
              </div>
            </b-col>

            <b-col sm="12" md="2" class="d-flex align-items-center">
              <b-button-group style="width: 100%">
                <b-button v-b-modal="`modal-update-${tabdetail.id}-${item.id}`" variant="outline-success" :aria-label="`Update ${item.name}`">
                  Update
                </b-button>
                <b-button variant="outline-danger" :aria-label="`Remove ${item.name}`" @click="removeRecord(item.id, 'delete')">
                  Remove
                </b-button>
              </b-button-group>

              <ModalUpdateExistingRecord
                :content="{
                  id: `modal-update-${tabdetail.id}-${item.id}`,
                  title: tabdetail.text.singular,
                  name: item.name,
                  endpoint: {
                    url: endpoint,
                    headers: endpointOptions.headers
                  }
                }"
              />
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
<<<<<<< HEAD
      endpoint: `https://join.wolfpackit.nl/api/v1/${this.tabdetail.text.plural}`,
      // endpoint: `http://localhost:3000/${this.tabdetail.text.plural}`,
=======
      // endpoint: `https://join.wolfpackit.nl/api/v1/${this.tabdetail.text.plural}`,
      endpoint: `http://localhost:3000/${this.tabdetail.text.plural}`,
>>>>>>> 68f304f8a7dd057732f3e24faf2ed75aa5a78fb1
      endpointOptions: {
        method: 'GET',
        headers: {
          'content-type': 'application/json',
          Authorization: 'Bearer 9bAqXRPplyiGfF6n81NVUGpAqeLI1QHw46aqICVL1BLaGI6'
        }
      },
      tabcontent: [],
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
      this.showDeletedRecordMessage = true
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
.padding-bottom-1 {
  padding-bottom: 1rem;
}
</style>
