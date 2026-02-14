    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>StudyPlanner Pro | Academic Excellence</title>
        <script src="https://cdn.tailwindcss.com"></script>
        <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;800&display=swap" rel="stylesheet">
        <style>
            :root { --primary: #6366f1; --accent: #f59e0b; }
            body { font-family: 'Inter', sans-serif; scroll-behavior: smooth; overflow-x: hidden; }
            
            /* Glassmorphism & Animations */
            .glass-card { background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border: 1px solid rgba(226, 232, 240, 0.8); }
            .hero-gradient { background: radial-gradient(circle at top right, #eef2ff, #ffffff); }
            
            /* Blind Mode Styles */
            .blind-mode-active { background: #000 !important; color: #fff !important; }
            .blind-mode-active .glass-card { background: #111 !important; border: 3px solid var(--accent) !important; color: #fff !important; }
            .blind-mode-active button { border: 2px solid #fff !important; font-size: 1.5rem !important; }
            .blind-mode-active select, .blind-mode-active input { background: #222 !important; color: #fff !important; border: 2px solid var(--accent) !important; }

            /* Component Logic Visibility */
            .hidden-section { display: none !important; }
            .active-section { display: block !important; animation: fadeIn 0.5s ease-in-out; }
            section { width: 100%; margin: 0; padding: 4rem 1.5rem; }
            @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
            
            /* Voice Features - Only for Blind Students */
            .voice-only { display: none !important; }
            body.blind-mode-active .voice-only { display: inline-block !important; }
            #voiceQuizBtn { display: none !important; }
            body.blind-mode-active #voiceQuizBtn { display: flex !important; }

            .pulse-ring { position: absolute; width: 100%; height: 100%; border-radius: 50%; background: var(--primary); opacity: 0.6; animation: pulse 2s infinite; }
            @keyframes pulse { 0% { transform: scale(1); opacity: 0.6; } 100% { transform: scale(1.8); opacity: 0; } }
            
            /* Listening Indicator */
            .listening-indicator { position: fixed; top-6 right-6; z-index: 9998; }
            .listening-dot { width: 12px; height: 12px; background: #ef4444; border-radius: 50%; display: inline-block; animation: listening-pulse 0.6s infinite; }
            @keyframes listening-pulse { 0%, 100% { opacity: 1; transform: scale(1); } 50% { opacity: 0.5; transform: scale(1.2); } }
            .listening-badge { background: #ef4444; color: white; padding: 8px 12px; border-radius: 20px; font-size: 12px; font-weight: bold; }
            .blind-mode-active .listening-badge { font-size: 16px; padding: 12px 16px; border: 2px solid white; }
        </style>
    </head>
    <body class="bg-slate-50 text-slate-900 hero-gradient">

        <div id="rolePortal" class="fixed inset-0 bg-indigo-600 z-[9999] flex flex-col items-center justify-center p-6 transition-all duration-700">
            <div class="text-center mb-12">
                <h1 class="text-6xl font-black text-white mb-4 tracking-tight">StudyPlanner</h1>
                <p class="text-indigo-100 text-xl font-light">The ultimate schedule master for Indian Students.</p>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8 w-full max-w-5xl">
                <div onclick="selectRole('student')" class="bg-white/10 hover:bg-white hover:text-indigo-600 p-12 rounded-[40px] cursor-pointer border-2 border-white/20 transition-all group">
                    <span class="text-7xl block mb-6 group-hover:scale-110 transition">📖</span>
                    <h3 class="text-4xl font-bold mb-2">General Student</h3>
                    <p class="opacity-80">Full Visual Planning, Analytics & PDF Schedules.</p>
                </div>
                <div onclick="selectRole('blind')" class="bg-white/10 hover:bg-white hover:text-indigo-600 p-12 rounded-[40px] cursor-pointer border-2 border-white/20 transition-all group">
                    <span class="text-7xl block mb-6 group-hover:scale-110 transition text-orange-400">🎙️</span>
                    <h3 class="text-4xl font-bold mb-2">Blind Student</h3>
                    <p class="opacity-80">Voice-Only Navigation & Audio Question Banks.</p>
                </div>
            </div>
        </div>

        <nav id="mainNav" class="hidden sticky top-0 bg-white/80 backdrop-blur-md border-b z-50">
            <div class="max-w-7xl mx-auto px-6 h-20 flex justify-between items-center">
                <div class="flex items-center gap-3">
                    <div class="w-12 h-12 bg-indigo-600 rounded-2xl flex items-center justify-center text-white font-black text-xl shadow-lg">SP</div>
                    <span class="text-2xl font-black text-slate-800 tracking-tighter">StudyPlanner</span>
                </div>
                <div id="authNav" class="flex items-center gap-6">
                    <button onclick="openAuth('login')" class="font-bold text-slate-500 hover:text-indigo-600">Login</button>
                    <button onclick="openAuth('register')" class="bg-indigo-600 text-white px-8 py-3 rounded-2xl font-bold shadow-indigo-200 shadow-xl hover:bg-indigo-700 transition">Join Now</button>
                </div>
                <div id="userNav" class="hidden flex items-center gap-4">
                    <div class="text-right">
                        <p id="navName" class="font-bold text-slate-800 leading-none"></p>
                        <span id="navRole" class="text-xs font-bold text-indigo-500 uppercase"></span>
                    </div>
                    <button onclick="location.reload()" class="w-10 h-10 bg-slate-100 rounded-full flex items-center justify-center text-red-500 hover:bg-red-50">✕</button>
                </div>
            </div>
        </nav>

        <main class="max-w-7xl mx-auto px-6">
            
            <section id="heroSection" class="hidden-section py-24 text-center">
                <h1 class="text-7xl font-black text-slate-900 mb-8 leading-[1.1]">Organize Your Study Life <br><span class="text-indigo-600">Like a Pro.</span></h1>
                <p class="text-xl text-slate-500 max-w-2xl mx-auto mb-12">Automated timetables, AI subject recommendations, and dedicated accessibility for students with vision impairment.</p>
                <div class="flex flex-wrap justify-center gap-4">
                    <button onclick="showSection('portalSection')" class="bg-indigo-600 text-white px-10 py-5 rounded-2xl font-bold text-lg shadow-2xl hover:bg-indigo-700 transition">Get Started</button>
                    <button onclick="openAuth('login')" class="bg-slate-900 text-white px-10 py-5 rounded-2xl font-bold text-lg shadow-2xl hover:bg-black transition">Login to Existing Account</button>
                </div>
            </section>

            <section id="portalSection" class="hidden-section py-24">
                <div class="max-w-4xl mx-auto">
                    <h2 class="text-5xl font-black text-center mb-4">Select Your Institution Type</h2>
                    <p class="text-center text-slate-500 text-lg mb-16 max-w-2xl mx-auto">Choose whether you're a school student or pursuing higher education to get a personalized setup.</p>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 max-w-4xl mx-auto mb-12">
                        <div class="glass-card p-12 rounded-[40px] text-center cursor-pointer hover:border-indigo-600 hover:shadow-2xl transition-all group transform hover:scale-105" onclick="handleSchoolingClick()" style="cursor: pointer;">
                            <span class="text-7xl block mb-6 group-hover:rotate-12 transition">🎒</span>
                            <h3 class="text-3xl font-bold mb-3">Schooling</h3>
                            <p class="text-slate-600">Class 6th to Class 12th</p>
                            <p class="text-sm text-slate-500 mt-2">CBSE • ICSE • State Board</p>
                        </div>
                        <div class="glass-card p-12 rounded-[40px] text-center cursor-pointer hover:border-indigo-600 hover:shadow-2xl transition-all group transform hover:scale-105" onclick="handleCollegeClick()" style="cursor: pointer;">
                            <span class="text-7xl block mb-6 group-hover:-rotate-12 transition">🎓</span>
                            <h3 class="text-3xl font-bold mb-3">College / University</h3>
                            <p class="text-slate-600">Higher Education Programs</p>
                            <p class="text-sm text-slate-500 mt-2">B.Tech • MCA • B.Sc • B.Com</p>
                        </div>
                    </div>
                    <div class="text-center">
                        <p class="text-slate-500 mb-4">Already have an account?</p>
                        <button onclick="openAuth('login')" class="bg-slate-600 text-white px-8 py-3 rounded-2xl font-bold hover:bg-slate-700 transition">Login Here</button>
                    </div>
                </div>
            </section>

            <section id="schoolingSection" class="hidden-section py-24">
                <div class="max-w-5xl mx-auto">
                    <h2 class="text-5xl font-black text-center mb-4">Select Your Board</h2>
                    <p class="text-center text-slate-500 text-lg mb-16">Choose which educational board your school follows</p>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-8 mb-12">
                        <div class="glass-card p-10 rounded-[40px] text-center cursor-pointer hover:border-indigo-600 hover:shadow-2xl transition-all group transform hover:scale-105" onclick="selectBoard('cbse')">
                            <span class="text-6xl block mb-4 group-hover:scale-110 transition">📋</span>
                            <h3 class="text-2xl font-bold mb-2">CBSE</h3>
                            <p class="text-slate-600">Central Board of Secondary Education</p>
                            <p class="text-xs text-slate-500 mt-3">Most popular across India</p>
                        </div>
                        <div class="glass-card p-10 rounded-[40px] text-center cursor-pointer hover:border-indigo-600 hover:shadow-2xl transition-all group transform hover:scale-105" onclick="selectBoard('icse')">
                            <span class="text-6xl block mb-4 group-hover:scale-110 transition">📚</span>
                            <h3 class="text-2xl font-bold mb-2">ICSE</h3>
                            <p class="text-slate-600">Indian School Board</p>
                            <p class="text-xs text-slate-500 mt-3">Full form: CISCE</p>
                        </div>
                        <div class="glass-card p-10 rounded-[40px] text-center cursor-pointer hover:border-indigo-600 hover:shadow-2xl transition-all group transform hover:scale-105" onclick="selectBoard('state')">
                            <span class="text-6xl block mb-4 group-hover:scale-110 transition">🏛️</span>
                            <h3 class="text-2xl font-bold mb-2">State Board</h3>
                            <p class="text-slate-600">Regional Education Board</p>
                            <p class="text-xs text-slate-500 mt-3">Your state curriculum</p>
                        </div>
                    </div>
                    <div class="text-center">
                        <button onclick="showSection('portalSection')" class="bg-slate-200 text-slate-700 px-8 py-3 rounded-2xl font-bold hover:bg-slate-300 transition">← Back to Institution Selection</button>
                    </div>
                </div>
            </section>

            <section id="collegeSection" class="hidden-section py-24">
                <div class="max-w-5xl mx-auto">
                    <h2 class="text-5xl font-black text-center mb-4">Select Your Degree Program</h2>
                    <p class="text-center text-slate-500 text-lg mb-16">Choose your degree or program of study</p>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mb-12">
                        <div class="glass-card p-10 rounded-[40px] text-center cursor-pointer hover:border-indigo-600 hover:shadow-2xl transition-all group transform hover:scale-105" onclick="selectDegree('btech')">
                            <span class="text-6xl block mb-4 group-hover:scale-110 transition">💻</span>
                            <h3 class="text-2xl font-bold mb-2">B.Tech</h3>
                            <p class="text-slate-600">Bachelor of Technology</p>
                            <p class="text-xs text-slate-500 mt-3">4-year engineering program</p>
                        </div>
                        <div class="glass-card p-10 rounded-[40px] text-center cursor-pointer hover:border-indigo-600 hover:shadow-2xl transition-all group transform hover:scale-105" onclick="selectDegree('mca')">
                            <span class="text-6xl block mb-4 group-hover:scale-110 transition">🎓</span>
                            <h3 class="text-2xl font-bold mb-2">MCA</h3>
                            <p class="text-slate-600">Master of Computer Applications</p>
                            <p class="text-xs text-slate-500 mt-3">3-year postgraduate program</p>
                        </div>
                        <div class="glass-card p-10 rounded-[40px] text-center cursor-pointer hover:border-indigo-600 hover:shadow-2xl transition-all group transform hover:scale-105" onclick="selectDegree('bsc')">
                            <span class="text-6xl block mb-4 group-hover:scale-110 transition">🔬</span>
                            <h3 class="text-2xl font-bold mb-2">B.Sc</h3>
                            <p class="text-slate-600">Bachelor of Science</p>
                            <p class="text-xs text-slate-500 mt-3">3-year science program</p>
                        </div>
                        <div class="glass-card p-10 rounded-[40px] text-center cursor-pointer hover:border-indigo-600 hover:shadow-2xl transition-all group transform hover:scale-105" onclick="selectDegree('bcom')">
                            <span class="text-6xl block mb-4 group-hover:scale-110 transition">📊</span>
                            <h3 class="text-2xl font-bold mb-2">B.Com</h3>
                            <p class="text-slate-600">Bachelor of Commerce</p>
                            <p class="text-xs text-slate-500 mt-3">3-year commerce program</p>
                        </div>
                    </div>
                    <div class="text-center">
                        <button onclick="showSection('portalSection')" class="bg-slate-200 text-slate-700 px-8 py-3 rounded-2xl font-bold hover:bg-slate-300 transition">← Back to Institution Selection</button>
                    </div>
                </div>
            </section>

            <section id="classSection" class="hidden-section py-24">
                <div class="max-w-5xl mx-auto">
                    <h2 class="text-5xl font-black text-center mb-4" id="classSectionTitle">Select Your Class / Year</h2>
                    <p class="text-center text-slate-500 text-lg mb-16">Which year or semester are you in?</p>
                    <div id="classGrid" class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-12">
                        <!-- Classes will be populated by JavaScript -->
                    </div>
                    <div class="text-center">
                        <button onclick="goBackToInstitution()" class="bg-slate-200 text-slate-700 px-8 py-3 rounded-2xl font-bold hover:bg-slate-300 transition">← Back to Board Selection</button>
                    </div>
                </div>
            </section>

            <section id="configSection" class="hidden-section py-24">
                <div class="max-w-2xl mx-auto glass-card p-12 rounded-[40px] border-2 border-indigo-200">
                    <h2 class="text-4xl font-black mb-2 text-center">Complete Your Profile</h2>
                    <p class="text-center text-slate-500 mb-8">Select your subjects to create your personalized schedule</p>
                    <div class="space-y-6">
                        <div>
                            <label class="block font-black text-xs uppercase tracking-widest text-slate-400 mb-3">Grade / Year</label>
                            <select id="levelDrop" onchange="renderSubjects()" class="w-full p-4 bg-slate-50 border-2 border-slate-200 rounded-2xl font-bold focus:border-indigo-600 focus:bg-white outline-none transition"></select>
                        </div>
                        <div>
                            <label class="block font-black text-xs uppercase tracking-widest text-slate-400 mb-3">Select Your Subjects</label>
                            <div id="subjectBox" class="grid grid-cols-2 gap-3 max-h-64 overflow-y-auto"></div>
                        </div>
                        <button onclick="saveProfile()" class="w-full bg-indigo-600 text-white py-5 rounded-2xl font-bold text-lg shadow-xl shadow-indigo-100 hover:bg-indigo-700 transition transform hover:scale-105">Generate My Dashboard</button>
                    </div>
                </div>
            </section>

            <section id="dashboardSection" class="hidden-section py-12">
                <div class="flex flex-wrap justify-between items-end gap-6 mb-12">
                    <div>
                        <h1 class="text-5xl font-black text-slate-900 mb-2">Hello, <span id="dashUser" class="text-indigo-600">Achiever</span>!</h1>
                        <p id="dashPath" class="text-slate-400 font-bold uppercase tracking-tighter"></p>
                    </div>
                    <div class="flex gap-3">
                        <button onclick="generateSchedule()" class="bg-indigo-50 text-indigo-600 px-6 py-3 rounded-xl font-bold hover:bg-indigo-100 transition">📅 Refresh Timetable</button>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-4 gap-6 mb-12">
                    <div class="bg-indigo-600 p-8 rounded-[35px] text-white">
                        <p class="opacity-80 font-bold">Today's Focus</p>
                        <h4 id="focusSub" class="text-2xl font-black mt-2">Mathematics</h4>
                    </div>
                    <div class="glass-card p-8 rounded-[35px]">
                        <p class="text-slate-400 font-bold">Study Streak</p>
                        <h4 class="text-3xl font-black mt-2">12 Days</h4>
                    </div>
                    <div class="glass-card p-8 rounded-[35px]">
                        <p class="text-slate-400 font-bold">Quiz Average</p>
                        <h4 class="text-3xl font-black mt-2">88%</h4>
                    </div>
                    <div class="glass-card p-8 rounded-[35px]">
                        <p class="text-slate-400 font-bold">Completed Goals</p>
                        <h4 class="text-3xl font-black mt-2">42</h4>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                    <div class="lg:col-span-2 space-y-6">
                        <div class="glass-card p-8 rounded-[40px]">
                            <h3 class="text-2xl font-bold mb-6 flex items-center gap-2"><span>📊</span> Your Smart Timetable</h3>
                            <div id="timetableGrid" class="space-y-4">
                                </div>
                        </div>
                    </div>
                    <div>
                        <button id="voiceQuizBtn" onclick="startVoiceQuiz()" class="voice-only w-full bg-orange-500 text-white p-10 rounded-[40px] shadow-2xl hover:bg-orange-600 transition-all hover:scale-[1.02] flex flex-col items-center text-center">
                            <span class="text-6xl mb-4">🎙️</span>
                            <h3 class="text-2xl font-black mb-2">Practice Assessment</h3>
                            <p class="opacity-90">Voice-guided MCQ test for your selected subjects.</p>
                        </button>
                        <button id="textQuizBtn" onclick="startTextQuiz()" class="w-full bg-indigo-500 text-white p-10 rounded-[40px] shadow-2xl hover:bg-indigo-600 transition-all hover:scale-[1.02] flex flex-col items-center text-center" style="display: none;">
                            <span class="text-6xl mb-4">📝</span>
                            <h3 class="text-2xl font-black mb-2">Practice Assessment</h3>
                            <p class="opacity-90">MCQ test for your selected subjects.</p>
                        </button>
                    </div>
                </div>
            </section>
        </main>

        <div id="authOverlay" class="fixed inset-0 bg-slate-900/80 backdrop-blur-md z-[1000] hidden flex items-center justify-center p-6">
            <div class="bg-white w-full max-w-md p-10 rounded-[45px] relative">
                <button onclick="closeAuth()" class="absolute top-8 right-8 text-slate-300 hover:text-slate-600 text-2xl">✕</button>
                <h2 id="authTitle" class="text-3xl font-black mb-8 text-center">Account</h2>
                
                <div id="authForm" class="space-y-4">
                    <div id="nameWrap">
                        <label class="text-[10px] font-black uppercase text-slate-400 tracking-widest ml-2">Full Name</label>
                        <input type="text" id="inName" class="w-full p-5 bg-slate-50 border-2 border-slate-100 rounded-2xl outline-none focus:border-indigo-600 font-bold">
                        <button type="button" id="voiceNameBtn" onclick="activateVoiceInput('name')" class="voice-only mt-2 text-xs font-bold text-indigo-600 hover:text-indigo-700">🎙️ Say your name</button>
                    </div>
                    <div>
                        <label class="text-[10px] font-black uppercase text-slate-400 tracking-widest ml-2">Mobile / Email</label>
                        <input type="text" id="inContact" class="w-full p-5 bg-slate-50 border-2 border-slate-100 rounded-2xl outline-none focus:border-indigo-600 font-bold">
                        <button type="button" id="voiceContactBtn" onclick="activateVoiceInput('contact')" class="voice-only mt-2 text-xs font-bold text-indigo-600 hover:text-indigo-700">🎙️ Say your contact</button>
                    </div>
                    <div id="otpWrap" class="hidden">
                        <label class="text-[10px] font-black uppercase text-green-500 tracking-widest ml-2">Enter 6-Digit OTP</label>
                        <input type="text" id="inOtp" maxlength="6" class="w-full p-5 bg-green-50 border-2 border-green-100 rounded-2xl text-center text-3xl font-black tracking-[10px]">
                        <button type="button" id="voiceOtpBtn" onclick="activateVoiceInput('otp')" class="voice-only mt-2 text-xs font-bold text-green-600 hover:text-green-700">🎙️ Say your OTP</button>
                    </div>
                    <button onclick="processAuth()" id="authBtn" class="w-full bg-indigo-600 text-white py-5 rounded-2xl font-bold text-lg shadow-xl hover:bg-indigo-700 transition">Continue</button>
                </div>
            </div>
        </div>

        <div id="quizOverlay" class="fixed inset-0 bg-slate-950 z-[10000] hidden flex flex-col items-center justify-center text-white p-10 text-center">
            <div class="max-w-4xl w-full">
                <div id="quizLoader" class="text-orange-400 font-black text-2xl mb-8 animate-pulse">Initializing Voice AI...</div>
                <p id="quizCat" class="text-indigo-400 font-bold tracking-widest uppercase mb-4"></p>
                <h2 id="quizQ" class="text-4xl md:text-6xl font-black mb-16 leading-tight"></h2>
                <div id="quizOptions" class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-12"></div>
                <button onclick="endQuiz()" class="text-slate-500 font-bold border-b border-slate-800">Exit Assessment</button>
            </div>
        </div>

        <div id="aiBot" class="fixed bottom-10 right-10 w-20 h-20 bg-indigo-600 rounded-full hidden flex items-center justify-center cursor-pointer shadow-2xl z-50 border-4 border-white" onclick="triggerAi()">
            <div class="pulse-ring"></div>
            <span class="text-3xl">🤖</span>
        </div>

        <div id="listeningIndicator" class="listening-indicator hidden">
            <div class="listening-badge">
                <span class="listening-dot"></span> Listening...
            </div>
        </div>

        <script>
            // --- GLOBAL STATE ---
            let state = {
                user: { id: null, name: '', contact: '', role: 'student', auth: false, portal: '', level: '', subs: [] },
                isOtp: false,
                recognition: null,
                isListening: false,
                inputMode: null,
                suppressAutoRestart: false,
                quizSetup: { classSelected: null, subjectSelected: null, quizType: null },
                language: 'en-US',  // 'en-US' for English, 'hi-IN' for Hindi
                apiUrl: 'http://localhost:3000/api'  // Backend API URL
            };

            const ACADEMIC_DB = {
                school: {
                    levels: ["Class 6", "Class 7", "Class 8", "Class 9", "Class 10", "Class 11 Science", "Class 11 Commerce", "Class 12 Science", "Class 12 Commerce"],
                    subjects: {
                        "General": ["Mathematics", "Science", "Social Science", "English", "Hindi"],
                        "Science": ["Physics", "Chemistry", "Mathematics", "Biology", "English", "CS"],
                        "Commerce": ["Accountancy", "Business Studies", "Economics", "Mathematics", "English"]
                    }
                },
                college: {
                    levels: ["B.Tech CS", "B.Tech IT", "B.Tech Mech", "MCA", "B.Sc Physics", "B.Com"],
                    subjects: {
                        "Tech": ["Data Structures", "Algorithms", "Operating Systems", "DBMS", "Software Eng."],
                        "General": ["Advanced Calculus", "Macro Economics", "Corporate Law", "Quantum Physics"]
                    }
                }
            };

            const QUIZ_BANK = [
                // School - Mathematics
                { id: 1, level: "any", sub: "Mathematics", q: "What is the value of 7 × 8?", o: ["54", "56", "58", "64"], a: 1 },
                { id: 2, level: "any", sub: "Mathematics", q: "What is the derivative of x^2?", o: ["2x", "x", "x^2", "1"], a: 0 },
                { id: 3, level: "any", sub: "Mathematics", q: "What is 15% of 200?", o: ["25", "30", "35", "40"], a: 1 },
                { id: 4, level: "any", sub: "Mathematics", q: "The LCM of 4 and 6 is?", o: ["12", "10", "8", "24"], a: 0 },

                // School - Science (general)
                { id: 5, level: "any", sub: "Science", q: "Which organ pumps blood throughout the body?", o: ["Lungs", "Liver", "Heart", "Kidney"], a: 2 },
                { id: 6, level: "any", sub: "Science", q: "Which of these is a non-metal?", o: ["Iron", "Copper", "Oxygen", "Gold"], a: 2 },
                { id: 7, level: "any", sub: "Science", q: "What is the boiling point of water at sea level (°C)?", o: ["90", "95", "100", "105"], a: 2 },
                { id: 8, level: "any", sub: "Science", q: "Which process do plants use to make food?", o: ["Respiration", "Photosynthesis", "Digestion", "Transpiration"], a: 1 },

                // School - Social Science
                { id: 9, level: "any", sub: "Social Science", q: "Who was the first Prime Minister of India?", o: ["Mahatma Gandhi", "Jawaharlal Nehru", "Sardar Patel", "Indira Gandhi"], a: 1 },
                { id: 10, level: "any", sub: "Social Science", q: "The Tropic of Cancer passes through which country?", o: ["India", "Brazil", "Canada", "Australia"], a: 0 },
                { id: 11, level: "any", sub: "Social Science", q: "Which river is the longest in the world?", o: ["Amazon", "Nile", "Ganges", "Yangtze"], a: 1 },
                { id: 12, level: "any", sub: "Social Science", q: "What is the study of past human events called?", o: ["Geography", "Civics", "History", "Economics"], a: 2 },

                // School - English
                { id: 13, level: "any", sub: "English", q: "Choose the correct plural form of 'mouse'.", o: ["mouses", "mice", "meese", "mices"], a: 1 },
                { id: 14, level: "any", sub: "English", q: "Identify the adjective: 'A quick brown fox'.", o: ["quick", "fox", "brown", "A"], a: 0 },
                { id: 15, level: "any", sub: "English", q: "Which is a synonym of 'happy'?", o: ["sad", "angry", "joyful", "tired"], a: 2 },
                { id: 16, level: "any", sub: "English", q: "Fill in: She _____ to school every day.", o: ["go", "goes", "gone", "going"], a: 1 },

                // School - Hindi
                { id: 17, level: "any", sub: "Hindi", q: "What is the Hindi word for 'water'?", o: ["Aag", "Paani", "Hawa", "Dharti"], a: 1 },
                { id: 18, level: "any", sub: "Hindi", q: "Which script is used to write Hindi?", o: ["Latin", "Devanagari", "Bengali", "Telugu"], a: 1 },
                { id: 19, level: "any", sub: "Hindi", q: "Translate: 'Ram went home' into Hindi (simple).", o: ["Ram ghar gaya", "Ram ja raha hai", "Ram kheta hai", "Ram khata hai"], a: 0 },
                { id: 20, level: "any", sub: "Hindi", q: "Which is a Hindi vowel?", o: ["क", "ख", "आ", "ग"], a: 2 },

                // School Science - Physics
                { id: 21, level: "any", sub: "Physics", q: "Unit of force is?", o: ["Joule", "Newton", "Watt", "Pascal"], a: 1 },
                { id: 22, level: "any", sub: "Physics", q: "Speed of light is approximately (km/s):", o: ["300,000", "150,000", "30,000", "3,000"], a: 0 },
                { id: 23, level: "any", sub: "Physics", q: "An object in motion stays in motion according to which law?", o: ["Gravity", "Conservation", "Newton's First Law", "Thermodynamics"], a: 2 },
                { id: 24, level: "any", sub: "Physics", q: "SI unit of electric current is?", o: ["Volt", "Ampere", "Ohm", "Coulomb"], a: 1 },

                // School Science - Chemistry
                { id: 25, level: "any", sub: "Chemistry", q: "What is H2O commonly known as?", o: ["Hydrogen Peroxide", "Water", "Ozone", "Hydrogen"], a: 1 },
                { id: 26, level: "any", sub: "Chemistry", q: "pH value below 7 indicates?", o: ["Neutral", "Acidic", "Alkaline", "Basic"], a: 1 },
                { id: 27, level: "any", sub: "Chemistry", q: "Which element has atomic number 1?", o: ["Helium", "Hydrogen", "Oxygen", "Carbon"], a: 1 },
                { id: 28, level: "any", sub: "Chemistry", q: "Salt is formed by reaction of an acid and a _____.", o: ["Metal", "Base", "Gas", "Alcohol"], a: 1 },

                // School Science - Biology
                { id: 29, level: "any", sub: "Biology", q: "Which organelle is called the powerhouse of the cell?", o: ["Nucleus", "Mitochondria", "Ribosome", "Chloroplast"], a: 1 },
                { id: 30, level: "any", sub: "Biology", q: "Which group of animals lays eggs?", o: ["Mammals", "Birds", "Amphibians", "All of the above"], a: 1 },
                { id: 31, level: "any", sub: "Biology", q: "Photosynthesis mainly occurs in which part of a plant?", o: ["Roots", "Stem", "Leaves", "Flowers"], a: 2 },
                { id: 32, level: "any", sub: "Biology", q: "Blood is pumped by the _____.", o: ["Liver", "Heart", "Kidney", "Lungs"], a: 1 },

                // School Science - CS
                { id: 33, level: "any", sub: "CS", q: "What does CPU stand for?", o: ["Central Processing Unit", "Computer Primary Unit", "Central Power Unit", "Compute Performance Unit"], a: 0 },
                { id: 34, level: "any", sub: "CS", q: "Which is a programming language?", o: ["HTTP", "HTML", "Python", "CSS"], a: 2 },
                { id: 35, level: "any", sub: "CS", q: "What does 'GUI' stand for?", o: ["Graphical User Interface", "General User Input", "Global UI", "Graphical Unit Input"], a: 0 },
                { id: 36, level: "any", sub: "CS", q: "Which device is used to store data permanently?", o: ["RAM", "ROM", "Hard Disk", "CPU"], a: 2 },

                // Commerce - Accountancy
                { id: 37, level: "any", sub: "Accountancy", q: "What is debit in accounting?", o: ["An increase in liabilities", "An entry on the left side", "Always a credit", "A profit"], a: 1 },
                { id: 38, level: "any", sub: "Accountancy", q: "Balance sheet shows?", o: ["Profit only", "Liabilities and Assets", "Cash flow", "Revenue"], a: 1 },
                { id: 39, level: "any", sub: "Accountancy", q: "Inventory is shown under which heading?", o: ["Assets", "Liabilities", "Expenses", "Income"], a: 0 },
                { id: 40, level: "any", sub: "Accountancy", q: "Which document records sales on credit?", o: ["Invoice", "Receipt", "Ledger", "Voucher"], a: 0 },

                // Commerce - Business Studies
                { id: 41, level: "any", sub: "Business Studies", q: "What is entrepreneurship?", o: ["Working for others", "Starting a new business", "Studying business only", "Managing taxes"], a: 1 },
                { id: 42, level: "any", sub: "Business Studies", q: "Marketing primarily focuses on?", o: ["Manufacturing", "Selling and promotion", "Accounting", "Legal affairs"], a: 1 },
                { id: 43, level: "any", sub: "Business Studies", q: "A mission statement defines?", o: ["Daily tasks", "Long-term purpose", "Employee salaries", "Inventory"], a: 1 },
                { id: 44, level: "any", sub: "Business Studies", q: "Expansion of business is called?", o: ["Closure", "Diversification", "Consolidation", "Bankruptcy"], a: 1 },

                // Commerce - Economics
                { id: 45, level: "any", sub: "Economics", q: "GDP measures?", o: ["Population", "Total market value of goods/services", "GDP growth rate only", "Exports only"], a: 1 },
                { id: 46, level: "any", sub: "Economics", q: "Demand increases when?", o: ["Price rises", "Price falls", "Supply decreases", "Market closes"], a: 1 },
                { id: 47, level: "any", sub: "Economics", q: "Inflation is?", o: ["Rise in general price level", "Fall in wages", "Increase in exports", "Decrease in money supply"], a: 0 },
                { id: 48, level: "any", sub: "Economics", q: "Which is a factor of production?", o: ["Land", "Love", "Law", "Language"], a: 0 },

                // College - Data Structures
                { id: 49, level: "any", sub: "Data Structures", q: "Which data structure uses FIFO?", o: ["Stack", "Queue", "Tree", "Graph"], a: 1 },
                { id: 50, level: "any", sub: "Data Structures", q: "Average-case search time for a balanced BST is?", o: ["O(n)", "O(log n)", "O(1)", "O(n log n)"], a: 1 },
                { id: 51, level: "any", sub: "Data Structures", q: "Which structure is best for LIFO?", o: ["Queue", "Stack", "Heap", "Hash Table"], a: 1 },
                { id: 52, level: "any", sub: "Data Structures", q: "A hash table primarily provides?", o: ["Sequential access", "Random access by key", "Graph traversal", "Sorting"], a: 1 },

                // College - Algorithms
                { id: 53, level: "any", sub: "Algorithms", q: "Big-O for binary search is?", o: ["O(n)", "O(n log n)", "O(log n)", "O(1)"], a: 2 },
                { id: 54, level: "any", sub: "Algorithms", q: "Dijkstra's algorithm solves?", o: ["MST", "Shortest path from source", "Sorting", "Max flow"], a: 1 },
                { id: 55, level: "any", sub: "Algorithms", q: "Which is a greedy algorithm example?", o: ["Merge Sort", "Kruskal's MST", "Quick Sort", "Binary Search"], a: 1 },
                { id: 56, level: "any", sub: "Algorithms", q: "Dynamic programming is mainly used to?", o: ["Divide and conquer", "Avoid recomputation using memoization", "Randomize input", "Graph drawing"], a: 1 },

                // College - Operating Systems
                { id: 57, level: "any", sub: "Operating Systems", q: "What is a process?", o: ["A program in execution", "A file on disk", "A network packet", "An input device"], a: 0 },
                { id: 58, level: "any", sub: "Operating Systems", q: "CPU scheduling aims to?", o: ["Maximize response time", "Minimize throughput", "Efficiently allocate CPU", "Delete processes"], a: 2 },
                { id: 59, level: "any", sub: "Operating Systems", q: "Virtual memory uses?", o: ["Registers", "Disk as extension of RAM", "CPU cache only", "Network storage"], a: 1 },
                { id: 60, level: "any", sub: "Operating Systems", q: "Which is a synchronization primitive?", o: ["Semaphore", "Compiler", "Linker", "Router"], a: 0 },

                // College - DBMS
                { id: 61, level: "any", sub: "DBMS", q: "SQL stands for?", o: ["Structured Query Language", "Simple Query Language", "Structured Question Language", "Sequential Query Language"], a: 0 },
                { id: 62, level: "any", sub: "DBMS", q: "Primary key must be?", o: ["Unique", "Null", "Duplicate", "Non-indexed"], a: 0 },
                { id: 63, level: "any", sub: "DBMS", q: "Which command removes a table?", o: ["DELETE TABLE", "DROP TABLE", "REMOVE TABLE", "TRUNCATE TABLE"], a: 1 },
                { id: 64, level: "any", sub: "DBMS", q: "ACID properties relate to?", o: ["File systems", "Transactions", "Networking", "Operating Systems"], a: 1 },

                // College - Software Engineering
                { id: 65, level: "any", sub: "Software Eng.", q: "SDLC stands for?", o: ["Software Development Life Cycle", "System Development Live Code", "Software Design Loop Cycle", "Server Deployment Lifecycle"], a: 0 },
                { id: 66, level: "any", sub: "Software Eng.", q: "Agile methodology focuses on?", o: ["Long release cycles", "Iterative delivery", "No testing", "Waterfall only"], a: 1 },
                { id: 67, level: "any", sub: "Software Eng.", q: "Which is a testing level?", o: ["Unit testing", "Finance testing", "HR testing", "Payroll testing"], a: 0 },
                { id: 68, level: "any", sub: "Software Eng.", q: "Version control system example?", o: ["Git", "Excel", "Notepad", "PowerPoint"], a: 0 },

                // College General - Advanced Calculus
                { id: 69, level: "any", sub: "Advanced Calculus", q: "The integral of 1/x dx is?", o: ["x", "ln x", "e^x", "1/x^2"], a: 1 },
                { id: 70, level: "any", sub: "Advanced Calculus", q: "A continuous function on a closed interval is?", o: ["Unbounded", "Has a maximum and minimum", "Always differentiable", "Discontinuous"], a: 1 },
                { id: 71, level: "any", sub: "Advanced Calculus", q: "Derivative of sin x is?", o: ["cos x", "-cos x", "sin x", "-sin x"], a: 0 },
                { id: 72, level: "any", sub: "Advanced Calculus", q: "Limit of (1 + 1/n)^n as n→∞ is?", o: ["1", "e", "0", "Infinity"], a: 1 },

                // College General - Macro Economics
                { id: 73, level: "any", sub: "Macro Economics", q: "Fiscal policy is managed by?", o: ["Central bank", "Government", "Private firms", "International bodies"], a: 1 },
                { id: 74, level: "any", sub: "Macro Economics", q: "Unemployment rate measures?", o: ["Employed share", "Unemployed share of labor force", "Total population", "GDP"], a: 1 },
                { id: 75, level: "any", sub: "Macro Economics", q: "Inflation targeting is used to control?", o: ["Prices", "Population", "Exports", "Imports"], a: 0 },
                { id: 76, level: "any", sub: "Macro Economics", q: "Aggregate demand equals?", o: ["C + I + G + (X-M)", "Savings only", "Investment only", "Government spending only"], a: 0 },

                // College General - Corporate Law
                { id: 77, level: "any", sub: "Corporate Law", q: "A 'company' is created by?", o: ["Contract only", "Registration under Companies Act", "Verbal agreement", "Tax filing"], a: 1 },
                { id: 78, level: "any", sub: "Corporate Law", q: "Board of directors is responsible for?", o: ["Day-to-day shop work", "Strategy and oversight", "Only sales", "Only HR"], a: 1 },
                { id: 79, level: "any", sub: "Corporate Law", q: "Memorandum of Association contains?", o: ["Company objectives", "Employee list", "Daily schedule", "Bank details"], a: 0 },
                { id: 80, level: "any", sub: "Corporate Law", q: "Shares represent?", o: ["Debt", "Ownership", "Tax", "Wages"], a: 1 },

                // College General - Quantum Physics
                { id: 81, level: "any", sub: "Quantum Physics", q: "Electron has which charge?", o: ["Positive", "Negative", "Neutral", "Variable"], a: 1 },
                { id: 82, level: "any", sub: "Quantum Physics", q: "Photon is a particle of?", o: ["Matter", "Light", "Sound", "Gravity"], a: 1 },
                { id: 83, level: "any", sub: "Quantum Physics", q: "Heisenberg uncertainty principle relates?", o: ["Position and momentum", "Mass and charge", "Energy and time only", "Temperature and pressure"], a: 0 },
                { id: 84, level: "any", sub: "Quantum Physics", q: "Ground state refers to?", o: ["Highest energy state", "Lowest energy state", "Excited state", "Unstable state"], a: 1 },

                // 5th questions for all subjects
                { id: 85, level: "any", sub: "Mathematics", q: "What is the square root of 144?", o: ["10", "12", "14", "16"], a: 1 },
                { id: 86, level: "any", sub: "Science", q: "What is the speed of sound in air approximately (m/s)?", o: ["100", "200", "340", "500"], a: 2 },
                { id: 87, level: "any", sub: "Social Science", q: "Which city is the capital of India?", o: ["Mumbai", "New Delhi", "Bangalore", "Kolkata"], a: 1 },
                { id: 88, level: "any", sub: "English", q: "What is the past tense of 'eat'?", o: ["eats", "eaten", "ate", "eating"], a: 2 },
                { id: 89, level: "any", sub: "Hindi", q: "What is the Hindi word for 'friend'?", o: ["Dost", "Dushman", "Parivaar", "Padosi"], a: 0 },
                { id: 90, level: "any", sub: "Physics", q: "What is the SI unit of temperature?", o: ["Celsius", "Fahrenheit", "Kelvin", "Rankine"], a: 2 },
                { id: 91, level: "any", sub: "Chemistry", q: "Which element is the most reactive metal?", o: ["Gold", "Sodium", "Lithium", "Potassium"], a: 2 },
                { id: 92, level: "any", sub: "Biology", q: "How many chambers does a human heart have?", o: ["2", "3", "4", "5"], a: 2 },
                { id: 93, level: "any", sub: "CS", q: "What does RAM stand for?", o: ["Read Access Memory", "Random Access Memory", "Real Access Memory", "Rapid Access Memory"], a: 1 },
                { id: 94, level: "any", sub: "Accountancy", q: "What are assets minus liabilities?", o: ["Capital", "Profit", "Equity", "Both A and C"], a: 3 },
                { id: 95, level: "any", sub: "Business Studies", q: "Which type of organization has one owner?", o: ["Partnership", "Sole Proprietorship", "Corporation", "Company"], a: 1 },
                { id: 96, level: "any", sub: "Economics", q: "A country's ability to produce goods efficiently is called?", o: ["Comparative advantage", "Absolute advantage", "Resource management", "Opportunity cost"], a: 0 },
                { id: 97, level: "any", sub: "Data Structures", q: "What is the time complexity of insertion at the beginning of a linked list?", o: ["O(n)", "O(log n)", "O(1)", "O(n^2)"], a: 2 },
                { id: 98, level: "any", sub: "Algorithms", q: "Which algorithm uses a divide and conquer approach?", o: ["Linear Search", "Bubble Sort", "Merge Sort", "Linear Insertion"], a: 2 },
                { id: 99, level: "any", sub: "Operating Systems", q: "What is the primary purpose of paging?", o: ["Speed up CPU", "Manage memory efficiently", "Reduce power consumption", "Increase bandwidth"], a: 1 },
                { id: 100, level: "any", sub: "DBMS", q: "A foreign key must be?", o: ["Unique", "Primary key in another table", "Non-nullable", "Indexed"], a: 1 },
                { id: 101, level: "any", sub: "Software Eng.", q: "Which testing phase checks individual components?", o: ["System testing", "Unit testing", "Integration testing", "UAT"], a: 1 },
                { id: 102, level: "any", sub: "Advanced Calculus", q: "What is the derivative of e^x?", o: ["e^x", "x * e^(x-1)", "e", "1"], a: 0 },
                { id: 103, level: "any", sub: "Macro Economics", q: "Which measure reduces purchasing power?", o: ["Deflation", "Inflation", "Appreciation", "Stabilization"], a: 1 },
                { id: 104, level: "any", sub: "Corporate Law", q: "A partnership agreement is also called?", o: ["Memorandum", "Articles of Association", "Partnership Deed", "Charter"], a: 2 },
                { id: 105, level: "any", sub: "Quantum Physics", q: "Which scientist proposed the photon theory?", o: ["Planck", "Einstein", "Heisenberg", "Bohr"], a: 1 }
            ];

            // --- CORE FUNCTIONS ---
            function selectRole(role) {
                state.user.role = role;
                document.getElementById('rolePortal').style.transform = 'translateY(-100%)';
                document.getElementById('mainNav').classList.remove('hidden');
                
                // Show/hide voice elements based on role
                if(role === 'blind') {
                    // Show voice features for blind students
                    document.querySelectorAll('.voice-only').forEach(el => el.style.display = '');
                    document.body.classList.add('blind-mode-active');
                    
                    // Show hero section WITHOUT resetting voice state
                    showSection('heroSection', true);
                    
                    // Request microphone permission ONLY for blind students
                    if(navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
                        navigator.mediaDevices.getUserMedia({ audio: true })
                            .then(stream => {
                                stream.getTracks().forEach(track => track.stop());
                                // Initialize voice recognition without resetting state
                                initVoice();
                                // Provide welcome guidance
                                speak("Blind Student Mode activated. Microphone access granted. Welcome to StudyPlanner Pro.", () => {
                                    speak("Say 'Register' to create a new account or 'Login' to access your existing account. You can say 'Hindi' or 'English' anytime to switch languages.", () => {
                                        // Start recognition after prompts complete
                                        if(state.recognition && !state.isListening) {
                                            try { state.recognition.start(); } catch(e) { console.warn('Recognition start failed:', e); }
                                        }
                                    });
                                });
                            })
                            .catch(err => {
                                speak("Microphone access denied. Please enable microphone access in your browser settings to use voice features.");
                                console.error("Microphone access error:", err);
                            });
                    } else {
                        showSection('heroSection', true);
                        initVoice();
                        speak("Blind Student Mode activated. Welcome to StudyPlanner Pro.", () => {
                            if(state.recognition && !state.isListening) {
                                try { state.recognition.start(); } catch(e) {}
                            }
                        });
                    }
                } else {
                    // Normal student mode - NO voice features
                    showSection('heroSection', true);
                    document.body.classList.remove('blind-mode-active');
                    document.querySelectorAll('.voice-only').forEach(el => el.style.display = 'none');
                    
                    // Show text quiz button for normal students
                    const textQuizBtn = document.getElementById('textQuizBtn');
                    if(textQuizBtn) textQuizBtn.style.display = '';
                    
                    state.recognition = null;
                    state.isListening = false;
                }
            }

            function resetVoiceState() {
                // Stop current speech recognition and reset all voice-related state
                try {
                    if(state.recognition && state.isListening) {
                        state.recognition.abort();
                        state.isListening = false;
                    }
                } catch(e) { console.warn('Failed to abort recognition:', e); }
                
                // Reset all voice input modes
                state.inputMode = null;
                state.quizSetup = { classSelected: null, subjectSelected: null, quizType: null };
                state.suppressAutoRestart = false;
                
                // Stop any ongoing speech synthesis
                try {
                    window.speechSynthesis.cancel();
                } catch(e) { console.warn('Failed to cancel speech:', e); }
                
                // Update listening indicator
                updateListeningIndicator();
                
                console.debug('Voice state reset');
            }

            function showSection(id, skipVoiceReset) {
                // Reset voice state when changing sections (optional)
                if(!skipVoiceReset) {
                    resetVoiceState();
                }
                
                console.log('Showing section:', id);
                
                // Hide all sections
                document.querySelectorAll('section').forEach(s => {
                    s.classList.add('hidden-section');
                    s.classList.remove('active-section');
                });
                
                // Show target section
                const targetSection = document.getElementById(id);
                if(!targetSection) {
                    console.error('Section not found:', id);
                    return;
                }
                
                targetSection.classList.remove('hidden-section');
                targetSection.classList.add('active-section');
                
                console.log('Section displayed:', id, 'Hidden:', targetSection.classList.contains('hidden-section'), 'Active:', targetSection.classList.contains('active-section'));
                
                // Auto-scroll to new section for blind students so voice commands navigate instantly
                if(state.user.role === 'blind') {
                    try {
                        targetSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
                    } catch(e) { console.warn('scrollIntoView failed', e); }
                }
                
                // Note: Voice guidance is now handled in the specific functions
                // (initSetup, selectBoard, selectDegree, selectClass, saveProfile, etc.)
                // to avoid duplicate speak() calls
            }

            // --- AUTH SYSTEM ---
            function openAuth(mode) {
                state.isOtp = false;
                state.inputMode = null;
                document.getElementById('authTitle').innerText = mode === 'login' ? 'Welcome Back' : 'Join StudyPlanner';
                document.getElementById('nameWrap').style.display = mode === 'login' ? 'none' : 'block';
                document.getElementById('authOverlay').classList.remove('hidden');
                if(state.user.role === 'blind') {
                    if(mode === 'register') {
                        speak("Registration window opened. First, enter your full name in the name field. Then provide your email or mobile number. Use Tab to move between fields.", () => {
                            speak("Say 'Name' when ready to speak your name.");
                        });
                    } else {
                        speak("Login window opened. Please enter your email or mobile number. We will send you a verification code.", () => {
                            speak("Say 'Contact' when ready to speak your email or phone number.");
                        });
                    }
                }
            }

            function closeAuth() { 
                resetVoiceState();
                document.getElementById('authOverlay').classList.add('hidden'); 
            }

            function processAuth() {
                if(!state.isOtp) {
                    const contact = document.getElementById('inContact').value;
                    if(!contact) {
                        speak("Please enter your email or mobile number first.");
                        return;
                    }
                    state.isOtp = true;
                    state.inputMode = null;
                    document.getElementById('nameWrap').classList.add('hidden');
                    document.getElementById('otpWrap').classList.remove('hidden');
                    document.getElementById('authBtn').innerText = "Verify OTP";
                    if(state.user.role === 'blind') {
                        speak("Verification code sent to your email or mobile number. A security code with six digits will arrive shortly. Please enter these digits in the OTP field.", () => {
                            speak("Say 'OTP' when ready to speak your six-digit verification code.");
                        });
                    }
                } else {
                    const otp = document.getElementById('inOtp').value;
                    if(!otp || otp.length !== 6) {
                        if(state.user.role === 'blind') {
                            speak("Please enter a valid six-digit code.");
                        }
                        return;
                    }
                    state.user.name = document.getElementById('inName').value || "Scholar";
                    state.user.contact = document.getElementById('inContact').value;
                    state.user.auth = true;
                    state.inputMode = null;
                    loginSuccess();
                }
            }

            function loginSuccess() {
                closeAuth();
                document.getElementById('authNav').classList.add('hidden');
                document.getElementById('userNav').classList.remove('hidden');
                document.getElementById('navName').innerText = state.user.name;
                document.getElementById('navRole').innerText = state.user.role + " Mode";
                
                // Provide immediate feedback for blind students
                if(state.user.role === 'blind') {
                    speak("Authentication successful. Loading your profile...", () => {
                        // Send user data to backend API after voice prompt
                        fetch(state.apiUrl + '/auth', {
                            method: 'POST',
                            headers: { 'Content-Type': 'application/json' },
                            body: JSON.stringify({
                                name: state.user.name,
                                contact: state.user.contact,
                                role: state.user.role
                            })
                        })
                        .then(res => res.json())
                        .then(data => {
                            if (data.userId) {
                                state.user.id = data.userId;
                                console.log('User authenticated with ID:', state.user.id);
                                
                                // Fetch saved profile after successful authentication
                                loadUserProfile();
                            } else {
                                // If no userId, still load profile to show portal
                                loadUserProfile();
                            }
                        })
                        .catch(err => {
                            console.error('Auth API error:', err);
                            // On error, still show portal for profile setup
                            loadUserProfile();
                        });
                    });
                } else {
                    // For regular students, proceed without voice feedback
                    fetch(state.apiUrl + '/auth', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({
                            name: state.user.name,
                            contact: state.user.contact,
                            role: state.user.role
                        })
                    })
                    .then(res => res.json())
                    .then(data => {
                        if (data.userId) {
                            state.user.id = data.userId;
                            console.log('User authenticated with ID:', state.user.id);
                            
                            // Fetch saved profile after successful authentication
                            loadUserProfile();
                        } else {
                            loadUserProfile();
                        }
                    })
                    .catch(err => {
                        console.error('Auth API error:', err);
                        loadUserProfile();
                    });
                }
            }
            
            function loadUserProfile() {
                // Try to load existing profile from backend
                if (!state.user.id) {
                    // Show portal section first, then provide voice guidance
                    showSection('portalSection', true);
                    if(state.user.role === 'blind') {
                        speak("Welcome " + state.user.name + ". Let's set up your account. Say 'School' for Classes 6 to 12, or 'College' for universities and higher education.");
                    }
                    return;
                }
                
                fetch(state.apiUrl + '/user/' + state.user.id, {
                    method: 'GET',
                    headers: { 'Content-Type': 'application/json' }
                })
                .then(res => res.json())
                .then(data => {
                    if (data && data.portal && data.level) {
                        // Profile exists - load it and go to dashboard
                        state.user.portal = data.portal;
                        state.user.level = data.level;
                        state.user.subs = data.subjects || [];
                        
                        console.log('Profile loaded:', data);
                        
                        // Update dashboard with user data
                        document.getElementById('dashUser').innerText = state.user.name;
                        document.getElementById('dashPath').innerText = `${state.user.level} • ${state.user.subs.length} Subjects`;
                        document.getElementById('aiBot').classList.remove('hidden');
                        
                        // Go directly to dashboard
                        showSection('dashboardSection', true);
                        generateSchedule();
                        
                        if(state.user.role === 'blind') {
                            const subjectSummary = state.user.subs.join(", ");
                            speak("Welcome back " + state.user.name + ". Your profile has been restored. Grade: " + state.user.level + ". Subjects: " + subjectSummary + ".", () => {
                                speak("Your dashboard is ready. Say 'Quiz' to start a practice assessment, 'Help' for more information, or 'Back' to return to the main menu.");
                            });
                        }
                    } else {
                        // No profile exists - show setup flow
                        showSection('portalSection', true);
                        if(state.user.role === 'blind') {
                            speak("Welcome " + state.user.name + ". Let's create your study profile. Say 'School' for Classes 6 to 12, or 'College' for universities and higher education.");
                        }
                    }
                })
                .catch(err => {
                    console.error('Profile API error:', err);
                    // If profile fetch fails, show setup flow as fallback
                    showSection('portalSection', true);
                    if(state.user.role === 'blind') {
                        speak("Welcome " + state.user.name + ". Let's set up your profile. Say 'School' for Classes 6 to 12, or 'College' for universities and higher education.");
                    }
                });
            }

            // --- SETUP SYSTEM ---
            function initSetup(portal) {
                state.user.portal = portal;
                
                if(portal === 'school') {
                    resetVoiceState();
                    showSection('schoolingSection', true);
                    if(state.user.role === 'blind') {
                        speak("You selected Schooling. Now choose your board. Say CBSE, ICSE, or State Board.", () => {
                            speak("Available boards: CBSE for Central Board of Secondary Education, ICSE for Indian School Board, or State Board for regional education boards.");
                        });
                    }
                } else {
                    resetVoiceState();
                    showSection('collegeSection', true);
                    if(state.user.role === 'blind') {
                        speak("You selected College or University. Now choose your degree. Say B.Tech, MCA, B.Sc, or B.Com.", () => {
                            speak("Available degrees: B.Tech for Bachelor of Technology, MCA for Master of Computer Applications, B.Sc for Bachelor of Science, or B.Com for Bachelor of Commerce.");
                        });
                    }
                }
            }

            function handleSchoolingClick() {
                console.log('Schooling clicked');
                state.user.portal = 'school';
                resetVoiceState();
                showSection('schoolingSection', true);
                if(state.user.role === 'blind') {
                    speak("You selected Schooling. Choose your board. Say CBSE, ICSE, or State Board.", () => {
                        speak("CBSE is Central Board of Secondary Education. ICSE is Indian Certificate of Secondary Education. State Board varies by region.");
                    });
                }
            }

            function handleCollegeClick() {
                console.log('College clicked');
                state.user.portal = 'college';
                resetVoiceState();
                showSection('collegeSection', true);
                if(state.user.role === 'blind') {
                    speak("You selected College or University. Choose your degree. Say B.Tech, MCA, B.Sc, or B.Com.", () => {
                        speak("B.Tech is Bachelor of Technology. MCA is Master of Computer Applications. B.Sc is Bachelor of Science. B.Com is Bachelor of Commerce.");
                    });
                }
            }

            function selectBoard(board) {
                state.user.board = board;
                populateClassGrid('school');
                resetVoiceState();
                showSection('classSection', true);
                
                if(state.user.role === 'blind') {
                    const boardName = board.charAt(0).toUpperCase() + board.slice(1);
                    speak("You selected " + boardName + " board. Now select your class using Tab to navigate and Enter to select.", () => {
                        const levels = ACADEMIC_DB.school.levels.filter(l => l.startsWith('Class'));
                        speak("Available classes: " + levels.join(", ") + ".");
                    });
                }
            }

            function selectDegree(degree) {
                state.user.degree = degree;
                populateClassGrid('college');
                resetVoiceState();
                showSection('classSection', true);
                
                if(state.user.role === 'blind') {
                    const degreeName = degree.toUpperCase();
                    speak("You selected " + degreeName + ". Now select your year or semester using Tab to navigate and Enter to select.", () => {
                        const levels = ACADEMIC_DB.college.levels;
                        speak("Available years: " + levels.join(", ") + ".");
                    });
                }
            }

            function populateClassGrid(type) {
                const classGrid = document.getElementById('classGrid');
                const classSectionTitle = document.getElementById('classSectionTitle');
                let classes = [];
                
                if(type === 'school') {
                    classSectionTitle.innerText = 'Select Your Class';
                    // Use the actual levels from ACADEMIC_DB
                    classes = ACADEMIC_DB.school.levels.filter(l => l.startsWith('Class'));
                } else {
                    classSectionTitle.innerText = 'Select Your Year / Semester';
                    // Use the actual levels from ACADEMIC_DB
                    classes = ACADEMIC_DB.college.levels;
                }
                
                classGrid.innerHTML = classes.map(cls => `
                    <button onclick="selectClass('${cls}')" class="glass-card p-6 rounded-2xl text-center hover:border-indigo-600 hover:shadow-lg transition font-bold text-lg transform hover:scale-105">
                        ${cls}
                    </button>
                `).join('');
            }

            function selectClass(classValue) {
                state.user.level = classValue;
                
                if(state.user.role === 'blind') {
                    speak("You selected " + classValue + ". Loading your subject configuration now.");
                }
                
                // Populate and show config section
                const drop = document.getElementById('levelDrop');
                const levels = ACADEMIC_DB[state.user.portal].levels;
                drop.innerHTML = levels.map(l => `<option value="${l}">${l}</option>`).join('');
                
                // Set the dropdown value to the selected class (if it exists in the dropdown)
                if(levels.includes(classValue)) {
                    drop.value = classValue;
                } else {
                    // If exact match not found, set to first option
                    drop.value = levels[0];
                }
                
                // Reset voice state, then show section
                resetVoiceState();
                showSection('configSection', true);
                renderSubjects();
                
                if(state.user.role === 'blind') {
                    setTimeout(() => {
                        speak("Subject configuration opened. Select the subjects you want to study. Use Tab and Space to select or deselect subjects. When done, say OK or press Tab to reach the Generate Dashboard button.");
                    }, 500);
                }
            }

            function goBackToInstitution() {
                if(state.user.portal === 'school') {
                    resetVoiceState();
                    showSection('schoolingSection', true);
                    if(state.user.role === 'blind') {
                        speak("Returned to Schooling section. Choose your board: CBSE, ICSE, or State Board.");
                    }
                } else {
                    resetVoiceState();
                    showSection('collegeSection', true);
                    if(state.user.role === 'blind') {
                        speak("Returned to College section. Choose your degree: B.Tech, MCA, B.Sc, or B.Com.");
                    }
                }
            }

            function proceedToSubjectSelection(selectedClass) {
                state.user.level = selectedClass;
                const drop = document.getElementById('levelDrop');
                const levels = ACADEMIC_DB[state.user.portal].levels;
                drop.innerHTML = levels.map(l => `<option value="${l}">${l}</option>`).join('');
                drop.value = selectedClass;
                
                resetVoiceState();
                showSection('configSection', true);
                renderSubjects();
                
                if(state.user.role === 'blind') {
                    speak("Proceeding to select your subjects for " + selectedClass + ". All subjects are pre-selected. Use Tab and Space to deselect any subject you don't need.", () => {
                        const currentSubjects = state.user.subs.length > 0 ? state.user.subs.join(", ") : "All available subjects";
                        speak("Current subjects: " + currentSubjects + ". When ready, say OK or use Tab to reach the Generate Dashboard button.");
                    });
                }
            }

            function renderSubjects() {
                const level = document.getElementById('levelDrop').value;
                let subs = [];
                if(state.user.portal === 'school') {
                    subs = level.includes("Science") ? ACADEMIC_DB.school.subjects.Science : 
                        level.includes("Commerce") ? ACADEMIC_DB.school.subjects.Commerce : ACADEMIC_DB.school.subjects.General;
                } else {
                    subs = level.includes("Tech") || level.includes("MCA") ? ACADEMIC_DB.college.subjects.Tech : ACADEMIC_DB.college.subjects.General;
                }
                
                document.getElementById('subjectBox').innerHTML = subs.map(s => `
                    <div class="flex items-center gap-3 p-4 bg-white border-2 border-slate-50 rounded-xl">
                        <input type="checkbox" name="sub" value="${s}" checked class="w-5 h-5 accent-indigo-600">
                        <span class="font-bold text-sm text-slate-700">${s}</span>
                    </div>
                `).join('');
                
                if(state.user.role === 'blind') {
                    const subjectList = subs.join(", ");
                    speak("Available subjects for " + level + ": " + subjectList + ". All subjects are selected by default. Use Tab and Space to deselect any subject you don't need.");
                }
            }

            function saveProfile() {
                state.user.level = document.getElementById('levelDrop').value;
                state.user.subs = Array.from(document.querySelectorAll('input[name="sub"]:checked')).map(i => i.value);
                
                document.getElementById('dashUser').innerText = state.user.name;
                document.getElementById('dashPath').innerText = `${state.user.level} • ${state.user.subs.length} Subjects`;
                document.getElementById('aiBot').classList.remove('hidden');
                
                // Save profile to backend API - will be persisted permanently
                if (state.user.id) {
                    fetch(state.apiUrl + '/user/' + state.user.id + '/profile', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({
                            level: state.user.level,
                            portal: state.user.portal,
                            subjects: state.user.subs
                        })
                    })
                    .then(res => res.json())
                    .then(data => {
                        console.log('Profile saved permanently to database:', data);
                        // Show success message
                        if(state.user.role === 'blind') {
                            speak("Profile saved successfully.");
                        }
                    })
                    .catch(err => {
                        console.error('Profile API error:', err);
                        if(state.user.role === 'blind') {
                            speak("Profile could not be saved. Please try again.");
                        }
                    });
                } else {
                    console.warn('User ID not set, profile not saved');
                }
                
                showSection('dashboardSection', true);
                generateSchedule();
                if(state.user.role === 'blind') {
                    const subjectSummary = state.user.subs.join(", ");
                    speak("Setup complete! Your personalized dashboard is ready. Grade: " + state.user.level + ". Subjects: " + subjectSummary + ".", () => {
                        speak("Your information has been saved. Next time you log in, your profile will be restored automatically. Say 'Quiz' to start a practice assessment, or 'Help' for more information.");
                    });
                }
            }

            // --- QUIZ SETUP FLOW ---
            function startQuizSetup() {
                if(state.user.role !== 'blind') {
                    startVoiceQuiz();
                    return;
                }
                // For blind students, go through the setup flow
                state.quizSetup = { classSelected: null, subjectSelected: null, quizType: null };
                askForClass();
            }

            function askForClass() {
                const levels = ACADEMIC_DB[state.user.portal].levels;
                const levelList = levels.join(", ");
                speak("Step 1: Select your class or year. Available options are: " + levelList + ". Which class do you want to practice?", () => {
                    state.inputMode = 'quizClass';
                });
            }

            function askForSubject(selectedClass) {
                state.quizSetup.classSelected = selectedClass;
                let subs = [];
                if(state.user.portal === 'school') {
                    subs = selectedClass.includes("Science") ? ACADEMIC_DB.school.subjects.Science : 
                        selectedClass.includes("Commerce") ? ACADEMIC_DB.school.subjects.Commerce : ACADEMIC_DB.school.subjects.General;
                } else {
                    subs = selectedClass.includes("Tech") || selectedClass.includes("MCA") ? ACADEMIC_DB.college.subjects.Tech : ACADEMIC_DB.college.subjects.General;
                }
                const subjectList = subs.join(", ");
                speak("Step 2: Select your subject. Available subjects for " + selectedClass + " are: " + subjectList + ". Which subject would you like to practice?", () => {
                    state.inputMode = 'quizSubject';
                });
            }

            function askForQuizType(selectedSubject) {
                state.quizSetup.subjectSelected = selectedSubject;
                speak("Step 3: How would you like your assessment? Say 'Topic wise' for questions based on specific topics, or 'Unit wise' for broad unit-based questions.", () => {
                    state.inputMode = 'quizType';
                });
            }

            function startQuiz(selectedSubject, quizType) {
                state.quizSetup.quizType = quizType;
                speak("Starting " + quizType + " assessment for " + state.quizSetup.classSelected + " " + selectedSubject + ". Loading questions...", () => {
                    beginVoiceQuiz();
                });
            }

            function beginVoiceQuiz() {
                document.getElementById('quizOverlay').classList.remove('hidden');
                const subject = (state.quizSetup.subjectSelected || state.user.subs[0] || '').toString();
                // Try to pick a question matching the subject (case-insensitive). If multiple available, pick a random one.
                const pool = QUIZ_BANK.filter(item => item.sub && item.sub.toLowerCase() === subject.toLowerCase());
                const q = (pool.length ? pool[Math.floor(Math.random() * pool.length)] : (QUIZ_BANK.find(item => item.level === state.quizSetup.classSelected && item.sub === subject) || QUIZ_BANK[0]));
                
                document.getElementById('quizCat').innerText = q.sub;
                document.getElementById('quizQ').innerText = q.q;
                document.getElementById('quizOptions').innerHTML = q.o.map((opt, i) => `
                    <button onclick="checkAns(${i}, ${q.a})" class="p-8 bg-white/5 border-2 border-white/10 rounded-[30px] text-2xl font-bold hover:bg-white hover:text-slate-900 transition">
                        ${opt}
                    </button>
                `).join('');
                
                speak("Practice Assessment started for " + q.sub + ".", () => {
                    speak(`Question: ${q.q}.`, () => {
                        speak(`Your options are: ${q.o.map((opt, i) => `Option ${i + 1}: ${opt}`).join(". ")}. Use arrow keys or Tab to select an option and press Enter to submit your answer.`);
                    });
                });
            }

            function generateSchedule() {
                const container = document.getElementById('timetableGrid');
                const slots = ["08:00 AM", "11:00 AM", "03:00 PM", "07:00 PM"];
                const colors = ["indigo", "orange", "emerald", "rose"];
                
                container.innerHTML = slots.map((time, i) => {
                    const sub = state.user.subs[i % state.user.subs.length];
                    return `
                        <div class="flex items-center gap-6 p-6 bg-slate-50 rounded-3xl border-l-8 border-${colors[i]}-500">
                            <span class="font-black text-slate-400 w-24">${time}</span>
                            <div>
                                <h4 class="font-black text-xl text-slate-800">${sub}</h4>
                                <p class="text-sm font-bold text-slate-400 uppercase">Self Study & MCQ Practice</p>
                            </div>
                        </div>
                    `;
                }).join('');
                document.getElementById('focusSub').innerText = state.user.subs[0];
                
                // Show appropriate quiz button based on role
                const voiceQuizBtn = document.getElementById('voiceQuizBtn');
                const textQuizBtn = document.getElementById('textQuizBtn');
                if(state.user.role === 'blind') {
                    if(voiceQuizBtn) voiceQuizBtn.style.display = '';
                    if(textQuizBtn) textQuizBtn.style.display = 'none';
                } else {
                    if(voiceQuizBtn) voiceQuizBtn.style.display = 'none';
                    if(textQuizBtn) textQuizBtn.style.display = '';
                }
            }

            // --- VOICE & QUIZ (BLIND MODE ONLY) ---
            function initVoice() {
                // Voice features ONLY for blind students
                if(state.user.role !== 'blind') {
                    return;
                }
                
                const Speech = window.SpeechRecognition || window.webkitSpeechRecognition;
                if(!Speech) {
                    speak("Speech recognition is not supported in your browser. Please use Chrome, Edge, or Firefox.");
                    return;
                }
                
                state.recognition = new Speech();
                state.recognition.continuous = true;
                state.recognition.interimResults = true;
                state.recognition.lang = state.language;
                
                state.recognition.onstart = () => {
                    state.isListening = true;
                    updateListeningIndicator();
                    console.debug('Recognition started');
                    const dbg = document.getElementById('voiceDebug'); if(dbg) dbg.innerText = 'Recognition: started';
                };
                
                state.recognition.onend = () => {
                    state.isListening = false;
                    updateListeningIndicator();
                    console.debug('Recognition ended');
                    const dbg = document.getElementById('voiceDebug'); if(dbg) dbg.innerText = 'Recognition: ended';
                    // Auto-restart recognition unless suppressed (e.g., during TTS prompt)
                    if(state.user.role === 'blind' && !state.suppressAutoRestart) {
                        setTimeout(() => {
                            try {
                                state.recognition.start();
                            } catch(e) { console.warn('Auto-restart failed', e); }
                        }, 800);
                    }
                };
                
                state.recognition.onerror = (event) => {
                    state.isListening = false;
                    updateListeningIndicator();
                    if(event.error === 'network' || event.error === 'service-not-available') {
                        speak("Network error. Attempting to reconnect...");
                    }
                    console.warn('Recognition error', event.error);
                    const dbg = document.getElementById('voiceDebug'); if(dbg) dbg.innerText = 'Error: ' + event.error;
                    // Auto-restart on error
                    if(state.user.role === 'blind') {
                        setTimeout(() => {
                            try {
                                state.recognition.start();
                            } catch(e) {}
                        }, 2000);
                    }
                };
                
                state.recognition.onresult = (event) => {
                    console.debug('Speech onresult', event);
                    let interimTranscript = '';
                    for (let i = event.resultIndex; i < event.results.length; i++) {
                        const transcript = (event.results[i][0].transcript || '').toLowerCase();
                        if (event.results[i].isFinal) {
                            console.debug('Speech final:', transcript);
                            // If we're in an explicit input mode, populate the input immediately
                            try {
                                if(state.inputMode === 'name' || state.inputMode === 'nameConfirm') {
                                        const el = document.getElementById('inName');
                                        el.focus();
                                        const val = transcript.split(/\s+/).map(w => w.charAt(0).toUpperCase() + w.slice(1)).join(' ').trim();
                                        el.value = val;
                                        try { el.setSelectionRange(val.length, val.length); } catch(e) {}
                                        el.dispatchEvent(new Event('input', { bubbles: true, composed: true }));
                                        el.dispatchEvent(new Event('change', { bubbles: true }));
                                    } else if(state.inputMode === 'contact' || state.inputMode === 'contactConfirm') {
                                        const el = document.getElementById('inContact');
                                        el.focus();
                                        const val = transcript.replace(/\s+at\s+|\s+@\s+/g,'@').replace(/\s+dot\s+|\s+period\s+/g,'.').replace(/\s+/g,'');
                                        el.value = val;
                                        try { el.setSelectionRange(val.length, val.length); } catch(e) {}
                                        el.dispatchEvent(new Event('input', { bubbles: true, composed: true }));
                                        el.dispatchEvent(new Event('change', { bubbles: true }));
                                    } else if(state.inputMode === 'otp') {
                                        const el = document.getElementById('inOtp');
                                        const digits = transcript.replace(/[^0-9]/g,'');
                                        el.focus();
                                        el.value = digits;
                                        try { el.setSelectionRange(el.value.length, el.value.length); } catch(e) {}
                                        el.dispatchEvent(new Event('input', { bubbles: true, composed: true }));
                                        el.dispatchEvent(new Event('change', { bubbles: true }));
                                    }
                            } catch(e) { console.warn('Final write failed', e); }

                            processVoiceCommand(transcript);
                            // show final in debug panel if present
                            const dbg = document.getElementById('voiceDebug'); if(dbg) dbg.innerText = 'Final: ' + transcript;
                        } else {
                            interimTranscript += transcript + ' ';
                        }
                    }

                    // If user is actively filling an input, show interim text for feedback
                    if(interimTranscript.trim()) {
                        try {
                            if(state.inputMode === 'name') {
                                document.getElementById('inName').value = interimTranscript.trim();
                            } else if(state.inputMode === 'contact') {
                                document.getElementById('inContact').value = interimTranscript.trim();
                            } else if(state.inputMode === 'otp') {
                                document.getElementById('inOtp').value = interimTranscript.replace(/[^0-9]/g,'');
                            }
                            const dbg = document.getElementById('voiceDebug'); if(dbg) dbg.innerText = 'Interim: ' + interimTranscript.trim();
                        } catch (e) { console.warn('Interim write failed', e); }
                    }
                };
                
                try {
                    state.recognition.start();
                } catch(e) {
                    speak("Could not start voice recognition. Please refresh the page.");
                }
            }
            
            function updateListeningIndicator() {
                const indicator = document.getElementById('listeningIndicator');
                if(state.isListening && state.user.role === 'blind') {
                    indicator.classList.remove('hidden');
                } else {
                    indicator.classList.add('hidden');
                }
            }
            
            function processVoiceCommand(transcript) {
                // Voice commands ONLY for blind students
                if(state.user.role !== 'blind') {
                    return;
                }
                transcript = (transcript || '').trim().toLowerCase();

                // helpers
                function wordsToDigits(s) {
                    const map = { zero: '0', one: '1', two: '2', three: '3', four: '4', five: '5', six: '6', seven: '7', eight: '8', nine: '9', ten: '10', eleven: '11', twelve: '12', oh: '0', o: '0' };
                    return s.replace(/\b(zero|one|two|three|four|five|six|seven|eight|nine|ten|eleven|twelve|oh|o)\b/g, m => map[m]);
                }
                function normalizeContact(s) {
                    let t = s;
                    t = t.replace(/\s+at\s+|\s+@\s+/g, '@');
                    t = t.replace(/\s+dot\s+|\s+period\s+/g, '.');
                    t = t.replace(/\s+underscore\s+/g, '_');
                    t = t.replace(/\s+(dash|hyphen)\s+/g, '-');
                    t = wordsToDigits(t);
                    t = t.replace(/\s+/g, '');
                    t = t.replace(/^at/, '@');
                    return t;
                }
                function toTitleCase(s) {
                    return s.split(/\s+/).map(w => w.charAt(0).toUpperCase() + w.slice(1)).join(' ').trim();
                }
                function normalizeForMatch(s) {
                    if(!s) return '';
                    s = s.toLowerCase();
                    s = wordsToDigits(s);
                    s = s.replace(/[.,/#!$%^&*;:{}=\-_`~()]/g, ' ');
                    s = s.replace(/\s+/g, ' ').trim();
                    return s;
                }

                // IMPORTANT: Check for "back" commands FIRST before processing any input mode
                // This allows users to exit at any point
                if(transcript.includes("back") || transcript.includes("go back") || transcript.includes("previous") || transcript.includes("exit")) {
                    
                    if(document.getElementById('authOverlay').classList.contains('hidden') === false) {
                        speak("Closing login window. Returning to main menu.");
                        closeAuth();
                        return;
                    }
                    
                    // 2. If in profile setup (setupClass mode)
                    if(state.inputMode === 'setupClass') {
                        speak("Exiting profile setup. Returning to institution selection.");
                        resetVoiceState();
                        showSection('portalSection');
                        return;
                    }
                    
                    // 3. If in quiz setup - multi-step back
                    if(state.inputMode === 'quizType') {
                        // Go back from quiz type to subject selection
                        state.quizSetup.quizType = null;
                        state.inputMode = 'quizSubject';
                        speak("Returning to subject selection. Available subjects for " + state.quizSetup.classSelected + " are: Data Structures, Algorithms, or other subjects.");
                        askForSubject(state.quizSetup.classSelected);
                        return;
                    } else if(state.inputMode === 'quizSubject' || state.quizSetup.subjectSelected) {
                        // Go back from subject to class selection
                        state.quizSetup.subjectSelected = null;
                        state.inputMode = 'quizClass';
                        speak("Returning to class selection. Which class would you like to practice?");
                        askForClass();
                        return;
                    } else if(state.inputMode === 'quizClass' || state.quizSetup.classSelected) {
                        // Exit entire quiz flow back to dashboard
                        resetVoiceState();
                        speak("Exiting assessment setup. Returning to your dashboard.");
                        showSection('dashboardSection');
                        return;
                    }
                    
                    // 4. If starting new quiz setup (after step 1)
                    if(state.quizSetup.classSelected && !state.inputMode) {
                        resetVoiceState();
                        speak("Exiting assessment. Returning to your dashboard.");
                        showSection('dashboardSection', true);
                        return;
                    }
                    
                    // 5. During quiz itself
                    if(document.getElementById('quizOverlay').classList.contains('hidden') === false) {
                        speak("Exiting practice assessment. Returning to your dashboard.");
                        endQuiz();
                        return;
                    }
                    
                    // 6. Section-level back navigation
                    const currentActiveSection = Array.from(document.querySelectorAll('section')).find(s => !s.classList.contains('hidden-section'));
                    const currentSectionId = currentActiveSection ? currentActiveSection.id : '';
                    
                    if(currentSectionId === 'configSection') {
                        // From config (class/subjects) back to portal selection
                        resetVoiceState();
                        speak("Returning to institution selection. Say School or College.");
                        showSection('portalSection', true);
                        return;
                    } else if(currentSectionId === 'portalSection') {
                        // From portal selection back to dashboard (if authenticated)
                        if(state.user.auth) {
                            resetVoiceState();
                            speak("Returning to your dashboard.");
                            showSection('dashboardSection', true);
                            return;
                        }
                    }
                    
                    // 7. From dashboard, stay on dashboard (it's home)
                    if(currentSectionId === 'dashboardSection') {
                        speak("You are on the home dashboard. Say Quiz to start an assessment, or ask for Help.");
                        return;
                    }
                    
                    return;
                }

                // Activation shortcuts when not in an input-mode
                if(!state.inputMode) {
                    if(transcript.includes('my name is') || transcript.startsWith('name ') || transcript === 'name') {
                        let name = transcript.replace(/.*my name is\s*/,'').replace(/^name\s*/,'').trim();
                        name = toTitleCase(name);
                        document.getElementById('inName').value = name;
                        state.inputMode = 'nameConfirm';
                        speak("You said: " + name + ". Is this correct? Say 'Yes' to confirm or 'No' to retry.");
                        return;
                    }
                    if(transcript.includes('my email') || transcript.includes('my contact') || transcript.includes('email') || transcript.includes('phone') || transcript.includes('mobile') || transcript === 'contact') {
                        let contact = transcript.replace(/.*(my email is|my contact is|my phone is|my mobile is)\s*/,'').trim();
                        contact = normalizeContact(contact);
                        document.getElementById('inContact').value = contact;
                        state.inputMode = 'contactConfirm';
                        speak("You said: " + contact + ". Is this correct? Say 'Yes' to confirm or 'No' to retry.");
                        return;
                    }
                    if(transcript === 'name') { activateVoiceInput('name'); return; }
                    if(transcript === 'contact') { activateVoiceInput('contact'); return; }
                }

                // Input mode specific handling
                if(state.inputMode === 'name' || state.inputMode === 'nameConfirm') {
                    let nameText = transcript.replace(/^(my name is\s*|name is\s*|i am\s*)/,'').trim();
                    nameText = toTitleCase(nameText);
                    document.getElementById('inName').value = nameText;
                    state.inputMode = 'nameConfirm';
                    speak("You said: " + nameText + ". Is this correct? Say 'Yes' to confirm or 'No' to retry.");
                    return;
                } else if(state.inputMode === 'contact' || state.inputMode === 'contactConfirm') {
                    let contactText = transcript.replace(/^(my email is\s*|my contact is\s*|my phone is\s*|my mobile is\s*)/,'').trim();
                    contactText = normalizeContact(contactText);
                    document.getElementById('inContact').value = contactText;
                    state.inputMode = 'contactConfirm';
                    speak("You said: " + contactText + ". Is this correct? Say 'Yes' to confirm or 'No' to retry.");
                    return;
                } else if(state.inputMode === 'otp') {
                    const digitTranscript = (transcript.replace(/[^0-9]/g, '') || wordsToDigits(transcript).replace(/[^0-9]/g, ''));
                    if(digitTranscript.length === 6) {
                        document.getElementById('inOtp').value = digitTranscript;
                        speak("OTP entered: " + digitTranscript.split('').join(' ') + ". Say 'Submit' to verify or 'Clear' to try again.");
                    } else {
                        speak("Please say all six digits. You said " + digitTranscript.length + " digits.");
                    }
                    return;
                } else if(state.inputMode === 'quizClass') {
                    const levels = ACADEMIC_DB[state.user.portal].levels;
                    const norm = normalizeForMatch(transcript);
                    let matchedLevel = null;
                    for(const l of levels) {
                        const nl = normalizeForMatch(l);
                        if(nl && (norm.includes(nl) || nl.includes(norm) || norm === nl)) { matchedLevel = l; break; }
                        const digits = nl.match(/\d+/);
                        if(digits && norm.includes(digits[0])) { matchedLevel = l; break; }
                    }
                    if(matchedLevel) {
                        speak("You selected " + matchedLevel + ". Proceeding to select subject.");
                        askForSubject(matchedLevel);
                    } else {
                        speak("Sorry, I didn't recognize that class. Please try again from the list: " + levels.join(", "));
                    }
                    return;
                } else if(state.inputMode === 'quizSubject') {
                    let subs = [];
                    const selectedClass = state.quizSetup.classSelected || state.user.level || document.getElementById('levelDrop').value;
                    if(state.user.portal === 'school') {
                        subs = selectedClass && selectedClass.includes("Science") ? ACADEMIC_DB.school.subjects.Science : 
                            selectedClass && selectedClass.includes("Commerce") ? ACADEMIC_DB.school.subjects.Commerce : ACADEMIC_DB.school.subjects.General;
                    } else {
                        subs = selectedClass && (selectedClass.includes("Tech") || selectedClass.includes("MCA")) ? ACADEMIC_DB.college.subjects.Tech : ACADEMIC_DB.college.subjects.General;
                    }
                    const norm = normalizeForMatch(transcript);
                    let matchedSubject = null;
                    for(const s of subs) {
                        const ns = normalizeForMatch(s);
                        if(ns && (norm.includes(ns) || ns.includes(norm) || norm === ns)) { matchedSubject = s; break; }
                    }
                    if(matchedSubject) {
                        speak("You selected " + matchedSubject + ". Now choosing quiz format.");
                        askForQuizType(matchedSubject);
                    } else {
                        speak("Sorry, I didn't recognize that subject. Please try again from: " + subs.join(", "));
                    }
                    return;
                } else if(state.inputMode === 'quizType') {
                    if(transcript.includes('topic')) {
                        speak("You selected Topic wise assessment. Starting quiz...");
                        startQuiz(state.quizSetup.subjectSelected, 'Topic wise');
                        state.inputMode = null;
                    } else if(transcript.includes('unit')) {
                        speak("You selected Unit wise assessment. Starting quiz...");
                        startQuiz(state.quizSetup.subjectSelected, 'Unit wise');
                        state.inputMode = null;
                    } else {
                        speak("Please say either 'Topic wise' or 'Unit wise'.");
                    }
                    return;
                } else if(state.inputMode === 'setupClass') {
                    // Handle class selection during profile setup
                    const levels = ACADEMIC_DB[state.user.portal].levels;
                    const norm = normalizeForMatch(transcript);
                    let matchedLevel = null;
                    for(const l of levels) {
                        const nl = normalizeForMatch(l);
                        if(nl && (norm.includes(nl) || nl.includes(norm) || norm === nl)) { matchedLevel = l; break; }
                        const digits = nl.match(/\d+/);
                        if(digits && norm.includes(digits[0])) { matchedLevel = l; break; }
                    }
                    if(matchedLevel) {
                        speak("You selected " + matchedLevel + ". Now, please choose your primary subjects.");
                        state.inputMode = null;
                        proceedToSubjectSelection(matchedLevel);
                    } else {
                        speak("Sorry, I didn't recognize that class. Please try again from the list: " + levels.join(", "));
                    }
                    return;
                }

                // General commands
                if(transcript.includes("register") || transcript.includes("registration")) { openAuth('register'); return; }
                if(transcript.includes("login") || transcript.includes("sign in")) { openAuth('login'); return; }
                if(transcript.includes("school")) { initSetup('school'); return; }
                if(transcript.includes("college") || transcript.includes("university")) { initSetup('college'); return; }
                
                // Check for class/level voice commands (direct shortcuts)
                // School levels
                if(transcript.includes("class 6")) { 
                    if(state.inputMode === 'setupClass') {
                        proceedToSubjectSelection("Class 6");
                        state.inputMode = null;
                    } else {
                        state.quizSetup = { classSelected: "Class 6", subjectSelected: null, quizType: null };
                        askForSubject("Class 6");
                    }
                    return; 
                }
                if(transcript.includes("class 7")) { 
                    if(state.inputMode === 'setupClass') {
                        proceedToSubjectSelection("Class 7");
                        state.inputMode = null;
                    } else {
                        state.quizSetup = { classSelected: "Class 7", subjectSelected: null, quizType: null };
                        askForSubject("Class 7");
                    }
                    return; 
                }
                if(transcript.includes("class 8")) { 
                    if(state.inputMode === 'setupClass') {
                        proceedToSubjectSelection("Class 8");
                        state.inputMode = null;
                    } else {
                        state.quizSetup = { classSelected: "Class 8", subjectSelected: null, quizType: null };
                        askForSubject("Class 8");
                    }
                    return; 
                }
                if(transcript.includes("class 9")) { 
                    if(state.inputMode === 'setupClass') {
                        proceedToSubjectSelection("Class 9");
                        state.inputMode = null;
                    } else {
                        state.quizSetup = { classSelected: "Class 9", subjectSelected: null, quizType: null };
                        askForSubject("Class 9");
                    }
                    return; 
                }
                if(transcript.includes("class 10")) { 
                    if(state.inputMode === 'setupClass') {
                        proceedToSubjectSelection("Class 10");
                        state.inputMode = null;
                    } else {
                        state.quizSetup = { classSelected: "Class 10", subjectSelected: null, quizType: null };
                        askForSubject("Class 10");
                    }
                    return; 
                }
                if(transcript.includes("class 11")) {
                    if(transcript.includes("science")) {
                        if(state.inputMode === 'setupClass') {
                            proceedToSubjectSelection("Class 11 Science");
                            state.inputMode = null;
                        } else {
                            state.quizSetup = { classSelected: "Class 11 Science", subjectSelected: null, quizType: null };
                            askForSubject("Class 11 Science");
                        }
                    } else if(transcript.includes("commerce")) {
                        if(state.inputMode === 'setupClass') {
                            proceedToSubjectSelection("Class 11 Commerce");
                            state.inputMode = null;
                        } else {
                            state.quizSetup = { classSelected: "Class 11 Commerce", subjectSelected: null, quizType: null };
                            askForSubject("Class 11 Commerce");
                        }
                    } else {
                        speak("Did you mean Class 11 Science or Class 11 Commerce? Please specify.");
                    }
                    return; 
                }
                if(transcript.includes("class 12")) {
                    if(transcript.includes("science")) {
                        if(state.inputMode === 'setupClass') {
                            proceedToSubjectSelection("Class 12 Science");
                            state.inputMode = null;
                        } else {
                            state.quizSetup = { classSelected: "Class 12 Science", subjectSelected: null, quizType: null };
                            askForSubject("Class 12 Science");
                        }
                    } else if(transcript.includes("commerce")) {
                        if(state.inputMode === 'setupClass') {
                            proceedToSubjectSelection("Class 12 Commerce");
                            state.inputMode = null;
                        } else {
                            state.quizSetup = { classSelected: "Class 12 Commerce", subjectSelected: null, quizType: null };
                            askForSubject("Class 12 Commerce");
                        }
                    } else {
                        speak("Did you mean Class 12 Science or Class 12 Commerce? Please specify.");
                    }
                    return; 
                }
                
                // College levels
                if(transcript.includes("cbse")) { selectBoard('cbse'); return; }
                if(transcript.includes("icse")) { selectBoard('icse'); return; }
                if(transcript.includes("state") && transcript.includes("board")) { selectBoard('state'); return; }
                
                if(transcript.includes("btech") || transcript.includes("b.tech") || transcript.includes("b tech")) { selectDegree('btech'); return; }
                if(transcript.includes("mca")) { selectDegree('mca'); return; }
                if(transcript.includes("bsc") || transcript.includes("b.sc") || transcript.includes("b sc")) { selectDegree('bsc'); return; }
                if(transcript.includes("bcom") || transcript.includes("b.com") || transcript.includes("b com")) { selectDegree('bcom'); return; }

                if(transcript.includes("yes")) {
                    if(state.inputMode === 'nameConfirm' || state.inputMode === 'name') {
                        state.user.name = document.getElementById('inName').value || state.user.name;
                        state.inputMode = null;
                        speak("Name confirmed.");
                        return;
                    }
                    if(state.inputMode === 'contactConfirm' || state.inputMode === 'contact') {
                        state.user.contact = document.getElementById('inContact').value || state.user.contact;
                        state.inputMode = null;
                        speak("Contact confirmed.");
                        return;
                    }
                    if(state.inputMode === 'otp') {
                        processAuth();
                        return;
                    }
                }

                if(transcript.includes("no")) {
                    if(state.inputMode === 'nameConfirm' || state.inputMode === 'name') {
                        document.getElementById('inName').value = '';
                        state.inputMode = 'name';
                        speak("Okay. Please say your full name again.");
                        return;
                    }
                    if(state.inputMode === 'contactConfirm' || state.inputMode === 'contact') {
                        document.getElementById('inContact').value = '';
                        state.inputMode = 'contact';
                        speak("Okay. Please say your contact again.");
                        return;
                    }
                    speak("Let me try that again.");
                    return;
                }

                if(transcript.includes("clear") || transcript.includes("reset")) {
                    document.getElementById('inOtp').value = '';
                    speak("Cleared. Please say the six-digit code.");
                    return;
                }
                if(transcript.includes("submit") || transcript.includes("verify")) {
                    if(state.inputMode === 'otp') {
                        processAuth();
                        return;
                    }
                }
                
                // Language switching commands
                if(transcript.includes("hindi")) {
                    state.language = 'hi-IN';
                    try { 
                        if(state.recognition && state.isListening) {
                            state.recognition.abort();
                            setTimeout(() => {
                                try {
                                    state.recognition.lang = 'hi-IN';
                                    state.recognition.start();
                                } catch(e) {}
                            }, 500);
                        } else if(state.recognition) {
                            state.recognition.lang = 'hi-IN';
                        }
                    } catch(e) {}
                    speak("भाषा हिंदी में परिवर्तित की गई। अब आप हिंदी में बोल सकते हैं।"); // Language switched to Hindi. You can now speak in Hindi.
                    return;
                }
                if(transcript.includes("english")) {
                    state.language = 'en-US';
                    try { 
                        if(state.recognition && state.isListening) {
                            state.recognition.abort();
                            setTimeout(() => {
                                try {
                                    state.recognition.lang = 'en-US';
                                    state.recognition.start();
                                } catch(e) {}
                            }, 500);
                        } else if(state.recognition) {
                            state.recognition.lang = 'en-US';
                        }
                    } catch(e) {}
                    speak("Language switched to English. You can now speak in English.");
                    return;
                }
                
                if(transcript.includes("test") || transcript.includes("quiz") || transcript.includes("assessment")) { startVoiceQuiz(); return; }
                if(transcript.includes("help")) { showHelp(); return; }
                if(transcript.includes("repeat")) { speak("I can help you navigate the application using voice commands."); return; }
            }
            
            function activateVoiceInput(mode) {
                // Voice input ONLY for blind students
                if(state.user.role !== 'blind') {
                    return;
                }
                // Ensure recognition is initialized; we'll speak the prompt and start recognition AFTER speech ends
                state.inputMode = mode;
                if(!state.recognition) {
                    try { initVoice(); } catch(e) { console.warn('initVoice error', e); }
                }

                // If recognition is active, stop it before speaking to avoid capturing TTS
                try {
                    if(state.recognition && state.isListening) {
                        state.recognition.abort();
                    }
                } catch(e) { /* ignore */ }

                updateListeningIndicator();

                let promptText = '';
                if(mode === 'name') promptText = "Please say your full name clearly.";
                else if(mode === 'contact') promptText = "Please say your email address or mobile number.";
                else if(mode === 'otp') promptText = "Please say each digit of your six-digit verification code clearly. For example, say: One, Two, Three, Four, Five, Six.";

                // Speak the prompt, then start recognition so we capture the user's reply
                speak(promptText, () => {
                    try {
                        if(state.recognition) {
                            try { state.recognition.lang = state.language; } catch(e) {}
                            if(!state.isListening) state.recognition.start();
                        }
                    } catch(e) { console.warn('start after speak failed', e); }
                });

                // Fallback: if speechSynthesis doesn't call back, attempt to start after 800ms
                setTimeout(() => {
                    try { if(state.recognition && !state.isListening) state.recognition.start(); } catch(e) {}
                }, 800);
            }
            
            function showHelp() {
                if(state.language === 'hi-IN') {
                    speak("आप इन कमांड्स को बोल सकते हैं: रजिस्टर, लॉगिन, स्कूल, कॉलेज, हाँ, नहीं, क्लियर, सबमिट, क्विज, हेल्प, हिन्दी, या इंग्लिश। भाषा बदलने के लिए हिन्दी या इंग्लिश बोलें।");
                } else {
                    speak("You can say the following commands: Register, Login, School, College, Yes, No, Clear, Submit, Quiz, Help, Hindi, or English. Say Hindi or English anytime to switch languages.");
                }
            }

            function speak(text, cb) {
                // Text-to-speech ONLY for blind students
                if(state.user.role !== 'blind') {
                    if(cb) cb();
                    return;
                }
                // suppress recognition auto-restarts while we speak
                state.suppressAutoRestart = true;
                try { window.speechSynthesis.cancel(); } catch(e) {}

                const utterance = new SpeechSynthesisUtterance(text);
                utterance.rate = 0.8;
                utterance.pitch = 1.3;
                utterance.volume = 1;
                utterance.lang = state.language;

                function attachHandlers() {
                    utterance.onstart = () => {
                        console.debug('TTS started');
                        const dbg = document.getElementById('voiceDebug'); if(dbg) dbg.innerText = 'TTS: speaking';
                    };
                    utterance.onend = () => {
                        state.suppressAutoRestart = false;
                        console.debug('TTS ended');
                        const dbg = document.getElementById('voiceDebug'); if(dbg) dbg.innerText = 'TTS: ended';
                        if(cb) cb();
                    };
                    utterance.onerror = (e) => {
                        console.error('Speech synthesis error:', e);
                        state.suppressAutoRestart = false;
                        if(cb) cb();
                    };
                }

                function selectVoiceAndSpeak() {
                    const voices = window.speechSynthesis.getVoices() || [];
                    let chosen = null;
                    if(voices.length) {
                        if(state.language === 'hi-IN') {
                            chosen = voices.find(v => v.lang && v.lang.toLowerCase().includes('hi')) || voices.find(v => /female|woman/i.test(v.name)) || voices[0];
                        } else {
                            chosen = voices.find(v => /female|woman/i.test(v.name)) || voices.find(v => /google us english female/i.test(v.name)) || voices[0];
                        }
                        if(chosen) utterance.voice = chosen;
                    }
                    attachHandlers();
                    try {
                        window.speechSynthesis.speak(utterance);
                    } catch(e) {
                        console.error('Could not speak:', e);
                        state.suppressAutoRestart = false;
                        if(cb) cb();
                    }
                }

                // If voices list is empty, wait for `voiceschanged` event, with a timeout fallback
                const currentVoices = window.speechSynthesis.getVoices();
                if(!currentVoices || currentVoices.length === 0) {
                    let fired = false;
                    const onVoices = () => {
                        if(fired) return; fired = true;
                        try { window.speechSynthesis.removeEventListener('voiceschanged', onVoices); } catch(e) {}
                        selectVoiceAndSpeak();
                    };
                    window.speechSynthesis.addEventListener('voiceschanged', onVoices);
                    // fallback after 500ms
                    setTimeout(() => {
                        if(fired) return; fired = true;
                        try { window.speechSynthesis.removeEventListener('voiceschanged', onVoices); } catch(e) {}
                        selectVoiceAndSpeak();
                    }, 500);
                } else {
                    selectVoiceAndSpeak();
                }
            }

            function startVoiceQuiz() {
                // Voice quiz ONLY for blind students
                if(state.user.role !== 'blind') {
                    return;
                }
                // Reset voice state before starting new quiz
                resetVoiceState();
                startQuizSetup();
            }

            function startTextQuiz() {
                // Text-based quiz for normal students
                if(state.user.role === 'blind') {
                    return;
                }
                // Start quiz directly without voice setup flow
                state.quizSetup = { classSelected: state.user.level, subjectSelected: state.user.subs[0], quizType: 'Text' };
                beginTextQuiz();
            }

            function beginTextQuiz() {
                document.getElementById('quizOverlay').classList.remove('hidden');
                const subject = (state.quizSetup.subjectSelected || state.user.subs[0] || '').toString();
                // Try to pick a question matching the subject (case-insensitive). If multiple available, pick a random one.
                const pool = QUIZ_BANK.filter(item => item.sub && item.sub.toLowerCase() === subject.toLowerCase());
                const q = (pool.length ? pool[Math.floor(Math.random() * pool.length)] : (QUIZ_BANK.find(item => item.level === state.quizSetup.classSelected && item.sub === subject) || QUIZ_BANK[0]));
                
                document.getElementById('quizCat').innerText = q.sub;
                document.getElementById('quizQ').innerText = q.q;
                document.getElementById('quizOptions').innerHTML = q.o.map((opt, i) => `
                    <button onclick="checkAns(${i}, ${q.a})" class="p-8 bg-white/5 border-2 border-white/10 rounded-[30px] text-2xl font-bold hover:bg-white hover:text-slate-900 transition">
                        ${opt}
                    </button>
                `).join('');
            }

            function checkAns(idx, correct) {
                const isCorrect = idx === correct;
                
                // Save result to backend API
                if (state.user.id && state.quizSetup.subjectSelected) {
                    const subject = state.quizSetup.subjectSelected;
                    const pool = QUIZ_BANK.filter(item => item.sub && item.sub.toLowerCase() === subject.toLowerCase());
                    const currentQuestion = pool.length ? pool[Math.floor(Math.random() * pool.length)] : QUIZ_BANK[0];
                    
                    fetch(state.apiUrl + '/quiz/result', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({
                            userId: state.user.id,
                            subject: subject,
                            questionId: currentQuestion.id,
                            userAnswer: idx,
                            isCorrect: isCorrect
                        })
                    })
                    .then(res => res.json())
                    .then(data => {
                        console.log('Quiz result saved:', data);
                    })
                    .catch(err => console.error('Quiz result API error:', err));
                }
                
                if(isCorrect) {
                    if(state.user.role === 'blind') {
                        speak("Correct! That is the right answer. Excellent work. Keep practicing to improve your skills.");
                    } else {
                        speak("That is correct! Well done.");
                    }
                } else {
                    if(state.user.role === 'blind') {
                        speak("Incorrect. The correct answer would have been option " + (correct + 1) + ". Don't worry, keep practicing and you will improve.");
                    } else {
                        speak("Incorrect. Keep practicing.");
                    }
                }
                setTimeout(endQuiz, 2000);
            }

            function endQuiz() { 
                // Reset voice state when exiting quiz
                resetVoiceState();
                document.getElementById('quizOverlay').classList.add('hidden');
                
                // Return to dashboard and provide voice guidance
                if(state.user.role === 'blind') {
                    showSection('dashboardSection', true);
                    speak("You have exited the practice assessment. Welcome back to your dashboard.");
                }
            }

            function triggerAi() {
                // AI assistant ONLY for blind students
                if(state.user.role === 'blind') {
                    speak("Hello! I am your AI study assistant. You have a study session scheduled for 11 AM for Mathematics. Your current study streak is 12 days, and your average quiz score is 88 percent. How can I help you today?");
                }
                // Normal students: no audio feedback
            }
        </script>
        <div id="voiceDebug" style="position:fixed;left:12px;bottom:12px;background:rgba(0,0,0,0.7);color:#fff;padding:8px 12px;border-radius:8px;font-size:12px;z-index:99999;max-width:320px;">Voice debug ready</div>
    </body>
    </html>
