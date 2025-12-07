<script lang="ts">
	import { onMount } from 'svelte';

	// Icons
	const AppleIcon = `<svg viewBox="0 0 24 24" fill="currentColor" class="w-5 h-5"><path d="M17.05 20.28c-.98.95-2.05.8-3.08.35-1.09-.46-2.09-.48-3.24 0-1.44.62-2.2.44-3.06-.35C2.79 15.25 3.51 7.59 9.05 7.31c1.35.07 2.29.74 3.08.74 1.18 0 2.45-1.64 4.54-1.29.84.14 2.11.58 3.12 2.05-2.61 1.58-2.18 5.68.5 6.83-.54 1.4-1.35 2.76-2.29 3.89-.57.83-1.25 1.62-1.95 1.75zm-4.35-14.8c.46-2.28 2.48-3.64 4.54-3.48.33 2.68-2.37 4.96-4.54 3.48z"/></svg>`;
	const ChevronDown = `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-4 h-4"><path d="m6 9 6 6 6-6"/></svg>`;
	const Check = `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-4 h-4"><polyline points="20 6 9 17 4 12"/></svg>`;
	const Play = `<svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor" class="w-4 h-4"><polygon points="5 3 19 12 5 21 5 3"/></svg>`;
	const Mic = `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-4 h-4"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"/><path d="M19 10v2a7 7 0 0 1-14 0v-2"/><line x1="12" x2="12" y1="19" y2="23"/><line x1="8" x2="16" y1="23" y2="23"/></svg>`;
	const Video = `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-4 h-4"><polygon points="23 7 16 12 23 17 23 7"/><rect x="1" y="5" width="15" height="14" rx="2" ry="2"/></svg>`;
	const X = `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-4 h-4"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg>`;
	const Search = `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-4 h-4"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.3-4.3"/></svg>`;
	const Command = `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-6 h-6"><path d="M18 3a3 3 0 0 0-3 3v12a3 3 0 0 0 3 3 3 3 0 0 0 3-3 3 3 0 0 0-3-3H6a3 3 0 0 0-3 3 3 3 0 0 0 3 3 3 3 0 0 0 3-3V6a3 3 0 0 0-3-3 3 3 0 0 0-3 3 3 3 0 0 0 3 3h12a3 3 0 0 0 3-3 3 3 0 0 0-3-3z"/></svg>`;
	const CornerDownLeft = `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-6 h-6"><polyline points="9 10 4 15 9 20"/><path d="M20 4v7a4 4 0 0 1-4 4H4"/></svg>`;
	const ArrowRight = `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-4 h-4"><path d="M5 12h14"/><path d="m12 5 7 7-7 7"/></svg>`;
	const Mail = `<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-4 h-4"><rect width="20" height="16" x="2" y="4" rx="2"/><path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"/></svg>`;
	const Linkedin = `<svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor" class="w-4 h-4"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"/><rect width="4" height="12" x="2" y="9"/><circle cx="4" cy="4" r="2"/></svg>`;

	// FAQ Data
	const faqs = [
		{ question: "Why real-time vs. a regular AI notetaker?", answer: "Real-time assistance allows you to be more present and effective during the meeting itself, rather than just reviewing notes afterwards." },
		{ question: "Who is Cluely for?", answer: "Cluely is for anyone who attends meetings and wants to be more productive, including sales professionals, managers, engineers, and consultants." },
		{ question: "Is Cluely free?", answer: "Cluely offers a free trial, but is a paid service to ensure privacy and high-quality AI processing." },
		{ question: "How is it undetectable in meetings?", answer: "Cluely runs locally on your Mac and captures system audio/video without joining the call as a bot participant." },
		{ question: "What languages and apps are supported?", answer: "We support over 12 languages and work with all major video conferencing platforms like Zoom, Google Meet, and Teams." },
		{ question: "Can I talk to customer support?", answer: "Yes, our support team is available to help you with any questions or issues." },
	];

	let openFaq = -1;
	const toggleFaq = (index: number) => {
		openFaq = openFaq === index ? -1 : index;
	};

</script>

