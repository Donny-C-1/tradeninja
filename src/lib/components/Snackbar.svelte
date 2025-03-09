<script>
    let { children, visible = false } = $props();
</script>

<div class="snack_bar" class:visible>{@render children()}</div>

<style>
    .snack_bar {
        position: fixed;
        left: 50%;
        bottom: 2rem;
        z-index: 5;
        translate: -50%;
        visibility: hidden;
        min-width: 15.625rem;
        background-color: var(--color-surface); 
        color: var(--color-text);
        text-align: center;
        border-radius: .25rem;
        padding: 1rem;
        box-shadow: 0 0 .5rem .1rem rgba(0, 0, 0, .25);

        &.visible {
            visibility: visible;
            -webkit-animation: slideUp .5s, fadeOut .5s 3s;
            animation: fadeIn .5s, fadeOut .5s 3s;
        }

        @-webkit-keyframes fadeIn {
            from {
                bottom: 0;
                opacity: 0;
            }
            to {
                bottom: 2rem;
                opacity: 1;
            }
        }

        @keyframes fadeIn {
            from {
                bottom: 0;
                opacity: 0;
            }
            to {
                bottom: 2rem;
                opacity: 1;
            }
        }

        @-webkit-keyframes fadeOut {
            from {
                bottom: 0;
                opacity: 0;
            }
            to {
                bottom: 2rem;
                opacity: 1;
            }
        }

        @keyframes fadeOut {
            from {
                bottom: 0;
                opacity: 0;
            }
            to {
                bottom: 2rem;
                opacity: 1;
            }
        }
    }
</style>