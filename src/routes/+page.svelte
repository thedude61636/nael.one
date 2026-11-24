<script lang="ts">
	import { onMount } from 'svelte';

	// Svelte 5 Runes for reactive state
	let currentSlide = $state(0);
	let copiedEmail = $state(false);
	let openSystems = $state<Record<number, boolean>>({});
	let openGovernance = $state<Record<number, boolean>>({});
	let isMatrixActive = $state(false);

	let canvasRef: HTMLCanvasElement | null = $state(null);
	let matrixAnimId: number | null = null;
	let lastFrameTime = 0;

	// Toggle system accordion
	function toggleSystem(id: number) {
		openSystems[id] = !openSystems[id];
	}

	// Toggle governance accordion
	function toggleGov(id: number) {
		openGovernance[id] = !openGovernance[id];
	}

	// Scroll to specific slide
	function scrollToSlide(index: number) {
		if (typeof document !== 'undefined') {
			const slides = document.querySelectorAll('.screen-slide');
			if (slides[index]) {
				slides[index].scrollIntoView({ behavior: 'smooth' });
				currentSlide = index;
			}
		}
	}

	// Email copy helper
	function copyEmail() {
		if (typeof navigator !== 'undefined' && navigator.clipboard) {
			navigator.clipboard.writeText('me@nael.one').then(() => {
				copiedEmail = true;
				setTimeout(() => {
					copiedEmail = false;
				}, 2000);
			});
		}
	}

	interface MatrixCell {
		char: string;
		swap: number;
	}

	interface MatrixStream {
		x: number;
		headY: number;
		speed: number;
		length: number;
		isBack: boolean;
		cells: MatrixCell[];
	}

	// Potato Matrix Rain Engine
	function triggerPotatoMatrix(e?: Event) {
		if (e) e.preventDefault();
		isMatrixActive = true;

		// Give DOM a frame to unhide canvas
		setTimeout(() => {
			if (!canvasRef) return;
			const canvas = canvasRef;
			const width = window.innerWidth;
			const height = window.innerHeight;
			canvas.width = width;
			canvas.height = height;
			const ctx = canvas.getContext('2d', { alpha: false });
			if (!ctx) return;

			const words = [
				'POTATO', 'FRIES', 'MASH', 'TOTS', 'HASHBROWN', 'GNOCCHI',
				'WEDGES', 'LATKE', 'POUTINE', 'BRAVAS', 'SALAD', 'CROQUETTE',
				'DAUPHINOISE', 'ROSTI', 'KNISH', 'HARRA', 'SAMOSA', 'WAFFLEFRIES', 'ALOO'
			];

			function getRandomChar() {
				const word = words[Math.floor(Math.random() * words.length)];
				return word[Math.floor(Math.random() * word.length)];
			}

			const colWidth = 28;
			const rowHeight = 22;
			const colCount = Math.floor(width / colWidth);
			const rowCount = Math.floor(height / rowHeight) + 1;

			const streams: MatrixStream[] = [];
			for (let i = 0; i < colCount; i++) {
				const isBack = i % 2 === 1;
				streams.push({
					x: i * colWidth + 8,
					headY: Math.random() * -rowCount,
					speed: isBack ? 0.38 : 0.72,
					length: 24 + Math.floor(Math.random() * 28), // 24-52 rows long trails
					isBack: isBack,
					cells: Array.from({ length: rowCount }, () => ({
						char: Math.random() < 0.1 ? '🥔' : getRandomChar(),
						swap: Math.floor(Math.random() * 60)
					}))
				});
			}

			function render(timestamp: number) {
				if (!isMatrixActive || !ctx) return;

				if (timestamp - lastFrameTime < 32) {
					matrixAnimId = requestAnimationFrame(render);
					return;
				}
				lastFrameTime = timestamp;

				ctx.fillStyle = '#020305';
				ctx.fillRect(0, 0, width, height);

				for (let s = 0; s < streams.length; s++) {
					const stream = streams[s];
					stream.headY += stream.speed;

					if (stream.headY - stream.length > rowCount) {
						stream.headY = -Math.floor(Math.random() * 12);
						stream.length = 24 + Math.floor(Math.random() * 28);
					}

					const currentHead = Math.floor(stream.headY);
					ctx.font = stream.isBack ? "11px 'Space Mono', monospace" : "14px 'Space Mono', monospace";

					for (let r = 0; r < rowCount; r++) {
						const dist = currentHead - r;
						if (dist >= 0 && dist < stream.length) {
							const cell = stream.cells[r];

							cell.swap--;
							if (cell.swap <= 0) {
								cell.char = Math.random() < 0.1 ? '🥔' : getRandomChar();
								cell.swap = 40 + Math.floor(Math.random() * 80);
							}

							const posY = r * rowHeight + rowHeight;

							if (cell.char === '🥔') {
								ctx.fillStyle = '#ffffff';
							} else if (dist === 0) {
								ctx.fillStyle = stream.isBack ? '#a3e635' : '#ffffff';
							} else if (dist < 4) {
								ctx.fillStyle = stream.isBack ? '#22c55e' : '#4ade80';
							} else if (dist < 12) {
								ctx.fillStyle = stream.isBack ? '#15803d' : '#16a34a';
							} else {
								const fade = Math.max(0, 1 - dist / stream.length);
								const g = Math.floor(120 * fade) + 20;
								ctx.fillStyle = stream.isBack ? `rgb(2, ${Math.floor(g * 0.6)}, 15)` : `rgb(5, ${g}, 25)`;
							}

							ctx.fillText(cell.char, stream.x, posY);
						}
					}
				}

				matrixAnimId = requestAnimationFrame(render);
			}

			if (matrixAnimId) cancelAnimationFrame(matrixAnimId);
			matrixAnimId = requestAnimationFrame(render);
		}, 20);
	}

	function executePotatoPayload() {
		if (matrixAnimId) {
			cancelAnimationFrame(matrixAnimId);
			matrixAnimId = null;
		}
		isMatrixActive = false;
		if (typeof window !== 'undefined') {
			window.open('https://www.youtube.com/watch?v=sm1qWQUEvfU', '_blank');
		}
	}

	onMount(() => {
		const handleScroll = () => {
			const scrollPos = window.scrollY;
			const height = window.innerHeight;
			currentSlide = Math.min(3, Math.floor((scrollPos + height / 2) / height));
		};

		const handleKeyDown = (e: KeyboardEvent) => {
			if (isMatrixActive) {
				executePotatoPayload();
				return;
			}

			if (e.key >= '1' && e.key <= '4') {
				scrollToSlide(parseInt(e.key) - 1);
				return;
			}

			const scrollPos = window.scrollY;
			const height = window.innerHeight;
			let idx = Math.floor((scrollPos + height / 2) / height);

			if (e.key === 'ArrowDown' || e.key === 'PageDown' || e.key === 'j') {
				e.preventDefault();
				scrollToSlide(Math.min(3, idx + 1));
			}
			if (e.key === 'ArrowUp' || e.key === 'PageUp' || e.key === 'k') {
				e.preventDefault();
				scrollToSlide(Math.max(0, idx - 1));
			}
		};

		window.addEventListener('scroll', handleScroll, { passive: true });
		window.addEventListener('keydown', handleKeyDown);

		return () => {
			window.removeEventListener('scroll', handleScroll);
			window.removeEventListener('keydown', handleKeyDown);
			if (matrixAnimId) cancelAnimationFrame(matrixAnimId);
		};
	});
