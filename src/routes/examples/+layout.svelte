<script lang="ts">
    import { PUBLIC_GITHUB_REPO_URL } from '$env/static/public';
    import type { Snippet } from 'svelte';
    import type { LayoutData } from './$types';

    type Props = {
        data: LayoutData;
        children: Snippet;
    };

    let { children, data }: Props = $props();
    const { currentExample, nextExample, previousExample, readmeHtml } = $derived(data);
</script>

<div class="layout-wrapper">
    <nav>
        <div>
            <a href="/">Home</a>
        </div>
        <div>
            <a href={PUBLIC_GITHUB_REPO_URL} target="_blank">GitHub Repo</a>
        </div>
    </nav>

    <header>
        <h1>{currentExample.title}</h1>
        <hr style="width:100%" />
        <a href="{PUBLIC_GITHUB_REPO_URL}/tree/main/{currentExample.githubPath}">Link to GitHub</a>
    </header>

    <div class="content-wrapper">
        {@render children()}

        <h2>Explanation</h2>

        <hr />

        <!-- eslint-disable-next-line svelte/no-at-html-tags -->
        {@html readmeHtml}
    </div>

    <hr />

    <footer>
        <div>
            {#if previousExample}
                <a href={previousExample.pathname}>Previous: {previousExample.title}</a>
            {/if}
        </div>
        <div>
            {#if nextExample}
                <a href={nextExample.pathname}>Next: {nextExample.title}</a>
            {/if}
        </div>
    </footer>
</div>

<style>
    nav {
        padding: 1rem 0px;
        border-bottom: 1px solid #ccc;
        display: flex;
        justify-content: space-between;
    }

    footer {
        display: flex;
        justify-content: space-between;
        padding: 2rem 0px;
    }

    .layout-wrapper {
        display: flex;
        flex-direction: column;
        height: 100%;
    }

    .content-wrapper {
        flex-grow: 1;
    }
</style>
