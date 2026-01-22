<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>بوابة التعليم</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;900&family=Amiri:wght@400;700&display=swap');
        :root { --spider-red: #e11d48; --spider-blue: #2563eb; --spider-dark: #020617; }
        body { font-family: 'Cairo', sans-serif; background-color: var(--spider-dark); color: #f8fafc; margin: 0; min-height: 100vh; display: flex; align-items: center; justify-content: center; overflow-x: hidden; }
        .screen { display: none; width: 100%; height: 100%; padding: 20px; position: relative; z-index: 10; }
        .screen.active { display: flex; flex-direction: column; align-items: center; justify-content: center; animation: fadeIn 0.2s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .spider-card { background: rgba(15, 23, 42, 0.95); border: 2px solid var(--spider-blue); border-bottom: 5px solid var(--spider-red); border-radius: 1.5rem; padding: 2rem; width: 100%; max-width: 500px; box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.3); }
        .quran-verse { font-family: 'Amiri', serif; color: #fbbf24; font-size: 1.3rem; font-weight: bold; text-align: center; margin-bottom: 1.5rem; line-height: 1.8; }
        .btn-spider { background: linear-gradient(135deg, var(--spider-red), #9f1239); color: white; font-weight: 800; padding: 0.85rem; border-radius: 0.8rem; border: none; width: 100%; cursor: pointer; transition: all 0.2s; display: flex; align-items: center; justify-content: center; gap: 8px; }
        .btn-spider:active { transform: scale(0.97); }
        .input-spider { background: #0f172a; border: 2px solid #1e293b; padding: 0.8rem 1rem; border-radius: 0.8rem; width: 100%; color: white; margin-bottom: 1rem; outline: none; text-align: center; transition: border-color 0.2s; }
        .input-spider:focus { border-color: var(--spider-blue); }
        .opt-correct { background: #16a34a !important; border-color: #22c55e !important; }
        .opt-wrong { background: #dc2626 !important; border-color: #ef4444 !important; }
    </style>
</head>
<body>

    <!-- شاشة الدخول -->
    <div id="screen-login" class="screen active">
        <div class="spider-card text-center">
            <div class="quran-verse">بِسْمِ اللهِ وَالْحَمْدُ للهِ الَّذِي خَلَقَنَا وَنَرْجِعُ إِلَيْهِ</div>
            <h2 class="text-white text-2xl font-black mb-1">بوابة الدخول</h2>
            <p id="login-instruction" class="text-slate-400 mb-6 font-semibold text-sm">يرجى إدخال معلوماتك للوصول إلى النظام</p>
            <input type="text" id="login-name" class="input-spider" placeholder="الاسم الثلاثي">
            <div id="password-section" class="hidden">
                <input type="password" id="login-code" class="input-spider" placeholder="رمز الدخول">
                <div id="register-section" class="hidden">
                    <input type="password" id="confirm-code" class="input-spider border-amber-600/50" placeholder="تأكيد الرمز">
                </div>
            </div>
            <button id="login-btn" onclick="handleLoginFlow()" class="btn-spider">متابعة</button>
            <p id="login-error" class="text-rose-500 text-sm mt-3 font-bold hidden"></p>
        </div>
    </div>

    <!-- الشاشة الرئيسية -->
    <div id="screen-dashboard" class="screen">
        <div class="w-full max-w-lg space-y-4">
            <div class="text-center mb-8">
                <h1 class="text-2xl font-black text-white">مرحباً، <span id="user-display-name" class="text-blue-500"></span></h1>
            </div>
            <div id="admin-controls" class="hidden space-y-4">
                <button onclick="openScreen('screen-members')" class="spider-card flex items-center justify-between hover:bg-slate-900/50 transition py-4 border-amber-500/50 w-full">
                    <span class="text-xl font-black">سجل الطلاب والرموز</span>
                </button>
            </div>
            <button onclick="openScreen('screen-create')" class="spider-card py-6 text-center group w-full">
                <h3 class="text-xl font-black group-hover:text-blue-400">إنشاء اختبار جديد</h3>
            </button>
            <button onclick="openScreen('screen-exams-list')" class="spider-card py-6 text-center group w-full">
                <h3 class="text-xl font-black group-hover:text-red-400">دخول قاعة الامتحانات</h3>
            </button>
            <button onclick="openScreen('screen-leaderboard')" class="spider-card py-6 text-center group w-full">
                <h3 class="text-xl font-black group-hover:text-amber-400">لوحة المتصدرين</h3>
            </button>
            <button onclick="logout()" class="btn-spider mt-4 bg-slate-800 border border-slate-700">تسجيل الخروج</button>
        </div>
    </div>

    <!-- شاشة الإنشاء -->
    <div id="screen-create" class="screen">
        <div class="spider-card max-w-2xl">
            <h2 class="text-2xl font-black mb-4 text-center">إنشاء الاختبار</h2>
            <input type="text" id="quiz-subject" class="input-spider" placeholder="عنوان المادة">
            <textarea id="quiz-content" class="input-spider h-48 text-right text-sm" placeholder="Q: السؤال؟&#10;A) خيار&#10;B) خيار ✅&#10;C) خيار&#10;D) خيار"></textarea>
            <button onclick="handleSaveQuiz()" class="btn-spider">نشر الاختبار</button>
            <button onclick="openScreen('screen-dashboard')" class="btn-spider mt-4 bg-slate-800 border border-slate-700">العودة للرئيسية</button>
        </div>
    </div>

    <!-- قاعة الامتحانات -->
    <div id="screen-exams-list" class="screen">
        <div class="max-w-xl w-full">
            <h2 class="text-3xl font-black mb-8 text-center">قاعة الامتحانات</h2>
            <div id="exams-list-container" class="space-y-4"></div>
            <button onclick="openScreen('screen-dashboard')" class="btn-spider mt-10">العودة للرئيسية</button>
        </div>
    </div>

    <!-- جلسة الاختبار -->
    <div id="screen-quiz-session" class="screen">
        <div class="spider-card max-w-2xl">
            <div class="flex justify-between items-center mb-6">
                <span id="quiz-progress" class="text-xs text-blue-400 font-bold"></span>
            </div>
            <h3 id="session-question" class="text-xl font-black mb-8 text-center text-white"></h3>
            <div id="session-options" class="grid gap-3"></div>
            <div id="session-controls" class="mt-8 hidden">
                <button id="next-q-btn" onclick="goToNextQuestion()" class="btn-spider">السؤال التالي</button>
            </div>
            <button onclick="openScreen('screen-exams-list')" class="btn-spider mt-6 bg-slate-800 border border-slate-700">انسحاب من الاختبار والعودة</button>
        </div>
    </div>

    <!-- لوحة المتصدرين -->
    <div id="screen-leaderboard" class="screen">
        <div class="spider-card max-w-xl text-center">
            <h2 class="text-2xl font-black mb-6 text-amber-400">لوحة المتصدرين</h2>
            
            <div id="leaderboard-subjects" class="flex flex-wrap gap-2 justify-center mb-6">
                <!-- أزرار المواد تظهر هنا -->
            </div>

            <div id="leaderboard-data" class="space-y-3 mb-6">
                <!-- النتائج تظهر هنا عند اختيار مادة -->
                <p class="text-slate-500 text-sm">يرجى اختيار مادة لعرض النتائج</p>
            </div>

            <div id="leaderboard-admin-btns" class="hidden space-y-2 mb-4">
                <button onclick="clearAllScores()" class="btn-spider bg-slate-800 border border-slate-700 text-xs py-2">مسح جميع السجلات نهائياً</button>
            </div>
            <button onclick="openScreen('screen-dashboard')" class="btn-spider">العودة للرئيسية</button>
        </div>
    </div>

    <!-- سجل الطلاب -->
    <div id="screen-members" class="screen">
        <div class="spider-card max-w-xl">
            <h2 class="text-2xl font-black mb-6 text-center">سجل الطلاب</h2>
            <div id="members-data" class="space-y-2 mb-6"></div>
            <button onclick="openScreen('screen-dashboard')" class="btn-spider">العودة للرئيسية</button>
        </div>
    </div>

    <script>
        const DB = {
            get: (key) => JSON.parse(localStorage.getItem('arass_' + key) || '[]'),
            save: (key, data) => {
                const list = DB.get(key);
                list.push(data);
                localStorage.setItem('arass_' + key, JSON.stringify(list));
            },
            update: (key, newList) => {
                localStorage.setItem('arass_' + key, JSON.stringify(newList));
            }
        };

        const ADMIN_NAME = "Son of abd";
        const ADMIN_CODE = "ARASS23";
        let currentUser = null, loginState = "NAME", activeExam = null, qIndex = 0, score = 0;

        window.openScreen = (id) => {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            if(id === 'screen-exams-list') renderExams();
            if(id === 'screen-leaderboard') renderLeaderboard();
            if(id === 'screen-members') renderMembers();
        };

        window.logout = () => {
            currentUser = null;
            loginState = "NAME";
            document.getElementById('login-name').value = "";
            document.getElementById('login-code').value = "";
            document.getElementById('confirm-code').value = "";
            document.getElementById('password-section').classList.add('hidden');
            document.getElementById('register-section').classList.add('hidden');
            document.getElementById('admin-controls').classList.add('hidden');
            document.getElementById('login-instruction').innerText = "يرجى إدخال معلوماتك للوصول إلى النظام";
            openScreen('screen-login');
        };

        window.handleLoginFlow = () => {
            const name = document.getElementById('login-name').value.trim();
            const code = document.getElementById('login-code').value.trim();
            const err = document.getElementById('login-error');
            
            if(!name) return;

            if(loginState === "NAME") {
                if(name === ADMIN_NAME) {
                    showPassUI("أدخل رمز الإدارة السري");
                    loginState = "ADMIN_PASS";
                } else {
                    const users = DB.get('users');
                    const found = users.find(u => u.name === name);
                    if(found) {
                        currentUser = found;
                        showPassUI("الاسم مسجل، أدخل رمزك:");
                        loginState = "USER_PASS";
                    } else {
                        showPassUI("حساب جديد، اختر رمزاً:", true);
                        loginState = "REGISTER";
                    }
                }
            } else {
                if(loginState === "ADMIN_PASS" && code === ADMIN_CODE) finalizeLogin({name: ADMIN_NAME, role:'admin'});
                else if(loginState === "USER_PASS" && code === currentUser.code) finalizeLogin(currentUser);
                else if(loginState === "REGISTER") {
                    const confirm = document.getElementById('confirm-code').value.trim();
                    if(code === confirm) {
                        const newUser = {name, code, role:'student'};
                        DB.save('users', newUser);
                        finalizeLogin(newUser);
                    } else { err.innerText = "الرموز غير متطابقة"; err.classList.remove('hidden'); }
                } else { err.innerText = "الرمز خاطئ"; err.classList.remove('hidden'); }
            }
        };

        function showPassUI(msg, reg) {
            document.getElementById('login-instruction').innerText = msg;
            document.getElementById('password-section').classList.remove('hidden');
            if(reg) document.getElementById('register-section').classList.remove('hidden');
        }

        function finalizeLogin(user) {
            currentUser = user;
            document.getElementById('user-display-name').innerText = user.name;
            if(user.role === 'admin') {
                document.getElementById('admin-controls').classList.remove('hidden');
                document.getElementById('leaderboard-admin-btns').classList.remove('hidden');
            } else {
                document.getElementById('leaderboard-admin-btns').classList.add('hidden');
            }
            openScreen('screen-dashboard');
        }

        window.handleSaveQuiz = () => {
            const sub = document.getElementById('quiz-subject').value;
            const content = document.getElementById('quiz-content').value;
            if(!sub || !content) return;
            const qs = [];
            content.split('Q:').filter(b => b.trim()).forEach(b => {
                const lines = b.split('\n').filter(l => l.trim());
                const correct = lines.findIndex(l => l.includes('✅')) - 1;
                if(correct >= 0) {
                    qs.push({q: lines[0], options: lines.slice(1).map(l => l.replace('✅','')), correct});
                }
            });
            if(qs.length > 0) {
                const now = new Date();
                const dateStr = `${now.getFullYear()}/${now.getMonth()+1}/${now.getDate()}`;
                DB.save('exams', {
                    id: Date.now(),
                    subject: sub, 
                    questions: qs, 
                    createdBy: currentUser.name,
                    date: dateStr
                });
                alert("تم الحفظ بنجاح");
                openScreen('screen-dashboard');
            }
        };

        function renderExams() {
            const cont = document.getElementById('exams-list-container');
            const exams = DB.get('exams');
            cont.innerHTML = exams.map((ex, i) => {
                const canDelete = currentUser.role === 'admin' || ex.createdBy === currentUser.name;
                return `
                <div class="spider-card w-full relative">
                    <div class="cursor-pointer" onclick="launchExam(${i})">
                        <h4 class="font-black text-lg">${ex.subject}</h4>
                        <div class="flex justify-between text-xs text-slate-500 mt-2">
                            <span>المنشئ: ${ex.createdBy}</span>
                            <span>التاريخ: ${ex.date}</span>
                        </div>
                        <p class="text-xs text-blue-400 mt-1">${ex.questions.length} سؤال</p>
                    </div>
                    ${canDelete ? `<button onclick="deleteExam(${ex.id}, '${ex.subject.replace(/'/g, "\\'")}')" class="absolute top-4 left-4 text-rose-500 text-xs font-bold bg-rose-500/10 px-2 py-1 rounded">حذف</button>` : ''}
                </div>
            `}).join('') || "<p class='text-center opacity-50'>لا توجد امتحانات متوفرة</p>";
        }

        window.deleteExam = (id, subject) => {
            if(!confirm("هل أنت متأكد من حذف هذا الاختبار وجميع نتائجه؟")) return;
            const exams = DB.get('exams');
            const filteredExams = exams.filter(e => e.id !== id);
            DB.update('exams', filteredExams);
            const scores = DB.get('scores');
            const filteredScores = scores.filter(s => s.subject !== subject);
            DB.update('scores', filteredScores);
            renderExams();
        };

        window.launchExam = (i) => {
            activeExam = DB.get('exams')[i];
            qIndex = 0; score = 0;
            renderQuestion();
            openScreen('screen-quiz-session');
        };

        function renderQuestion() {
            const q = activeExam.questions[qIndex];
            document.getElementById('quiz-progress').innerText = `سؤال ${qIndex+1} من ${activeExam.questions.length}`;
            document.getElementById('session-question').innerText = q.q;
            document.getElementById('session-controls').classList.add('hidden');
            document.getElementById('session-options').innerHTML = q.options.map((opt, i) => `
                <button class="btn-spider bg-slate-900 border border-slate-700 w-full" onclick="checkAns(${i})">${opt}</button>
            `).join('');
        }

        window.checkAns = (i) => {
            const btns = document.querySelectorAll('#session-options button');
            btns.forEach(b => b.disabled = true);
            if(i === activeExam.questions[qIndex].correct) { btns[i].classList.add('opt-correct'); score++; }
            else { btns[i].classList.add('opt-wrong'); btns[activeExam.questions[qIndex].correct].classList.add('opt-correct'); }
            document.getElementById('session-controls').classList.remove('hidden');
        };

        window.goToNextQuestion = () => {
            if(++qIndex < activeExam.questions.length) renderQuestion();
            else {
                const now = new Date();
                const dateStr = `${now.getFullYear()}/${now.getMonth()+1}/${now.getDate()}`;
                DB.save('scores', {
                    user: currentUser.name, 
                    subject: activeExam.subject, 
                    score, 
                    total: activeExam.questions.length,
                    date: dateStr
                });
                alert(`انتهى! نتيجتك: ${score}/${activeExam.questions.length}`);
                openScreen('screen-dashboard');
            }
        };

        function renderLeaderboard() {
            const scores = DB.get('scores');
            const subjectsDiv = document.getElementById('leaderboard-subjects');
            const dataDiv = document.getElementById('leaderboard-data');
            
            // استخراج المواد الفريدة التي لها نتائج
            const uniqueSubjects = [...new Set(scores.map(s => s.subject))];
            
            if(uniqueSubjects.length === 0) {
                subjectsDiv.innerHTML = "";
                dataDiv.innerHTML = "<p class='text-slate-500 text-sm'>لا توجد نتائج مسجلة حالياً</p>";
                return;
            }

            subjectsDiv.innerHTML = uniqueSubjects.map(sub => `
                <button onclick="showSubjectScores('${sub.replace(/'/g, "\\'")}')" class="px-3 py-1 bg-blue-900/30 border border-blue-500/50 rounded-full text-xs text-blue-300 hover:bg-blue-500 hover:text-white transition">
                    ${sub}
                </button>
            `).join('');
        }

        window.showSubjectScores = (subject) => {
            const scores = DB.get('scores').filter(s => s.subject === subject);
            const dataDiv = document.getElementById('leaderboard-data');
            
            // ترتيب حسب الدرجة الأعلى
            scores.sort((a,b) => b.score - a.score);

            dataDiv.innerHTML = `
                <h4 class="text-blue-400 font-bold mb-4 text-sm underline decoration-spider-red underline-offset-4">نتائج مادة: ${subject}</h4>
                ${scores.map((s, idx) => `
                    <div class="p-3 bg-slate-800/80 rounded-lg text-sm text-right border-r-4 ${idx === 0 ? 'border-amber-400' : 'border-slate-600'}">
                        <div class="flex justify-between font-bold">
                            <span>${idx + 1}. ${s.user}</span>
                            <span class="text-blue-400">${s.score}/${s.total}</span>
                        </div>
                        <div class="text-[10px] text-slate-500 mt-1">
                            ${s.date || ''}
                        </div>
                    </div>
                `).join('')}
            `;
        }

        window.clearAllScores = () => {
            if(!confirm("سيتم حذف جميع السجلات لكل المواد نهائياً. هل أنت متأكد؟")) return;
            DB.update('scores', []);
            renderLeaderboard();
        }

        function renderMembers() {
            document.getElementById('members-data').innerHTML = DB.get('users').map(u => `
                <div class="p-2 bg-slate-900 border border-slate-800 rounded flex justify-between">
                    <span>${u.name}</span>
                    <code class="text-blue-400 font-bold">${u.code}</code>
                </div>
            `).join('') || "لا يوجد طلاب مسجلين";
        }
    </script>
</body>
</html>
