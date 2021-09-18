<script context="module">
	const mainNavLinks = [
		{ label: 'Dashboard', href: '/' },
		{ label: 'NPCs', href: '/npcs' },
		{ label: 'Places', href: '/places' },
		{ label: 'General Knowledge', href: '/generalknowledge' }
	];

	const profileNavLinks = [
		{ label: 'Profile', href: '/profile' },
		{ label: 'Settings', href: '/settings' },
		{ label: 'Logout', href: '/' }
	];
</script>

<script>
	import { onMount } from 'svelte';
	import { scale } from 'svelte/transition';
	import { page } from '$app/stores';

	$: currentPage = '/' + $page.path.split('/')[1];

	let profileMenu;
	let profileMenuButton;
	let profileNavIsOpen = false;
	let mobileNavIsOpen = false;

	const toggleMobileNav = () => {
		mobileNavIsOpen = !mobileNavIsOpen;
		if (mobileNavIsOpen) {
			profileNavIsOpen = false;
		}
	};

	const toggleProfileNav = () => {
		profileNavIsOpen = !profileNavIsOpen;
		if (profileNavIsOpen) {
			mobileNavIsOpen = false;
		}
	};

	onMount(() => {
		const handleOutsideClick = (event) => {
			if (
				profileNavIsOpen &&
				!profileMenu.contains(event.target) &&
				!profileMenuButton.contains(event.target)
			) {
				profileNavIsOpen = false;
			}
		};

		const handleEscape = (event) => {
			if (event.key === 'Escape') {
				profileNavIsOpen = false;
				mobileNavIsOpen = false;
			}
		};

		document.addEventListener('click', handleOutsideClick, false);
		document.addEventListener('keyup', handleEscape, false);

		return () => {
			document.removeEventListener('click', handleOutsideClick, false);
			document.removeEventListener('keyup', handleEscape, false);
		};
	});
</script>

<!--            The Header is arranged as follows           -->
<!--   MOBILE    [profile]      [icon]      [hamburger]     -->
<!--   DESKTOP   [( icon, links )]            [profile]     -->

<!--   This is accomplished with four sections:             -->
<!--     1. DESKTOP ONLY (icon, links)                      -->
<!--     2. ALL profile + profile dropdown                  -->
<!--     3. MOBILE ONLY icon                                -->
<!--     4. MOBILE ONLY hamburger                           -->

<!--   Mobile menu is at bottom                             -->

