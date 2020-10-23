<!--This 'dumb' component is receiving a list of countries from its parent-->
<script>
    import {createEventDispatcher} from "svelte";

    export let countries;

    // This is Svelte's way of throwing events from a nested (child) component:
    // create an event dispatcher and dispatch some event with optional value
    const dispatch = createEventDispatcher();
    const selectCountry = name => {
        dispatch('selected', {
            name: name
        })
    }

</script>

<!--Personally I like to wrap my components in a *single root element* (here: a <div>) but -->
<!--    this is not mandatory in Svelte, as it is in other frameworks. -->
<div>
    <ul class="list-group">
        {#each countries as country}
            <!--            Clicking on this listitem fires an event to load -->
            <!--            details for the specific country to the parent component.-->
            <li class="list-group-item"
                on:click={selectCountry(country.name)}>
                <img src={country.flag}
                     class="countryFlag"
                     alt="Flag of {country.name}">
                <h4>{country.name }</h4>
                <p>{country.capital}</p>
            </li>
        {/each}
    </ul>
</div>

<style>
    /*styles defined in ../public/global.css */
</style>
