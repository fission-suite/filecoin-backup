<script lang="ts">
  export let status: number;
  export let error: { message: any; stack: any };

  import { Row, Column, Link } from 'carbon-components-svelte';

  const dev = process.env.NODE_ENV === 'development';
</script>

<Row>
  <Column lg={16}>
    <h1>{status}</h1>
    <div>
      {error.message}
      <Link inline href="/">Return home</Link>
    </div>
    {#if dev && error.stack}
      <div>
        <pre>{error.stack}</pre>
      </div>
    {/if}
  </Column>
</Row>
