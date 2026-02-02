# Shwapnosiri-
<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>স্বপ্নসিঁড়ি - মেম্বার পোর্টাল</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        :root { --primary: #008955; --bg: #f0f2f5; }
        body { background-color: var(--bg); font-family: 'Segoe UI', sans-serif; overflow-x: hidden; }
        .tab-content { display: none; }
        .active-tab { display: block; }
        .profile-img { width: 60px; height: 60px; border-radius: 50%; object-fit: cover; border: 3px solid white; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        .unseen-dot { position: absolute; top: 10px; right: 25%; width: 10px; height: 10px; background: red; border-radius: 50%; border: 2px solid white; display: none; }
        .unseen-notice { border-left: 4px solid #ef4444 !important; background: #fff5f5 !important; }
    </style>
</head>
<body class="pb-24">

    <div id="login-screen" class="fixed inset-0 z-[2000] bg-green-700 flex items-center justify-center p-6 text-center">
        <div class="bg-white w-full max-w-md rounded-3xl p-8 shadow-2xl">
            <h2 class="text-3xl font-black text-green-700 mb-2">স্বপ্নসিঁড়ি</h2>
            <p class="text-xs font-bold text-gray-400 uppercase tracking-widest mb-8">মেম্বার পোর্টাল</p>
            <div class="w-20 h-20 bg-green-100 text-green-700 rounded-full flex items-center justify-center mx-auto mb-6 text-3xl">
                <i class="fas fa-fingerprint"></i>
            </div>
            <h1 class="text-xl font-bold text-gray-800 mb-6">আপনার সদস্য আইডি দিন</h1>
            <input id="user-id-input" type="text" placeholder="সদস্য আইডি" 
                   class="w-full border-2 border-gray-100 bg-gray-50 p-4 rounded-2xl mb-4 focus:border-green-500 outline-none text-center font-bold text-lg">
            <button onclick="login()" class="w-full bg-green-700 text-white py-4 rounded-2xl font-bold shadow-lg active:scale-95 transition">প্রবেশ করুন</button>
        </div>
    </div>

    <div id="main-app" class="hidden">
        <header class="bg-green-700 text-white p-6 rounded-b-[40px] shadow-lg mb-6">
            <div class="flex justify-between items-center mb-6">
                <div class="flex items-center space-x-4">
                    <img id="user-photo" src="" class="profile-img" onerror="this.src='https://cdn-icons-png.flaticon.com/512/149/149071.png'">
                    <div>
                        <p class="text-green-100 text-xs italic">আসসালামু আলাইকুম,</p>
                        <h2 id="user-name" class="text-lg font-bold">লোড হচ্ছে...</h2>
                    </div>
                </div>
                <button onclick="location.reload()" class="w-10 h-10 bg-white/10 rounded-full flex items-center justify-center">
                    <i class="fas fa-power-off"></i>
                </button>
            </div>
            
            <div class="bg-white text-gray-800 p-6 rounded-3xl shadow-xl">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-[10px] font-bold text-gray-400 uppercase tracking-wider mb-1">আপনার মোট জমা (সঞ্চয়)</p>
                        <h3 id="user-total" class="text-3xl font-black text-green-700">৳ ০</h3>
                    </div>
                    <div class="bg-green-100 text-green-700 px-3 py-1 rounded-full text-[10px] font-bold uppercase">Active</div>
                </div>
                <div class="flex justify-between mt-4 pt-4 border-t border-gray-100">
                    <div>
                        <p class="text-[10px] text-gray-400 uppercase font-bold">মেম্বার আইডি</p>
                        <p id="user-id-display" class="font-bold text-sm text-gray-700">----</p>
                    </div>
                    <div class="text-right">
                        <p class="text-[10px] text-gray-400 uppercase font-bold">সংগঠন</p>
                        <p class="font-bold text-sm text-green-700">স্বপ্নসিঁড়ি</p>
                    </div>
                </div>
            </div>
        </header>

        <main class="px-4 space-y-6">
            <div class="bg-white p-4 rounded-3xl shadow-sm border-l-4 border-pink-500 flex items-center justify-between">
                <div class="flex items-center space-x-3">
                    <div class="bg-pink-100 p-3 rounded-2xl">
                        <i class="fas fa-mobile-alt text-pink-600 text-xl"></i>
                    </div>
                    <div>
                        <p class="text-[10px] text-gray-400 font-bold uppercase tracking-tighter">বিকাশ পেমেন্ট (পার্সোনাল)</p>
                        <p id="bkash-number-display" class="text-lg font-black text-gray-800">+8801771082008</p> 
                    </div>
                </div>
                <button onclick="copyBkash()" class="bg-gray-50 p-3 rounded-xl text-gray-400 hover:text-pink-600 active:scale-90 transition">
                    <i class="far fa-copy text-lg"></i>
                </button>
            </div>

            <div id="tab-my-ledger" class="tab-content active-tab">
                <h4 class="font-bold text-gray-700 mb-4"><i class="fas fa-wallet mr-2 text-green-600"></i> জমার বিবরণ</h4>
                <div id="my-transactions" class="space-y-3"></div>
            </div>

            <div id="tab-fund" class="tab-content">
                <div class="flex justify-between items-center mb-4">
                    <h4 class="font-bold text-gray-700">সংগঠনের ফান্ড রিপোর্ট</h4>
                    <button onclick="downloadReport()" class="text-xs bg-green-50 text-green-700 px-3 py-2 rounded-xl font-bold border border-green-100 shadow-sm">
                        <i class="fas fa-download mr-1"></i> এক্সেল
                    </button>
                </div>
                <div class="grid grid-cols-2 gap-4 mb-4">
                    <div class="bg-white p-5 rounded-3xl shadow-sm border-b-4 border-green-500">
                        <p class="text-[10px] text-gray-400 font-bold uppercase">মোট জমা</p>
                        <h5 id="fund-total-earn" class="text-lg font-black text-green-600">৳ ০</h5>
                    </div>
                    <div class="bg-white p-5 rounded-3xl shadow-sm border-b-4 border-red-500">
                        <p class="text-[10px] text-gray-400 font-bold uppercase">মোট খরচ</p>
                        <h5 id="fund-total-exp" class="text-lg font-black text-red-500">৳ ০</h5>
                    </div>
                </div>
                <div class="bg-green-700 p-6 rounded-3xl text-white text-center shadow-lg mb-6">
                    <p class="text-sm opacity-80">বর্তমান ফান্ড ব্যালেন্স</p>
                    <h5 id="fund-balance" class="text-3xl font-black">৳ ০</h5>
                </div>

                <h4 class="font-bold text-gray-700 mb-3 text-sm italic">সাম্প্রতিক সকল লেনদেন (ডেসক্রিপশনসহ)</h4>
                <div id="all-org-transactions" class="space-y-3"></div>
            </div>

            <div id="tab-notices" class="tab-content">
                <div class="flex justify-between items-center mb-4">
                    <h4 class="font-bold text-gray-700">নোটিশ বোর্ড</h4>
                    <button onclick="markAllAsRead()" class="text-[10px] text-blue-600 font-bold">সব পঠিত করুন</button>
                </div>
                <div id="user-notice-list" class="space-y-3"></div>
            </div>
        </main>

        <nav class="fixed bottom-0 left-0 right-0 bg-white border-t flex justify-around p-3 shadow-2xl z-[1000]">
            <button onclick="showTab('my-ledger')" id="btn-my-ledger" class="flex flex-col items-center text-green-700">
                <i class="fas fa-user-circle text-xl"></i>
                <span class="text-[10px] font-bold mt-1">আমার হিসাব</span>
            </button>
            <button onclick="showTab('fund')" id="btn-fund" class="flex flex-col items-center text-gray-400">
                <i class="fas fa-university text-xl"></i>
                <span class="text-[10px] font-bold mt-1">ফান্ড রিপোর্ট</span>
            </button>
            <button onclick="showTab('notices')" id="btn-notices" class="flex flex-col items-center text-gray-400 relative">
                <i class="fas fa-bell text-xl"></i>
                <div id="unseen-badge" class="unseen-dot"></div>
                <span class="text-[10px] font-bold mt-1">নোটিশ</span>
            </button>
        </nav>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getDatabase, ref, onValue, get } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js";

        const firebaseConfig = {
            apiKey: "AIzaSyDmzzpGt6zE__9nBIP51Yt0s6jY58B10dY",
            authDomain: "shwapnosiri-2.firebaseapp.com",
            databaseURL: "https://shwapnosiri-2-default-rtdb.asia-southeast1.firebasedatabase.app",
            projectId: "shwapnosiri-2",
            storageBucket: "shwapnosiri-2.firebasestorage.app",
            messagingSenderId: "261712251859",
            appId: "1:261712251859:web:c7fc7f482c56f2d55b3d26"
        };

        const app = initializeApp(firebaseConfig);
        const db = getDatabase(app);

        let currentUser = null;
        let seenNotices = JSON.parse(localStorage.getItem('seenNotices') || '[]');

        window.login = () => {
            const id = document.getElementById('user-id-input').value;
            if(!id) return alert("আইডি দিন");

            onValue(ref(db, "members/" + id), (snap) => {
                const data = snap.val();
                if(data) {
                    currentUser = data;
                    document.getElementById('login-screen').classList.add('hidden');
                    document.getElementById('main-app').classList.remove('hidden');
                    document.getElementById('user-name').innerText = data.name;
                    document.getElementById('user-id-display').innerText = data.memberId;
                    document.getElementById('user-photo').src = data.photo || '';
                    loadData();
                } else { 
                    alert("আইডি পাওয়া যায়নি!");
                }
            });
        };

        function loadData() {
            onValue(ref(db, "transactions"), (snap) => {
                const txData = snap.val() || {};
                const allTransactions = Object.values(txData);
                
                // শুধুমাত্র বর্তমান ইউজারের জমা (EARN) ফিল্টার
                const myTxs = allTransactions.filter(t => 
                    String(t.memberId) === String(currentUser.memberId) && t.type === 'EARN'
                );

                let myTotal = 0, myH = '';
                myTxs.slice().reverse().forEach(t => {
                    myTotal += Number(t.amount);
                    myH += `<div class="bg-white p-4 rounded-2xl flex justify-between shadow-sm border border-gray-50">
                                <div>
                                    <p class="font-bold text-sm text-gray-800">${t.category}</p>
                                    <p class="text-[10px] text-gray-400">${t.date} ${t.note ? '| ' + t.note : ''}</p>
                                </div>
                                <p class="font-black text-green-600">+ ৳${Number(t.amount).toLocaleString('bn-BD')}</p>
                            </div>`;
                });
                
                document.getElementById('user-total').innerText = "৳ " + myTotal.toLocaleString('bn-BD');
                document.getElementById('my-transactions').innerHTML = myH || '<p class="text-center text-gray-400 py-4">কোনো জমার রেকর্ড নেই</p>';

                // ফান্ড সামারি (সব ট্রানজেকশন)
                const earn = allTransactions.filter(t => t.type === 'EARN').reduce((a, b) => a + Number(b.amount), 0);
                const exp = allTransactions.filter(t => t.type === 'EXPENSE').reduce((a, b) => a + Number(b.amount), 0);
                
                document.getElementById('fund-total-earn').innerText = "৳ " + earn.toLocaleString('bn-BD');
                document.getElementById('fund-total-exp').innerText = "৳ " + exp.toLocaleString('bn-BD');
                document.getElementById('fund-balance').innerText = "৳ " + (earn - exp).toLocaleString('bn-BD');

                // সংগঠনের সাম্প্রতিক লেনদেন (ডেসক্রিপশনসহ)
                let allH = '';
                allTransactions.slice().reverse().slice(0, 25).forEach(t => {
                    allH += `<div class="bg-white p-3 rounded-xl border border-gray-50 shadow-sm mb-2">
                                <div class="flex justify-between items-start">
                                    <div class="flex flex-col">
                                        <span class="text-gray-700 font-bold text-xs">${t.category}</span>
                                        <span class="text-[9px] text-gray-400">${t.date} ${t.memberId === 'ORGANIZATION' ? '• সংগঠন' : ''}</span>
                                    </div>
                                    <span class="font-bold text-xs ${t.type === 'EARN' ? 'text-green-600' : 'text-red-500'}">
                                        ${t.type === 'EARN' ? '+' : '-'} ৳${t.amount}
                                    </span>
                                </div>
                                ${t.note ? `<p class="mt-1 text-[10px] text-gray-500 italic border-t pt-1 border-gray-50">${t.note}</p>` : ''}
                             </div>`;
                });
                document.getElementById('all-org-transactions').innerHTML = allH || '<p class="text-center text-gray-400 py-4">কোনো লেনদেন নেই</p>';
            });

            onValue(ref(db, "notices"), (snap) => {
                const noticesData = snap.val() || {};
                const notices = Object.keys(noticesData).map(key => ({ id: key, ...noticesData[key] })).reverse();
                let h = '', unseenCount = 0;
                notices.forEach(n => {
                    const isUnseen = !seenNotices.includes(n.id);
                    if(isUnseen) unseenCount++;
                    h += `<div onclick="markAsSeen('${n.id}')" class="p-4 rounded-2xl shadow-sm border border-gray-100 ${isUnseen ? 'unseen-notice' : 'bg-white'}">
                            <div class="flex justify-between text-[10px] font-bold text-gray-400 mb-1"><span>${n.date}</span>${isUnseen?'<span class="text-red-500">NEW</span>':''}</div>
                            <p class="text-sm text-gray-700">${n.message}</p>
                          </div>`;
                });
                document.getElementById('user-notice-list').innerHTML = h || '<p class="text-center text-gray-400 py-4">কোনো নোটিশ নেই</p>';
                document.getElementById('unseen-badge').style.display = unseenCount > 0 ? 'block' : 'none';
            });
        }

        window.markAsSeen = (id) => { 
            if(!seenNotices.includes(id)) { 
                seenNotices.push(id);
                localStorage.setItem('seenNotices', JSON.stringify(seenNotices));
                loadData();
            } 
        };
        
        window.markAllAsRead = () => { 
            get(ref(db, "notices")).then(snap => { 
                seenNotices = Object.keys(snap.val() || {});
                localStorage.setItem('seenNotices', JSON.stringify(seenNotices));
                loadData();
            }); 
        };
        
        window.copyBkash = () => { 
            navigator.clipboard.writeText("+8801771082008");
            alert("বিকাশ নাম্বার কপি হয়েছে!");
        };

        window.showTab = (tabId) => {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById('tab-' + tabId).classList.add('active-tab');
            document.querySelectorAll('nav button').forEach(b => b.classList.replace('text-green-700', 'text-gray-400'));
            document.getElementById('btn-' + tabId).classList.replace('text-gray-400', 'text-green-700');
        };

        window.downloadReport = () => {
            get(ref(db, "transactions")).then(snap => {
                const data = Object.values(snap.val() || {});
                const ws = XLSX.utils.json_to_sheet(data);
                const wb = XLSX.utils.book_new();
                XLSX.utils.book_append_sheet(wb, ws, "Financial Report");
                XLSX.writeFile(wb, "Siri_Fund_Report.xlsx");
            });
        };
    </script>
</body>
</html>
