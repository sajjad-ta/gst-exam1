[index.html](https://github.com/user-attachments/files/24992913/index.html)
<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Article Practice Exam - Result with Explanation</title>
    <style>
        :root { --primary: #1a73e8; --success: #28a745; --danger: #d93025; --gray: #6c757d; }
        body { font-family: 'Segoe UI', Arial, sans-serif; background-color: #f0f2f5; margin: 0; padding: 10px; }
        .container { max-width: 600px; margin: auto; background: white; padding: 15px; border-radius: 12px; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
        h1 { text-align: center; color: var(--primary); font-size: 1.5rem; }
        #timer { position: sticky; top: 0; background: var(--danger); color: white; text-align: center; padding: 8px; font-weight: bold; border-radius: 5px; margin-bottom: 15px; z-index: 100; }
        
        .question-card { background: #fff; border: 1px solid #ddd; padding: 15px; border-radius: 10px; margin-bottom: 15px; }
        .question-card p { font-weight: bold; margin-top: 0; }
        .options-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
        .opt-label { padding: 12px; border: 1px solid #eee; border-radius: 8px; background: #f9f9f9; display: flex; align-items: center; cursor: pointer; font-size: 0.9rem; }
        input[type="radio"] { margin-right: 10px; }

        /* রেজাল্ট সেকশন */
        #result-box { display: none; }
        .stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 20px; }
        .stat-item { padding: 15px; text-align: center; border-radius: 8px; color: white; }
        
        /* রিভিউ কার্ড স্টাইল */
        .review-card { border-left: 5px solid #ccc; margin-bottom: 20px; padding: 12px; background: #fafafa; border-radius: 5px; box-shadow: 0 1px 4px rgba(0,0,0,0.05); }
        .review-correct { border-left-color: var(--success); background: #e6ffed; }
        .review-wrong { border-left-color: var(--danger); background: #ffeef0; }
        .review-skip { border-left-color: var(--gray); background: #f8f9fa; }
        
        .explanation-text { margin-top: 10px; padding: 10px; background: #fffde7; border-top: 1px dashed #fbc02d; font-size: 0.85rem; color: #444; }
        .btn { width: 100%; padding: 15px; border: none; border-radius: 8px; font-size: 1rem; font-weight: bold; cursor: pointer; }
        #submit-btn { background: var(--primary); color: white; }
        .print-btn { background: var(--success); color: white; margin-top: 10px; }
        
        @media print { #timer, #submit-btn, .print-btn { display: none; } }
    </style>
</head>
<body>

<div id="timer">সময় বাকি: ২০:০০</div>

<div class="container">
    <div id="exam-ui">
        <h1>Article Mastery Exam</h1>
        <div style="margin-bottom:15px;">
            <input type="text" id="stu-name" placeholder="ছাত্রের নাম" style="width:94%; padding:10px; border-radius:5px; border:1px solid #ddd;">
        </div>
        <form id="quizForm">
            <div id="questions-list"></div>
            <button type="button" id="submit-btn" class="btn" onclick="calculateResult()">পরীক্ষা শেষ করুন</button>
        </form>
    </div>

    <div id="result-box">
        <h1>ফলাফল রিপোর্ট</h1>
        <div class="stat-grid">
            <div class="stat-item" style="background:var(--primary)">মোট নম্বর: <br><strong id="res-total">0</strong></div>
            <div class="stat-item" style="background:var(--success)">সঠিক: <br><strong id="res-correct">0</strong></div>
            <div class="stat-item" style="background:var(--danger)">ভুল: <br><strong id="res-wrong">0</strong></div>
            <div class="stat-item" style="background:var(--gray)">স্কিপ: <br><strong id="res-skip">0</strong></div>
        </div>
        
        <button class="print-btn btn" onclick="window.print()">রিপোর্টটি PDF সেভ করুন</button>
        
        <h2 style="border-bottom: 2px solid #ddd; padding-bottom: 10px;">প্রশ্ন ও ব্যাখ্যা:</h2>
        <div id="review-list"></div>
    </div>
</div>

<script>
    const questions = [
        { q: "He is ______ honest man.", options: ["a", "an", "the", "x"], ans: "an", exp: "Honest-এর 'h' অনুচ্চারিত এবং শুরুটা ভাওয়েল সাউন্ড 'অ' দিয়ে, তাই 'an' বসে।" },
        { q: "I saw ______ one-eyed man.", options: ["a", "an", "the", "x"], ans: "a", exp: "'One' শব্দের উচ্চারণ 'ওয়া' এর মতো হলে তার আগে 'a' বসে।" },
        { q: "It was ______ unique opportunity.", options: ["a", "an", "the", "x"], ans: "a", exp: "'U' এর উচ্চারণ 'ইউ' এর মতো হলে ভাওয়েল থাকা সত্ত্বেও 'a' বসে।" },
        { q: "Gold is ______ precious metal.", options: ["a", "an", "the", "x"], ans: "a", exp: "সাধারণভাবে কোনো একটি ধাতু বোঝাতে Consonant-এর আগে 'a' বসে।" },
        { q: "______ honesty of the boy is praiseworthy.", options: ["A", "An", "The", "x"], ans: "The", exp: "Abstract noun-এর পর 'of' থাকলে এবং নির্দিষ্ট গুণ বোঝালে 'The' বসে।" },
        { q: "He is ______ M.B.B.S.", options: ["a", "an", "the", "x"], ans: "an", exp: "'M' উচ্চারণ করতে শুরুতে ভাওয়েল সাউন্ড 'এ' আসে, তাই 'an' বসে।" },
        { q: "Twelve inches make ______ foot.", options: ["a", "an", "the", "x"], ans: "a", exp: "সংখ্যাগত এক (one) বোঝাতে 'a' বসে।" },
        { q: "______ rich are not always happy.", options: ["A", "An", "The", "x"], ans: "The", exp: "Adjective যখন পুরো একটি শ্রেণিকে বোঝায়, তখন তার আগে 'The' বসে।" },
        { q: "Iron is ______ useful metal.", options: ["a", "an", "the", "x"], ans: "a", exp: "'Useful' এর শুরুতে 'ইউ' সাউন্ড থাকায় 'a' বসে।" },
        { q: "______ water of this pond is dirty.", options: ["A", "An", "The", "x"], ans: "The", exp: "নির্দিষ্ট স্থানের (এই পুকুরের) পানি বোঝানো হয়েছে, তাই 'The' বসবে।" },
        { q: "He is ______ university student.", options: ["a", "an", "the", "x"], ans: "a", exp: "'U' এর উচ্চারণ 'ইউ' এর মতো হলে 'a' বসে।" },
        { q: "This is ______ book I want.", options: ["a", "an", "the", "x"], ans: "the", exp: "নির্দিষ্ট কোনো বইয়ের কথা বলা হয়েছে যা বক্তা চায়।" },
        { q: "He will come back in ______ hour.", options: ["a", "an", "the", "x"], ans: "an", exp: "'h' অনুচ্চারিত থাকায় ভাওয়েল সাউন্ড 'আ' অনুযায়ী 'an' বসে।" },
        { q: "My father is ______ L.L.B.", options: ["a", "an", "the", "x"], ans: "an", exp: "'L' (এল) এর শুরুতে 'এ' সাউন্ড আসে।" },
        { q: "______ Quran is a holy book.", options: ["A", "An", "The", "x"], ans: "The", exp: "পবিত্র ধর্মগ্রন্থের নামের আগে 'The' বসে।" },
        { q: "I saw ______ beggar in the street.", options: ["a", "an", "the", "x"], ans: "a", exp: "সাধারণ কোনো ব্যক্তিকে প্রথমবার পরিচয় করিয়ে দিতে 'a' বসে।" },
        { q: "Copper is ______ useful metal.", options: ["a", "an", "the", "x"], ans: "a", exp: "'ইউ' সাউন্ডের আগে 'a' বসে।" },
        { q: "He is ______ B.A.", options: ["a", "an", "the", "x"], ans: "a", exp: "'B' কনসোনেন্ট সাউন্ড দিয়ে শুরু হয়েছে।" },
        { q: "______ sun rises in the east.", options: ["A", "An", "The", "x"], ans: "The", exp: "পৃথিবীতে যা একটিই আছে (সূর্য) তার আগে 'The' বসে।" },
        { q: "I met him ______ year ago.", options: ["a", "an", "the", "x"], ans: "a", exp: "Year এর উচ্চারণ 'য়ি' (semi-vowel) দিয়ে শুরু, তাই 'a' বসে।" },
        { q: "He is ______ unlucky man.", options: ["a", "an", "the", "x"], ans: "an", exp: "এখানে 'U' এর উচ্চারণ 'আ' (Vowel sound)।" },
        { q: "He is ______ European.", options: ["a", "an", "the", "x"], ans: "a", exp: "European এর উচ্চারণ 'ইউ' এর মতো।" },
        { q: "______ more you read, the more you learn.", options: ["A", "An", "The", "x"], ans: "The", exp: "যত-তত বোঝাতে Comparative-এর আগে 'The' বসে।" },
        { q: "He plays ______ flute well.", options: ["a", "an", "the", "x"], ans: "the", exp: "বাদ্যযন্ত্র বাজানোর ক্ষেত্রে যন্ত্রের নামের আগে 'The' বসে।" },
        { q: "He is ______ best boy in the class.", options: ["a", "an", "the", "x"], ans: "the", exp: "Superlative (best) এর আগে 'The' বসে।" },
        { q: "English is ______ international language.", options: ["a", "an", "the", "x"], ans: "an", exp: "International শব্দের শুরু ভাওয়েল সাউন্ড 'ই' দিয়ে।" },
        { q: "Dhaka is on ______ Buriganga.", options: ["a", "an", "the", "x"], ans: "the", exp: "নদীর নামের আগে সবসময় 'The' বসে।" },
        { q: "Mr. Rahim is ______ honorary magistrate.", options: ["a", "an", "the", "x"], ans: "an", exp: "'h' অনুচ্চারিত থাকায় 'an' বসেছে।" },
        { q: "What ______ pity!", options: ["a", "an", "the", "x"], ans: "a", exp: "বিস্ময়সূচক বাক্যে What-এর পর 'a' বসে।" },
        { q: "______ Padma is a big river.", options: ["A", "An", "The", "x"], ans: "The", exp: "নদীর নামের আগে 'The' বসে।" },
        { q: "He is ______ heir to the property.", options: ["a", "an", "the", "x"], ans: "an", exp: "Heir এর উচ্চারণ 'এয়ার' (ভাওয়েল সাউন্ড)।" },
        { q: "I saw ______ owl in the tree.", options: ["a", "an", "the", "x"], ans: "an", exp: "Owl এর শুরুতে ভাওয়েল সাউন্ড আছে।" },
        { q: "This is ______ unique site.", options: ["a", "an", "the", "x"], ans: "a", exp: "'Unique' এর উচ্চারণ 'ইউ' এর মতো।" },
        { q: "Man is ______ mortal.", options: ["a", "an", "the", "x"], ans: "x", exp: "মানুষ জাতি বোঝাতে 'Man' এর আগে আর্টিকেল বসে না।" },
        { q: "______ lion is the king of beasts.", options: ["A", "An", "The", "x"], ans: "The", exp: "সিংহ জাতিকে নির্দেশ করায় 'The' বসেছে।" },
        { q: "Rice is ______ principal food of Bangladesh.", options: ["a", "an", "the", "x"], ans: "the", exp: "নির্দিষ্ট প্রধান খাবার বোঝাতে 'The' বসে।" },
        { q: "He is ______ F.R.C.S.", options: ["a", "an", "the", "x"], ans: "an", exp: "'F' উচ্চারণ করতে 'এ' সাউন্ড আসে।" },
        { q: "______ Himalayas are to the north of India.", options: ["A", "An", "The", "x"], ans: "The", exp: "পর্বতমালা বোঝাতে 'The' বসে।" },
        { q: "They go to ______ school every day.", options: ["a", "an", "the", "x"], ans: "x", exp: "পড়াশোনার উদ্দেশ্যে স্কুলে গেলে আর্টিকেল বসে না।" },
        { q: "It is ______ half-past ten.", options: ["a", "an", "the", "x"], ans: "x", exp: "সময়ের ফ্রেজে আর্টিকেল ব্যবহৃত হয় না।" }
    ];

    const listContainer = document.getElementById('questions-list');
    questions.forEach((item, i) => {
        listContainer.innerHTML += `
            <div class="question-card">
                <p>${i+1}. ${item.q}</p>
                <div class="options-grid">
                    ${item.options.map(opt => `
                        <label class="opt-label"><input type="radio" name="q${i}" value="${opt}"> ${opt}</label>
                    `).join('')}
                </div>
            </div>
        `;
    });

    let timeLeft = 20 * 60;
    const timerEl = document.getElementById('timer');
    const timerInterval = setInterval(() => {
        let min = Math.floor(timeLeft / 60);
        let sec = timeLeft % 60;
        timerEl.innerText = `সময় বাকি: ${min}:${sec < 10 ? '0' : ''}${sec}`;
        if (timeLeft <= 0) calculateResult();
        timeLeft--;
    }, 1000);

    function calculateResult() {
        clearInterval(timerInterval);
        let correct = 0, wrong = 0, skip = 0;
        let reviewHtml = "";

        questions.forEach((item, i) => {
            const sel = document.querySelector(`input[name="q${i}"]:checked`);
            let sAns = sel ? sel.value : "উত্তর দেননি";
            let resClass = "review-skip";
            
            if (!sel) {
                skip++;
            } else if (sel.value === item.ans) {
                correct++;
                resClass = "review-correct";
            } else {
                wrong++;
                resClass = "review-wrong";
            }

            reviewHtml += `
                <div class="review-card ${resClass}">
                    <p>${i+1}. ${item.q}</p>
                    <div style="font-size:0.9rem;">
                        <span style="color:var(--success)">সঠিক উত্তর: <b>${item.ans}</b></span> | 
                        <span>আপনার উত্তর: <b>${sAns}</b></span>
                    </div>
                    <div class="explanation-text">
                        <strong>ব্যাখ্যা:</strong> ${item.exp}
                    </div>
                </div>
            `;
        });

        const score = correct - (wrong * 0.25);
        document.getElementById('res-total').innerText = score.toFixed(2);
        document.getElementById('res-correct').innerText = correct;
        document.getElementById('res-wrong').innerText = wrong;
        document.getElementById('res-skip').innerText = skip;
        document.getElementById('review-list').innerHTML = reviewHtml;

        document.getElementById('exam-ui').style.display = "none";
        document.getElementById('timer').style.display = "none";
        document.getElementById('result-box').style.display = "block";
        window.scrollTo(0,0);
    }
</script>
</body>
</html>