<div class="min-h-screen bg-white font-sans text-gray-900 selection:bg-blue-100">
	<!-- Navbar -->
	<nav class="fixed top-0 left-0 right-0 z-50 flex items-center justify-between px-6 py-4 bg-white/80 backdrop-blur-md border-b border-transparent transition-all duration-300">
		<div class="flex items-center gap-8">
			<a href="/" class="flex items-center gap-2 text-xl font-bold tracking-tight">
				<div class="w-6 h-6 flex items-center justify-center bg-gray-900 text-white rounded-full">
					<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" class="w-4 h-4"><path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/></svg>
				</div>
				Cluely
			</a>
			<div class="hidden md:flex items-center gap-6 text-sm font-medium text-gray-600">
				<a href="#" class="hover:text-gray-900">Pricing</a>
				<a href="#" class="hover:text-gray-900">Enterprise</a>
				<a href="#" class="hover:text-gray-900">Careers</a>
				<a href="#" class="hover:text-gray-900">Blog</a>
			</div>
		</div>
		<button class="hidden md:flex items-center gap-2 bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg text-sm font-medium transition-colors shadow-sm">
			{@html AppleIcon}
			Get for Mac
		</button>
	</nav>

	<!-- Hero Section -->
	<section class="pt-32 pb-20 px-4 md:px-6 relative overflow-hidden">
		<!-- Background Gradient -->
		<div class="absolute inset-0 bg-gradient-to-b from-blue-50 via-white to-white -z-10"></div>
		
		<div class="max-w-5xl mx-auto text-center mb-16">
			<h1 class="text-5xl md:text-7xl font-serif font-medium leading-[1.1] text-blue-500 mb-6">
				#1 AI assistant<br/>
				<span class="text-blue-400">for meetings</span>
			</h1>
			<p class="text-lg md:text-xl text-gray-600 max-w-2xl mx-auto mb-8 leading-relaxed">
				Takes perfect notes, answers questions in real-time, and makes you the most prepared person on every call.
			</p>
			<button class="inline-flex items-center gap-2 bg-blue-500 hover:bg-blue-600 text-white px-6 py-3 rounded-xl text-base font-medium transition-all shadow-lg hover:shadow-xl hover:-translate-y-0.5">
				{@html AppleIcon}
				Get for Mac
			</button>
		</div>

		<!-- Hero Image / Mockup -->
		<div class="max-w-6xl mx-auto relative">
			<!-- Abstract Background behind laptop -->
			<div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[120%] h-[120%] bg-gradient-to-tr from-orange-200 via-purple-200 to-blue-200 blur-3xl opacity-60 rounded-full -z-10"></div>

			<!-- Laptop Frame -->
			<div class="relative bg-gray-900 rounded-[2rem] p-3 shadow-2xl border border-gray-800">
				<!-- Screen -->
				<div class="bg-black rounded-xl overflow-hidden aspect-[16/10] relative">
					<!-- Video Call Interface Simulation -->
					<div class="absolute inset-0 flex flex-col">
						<!-- Top Bar -->
						<div class="h-8 bg-gray-800/50 flex items-center justify-between px-4">
							<div class="flex gap-1.5">
								<div class="w-3 h-3 rounded-full bg-red-500"></div>
								<div class="w-3 h-3 rounded-full bg-yellow-500"></div>
								<div class="w-3 h-3 rounded-full bg-green-500"></div>
							</div>
							<div class="flex gap-4 text-gray-400">
								<div class="w-4 h-4 bg-gray-600 rounded-full"></div>
								<div class="w-4 h-4 bg-gray-600 rounded-full"></div>
							</div>
						</div>

						<!-- Main Video Area -->
						<div class="flex-1 relative bg-gray-800">
							<!-- Two people grid -->
							<div class="grid grid-cols-2 h-full">
								<div class="bg-gray-700 relative overflow-hidden border-r border-gray-900">
                                    <!-- Placeholder for person 1 -->
                                    <div class="absolute inset-0 bg-gradient-to-br from-gray-600 to-gray-800 flex items-center justify-center">
                                        <div class="w-20 h-20 rounded-full bg-gray-500 opacity-50"></div>
                                    </div>
                                    <!-- Name tag -->
                                    <div class="absolute bottom-4 left-4 bg-black/50 text-white text-xs px-2 py-1 rounded">You</div>
                                </div>
								<div class="bg-gray-700 relative overflow-hidden">
                                     <!-- Placeholder for person 2 -->
                                     <div class="absolute inset-0 bg-gradient-to-bl from-gray-600 to-gray-800 flex items-center justify-center">
                                        <div class="w-20 h-20 rounded-full bg-gray-500 opacity-50"></div>
                                     </div>
                                      <!-- Name tag -->
                                    <div class="absolute bottom-4 left-4 bg-black/50 text-white text-xs px-2 py-1 rounded">Alex</div>
                                </div>
							</div>

							<!-- AI Overlay (The main feature) -->
							<div class="absolute top-8 left-1/2 -translate-x-1/2 w-[480px] bg-gray-900/90 backdrop-blur-xl border border-gray-700/50 rounded-xl shadow-2xl overflow-hidden text-white">
								<!-- Header -->
								<div class="px-4 py-3 border-b border-gray-700/50 flex items-center gap-2">
									<div class="w-4 h-4 text-blue-400">{@html Search}</div>
									<span class="text-sm text-gray-400">Searched records</span>
                                    <div class="ml-auto bg-blue-600 text-xs font-medium px-2 py-0.5 rounded text-white">What should I say?</div>
								</div>
								<!-- Content -->
								<div class="p-4 space-y-4">
									<p class="text-sm leading-relaxed text-gray-200">
										"So just to recap—you need new cabinets and lighting. I'll send you a quote within the hour. Let's do a kickoff call next Wednesday if that works for you?"
									</p>
									<div class="flex items-center gap-4 text-xs text-gray-500 mt-2">
										<button class="flex items-center gap-1 hover:text-gray-300 transition-colors">
											<span class="opacity-70">✨</span> What should I say?
										</button>
										<button class="flex items-center gap-1 hover:text-gray-300 transition-colors">
											<span class="opacity-70">💬</span> Follow-up questions
										</button>
									</div>
                                    <!-- Input -->
                                    <div class="relative mt-2">
                                        <input type="text" placeholder="Ask, ⌘ ⏎ to start typing" class="w-full bg-gray-800/50 border border-gray-700 rounded-lg py-2 px-3 text-sm text-gray-300 placeholder-gray-600 focus:outline-none focus:border-gray-600">
                                    </div>
								</div>
							</div>

                            <!-- Bottom Controls -->
                            <div class="absolute bottom-6 left-1/2 -translate-x-1/2 flex gap-4">
                                <div class="w-10 h-10 rounded-full bg-gray-800/80 flex items-center justify-center text-white hover:bg-gray-700 cursor-pointer">{@html Mic}</div>
                                <div class="w-10 h-10 rounded-full bg-gray-800/80 flex items-center justify-center text-white hover:bg-gray-700 cursor-pointer">{@html Video}</div>
                                <div class="w-10 h-10 rounded-full bg-red-600 flex items-center justify-center text-white hover:bg-red-700 cursor-pointer px-4 w-auto text-xs font-medium">End</div>
                            </div>
						</div>
					</div>
				</div>
			</div>
            
            <!-- Dock Icons (Simulated) -->
            <div class="flex justify-center mt-8 gap-4">
                 <div class="bg-white/20 backdrop-blur-md p-2 rounded-2xl flex gap-3 border border-white/30 shadow-lg">
                    <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-pink-500 to-purple-600 shadow-inner"></div>
                    <div class="w-10 h-10 rounded-xl bg-gray-800 shadow-inner"></div>
                    <div class="w-10 h-10 rounded-xl bg-blue-500 shadow-inner flex items-center justify-center text-white font-bold text-xs">zoom</div>
                    <div class="w-10 h-10 rounded-xl bg-blue-400 shadow-inner flex items-center justify-center text-white">
                        <div class="w-6 h-6 border-2 border-white rounded-full"></div>
                    </div>
                 </div>
            </div>
		</div>
	</section>

	<!-- Features Grid -->
	<section class="py-24 px-4 md:px-6 bg-white">
		<div class="max-w-6xl mx-auto">
			<h2 class="text-4xl md:text-5xl font-serif text-gray-900 mb-16 max-w-xl">
				Four ways we make your meetings better
			</h2>

			<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
				<!-- Card 1: Blue -->
				<div class="bg-[#4F86F7] rounded-3xl p-8 md:p-10 text-white relative overflow-hidden min-h-[400px] flex flex-col justify-end group">
                    <!-- UI Overlay Simulation -->
                    <div class="absolute top-8 left-8 right-8 bg-gray-900/40 backdrop-blur-md rounded-xl p-4 border border-white/10 shadow-lg transform group-hover:-translate-y-2 transition-transform duration-500">
                         <div class="flex justify-end mb-2">
                             <span class="bg-blue-600 text-[10px] font-bold px-2 py-0.5 rounded-full">What do I say next?</span>
                         </div>
                         <p class="text-sm text-gray-100 mb-3">"I hear you on the integration concerns—that's usually the first thing that comes up. We've actually built direct connectors for the tools you mentioned, and our average setup time is only half a day."</p>
                         <div class="h-8 bg-white/10 rounded-lg w-full"></div>
                    </div>

					<div class="relative z-10 mt-32">
						<h3 class="text-2xl font-semibold mb-2">AI that answers questions for you, real-time</h3>
						<p class="text-blue-100 text-sm">Cluely uses the screen, transcript, and AI to answer questions for you, live.</p>
					</div>
				</div>

				<!-- Card 2: Instant follow-up -->
				<div class="bg-gray-50 rounded-3xl p-8 md:p-10 relative overflow-hidden min-h-[400px] flex flex-col justify-between group border border-gray-100">
					<div>
						<h3 class="text-2xl font-serif font-medium mb-2 text-gray-900">Instant follow-up emails</h3>
						<p class="text-gray-500 text-sm">Send perfectly drafted follow-up emails within seconds after every call.</p>
					</div>
                    
                    <!-- Email UI -->
                    <div class="mt-8 bg-white rounded-xl shadow-sm border border-gray-200 p-6 transform group-hover:scale-[1.02] transition-transform duration-500">
                        <div class="flex items-center justify-between mb-4">
                            <h4 class="font-semibold text-lg">Senior Software Engineer Technical Interview</h4>
                        </div>
                        <div class="space-y-3">
                            <div class="h-2 bg-gray-100 rounded w-3/4"></div>
                            <div class="h-2 bg-gray-100 rounded w-full"></div>
                            <div class="h-2 bg-gray-100 rounded w-5/6"></div>
                        </div>
                         <div class="mt-6 flex items-center justify-between border-t border-gray-100 pt-4">
                            <div class="flex items-center gap-2">
                                <div class="w-8 h-8 bg-gray-200 rounded-full"></div>
                                <span class="text-sm font-medium">Roy Lee</span>
                            </div>
                            <button class="bg-blue-500 text-white text-xs px-3 py-1.5 rounded-full flex items-center gap-1">
                                {@html Mail} Follow-up email
                            </button>
                        </div>
                    </div>
				</div>

				<!-- Card 3: Who are you talking to -->
				<div class="bg-gray-50 rounded-3xl p-8 md:p-10 relative overflow-hidden min-h-[300px] flex flex-col justify-center group border border-gray-100">
                    <div class="flex flex-col md:flex-row gap-8 items-center">
                        <div class="flex-1">
                            <h3 class="text-2xl font-serif font-medium mb-2 text-gray-900">Who are you really talking to?</h3>
                            <p class="text-gray-500 text-sm">Learn everything about anyone before every call — where they work, what they do, and more.</p>
                        </div>
                        <!-- Profile Card UI -->
                        <div class="w-full md:w-64 bg-white rounded-xl shadow-sm border border-gray-200 p-4 text-xs transform group-hover:translate-x-2 transition-transform duration-500">
                            <div class="flex items-center gap-2 mb-3 text-gray-500">
                                <span>Cluely</span>
                                <span>&lt;&gt;</span>
                                <span>Acme Intro</span>
                            </div>
                            <div class="flex items-start gap-3 mb-3">
                                <div class="w-10 h-10 bg-gray-200 rounded-full"></div>
                                <div>
                                    <div class="font-bold text-gray-900">Roy Lee</div>
                                    <div class="text-gray-500 flex items-center gap-1 mt-1">
                                        <span>San Francisco</span> • <span>CEO</span>
                                    </div>
                                </div>
                            </div>
                            <p class="text-gray-600 leading-relaxed">
                                21-year-old CEO of Cluely, an AI real-time meeting assistant...
                            </p>
                        </div>
                    </div>
				</div>

				<!-- Card 4: Beautiful meeting notes -->
				<div class="bg-gray-50 rounded-3xl p-8 md:p-10 relative overflow-hidden min-h-[300px] flex flex-col justify-center group border border-gray-100">
                     <div class="flex flex-col-reverse md:flex-row gap-8 items-center">
                        <div class="flex-1">
                            <h3 class="text-2xl font-serif font-medium mb-2 text-gray-900">Beautiful meeting notes</h3>
                            <p class="text-gray-500 text-sm">Instant shareable meeting notes generated by AI.</p>
                        </div>
                         <!-- Notes UI -->
                         <div class="w-full md:w-64 bg-white rounded-xl shadow-sm border border-gray-200 p-4 transform group-hover:-translate-y-2 transition-transform duration-500">
                            <div class="flex items-center gap-2 mb-4">
                                <div class="w-6 h-6 bg-gray-200 rounded-full"></div>
                                <div class="text-[10px] text-gray-500">Roy Lee</div>
                                <div class="ml-auto text-[10px] text-gray-400">{@html Linkedin}</div>
                            </div>
                            <div class="h-px bg-gray-100 mb-3"></div>
                            <div class="space-y-2">
                                <div class="h-1.5 bg-gray-100 rounded w-1/3"></div>
                                <div class="h-3 bg-gray-50 rounded w-full"></div>
                            </div>
                         </div>
                     </div>
				</div>
			</div>
		</div>
	</section>

	<!-- Steps Section -->
	<section class="py-24 px-4 md:px-6 bg-white">
		<div class="max-w-4xl mx-auto text-center mb-16">
			<h2 class="text-4xl md:text-5xl font-serif text-gray-900 mb-4">Meeting notes in 3 steps</h2>
			<p class="text-gray-500 text-lg">The easiest way to get beautiful, shareable meeting notes.</p>
		</div>

		<div class="max-w-6xl mx-auto grid grid-cols-1 md:grid-cols-3 gap-12 relative">
            <!-- Connecting Arrows (Hidden on mobile) -->
            <div class="hidden md:block absolute top-1/3 left-[28%] w-20 h-10 border-t-2 border-r-2 border-gray-200 rounded-tr-3xl transform rotate-45"></div>
            <div class="hidden md:block absolute top-1/3 right-[28%] w-20 h-10 border-t-2 border-r-2 border-gray-200 rounded-tr-3xl transform rotate-45"></div>

			<!-- Step 1 -->
			<div class="flex flex-col gap-6">
				<div class="bg-gray-100 rounded-2xl overflow-hidden aspect-[4/3] relative group shadow-inner">
                    <div class="absolute inset-4 bg-white rounded-lg shadow-sm border border-gray-200 flex flex-col p-3">
                        <div class="flex items-center gap-2 mb-2">
                            <div class="w-2 h-2 rounded-full bg-red-400"></div>
                            <div class="w-2 h-2 rounded-full bg-yellow-400"></div>
                        </div>
                        <div class="flex-1 bg-blue-50 rounded flex items-center justify-center relative">
                            <div class="absolute bottom-4 left-1/2 -translate-x-1/2 bg-white px-3 py-1 rounded-full shadow-md text-xs font-medium text-blue-600 flex items-center gap-1">
                                Start Cluely {@html Play}
                            </div>
                            <!-- Cursor -->
                            <div class="absolute bottom-2 left-1/2 translate-x-4 w-4 h-4 text-black">
                                <svg viewBox="0 0 24 24" fill="currentColor" class="drop-shadow-md"><path d="M5.5 3.21V20.8c0 .45.54.67.85.35l4.86-4.86a.5.5 0 0 1 .35-.15h6.87a.5.5 0 0 0 .35-.85L6.35 2.85a.5.5 0 0 0-.85.36z"/></svg>
                            </div>
                        </div>
                    </div>
                </div>
				<div>
					<div class="text-2xl font-serif text-gray-400 mb-2">1 <span class="text-gray-900">Start Cluely</span></div>
					<p class="text-sm text-gray-500 leading-relaxed">Simply click Start Cluely before your meeting begins.</p>
				</div>
			</div>

			<!-- Step 2 -->
			<div class="flex flex-col gap-6">
				<div class="bg-gray-100 rounded-2xl overflow-hidden aspect-[4/3] relative shadow-inner">
                     <div class="absolute inset-4 bg-gray-800 rounded-lg shadow-sm border border-gray-700 flex flex-col items-center justify-center">
                         <div class="flex items-center gap-2 bg-black/50 px-3 py-2 rounded-full backdrop-blur-sm border border-white/10">
                             <span class="text-[10px] text-white">for Sales (Roy)</span>
                             <div class="w-px h-3 bg-white/20"></div>
                             <div class="w-4 h-4 bg-red-500 rounded-sm"></div>
                             <div class="w-4 h-4 text-white">{@html X}</div>
                         </div>
                         <!-- Cursor -->
                         <div class="absolute top-1/2 left-1/2 translate-x-4 translate-y-4 w-4 h-4 text-white">
                            <svg viewBox="0 0 24 24" fill="currentColor" class="drop-shadow-md"><path d="M5.5 3.21V20.8c0 .45.54.67.85.35l4.86-4.86a.5.5 0 0 1 .35-.15h6.87a.5.5 0 0 0 .35-.85L6.35 2.85a.5.5 0 0 0-.85.36z"/></svg>
                        </div>
                     </div>
                </div>
				<div>
					<div class="text-2xl font-serif text-gray-400 mb-2">2 <span class="text-gray-900">End Cluely</span></div>
					<p class="text-sm text-gray-500 leading-relaxed">Click the Stop button to end recording. That's it.</p>
				</div>
			</div>

			<!-- Step 3 -->
			<div class="flex flex-col gap-6">
				<div class="bg-gray-100 rounded-2xl overflow-hidden aspect-[4/3] relative shadow-inner">
                     <div class="absolute inset-4 bg-white rounded-lg shadow-sm border border-gray-200 p-4 overflow-hidden">
                         <h4 class="font-bold text-xs mb-2">Senior Software Engineer Technical Interview</h4>
                         <div class="space-y-2 opacity-50">
                             <div class="h-1.5 bg-gray-300 rounded w-1/2"></div>
                             <div class="h-1.5 bg-gray-300 rounded w-full"></div>
                             <div class="h-1.5 bg-gray-300 rounded w-3/4"></div>
                             <div class="h-1.5 bg-gray-300 rounded w-full"></div>
                         </div>
                     </div>
                </div>
				<div>
					<div class="text-2xl font-serif text-gray-400 mb-2">3 <span class="text-gray-900">Get notes</span></div>
					<p class="text-sm text-gray-500 leading-relaxed">Cluely uses what it heard and what it saw on your screen to generate notes.</p>
				</div>
			</div>
		</div>
	</section>

	<!-- Comparison Section -->
	<section class="py-24 px-4 md:px-6 bg-white">
		<div class="max-w-4xl mx-auto text-center mb-12">
			<h2 class="text-4xl md:text-5xl font-serif text-gray-900 mb-4">No meeting bots.<br>100% undetectable.</h2>
			<a href="#" class="text-blue-500 hover:text-blue-600 font-medium text-sm">How does Cluely stay undetectable?</a>
		</div>

		<div class="max-w-5xl mx-auto grid grid-cols-1 md:grid-cols-2 rounded-3xl overflow-hidden shadow-2xl">
			<!-- Left: Other AI -->
			<div class="bg-gray-100 p-8 md:p-12 flex flex-col items-center justify-center text-center relative min-h-[400px]">
				<h3 class="text-xl font-semibold text-gray-900 mb-2">Other AI Notetakers</h3>
				<div class="flex items-center gap-2 text-gray-500 text-xs mb-8">
					<div class="w-4 h-4 rounded-full bg-gray-400 text-white flex items-center justify-center text-[10px]">✕</div>
					Joins as an invasive participant
				</div>

				<!-- Mockup -->
				<div class="w-full max-w-sm bg-[#3B5998] rounded-xl aspect-video relative flex items-center justify-center shadow-lg">
					<div class="w-16 h-16 bg-white rounded-full flex items-center justify-center">
						<div class="text-2xl">🤖</div>
					</div>
					<div class="absolute bottom-3 left-3 flex items-center gap-2">
						<span class="text-white text-xs font-medium">David's AI Notetaker</span>
                        <div class="w-4 h-4 bg-white/20 rounded-full flex items-center justify-center text-[10px] text-white">🎤</div>
					</div>
				</div>
			</div>

			<!-- Right: Cluely -->
			<div class="bg-[#4F5B6F] p-8 md:p-12 flex flex-col items-center justify-center text-center relative min-h-[400px] overflow-hidden">
                <!-- Background Pattern -->
                <div class="absolute inset-0 opacity-10" style="background-image: repeating-linear-gradient(45deg, #000 0, #000 10px, transparent 10px, transparent 20px);"></div>
                
				<h3 class="text-xl font-semibold text-white mb-2 relative z-10 flex items-center gap-2">
                    <div class="w-5 h-5 bg-white text-gray-800 rounded-full flex items-center justify-center text-xs font-bold">C</div>
                    Cluely
                </h3>
				<div class="flex items-center gap-2 text-gray-300 text-xs mb-8 relative z-10">
					<div class="w-4 h-4 rounded-full bg-green-500 text-white flex items-center justify-center text-[10px]">✓</div>
					Undetectable to screen share, visible to you
				</div>

                <!-- Slider UI -->
                <div class="relative w-full max-w-sm h-48 bg-gray-800/50 rounded-xl border border-gray-600 p-4 flex flex-col justify-center gap-3">
                     <!-- Visible label -->
                     <div class="absolute top-4 left-4 bg-white text-gray-900 text-[10px] font-bold px-2 py-0.5 rounded-full">Visible to you</div>
                     <!-- Invisible label -->
                     <div class="absolute top-4 right-4 bg-gray-700 text-gray-300 text-[10px] font-bold px-2 py-0.5 rounded-full">Invisible to others</div>

                     <!-- Content lines -->
                     <div class="h-2 bg-gray-600 rounded w-3/4"></div>
                     <div class="h-2 bg-gray-600 rounded w-1/2"></div>
                     <div class="h-2 bg-gray-600 rounded w-full"></div>

                     <!-- Slider Handle (Center) -->
                     <div class="absolute inset-y-0 left-1/2 w-0.5 bg-white flex items-center justify-center">
                         <div class="w-6 h-6 bg-white rounded-full shadow-lg flex items-center justify-center text-gray-600">
                             <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3"><path d="m9 18 6-6-6-6"/></svg>
                         </div>
                     </div>
                </div>
			</div>
		</div>
	</section>

	<!-- Transcription Section -->
	<section class="py-24 px-4 md:px-6 bg-white">
		<div class="max-w-6xl mx-auto flex flex-col md:flex-row items-center gap-16">
			<!-- Left: Image -->
			<div class="w-full md:w-1/2">
				<div class="bg-gray-100 rounded-3xl p-8 md:p-12 shadow-sm">
					<div class="bg-white rounded-xl shadow-lg p-6 md:p-8 aspect-[3/4] max-w-sm mx-auto">
						<div class="flex items-center gap-3 mb-6">
							<div class="w-8 h-8 bg-gray-200 rounded-full"></div>
							<div>
								<div class="text-[10px] text-gray-500">Thursday, Oct 24 • 👥 Alexa, Roy +3 more</div>
								<h3 class="text-lg font-serif font-bold leading-tight mt-1">Strategic Sales Growth and Client Relationship Management</h3>
							</div>
						</div>
						
						<div class="flex gap-2 mb-6">
							<span class="bg-gray-100 text-gray-600 text-[10px] px-2 py-1 rounded">Summary</span>
							<span class="bg-blue-50 text-blue-600 text-[10px] px-2 py-1 rounded font-medium">Transcript</span>
							<span class="bg-gray-100 text-gray-600 text-[10px] px-2 py-1 rounded">Chats</span>
						</div>

						<div class="space-y-4 text-[11px] leading-relaxed">
							<div>
								<div class="flex justify-between text-gray-400 mb-0.5">
									<span class="text-blue-500 font-medium">Neesh</span>
									<span>12:32 PM</span>
								</div>
								<p class="text-gray-800">Hi, this is Neesh from TechSolutions.</p>
							</div>
							<div>
								<div class="flex justify-between text-gray-400 mb-0.5">
									<span class="font-medium text-gray-700">Them</span>
									<span>12:33 PM</span>
								</div>
								<p class="text-gray-800">Hello.</p>
							</div>
							<div>
								<div class="flex justify-between text-gray-400 mb-0.5">
									<span class="text-blue-500 font-medium">Neesh</span>
									<span>12:32 PM</span>
								</div>
								<p class="text-gray-800">How are you today?</p>
							</div>
                             <div>
								<div class="flex justify-between text-gray-400 mb-0.5">
									<span class="font-medium text-gray-700">Them</span>
									<span>12:33 PM</span>
								</div>
								<p class="text-gray-800">Good, thanks.</p>
							</div>
                            <div>
								<div class="flex justify-between text-gray-400 mb-0.5">
									<span class="text-blue-500 font-medium">Neesh</span>
									<span>12:32 PM</span>
								</div>
								<p class="text-gray-800">I wanted to talk about your current software.</p>
							</div>
						</div>
					</div>
				</div>
			</div>

			<!-- Right: Stats -->
			<div class="w-full md:w-1/2 space-y-12">
				<h2 class="text-4xl md:text-5xl font-serif text-gray-900 mb-8">Real-time transcription</h2>
				
				<div class="space-y-10">
					<div class="flex gap-6">
						<div class="w-24 shrink-0 text-4xl font-serif text-gray-900">12+</div>
						<div>
							<h3 class="text-xl font-medium text-gray-900 mb-2">Languages</h3>
							<p class="text-gray-500 text-sm leading-relaxed">We support over 12 different languages, including English, Chinese, Spanish, and more.</p>
						</div>
					</div>

					<div class="flex gap-6">
						<div class="w-24 shrink-0 text-4xl font-serif text-gray-900">300<span class="text-lg">ms</span></div>
						<div>
							<h3 class="text-xl font-medium text-gray-900 mb-2">Response time</h3>
							<p class="text-gray-500 text-sm leading-relaxed">We have the fastest live transcription available. Test us against any other competitor.</p>
						</div>
					</div>

					<div class="flex gap-6">
						<div class="w-24 shrink-0 text-4xl font-serif text-gray-900">95%</div>
						<div>
							<h3 class="text-xl font-medium text-gray-900 mb-2">Transcription accuracy</h3>
							<p class="text-gray-500 text-sm leading-relaxed">Trusted by many teams for reliable transcription. All processed with industry-leading accuracy.</p>
						</div>
					</div>
				</div>
			</div>
		</div>
	</section>

	<!-- FAQ Section -->
	<section class="py-24 px-4 md:px-6 bg-white">
		<div class="max-w-3xl mx-auto">
			<h2 class="text-3xl md:text-4xl font-serif text-gray-900 mb-12">Frequently asked questions</h2>
			
			<div class="space-y-4">
				{#each faqs as faq, i}
					<div class="border-b border-gray-100 pb-4">
						<button 
							class="w-full flex items-center justify-between text-left py-4 hover:text-blue-600 transition-colors"
							onclick={() => toggleFaq(i)}
						>
							<span class="text-lg font-medium text-gray-900">{faq.question}</span>
							<span class="transform transition-transform duration-300 {openFaq === i ? 'rotate-180' : ''}">
								{@html ChevronDown}
							</span>
						</button>
						{#if openFaq === i}
							<div class="pb-4 text-gray-500 leading-relaxed">
								{faq.answer}
							</div>
						{/if}
					</div>
				{/each}
			</div>
		</div>
	</section>

	<!-- Footer / CTA -->
	<section class="bg-gray-100 pt-32 pb-12 px-4 md:px-6 relative overflow-hidden">
        <!-- Floating Keys -->
        <div class="absolute top-20 right-[15%] w-24 h-24 bg-white rounded-2xl shadow-xl flex items-center justify-center text-gray-300 transform rotate-12 opacity-80">
            {@html CornerDownLeft}
        </div>
        <div class="absolute bottom-1/3 right-[25%] w-20 h-20 bg-white rounded-2xl shadow-xl flex items-center justify-center text-gray-300 transform -rotate-12 opacity-60 blur-[1px]">
            {@html Command}
        </div>

		<div class="max-w-4xl mx-auto text-center mb-24 relative z-10">
			<h2 class="text-4xl md:text-5xl font-serif text-gray-900 mb-4">Meeting AI that helps during the call, not after.</h2>
			<p class="text-xl text-blue-500 mb-8">Try Cluely on your next meeting today.</p>
			<button class="inline-flex items-center gap-2 bg-blue-500 hover:bg-blue-600 text-white px-6 py-3 rounded-xl text-base font-medium transition-all shadow-lg hover:shadow-xl hover:-translate-y-0.5">
				{@html AppleIcon}
				Get for Mac
			</button>
		</div>

		<div class="max-w-6xl mx-auto border-t border-gray-200 pt-16">
			<div class="grid grid-cols-2 md:grid-cols-6 gap-8 mb-16">
				<div class="col-span-2 md:col-span-2">
					<a href="/" class="flex items-center gap-2 text-xl font-bold tracking-tight mb-6">
						<div class="w-6 h-6 flex items-center justify-center bg-gray-900 text-white rounded-full">
							<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" class="w-4 h-4"><path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/></svg>
						</div>
						Cluely
					</a>
				</div>
				
				<div>
					<h4 class="font-bold text-sm mb-4">Use Cases</h4>
					<ul class="space-y-3 text-sm text-gray-600">
						<li><a href="#" class="hover:text-gray-900">Sales</a></li>
						<li><a href="#" class="hover:text-gray-900">Support</a></li>
						<li><a href="#" class="hover:text-gray-900">Consulting</a></li>
						<li><a href="#" class="hover:text-gray-900">Recruiting</a></li>
					</ul>
				</div>

				<div>
					<h4 class="font-bold text-sm mb-4">Enterprise</h4>
					<ul class="space-y-3 text-sm text-gray-600">
						<li><a href="#" class="hover:text-gray-900">Cluely for Enterprise</a></li>
						<li><a href="#" class="hover:text-gray-900">Enterprise Guides</a></li>
						<li><a href="#" class="hover:text-gray-900">Security</a></li>
						<li><a href="#" class="hover:text-gray-900">Vendor Profile</a></li>
						<li><a href="#" class="hover:text-gray-900">ROI Calculator</a></li>
						<li><a href="#" class="hover:text-gray-900">Book A Demo</a></li>
					</ul>
				</div>

				<div>
					<h4 class="font-bold text-sm mb-4">Resources</h4>
					<ul class="space-y-3 text-sm text-gray-600">
						<li><a href="#" class="hover:text-gray-900">Manifesto</a></li>
						<li><a href="#" class="hover:text-gray-900">Press</a></li>
						<li><a href="#" class="hover:text-gray-900">Careers</a></li>
						<li><a href="#" class="hover:text-gray-900">Bug Bounty</a></li>
					</ul>
				</div>

				<div>
					<h4 class="font-bold text-sm mb-4">Support</h4>
					<ul class="space-y-3 text-sm text-gray-600">
						<li><a href="#" class="hover:text-gray-900">Help Center</a></li>
						<li><a href="#" class="hover:text-gray-900">Contact Us</a></li>
					</ul>
                    
                    <h4 class="font-bold text-sm mt-8 mb-4">Legal</h4>
                    <ul class="space-y-3 text-sm text-gray-600">
						<li><a href="#" class="hover:text-gray-900">Privacy Policy</a></li>
						<li><a href="#" class="hover:text-gray-900">Terms of Service</a></li>
                        <li><a href="#" class="hover:text-gray-900">Data Processing Agreement</a></li>
                        <li><a href="#" class="hover:text-gray-900">Subprocessors</a></li>
					</ul>
				</div>
			</div>

			<div class="flex flex-col md:flex-row items-center justify-between gap-6 pt-8 border-t border-gray-200">
				<div class="flex flex-col gap-4">
                    <div class="flex items-center gap-2">
                        <div class="w-2 h-2 rounded-full bg-green-500"></div>
                        <span class="text-xs text-gray-500">All systems operational</span>
                    </div>
                    <div class="flex gap-4 opacity-50 grayscale hover:grayscale-0 transition-all">
                        <!-- Certification Badges (Placeholders) -->
                        <div class="w-8 h-8 bg-gray-800 rounded-full flex items-center justify-center text-[8px] text-white font-bold border border-gray-600">SOC2</div>
                        <div class="w-8 h-8 bg-gray-800 rounded-full flex items-center justify-center text-[8px] text-white font-bold border border-gray-600">ISO</div>
                        <div class="w-8 h-8 bg-gray-800 rounded-full flex items-center justify-center text-[8px] text-white font-bold border border-gray-600">GDPR</div>
                        <div class="w-8 h-8 bg-gray-800 rounded-full flex items-center justify-center text-[8px] text-white font-bold border border-gray-600">CCPA</div>
                    </div>
                    <p class="text-xs text-blue-500">List of subprocessors.</p>
                </div>

				<div class="flex flex-col md:flex-row items-center gap-6 text-xs text-gray-500">
					<span>© 2025 Cluely. All rights reserved.</span>
					<div class="flex gap-4">
						<a href="#" class="hover:text-gray-900">𝕏</a>
						<a href="#" class="hover:text-gray-900">👾</a>
						<a href="#" class="hover:text-gray-900">📸</a>
                        <a href="#" class="hover:text-gray-900">🐙</a>
					</div>
				</div>
			</div>
		</div>
	</section>
</div>

