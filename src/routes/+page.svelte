<script lang="ts">
    import type { UserInfo } from "$lib/api";
    import { APIClient } from "$lib/api";
    import { writable } from "svelte/store";
    import { goto } from "$app/navigation";
    import { debounce } from "$lib/debounce";
    import './styles/global.css';
    
    const userPlaceholders = ["natix", "Frosty", "Pan", "Sandie"];
    const hoursSinceEpoch = Math.floor((new Date()).getTime() / 1000 / 60 / 60);
    const userPlaceholder = userPlaceholders[hoursSinceEpoch % userPlaceholders.length];

    const searchResults = writable<UserInfo[]>([]);
    let searchQuery = "";

    const apiClient = new APIClient(fetch);

    async function onSearchFieldChange(event: InputEvent) {
        if (searchQuery.trim() === "") {
            $searchResults = [];
        } else {
            searchResults.set(await apiClient.searchForUserByName(searchQuery));
        }
    }
</script>

<style>
    .main-content-placer {
        display: grid;
        place-items: center;
    }
    .main-content {
        display: flex;
        flex-direction: column;
        gap: 1rem;
    }
    .main-title {
        font-family: "Ailerons";
        font-weight: 1000;
        font-size: 5rem;
        color: #fff;
    }
    .main-sub-title {
        font-family: "Orbitron";
        color: #fff;
        font-size: 1.5rem;

    }
    .search-user-title {
        font-family: "Ailerons";
        color: #fff;
        font-size: 3rem;
    }
    .user-search {
        width: 80%;
    }
    label[for="username-field"] {
        display: inline-block;
        width: 100%;

        color: #fff;
        text-align: center;
        font-size: 1.5rem;
    }
    #username-field {
        height: 3rem;
        width: 100%;
        background: #000;
        color: #ccc;
    }
    .users {
        display: flex;
        flex-direction: column;
        gap: .5rem;
        margin: .5rem;
    }
    .user {
        width: 15rem;
        line-height: 2.5rem;
        border: 1px solid #000;
        display: block;
        background: linear-gradient(45deg, rgba(72, 61, 139, 1), rgba(75, 0, 130, 1));
        color: #ccc;
        text-decoration: none;
        text-align: center;
        border-radius: 5px;
    }
    .user:hover {
        box-shadow: 0 0 20px rgba(75, 0, 130, 0.7);
        background: linear-gradient(45deg, rgba(72, 61, 139, 1), rgba(75, 0, 130, 1));
    }
    .user:active {
        box-shadow: 0 0 20px rgba(75, 0, 130, 0.7);
    }

    <h1 class="orbitron-font">Search for user</h1>
    <form on:submit|preventDefault={() => {}}>
        <label class="orbitron-font" for="username-field">Username</label>
        <input id="username-field" class="orbitron-font" type="search" placeholder={userPlaceholder} on:input={debounce(onSearchFieldChange)} bind:value={searchQuery}>
    </form>

<svelte:head>
    <title>Vail stats far from this world</title>
    <meta name="description" content="View official VAIL VR stats for any VAIL user">
</svelte:head>

<div>
    <div class="main-content-placer">
        <div class="main-content">
            <div class="title">
                <h1 class="main-title">Vail stat tracker</h1>
                <p class="main-sub-title">Delivered to you from far outside this world.</p>
            </div>
            
            <div class="user-search">
                <h1 class="search-user-title">Search for user</h1>
                <form on:submit|preventDefault={() => {}}>
                    <label for="username-field">Username</label>
                    <br/>
                    <input id="username-field" type="search" placeholder={userPlaceholder} on:input={debounce(onSearchFieldChange)} bind:value={searchQuery}>
                </form>

                <div class="users">
                    {#each $searchResults as user}
                        <a class="user" href={`/players/${user.id}`}>
                            {user.name}
                        </a>
                    {/each}
                </div>
            </div>
        </div>
    </div>
</div>

<style>
    /* Define font styles */
    .global-font {
        /* Use the font specified in global.css */
        font-family: 'Ailerons', sans-serif;
        /* Add any other styles if needed */
    }

    .orbitron-font {
        /* Use the Orbitron font */
        font-family: 'Orbitron', sans-serif;
        /* Add any other styles if needed */
    }
        /* Style for the footer */
    .footer {
        background-color: #333; /* Dark background color */
        color: #fff; /* White text color */
        text-align: center; /* Center align text */
        padding: 10px 0; /* Add padding */
        position: fixed; /* Fixed position at the bottom */
        width: 100%; /* Full width */
        bottom: 0; /* Stick to the bottom */
    }

    .footer-text {
        margin: 0; /* Remove default margin */
        font-size: 14px; /* Adjust font size */
    }
</style>
