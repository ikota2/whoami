<script>
    import Favs from './Favs.svelte';
    import Discoveries from './Discoveries.svelte';
    import Links from './Links.svelte';
    import About from './About.svelte';

	let activeTab = 'favs';

	const tabs = [
		{ id: 'favs', name: 'Favorites', component: Favs },
		{ id: 'discoveries', name: 'Discoveries', component: Discoveries },
		{ id: 'links', name: 'Links', component: Links },
		{ id: 'about', name: 'About', component: About },
	];

	function setActiveTab(tabId) {
		activeTab = tabId;
	}
</script>

<nav>
    <ul>
        {#each tabs as tab}
            <li>
                <button on:click={() => setActiveTab(tab.id)}
                        class:selected={activeTab === tab.id}>
                    {tab.name}
                </button>
            </li>
        {/each}
    </ul>
</nav>
<main>
    <section>
        {#each tabs as tab}
            {#if activeTab === tab.id}
                <svelte:component this={tab.component} />
            {/if}
        {/each}
    </section>
</main>

<style>

nav ul {
	display: flex;
	justify-content: center;
	list-style: none;
}

nav li {
	margin-right: 1rem;
}

nav button {
	background: none;
	border: none;
	cursor: pointer;
	font-size: 1rem;
}

nav button.selected {
	font-weight: bold;
}
</style>

