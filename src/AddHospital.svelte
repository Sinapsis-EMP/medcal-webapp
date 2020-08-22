<script>
  import GooglePlacesAutocomplete from '@silintl/svelte-google-places-autocomplete';
  const options = {
    fields: ['address_components', 'geometry', 'review', 'type', 'url'],
    types: ['establishment'],
  };
  const placeholder = 'Destination city';
  const googlePlacesApiKey = 'AIzaSyCUPgy21XQ3YaF94LGU1Ap0w6ZcxvYxy5Y';
  const onPlaceChanged = (ev) => {
    place = ev.detail.place;
  };
  const onReady = (ev) => {
    // console.log('a', pl);
  };

  export let place;

  let locationName = ''; // localStorage.get('locationName') || '';
</script>

<style>
  .ancho {
    width: 5005px;
  }
</style>

<GooglePlacesAutocomplete
  apiKey={googlePlacesApiKey}
  class="ancho"
  on:place_changed={onPlaceChanged}
  {options}
  on:ready={onReady}
  {placeholder}
  value={locationName} />
