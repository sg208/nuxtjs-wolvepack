<template>
  <b-tab :title="tabdetail.text.heading" class="padding-bottom-5">
    <!-- Alert when $fetchState.pending is true -->
    <b-alert v-if="$fetchState.pending" role="alert" variant="secondary" class="fetching-container" show>
      <b-icon icon="arrow-clockwise" animation="spin" font-scale="8" />
      <p>We're getting {{ tabdetail.text.plural }} data...</p>
    </b-alert>
    <!-- Alert when $fetchState.error is true -->
    <b-alert v-else-if="$fetchState.error" role="alert" variant="danger" show>
      An error has occurred while retrieving {{ tabdetail.text.plural }} data. <em>({{ $fetchState.error.message }}, error {{ $fetchState.error.statusCode }})</em>
    </b-alert>
    <!-- When data is good -->
    <div v-else>
      <b-row class="mini-gap">
        <b-col sm="12" md="9">
          <h2>
            {{ tabdetail.text.heading }} collection
          </h2>
        </b-col>
        <b-col class="d-flex align-items-center" sm="12" md="3">
          <b-button
            v-b-modal="`modal-addnew-${tabdetail.id}`"
            block
            variant="success"
            size="lg"
            :disabled="tabdetail.id === 'wolve'"
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

      <RandomAlert
        v-if="showDeletedRecordMessage"
        :content="{
          text: `${removedRecordName || tabdetail.text.singular} has been deleted from the ${tabdetail.text.plural} collection, thx :)`,
          dismissible: true
        }"
      />

      <RandomAlert
        v-if="!tabcontent.length"
        :content="{
          text: `There are no ${tabdetail.text.plural} data available at this time. Maybe you should add a new one :)`,
        }"
      />
      <ul v-else class="list">
        <li v-for="item in tabcontent" :key="item.id">
          <b-row>
            <b-col sm="12" md="8">
              <h2 class="h5">
                <b-badge variant="secondary">
                  {{ item.id }}
                </b-badge> {{ item.name }}
              </h2>
              <!-- Additional detail for Wolves -->
              <p v-if="item.gender">
                Gender: {{ item.gender }} | Birthday: {{ item.birthday }} {{ tabcontent.status }}
              </p>
              <!-- Additional detail for Packs -->
              <div v-else>
                <p>
                  Lat: {{ item.lat }} | Lng: {{ item.lng }} <b-button
                    v-b-modal="`modal-googlemapstatic-${item.id}`"
                    variant="outline-secondary"
                    size="sm"
                  >
                    View in map
                  </b-button>
                </p>
                <ModalGoogleMapStatic
                  :content="{
                    id: `modal-googlemapstatic-${item.id}`,
                    title: item.name,
                    coordinate: {
                      center: `${item.lat},${item.lng}`,
                      lat: item.lat,
                      lng: item.lng
                    }
                  }"
                />
              </div>
            </b-col>

            <b-col sm="12" md="4" class="d-flex align-items-center">
              <b-button-group style="width: 100%">
                <b-button variant="outline-success" :aria-label="`Update ${item.name}`" @click="showTempModal(`You can update ${item.name} data soon, just not today!`)">
                  Update
                </b-button>
                <b-button variant="outline-success" :aria-label="`Add to pack ${item.name}`" @click="showTempModal(`You can add ${tabdetail.id === 'wolve' ? `${item.name} to packs` : 'wolve to this pack'}, just not today!`)">
                  Add {{ tabdetail.id === 'wolve' ? 'to packs' : 'wolve' }}
                </b-button>
                <b-button variant="outline-danger" :aria-label="`Remove ${item.name}`" @click="confirmRemoveRecord(item.id, 'delete', item.name, tabdetail.text.plural)">
                  Remove
                </b-button>
              </b-button-group>

              <!-- TODO Need to complete the updating functionality -->
              <!-- <ModalUpdateExistingRecord
                :content="{
                  id: `modal-update-${tabdetail.id}-${item.id}`,
                  title: tabdetail.text.singular,
                  name: item.name,
                  endpoint: {
                    url: endpoint,
                    headers: endpointOptions.headers
                  }
                }"
              /> -->
            </b-col>
          </b-row>
        </li>
      </ul>
    </div>
  </b-tab>
</template>

<script>
export default {
  props: {
    tabdetail: {
      type: Object,
      default: () => ({
        id: String,
        text: {
          type: Object,
          default: () => ({
            heading: String,
            singular: String,
            plural: String
          })
        }
      })
    }
  },
  data () {
    return {
      endpoint: `https://join.wolfpackit.nl/api/v1/${this.tabdetail.text.plural}`,
      endpointOptions: {
        method: 'GET',
        headers: {
          'Content-Type': 'application/json',
          Authorization: `Bearer ${this.$config.apiSecret}`
        }
      },
      tabcontent: [],
      showDeletedRecordMessage: false,
      removedRecordName: null,
      errorStatusCode: null,
      errorMessage: null
    }
  },
  async fetch () {
    this.tabcontent = await fetch(this.endpoint, this.endpointOptions)
      .then((response) => {
        return response.json()
      })
  },
  fetchOnServer: false,
  fetchDelay: 1500,
  methods: {
    async removeRecord (id, method = 'get', name) {
      await fetch(`${this.endpoint}/${id}`, {
        method,
        headers: this.endpointOptions.headers
      })
      await this.$nuxt.refresh()
      this.$nextTick(() => {
        this.removedRecordName = name
        this.showDeletedRecordMessage = true
      })
    },
    showTempModal (modalContent) {
      this.$bvModal.msgBoxOk(modalContent, {
        centered: true,
        buttonSize: 'lg',
        size: 'md',
        okTitle: 'OK, thx'
      })
    },
    confirmRemoveRecord (id, apiMethod, name, collection) {
      this.$bvModal.msgBoxConfirm(`Please confirm you want to delete ${name} from the ${collection} collection?`, {
        title: 'Please confirm',
        size: 'md',
        buttonSize: 'lg',
        okVariant: 'danger',
        okTitle: 'YES',
        cancelTitle: 'Cancel',
        footerClass: 'p-2',
        hideHeaderClose: false,
        centered: true
      })
        .then((clickResult) => {
          if (clickResult) {
            this.removeRecord(id, apiMethod, name)
          }
        })
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
.padding-bottom-5 {
  padding-bottom: 5rem;
}
.fetching-container {
  text-align: center;
  padding: 5rem 0;
}
</style>
