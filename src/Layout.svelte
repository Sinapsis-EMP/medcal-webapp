<script>
  import Tabs from 'smelte/src/components/Tabs';

  // import { stores } from '@sapper/app';
  // const { page } = stores();

  const topMenu = [
    { to: '/', text: 'Opiniones', icon: 'favorite' },
    { to: '/', text: 'Typography' },
    { to: '/', text: 'Color' },
  ];

  // Or simply use document.location.pathname
  // if your app isn't sapper.
  //$: path = $document.location.pathname;
  //   let path = $document.location.pathname;
</script>

<div>
  sssss
  <Tabs items={topMenu} path="/" color="black" />

</div>
