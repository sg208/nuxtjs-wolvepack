<template>
  <b-modal
    :id="content.id"
    centered
    hide-footer
    :title="`View ${content.title} in map (Lat: ${content.coordinate.lat}, Lng: ${content.coordinate.lng})`"
    size="lg"
    ok-only
  >
    <img
      :src="`https://maps.googleapis.com/maps/api/staticmap?center=${content.coordinate.center}&zoom=13&size=1000x400&maptype=roadmap&markers=color:blue%7Clabel:S%7C40.702147,-74.015794&markers=color:green%7Clabel:G%7C40.711614,-74.012318&markers=color:red%7Clabel:C%7C40.718217,-73.998284&key=${$config.googleMapSecret}`"
      :alt="`Map location of ${content.title} (Lat: ${content.coordinate.lat}, Lng: ${content.coordinate.lng})`"
      class="map"
    >
  </b-modal>
</template>
<script>
export default {
  // eslint-disable-next-line vue/require-prop-types
  props: ['content']
}
</script>

<style lang="scss" scoped>
    .map {
        width: 100%;
        height: auto;
    }
</style>
