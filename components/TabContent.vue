<template>
  <b-tab :title="tabdetail.text.heading">
    <RandomAlert
      v-if="$fetchState.pending"
      :content="{ text: `Fetching ${tabdetail.text.plural}`}"
    />
    <RandomAlert
      v-else-if="$fetchState.error"
      :content="{
        text: `An error occurred, while fetching ${tabdetail.text.plural} data :(`
      }"
    />
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
                Gender: {{ item.gender }} | Birthday: {{ item.birthday }}
              </p>
              <!-- Additional detail for Packs -->
              <p v-else>
                Lat: {{ item.lat }}, Lng: {{ item.lng }}
              </p>
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
      showDeletedRecordMessage: false,
      removedRecordName: null
    }
  },
  async fetch () {
    this.tabcontent = await fetch(this.endpoint, this.endpointOptions).then(res => res.json())
  },
  fetchOnServer: false,
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
        .then((clickValue) => {
          // eslint-disable-next-line no-console
          console.log(clickValue)
        })
        .catch((err) => {
          // eslint-disable-next-line no-console
          console.log(err)
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
        .catch((err) => {
          // eslint-disable-next-line no-console
          console.log(err)
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
</style>
