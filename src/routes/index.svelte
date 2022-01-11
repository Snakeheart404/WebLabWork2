<script>
  import Loader from '../lib/Loader.svelte';

  let form = {
    reset: () => {}
  };
  let textError = '';
  let showSpinner = false;
  let statusMessage = false;
  let formBtnDisable = false;
  function resetFormStatus() {
    statusMessage = false;
    formBtnDisable = false;
  }
  let contactFormHandler = async (e) => {
    formBtnDisable = true;
    showSpinner = true;
    statusMessage = false;
    const referrerVal = document.referrer;
    let formData = { referrer: referrerVal };
    Array.from(form.elements)
      .filter((e) => e.tagName !== 'BUTTON')
      .forEach((e) => {
        formData[e.name] = e.value;
      });
    try {
      await fetch('/api/sendmail', {
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(formData),
        method: 'POST'
      }).then((res) => {
        if (res.ok) {
          return res;
        }
        throw res;
      });
      statusMessage = true;
      e.target.reset();
    } catch (e) {
      if (e.status >= 500) {
        textError = 'Error server';
      } else if (e.status === 400) {
        textError = 'Empty email message!';
      } else if (e.status === 429) {
        textError = 'Too many messages!';
      }
      statusMessage = false;
      console.log(e);
    } finally {
      showSpinner = false;
      formBtnDisable = false;
    }
  };
</script>

<form
  class="contact-form"
  bind:this={form}
  on:submit|preventDefault={contactFormHandler}
>
  <h1>Fill in the form:</h1>
  <input
    class="contact-form-input"
    type="text"
    name="userName"
    placeholder="Name"
    required
  />
  <input
    class="contact-form-input"
    type="email"
    name="userEmail"
    placeholder="Email"
    required
  />
  <textarea
    class="contact-form-message"
    name="userMessage"
    cols="30"
    rows="10"
    placeholder="Your message goes here..."
    required
  />

  <button
    class="button contact-form-button"
    type="submit"
    disabled={formBtnDisable}
  >
    {#if showSpinner}
      <Loader />
    {:else if true}
      Send
    {/if}
  </button>

  {#if statusMessage}
    <p class="status-text success">
      Message sent!
      <button class="button class-btn" on:click={resetFormStatus}>
        &times;
      </button>
    </p>
  {:else if textError != ''}
    <p class="status-text error">
      {textError}
      <button class="button class-btn" on:click={resetFormStatus}>
        &times;
      </button>
    </p>
  {/if}
</form>

<style>
  :root {
    --custom-red: #cd5c5c;
    --form-background: #f5f5f5;
    --input-background: #ffffff;
    --button-text: #ffffff;
  }
  form {
    background-color: var(--form-background);
    padding: 10px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 50%;
    width: 25%;
    border-radius: 10px;
    margin: 10px auto auto auto;
  }
  form input {
    width: 100%;
    border: none;
    border-radius: 10px;
    background: var(--input-background);
    margin: 5px;
  }
  form button {
    color: var(--button-text);
    background-color: var(--custom-red);
    border-radius: 10px;
    margin: 5px;
  }
  form textarea {
    width: 100%;
    border: none;
    border-radius: 10px;
    background: var(--input-background);
    resize: none;
    margin: 5px;
  }
  main {
    text-align: center;
    display: flex;
  }
  form h1 {
    color: var(--custom-red);
    text-transform: uppercase;
    font-size: 2em;
    font-weight: 100;
  }
  p {
    background: var(--form-background);
    border: 1px solid;
    border-color: var(--custom-red);
  }
  img {
    width: 30%;
    height: 30%;
  }
</style>
