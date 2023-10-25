<script lang='ts'>
    import { onMount } from 'svelte';
    // wasm path is relative to the file that it is being imported from 
    import init from '../../example-rs/pkg/example_rs_bg.wasm?init';

    // You probably want to catch all args to avoid errors
    let addFunction: CallableFunction = (...args: any[]) => !args;

    function initialize() {
        init().then((instance) => {
            addFunction = instance.exports.add as CallableFunction;
            console.log(addFunction(5, 8));
        });
    }

    onMount(initialize);
</script>

<p>{addFunction(3, 5)}</p>

<style>
    p {
        font-size: 2em;
    }
</style>
