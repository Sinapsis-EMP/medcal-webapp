<script>
  import List from 'smelte/src/components/List';
  import Icon from 'smelte/src/components/Icon';

  export let instituciones;

  let selected = false;
</script>

<h6 class="mb-3 mt-6">Dense</h6>
<List dense items={instituciones} />
