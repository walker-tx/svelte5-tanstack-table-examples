<script lang="ts">
    import { PUBLIC_GITHUB_REPO_URL } from '$env/static/public';
    import exampleRegistry from '$lib/services/example-registry';
    import { createSvelteTable, renderSnippet } from '$lib/table';
    import FlexRender from '$lib/table/flex-render.svelte';
    import { createColumnHelper, getCoreRowModel } from '@tanstack/table-core';
    import { createRawSnippet } from 'svelte';

    const colHelp = createColumnHelper<(typeof exampleRegistry)[number]>();

    const linkCell = createRawSnippet<[{ href: string; text: string }]>((props) => {
        const { href, text } = props();
        return {
            render: () => `<a href="${href}">${text}</a>`
        };
    });

    const columnDefs = [
        colHelp.accessor('title', { header: 'Title' }),
        colHelp.accessor('pathname', {
            header: 'Demo',
            cell({ cell }) {
                return renderSnippet(linkCell, { href: cell.getValue(), text: 'Link to Demo' });
            }
        }),
        colHelp.accessor('githubPath', {
            header: 'GitHub',
            cell({ cell }) {
                const href = `${PUBLIC_GITHUB_REPO_URL}/tree/main/${cell.getValue()}`;
                return renderSnippet(linkCell, { href, text: 'Link to GitHub' });
            }
        })
    ];

    const table = createSvelteTable({
        data: exampleRegistry,
        columns: columnDefs,
        getCoreRowModel: getCoreRowModel()
    });
</script>

<h1>Tanstack Table v8 + Svelte 5 Reference</h1>

<a href={PUBLIC_GITHUB_REPO_URL}>GitHub Repository</a>

<hr />

<h2>Overview</h2>

<hr />

<p>
    The <code>@tanstack/table-svelte</code> adapter doesn't support Svelte 5 because it uses the
    <code>svelte/internal</code> package, which is being deprecated. This project is a reference for
    how to implement TanStack Table v8 with Svelte 5.
</p>

<p>
    You can use this site and its repository on <a href={PUBLIC_GITHUB_REPO_URL}>GitHub</a> as a reference
    for implementing and using your own adapter.
</p>

<h2>Examples</h2>

<hr />

<table class="examples-table">
    <thead>
        <tr>
            {#each table.getHeaderGroups() as headerGroup}
                {#each headerGroup.headers as header}
                    <th>
                        <FlexRender
                            content={header.column.columnDef.header}
                            context={header.getContext()}
                        />
                    </th>
                {/each}
            {/each}
        </tr>
    </thead>
    <tbody>
        {#each table.getRowModel().rows as row}
            <tr>
                {#each row.getVisibleCells() as cell}
                    <td>
                        <FlexRender
                            content={cell.column.columnDef.cell}
                            context={cell.getContext()}
                        />
                    </td>
                {/each}
            </tr>
        {/each}
    </tbody>
</table>

<h2>Questions you may have</h2>

<hr />

<h3>Why not just use the alpha (v9) branch?</h3>

<p>
    Alpha is under development. It changes very frequently, and therefore breaks very frequently.
    While things are so volatile, maintainers and contributors can't be expected to support it.
</p>

<p>
    This applies to both <code>@tanstack/table-core</code> and <code>@tanstack/table-svelte</code>.
</p>

<h3>How can I get help?</h3>

<p>
    You can ask questions by submitting an issue in the <a href={PUBLIC_GITHUB_REPO_URL}
        >GitHub repository</a
    >. Please try and figure things out on your own first, though.
</p>

<p style="font-weight: bold;">
    Please also DO NOT submit an issue that you have with this project on the official TanStack
    Table repository. ONLY submit an issue there if you're sure that it's a problem with their
    library, and not this project or your own implementation.
</p>

<style>
    h2 {
        margin-top: 2rem;
    }
</style>
