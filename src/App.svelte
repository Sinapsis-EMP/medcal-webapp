<script>
  import { FirebaseApp, User, Doc, Collection } from 'sveltefire';

  import firebase from 'firebase/app';
  import 'firebase/firestore';
  import 'firebase/auth';
  import 'firebase/performance';
  import 'firebase/analytics';
  import 'smelte/src/tailwind.css';
  import TextField from 'smelte/src/components/TextField';

  import Button from 'smelte/src/components/Button';
  import AddHospital from './AddHospital.svelte';
  import ListaInstituciones from './ListaInstituciones.svelte';
  import Layout from './Layout.svelte';

  let firebaseConfig = {
    apiKey: 'AIzaSyDLjnjDgQe92lXVasJm_S7BXEzgOwFWUfM',
    authDomain: 'marco-6beb1.firebaseapp.com',
    databaseURL: 'https://marco-6beb1.firebaseio.com',
    projectId: 'marco-6beb1',
    storageBucket: 'marco-6beb1.appspot.com',
    messagingSenderId: '877543684199',
    appId: '1:877543684199:web:f382336f906065948bbcc2',
    measurementId: 'G-S1TV8D7S1H',
  };

  let lugar;
  let comentario;
  const instituciones = [
    {
      text: 'Item 1',
      icon: 'favorite',
      subheading: 'Subheading 1',
    },
    {
      text: 'Item 2',
      icon: 'favorite',
      subheading: 'Subheading 2',
    },
    {
      text: 'Item 3',
      icon: 'favorite',
      subheading: 'Subheading 3',
    },
  ];

  firebase.initializeApp(firebaseConfig);
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1,
  em {
    color: #ff3e00;
  }

  hr {
    height: 1px;
    border: none;
    background: rgb(195, 195, 195);
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <Layout />
  <h6 class="mb-3 mt-6">Basic</h6>
  <TextField label="Test label" textarea bind:value={comentario} />

  <AddHospital bind:place={lugar} />

  {#if !firebaseConfig.projectId}
    <strong>Step 0</strong>
    Create a
    <a href="https://firebase.google.com/">Firebase Project</a>
    and paste your web config into
    <code>App.svelte</code>
    .
  {/if}

  <!-- 1. 🔥 Firebase App -->
  <FirebaseApp {firebase}>

    <Collection
      path="instituciones"
      query={(ref) => ref.orderBy('createdAt')}
      let:data={instituciones}
      let:ref={institucionesRef}
      log>

      {#if !instituciones.length}No hay instituciones...{/if}

      <ListaInstituciones {instituciones} />

      <!-- <Button
      on:click={() => commentsRef.add({
          text: comentario,
          createdAt: Date.now(),
          lugar: lugar.url,
        })}>
      Add Comment
    </Button> -->

      <span slot="loading">Cargando instituciones...</span>

    </Collection>

    {#if lugar}
      El usuario ya eligió lugar, entonces ya podemos creart el doc. en Firebase
      <Doc
        path={`instituciones/${lugar.place_id}`}
        let:data={institucion}
        let:ref={institucionRef}
        log>

        <div>La institución esd {institucion.url}</div>
        <span slot="loading">Cargando datos .t...</span>
        <span slot="fallback">

          <br />
          <Button
            on:click={() => {
              console.log(lugar);
              institucionRef.set({
                createdAt: Date.now(),
                types: lugar.types,
                url: lugar.url,
                name: lugar.name,
                icon: lugar.icon,
                photos: lugar.photos.map((photo) => {
                  return photo.getUrl();
                }),
              });
            }}>
            Create Institución
          </Button>
        </span>

      </Doc>
    {/if}

    <h1>💪🔥 Mode ctivated</h1>

    <p>
      <strong>Tip:</strong>
      Open the browser console for development logging.
    </p>

    <!-- 2. 😀 Get the current user -->
    <User let:user let:auth>
      Howdy 😀! User
      <em>{user.uid}</em>

      <button on:click={() => auth.signOut()}>Sign Out</button>

      <div slot="signed-out">
        <Button on:click={() => auth.signInAnonymously()}>
          Sign In Anonymously
        </Button>
      </div>

      <hr />
      <!-- 3. 📜 Get a Firestore document owned by a user -->

      <Doc path={`posts/${user.uid}`} let:data={post} let:ref={postRef} log>

        <h2>{post.title}</h2>

        <p>
          Document created at
          <em>{new Date(post.createdAt).toLocaleString()}</em>
        </p>

        <span slot="loading">Loading post...</span>
        <span slot="fallback">
          <button
            on:click={() => postRef.set({
                title: '📜 I like Svelte',
                createdAt: Date.now(),
              })}>
            Create Document
          </button>
        </span>

        <!-- 4. 💬 Get all the comments in its subcollection -->

        <h3>Comments</h3>
        <Collection
          path={postRef.collection('comments')}
          query={(ref) => ref.orderBy('createdAt')}
          let:data={comments}
          let:ref={commentsRef}
          log>

          {#if !comments.length}No comments yet...{/if}

          {#each comments as comment}
            <p>
              ID:v
              <em>{comment.ref.id}</em>
            </p>
            <p>
              {comment.text}
              <button on:click={() => comment.ref.delete()}>Delete</button>
            </p>
          {/each}

          <Button
            on:click={() => commentsRef.add({
                text: comentario,
                createdAt: Date.now(),
                lugar: lugar.url,
              })}>
            Add Comment
          </Button>

          <span slot="loading">Loading comments...</span>

        </Collection>
      </Doc>
    </User>
  </FirebaseApp>

</main>

<!-- Styles -->