<nav class="bg-gray-800">
	<div class="max-w-7xl mx-auto px-2 sm:px-6 lg:px-8">
		<div class="relative flex items-center justify-between h-16">
			<!-- 1. DESKTOP ONLY (icon, links) -->
			<div
				class="hidden sm:flex flex-1 flex-row items-center justify-center sm:items-stretch sm:justify-start"
			>
				<div class="flex-shrink-0 flex items-center">
					<img
						class="block lg:hidden h-8 w-auto"
						src="https://tailwindui.com/img/logos/workflow-mark-indigo-500.svg"
						alt="Workflow"
					/>
					<img
						class="hidden lg:block h-8 w-auto"
						src="https://tailwindui.com/img/logos/workflow-logo-indigo-500-mark-white-text.svg"
						alt="Workflow"
					/>
				</div>
				<div class="hidden sm:block sm:ml-6">
					<div class="flex space-x-4">
						<!-- Current: "bg-gray-900 text-white", Default: "text-gray-300 hover:bg-gray-700 hover:text-white" -->
						{#each mainNavLinks as { label, href }}
							{#if currentPage === href}
								<a
									{href}
									sveltekit:prefetch
									class="bg-gray-900 text-white px-3 py-2 rounded-md text-sm font-medium"
									aria-current="page">{label}</a
								>
							{:else}
								<a
									{href}
									sveltekit:prefetch
									class="text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium"
									>{label}</a
								>
							{/if}
						{/each}
					</div>
				</div>
			</div>
			<!-- 2. profile + profile dropdown -->
			<div class="flex flex-col content-center pr-2">
				<!-- Profile dropdown -->
				<div class="ml-3">
					<div>
						<button
							type="button"
							class="bg-gray-800 flex text-sm rounded-full focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-gray-800 focus:ring-white"
							id="user-menu-button"
							aria-haspopup="true"
							aria-expanded={profileNavIsOpen}
							on:click={toggleProfileNav}
							bind:this={profileMenuButton}
						>
							<span class="sr-only">Open user menu</span>
							<img
								class="h-8 w-8 rounded-full"
								src="https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80"
								alt=""
							/>
						</button>
					</div>
				</div>
				<!-- Profile Menu -->
				<div class="relative">
					<div
						class="absolute dropdown {profileNavIsOpen &&
							'active'} left-0 top-4 sm:left-auto sm:right-0 w-44 sm:w-80 rounded-md shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none"
						role="menu"
						aria-orientation="vertical"
						aria-labelledby="user-menu-button"
						bind:this={profileMenu}
					>
						{#each profileNavLinks as { label, href }}
							<a
								{href}
								sveltekit:prefetch
								class="bg-gray-100 hover:bg-gray-300 block px-6 py-4 text-base font-medium text-gray-700 {href ===
									currentPage && 'font-bold'}"
								role="menuitem"
								id="user-menu-item-0"
								on:click={toggleProfileNav}>{label}</a
							>
						{/each}
					</div>
				</div>
			</div>
			<!-- 3. MOBILE ONLY icon -->
			<div class="flex-shrink-0 flex items-center sm:hidden">
				<img
					class="block lg:hidden h-8 w-auto"
					src="https://tailwindui.com/img/logos/workflow-mark-indigo-500.svg"
					alt="Workflow"
				/>
				<img
					class="hidden lg:block h-8 w-auto"
					src="https://tailwindui.com/img/logos/workflow-logo-indigo-500-mark-white-text.svg"
					alt="Workflow"
				/>
			</div>
			<!-- 4. MOBILE ONLY hamburger -->
			<div class="flex items-center sm:hidden">
				<!-- Mobile menu button-->
				<button
					type="button"
					class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-white hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-white"
					aria-controls="mobile-menu"
					aria-expanded="false"
					on:click={toggleMobileNav}
				>
					<span class="sr-only">Open main menu</span>

					<!--
            Icon when menu is open.

            Heroicon name: outline/x

            Menu open: "block", Menu closed: "hidden"
          -->
					<svg
						class="{mobileNavIsOpen ? 'block' : 'hidden'} h-6 w-6"
						xmlns="http://www.w3.org/2000/svg"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
						aria-hidden="true"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M6 18L18 6M6 6l12 12"
						/>
					</svg>

					<!--
            Icon when menu is closed.

            Heroicon name: outline/menu

            Menu open: "hidden", Menu closed: "block"
          -->
					<svg
						class="{mobileNavIsOpen ? 'hidden' : 'block'} h-6 w-6"
						xmlns="http://www.w3.org/2000/svg"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
						aria-hidden="true"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M4 6h16M4 12h16M4 18h16"
						/>
					</svg>
				</button>
			</div>
		</div>
	</div>

	<!-- Mobile Menu -->
	<div class="sm:hidden dropdown {mobileNavIsOpen && 'active'} bg-gray-800" id="mobile-menu">
		<div class="px-2 pt-2 h-screen w-screen text-center">
			{#each mainNavLinks as { label, href }}
				<a
					{href}
					sveltekit:prefetch
					on:click={toggleMobileNav}
					class="block px-3 py-4 rounded-md text-base font-medium {currentPage === href
						? 'bg-gray-900 text-white'
						: 'text-gray-300'}"
					aria-current={currentPage === href ? 'page' : false}
				>
					{label}
				</a>
			{/each}
		</div>
	</div>
</nav>

<style>
	.dropdown {
		position: absolute;
		height: auto;
		max-height: 0;
		transition: max-height 0.25s ease-in-out;
		overflow: hidden;
	}
	.dropdown.active {
		max-height: 100vh;
	}
</style>
