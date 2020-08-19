<script>
  import GooglePlacesAutocomplete from '@silintl/svelte-google-places-autocomplete';
  const options = {
    fields: ['address_components', 'geometry'],
    types: ['establishment'],
  };
  const placeholder = 'Destination city';
  const googlePlacesApiKey = 'AIzaSyCUPgy21XQ3YaF94LGU1Ap0w6ZcxvYxy5Y';
  const onPlaceChanged = (place) => {
    console.log('a', place);
  };
  const onReady = (place) => {
    console.log('a', place);
  };

  let locationName = ''; // localStorage.get('locationName') || '';
</script>

<GooglePlacesAutocomplete
  apiKey={googlePlacesApiKey}
  class="form-control"
  on:place_changed={onPlaceChanged}
  {options}
  on:ready={onReady}
  {placeholder}
  value={locationName} />
