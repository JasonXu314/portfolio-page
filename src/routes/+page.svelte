<script lang="ts">
	import { onMount } from 'svelte';
	import Article from '../components/article.svelte';
	import DownButton from '../components/down-button.svelte';
	import ExternalLink from '../components/external-link.svelte';
	import Typewriter from '../components/typewriter.svelte';
	import { typewriterTimeout } from '../stores';
	import { sleep } from '../utils';

	type Writer = (str: string) => Promise<void>;

	let setHeader: Writer,
		setSub: Writer,
		headFocus = true,
		subFocus = false;

	async function write(writer: Writer, str: string, delay: number): Promise<void> {
		await writer(str);
		return sleep(delay);
	}

	async function playAnimation() {
		await write(setHeader, 'Hi!', 350);
		await write(setHeader, "I'm Jason Xu", 750);
		await write(setHeader, 'I', 350);
		await write(setHeader, 'I.', 350);
		await write(setHeader, 'I..', 350);
		await write(setHeader, 'I...', 750);
		await write(setHeader, 'I make things!', 750);
		subFocus = true;
		await sleep(500);
		typewriterTimeout.set(50);
		await Promise.all([write(setHeader, 'I make cool things!', 750), write(setSub, 'idk im not very creative lol', 750)]);
		headFocus = false;
		await write(setSub, 'click the arrow to check them out', 1500);
		headFocus = true;
		subFocus = true;
		typewriterTimeout.set(25);
		await Promise.all([write(setHeader, '', 0), write(setSub, '', 0)]);
		await sleep(200);
		subFocus = false;
		await sleep(750);
		typewriterTimeout.set(125);

		requestAnimationFrame(playAnimation);
	}

	onMount(playAnimation);
</script>

<svelte:head>
	<title>Jason Xu</title>
	<meta name="author" content="Jason Xu" />
	<meta name="description" content="About me" />
	<meta name="keywords" content="jason xu,portfolio,resume" />
	<meta property="og:locale" content="en_US" />
	<meta property="og:type" content="website" />
	<meta property="og:title" content="Home" />
	<meta property="og:description" content="About me" />
	<meta property="og:url" content="https://jasonxu.dev" />
	<meta property="og:site_name" content="Jason Xu" />
	<meta property="og:image" content="https://jasonxu.dev/logo.png" />
	<meta name="theme-color" content="#1897C2" />
</svelte:head>

<main class="container">
	<section id="animation">
		<Typewriter bind:setText={setHeader} element="h1" focused={headFocus} />
		<Typewriter bind:setText={setSub} focused={subFocus} />
		<DownButton target="#projects" />
	</section>
	<section id="projects">
		<hgroup>
			<h1>Some assorted personal projects</h1>
			<h3>Full list can be found on my <a href="https://github.com/JasonXu314" target="_blank" rel="noreferrer noopener">GitHub</a></h3>
		</hgroup>
		<div class="grid">
			<Article links={{ Repo: 'https://github.com/JasonXu314/g--' }}>
				<svelte:fragment slot="headings">
					<h2>G--</h2>
					<h4>A swiss army knife for C++</h4>
				</svelte:fragment>
				<p>
					A project I started during second semester of freshman year to simplify compilation for grading homeworks. Given an entry file, analyzes
					<code>#include</code>
					statements to automatically find required sources. Also includes utility commands for automatically running the compiled executable, debugging,
					and running code coverage.
				</p>
			</Article>
			<Article
				links={{
					Website: 'https://cstk.jasonxu.dev/',
					'Frontend Repo': 'https://github.com/JasonXu314/er-diagram',
					'Backend Repo': 'https://github.com/JasonXu314/er-backend'
				}}
			>
				<svelte:fragment slot="headings">
					<h2>CSTK</h2>
					<h4>A collection of tools (including G--) for making CS at S&T easier</h4>
				</svelte:fragment>
				<p>
					A collection of various projects written throughout the classes I have taken at S&T with the objective of filling vacancies in tooling, or
					improving upon UX of existing tools. Technologies used vary depending on the targetted environment, but mainly consist of
					<ExternalLink href="https://www.svelteui.org/">SvelteUI</ExternalLink>
					for fullstack applications, and C++ for CLI applications.
				</p>
			</Article>
		</div>
		<div class="grid">
			<Article
				links={{
					Website: 'https://feaux-s.vercel.app/',
					Repo: 'https://github.com/JasonXu314/feaux-s'
				}}
			>
				<svelte:fragment slot="headings">
					<h2>FeauxS</h2>
					<h4>A very <b><em>very</em></b> <span data-tooltip="Fake, false, or otherwise suspicious.">sussy</span> OS</h4>
				</svelte:fragment>
				<p>
					Final project for CS3800 (Intro to Operating Systems) doing a little bit of everything:
					<ExternalLink href="https://github.com/JasonXu314/feaux-s/blob/master/feaux-s/machine.cpp">hardware simulation</ExternalLink>,
					<ExternalLink href="https://github.com/JasonXu314/feaux-s/blob/master/feaux-s/os.cpp#L44">scheduling</ExternalLink>, and
					<ExternalLink href="https://github.com/JasonXu314/feaux-s/blob/master/feaux-s/main.cpp#LL288">memory allocation</ExternalLink>, all wrapped
					up into one neat little GUI application.
				</p>
			</Article>
			<Article>
				<svelte:fragment slot="headings">
					<h2>More to come soon!</h2>
					<h4>I do way too much of this for my own good...</h4>
				</svelte:fragment>
				<p>I am constantly exploring new technologies and making projects to improve my skills!</p>
				<svelte:fragment slot="footer">Keep an eye on my pinned repos for stuff :D</svelte:fragment>
			</Article>
		</div>
		<DownButton target="#more" />
	</section>
	<section id="more">
		<hgroup>
			<h1>More coming soon!</h1>
			<h3>* Probably</h3>
		</hgroup>
	</section>
</main>

<style>
	section {
		position: relative;
		min-height: calc(100vh - var(--block-spacing-vertical));
		padding-bottom: 150px;
	}
</style>
