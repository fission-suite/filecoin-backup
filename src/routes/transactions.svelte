<script lang="ts">
  import {
    Button,
    DataTable,
    Form,
    FormGroup,
    NumberInput,
    CodeSnippet,
    Row,
    Column,
    Loading
  } from 'carbon-components-svelte';
  import { onMount, onDestroy } from 'svelte';
  import { goto } from '@sapper/app';
  import copy from 'clipboard-copy';

  const wallet = 'f16tugakjopyofmxy7uv2d6wdj7wyyr3ueyfu7w7a';

  const headers = [
    { key: 'date', value: 'Date' },
    { key: 'amount', value: 'Amount' }
  ];

  const placeholderRows = [
    {
      id: 'a',
      date: '02/03/2021',
      amount: '1.0'
    },
    {
      id: 'b',
      date: '02/11/2021',
      amount: '2.2'
    },
    {
      id: 'c',
      date: '02/12/2021',
      amount: '90.5'
    }
  ];
  /**
   * Webnative initialization. In order to avoid running webnative on the
   * server, we initialize webnative inside onMount. Default values are
   * provided to avoid server-side errors. We update them as soon as we can
   * on the client.
   */

  let session = {
    username: '',
    errorMessage: '',
    authed: false,
    loading: true,
    error: false
  };
  let unsubscribe: VoidFunction = () => {};

  onMount(async () => {
    const { sessionStore } = await import('../webnative');

    unsubscribe = sessionStore.subscribe(val => {
      if (!val.loading && !val.authed) {
        goto('/');
      }
      session = val;
    });
  });

  onDestroy(unsubscribe);
</script>

{#if session.loading}
  <div class="loading">
    <Loading withOverlay={false} />
  </div>
{:else if session.authed}
  <Row>
    <Column padding aspectRatio="16x9" style="margin: 2rem">
      <Row>
        <Column>
          <Row>
            <Column>
              <div class="balance">
                <h1 class="balance-value">10.0</h1>
                <div class="balance-label">
                  <img
                    class="logo"
                    src="/filecoin-logo.svg"
                    alt="Filecoin wallet balance"
                  />
                  <h2>Filecoin Wallet</h2>
                </div>
              </div>
            </Column>
          </Row>
          <Row>
            <Column
              padding
              aspectRatio="3x4"
              style="border: 2px solid #aaa; border-radius: 4px; margin: 2rem"
            >
              <div class="card">
                <h2>Filecoin Wallet</h2>
                <p>
                  Filecoin Backup is currently running on the
                  <span class="bold">Nerpa</span> testnet.
                </p>
                <h4>Add Funds</h4>
                <p>
                  Please visit the
                  <a
                    href="https://faucet.nerpa.interplanetary.dev/"
                    target="_blank"
                  >
                    Lotus Devnet Faucet
                  </a>
                  to deposit FIL to your wallet.
                </p>
                <CodeSnippet on:click={() => copy(wallet)}>{wallet}</CodeSnippet
                >
                <h4>Paying for Storage</h4>
                <p>
                  Before you can make a storage deal, you will want to send some
                  Filecoin to a Lotus provider. We have configured a Lotus
                  Provider for you to use. Once the Lotus provider is holding
                  your funds, you will be ready to start backing up files!
                </p>
                <h4>Send Funds to Lotus Provider</h4>
                <Form on:submit>
                  <FormGroup>
                    <NumberInput helperText="Enter the amount of FIL to send" />
                  </FormGroup>
                  <Button type="submit">Send Funds</Button>
                </Form>
              </div>
            </Column>
          </Row>
        </Column>
        <Column>
          <Row>
            <Column>
              <div class="balance">
                <h1 class="balance-value">250.3</h1>
                <div class="balance-label">
                  <img
                    class="logo"
                    src="/filecoin-symbol-color.svg"
                    alt="Lotus provider balance"
                  />
                  <h2>Lotus Provider</h2>
                </div>
              </div>
            </Column>
          </Row>
          <Row>
            <Column
              padding
              aspectRatio="3x4"
              style="border: 2px solid #aaa; border-radius: 4px; margin: 2rem"
            >
              <div class="card">
                <h2>Lotus Provider</h2>
                <p>
                  The <span class="bold">Blossom of Splendor</span> is holding your
                  FIL. You are ready to backup your files.
                </p>
                <h4>Transactions</h4>
                <p>
                  Funds sent from your Filecoin wallet to the Lotus Provider are
                  listed here. Check the backups page for a listing of storage
                  deals.
                </p>
                <DataTable {headers} rows={placeholderRows} />
              </div>
            </Column>
          </Row>
        </Column>
      </Row>
    </Column>
  </Row>
{/if}

<style>
  .loading {
    display: grid;
    place-items: center center;
    height: 100%;
  }

  .balance {
    display: grid;
    place-items: center center;
    grid-gap: 0.3rem;
  }

  .balance-value {
    font-size: 82px;
  }

  .balance-label {
    display: grid;
    place-items: center center;
    grid-template-columns: auto 1fr;
    grid-gap: 1rem;
  }

  .logo {
    display: inline-block;
    width: 3rem;
    height: 3rem;
  }

  .card {
    display: grid;
    row-gap: 1rem;
  }

  .bold {
    font-weight: bold;
  }
</style>