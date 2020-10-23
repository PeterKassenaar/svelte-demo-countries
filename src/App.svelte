<script>
    // This is the container/smart/statefull component App.svelte. It holds most
    // of the logic and passes it down to child components.

    // TODO: Error handling! If you type an invalid country name it logs a 404 to the console,
    // but not in the UI. We can fix this by using {#await} and then-try-catch blocks.
    // See https://svelte.dev/tutorial/await-blocks for details.

    // Imports
    import * as axios from "axios";
    import CountryList from "./components/CountryList.svelte";
    import CountryDetail from "./components/CountryDetail.svelte";

    // 0. Variables/state in this application
    const src = 'img/svelte-logo.png'; // we could have referenced the image directly in html.
    const url = 'https://restcountries.eu/rest/v2/name/';
    const fields = '?fields=name;flag;capital';
    let countryName = '';
    let countries = [];
    let selectedCountry = '';

    // 1. Methods/functions
    const search = async () => {
        selectedCountry = ''; // 1a. Reset, then get new countries
        countries = await axios.get(`${url}${countryName}${fields}`)
            .then(res => res.data)
    }

    const getDetails = async (event) => {
        // 1b. Some simple logging, then get details for the selected country
        // and do a subsequent lookup. Only pass the first result to the child component.
        console.log('search details for:: ', event.detail.name);
        selectedCountry = await axios.get(`${url}${event.detail.name}`)
            .then(res => res.data[0]);
        console.log(selectedCountry);
    }

    const clear = () => {
        // 1c. Reset both
        countries = [];
        selectedCountry = '';
    }
</script>

<div class="container">
    <!--    First row, title, logo and textbox-->
    <div class="row">
        <div class="col-12">
            <h1><img {src} class="logo" alt="Svelte logo"> Svelte Country Picker</h1>
            <label>
                <input class="form-control-lg"
                       type="text"
                       bind:value={countryName}
                       placeholder="search countries...">
                <button class="btn btn-success" on:click={search}>Search</button>
                <button class="btn btn-warning" on:click={clear}>Clear</button>
            </label>
        </div>
    </div>
    <!--    Second row, CountryList and CountryDetail components-->
    <div class="row">
        <div class="col-6">
            <!-- Only show child components if there are countries available-->
            {#if countries.length > 0}
                <CountryList countries={countries} on:selected={getDetails}/>
            {/if}
        </div>
        <div class="col-6">
            {#if selectedCountry}
                <CountryDetail country={selectedCountry}/>
            {/if}
        </div>
    </div>
    <!--    Third row - small disclaimer-->
    <div class="row">
        <div class="col">
            <hr>
            <p class="text-sm-center muted">
                Copyright (C) 2020 - Peter Kassenaar, info@kassenaar.com.<br>
                <code>For learning purposes. See source code</code>.
            </p>
        </div>
    </div>
</div>

<style>
    /* Styles in ../public/global.css */
</style>
