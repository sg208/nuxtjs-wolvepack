<template>
  <b-modal :id="content.id" centered hide-footer :title="`+ Add new ${content.title}`">
    <b-form @submit="onSubmit" @reset="onReset">
      <b-form-group id="input-group-name" label-for="inputName">
        <b-form-input
          id="inputName"
          ref="inputName"
          v-model="form.name"
          aria-label="Name"
          :placeholder="`Enter ${content.title} name`"
          required
          :class="isError('name') ? 'errorField' : ''"
        />
        <p v-if="isError('name')" class="errorText">
          This field can't be empty.
        </p>
      </b-form-group>

      <!-- Wolve related fields only -->
      <template v-if="isWolveRelatedField">
        <b-form-group id="input-group-2" label-for="input-2">
          <b-form-select
            id="input-2"
            v-model="form.gender"
            aria-label="Gender"
            :options="wolveGender"
            required
          />
        </b-form-group>

        <b-form-group id="input-group-3" label-for="input-3">
          <b-form-input
            id="input-3"
            v-model="form.birthday"
            aria-label="Birthday"
            :placeholder="`Enter ${content.title}'s birthday (YYYY-MM-DD)`"
            required
          />
        </b-form-group>
      </template>

      <!-- Pack related fields only -->
      <template v-if="isPackRelatedField">
        <b-form-group id="input-group-4" label-for="input-4">
          <b-form-input
            id="input-4"
            v-model="form.lat"
            aria-label="Latitude"
            placeholder="Enter latitude"
            required
            :class="isError('lat') ? 'errorField' : ''"
          />
          <p v-if="isError('lat')" class="errorText">
            Please enter numeric characters only, full or fractional.
          </p>
        </b-form-group>

        <b-form-group id="input-group-5" label-for="input-5">
          <b-form-input
            id="input-5"
            v-model="form.lng"
            aria-label="Longitude"
            placeholder="Enter longitude"
            required
            :class="isError('lng') ? 'errorField' : ''"
          />
          <p v-if="isError('lng')" class="errorText">
            Please enter numeric characters only, full or fractional.
          </p>
        </b-form-group>
      </template>

      <b-button type="submit" variant="success">
        Add new {{ content.title }}
      </b-button>
      <b-button type="reset" variant="secondary">
        Clear all fields
      </b-button>
    </b-form>
  </b-modal>
</template>

<script>
export default {
  props: {
    content: {
      type: Object,
      default: () => ({
        id: Number,
        title: String,
        endpoint: {
          type: Object,
          default: () => ({
            url: String,
            headers: {
              type: Object,
              default: () => ({
                'Content-Type': String,
                Authorization: String
              })
            }
          })
        }
      })
    }
  },
  data () {
    return {
      form: {
        name: '',
        gender: null,
        birthday: '',
        lat: '',
        lng: ''
      },
      wolveGender: [{ text: 'Select gender', value: null }, 'Female', 'Male'],
      isWolveRelatedField: this.content.title === 'wolve',
      isPackRelatedField: this.content.title === 'pack',
      error: []
    }
  },
  methods: {
    async onSubmit (event) {
      this.clearErrors()

      event.preventDefault()

      const { name, gender, birthday, lat, lng } = this.form
      const filteredFields = this.content.title === 'wolve' ? { name, gender, birthday } : { name, lat, lng }

      // Form validation for lat + lng where both needs to be numbers
      // name field is required - phase 1
      if (typeof name === 'undefined') {
        this.error.push({
          field: 'name',
          message: 'This field is required'
        })
      }

      // lat is required field needs to be a number - phase 1
      if (isNaN(lat)) {
        this.error.push({
          field: 'lat'
        })
      }

      // lng is required field needs to be a number - phase 1
      if (isNaN(lng)) {
        this.error.push({
          field: 'lng'
        })
      }

      if (this.error.length > 0) {
        // Stop addding data when error found
        return
      }

      await fetch(this.content.endpoint.url, {
        method: 'POST',
        headers: {
          ...this.content.endpoint.headers,
          'Access-Control-Allow-Origin': '*',
          'Access-Control-Allow-Headers': 'Origin, X-Requested-With, Content-Type, Accept'
        },
        body: JSON.stringify(filteredFields)
      })
      // TODO Need to add alert to confirm submission has been added with auto dissapear in 5 secs
      await this.$nuxt.refresh()
    },
    onReset (event) {
      this.clearErrors()

      event.preventDefault()

      this.form = {
        name: '',
        gender: null,
        birthday: '',
        lat: '',
        lng: ''
      }
    },
    isError (fieldName) {
      return this.error.find((item) => {
        const { field: formFieldName } = item
        return fieldName === formFieldName
      })
    },
    clearErrors () {
      this.error.length = 0
    }
  }
}
</script>
<style lang="scss" scoped>
.errorText {
  margin-left: 0.8rem;
  font-size: 0.8rem;
  color: red;
}
.form-control {
  &.errorField {
    border-color: red;
  }
}
</style>
