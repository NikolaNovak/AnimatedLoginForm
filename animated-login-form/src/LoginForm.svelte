<script>
  let strength = 0;
  let validations = []
	let showPassword = false
	let showToggle = false

  const validatePassword = (e) => {
    const password = e.target.value;

    validations = [
        (password.length > 5), 
        (password.search(/[A-Z]/) > -1), 
        (password.search(/[0-9]/) > -1), 
        (password.search(/[$&+,:;=?@#]/) > -1) 
    ]

    strength = validations.reduce((acc, cur) => acc + cur, 0)
		showToggle = password.length > 0
  }
</script>

<style>
	form {
		--text-color: #afafaf;
	}

  .field {
    width: 100%;
    position: relative;
    border-bottom: 2px dashed var(--text-color);
    margin: 4rem auto 1rem;
    transition: 500ms;
  }

	.input {
		outline: none;
    border: none;
    overflow: hidden;
    margin: 0;
    width: 100%;
    padding: 0;
    background: none;
    color: white;
    font-size: 1.25rem;
    font-weight: bold;
	}

	.inputEmail:valid {
		color: yellowgreen;
	}

	.inputEmail:invalid {
		color: orangered;
	}

	.inputPassword {
		color: orangered;
	}

	.inputPassword.valid {
		color: yellowgreen;
	}

	.field::after {
		content: "";
		position: relative;
		display: block;
		height: 4px;
		width: 100%;
		background: #cf68ff;
		transform: scaleX(0);
		transform-origin: 0%;
		transition: transform 500ms ease;
		top: 2px;
	}

	.field:focus-within {
		border-color: transparent;
	}

	.field:focus-within::after {
		transform: scaleX(1);
	}

	.label {
		color: var(--text-color);
		font-size: 1.2rem;
		z-index: -1;
		position: absolute;
		transform: translateY(-1.75rem);
		transform-origin: 0%;
		transition: transform 400ms;
	}

	.field:focus-within .label,
	.input:not(:placeholder-shown) + .label {
		transform: scale(0.8) translateY(-4.25rem);
	}

  .toggle-password {
    position: absolute;
    cursor: pointer;
    font-size: 0.8rem;
    right: 0;
    bottom: 0.5rem;
  }
	
	.strength {
    display: flex;
    height: 10px;
    width: 100%;
  }

	.bar {
    margin-right: 5px;
    height: 100%;
    width: 25%;
    transition: box-shadow 500ms;
    box-shadow: inset 0px 10px #1f1f1f;
  }

	.bar:last-child {
		margin-right: 0;
	}

	.bar-show {
		box-shadow: none;
	}

	.bar-1 {
		background: linear-gradient(to right, red, orangered);
	}

	.bar-2 {
		background: linear-gradient(to right, orangered, yellow);
	}

	.bar-3 {
		background: linear-gradient(to right, yellow, yellowgreen);
	}

	.bar-4 {
		background: linear-gradient(to right, yellowgreen, green);
	}

	ul {
		list-style: none;
		margin: 20px 0;
		padding: 0;
		text-align: left;
		font-size: 0.8rem;
		color: var(--text-color);
	}

	button {
		margin-top: 1.5rem;
		padding: 10px 30px;
		font-weight: bold;
		border: 2px solid greenyellow;
		color: greenyellow;
		border-radius: 100px;
		background: transparent;
		transition: all 1000ms;
		cursor: pointer;
	}

	button:disabled {
		border-color: rgb(70, 70, 70);
		color: rgb(70, 70, 70);
		cursor: not-allowed;
	}

</style>

<main>
	<form>
		<div class="field">
			<input type="email" name="email" class="input inputEmail" placeholder=" " />
			<label for="email" class="label">Email</label>
		</div>

		<div class="field">
			<input type={showPassword ? 'text' : 'password'} name="password" class="input inputPassword" class:valid={strength === 4} placeholder=" " on:input={validatePassword} />
			<label for="password" class="label">Password</label>
			{#if showToggle}
			<span
				class="toggle-password"
				on:click={() => (showPassword = !showPassword)}>
				{showPassword ? '🙈' : '👁️'}
			</span>
			{/if}
		</div>

		<div class="strength">
			<span class="bar bar-1" class:bar-show={strength > 0} />
			<span class="bar bar-2" class:bar-show={strength > 1}/>
			<span class="bar bar-3" class:bar-show={strength > 2}/>
			<span class="bar bar-4" class:bar-show={strength > 3}/>
		</div>

    <ul>
			<li> {validations[0] ? '✔️' : '❌'} must be at least 5 characters</li>
			<li> {validations[1] ? '✔️' : '❌'} must contain a capital letter</li>
			<li> {validations[2] ? '✔️' : '❌'} must contain a number</li>
			<li> {validations[3] ? '✔️' : '❌'} must contain one of $&+,:;=?@#</li>
		</ul>

		<button disabled={strength < 4}>Sign In</button>
	</form>
</main>