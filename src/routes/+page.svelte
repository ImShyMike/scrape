<script lang="ts">
	import { slide, fade } from 'svelte/transition';
	import hackClubFlag from '$lib/assets/hack-club-flag.svg';
	import scrolldown from '$lib/assets/scrolldown.svg';
	import cobweb from '$lib/assets/cobweb.png';
	import spiderPlushie from '$lib/assets/cute-spider-plushie.png';
	import { faqs } from '$lib/faqs';

	let openIndex = $state(0);
	let greet = $state(false);
	let typed = $state('');

	const greetMessage = 'Hi!! Did you know that im the prize for this YSWS? :D';

	$effect(() => {
		if (!greet) {
			typed = '';
			return;
		}
		let i = 0;
		const id = setInterval(() => {
			i++;
			typed = greetMessage.slice(0, i);
			if (i >= greetMessage.length) clearInterval(id);
		}, 40);
		return () => clearInterval(id);
	});

	function toggle(i: number) {
		openIndex = openIndex === i ? -1 : i;
	}
</script>

<a
	href="https://hackclub.com"
	target="_blank"
	rel="noopener noreferrer"
	class="absolute top-6 left-0 z-10"
>
	<img src={hackClubFlag} alt="Hack Club" class="w-70 sm:w-85" />
</a>

<img
	src={cobweb}
	alt="cobweb"
	class="pointer-events-none absolute top-0 right-0 w-40 -scale-x-100 sm:w-lg"
/>

<section
	class="relative flex min-h-screen flex-col items-center justify-center gap-0 px-6 text-center"
>
	<h1 class="font-title text-[30vw] leading-none text-ink sm:text-[20vw]">scrape</h1>
	<p class="max-w-3xl -translate-y-5 text-2xl text-slate sm:text-4xl">
		make a web scraper, get a spider plushie
	</p>
	<div class="mt-4 flex flex-wrap justify-center gap-6">
		<a
			href="https://forms.hackclub.com/t/2hcaSwewZjus"
			target="_blank"
			rel="noopener noreferrer"
			class="cursor-pointer rounded-2xl bg-yellow px-10 py-5 text-2xl text-black transition-colors duration-150 hover:brightness-90 sm:text-3xl"
		>
			submit
		</a>
		<a
			href="#faq"
			class="cursor-pointer rounded-2xl bg-sage px-10 py-5 text-2xl text-black transition-colors duration-150 hover:brightness-90 sm:text-3xl"
		>
			FAQ
		</a>
	</div>
	<a href="#faq" class="absolute bottom-8 animate-bounce cursor-pointer" aria-label="scroll to FAQ">
		<img src={scrolldown} alt="" class="w-16" />
	</a>
</section>

<section id="faq" class="mx-auto flex max-w-4xl flex-col gap-6 px-6 pt-10 pb-40">
	<h2 class="pb-8 text-center font-heading text-6xl text-ink sm:text-8xl">FAQ</h2>
	{#each faqs as faq, i (i)}
		<div class="overflow-hidden rounded-[20px] bg-sage">
			<button
				type="button"
				onclick={() => toggle(i)}
				class="w-full cursor-pointer px-8 py-5 text-left"
			>
				<!-- eslint-disable-next-line svelte/no-at-html-tags -->
				<span class="text-2xl text-ink sm:text-3xl">&gt; {@html faq.q}</span>
			</button>
			{#if openIndex === i}
				<div transition:slide={{ duration: 200 }}>
					<!-- eslint-disable-next-line svelte/no-at-html-tags -->
					<p class="px-8 pb-6 text-xl text-slate sm:text-2xl">{@html faq.a}</p>
				</div>
			{/if}
		</div>
	{/each}
</section>

<footer class="bg-ink px-6 py-12 text-center text-cream">
	<nav class="mb-6 flex flex-wrap justify-center gap-x-8 gap-y-3 text-lg">
		<a
			href="https://hackclub.com"
			target="_blank"
			rel="noopener noreferrer"
			class="cursor-pointer hover:underline">Hack Club</a
		>
		<a
			href="https://hackclub.com/clubs"
			target="_blank"
			rel="noopener noreferrer"
			class="cursor-pointer hover:underline">Clubs</a
		>
		<a
			href="https://hackathons.hackclub.com"
			target="_blank"
			rel="noopener noreferrer"
			class="cursor-pointer hover:underline">Hackathons</a
		>
		<a
			href="https://slack.hackclub.com"
			target="_blank"
			rel="noopener noreferrer"
			class="cursor-pointer hover:underline">Slack</a
		>
		<a
			href="https://forms.hackclub.com/bounty"
			target="_blank"
			rel="noopener noreferrer"
			class="cursor-pointer hover:underline">Bounty</a
		>
		<a
			href="https://hackclub.com/privacy-and-terms"
			target="_blank"
			rel="noopener noreferrer"
			class="cursor-pointer hover:underline">Privacy &amp; Terms</a
		>
	</nav>
	<p class="text-sage">
		<span class="font-title">scrape</span> is a YSWS program run by
		<a
			href="https://hackclub.com"
			target="_blank"
			rel="noopener noreferrer"
			class="cursor-pointer underline">Hack Club</a
		>
		and
		<a
			href="https://github.com/ImShyMike"
			target="_blank"
			rel="noopener noreferrer"
			class="cursor-pointer underline">@miggy</a
		>
		with &lt;3
	</p>
</footer>

<div class="fixed -bottom-2 left-2 z-50 sm:-bottom-1 sm:left-9">
	{#if greet}
		<div
			transition:fade={{ duration: 150 }}
			class="absolute bottom-[125%] left-[140%] mb-2 w-64 max-w-[70vw] -translate-x-1/2 rounded-xl bg-ink px-4 py-3 text-left text-lg text-cream sm:left-[90%]"
		>
			{typed}
		</div>
	{/if}
	<button
		type="button"
		onclick={() => (greet = !greet)}
		class="cursor-pointer"
		aria-label="say hi to the spider"
	>
		<img
			src={spiderPlushie}
			alt="cute spider plushie"
			class="w-24 rotate-14 sm:w-32 sm:scale-125"
		/>
	</button>
</div>
