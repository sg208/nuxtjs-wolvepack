<template>
  <b-modal :id="content.id" centered hide-footer :title="`+ Add new ${content.title}`">
    <b-form @submit="onSubmit" @reset="onReset">
      <b-form-group id="input-group-1" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="form.name"
          aria-label="Name"
          :placeholder="`Enter ${content.title} name`"
          required
        />
      </b-form-group>

      <b-form-group v-if="content.title === 'wolve'" id="input-group-2" label-for="input-2">
        <b-form-select
          id="input-2"
          v-model="form.gender"
          aria-label="Gender"
          :options="wolveGender"
          required
        />
      </b-form-group>

      <b-form-group v-if="content.title === 'wolve'" id="input-group-3" label-for="input-3">
        <b-form-input
          id="input-3"
          v-model="form.birthday"
          aria-label="Birthday"
          :placeholder="`Enter ${content.title}'s birthday (YYYY-MM-DD)`"
          required
        />
      </b-form-group>

      <b-form-group v-if="content.title === 'pack'" id="input-group-4" label-for="input-4">
        <b-form-input
          id="input-4"
          v-model="form.lat"
          aria-label="Latitude"
          placeholder="Enter latitude"
          required
        />
      </b-form-group>

      <b-form-group v-if="content.title === 'pack'" id="input-group-5" label-for="input-5">
        <b-form-input
          id="input-5"
          v-model="form.lng"
          aria-label="Longitude"
          placeholder="Enter longitude"
          required
        />
      </b-form-group>

      <b-button type="submit" variant="primary">
        Submit
      </b-button>
      <b-button type="reset" variant="danger">
        Reset
      </b-button>
    </b-form>
  </b-modal>
</template>

<script>
export default {
  // eslint-disable-next-line vue/require-prop-types
  props: ['content'],
  data () {
    return {
      form: {
        name: '',
        gender: null,
        birthday: '',
        lat: '',
        lng: ''
      },
      wolveGender: [{ text: 'Select gender', value: null }, 'Female', 'Male']
    }
  },
  methods: {
    async onSubmit (event) {
      event.preventDefault()
      const { name, gender, birthday, lat, lng } = this.form
      const filteredFields = this.content.title === 'wolve' ? { name, gender, birthday } : { name, lat, lng }

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
      event.preventDefault()
      this.form.name = ''
      this.form.gender = null
      this.form.birthday = ''
      this.form.lat = ''
      this.form.lng = ''
    }
  }
}
</script>

<style lang="scss" scoped>

</style>
