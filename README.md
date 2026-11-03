# TODOLIST-NEO
TODO LIST
<!DOCTYPE html>
<html lang="en" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TaskAI - Smart Prompt-Driven To-Do List</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0f7ff',
                            100: '#e0effe',
                            500: '#3b82f6',
                            600: '#2563eb',
                            700: '#1d4ed8',
                        }
                    }
                }
            }
        }
    </script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
        /* Custom scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 9999px; }
        .dark ::-webkit-scrollbar-thumb { background: #334155; }
        /* Smooth transitions */
        .task-item { transition: all 0.2s ease-in-out; }
    </style>
</head>
<body class="bg-slate-50 dark:bg-slate-900 text-slate-800 dark:text-slate-100 min-h-full flex flex-col transition-colors duration-200">

    <!-- Header / Navbar -->
    <header class="border-b border-slate-200 dark:border-slate-800 bg-white/80 dark:bg-slate-900/80 backdrop-blur sticky top-0 z-30">
        <div class="max-w-5xl mx-auto px-4 py-3 flex items-center justify-between gap-4">
            <div class="flex items-center space-x-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-blue-600 to-indigo-500 flex items-center justify-center text-white shadow-md shadow-blue-500/20">
                    <i class="fa-solid font-bold fa-list-check text-xl"></i>
                </div>
                <div>
                    <h1 class="font-bold text-lg leading-tight flex items-center gap-2">
                        TaskAI <span class="text-xs font-semibold px-2 py-0.5 rounded-full bg-blue-100 dark:bg-blue-900/50 text-blue-600 dark:text-blue-300">Smart Engine</span>
                    </h1>
                    <p class="text-xs text-slate-500 dark:text-slate-400">Transform prompts & thoughts into structured tasks</p>
                </div>
            </div>

            <div class="flex items-center space-x-2">
                <!-- Theme Toggle -->
                <button id="themeToggleBtn" onclick="toggleTheme()" class="p-2.5 rounded-xl border border-slate-200 dark:border-slate-800 hover:bg-slate-100 dark:hover:bg-slate-800 text-slate-600 dark:text-slate-300 transition-colors" title="Toggle Light/Dark Theme">
                    <i id="themeIcon" class="fa-solid fa-moon"></i>
                </button>
                <!-- Export Menu Button -->
                <button onclick="toggleExportMenu()" class="p-2.5 rounded-xl border border-slate-200 dark:border-slate-800 hover:bg-slate-100 dark:hover:bg-slate-800 text-slate-600 dark:text-slate-300 transition-colors relative" title="Export Tasks">
                    <i class="fa-solid fa-download"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="max-w-5xl mx-auto px-4 py-6 flex-grow w-full space-y-6">

        <!-- Progress Stats Banner -->
        <div class="bg-white dark:bg-slate-800/80 border border-slate-200/80 dark:border-slate-700/60 rounded-2xl p-5 shadow-sm">
            <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-3">
                <div>
                    <h2 class="text-base font-semibold text-slate-800 dark:text-slate-100">Project Execution Dashboard</h2>
                    <p class="text-xs text-slate-500 dark:text-slate-400" id="progressSubtitle">0 of 0 tasks completed</p>
                </div>
                <div class="flex items-center space-x-4 text-xs font-medium text-slate-600 dark:text-slate-300">
                    <div class="flex items-center gap-1.5"><span class="w-2.5 h-2.5 rounded-full bg-slate-300 dark:bg-slate-600"></span> Total: <span id="statTotal">0</span></div>
                    <div class="flex items-center gap-1.5"><span class="w-2.5 h-2.5 rounded-full bg-amber-500"></span> Pending: <span id="statPending">0</span></div>
                    <div class="flex items-center gap-1.5"><span class="w-2.5 h-2.5 rounded-full bg-emerald-500"></span> Completed: <span id="statDone">0</span></div>
                </div>
            </div>
            <!-- Progress Bar -->
            <div class="w-full bg-slate-100 dark:bg-slate-700/60 h-3 rounded-full overflow-hidden">
                <div id="progressBar" class="bg-gradient-to-r from-blue-500 to-indigo-600 h-full w-0 transition-all duration-500 rounded-full"></div>
            </div>
        </div>

        <!-- Input Modes (Tabs: AI AI Plan Generator vs Quick NLP Input) -->
        <div class="bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-2xl shadow-sm overflow-hidden">
            <!-- Mode Selector Header -->
            <div class="flex border-b border-slate-200 dark:border-slate-700 bg-slate-50/50 dark:bg-slate-800/50">
                <button id="tabAiBtn" onclick="switchInputTab('ai')" class="flex-1 py-3 px-4 text-sm font-semibold flex items-center justify-center gap-2 border-b-2 border-blue-600 text-blue-600 dark:text-blue-400 bg-white dark:bg-slate-800 transition-all">
                    <i class="fa-solid fa-wand-magic-sparkles text-blue-500"></i> AI Goal Breakdown Prompt
                </button>
                <button id="tabQuickBtn" onclick="switchInputTab('quick')" class="flex-1 py-3 px-4 text-sm font-medium text-slate-500 dark:text-slate-400 hover:text-slate-700 dark:hover:text-slate-200 flex items-center justify-center gap-2 border-b-2 border-transparent transition-all">
                    <i class="fa-solid fa-bolt text-amber-500"></i> Quick NLP Task Input
                </button>
            </div>

            <!-- AI Prompt Mode Panel -->
            <div id="aiInputPanel" class="p-5 space-y-3">
                <div class="flex flex-col md:flex-row gap-3">
                    <div class="relative flex-grow">
                        <input type="text" id="aiPromptInput" placeholder="Enter high-level goal or prompt e.g., 'Plan a 3-day trip to Tokyo' or 'Launch a SaaS MVP'..." 
                               class="w-full pl-10 pr-4 py-3 rounded-xl border border-slate-200 dark:border-slate-700 bg-slate-50 dark:bg-slate-900 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500/50 focus:border-blue-500 dark:text-slate-100 transition-all"
                               onkeydown="if(event.key==='Enter') generateAiTasks()">
                        <i class="fa-solid fa-brain absolute left-3.5 top-3.5 text-slate-400"></i>
                    </div>
                    <button onclick="generateAiTasks()" id="aiGenerateBtn" class="px-5 py-3 rounded-xl bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-700 hover:to-indigo-700 text-white font-medium text-sm flex items-center justify-center gap-2 shadow-md shadow-blue-500/20 active:scale-[0.98] transition-all">
                        <span>Generate Checklist</span>
                        <i class="fa-solid fa-arrow-right text-xs"></i>
                    </button>
                </div>
                <!-- Prompt Presets -->
                <div class="flex items-center gap-2 flex-wrap pt-1 text-xs text-slate-500 dark:text-slate-400">
                    <span class="font-medium text-slate-400">Try prompts:</span>
                    <button onclick="setPromptPreset('Plan a 3-day product design sprint')" class="px-2.5 py-1 rounded-lg bg-slate-100 dark:bg-slate-700 hover:bg-slate-200 dark:hover:bg-slate-600 text-slate-700 dark:text-slate-300 transition-colors">🚀 Design Sprint</button>
                    <button onclick="setPromptPreset('Prepare for a 5K running race in 4 weeks')" class="px-2.5 py-1 rounded-lg bg-slate-100 dark:bg-slate-700 hover:bg-slate-200 dark:hover:bg-slate-600 text-slate-700 dark:text-slate-300 transition-colors">🏃 5K Race Prep</button>
                    <button onclick="setPromptPreset('Organize a family weekend camping trip')" class="px-2.5 py-1 rounded-lg bg-slate-100 dark:bg-slate-700 hover:bg-slate-200 dark:hover:bg-slate-600 text-slate-700 dark:text-slate-300 transition-colors">🏕️ Weekend Camping</button>
                </div>
            </div>

            <!-- Quick NLP Mode Panel -->
            <div id="quickInputPanel" class="p-5 hidden space-y-3">
                <form onsubmit="handleQuickTaskAdd(event)" class="flex flex-col md:flex-row gap-3">
                    <div class="relative flex-grow">
                        <input type="text" id="quickTaskInput" placeholder="e.g., 'Finish landing page mockup tomorrow p1 #Work ~45m'" 
                               class="w-full pl-10 pr-4 py-3 rounded-xl border border-slate-200 dark:border-slate-700 bg-slate-50 dark:bg-slate-900 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500/50 focus:border-blue-500 dark:text-slate-100 transition-all">
                        <i class="fa-solid fa-plus absolute left-3.5 top-3.5 text-slate-400"></i>
                    </div>
                    <button type="submit" class="px-5 py-3 rounded-xl bg-slate-800 dark:bg-slate-200 text-white dark:text-slate-900 hover:bg-slate-900 dark:hover:bg-white font-medium text-sm flex items-center justify-center gap-2 transition-all">
                        <span>Add Task</span>
                    </button>
                </form>
                <p class="text-xs text-slate-400">
                    <i class="fa-solid fa-circle-info mr-1"></i> Supports inline syntax: <span class="text-blue-500 dark:text-blue-400 font-mono">p1/p2/p3</span> (priority), <span class="text-emerald-500 font-mono">#tag</span> (category), <span class="text-amber-500 font-mono">tomorrow/next tuesday</span> (due date), <span class="text-purple-500 font-mono">~30m</span> (estimated time).
                </p>
            </div>
        </div>

        <!-- Filter, Search & Category Controls Bar -->
        <div class="flex flex-col sm:flex-row items-stretch sm:items-center justify-between gap-3 bg-white dark:bg-slate-800 p-3 rounded-xl border border-slate-200 dark:border-slate-700 shadow-sm">
            <!-- Filter Tabs -->
            <div class="flex items-center space-x-1 overflow-x-auto pb-1 sm:pb-0">
                <button onclick="setFilter('all')" id="filterBtn-all" class="px-3 py-1.5 rounded-lg text-xs font-semibold bg-blue-50 text-blue-600 dark:bg-blue-900/40 dark:text-blue-300 transition-colors">All</button>
                <button onclick="setFilter('pending')" id="filterBtn-pending" class="px-3 py-1.5 rounded-lg text-xs font-medium text-slate-600 dark:text-slate-300 hover:bg-slate-100 dark:hover:bg-slate-700 transition-colors">Pending</button>
                <button onclick="setFilter('completed')" id="filterBtn-completed" class="px-3 py-1.5 rounded-lg text-xs font-medium text-slate-600 dark:text-slate-300 hover:bg-slate-100 dark:hover:bg-slate-700 transition-colors">Completed</button>
                <button onclick="setFilter('high')" id="filterBtn-high" class="px-3 py-1.5 rounded-lg text-xs font-medium text-red-600 dark:text-red-400 hover:bg-red-50 dark:hover:bg-red-950/40 transition-colors">⚡ High Priority</button>
            </div>

            <!-- Search & Sort Controls -->
            <div class="flex items-center space-x-2">
                <div class="relative flex-1 sm:w-48">
                    <input type="text" id="searchInput" oninput="renderTasks()" placeholder="Search tasks..." class="w-full pl-8 pr-3 py-1.5 text-xs rounded-lg border border-slate-200 dark:border-slate-700 bg-slate-50 dark:bg-slate-900 focus:outline-none focus:ring-1 focus:ring-blue-500 dark:text-slate-200">
                    <i class="fa-solid fa-magnifying-glass absolute left-2.5 top-2 text-slate-400 text-xs"></i>
                </div>
                <select id="sortSelect" onchange="renderTasks()" class="px-2.5 py-1.5 rounded-lg border border-slate-200 dark:border-slate-700 bg-slate-50 dark:bg-slate-900 text-xs text-slate-700 dark:text-slate-300 focus:outline-none">
                    <option value="created">Sort: Default</option>
                    <option value="priority">Sort: Priority</option>
                    <option value="due">Sort: Due Date</option>
                </select>
                <button onclick="clearCompletedTasks()" class="px-2.5 py-1.5 text-xs text-slate-500 dark:text-slate-400 hover:text-red-500 dark:hover:text-red-400 rounded-lg border border-transparent hover:border-slate-200 dark:hover:border-slate-700 transition-colors" title="Clear completed tasks">
                    <i class="fa-regular fa-trash-can"></i>
                </button>
            </div>
        </div>

        <!-- Task Items Container -->
        <div id="taskList" class="space-y-3 min-h-[250px]">
            <!-- Rendered tasks will appear here -->
        </div>

        <!-- Notification / Toast Alert Banner -->
        <div id="toast" class="fixed bottom-5 right-5 max-w-sm bg-slate-900 text-white dark:bg-slate-100 dark:text-slate-900 px-4 py-3 rounded-xl shadow-xl flex items-center gap-3 transform translate-y-20 opacity-0 transition-all duration-300 z-50">
            <i id="toastIcon" class="fa-solid fa-circle-check text-emerald-400 dark:text-emerald-600"></i>
            <span id="toastMsg" class="text-xs font-medium">Notification message</span>
        </div>

        <!-- Export Modal -->
        <div id="exportModal" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm hidden items-center justify-center p-4 z-50">
            <div class="bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-2xl max-w-md w-full p-6 shadow-2xl space-y-4">
                <div class="flex items-center justify-between">
                    <h3 class="font-bold text-base text-slate-800 dark:text-slate-100 flex items-center gap-2">
                        <i class="fa-solid fa-file-export text-blue-500"></i> Export To-Do List
                    </h3>
                    <button onclick="toggleExportMenu()" class="text-slate-400 hover:text-slate-600 dark:hover:text-slate-200"><i class="fa-solid fa-xmark"></i></button>
                </div>
                <p class="text-xs text-slate-500 dark:text-slate-400">Choose a format to export your remaining or total tasks checklist.</p>
                
                <div class="space-y-2">
                    <button onclick="copyAsMarkdown()" class="w-full text-left p-3 rounded-xl border border-slate-200 dark:border-slate-700 hover:bg-slate-50 dark:hover:bg-slate-700/50 flex items-center justify-between text-xs font-medium transition-colors">
                        <span class="flex items-center gap-2.5"><i class="fa-brands fa-markdown text-blue-500 text-base"></i> Copy as Markdown Checkbox List</span>
                        <i class="fa-regular fa-copy text-slate-400"></i>
                    </button>
                    <button onclick="downloadAsTxt()" class="w-full text-left p-3 rounded-xl border border-slate-200 dark:border-slate-700 hover:bg-slate-50 dark:hover:bg-slate-700/50 flex items-center justify-between text-xs font-medium transition-colors">
                        <span class="flex items-center gap-2.5"><i class="fa-regular fa-file-lines text-emerald-500 text-base"></i> Download plain text (.txt) file</span>
                        <i class="fa-solid fa-download text-slate-400"></i>
                    </button>
                </div>
            </div>
        </div>

    </main>

    <!-- Footer -->
    <footer class="py-4 border-t border-slate-200 dark:border-slate-800 text-center text-xs text-slate-400 dark:text-slate-500 mt-auto">
        TaskAI &copy; Powered by Gemini API & NLP Parser • Local Browser Persistence
    </footer>

    <!-- Application Script -->
    <script>
        // --- State Management ---
        let tasks = JSON.parse(localStorage.getItem('taskai_tasks_v1')) || [];
        let currentFilter = 'all';

        // --- Initialization ---
        window.onload = function() {
            // Apply theme if set
            if (localStorage.getItem('theme') === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
                document.documentElement.classList.add('dark');
                document.getElementById('themeIcon').className = 'fa-solid fa-sun';
            }
            
            // Seed sample tasks if list is empty
            if (tasks.length === 0) {
                tasks = [
                    {
                        id: '1',
                        title: 'Define target audience personas',
                        phase: 'Phase 1: Research',
                        priority: 'High',
                        category: 'Work',
                        estTime: '45m',
                        dueDate: getRelativeDateString(1),
                        completed: false,
                        createdAt: Date.now() - 100000
                    },
                    {
                        id: '2',
                        title: 'Draft landing page value proposition #Marketing',
                        phase: 'Phase 2: Execution',
                        priority: 'Medium',
                        category: 'Marketing',
                        estTime: '30m',
                        dueDate: getRelativeDateString(2),
                        completed: true,
                        createdAt: Date.now() - 50000
                    }
                ];
                saveTasks();
            }
            renderTasks();
        };

        // --- Persistence ---
        function saveTasks() {
            localStorage.setItem('taskai_tasks_v1', JSON.stringify(tasks));
            updateDashboardStats();
        }

        // --- UI Tabs Switcher ---
        function switchInputTab(mode) {
            const aiPanel = document.getElementById('aiInputPanel');
            const quickPanel = document.getElementById('quickInputPanel');
            const tabAiBtn = document.getElementById('tabAiBtn');
            const tabQuickBtn = document.getElementById('tabQuickBtn');

            if (mode === 'ai') {
                aiPanel.classList.remove('hidden');
                quickPanel.classList.add('hidden');
                tabAiBtn.className = "flex-1 py-3 px-4 text-sm font-semibold flex items-center justify-center gap-2 border-b-2 border-blue-600 text-blue-600 dark:text-blue-400 bg-white dark:bg-slate-800 transition-all";
                tabQuickBtn.className = "flex-1 py-3 px-4 text-sm font-medium text-slate-500 dark:text-slate-400 hover:text-slate-700 dark:hover:text-slate-200 flex items-center justify-center gap-2 border-b-2 border-transparent transition-all";
            } else {
                aiPanel.classList.add('hidden');
                quickPanel.classList.remove('hidden');
                tabQuickBtn.className = "flex-1 py-3 px-4 text-sm font-semibold flex items-center justify-center gap-2 border-b-2 border-blue-600 text-blue-600 dark:text-blue-400 bg-white dark:bg-slate-800 transition-all";
                tabAiBtn.className = "flex-1 py-3 px-4 text-sm font-medium text-slate-500 dark:text-slate-400 hover:text-slate-700 dark:hover:text-slate-200 flex items-center justify-center gap-2 border-b-2 border-transparent transition-all";
            }
        }

        function setPromptPreset(text) {
            document.getElementById('aiPromptInput').value = text;
        }

        // --- Gemini API Prompt Breakdown Generator ---
        async function generateAiTasks() {
            const promptInput = document.getElementById('aiPromptInput');
            const userPrompt = promptInput.value.trim();
            const btn = document.getElementById('aiGenerateBtn');

            if (!userPrompt) {
                showToast("Please enter a goal or project prompt first!", "fa-circle-exclamation", "text-amber-400");
                return;
            }

            // Button loading state
            btn.disabled = true;
            btn.innerHTML = `<i class="fa-solid fa-spinner fa-spin text-sm"></i> Generating...`;

            // Prepare structured JSON Schema Gemini Request
            const systemPrompt = `You are an expert project manager AI. Break down the user's high-level project goal or prompt into a clean, actionable task checklist with 3 to 4 logical phases. Return JSON array format only.`;
            
            const payload = {
                contents: [
                    {
                        role: "user",
                        parts: [{ text: `Create a structured to-do list breakdown for this prompt/goal: "${userPrompt}"` }]
                    }
                ],
                systemInstruction: {
                    parts: [{ text: systemPrompt }]
                },
                generationConfig: {
                    responseMimeType: "application/json",
                    responseSchema: {
                        type: "ARRAY",
                        description: "List of structured actionable tasks",
                        items: {
                            type: "OBJECT",
                            properties: {
                                title: { type: "STRING", description: "Bite-sized task description under 1 hour" },
                                phase: { type: "STRING", description: "Phase name like Phase 1: Planning, Phase 2: Setup, etc." },
                                priority: { type: "STRING", enum: ["High", "Medium", "Low"] },
                                estTime: { type: "STRING", description: "Estimated time like 20m, 45m" },
                                category: { type: "STRING", description: "Category tag like Work, Personal, Travel, Dev" }
                            },
                            required: ["title", "phase", "priority", "estTime", "category"]
                        }
                    }
                }
            };

            const apiKey = ""; // Canvas runtime provides key
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${apiKey}`;

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) throw new Error("API call failed");

                const result = await response.json();
                const rawJson = result.candidates?.[0]?.content?.parts?.[0]?.text;

                if (rawJson) {
                    const generatedItems = JSON.parse(rawJson);
                    let addedCount = 0;

                    generatedItems.forEach((item, idx) => {
                        tasks.unshift({
                            id: Date.now().toString() + idx,
                            title: item.title,
                            phase: item.phase || "General",
                            priority: item.priority || "Medium",
                            category: item.category || "General",
                            estTime: item.estTime || "30m",
                            dueDate: getRelativeDateString(1),
                            completed: false,
                            createdAt: Date.now() + idx
                        });
                        addedCount++;
                    });

                    saveTasks();
                    renderTasks();
                    promptInput.value = "";
                    showToast(`Successfully added ${addedCount} AI-generated tasks!`, "fa-wand-magic-sparkles", "text-blue-400");
                } else {
                    throw new Error("Invalid response content");
                }

            } catch (error) {
                console.error("Gemini API Error, falling back to local heuristic breakdown:", error);
                // Fallback generator if offline/throttled
                generateFallbackTasks(userPrompt);
            } finally {
                btn.disabled = false;
                btn.innerHTML = `<span>Generate Checklist</span><i class="fa-solid fa-arrow-right text-xs"></i>`;
            }
        }

        // --- Local Heuristic Fallback for AI Prompt ---
        function generateFallbackTasks(goalPrompt) {
            const fallbackTasks = [
                { title: `Define requirements & objectives for ${goalPrompt}`, phase: "Phase 1: Planning", priority: "High", estTime: "30m", category: "General" },
                { title: `Gather required resources & tools`, phase: "Phase 1: Planning", priority: "Medium", estTime: "25m", category: "Setup" },
                { title: `Execute core setup action step`, phase: "Phase 2: Execution", priority: "High", estTime: "45m", category: "Execution" },
                { title: `Review output and refine details`, phase: "Phase 3: Review", priority: "Low", estTime: "20m", category: "QA" }
            ];

            fallbackTasks.forEach((item, idx) => {
                tasks.unshift({
                    id: Date.now().toString() + idx,
                    title: item.title,
                    phase: item.phase,
                    priority: item.priority,
                    category: item.category,
                    estTime: item.estTime,
                    dueDate: getRelativeDateString(1),
                    completed: false,
                    createdAt: Date.now() + idx
                });
            });

            saveTasks();
            renderTasks();
            showToast("Generated checklist using standard breakdown engine", "fa-bolt", "text-amber-400");
        }

        // --- Smart Natural Language Processing Parser ---
        function handleQuickTaskAdd(e) {
            e.preventDefault();
            const input = document.getElementById('quickTaskInput');
            let rawText = input.value.trim();

            if (!rawText) return;

            let priority = "Medium";
            let category = "General";
            let estTime = "30m";
            let dueDate = getRelativeDateString(0); // Default today

            // 1. Extract Priority (p1 = High, p2 = Medium, p3 = Low)
            if (/\bp1\b/i.test(rawText)) { priority = "High"; rawText = rawText.replace(/\bp1\b/i, ''); }
            else if (/\bp2\b/i.test(rawText)) { priority = "Medium"; rawText = rawText.replace(/\bp2\b/i, ''); }
            else if (/\bp3\b/i.test(rawText)) { priority = "Low"; rawText = rawText.replace(/\bp3\b/i, ''); }

            // 2. Extract Category Tag (#Work, #Personal, etc.)
            const tagMatch = rawText.match(/#(\w+)/);
            if (tagMatch) {
                category = tagMatch[1];
                rawText = rawText.replace(/#\w+/, '');
            }

            // 3. Extract Time Estimate (~45m, ~1h)
            const timeMatch = rawText.match(/~(\d+[mh])/i);
            if (timeMatch) {
                estTime = timeMatch[1];
                rawText = rawText.replace(/~(\d+[mh])/i, '');
            }

            // 4. Extract Due Date keywords
            if (/\btomorrow\b/i.test(rawText)) {
                dueDate = getRelativeDateString(1);
                rawText = rawText.replace(/\btomorrow\b/i, '');
            } else if (/\bnext tuesday\b/i.test(rawText)) {
                dueDate = getRelativeDateString(5);
                rawText = rawText.replace(/\bnext tuesday\b/i, '');
            } else if (/\bnext week\b/i.test(rawText)) {
                dueDate = getRelativeDateString(7);
                rawText = rawText.replace(/\bnext week\b/i, '');
            }

            // Clean title extra spaces
            const cleanTitle = rawText.replace(/\s+/g, ' ').trim();

            const newTask = {
                id: Date.now().toString(),
                title: cleanTitle || "New Task",
                phase: "Quick Tasks",
                priority: priority,
                category: category,
                estTime: estTime,
                dueDate: dueDate,
                completed: false,
                createdAt: Date.now()
            };

            tasks.unshift(newTask);
            saveTasks();
            renderTasks();
            input.value = "";
            showToast("Task added with NLP smart tags!", "fa-check", "text-emerald-400");
        }

        // --- Tasks Rendering Engine ---
        function renderTasks() {
            const listEl = document.getElementById('taskList');
            const searchQuery = document.getElementById('searchInput').value.toLowerCase();
            const sortBy = document.getElementById('sortSelect').value;

            // Filter logic
            let filtered = tasks.filter(t => {
                if (currentFilter === 'pending') return !t.completed;
                if (currentFilter === 'completed') return t.completed;
                if (currentFilter === 'high') return t.priority === 'High';
                return true;
            });

            // Search filter
            if (searchQuery) {
                filtered = filtered.filter(t => 
                    t.title.toLowerCase().includes(searchQuery) || 
                    t.category.toLowerCase().includes(searchQuery) ||
                    (t.phase && t.phase.toLowerCase().includes(searchQuery))
                );
            }

            // Sorting logic
            filtered.sort((a, b) => {
                if (sortBy === 'priority') {
                    const pMap = { 'High': 3, 'Medium': 2, 'Low': 1 };
                    return pMap[b.priority] - pMap[a.priority];
                }
                if (sortBy === 'due') {
                    return (a.dueDate || '').localeCompare(b.dueDate || '');
                }
                return b.createdAt - a.createdAt; // Default latest
            });

            if (filtered.length === 0) {
                listEl.innerHTML = `
                    <div class="text-center py-12 px-4 border-2 border-dashed border-slate-200 dark:border-slate-800 rounded-2xl">
                        <i class="fa-regular fa-clipboard text-3xl text-slate-300 dark:text-slate-600 mb-2"></i>
                        <p class="text-sm font-medium text-slate-500 dark:text-slate-400">No tasks found</p>
                        <p class="text-xs text-slate-400 dark:text-slate-500 mt-1">Try entering an AI prompt goal above or changing filter criteria.</p>
                    </div>
                `;
                updateDashboardStats();
                return;
            }

            // Group by Phases if applicable
            let html = '';
            let currentPhase = null;

            filtered.forEach(task => {
                // Render Phase Header if changed
                if (task.phase && task.phase !== currentPhase) {
                    currentPhase = task.phase;
                    html += `
                        <div class="pt-3 pb-1 flex items-center gap-2">
                            <span class="text-xs font-bold uppercase tracking-wider text-slate-400 dark:text-slate-500">${escapeHtml(currentPhase)}</span>
                            <div class="h-px bg-slate-200 dark:bg-slate-800 flex-grow"></div>
                        </div>
                    `;
                }

                const priorityColors = {
                    'High': 'bg-red-50 dark:bg-red-950/40 text-red-600 dark:text-red-400 border-red-200 dark:border-red-900/50',
                    'Medium': 'bg-amber-50 dark:bg-amber-950/40 text-amber-600 dark:text-amber-400 border-amber-200 dark:border-amber-900/50',
                    'Low': 'bg-slate-100 dark:bg-slate-800 text-slate-600 dark:text-slate-400 border-slate-200 dark:border-slate-700'
                };

                html += `
                    <div class="task-item group flex items-center justify-between p-3.5 rounded-xl bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700/80 shadow-sm hover:shadow transition-all ${task.completed ? 'opacity-60 bg-slate-50/50 dark:bg-slate-800/40' : ''}">
                        <div class="flex items-center space-x-3 min-w-0 flex-grow pr-2">
                            <!-- Custom Checkbox -->
                            <button onclick="toggleTask('${task.id}')" class="w-5 h-5 rounded-md border ${task.completed ? 'bg-emerald-500 border-emerald-500 text-white' : 'border-slate-300 dark:border-slate-600 hover:border-blue-500'} flex items-center justify-center transition-colors flex-shrink-0">
                                ${task.completed ? '<i class="fa-solid fa-check text-xs"></i>' : ''}
                            </button>

                            <!-- Task Details -->
                            <div class="min-w-0 flex-grow">
                                <p class="text-sm font-medium text-slate-800 dark:text-slate-100 truncate ${task.completed ? 'line-through text-slate-400 dark:text-slate-500' : ''}">
                                    ${escapeHtml(task.title)}
                                </p>
                                <div class="flex items-center gap-2 mt-1 flex-wrap text-[11px] text-slate-400 dark:text-slate-500">
                                    <span class="px-2 py-0.5 rounded-md font-medium border ${priorityColors[task.priority]}">
                                        ${task.priority}
                                    </span>
                                    <span class="bg-slate-100 dark:bg-slate-700/60 px-2 py-0.5 rounded-md text-slate-600 dark:text-slate-300">
                                        #${escapeHtml(task.category)}
                                    </span>
                                    <span class="flex items-center gap-1">
                                        <i class="fa-regular fa-clock text-[10px]"></i> ${task.estTime}
                                    </span>
                                    <span class="flex items-center gap-1">
                                        <i class="fa-regular fa-calendar text-[10px]"></i> ${task.dueDate}
                                    </span>
                                </div>
                            </div>
                        </div>

                        <!-- Action Buttons -->
                        <div class="flex items-center space-x-1 opacity-80 group-hover:opacity-100 transition-opacity">
                            <button onclick="deleteTask('${task.id}')" class="p-1.5 rounded-lg text-slate-400 hover:text-red-500 hover:bg-red-50 dark:hover:bg-red-950/40 transition-colors" title="Delete Task">
                                <i class="fa-regular fa-trash-can text-xs"></i>
                            </button>
                        </div>
                    </div>
                `;
            });

            listEl.innerHTML = html;
            updateDashboardStats();
        }

        // --- Task State Mutations ---
        function toggleTask(id) {
            tasks = tasks.map(t => t.id === id ? { ...t, completed: !t.completed } : t);
            saveTasks();
            renderTasks();
        }

        function deleteTask(id) {
            tasks = tasks.filter(t => t.id !== id);
            saveTasks();
            renderTasks();
            showToast("Task removed", "fa-trash-can", "text-slate-400");
        }

        function clearCompletedTasks() {
            const prevLen = tasks.length;
            tasks = tasks.filter(t => !t.completed);
            saveTasks();
            renderTasks();
            const removed = prevLen - tasks.length;
            showToast(`Cleared ${removed} completed task(s)`, "fa-circle-check", "text-blue-400");
        }

        // --- Filter Tabs Selector ---
        function setFilter(filter) {
            currentFilter = filter;
            ['all', 'pending', 'completed', 'high'].forEach(f => {
                const btn = document.getElementById(`filterBtn-${f}`);
                if (f === filter) {
                    btn.className = "px-3 py-1.5 rounded-lg text-xs font-semibold bg-blue-50 text-blue-600 dark:bg-blue-900/40 dark:text-blue-300 transition-colors";
                } else {
                    btn.className = "px-3 py-1.5 rounded-lg text-xs font-medium text-slate-600 dark:text-slate-300 hover:bg-slate-100 dark:hover:bg-slate-700 transition-colors";
                }
            });
            renderTasks();
        }

        // --- Progress Dashboard Calculations ---
        function updateDashboardStats() {
            const total = tasks.length;
            const completed = tasks.filter(t => t.completed).length;
            const pending = total - completed;
            const percentage = total === 0 ? 0 : Math.round((completed / total) * 100);

            document.getElementById('statTotal').innerText = total;
            document.getElementById('statPending').innerText = pending;
            document.getElementById('statDone').innerText = completed;
            document.getElementById('progressBar').style.width = `${percentage}%`;
            document.getElementById('progressSubtitle').innerText = `${completed} of ${total} tasks completed (${percentage}%)`;
        }

        // --- Exporters ---
        function toggleExportMenu() {
            const modal = document.getElementById('exportModal');
            modal.classList.toggle('hidden');
            modal.classList.toggle('flex');
        }

        function copyAsMarkdown() {
            if (tasks.length === 0) {
                showToast("No tasks available to export!", "fa-triangle-exclamation", "text-amber-400");
                return;
            }

            let markdown = `# Project Task Checklist\n\n`;
            let currentPhase = '';

            tasks.forEach(t => {
                if (t.phase && t.phase !== currentPhase) {
                    currentPhase = t.phase;
                    markdown += `\n### ${currentPhase}\n`;
                }
                const check = t.completed ? '[x]' : '[ ]';
                markdown += `- ${check} **${t.title}** (Priority: ${t.priority}, Est: ${t.estTime}, Category: #${t.category})\n`;
            });

            // Copy to clipboard via standard text fallback
            const dummy = document.createElement("textarea");
            document.body.appendChild(dummy);
            dummy.value = markdown;
            dummy.select();
            document.execCommand("copy");
            document.body.removeChild(dummy);

            toggleExportMenu();
            showToast("Copied Markdown checklist to clipboard!", "fa-copy", "text-emerald-400");
        }

        function downloadAsTxt() {
            if (tasks.length === 0) return;

            let textContent = "PROJECT TO-DO LIST\n==================\n\n";
            tasks.forEach((t, i) => {
                const status = t.completed ? "[DONE]" : "[PENDING]";
                textContent += `${i + 1}. ${status} ${t.title}\n   Priority: ${t.priority} | Est Time: ${t.estTime} | Tag: #${t.category}\n\n`;
            });

            const blob = new Blob([textContent], { type: 'text/plain' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `TaskAI_Checklist_${new Date().toISOString().slice(0, 10)}.txt`;
            a.click();
            URL.revokeObjectURL(url);

            toggleExportMenu();
            showToast("Downloaded text checklist file!", "fa-file-lines", "text-emerald-400");
        }

        // --- Theme Switcher ---
        function toggleTheme() {
            const html = document.documentElement;
            const icon = document.getElementById('themeIcon');
            if (html.classList.contains('dark')) {
                html.classList.remove('dark');
                localStorage.setItem('theme', 'light');
                icon.className = 'fa-solid fa-moon';
            } else {
                html.classList.add('dark');
                localStorage.setItem('theme', 'dark');
                icon.className = 'fa-solid fa-sun';
            }
        }

        // --- Toast Notification Banner ---
        function showToast(msg, iconClass = "fa-circle-check", iconColor = "text-emerald-400") {
            const toast = document.getElementById('toast');
            const toastMsg = document.getElementById('toastMsg');
            const toastIcon = document.getElementById('toastIcon');

            toastMsg.innerText = msg;
            toastIcon.className = `fa-solid ${iconClass} ${iconColor}`;

            toast.classList.remove('translate-y-20', 'opacity-0');
            toast.classList.add('translate-y-0', 'opacity-100');

            setTimeout(() => {
                toast.classList.add('translate-y-20', 'opacity-0');
                toast.classList.remove('translate-y-0', 'opacity-100');
            }, 3000);
        }

        // --- Helper Utilities ---
        function getRelativeDateString(daysOffset) {
            const d = new Date();
            d.setDate(d.getDate() + daysOffset);
            return d.toISOString().slice(0, 10);
        }

        function escapeHtml(str) {
            return str.replace(/[&<>"']/g, function(m) {
                return {
                    '&': '&amp;',
                    '<': '&lt;',
                    '>': '&gt;',
                    '"': '&quot;',
                    "'": '&#039;'
                }[m];
            });
        }
    </script>
</body>
</html>
