<script>
    let { type, name, id, placeholder, required, autocomplete, autofocus, pattern, title, minlength, maxlength, value = $bindable("")} = $props();
	import 'remixicon/fonts/remixicon.css';

    let isPassword = type === "password";
    let isVisible = $state(false);
    function toggleShow() {
        if (type === "password") {
            type = "text";
            isVisible = true;
        } else {
            type = "password";
            isVisible = false;
        }
    }
</script>

<label for="id" style:--content={`"${placeholder}"`}>
    <input {type} {name} {id} {required} {autocomplete} {autofocus} {pattern} {title} {minlength} {maxlength} placeholder="" bind:value>
    {#if isPassword}
    <button type="button" onclick={toggleShow} aria-label="toggle password"><i class="{isVisible ? 'ri-eye-line' : 'ri-eye-off-line'}"></i></button>
    {/if}
</label>

<style>
    label {
        display: block;
        position: relative;
        border: 2px solid currentColor;
        border-radius: .5rem;

        &::before {
            content: var(--content);
            position: absolute;
            top: 0;
            left: 1rem;
            translate: 0 -50%;
            color: var(--color-text-secondary);
            background-color: var(--color-background);
            transition: .3s ease;
            padding-inline: .25rem;
            z-index: 2;
        }

        &:has(input:placeholder-shown:not(:focus))::before {
            top: 50%;
        }

        &:has(input:not(:placeholder-shown):invalid) {
            border-color: var(--color-danger);
            /* background-color: rgba(var(--color-danger-rgb), .2); */
        }

        &:has(input:not(:placeholder-shown):valid) {
            border-color: var(--color-success);
            /* background-color: rgba(var(--color-success-rgb), .2); */
        }

        &:hover, &:has(input:is(:hover, :focus-visible)) {
            border-color: var(--color-secondary);
        }

        & input {
            width: 100%;
            padding: 1rem;
            padding-right: 3rem;
            background-color: transparent;
            border: 0;

            &:-webkit-autofill, &:-webkit-autofill:focus, &:-webkit-autofill:hover, &:-webkit-autofill:active {
                -webkit-transition: background-color  5000s 0s, color 5000s 0s;
            }
        }

        & button {
            position: absolute;
            top: 50%;
            right: .5rem;
            translate: 0 -50%;
            border: 0;
            background-color: transparent;
            cursor: pointer;
            padding: .5em;
            font-size: 1.25rem;
            transition: .3s ease;

            &:active {
                scale: .9;
            }

            &:focus-visible {
                color: var(--color-secondary);
            }
        }
    }
</style>