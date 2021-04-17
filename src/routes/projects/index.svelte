<script context="module">
	// fetch post logic
	const posts = import.meta.glob('./*.md');

	let body = [];
	for (const path in posts) {
		body.push(posts[path]().then(({ metadata }) => metadata));
	}

	/**
	 * @type {import('@sveltejs/kit').Load}
	 */
	export async function load({ page, fetch }) {
		const posts = await Promise.all(body);

		// console.log(body);

		return {
			props: {
				posts
			}
		};
	}

	// src:
	// https://megzari.com/blog/about_this_site
</script>

<script>
	export let posts;
</script>

<ul>
	{#each posts as post}
		{#if post}
			<li>
				<a href={`./projects/${post.slug}`}>{post.title}</a>
			</li>
		{/if}
	{/each}
</ul>