</script>

<svelte:head>
	<title>NAEL.ONE // SYSTEMS ARCHITECT</title>
</svelte:head>

<!-- FIXED TOP HUD NAVIGATION -->
<header class="fixed top-0 left-0 right-0 z-40 flex justify-between items-center px-6 md:px-24 py-5 md:py-8 bg-gradient-to-b from-black/95 to-transparent pointer-events-none font-mono-swiss text-xs">
	<div class="flex items-center gap-3 pointer-events-auto">
		<span class="w-2 h-2 bg-white inline-block"></span>
		<span class="font-bold tracking-tight text-white text-sm">NAEL.ONE</span>
	</div>

	<div class="flex items-center gap-4 sm:gap-6 pointer-events-auto">
		<a href="https://t.me/TheDude61636" target="_blank" rel="noopener noreferrer" class="text-neutral-500 hover:text-white transition-colors hidden sm:inline">
			telegram // @TheDude61636 ↗
		</a>
		<span class="text-white font-bold tracking-widest text-[11px]">0{currentSlide + 1} / 04</span>
		<button onclick={copyEmail} class="px-3 py-1.5 rounded-full border border-white/20 hover:border-white text-white transition-colors text-xs cursor-pointer">
			<span>{copiedEmail ? 'Copied! ✓' : 'me@nael.one ↗'}</span>
		</button>
	</div>
