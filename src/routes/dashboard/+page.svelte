<script lang="ts">
	import ChartLine from 'lucide-svelte/icons/chart-line';
	import Package from 'lucide-svelte/icons/package';
	import House from 'lucide-svelte/icons/house';
	import Bell from 'lucide-svelte/icons/bell';
	import PanelLeftClose from 'lucide-svelte/icons/panel-left-close';
	import PanelLeftOpen from 'lucide-svelte/icons/panel-left-open';

	import MonacoEditor from '$lib/components/editor/MonacoEditor.svelte';
	import { Button } from '$lib/components/ui/button/index.js';

	let editorComponent: MonacoEditor;
	let isSaving = false;
	let isSidebarExpanded = true;

	function toggleSidebar() {
		isSidebarExpanded = !isSidebarExpanded;
	}

	async function handleSave() {
		if (!editorComponent || isSaving) return;

		try {
			isSaving = true;
			const content = editorComponent.getContent();
			if (!content) {
				alert('Cannot save empty content');
				return;
			}

			//await saveGenesis(content);
			alert('Genesis saved successfully!');
		} catch (error) {
			console.error('Error saving genesis:', error);
			alert('Failed to save genesis');
		} finally {
			isSaving = false;
		}
	}
</script>

<div class="grid min-h-screen w-full">
	<div class="flex">
		<div
			class={`relative border-r bg-muted/40 transition-all duration-300 ${isSidebarExpanded ? 'w-[220px] md:w-[280px]' : 'w-[60px]'}`}
		>
			<div class="flex h-full max-h-screen flex-col gap-2">
				<!-- Sidebar Header: Adjust visibility based on isSidebarExpanded -->
				<div
					class={`flex h-14 items-center border-b px-2 lg:h-[60px] ${isSidebarExpanded ? '' : 'hidden'}`}
				>
					<a href="/" class="flex items-center gap-2 font-bold">
						<span class={isSidebarExpanded ? '' : 'hidden'}>Genesis</span>
					</a>
					<div class="ml-auto flex gap-1">
						{#if isSidebarExpanded}{/if}
					</div>
				</div>
				<!-- Navigation Menu: Grows when header is hidden -->
				<div class="flex-1 transition-all duration-300">
					<nav class="grid items-start px-2 text-sm font-medium">
						<a
							href="##"
							class="flex items-center gap-3 rounded-lg px-3 py-2 text-muted-foreground transition-all hover:text-primary"
							title="Dashboard"
						>
							<House class="h-4 w-4" />
							<span class={isSidebarExpanded ? '' : 'hidden'}>Dashboard</span>
						</a>
						<a
							href="##"
							class="flex items-center gap-3 rounded-lg bg-muted px-3 py-2 text-primary transition-all hover:text-primary"
							title="Products"
						>
							<Package class="h-4 w-4" />
							<span class={isSidebarExpanded ? '' : 'hidden'}>Products</span>
						</a>
						<a
							href="##"
							class="flex items-center gap-3 rounded-lg px-3 py-2 text-muted-foreground transition-all hover:text-primary"
							title="Analytics"
						>
							<ChartLine class="h-4 w-4" />
							<span class={isSidebarExpanded ? '' : 'hidden'}>Analytics</span>
						</a>
					</nav>
				</div>
				<div class="mt-auto p-4"></div>
			</div>
			<!-- Toggle Button for Sidebar -->
			<Button
				variant="outline"
				size="icon"
				class="absolute -right-4 top-3 h-8 w-8 rounded-full bg-background shadow-md transition-all duration-300"
				on:click={toggleSidebar}
			>
				{#if isSidebarExpanded}
					<PanelLeftClose class="h-4 w-4" />
				{:else}
					<PanelLeftOpen class="h-4 w-4" />
				{/if}
				<span class="sr-only">Toggle sidebar</span>
			</Button>
		</div>

		<div class="flex flex-1 flex-col">
			<main class="flex flex-1 flex-col pt-4">
				<MonacoEditor
					bind:this={editorComponent}
					value="~This is a regular atom with [an enveloped content] in it

~Another atom with [multiple] [enveloped] [contents]"
					language="genesis"
					theme="genesis-theme"
				/>
			</main>
		</div>
	</div>
</div>
