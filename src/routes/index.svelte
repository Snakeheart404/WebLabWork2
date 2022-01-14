<script>
    import Loader from '../lib/Loader.svelte';

    let form;
    let notificationText;
    let showSpinner = false;
    let formBtnDisable = false;
    function resetFormStatus() {
        formBtnDisable = false;
        notificationText = '';
    }
    let contactFormHandler = async (e) => {
        notificationText = '';
        formBtnDisable = true;
        showSpinner = true;
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
            notificationText = 'Message sent!';
            e.target.reset();
        } catch (e) {
            if (e.status >= 500) {
                notificationText = 'Error server';
            } else if (e.status === 400) {
                notificationText = 'Empty email message!';
            } else if (e.status === 429) {
                notificationText = 'Too many messages!';
            }
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

    {#if notificationText}
        <p class="status-text success">
            {notificationText}
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
        width: 20%;
        min-width: 300px;
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
</style>