</header>

<!-- FLOATING SLIDE RAIL INDICATOR -->
<aside class="fixed right-6 md:right-8 top-1/2 -translate-y-1/2 z-40 hidden md:flex flex-col gap-3">
	{#each [0, 1, 2, 3] as idx}
		<button
			onclick={() => scrollToSlide(idx)}
			class="w-1 transition-all duration-300 rounded-sm cursor-pointer {currentSlide === idx ? 'h-12 bg-white' : 'h-6 bg-neutral-800 hover:bg-neutral-600'}"
			aria-label="Slide {idx + 1}"
		></button>
	{/each}
</aside>

<!-- ======================================================== -->
<!-- SLIDE 01: IDENTITY                                       -->
<!-- ======================================================== -->
<section class="screen-slide min-h-screen h-screen snap-start snap-always flex flex-col justify-between px-6 md:px-24 py-16 md:py-20 border-b border-neutral-900 relative">
	<div class="font-mono-swiss text-xs text-neutral-500 tracking-widest pt-4">
		01 — IDENTITY // SYSTEMS ARCHITECT
	</div>

	<div class="my-auto max-w-5xl">
		<h1 class="text-5xl sm:text-7xl md:text-8xl lg:text-9xl font-black tracking-tighter uppercase leading-[0.88] mb-8">
			The one<br />
			and only<br />
			<span class="text-white">Nael.</span>
		</h1>

		<p class="text-xl sm:text-2xl md:text-3xl font-medium text-neutral-400 max-w-2xl leading-snug mb-12">
			I fix problems. Mobile, desktop, web, and distributed systems. Grounded in first-principles software architecture.
		</p>

		<!-- Scope Footnote -->
		<div class="flex flex-wrap gap-x-8 gap-y-2 font-mono-swiss text-xs text-neutral-500 border-t border-neutral-900 pt-6">
			<div><strong class="text-white">12+</strong> GOVERNORATES NATIONWIDE</div>
			<div><strong class="text-white">ARDH AL YAMAMA</strong> PETROLEUM INFRASTRUCTURE</div>
			<div><strong class="text-white">BIOMETRICS & UHF RFID</strong> LOW-LEVEL</div>
			<div><strong class="text-white">#1</strong> GOOGLE HASH CODE IRAQ</div>
		</div>
	</div>

	<div class="flex justify-between items-center font-mono-swiss text-xs text-neutral-500 pb-2">
		<div>BAGHDAD, IRAQ</div>
		<div class="animate-bounce">SCROLL OR PRESS [2] ↓</div>
	</div>
</section>

<!-- ======================================================== -->
<!-- SLIDE 02: PRODUCTION SYSTEMS (7 SEPARATE ACCORDIONS)     -->
<!-- ======================================================== -->
<section class="screen-slide min-h-screen h-screen snap-start snap-always flex flex-col justify-between px-6 md:px-24 py-16 md:py-20 border-b border-neutral-900 relative overflow-y-auto">
	<div class="font-mono-swiss text-xs text-neutral-500 tracking-widest pt-4 flex justify-between">
		<span>02 — PRODUCTION SYSTEMS</span>
		<span class="text-white hidden sm:inline">CLICK TO EXPAND</span>
	</div>

	<div class="my-auto max-w-4xl w-full">
		<h2 class="text-4xl sm:text-6xl md:text-7xl font-black tracking-tighter uppercase leading-[0.9] mb-8">
			I build real<br />
			systems.
		</h2>

		<div class="w-full">
			<!-- System 01: Fuel Station Automation & Management -->
			<div class="border-t border-neutral-900 py-3.5 cursor-pointer hover:border-neutral-700 transition-colors">
				<button onclick={() => toggleSystem(1)} class="w-full text-left flex justify-between items-center text-base sm:text-lg font-bold text-white cursor-pointer">
					<span>01 // National Fuel Station Automation & Supervisory Network (Ardh Al Yamama)</span>
					<span class="font-mono-swiss text-sm text-neutral-500 transition-transform duration-200 {openSystems[1] ? 'rotate-45' : ''}">+</span>
				</button>
				{#if openSystems[1]}
					<div class="mt-3 text-sm text-neutral-400 font-sans leading-relaxed">
						<p class="mb-2">
							Architected on-site desktop terminal automation clients and provincial supervisory platforms operating in retail fuel stations across all 12+ Iraqi governorates with <a href="https://yamama.tech" target="_blank" rel="noopener noreferrer" class="text-white underline hover:text-neutral-300">Ardh Al Yamama</a>. Direct serial dispenser pump integration, real-time provincial telemetry, and zero-downtime automated deployment pipelines. Also engineered dedicated cross-platform management applications in <strong>Flutter (Mobile & Web)</strong> enabling executive leadership and operations managers to inspect live station dispensing data and performance telemetry on the go.
						</p>
						<div class="font-mono-swiss text-xs text-white">C# WPF • FLUTTER (MOBILE & WEB) • MVVM / DI • SERIAL HARDWARE PROTOCOLS • INNO SETUP • 12+ GOVERNORATES</div>
					</div>
				{/if}
			</div>

			<!-- System 02: OPDC Tanker Logistics -->
			<div class="border-t border-neutral-900 py-3.5 cursor-pointer hover:border-neutral-700 transition-colors">
				<button onclick={() => toggleSystem(2)} class="w-full text-left flex justify-between items-center text-base sm:text-lg font-bold text-white cursor-pointer">
					<span>02 // Petroleum Tanker Fleet Logistics (Ardh Al Yamama for OPDC)</span>
					<span class="font-mono-swiss text-sm text-neutral-500 transition-transform duration-200 {openSystems[2] ? 'rotate-45' : ''}">+</span>
				</button>
				{#if openSystems[2]}
					<div class="mt-3 text-sm text-neutral-400 font-sans leading-relaxed">
						<p class="mb-2">
							Engineered the enterprise petroleum distribution and tanker tracking platform with <a href="https://yamama.tech" target="_blank" rel="noopener noreferrer" class="text-white underline hover:text-neutral-300">Ardh Al Yamama</a> for the Oil Products Distribution Company (OPDC / Ministry of Oil). Built in Blazor WebAssembly (.NET 8), integrating desktop TWAIN document scanners to digitize paper manifests inside browser sessions, with automated thermal QR waybill printing and high-volume Excel exports.
						</p>
						<div class="font-mono-swiss text-xs text-white">BLAZOR WASM (.NET 8) • TWAIN HARDWARE SCANNING • THERMAL QR • CLOSEDXML</div>
					</div>
				{/if}
			</div>

			<!-- System 03: GPalms Biometrics Socket Daemon -->
			<div class="border-t border-neutral-900 py-3.5 cursor-pointer hover:border-neutral-700 transition-colors">
				<button onclick={() => toggleSystem(3)} class="w-full text-left flex justify-between items-center text-base sm:text-lg font-bold text-white cursor-pointer">
					<span>03 // Biometric Facial Recognition Socket Daemon (GPalms)</span>
					<span class="font-mono-swiss text-sm text-neutral-500 transition-transform duration-200 {openSystems[3] ? 'rotate-45' : ''}">+</span>
				</button>
				{#if openSystems[3]}
					<div class="mt-3 text-sm text-neutral-400 font-sans leading-relaxed">
						<p class="mb-2">
							High-throughput async socket daemons communicating over raw TCP with facial recognition camera hardware. Engineered with a .NET 10 backend, reactive Svelte supervisory dashboard, Fluent UI design system, and strict release safety verification.
						</p>
						<div class="font-mono-swiss text-xs text-white">.NET 10 • SVELTE • RAW TCP SOCKETS • FLUENT UI • DOCKER</div>
					</div>
				{/if}
			</div>

			<!-- System 04: Industrial UHF RFID Handheld Logistics -->
			<div class="border-t border-neutral-900 py-3.5 cursor-pointer hover:border-neutral-700 transition-colors">
				<button onclick={() => toggleSystem(4)} class="w-full text-left flex justify-between items-center text-base sm:text-lg font-bold text-white cursor-pointer">
					<span>04 // Industrial UHF RFID Handheld Logistics (GPalms)</span>
					<span class="font-mono-swiss text-sm text-neutral-500 transition-transform duration-200 {openSystems[4] ? 'rotate-45' : ''}">+</span>
				</button>
				{#if openSystems[4]}
					<div class="mt-3 text-sm text-neutral-400 font-sans leading-relaxed">
						<p class="mb-2">
							Offline-first asset tracking running on rugged handheld industrial PDAs for warehouse and basement environments with zero network connectivity. Engineered with aggressive client dictionary caching (Blazor WASM PWA) and zero-allocation binary serialization.
						</p>
						<div class="font-mono-swiss text-xs text-white">UHF RFID • RUGGED PDAs • OFFLINE WASM • ZERO-ALLOC SERIALIZATION • LARGEXLSX</div>
					</div>
				{/if}
			</div>

			<!-- System 05: Compound Management & Financial Ledgers -->
			<div class="border-t border-neutral-900 py-3.5 cursor-pointer hover:border-neutral-700 transition-colors">
				<button onclick={() => toggleSystem(5)} class="w-full text-left flex justify-between items-center text-base sm:text-lg font-bold text-white cursor-pointer">
					<span>05 // Multi-Tenant Compound Management & Financial Ledger Platform</span>
					<span class="font-mono-swiss text-sm text-neutral-500 transition-transform duration-200 {openSystems[5] ? 'rotate-45' : ''}">+</span>
				</button>
				{#if openSystems[5]}
					<div class="mt-3 text-sm text-neutral-400 font-sans leading-relaxed">
						<p class="mb-2">
							Enterprise compound and property operations platform managing tenant leases, utilities, and financial audit ledgers. Built with a Go backend using <code class="text-white font-mono text-xs">sqlc</code>, PostgreSQL with strict Row-Level Security (RLS) tenant isolation, integer-based currency invariants (zero float math), and a modern RTL Arabic interface in Svelte 5 (runes) + SvelteKit.
						</p>
						<div class="font-mono-swiss text-xs text-white">GO (GOLANG) • POSTGRESQL RLS • SQLC • SVELTE 5 • SVELTEKIT • DOCKER</div>
					</div>
				{/if}
			</div>

			<!-- System 06: Ardunic Mobile Platform -->
			<div class="border-t border-neutral-900 py-3.5 cursor-pointer hover:border-neutral-700 transition-colors">
				<button onclick={() => toggleSystem(6)} class="w-full text-left flex justify-between items-center text-base sm:text-lg font-bold text-white cursor-pointer">
					<span>06 // Ardunic Mobile Commerce Platform</span>
					<span class="font-mono-swiss text-sm text-neutral-500 transition-transform duration-200 {openSystems[6] ? 'rotate-45' : ''}">+</span>
				</button>
				{#if openSystems[6]}
					<div class="mt-3 text-sm text-neutral-400 font-sans leading-relaxed">
						<p class="mb-2">
							Architected and engineered the production mobile application for Ardunic on Flutter (Dart 3.6+) with Riverpod reactive state management, high-speed faceted search indexing via Typesense, Dio HTTP networking with custom interceptors, camera barcode scanning, Cloudflare Turnstile bot verification, and automated mobile CI/CD deployment pipelines via Codemagic.
						</p>
						<div class="font-mono-swiss text-xs text-white">FLUTTER (DART 3.6+) • RIVERPOD • TYPESENSE • DIO • CODEMAGIC CI/CD • PLAY STORE</div>
					</div>
				{/if}
			</div>

			<!-- System 07: Iraq Mall Headless Commerce -->
			<div class="border-t border-b border-neutral-900 py-3.5 cursor-pointer hover:border-neutral-700 transition-colors">
				<button onclick={() => toggleSystem(7)} class="w-full text-left flex justify-between items-center text-base sm:text-lg font-bold text-white cursor-pointer">
					<span>07 // Iraq Mall Headless Commerce & Restaurant Platform</span>
					<span class="font-mono-swiss text-sm text-neutral-500 transition-transform duration-200 {openSystems[7] ? 'rotate-45' : ''}">+</span>
				</button>
				{#if openSystems[7]}
					<div class="mt-3 text-sm text-neutral-400 font-sans leading-relaxed">
						<p class="mb-2">
							Engineered the modern headless commerce and restaurant management platform for Iraq Mall built with Payload CMS 3, Next.js, and React 19. Implemented structured schema collections, multi-tenant quotation generation, automated media handling with Sharp, and self-hosted deployment automation using Dokku and Dokploy.
						</p>
						<div class="font-mono-swiss text-xs text-white">PAYLOAD CMS 3 • NEXT.JS • REACT 19 • TYPESCRIPT • DOKKU • DOKPLOY • DOCKER</div>
					</div>
				{/if}
			</div>
		</div>
	</div>

	<div class="flex justify-between items-center font-mono-swiss text-xs text-neutral-500 pb-2">
		<div>ZERO CODE SLOP. PURE ARCHITECTURE.</div>
		<div>PRESS [3] FOR GOVERNANCE ↓</div>
	</div>
</section>

<!-- ======================================================== -->
<!-- SLIDE 03: AGENTIC SYSTEMS ARCHITECTURE                   -->
<!-- ======================================================== -->
<section class="screen-slide min-h-screen h-screen snap-start snap-always flex flex-col justify-between px-6 md:px-24 py-16 md:py-20 border-b border-neutral-900 relative">
	<div class="font-mono-swiss text-xs text-neutral-500 tracking-widest pt-4 flex justify-between">
		<span>03 — AGENTIC SYSTEMS ARCHITECTURE</span>
		<span class="text-white hidden sm:inline">CLICK TO EXPAND</span>
	</div>

	<div class="my-auto max-w-4xl w-full">
		<h2 class="text-4xl sm:text-6xl md:text-7xl font-black tracking-tighter uppercase leading-[0.9] mb-4">
			Autonomous speed.<br />
			<span class="text-white">Strict governance.</span>
		</h2>

		<p class="text-lg sm:text-xl text-neutral-400 font-medium max-w-2xl leading-relaxed mb-10">
			I don't manually churn boilerplate code. I operate as a Systems Architect: codifying domain boundaries, schema contracts, and compiler gates while autonomous agents execute with squad velocity.
		</p>

		<div class="w-full">
			<div class="border-t border-neutral-900 py-3.5 cursor-pointer hover:border-neutral-700 transition-colors">
				<button onclick={() => toggleGov(1)} class="w-full text-left flex justify-between items-center text-base sm:text-lg font-bold text-white cursor-pointer">
					<span>01 // Machine-Readable Domain Boundaries (AGENTS.md)</span>
					<span class="font-mono-swiss text-sm text-neutral-500 transition-transform duration-200 {openGovernance[1] ? 'rotate-45' : ''}">+</span>
				</button>
				{#if openGovernance[1]}
					<div class="mt-3 text-sm text-neutral-400 font-sans leading-relaxed">
						<p>Every repository is codified with strict architectural constraints. Agents are constrained to C# .NET conventions, zero-alloc buffer reuse patterns, and strict UI dispatcher thread decoupling.</p>
					</div>
				{/if}
			</div>

			<div class="border-t border-neutral-900 py-3.5 cursor-pointer hover:border-neutral-700 transition-colors">
				<button onclick={() => toggleGov(2)} class="w-full text-left flex justify-between items-center text-base sm:text-lg font-bold text-white cursor-pointer">
					<span>02 // Toolchain & MCP Compiler Gates</span>
					<span class="font-mono-swiss text-sm text-neutral-500 transition-transform duration-200 {openGovernance[2] ? 'rotate-45' : ''}">+</span>
				</button>
				{#if openGovernance[2]}
					<div class="mt-3 text-sm text-neutral-400 font-sans leading-relaxed">
						<p>Agents run automated compiler diagnostics, linters, and verification harnesses via JetBrains MCP servers before building. Zero hallucinations survive compiler diagnostics.</p>
					</div>
				{/if}
			</div>

			<div class="border-t border-b border-neutral-900 py-3.5 cursor-pointer hover:border-neutral-700 transition-colors">
				<button onclick={() => toggleGov(3)} class="w-full text-left flex justify-between items-center text-base sm:text-lg font-bold text-white cursor-pointer">
					<span>03 // Release Safety & Double-Confirmation</span>
					<span class="font-mono-swiss text-sm text-neutral-500 transition-transform duration-200 {openGovernance[3] ? 'rotate-45' : ''}">+</span>
				</button>
				{#if openGovernance[3]}
					<div class="mt-3 text-sm text-neutral-400 font-sans leading-relaxed">
						<p>Strict release protocols: automated tools cannot alter production branches or deploy without explicit dual-step human verification of changes and risk boundaries.</p>
					</div>
				{/if}
			</div>
		</div>
	</div>

	<div class="flex justify-between items-center font-mono-swiss text-xs text-neutral-500 pb-2">
		<div>DISCIPLINE OVER SHORTCUTS</div>
		<div>PRESS [4] FOR PROOF & CONNECT ↓</div>
	</div>
</section>

<!-- ======================================================== -->
<!-- SLIDE 04: PROOF & DIRECT LINE                            -->
<!-- ======================================================== -->
<section class="screen-slide min-h-screen h-screen snap-start snap-always flex flex-col justify-between px-6 md:px-24 py-16 md:py-20 border-b border-neutral-900 relative">
	<div class="font-mono-swiss text-xs text-neutral-500 tracking-widest pt-4">
		04 — TRACK RECORD & DIRECT CONNECTION
	</div>

	<div class="my-auto max-w-5xl">
		<!-- Proof Grid -->
		<div class="grid grid-cols-2 sm:grid-cols-4 gap-6 font-mono-swiss mb-12 border-b border-neutral-900 pb-8">
			<div>
				<div class="text-3xl sm:text-4xl font-black text-white mb-1">#1</div>
				<div class="text-xs font-bold text-white uppercase mb-1">Google Hash Code</div>
				<p class="text-xs text-neutral-500 font-sans">1st Place in Iraq (2018) with d3 team.</p>
			</div>
			<div>
				<div class="text-3xl sm:text-4xl font-black text-white mb-1">60+</div>
				<div class="text-xs font-bold text-white uppercase mb-1">Engineers Mentored</div>
				<p class="text-xs text-neutral-500 font-sans">Codelab Bootcamp instructor & organizer.</p>
			</div>
			<div>
				<div class="text-3xl sm:text-4xl font-black text-white mb-1">PHYSICS</div>
				<div class="text-xs font-bold text-white uppercase mb-1">Applied Physics BSc</div>
				<p class="text-xs text-neutral-500 font-sans">University of Technology, Baghdad.</p>
			</div>
			<div>
				<div class="text-3xl sm:text-4xl font-black text-white mb-1">UNITAR</div>
				<div class="text-xs font-bold text-white uppercase mb-1">Hiroshima Fellow</div>
				<p class="text-xs text-neutral-500 font-sans">Fellowship in Japan & GDG Baghdad Coordinator.</p>
			</div>
		</div>

		<!-- Action Headline -->
		<h2 class="text-4xl sm:text-6xl md:text-7xl lg:text-8xl font-black tracking-tighter uppercase leading-[0.88] mb-8">
			Let's get it<br />
			<span class="text-white">fixed.</span>
		</h2>

		<!-- Direct Copy Action -->
		<div class="mb-10">
			<button onclick={copyEmail} class="group text-2xl sm:text-4xl md:text-5xl font-mono-swiss font-bold text-white hover:text-neutral-300 transition-colors flex items-center gap-4 cursor-pointer">
				<span class="hover-underline">me@nael.one</span>
				<span class="text-xs font-sans font-normal px-3 py-1 bg-white/10 rounded-full group-hover:bg-white group-hover:text-black transition-all">
					{copiedEmail ? 'Copied! ✓' : 'Click to copy'}
				</span>
			</button>
		</div>

		<!-- Links Row -->
		<div class="flex flex-wrap gap-8 font-mono-swiss text-sm">
			<a href="https://t.me/TheDude61636" target="_blank" rel="noopener noreferrer" class="hover-underline text-white">
				TELEGRAM // @TheDude61636 ↗
			</a>
			<a href="https://github.com/thedude61636" target="_blank" rel="noopener noreferrer" class="hover-underline text-white">
				GITHUB // thedude61636 ↗
			</a>
			<a href="https://medium.com/@thedude61636" target="_blank" rel="noopener noreferrer" class="hover-underline text-white">
				MEDIUM // @thedude61636 ↗
			</a>
			<button onclick={triggerPotatoMatrix} class="hover-underline text-neutral-500 hover:text-white transition-colors cursor-pointer text-left" title="Click for Potato Protocol">
				POTATO 🥔 ↗
			</button>
		</div>
	</div>

	<div class="flex justify-between items-center font-mono-swiss text-xs text-neutral-500 pb-2 border-t border-neutral-900 pt-4">
		<div>© 2026 NAEL AL ABBASI • BAGHDAD, IRAQ</div>
		<div>BUILT WITH SVELTEKIT ON GITHUB PAGES</div>
	</div>
</section>

<!-- ======================================================== -->
<!-- POTATO MATRIX RAIN EASTER EGG OVERLAY (ZERO LAG)         -->
<!-- ======================================================== -->
{#if isMatrixActive}
	<div
		id="potato-matrix-overlay"
		class="fixed inset-0 z-50 bg-[#020305] cursor-pointer overflow-hidden flex flex-col justify-between p-6 sm:p-12 select-none"
		onclick={executePotatoPayload}
		role="button"
		tabindex="0"
		onkeydown={(e) => { if (e.key === 'Enter' || e.key === ' ') executePotatoPayload(); }}
	>
		<canvas bind:this={canvasRef} class="absolute inset-0 z-0"></canvas>

		<div class="relative z-10 font-mono-swiss text-xs text-emerald-400 flex justify-between items-center drop-shadow-[0_2px_8px_rgba(0,0,0,0.9)]">
			<span>// POTATO_PROTOCOL_v4.2 // STREAMING ACTIVE</span>
			<span class="animate-pulse">CLICK ANYWHERE TO LAUNCH PAYLOAD ↵</span>
		</div>

		<!-- PURE FLOATING TEXT (NO BOX BACKGROUND) -->
		<div class="relative z-10 my-auto text-center font-mono-swiss px-4">
			<div class="text-4xl sm:text-6xl md:text-7xl lg:text-8xl font-black text-white tracking-tighter uppercase mb-4 leading-none drop-shadow-[0_6px_30px_rgba(0,0,0,0.95)]">
				🥔 ALL YOUR POTATOES<br />BELONG TO US. 🥔
			</div>
			<p class="text-xs sm:text-base text-emerald-400 font-bold tracking-widest uppercase mb-3 drop-shadow-[0_2px_12px_rgba(0,0,0,0.9)]">
				[ CRITICAL CARB OVERLOAD DETECTED • TAP ANYWHERE TO CONSUME ]
			</p>
			<div class="text-[11px] text-neutral-400 font-mono-swiss drop-shadow-[0_2px_8px_rgba(0,0,0,0.9)]">
				CLICK ANYWHERE OR PRESS ANY KEY TO EXECUTE PAYLOAD
			</div>
		</div>

		<div class="relative z-10 font-mono-swiss text-xs text-neutral-500 text-center drop-shadow-[0_2px_8px_rgba(0,0,0,0.9)]">
			TARGET: YOUTUBE // SM1QWQUEVFU
		</div>
	</div>
{/if}
