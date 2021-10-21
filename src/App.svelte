<main>
	<form><h1>Fill in the form:</h1>
		<input type="text" name="name" placeholder="Name">
		<input type="email" name="email" placeholder="Email">
		<textarea name="text" placeholder="Enter your mesage..."></textarea>
		<button>Send</button>
	</form>
</main>

<style>
    :root {
        --custom-red: #CD5C5C;
        --form-background: #F5F5F5;
				--input-background: #FFFFFF;
        --button-text: #FFFFFF;
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
        margin: auto auto auto auto;
		}

    form input {
				width: 100%;
        border: none;
        border-radius: 10px;
        background: var(--input-background);
    }

		form button {
        color: var(--button-text);
				background-color: var(--custom-red);
        border-radius: 10px;
		}

		form textarea {
        width: 100%;
        border: none;
        border-radius: 10px;
        background: var(--input-background);
				resize: none;
		}

	main {
			text-align: center;
			display: flex;
      /*padding: 1em;
			max-width: 240px;
			margin-right: auto;
			margin-left: auto;*/
	}

	form h1 {
		color: var(--custom-red);
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>