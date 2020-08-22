<script>
  import List from 'smelte/src/components/List';
  import Icon from 'smelte/src/components/Icon';
  import Image from 'smelte/src/components/Image';

  export let instituciones;

  let selected = false;
</script>

<h6 class="mb-3 mt-6">aaa</h6>
<List
  dense
  items={instituciones.map((inst) => ({
    text: inst.name,
    icon: 'user',
    subheading: (inst.photos && inst.photos[0]) || 'no foto',
  }))} />
