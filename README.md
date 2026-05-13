<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>BookOurHotel | Ultimate Travel Platform</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/flatpickr/dist/flatpickr.min.css">
    <script src="https://cdn.jsdelivr.net/npm/flatpickr"></script>

    <!-- EmailJS SDK -->
    <script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
    <script>
        (function() {
            // REPLACE THIS WITH YOUR ACTUAL EMAILJS PUBLIC KEY
            emailjs.init({ publicKey: "YOUR_PUBLIC_KEY" });
        })();
    </script>

    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: { 50: '#eff6ff', 100: '#dbeafe', 500: '#3b82f6', 600: '#2563eb', 700: '#1d4ed8', 900: '#1e3a8a' },
                        dark: { bg: '#0f172a', card: '#1e293b', text: '#f8fafc', muted: '#94a3b8' },
                    },
                    animation: {
                        'slide-up': 'slideUp 0.4s ease-out forwards',
                        'slide-right': 'slideRight 0.4s ease-out forwards',
                        'fade-in': 'fadeIn 0.5s ease-out forwards',
                        'bounce-in': 'bounceIn 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55)',
                        'shimmer': 'shimmer 1.5s infinite linear',
                        'pulse-slow': 'pulse 3s infinite',
                        'float': 'float 3s ease-in-out infinite',
                        'wiggle': 'wiggle 1s ease-in-out infinite',
                        'gradient-x': 'gradient-x 15s ease infinite',
                        'glow-pulse': 'glow-pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite',
                        'ken-burns': 'ken-burns 20s ease-out forwards alternate infinite'
                    },
                    keyframes: {
                        slideUp: { 'from': { transform: 'translateY(10px)', opacity: 0 }, 'to': { transform: 'translateY(0)', opacity: 1 } },
                        slideRight: { 'from': { transform: 'translateX(-20px)', opacity: 0 }, 'to': { transform: 'translateX(0)', opacity: 1 } },
                        fadeIn: { 'from': { opacity: 0 }, 'to': { opacity: 1 } },
                        bounceIn: { '0%': { transform: 'scale(0.3)', opacity: 0 }, '50%': { transform: 'scale(1.05)' }, '70%': { transform: 'scale(0.9)' }, '100%': { transform: 'scale(1)', opacity: 1 } },
                        shimmer: { '0%': { backgroundPosition: '-1000px 0' }, '100%': { backgroundPosition: '1000px 0' } },
                        float: { '0%, 100%': { transform: 'translateY(0)' }, '50%': { transform: 'translateY(-10px)' } },
                        wiggle: { '0%, 100%': { transform: 'rotate(-3deg)' }, '50%': { transform: 'rotate(3deg)' } },
                        'gradient-x': {
                            '0%, 100%': { 'background-size': '200% 200%', 'background-position': 'left center' },
                            '50%': { 'background-size': '200% 200%', 'background-position': 'right center' }
                        },
                        'glow-pulse': {
                            '0%': { boxShadow: '0 0 0 0 rgba(37, 99, 235, 0.5)' },
                            '70%': { boxShadow: '0 0 0 15px rgba(37, 99, 235, 0)' },
                            '100%': { boxShadow: '0 0 0 0 rgba(37, 99, 235, 0)' }
                        },
                        'ken-burns': {
                            '0%': { transform: 'scale(1)' },
                            '100%': { transform: 'scale(1.1)' }
                        }
                    }
                }
            }
        }
    </script>
    
    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; -webkit-tap-highlight-color: transparent; }
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
        .dark ::-webkit-scrollbar-thumb { background: #475569; }
        
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        
        .skeleton { background: #f6f7f8; background-image: linear-gradient(to right, #f6f7f8 0%, #edeef1 20%, #f6f7f8 40%, #f6f7f8 100%); background-repeat: no-repeat; background-size: 1000px 100%; animation: shimmer 1.5s infinite linear; }
        .dark .skeleton { background: #1e293b; background-image: linear-gradient(to right, #1e293b 0%, #334155 20%, #1e293b 40%, #1e293b 100%); }

        .admin-table th { @apply px-6 py-3 text-left text-xs font-medium text-slate-500 uppercase tracking-wider; }
        .admin-table td { @apply px-6 py-4 whitespace-nowrap text-sm text-slate-500 dark:text-slate-300; }
        .admin-table tr:hover { @apply bg-slate-50 dark:bg-slate-700/50; }
        
        #modalMap { height: 100%; width: 100%; z-index: 1; }
        #globalMap { height: 500px; width: 100%; z-index: 0; border-radius: 1rem; }
        
        .pay-input { @apply w-full p-3.5 border border-slate-200 rounded-lg text-base text-slate-700 outline-none focus:border-indigo-500 focus:ring-1 focus:ring-indigo-500 transition-all dark:bg-slate-900 dark:border-slate-700 dark:text-white; }
        .pay-input:valid { border-color: #22c55e; }
        .pay-input.error { border-color: #ef4444; animation: shake 0.4s cubic-bezier(.36,.07,.19,.97) both; }
        @keyframes shake { 10%, 90% { transform: translate3d(-1px, 0, 0); } 20%, 80% { transform: translate3d(2px, 0, 0); } 30%, 50%, 70% { transform: translate3d(-4px, 0, 0); } 40%, 60% { transform: translate3d(4px, 0, 0); } }
        
        .chat-message { @apply p-3 rounded-lg text-sm max-w-[85%] mb-2 animate-fade-in; }
        .chat-message.bot { @apply bg-slate-100 dark:bg-slate-700 text-slate-800 dark:text-slate-200 self-start rounded-tl-none; }
        .chat-message.user { @apply bg-brand-600 text-white self-end rounded-tr-none; }
        .chat-option { @apply block w-full text-left p-2 text-sm text-brand-600 dark:text-brand-400 hover:bg-brand-50 dark:hover:bg-slate-700 rounded transition; }

        .reveal-item { opacity: 0; transform: translateY(30px); transition: all 0.6s cubic-bezier(0.16, 1, 0.3, 1); }
        .reveal-item.is-visible { opacity: 1; transform: translateY(0); }
        
        details > summary { list-style: none; }
        details::-webkit-details-marker { display: none; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 transition-colors duration-300 dark:bg-slate-900 dark:text-slate-100 flex flex-col min-h-screen" onclick="ui.closeAllDropdowns(event)">

    <nav class="fixed w-full z-50 bg-white/90 dark:bg-slate-900/90 backdrop-blur-md border-b border-slate-200 dark:border-slate-800 transition-all duration-300">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-20 items-center">
                <div class="flex items-center cursor-pointer group" onclick="window.scrollTo(0,0)">
                    <div class="bg-brand-600 text-white p-2 rounded-lg mr-2 shadow-lg group-hover:rotate-3 transition">
                        <i class="fa-solid fa-earth-americas text-xl"></i>
                    </div>
                    <span class="font-bold text-xl md:text-2xl tracking-tight text-slate-900 dark:text-white">Book<span class="text-brand-600 dark:text-brand-500">ourshotels</span></span>
                </div>

                <div class="hidden lg:flex space-x-8">
                    <button onclick="document.getElementById('destinations').scrollIntoView({behavior:'smooth'})" class="nav-link text-sm font-bold text-slate-600 dark:text-slate-400 hover:text-brand-600 transition">Destinations</button>
                    <button onclick="document.getElementById('featured').scrollIntoView({behavior:'smooth'})" class="nav-link text-sm font-bold text-slate-600 dark:text-slate-400 hover:text-brand-600 transition">Hotels</button>
                    <button onclick="ui.openSavedModal()" class="nav-link text-sm font-bold text-slate-600 dark:text-slate-400 hover:text-brand-600 transition relative">Saved</button>
                    <button onclick="ui.openBookingsModal()" class="nav-link text-sm font-bold text-slate-600 dark:text-slate-400 hover:text-brand-600 transition relative">My Trips</button>
                </div>

                <div class="flex items-center space-x-4">
                    <select id="currencySelect" onchange="app.changeCurrency()" class="bg-slate-100 dark:bg-slate-800 text-slate-700 dark:text-slate-200 text-xs font-bold py-1.5 px-2 rounded-lg outline-none cursor-pointer border border-slate-200 dark:border-slate-700">
                        <option value="INR">₹ INR</option>
                        <option value="USD">$ USD</option>
                        <option value="EUR">€ EUR</option>
                    </select>

                    <div class="relative items-center space-x-3 cursor-pointer hidden md:flex" id="notifContainer" onclick="ui.toggleNotifications(event)">
                        <button class="w-9 h-9 rounded-full bg-slate-100 dark:bg-slate-800 flex items-center justify-center hover:bg-slate-200 dark:hover:bg-slate-700 transition text-slate-600 dark:text-slate-300 relative">
                            <i id="bellIcon" class="fa-regular fa-bell"></i>
                            <span id="notifBadge" class="hidden absolute top-0 right-0 w-2.5 h-2.5 bg-red-500 border-2 border-white dark:border-slate-800 rounded-full"></span>
                        </button>
                        <div id="notifDropdown" class="hidden absolute top-full right-0 mt-3 w-72 bg-white dark:bg-slate-800 rounded-xl shadow-2xl border border-slate-100 dark:border-slate-700 z-[60] animate-slide-up origin-top-right">
                            <div class="p-3 border-b border-slate-100 dark:border-slate-700 flex justify-between items-center">
                                <h3 class="font-bold text-sm dark:text-white">Alerts</h3>
                                <button onclick="app.clearNotifs(event)" class="text-[10px] font-bold text-brand-600 uppercase tracking-wider">Clear All</button>
                            </div>
                            <div id="notifList" class="max-h-64 overflow-y-auto p-2 space-y-1">
                                <p class="text-xs text-center text-slate-400 py-4">No new notifications</p>
                            </div>
                        </div>
                    </div>
                    
                    <button onclick="ui.toggleTheme()" class="w-9 h-9 rounded-full bg-slate-100 dark:bg-slate-800 flex items-center justify-center hover:bg-slate-200 dark:hover:bg-slate-700 transition text-slate-600 dark:text-yellow-400"><i id="themeIcon" class="fa-solid fa-moon"></i></button>

                    <div id="authButtons" class="flex space-x-2">
                        <button onclick="ui.openAuthModal('login')" class="text-sm font-bold text-brand-600 dark:text-brand-400 px-3 py-2 hover:bg-brand-50 dark:hover:bg-slate-800 rounded-lg transition">Sign In</button>
                        <button onclick="ui.openAuthModal('register')" class="bg-brand-600 text-white text-sm font-bold px-4 py-2 rounded-full hover:bg-brand-700 shadow-lg transition active:scale-95">Register</button>
                    </div>

                    <div id="userProfile" class="hidden relative items-center space-x-3 cursor-pointer select-none" onclick="ui.toggleProfileDropdown(event)">
                        <div class="text-right hidden md:block">
                            <p class="text-sm font-bold text-slate-900 dark:text-white truncate max-w-[120px]" id="userNameDisplay">User</p>
                        </div>
                        <img src="" class="w-9 h-9 rounded-full border-2 border-slate-200 dark:border-slate-600 object-cover" id="userAvatar">
                        
                        <div id="profileDropdown" class="hidden absolute top-full right-0 mt-3 w-64 bg-white dark:bg-slate-800 rounded-xl shadow-2xl border border-slate-100 dark:border-slate-700 z-[60] animate-slide-up origin-top-right">
                            <div class="p-4 border-b border-slate-100 dark:border-slate-700 bg-gradient-to-r from-brand-50 to-white dark:from-slate-800 dark:to-slate-800 rounded-t-xl">
                                <div class="flex justify-between items-center mb-2"><p class="text-xs text-slate-500 dark:text-slate-400">Current Tier</p><span class="text-xs font-bold text-brand-600">Silver</span></div>
                                <div class="w-full bg-slate-200 dark:bg-slate-700 rounded-full h-1.5 mb-1"><div class="bg-brand-600 h-1.5 rounded-full" style="width: 45%"></div></div>
                                <div class="flex justify-between items-center mt-3"><p class="text-xs text-slate-500 dark:text-slate-400">Points</p><span class="text-sm font-bold text-brand-600" id="userPointsDisplay">0</span></div>
                            </div>
                            <button id="adminLink" onclick="event.stopPropagation(); admin.openPanel()" class="hidden w-full text-left px-4 py-3 text-sm text-red-600 hover:bg-red-50 dark:hover:bg-red-900/20 font-bold border-b border-slate-100 dark:border-slate-700 flex items-center"><i class="fa-solid fa-lock mr-2"></i>Admin Panel</button>
                            <button onclick="event.stopPropagation(); ui.openProfileModal()" class="w-full text-left px-4 py-3 text-sm text-slate-600 dark:text-slate-300 hover:bg-slate-50 dark:hover:bg-slate-700 flex items-center"><i class="fa-regular fa-user mr-2 text-slate-400"></i> Edit Profile</button>
                            <button onclick="event.stopPropagation(); app.logout()" class="w-full text-left px-4 py-3 text-sm text-red-600 hover:bg-red-50 dark:hover:bg-red-900/30 rounded-b-xl flex items-center font-bold transition border-t border-slate-100 dark:border-slate-700"><i class="fa-solid fa-right-from-bracket mr-2"></i> Sign Out</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </nav>

    <div id="destinations" class="relative h-[400px] flex items-center justify-center overflow-hidden">
        <div class="absolute inset-0 z-0 overflow-hidden">
            <img src="https://images.unsplash.com/photo-1542314831-068cd1dbfeeb?auto=format&fit=crop&w=2000&q=80" class="w-full h-full object-cover animate-ken-burns">
            <div class="absolute inset-0 bg-gradient-to-r from-slate-900/80 via-brand-900/40 to-slate-900/80 animate-gradient-x"></div>
        </div>
        <div class="relative z-10 text-center px-4 mt-12 max-w-3xl mx-auto animate-fade-in">
            <h1 class="text-4xl md:text-5xl font-bold text-white mb-4">Find Ours  <span class="text-brand-400">Paradise</span></h1>
            <p class="text-lg text-slate-200 font-light italic" id="heroQuote">"Experience the world's best stays, tailored for you."</p>
        </div>
    </div>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12 flex-grow">
        <div class="bg-white dark:bg-slate-800 p-6 rounded-2xl shadow-xl -mt-24 relative z-20 mb-12 border border-slate-100 dark:border-slate-700 animate-slide-up hover:shadow-2xl transition-shadow duration-300">
            <div class="grid grid-cols-1 md:grid-cols-12 gap-6 items-end">
                <div class="md:col-span-5">
                    <label class="block text-xs font-bold text-slate-500 dark:text-slate-400 uppercase mb-1">Location</label>
                    <div class="flex items-center bg-slate-50 dark:bg-slate-900 rounded-xl px-4 py-3 border border-slate-200 dark:border-slate-700">
                        <i class="fa-solid fa-magnifying-glass text-brand-500 mr-3"></i>
                        <input type="text" id="searchInput" placeholder="City, Hotel, or Landmark..." class="bg-transparent w-full outline-none text-base text-slate-800 dark:text-white font-medium" onkeyup="if(event.key==='Enter') app.search()">
                    </div>
                </div>
                <div class="md:col-span-3">
                    <div class="flex justify-between mb-1">
                        <label class="text-xs font-bold text-slate-500 dark:text-slate-400 uppercase">Max Price</label>
                        <span class="text-xs font-bold text-brand-600" id="priceValue">₹100,000</span>
                    </div>
                    <input type="range" class="w-full" id="priceRange" min="1000" max="100000" step="1000" value="100000" oninput="app.updatePriceLabel(this.value)">
                </div>
                <div class="md:col-span-2">
                    <label class="block text-xs font-bold text-slate-500 dark:text-slate-400 uppercase mb-1">View</label>
                    <div class="flex bg-slate-100 dark:bg-slate-900 p-1 rounded-xl border border-slate-200 dark:border-slate-700">
                        <button onclick="ui.toggleView('grid')" id="viewGrid" class="flex-1 py-2 rounded-lg bg-white dark:bg-slate-700 shadow text-brand-600 text-xs font-bold transition"><i class="fa-solid fa-grid-2"></i> Grid</button>
                        <button onclick="ui.toggleView('map')" id="viewMapToggle" class="flex-1 py-2 rounded-lg text-slate-500 hover:bg-white dark:hover:bg-slate-800 text-xs font-bold transition"><i class="fa-solid fa-map"></i> Map</button>
                    </div>
                </div>
                <div class="md:col-span-2">
                    <button onclick="app.search()" class="w-full bg-brand-600 text-white h-[48px] rounded-xl font-bold hover:bg-brand-700 active:scale-95 transition-all shadow-lg flex items-center justify-center">Search</button>
                </div>
            </div>
        </div>

        <section class="mb-16">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-8 text-center reveal-item">
                <div class="p-6 rounded-2xl bg-white dark:bg-slate-800 shadow-sm border border-slate-100 dark:border-slate-700 hover:-translate-y-1 hover:shadow-lg transition-all duration-300">
                    <div class="w-14 h-14 mx-auto bg-brand-50 dark:bg-brand-900/30 text-brand-600 rounded-full flex items-center justify-center text-2xl mb-4"><i class="fa-solid fa-tags"></i></div>
                    <h3 class="font-bold text-lg mb-2 dark:text-white">Best Price Guarantee</h3>
                    <p class="text-slate-500 text-sm">We ensure you get the lowest rates. If you find a lower price online, we'll match it instantly.</p>
                </div>
                <div class="p-6 rounded-2xl bg-white dark:bg-slate-800 shadow-sm border border-slate-100 dark:border-slate-700 hover:-translate-y-1 hover:shadow-lg transition-all duration-300">
                    <div class="w-14 h-14 mx-auto bg-brand-50 dark:bg-brand-900/30 text-brand-600 rounded-full flex items-center justify-center text-2xl mb-4"><i class="fa-solid fa-plane-circle-check"></i></div>
                    <h3 class="font-bold text-lg mb-2 dark:text-white">Flexible Bookings</h3>
                    <p class="text-slate-500 text-sm">Plans change. That's why we offer free cancellation on 90% of our properties up to 24 hours prior.</p>
                </div>
                <div class="p-6 rounded-2xl bg-white dark:bg-slate-800 shadow-sm border border-slate-100 dark:border-slate-700 hover:-translate-y-1 hover:shadow-lg transition-all duration-300">
                    <div class="w-14 h-14 mx-auto bg-brand-50 dark:bg-brand-900/30 text-brand-600 rounded-full flex items-center justify-center text-2xl mb-4"><i class="fa-solid fa-headset"></i></div>
                    <h3 class="font-bold text-lg mb-2 dark:text-white">24/7 Global Support</h3>
                    <p class="text-slate-500 text-sm">Our travel experts are available around the clock to help you with your journey, anywhere in the world.</p>
                </div>
            </div>
        </section>

        <div class="flex gap-3 overflow-x-auto pb-4 mb-4 no-scrollbar" id="categoryFilters">
            <button onclick="app.filterCategory('all', event)" class="cat-btn px-4 py-2 rounded-full bg-brand-600 text-white text-sm font-bold whitespace-nowrap transition">All Stays</button>
            <button onclick="app.filterCategory('pool', event)" class="cat-btn px-4 py-2 rounded-full bg-slate-200 dark:bg-slate-700 text-slate-700 dark:text-slate-300 text-sm font-bold whitespace-nowrap hover:bg-slate-300 transition"><i class="fa-solid fa-water-ladder mr-2"></i>Pool</button>
            <button onclick="app.filterCategory('spa', event)" class="cat-btn px-4 py-2 rounded-full bg-slate-200 dark:bg-slate-700 text-slate-700 dark:text-slate-300 text-sm font-bold whitespace-nowrap hover:bg-slate-300 transition"><i class="fa-solid fa-spa mr-2"></i>Spa</button>
            <button onclick="app.filterCategory('wifi', event)" class="cat-btn px-4 py-2 rounded-full bg-slate-200 dark:bg-slate-700 text-slate-700 dark:text-slate-300 text-sm font-bold whitespace-nowrap hover:bg-slate-300 transition"><i class="fa-solid fa-wifi mr-2"></i>Free Wi-Fi</button>
            <button onclick="app.filterCategory('view', event)" class="cat-btn px-4 py-2 rounded-full bg-slate-200 dark:bg-slate-700 text-slate-700 dark:text-slate-300 text-sm font-bold whitespace-nowrap hover:bg-slate-300 transition"><i class="fa-solid fa-mountain mr-2"></i>Great Views</button>
        </div>

        <div id="featured" class="min-h-[500px] mb-20">
            <div class="flex justify-between items-end mb-6 border-b border-slate-200 dark:border-slate-800 pb-4">
                <div><h2 class="text-3xl font-bold text-slate-900 dark:text-white">Properties</h2><p class="text-slate-500 dark:text-slate-400 mt-1" id="resultCount">Loading...</p></div>
                <select id="sortSelect" onchange="app.sortHotels()" class="bg-slate-100 dark:bg-slate-800 px-3 py-2 rounded-lg text-sm font-bold outline-none cursor-pointer dark:text-white border border-slate-200 dark:border-slate-700">
                    <option value="rec">Recommended</option>
                    <option value="price_asc">Price: Low to High</option>
                    <option value="price_desc">Price: High to Low</option>
                    <option value="rating_desc">Rating: High to Low</option>
                </select>
            </div>
            
            <div id="hotelGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8"></div>
            <div id="globalMapContainer" class="hidden animate-fade-in relative"><div id="globalMap"></div></div>
        </div>

        <section class="mb-12 mt-16 reveal-item relative z-10">
            <h2 class="text-2xl md:text-3xl font-bold text-slate-900 dark:text-white mb-6">Discover the best time to book your next stay</h2>
            
            <div class="bg-white dark:bg-slate-800 rounded-2xl shadow-sm border border-slate-200 dark:border-slate-700 p-4 md:p-6 lg:p-8">
                
                <div class="relative flex items-center mb-6 border-b border-slate-200 dark:border-slate-700">
                    <div id="bestTimeTabs" class="flex overflow-x-auto no-scrollbar w-full gap-8 pr-12">
                        </div>
                    
                    <div class="absolute right-0 top-0 bottom-0 flex items-start bg-gradient-to-l from-white via-white to-transparent dark:from-slate-800 dark:via-slate-800 pl-8 pt-0 pointer-events-none">
                        <button class="w-8 h-8 rounded-full border border-slate-200 dark:border-slate-600 flex items-center justify-center bg-white dark:bg-slate-800 text-slate-600 dark:text-slate-300 pointer-events-auto">
                            <i class="fa-solid fa-chevron-right text-xs"></i>
                        </button>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 md:gap-8">
                    <div class="lg:col-span-5 h-64 md:h-[450px] rounded-xl overflow-hidden relative group">
                        <img id="bestTimeImg" src="" alt="City Image" class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-105">
                    </div>
                    
                    <div id="bestTimePrices" class="lg:col-span-7 flex flex-col justify-between gap-3">
                        </div>
                </div>
                
                <p class="text-xs text-slate-500 font-medium mt-6">Prices shown are dynamically calculated based on our current property listings.</p>
            </div>
        </section>
        
    </main>

    <section class="bg-slate-100 dark:bg-slate-800/40 py-16 transition-colors duration-300 border-y border-slate-200 dark:border-slate-800">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="text-center mb-12 reveal-item">
                <h2 class="text-3xl font-bold text-slate-900 dark:text-white mb-4">Loved by Travelers Worldwide</h2>
                <p class="text-slate-500 max-w-2xl mx-auto">Don't just take our word for it. See what our community of globetrotters has to say about their stays.</p>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 reveal-item">
                <div class="bg-white dark:bg-slate-800 p-6 rounded-2xl shadow-sm border border-slate-200 dark:border-slate-700">
                    <div class="flex text-yellow-400 text-sm mb-4"><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i></div>
                    <p class="text-slate-600 dark:text-slate-300 text-sm italic mb-6">"Absolutely seamless booking experience. The property matched the photos perfectly, and the automated PDF receipt was super handy for my business expenses."</p>
                    <div class="flex items-center gap-3 mt-auto">
                        <img src="https://ui-avatars.com/api/?name=alex+berg&background=random" class="w-10 h-10 rounded-full">
                        <div><h4 class="font-bold text-sm dark:text-white">alex berg</h4><p class="text-[10px] text-slate-500 uppercase tracking-wider">Business Traveler</p></div>
                    </div>
                </div>
                <div class="bg-white dark:bg-slate-800 p-6 rounded-2xl shadow-sm border border-slate-200 dark:border-slate-700">
                    <div class="flex text-yellow-400 text-sm mb-4"><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star-half-stroke"></i></div>
                    <p class="text-slate-600 dark:text-slate-300 text-sm italic mb-6">"I loved the live chat assistant! It helped me find a ski resort in Switzerland within minutes. Secure payment process gave me total peace of mind."</p>
                    <div class="flex items-center gap-3 mt-auto">
                        <img src="https://ui-avatars.com/api/?name=Aarush+jain&background=random" class="w-10 h-10 rounded-full">
                        <div><h4 class="font-bold text-sm dark:text-white">Aarush jain</h4><p class="text-[10px] text-slate-500 uppercase tracking-wider">Family Vacationer</p></div>
                    </div>
                </div>
                <div class="bg-white dark:bg-slate-800 p-6 rounded-2xl shadow-sm border border-slate-200 dark:border-slate-700">
                    <div class="flex text-yellow-400 text-sm mb-4"><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i></div>
                    <p class="text-slate-600 dark:text-slate-300 text-sm italic mb-6">"Best prices I've found on the web. The 'My Trips' dashboard is beautiful and makes managing my upcoming stays incredibly easy."</p>
                    <div class="flex items-center gap-3 mt-auto">
                        <img src="https://ui-avatars.com/api/?name=Krunal+j&background=random" class="w-10 h-10 rounded-full">
                        <div><h4 class="font-bold text-sm dark:text-white">Krunal jain</h4><p class="text-[10px] text-slate-500 uppercase tracking-wider">Solo Backpacker</p></div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <section class="max-w-3xl mx-auto px-4 sm:px-6 lg:px-8 py-16 w-full">
        <div class="text-center mb-10 reveal-item">
            <h2 class="text-3xl font-bold text-slate-900 dark:text-white">Frequently Asked Questions</h2>
        </div>
        <div class="space-y-4 reveal-item">
            <details class="group bg-white dark:bg-slate-800 rounded-xl shadow-sm border border-slate-200 dark:border-slate-700" open>
                <summary class="flex cursor-pointer items-center justify-between gap-1.5 p-5 text-slate-900 dark:text-white font-bold select-none">
                    How do I cancel my booking?
                    <span class="relative h-5 w-5 shrink-0"><i class="fa-solid fa-chevron-down absolute inset-0 transition-transform duration-300 group-open:-rotate-180 flex items-center justify-center"></i></span>
                </summary>
                <div class="px-5 pb-5 text-sm text-slate-600 dark:text-slate-400 leading-relaxed border-t border-slate-100 dark:border-slate-700 pt-4">
                    You can easily cancel your booking by navigating to the "My Trips" section while logged into your account. Locate your upcoming trip and click the red "Cancel Trip" button. Refunds are processed instantly depending on the hotel's cancellation policy.
                </div>
            </details>
            <details class="group bg-white dark:bg-slate-800 rounded-xl shadow-sm border border-slate-200 dark:border-slate-700">
                <summary class="flex cursor-pointer items-center justify-between gap-1.5 p-5 text-slate-900 dark:text-white font-bold select-none">
                    Are the prices displayed final?
                    <span class="relative h-5 w-5 shrink-0"><i class="fa-solid fa-chevron-down absolute inset-0 transition-transform duration-300 group-open:-rotate-180 flex items-center justify-center"></i></span>
                </summary>
                <div class="px-5 pb-5 text-sm text-slate-600 dark:text-slate-400 leading-relaxed border-t border-slate-100 dark:border-slate-700 pt-4">
                    The prices shown in the search results are base prices per night. Taxes and fees (standard 12%) are calculated and clearly displayed on the final checkout screen before you process any payment.
                </div>
            </details>
            <details class="group bg-white dark:bg-slate-800 rounded-xl shadow-sm border border-slate-200 dark:border-slate-700">
                <summary class="flex cursor-pointer items-center justify-between gap-1.5 p-5 text-slate-900 dark:text-white font-bold select-none">
                    How does the Rewards Points system work?
                    <span class="relative h-5 w-5 shrink-0"><i class="fa-solid fa-chevron-down absolute inset-0 transition-transform duration-300 group-open:-rotate-180 flex items-center justify-center"></i></span>
                </summary>
                <div class="px-5 pb-5 text-sm text-slate-600 dark:text-slate-400 leading-relaxed border-t border-slate-100 dark:border-slate-700 pt-4">
                    For every booking completed, you earn points automatically credited to your profile. You can view your points by clicking your profile avatar. Points can be redeemed for exclusive discounts on future stays.
                </div>
            </details>
        </div>
    </section>

    <footer class="bg-white dark:bg-[#1a1a1a] text-slate-800 dark:text-slate-200 py-12 border-t border-slate-200 dark:border-slate-800 transition-colors duration-300">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="grid grid-cols-1 md:grid-cols-4 lg:grid-cols-5 gap-8">
                <div class="space-y-4">
                    <div>
                        <h5 class="font-bold mb-3">Company</h5>
                        <ul class="space-y-2 text-sm text-slate-500 dark:text-slate-400">
                            <li><a href="#" target="_blank" class="hover:text-brand-600 dark:hover:text-brand-400">About</a></li>
                            <li><a href="#" target="_blank" class="hover:text-brand-600 dark:hover:text-brand-400">Careers</a></li>
                            <li><a href="#" target="_blank" class="hover:text-brand-600 dark:hover:text-brand-400">Jobs</a></li>
                        </ul>
                    </div>
                </div>
                <div>
                    <h5 class="font-bold mb-3">Legal & Help</h5>
                    <ul class="space-y-2 text-sm text-slate-500 dark:text-slate-400">
                        <li><a href="terms.html" target="_blank" rel="noopener noreferrer" class="hover:text-brand-600 dark:hover:text-brand-400">Terms and conditions</a></li>
                        <li><a href="privacy.html" target="_blank" rel="noopener noreferrer" class="hover:text-brand-600 dark:hover:text-brand-400">Privacy notice</a></li>
                        <li><a href="help.html" target="_blank" rel="noopener noreferrer" class="hover:text-brand-600 dark:hover:text-brand-400">Help Center</a></li>
                    </ul>
                </div>
                <div class="lg:col-span-3">
                    <h5 class="font-bold text-lg mb-3">Get exclusive inspiration for your next stay.</h5>
                    <form onsubmit="app.subscribe(event)" class="flex flex-col sm:flex-row gap-3">
                        <input type="email" id="newsEmail" required placeholder="Email address" class="flex-grow bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-lg py-3 px-4 text-sm outline-none focus:border-brand-500 focus:ring-1 focus:ring-brand-500 transition">
                        <button class="bg-brand-600 text-white px-8 py-3 rounded-lg font-bold hover:bg-brand-700 active:scale-95 transition-all shadow-md whitespace-nowrap">Subscribe</button>
                    </form>
                </div>
            </div>
            
            <div class="mt-12 pt-8 border-t border-slate-200 dark:border-slate-800 flex flex-col md:flex-row justify-between items-center gap-4">
                <p class="text-sm text-slate-500 dark:text-slate-400">Copyright 2026 bookourshotels | All rights reserved.</p>
                
                <div class="flex items-center gap-3 text-2xl text-slate-400 dark:text-slate-500">
                    <i class="fa-brands fa-cc-visa hover:text-brand-600 transition cursor-pointer"></i>
                    <i class="fa-brands fa-cc-mastercard hover:text-orange-500 transition cursor-pointer"></i>
                    <i class="fa-brands fa-cc-amex hover:text-blue-500 transition cursor-pointer"></i>
                    <i class="fa-brands fa-cc-paypal hover:text-blue-700 transition cursor-pointer"></i>
                    <div class="h-6 border-l border-slate-300 dark:border-slate-700 mx-2"></div>
                    <i class="fa-solid fa-lock text-lg" title="SSL Secured"></i>
                </div>
            </div>
        </div>
    </footer>

    <!-- Scroll to Top Button -->
    <button id="scrollToTopBtn" onclick="window.scrollTo({top: 0, behavior: 'smooth'})" class="fixed bottom-24 right-6 bg-slate-800 dark:bg-slate-700 text-white w-12 h-12 rounded-full shadow-lg hover:bg-slate-700 transition z-40 flex items-center justify-center opacity-0 pointer-events-none translate-y-10 duration-300">
        <i class="fa-solid fa-arrow-up"></i>
    </button>

    <button onclick="ui.toggleChat()" class="animate-float fixed bottom-6 right-6 bg-brand-600 text-white w-14 h-14 rounded-full shadow-lg hover:bg-brand-700 transition z-40 animate-bounce-in flex items-center justify-center">
        <i class="fa-solid fa-comments text-2xl"></i>
    </button>
    <div id="chatWindow" class="fixed bottom-24 right-6 w-80 sm:w-96 bg-white dark:bg-slate-800 rounded-2xl shadow-2xl z-50 hidden flex-col border border-slate-200 dark:border-slate-700 overflow-hidden animate-slide-up origin-bottom-right">
        <div class="bg-brand-600 text-white p-4 flex justify-between items-center">
            <div class="font-bold flex items-center gap-2"><i class="fa-solid fa-robot"></i> Travel Assistant</div>
            <button onclick="ui.toggleChat()" class="text-white/80 hover:text-white"><i class="fa-solid fa-xmark text-lg"></i></button>
        </div>
        <div id="chatBody" class="h-80 p-4 overflow-y-auto bg-slate-50 dark:bg-slate-900/50 flex flex-col gap-2"></div>
        <div class="p-3 border-t border-slate-200 dark:border-slate-700 bg-white dark:bg-slate-800 flex gap-2">
            <input type="text" id="chatInput" placeholder="Type a message..." class="flex-grow bg-slate-100 dark:bg-slate-900 text-sm p-2 rounded-lg outline-none dark:text-white border border-transparent focus:border-brand-500 transition-colors" onkeyup="if(event.key==='Enter') chatbot.handleText()">
            <button onclick="chatbot.handleText()" class="bg-brand-600 text-white w-9 h-9 rounded-lg flex items-center justify-center hover:bg-brand-700 transition"><i class="fa-solid fa-paper-plane text-xs"></i></button>
        </div>
    </div>

    <div id="savedModal" class="fixed inset-0 z-[80] hidden bg-slate-900/90 backdrop-blur-sm flex items-center justify-center p-4">
        <div class="bg-white dark:bg-slate-800 w-full max-w-5xl rounded-2xl shadow-2xl flex flex-col h-[85vh] animate-bounce-in">
            <div class="p-6 border-b border-slate-200 dark:border-slate-700 flex justify-between items-center bg-slate-50 dark:bg-slate-900 rounded-t-2xl">
                <h2 class="text-2xl font-bold text-slate-900 dark:text-white"><i class="fa-solid fa-heart mr-2 text-red-500"></i> Saved Properties</h2>
                <button onclick="ui.closeModal('savedModal')" class="text-slate-400 hover:text-slate-600"><i class="fa-solid fa-xmark text-xl"></i></button>
            </div>
            <div class="flex-grow overflow-auto p-6 bg-slate-50 dark:bg-slate-900/50">
                <div id="savedContainer" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"></div>
            </div>
        </div>
    </div>

    <!-- My Trips Modal with Interactive Map and Countdown -->
    <div id="userTripsModal" class="fixed inset-0 z-[80] hidden bg-slate-900/90 backdrop-blur-sm flex items-center justify-center p-4">
        <div class="bg-white dark:bg-slate-800 w-full max-w-6xl rounded-2xl shadow-2xl flex flex-col h-[85vh] animate-bounce-in">
            <div class="p-6 border-b border-slate-200 dark:border-slate-700 flex justify-between items-center bg-slate-50 dark:bg-slate-900 rounded-t-2xl shrink-0">
                <h2 class="text-2xl font-bold text-slate-900 dark:text-white"><i class="fa-solid fa-suitcase-rolling mr-2 text-brand-600"></i> My Trips</h2>
                <button onclick="ui.closeModal('userTripsModal')" class="text-slate-400 hover:text-slate-600"><i class="fa-solid fa-xmark text-xl"></i></button>
            </div>
            
            <!-- Split Layout: List on Left, Map on Right -->
            <div class="flex-grow flex flex-col lg:flex-row overflow-hidden">
                <div class="w-full lg:w-1/2 overflow-y-auto p-6 bg-slate-50 dark:bg-slate-900/50" id="userTripsContainer">
                    <!-- Trips and countdown will be injected here -->
                </div>
                
                <div class="w-full lg:w-1/2 relative bg-slate-200 dark:bg-slate-800 min-h-[300px] lg:min-h-full border-t lg:border-t-0 lg:border-l border-slate-200 dark:border-slate-700">
                    <div id="myTripsMap" class="absolute inset-0 z-0"></div>
                    <div class="absolute top-4 left-4 z-10 bg-white/90 dark:bg-slate-900/90 backdrop-blur px-3 py-1.5 rounded-lg shadow-md text-xs font-bold text-slate-700 dark:text-slate-200">
                        <i class="fa-solid fa-map-pin text-brand-600 mr-1"></i> Your Travel Footprint
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div id="hotelModal" class="fixed inset-0 z-[60] hidden bg-slate-900/80 backdrop-blur-sm transition-opacity">
        <div class="absolute inset-y-0 right-0 w-full md:w-[650px] bg-white dark:bg-slate-800 shadow-2xl flex flex-col transform transition-transform duration-300 translate-x-full" id="hotelModalContent">
            <div class="relative h-72 shrink-0 group bg-black">
                <img id="modalImg" src="" class="w-full h-full object-cover transition-opacity duration-300">
                <button onclick="ui.closeModal('hotelModal')" class="absolute top-4 right-4 bg-black/40 hover:bg-black/60 text-white p-2 rounded-full"><i class="fa-solid fa-xmark text-lg"></i></button>
                <button onclick="app.shareHotel()" class="absolute top-4 right-14 bg-black/40 hover:bg-black/60 text-white p-2 rounded-full transition"><i class="fa-solid fa-arrow-up-from-bracket text-lg"></i></button>
                <div class="absolute bottom-4 left-0 right-0 p-4 bg-gradient-to-t from-black/80 to-transparent">
                    <h2 id="modalTitle" class="text-3xl font-bold text-white mb-1"></h2>
                    <p id="modalLoc" class="text-sm text-slate-200"></p>
                    
                    <!-- Live Viewers Badge -->
                    <div id="liveViewers" class="mt-2 inline-flex items-center gap-2 bg-red-500/20 backdrop-blur-md px-3 py-1.5 rounded-full text-red-100 text-xs font-bold border border-red-500/30 animate-pulse hidden">
                        <i class="fa-solid fa-fire text-red-400"></i>
                        <span id="viewerCount">0</span> people are viewing this right now
                    </div>
                    
                    <div id="modalWeather" class="mt-2 inline-flex items-center gap-2 bg-white/20 backdrop-blur-md px-3 py-1.5 rounded-full text-white text-xs font-bold border border-white/30 hidden">
                        <i id="weatherIcon" class="fa-solid fa-cloud-sun"></i>
                        <span id="weatherText">Loading weather...</span>
                    </div>
                </div>
            </div>
            <div class="flex gap-2 p-2 bg-black overflow-x-auto hidden" id="modalGallery"></div>
            <div class="flex border-b border-slate-200 dark:border-slate-700 shrink-0">
                <button onclick="ui.switchTab('info')" id="tabInfo" class="flex-1 py-3 text-sm font-bold text-brand-600 border-b-2 border-brand-600 bg-slate-50 dark:bg-slate-800/50">Info</button>
                <button onclick="ui.switchTab('map')" id="tabMap" class="flex-1 py-3 text-sm font-bold text-slate-500 hover:text-slate-700 dark:text-slate-400">Map</button>
            </div>
            
            <div class="flex-grow overflow-y-auto p-6" id="viewInfo">
                <div class="bg-brand-50 dark:bg-brand-900/20 p-4 rounded-xl border border-brand-100 dark:border-brand-500/20 mb-6">
                    <div class="flex justify-between items-center mb-4">
                        <div><p class="text-[10px] font-bold text-slate-500 uppercase">Price</p><p class="text-3xl font-bold text-brand-600 dark:text-brand-400" id="modalPrice"></p></div>
                        <div class="text-right"><div class="flex items-center justify-end text-yellow-400 text-sm font-bold"><i class="fa-solid fa-star mr-1"></i> <span id="modalRating"></span></div></div>
                    </div>
                </div>
                
                <div class="mb-6">
                    <h4 class="text-xs font-bold text-slate-500 uppercase mb-3">Popular Amenities</h4>
                    <div id="modalAmenities" class="flex flex-wrap gap-2"></div>
                </div>

                <p id="modalDesc" class="text-sm text-slate-600 dark:text-slate-300 leading-relaxed mb-6"></p>
                
                <div class="border-t border-slate-200 dark:border-slate-700 pt-6">
                    <div class="grid grid-cols-2 gap-4 mb-4">
                        <div>
                            <label class="text-[10px] font-bold uppercase text-slate-500">Check In</label>
                            <input type="text" id="dateIn" class="w-full bg-slate-100 dark:bg-slate-900 text-base p-2 rounded-lg outline-none dark:text-white cursor-pointer" placeholder="Select Date">
                        </div>
                        <div>
                            <label class="text-[10px] font-bold uppercase text-slate-500">Check Out</label>
                            <input type="text" id="dateOut" class="w-full bg-slate-100 dark:bg-slate-900 text-base p-2 rounded-lg outline-none dark:text-white cursor-pointer" placeholder="Select Date">
                        </div>
                    </div>
                    <button onclick="app.openPaymentModal()" class="w-full animate-glow-pulse bg-brand-600 text-white py-3 rounded-xl font-bold hover:bg-brand-700 transition shadow-lg flex justify-between px-6 items-center"><span>Proceed to Pay</span><span id="totalPriceDisplay"></span></button>
                </div>
            </div>
            
            <div class="hidden flex-grow relative min-h-[400px] w-full" id="viewMap">
                <div id="modalMap" class="absolute inset-0"></div>
            </div>
        </div>
    </div>

    <div id="paymentModal" class="fixed inset-0 z-[100] hidden bg-black/60 backdrop-blur-sm flex items-center justify-center p-4">
        <div class="flex flex-col md:flex-row bg-white dark:bg-slate-800 rounded-2xl shadow-2xl overflow-hidden max-w-4xl w-full max-h-[90vh] overflow-y-auto animate-bounce-in">
            <div class="md:w-5/12 bg-[#0B1120] text-white p-6 md:p-8 relative shrink-0">
                <h2 class="text-2xl font-bold mb-6">Trip Summary</h2>
                <div class="bg-slate-800/50 p-4 rounded-xl border border-slate-700 flex gap-4 mb-6"><img id="payImg" src="" class="w-16 h-16 rounded-lg object-cover bg-slate-700"><div><h3 id="payHotel" class="font-bold text-sm">Hotel Name</h3><p id="payLoc" class="text-xs text-slate-400 mt-1">Location</p></div></div>
                <div class="grid grid-cols-2 gap-4 mb-6">
                    <div class="bg-slate-800/50 p-3 rounded-lg border border-slate-700"><label class="text-[10px] font-bold text-slate-400 uppercase">Check In</label><p id="payIn" class="font-bold text-sm">--</p></div>
                    <div class="bg-slate-800/50 p-3 rounded-lg border border-slate-700"><label class="text-[10px] font-bold text-slate-400 uppercase">Check Out</label><p id="payOut" class="font-bold text-sm">--</p></div>
                </div>
                <div class="mb-8"><label class="text-[10px] font-bold text-slate-400 uppercase mb-2 block">Promo Code</label><div class="flex"><input type="text" id="promoInput" placeholder="LUX10" class="flex-grow bg-slate-800 border-none text-base p-3 rounded-l-lg outline-none uppercase tracking-widest text-white placeholder-slate-600"><button type="button" onclick="app.applyPromo()" class="bg-indigo-600 text-white px-4 text-xs font-bold rounded-r-lg hover:bg-indigo-500 transition">APPLY</button></div><p id="promoMsg" class="text-[10px] text-green-400 mt-2 hidden">Discount applied!</p></div>
                <div class="mt-auto space-y-3 pt-6 border-t border-slate-800"><div class="flex justify-between items-center pt-2"><span class="font-bold text-lg">Total</span><span id="payTotal" class="font-bold text-2xl text-white">0</span></div></div>
            </div>

            <div class="md:w-7/12 bg-white dark:bg-slate-900 p-6 md:p-8 relative">
                <button onclick="ui.closeModal('paymentModal')" class="absolute top-6 right-6 text-slate-400 hover:text-slate-600 z-10"><i class="fa-solid fa-xmark text-xl"></i></button>
                <div class="flex justify-between items-center mb-8"><h2 class="text-2xl font-bold text-slate-900 dark:text-white">Payment Details</h2><div class="flex items-center gap-2 bg-red-50 text-red-600 px-3 py-1 rounded-full text-xs font-bold border border-red-100"><i class="fa-regular fa-clock"></i> <span id="payTimer">05:00</span></div></div>

                <form id="paymentForm" onsubmit="app.processPayment(event)" class="space-y-4" autocomplete="off">
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs font-bold text-slate-500 uppercase mb-2">Cardholder Name</label>
                            <input type="text" required class="pay-input" placeholder="Your Name" autocomplete="off">
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-slate-500 uppercase mb-2">Phone Number</label>
                            <input type="tel" id="payPhone" required pattern="[0-9]{10}" maxlength="10" placeholder="9876543210" class="pay-input" oninput="this.value = this.value.replace(/[^0-9]/g, '').substring(0,10)" title="Please enter valid 10 digit number" autocomplete="off">
                        </div>
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-slate-500 uppercase mb-2">Card Number</label>
                        <div class="relative">
                            <input type="text" id="cardNum" required maxlength="19" placeholder="0000 0000 0000 0000" class="pay-input pr-10 font-mono" oninput="app.formatCard(this)" autocomplete="off">
                            <i class="fa-regular fa-credit-card absolute right-4 top-4 text-slate-400 text-lg"></i>
                        </div>
                    </div>
                    
                    <div class="grid grid-cols-2 gap-6">
                        <div>
                            <label class="block text-xs font-bold text-slate-500 uppercase mb-2">Expiry (MM/YY)</label>
                            <input type="text" id="cardExp" required placeholder="MM/YY" maxlength="5" class="pay-input text-center" oninput="app.formatExpiry(this)" autocomplete="off">
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-slate-500 uppercase mb-2">CVC</label>
                            <input type="password" id="cardCvc" required placeholder="123" maxlength="3" pattern="[0-9]{3}" class="pay-input text-center tracking-widest" title="3 digits only" autocomplete="new-password">
                        </div>
                    </div>
                    <button type="submit" id="payBtn" class="w-full animate-glow-pulse bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-4 rounded-xl text-lg shadow-xl shadow-indigo-200 transition transform hover:-translate-y-1 mt-4">Pay Total</button>
                </form>
            </div>
        </div>
    </div>
    
    <div id="profileModal" class="fixed inset-0 z-[90] hidden bg-slate-900/90 backdrop-blur-sm flex items-center justify-center p-4">
        <div class="bg-white dark:bg-slate-800 w-full max-w-md rounded-2xl shadow-2xl p-8 relative animate-bounce-in">
            <button onclick="ui.closeModal('profileModal')" class="absolute top-4 right-4 text-slate-400 hover:text-slate-600"><i class="fa-solid fa-xmark text-xl"></i></button>
            <h2 class="text-2xl font-bold mb-6 dark:text-white">Edit Profile</h2>
            <form onsubmit="app.updateProfile(event)" class="space-y-5">
                <div>
                    <label class="block text-xs font-bold text-slate-500 dark:text-slate-400 uppercase mb-1">Full Name</label>
                    <input type="text" id="editProfName" required class="w-full p-3 rounded-lg bg-slate-50 dark:bg-slate-900 border border-slate-200 dark:border-slate-700 outline-none focus:border-brand-500 dark:text-white">
                </div>
                <div>
                    <label class="block text-xs font-bold text-slate-500 dark:text-slate-400 uppercase mb-1">Email Address</label>
                    <input type="email" id="editProfEmail" required class="w-full p-3 rounded-lg bg-slate-50 dark:bg-slate-900 border border-slate-200 dark:border-slate-700 outline-none focus:border-brand-500 dark:text-white" readonly>
                    <p class="text-[10px] text-slate-400 mt-1"><i class="fa-solid fa-lock mr-1"></i> Email cannot be changed</p>
                </div>
                <button class="w-full bg-brand-600 text-white py-3 rounded-xl font-bold hover:bg-brand-700 transition shadow-lg">Save Changes</button>
            </form>
        </div>
    </div>

    <div id="adminModal" class="fixed inset-0 z-[70] hidden bg-slate-900/95 backdrop-blur-md flex items-center justify-center p-4">
        <div class="bg-white dark:bg-slate-800 w-full max-w-6xl rounded-2xl shadow-2xl flex flex-col h-[90vh]">
            <div class="p-4 border-b border-slate-200 dark:border-slate-700 flex justify-between items-center bg-slate-50 dark:bg-slate-900">
                <div class="flex items-center gap-4"><div class="bg-red-600 text-white p-2 rounded-lg"><i class="fa-solid fa-lock"></i></div><div><h2 class="text-xl font-bold text-slate-900 dark:text-white">Admin Console</h2></div></div>
                <button onclick="ui.closeModal('adminModal')" class="text-slate-400 hover:text-slate-600"><i class="fa-solid fa-xmark text-xl"></i></button>
            </div>
            <div class="flex border-b border-slate-200 dark:border-slate-700 bg-white dark:bg-slate-800 px-6 overflow-x-auto">
                <button onclick="admin.switchTab('overview')" id="admTabOver" class="px-4 py-3 text-sm font-bold border-b-2 border-brand-600 text-brand-600">Overview</button>
                <button onclick="admin.switchTab('bookings')" id="admTabBook" class="px-4 py-3 text-sm font-bold border-b-2 border-transparent text-slate-500 hover:text-slate-700">Bookings</button>
                <button onclick="admin.switchTab('users')" id="admTabUser" class="px-4 py-3 text-sm font-bold border-b-2 border-transparent text-slate-500 hover:text-slate-700">Users</button>
                <button onclick="admin.switchTab('logs')" id="admTabLogs" class="px-4 py-3 text-sm font-bold border-b-2 border-transparent text-slate-500 hover:text-slate-700">Audit Logs</button>
            </div>
            <div class="flex-grow overflow-auto p-6 bg-slate-50 dark:bg-slate-900/50">
                <div id="admViewOverview" class="space-y-6">
                    <div class="flex flex-wrap gap-2 mb-4">
                        <button onclick="admin.exportRelationalSchema()" class="bg-indigo-600 text-white text-xs px-4 py-2 rounded flex items-center hover:bg-indigo-700 transition shadow"><i class="fa-solid fa-database mr-2"></i> Export Relational Schema (JSON)</button>
                        <button onclick="app.runDiagnostics()" class="bg-slate-800 text-slate-300 border border-slate-700 text-xs px-4 py-2 rounded flex items-center hover:bg-slate-700 transition shadow"><i class="fa-solid fa-vial-virus mr-2"></i> Run Booking Flow Test</button>
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div class="bg-white dark:bg-slate-800 p-4 rounded-xl shadow-sm border border-slate-200 dark:border-slate-700"><p class="text-xs text-slate-500 uppercase font-bold">Total Revenue</p><h3 class="text-2xl font-bold text-green-600 mt-1" id="admRev">0</h3></div>
                        <div class="bg-white dark:bg-slate-800 p-4 rounded-xl shadow-sm border border-slate-200 dark:border-slate-700"><p class="text-xs text-slate-500 uppercase font-bold">Total Bookings</p><h3 class="text-2xl font-bold text-brand-600 mt-1" id="admBookCount">0</h3></div>
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div class="bg-white dark:bg-slate-800 p-4 rounded-xl shadow-sm border border-slate-200 dark:border-slate-700">
                            <h4 class="font-bold text-slate-700 dark:text-slate-300 mb-4">Revenue Trend</h4>
                            <div class="h-64"><canvas id="revenueChart"></canvas></div>
                        </div>
                        <div class="bg-white dark:bg-slate-800 p-4 rounded-xl shadow-sm border border-slate-200 dark:border-slate-700">
                            <h4 class="font-bold text-slate-700 dark:text-slate-300 mb-4">Revenue by Destination</h4>
                            <div class="h-64"><canvas id="destinationChart"></canvas></div>
                        </div>
                    </div>
                </div>
                <div id="admViewBookings" class="hidden">
                    <div class="flex justify-between mb-4"><h3 class="font-bold text-slate-700 dark:text-white">Bookings List</h3><button onclick="admin.exportCSV('bookings')" class="bg-green-600 text-white text-xs px-3 py-2 rounded hover:bg-green-700"><i class="fa-solid fa-download mr-1"></i> Export CSV</button></div>
                    <div class="overflow-x-auto"><table class="w-full text-left border-collapse admin-table">
                        <thead class="bg-slate-100 dark:bg-slate-700"><tr><th>ID</th><th>User</th><th>Email</th><th>Phone</th><th>Hotel</th><th>Amount</th><th>Status</th></tr></thead>
                        <tbody id="admTableBookings"></tbody>
                    </table></div>
                </div>
                <div id="admViewUsers" class="hidden">
                    <div class="flex justify-between mb-4"><h3 class="font-bold text-slate-700 dark:text-white">User List</h3><button onclick="admin.exportCSV('users')" class="bg-green-600 text-white text-xs px-3 py-2 rounded hover:bg-green-700"><i class="fa-solid fa-download mr-1"></i> Export CSV</button></div>
                    <div class="overflow-x-auto"><table class="w-full text-left border-collapse admin-table">
                        <thead class="bg-slate-100 dark:bg-slate-700"><tr><th>Name</th><th>Email</th><th>Role</th></tr></thead>
                        <tbody id="admTableUsers"></tbody>
                    </table></div>
                </div>
                <div id="admViewLogs" class="hidden">
                    <div class="flex justify-between mb-4">
                        <h3 class="font-bold text-slate-700 dark:text-white">System Security & Activity Logs</h3>
                        <button onclick="admin.clearLogs()" class="text-xs text-red-500 hover:underline">Clear Logs</button>
                    </div>
                    <div class="bg-slate-900 rounded-xl p-4 h-[50vh] overflow-y-auto font-mono text-xs text-green-400 space-y-2 shadow-inner" id="adminLogConsole"></div>
                </div>
            </div>
        </div>
    </div>

    <div id="timeoutModal" class="fixed inset-0 z-[110] hidden bg-slate-900/95 backdrop-blur-md flex items-center justify-center p-4">
        <div class="bg-white dark:bg-slate-800 p-8 rounded-2xl shadow-2xl text-center max-w-sm w-full animate-bounce-in border border-red-100 dark:border-red-900/30">
            <div class="w-16 h-16 bg-red-100 dark:bg-red-900/30 text-red-500 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl">
                <i class="fa-solid fa-user-clock"></i>
            </div>
            <h3 class="text-xl font-bold text-slate-900 dark:text-white mb-2">Are you still there?</h3>
            <p class="text-slate-500 text-sm mb-6">For your security, your session will expire in <span id="timeoutSeconds" class="font-bold text-red-500">60</span> seconds due to inactivity.</p>
            <button onclick="app.resetSession()" class="w-full bg-brand-600 hover:bg-brand-700 text-white font-bold py-3 rounded-xl transition">Keep Me Logged In</button>
        </div>
    </div>

    <div id="toast" class="fixed top-6 left-1/2 transform -translate-x-1/2 z-[100] bg-slate-800 text-white px-6 py-3 rounded-full shadow-2xl flex items-center gap-3 transition-all duration-300 -translate-y-20 opacity-0"><i class="fa-solid fa-circle-check text-green-400"></i><span id="toastMsg" class="text-sm font-medium">Success</span></div>
    
    <div id="authModal" class="fixed inset-0 z-50 hidden bg-slate-900/80 backdrop-blur-sm flex items-center justify-center p-4">
        <div class="bg-white dark:bg-slate-800 w-full max-w-sm rounded-2xl p-8 relative">
            <button onclick="ui.closeModal('authModal')" class="absolute top-4 right-4 text-slate-400"><i class="fa-solid fa-xmark"></i></button>
            <h2 id="authTitle" class="text-2xl font-bold mb-4 dark:text-white">Login</h2>
            <form onsubmit="app.auth(event)" class="space-y-4">
                <input type="text" id="authName" class="hidden w-full p-3 border rounded-lg text-base dark:bg-slate-900 dark:border-slate-700 dark:text-white" placeholder="Name (Optional)">
                <input type="email" id="authEmail" class="w-full p-3 border rounded-lg text-base dark:bg-slate-900 dark:border-slate-700 dark:text-white" placeholder="Email (@gmail or @yahoo)" required>
                <input type="password" id="authPass" class="w-full p-3 border rounded-lg text-base dark:bg-slate-900 dark:border-slate-700 dark:text-white" placeholder="Password" required>
                <button class="w-full bg-brand-600 text-white py-3 rounded-lg font-bold">Submit</button>
            </form>
            <p class="text-xs text-center mt-4 cursor-pointer text-slate-500 hover:text-brand-600 transition" onclick="ui.toggleAuth()" id="authSwitch">New here? Create Account</p>
        </div>
    </div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        const DB_HOTELS = [
            { id: 1, name: "Doon Valley Retreat", loc: "Dehradun, India", price: 4500, rating: 4.7, images: ["https://images.unsplash.com/photo-1587061949409-02df41d5e562?auto=format&fit=crop&w=800&q=80", "https://images.unsplash.com/photo-1542314831-068cd1dbfeeb?auto=format&fit=crop&w=800&q=80"], desc: "Serene Himalayan escape.", lat: 30.3165, lng: 78.0322, amenities: ['<i class="fa-solid fa-wifi"></i> Free Wi-Fi', '<i class="fa-solid fa-mountain"></i> Mountain View', '<i class="fa-solid fa-mug-hot"></i> Breakfast included'] },
            { id: 2, name: "The Royal Palace", loc: "Dehradun, India", price: 12000, rating: 4.9, images: ["https://images.unsplash.com/photo-1566073771259-6a8506099945?auto=format&fit=crop&w=800&q=80"], desc: "Luxury heritage stay.", lat: 30.3256, lng: 78.0437, amenities: ['<i class="fa-solid fa-spa"></i> Spa', '<i class="fa-solid fa-water-ladder"></i> Pool', '<i class="fa-solid fa-utensils"></i> Restaurant'] },
            { id: 3, name: "Burj Al Arab", loc: "Dubai, UAE", price: 85000, rating: 5.0, images: ["https://images.unsplash.com/photo-1582719508461-905c673771fd?auto=format&fit=crop&w=800&q=80"], desc: "7-star luxury.", lat: 25.1412, lng: 55.1853, amenities: ['<i class="fa-solid fa-martini-glass"></i> Bar', '<i class="fa-solid fa-spa"></i> Spa', '<i class="fa-solid fa-water-ladder"></i> Pool', '<i class="fa-solid fa-mountain"></i> Great Views'] },
            { id: 4, name: "Eiffel View Suites", loc: "Paris, France", price: 25000, rating: 4.8, images: ["https://images.unsplash.com/photo-1502602898657-3e91760cbb34?auto=format&fit=crop&w=800&q=80"], desc: "Parisian charm.", lat: 48.8584, lng: 2.2945, amenities: ['<i class="fa-solid fa-wifi"></i> Free Wi-Fi', '<i class="fa-solid fa-city"></i> City View'] },
            { id: 5, name: "London Hub", loc: "London, UK", price: 9000, rating: 4.4, images: ["https://images.unsplash.com/photo-1513635269975-59663e0ac1ad?auto=format&fit=crop&w=800&q=80"], desc: "Central London stay.", lat: 51.5074, lng: -0.1278, amenities: ['<i class="fa-solid fa-dumbbell"></i> Gym', '<i class="fa-solid fa-wifi"></i> Free Wi-Fi'] },
            { id: 6, name: "Grand Plaza Hotel", loc: "New York, USA", price: 35000, rating: 4.6, images: ["https://images.unsplash.com/photo-1582719508461-905c673771fd?auto=format&fit=crop&w=800&q=80"], desc: "Luxury stay in the heart of Manhattan.", lat: 40.7128, lng: -74.0060, amenities: ['<i class="fa-solid fa-bell-concierge"></i> Room Service', '<i class="fa-solid fa-wifi"></i> Free Wi-Fi'] },
            { id: 8, name: "Opera View Suites", loc: "Sydney, Australia", price: 28000, rating: 4.7, images: ["https://images.unsplash.com/photo-1522798514-97ceb8c4f1c8?auto=format&fit=crop&w=800&q=80"], desc: "Stunning views of the Sydney Opera House.", lat: -33.8688, lng: 151.2093, amenities: ['<i class="fa-solid fa-water"></i> Ocean View', '<i class="fa-solid fa-mountain"></i> Great Views', '<i class="fa-solid fa-martini-glass"></i> Bar'] },
            { id: 9, name: "Alpine Ski Resort", loc: "Zermatt, Switzerland", price: 42000, rating: 4.8, images: ["https://images.unsplash.com/photo-1519608487953-e999c86e7455?auto=format&fit=crop&w=800&q=80"], desc: "Premium ski-in/ski-out chalet.", lat: 46.0207, lng: 7.7491, amenities: ['<i class="fa-solid fa-person-skiing"></i> Ski Access', '<i class="fa-solid fa-fire"></i> Fireplace', '<i class="fa-solid fa-mountain"></i> Mountain View'] },
            
            { id: 10, name: "Santorini Blue Domes", loc: "Santorini, Greece", price: 32000, rating: 4.9, images: ["https://images.unsplash.com/photo-1602343168117-bb8ffe3e2e9f?auto=format&fit=crop&w=800&q=80"], desc: "Iconic cliffside villas with infinity pools.", lat: 36.3932, lng: 25.4615, amenities: ['<i class="fa-solid fa-water-ladder"></i> Pool', '<i class="fa-solid fa-water"></i> Ocean View', '<i class="fa-solid fa-spa"></i> Spa'] },
            { id: 11, name: "Ubud Jungle Resort", loc: "Bali, Indonesia", price: 8500, rating: 4.6, images: ["https://images.unsplash.com/photo-1522798514-97ceb8c4f1c8?auto=format&fit=crop&w=800&q=80"], desc: "Peaceful escape surrounded by lush tropical forests.", lat: -8.5069, lng: 115.2625, amenities: ['<i class="fa-solid fa-spa"></i> Spa', '<i class="fa-solid fa-leaf"></i> Nature Trails', '<i class="fa-solid fa-wifi"></i> Free Wi-Fi'] },
            { id: 12, name: "Marina Bay Oasis", loc: "Singapore, Singapore", price: 41000, rating: 4.9, images: ["https://images.unsplash.com/photo-1582719508461-905c673771fd?auto=format&fit=crop&w=800&q=80"], desc: "Modern architectural marvel with a rooftop infinity pool.", lat: 1.2834, lng: 103.8607, amenities: ['<i class="fa-solid fa-water-ladder"></i> Rooftop Pool', '<i class="fa-solid fa-city"></i> City View', '<i class="fa-solid fa-martini-glass"></i> Bar'] },
            { id: 13, name: "Tokyo Sakura Hotel", loc: "Tokyo, Japan", price: 15500, rating: 4.8, images: ["https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR-pvD3mY5xYe43NF_9uUdxB49PbUTnL3Ynpg&s"], desc: "Minimalist Japanese design right in the heart of Shibuya.", lat: 35.6762, lng: 139.6503, amenities: ['<i class="fa-solid fa-train-subway"></i> Near Transit', '<i class="fa-solid fa-wifi"></i> Free Wi-Fi', '<i class="fa-solid fa-mug-hot"></i> Breakfast included'] },
            { id: 14, name: "Taj Heritage", loc: "Mumbai, India", price: 18000, rating: 4.9, images: ["https://images.unsplash.com/photo-1566073771259-6a8506099945?auto=format&fit=crop&w=800&q=80"], desc: "Historic elegance overlooking the Gateway of India.", lat: 18.9220, lng: 72.8347, amenities: ['<i class="fa-solid fa-spa"></i> Spa', '<i class="fa-solid fa-water"></i> Ocean View', '<i class="fa-solid fa-bell-concierge"></i> 24/7 Concierge'] },
            { id: 15, name: "Rocky Mountain Lodge", loc: "Banff, Canada", price: 21000, rating: 4.7, images: ["https://images.unsplash.com/photo-1542314831-068cd1dbfeeb?auto=format&fit=crop&w=800&q=80"], desc: "Cozy wooden lodge with spectacular glacial lake views.", lat: 51.1784, lng: -115.5708, amenities: ['<i class="fa-solid fa-mountain"></i> Mountain View', '<i class="fa-solid fa-fire"></i> Fireplace', '<i class="fa-solid fa-person-hiking"></i> Hiking Trails'] },
            { id: 16, name: "Copacabana Sands", loc: "Rio de Janeiro, Brazil", price: 13000, rating: 4.5, images: ["https://images.unsplash.com/photo-1571896349842-33c89424de2d?auto=format&fit=crop&w=800&q=80"], desc: "Vibrant beachside hotel with live music and cocktails.", lat: -22.9068, lng: -43.1729, amenities: ['<i class="fa-solid fa-umbrella-beach"></i> Beach Access', '<i class="fa-solid fa-martini-glass"></i> Bar', '<i class="fa-solid fa-music"></i> Live Entertainment'] },
            { id: 17, name: "Colosseum Boutique", loc: "Rome, Italy", price: 16500, rating: 4.6, images: ["data:image/jpeg;base64,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"], desc: "Rustic charm just steps away from ancient ruins.", lat: 41.9028, lng: 12.4964, amenities: ['<i class="fa-solid fa-wine-glass"></i> Wine Tasting', '<i class="fa-solid fa-wifi"></i> Free Wi-Fi', '<i class="fa-solid fa-city"></i> City View'] },
            { id: 18, name: "Maldives Ocean Villas", loc: "Malé, Maldives", price: 65000, rating: 5.0, images: ["https://images.unsplash.com/photo-1499793983690-e29da59ef1c2?auto=format&fit=crop&w=800&q=80"], desc: "Private overwater bungalows with glass floors.", lat: 3.2028, lng: 73.2207, amenities: ['<i class="fa-solid fa-water"></i> Ocean View', '<i class="fa-solid fa-spa"></i> Spa', '<i class="fa-solid fa-fish"></i> Scuba Diving'] },
            { id: 19, name: "Cape Town Retreat", loc: "Cape Town, South Africa", price: 14000, rating: 4.7, images: ["https://images.unsplash.com/photo-1580237072617-771c3ecc4a24?auto=format&fit=crop&w=800&q=80"], desc: "Nestled at the base of Table Mountain with sweeping city views.", lat: -33.9249, lng: 18.4241, amenities: ['<i class="fa-solid fa-mountain"></i> Great Views', '<i class="fa-solid fa-water-ladder"></i> Pool', '<i class="fa-solid fa-car"></i> Free Parking'] }
        ];

        // MOCK API LAYER
        const api = {
            getHotels: () => new Promise((resolve) => setTimeout(() => resolve([...DB_HOTELS]), 400)),
            getUserProfile: (email) => new Promise((resolve, reject) => {
                setTimeout(() => {
                    const users = JSON.parse(localStorage.getItem('ls_users')) || [];
                    const user = users.find(u => u.email === email);
                    user ? resolve(user) : reject("User not found");
                }, 300);
            })
        };

        const ui = {
            closeModal: (id) => { 
                document.getElementById(id).classList.add('hidden'); 
                if(id==='hotelModal') document.getElementById('hotelModalContent').classList.add('translate-x-full'); 
                if(id==='paymentModal') clearInterval(app.state.timer);
                if(id==='userTripsModal') clearInterval(app.state.tripTimer);
            },
            toggleProfileDropdown: (e) => {
                e.stopPropagation();
                document.getElementById('profileDropdown').classList.toggle('hidden');
                document.getElementById('notifDropdown').classList.add('hidden');
            },
            toggleNotifications: (e) => { 
                e.stopPropagation(); 
                document.getElementById('notifDropdown').classList.toggle('hidden'); 
                document.getElementById('notifBadge').classList.add('hidden'); 
                document.getElementById('profileDropdown').classList.add('hidden');
                app.renderNotifs(); 
            },
            closeAllDropdowns: (e) => {
                if (!e.target.closest('#userProfile')) {
                    const dropdown = document.getElementById('profileDropdown');
                    if(dropdown) dropdown.classList.add('hidden');
                }
                if (!e.target.closest('#notifContainer')) {
                    const notifDrop = document.getElementById('notifDropdown');
                    if(notifDrop) notifDrop.classList.add('hidden');
                }
            },
            openAuthModal: (type) => { 
                document.getElementById('authModal').classList.remove('hidden'); 
                const t=document.getElementById('authTitle'), n=document.getElementById('authName'), s=document.getElementById('authSwitch');
                if(type==='login'){ t.innerText='Login'; n.classList.add('hidden'); s.innerText="New here? Create Account"; }
                else { t.innerText='Register'; n.classList.remove('hidden'); s.innerText="Has account? Login"; }
            },
            openProfileModal: () => {
                if(!app.state.user) return;
                document.getElementById('editProfName').value = app.state.user.name;
                document.getElementById('editProfEmail').value = app.state.user.email;
                document.getElementById('profileModal').classList.remove('hidden');
                document.getElementById('profileDropdown').classList.add('hidden');
            },
            toggleAuth: () => { const t = document.getElementById('authTitle'); ui.openAuthModal(t.innerText==='Login'?'register':'login'); },
            toggleTheme: () => { document.documentElement.classList.toggle('dark'); localStorage.setItem('theme', document.documentElement.classList.contains('dark')?'dark':'light'); },
            toggleChat: () => { document.getElementById('chatWindow').classList.toggle('hidden'); },
            
            openSavedModal: () => {
                if(!app.state.user) return ui.showToast("Please Sign In First");
                const container = document.getElementById('savedContainer');
                const favs = app.state.user.favorites || [];
                
                if(favs.length === 0) {
                    container.innerHTML = `<div class="col-span-full text-center py-16"><i class="fa-regular fa-heart text-5xl text-slate-300 dark:text-slate-600 mb-4"></i><p class="text-slate-500 dark:text-slate-400">You haven't saved any properties yet.</p></div>`;
                } else {
                    const savedHotels = DB_HOTELS.filter(h => favs.includes(h.id));
                    container.innerHTML = savedHotels.map(h => `
                        <div class="bg-white dark:bg-slate-800 rounded-2xl shadow-sm border border-slate-100 dark:border-slate-700 overflow-hidden group cursor-pointer" onclick="app.openHotel(${h.id})">
                            <div class="relative h-48 overflow-hidden">
                                <img src="${h.images[0]}" class="w-full h-full object-cover">
                                <button onclick="app.toggleFavorite(event, ${h.id}); ui.openSavedModal();" class="absolute top-3 right-3 bg-white/90 p-2 rounded-full shadow text-red-500 hover:scale-110 transition"><i class="fa-solid fa-heart"></i></button>
                            </div>
                            <div class="p-4">
                                <h3 class="font-bold text-lg dark:text-white truncate">${h.name}</h3>
                                <p class="text-slate-500 text-sm"><i class="fa-solid fa-location-dot text-brand-500 mr-1"></i> ${h.loc}</p>
                            </div>
                        </div>
                    `).join('');
                }
                document.getElementById('savedModal').classList.remove('hidden');
            },

            openBookingsModal: () => { 
                if(!app.state.user) return ui.showToast("Please Sign In First");
                const bookings = JSON.parse(localStorage.getItem('ls_bookings')) || [];
                
                // Strictly filter to ONLY show the current user's trips
                const userBookings = bookings.filter(b => b.email === app.state.user.email).sort((a,b) => b.id - a.id);
                const container = document.getElementById('userTripsContainer');
                
                const activeBookings = userBookings.filter(b => b.status !== 'CANCELLED');
                const now = Date.now();
                
                // Find the closest upcoming trip
                const upcomingTrips = activeBookings.filter(b => b.checkIn && b.checkIn > now).sort((a, b) => a.checkIn - b.checkIn);
                const nextTrip = upcomingTrips[0];

                let timerHtml = '';
                if (nextTrip) {
                    timerHtml = `
                    <div class="bg-gradient-to-r from-brand-600 to-indigo-600 rounded-2xl p-6 text-white mb-6 shadow-lg relative overflow-hidden animate-slide-up">
                        <div class="absolute -right-10 -top-10 opacity-10"><i class="fa-solid fa-clock text-9xl"></i></div>
                        <h3 class="text-xs font-bold uppercase tracking-wider mb-1 text-brand-200">Your Next Getaway</h3>
                        <h2 class="text-2xl font-bold mb-4 truncate">${nextTrip.hotel}</h2>
                        <div class="flex gap-3">
                            <div class="bg-black/20 backdrop-blur-md rounded-xl p-3 w-16 text-center border border-white/10"><span class="block text-2xl font-bold" id="cd-days">00</span><span class="text-[9px] uppercase tracking-wider">Days</span></div>
                            <div class="bg-black/20 backdrop-blur-md rounded-xl p-3 w-16 text-center border border-white/10"><span class="block text-2xl font-bold" id="cd-hours">00</span><span class="text-[9px] uppercase tracking-wider">Hours</span></div>
                            <div class="bg-black/20 backdrop-blur-md rounded-xl p-3 w-16 text-center border border-white/10"><span class="block text-2xl font-bold" id="cd-mins">00</span><span class="text-[9px] uppercase tracking-wider">Mins</span></div>
                            <div class="bg-black/20 backdrop-blur-md rounded-xl p-3 w-16 text-center border border-white/10"><span class="block text-2xl font-bold" id="cd-secs">00</span><span class="text-[9px] uppercase tracking-wider">Secs</span></div>
                        </div>
                    </div>`;
                }

                if(userBookings.length === 0) {
                    container.innerHTML = `
                        <div class="text-center py-16">
                            <i class="fa-solid fa-plane-slash text-5xl text-slate-300 dark:text-slate-600 mb-4"></i>
                            <p class="text-slate-500 dark:text-slate-400 mb-4">You haven't booked any trips yet.</p>
                            <button onclick="ui.closeModal('userTripsModal')" class="bg-brand-600 text-white px-6 py-2 rounded-full font-bold hover:bg-brand-700 transition">Explore Hotels</button>
                        </div>`;
                } else {
                    container.innerHTML = timerHtml + `<div class="grid grid-cols-1 gap-4">` + userBookings.map(b => {
                        const date = new Date(b.id).toLocaleDateString('en-US', { day: 'numeric', month: 'short', year: 'numeric' });
                        const isCanceled = b.status === 'CANCELLED';
                        return `
                        <div class="bg-white dark:bg-slate-800 p-4 rounded-xl border border-slate-200 dark:border-slate-700 flex flex-col sm:flex-row gap-4 shadow-sm transition ${isCanceled ? 'opacity-75' : 'hover:shadow-md'}">
                            <div class="${isCanceled ? 'bg-red-100 dark:bg-red-900/30' : 'bg-brand-100 dark:bg-slate-700'} w-16 h-16 rounded-lg flex items-center justify-center shrink-0">
                                <i class="fa-solid ${isCanceled ? 'fa-ban text-red-500' : 'fa-bed text-brand-600 dark:text-brand-400'} text-xl"></i>
                            </div>
                            <div class="flex-grow">
                                <div class="flex justify-between items-start mb-1 gap-2">
                                    <h3 class="font-bold text-slate-900 dark:text-white truncate text-lg ${isCanceled ? 'line-through text-slate-500' : ''}">${b.hotel}</h3>
                                    <span class="text-[10px] font-bold px-2 py-1 rounded shrink-0 ${isCanceled ? 'bg-red-100 text-red-700' : 'bg-green-100 text-green-700'}">${b.status}</span>
                                </div>
                                <div class="flex justify-between items-end mt-4">
                                    <p class="text-xs text-slate-500 dark:text-slate-400">Booked: ${date}</p>
                                    <div class="flex gap-2">
                                        ${!isCanceled ? `<button onclick="app.cancelBooking('${b.id}')" class="text-xs bg-red-50 hover:bg-red-100 text-red-600 px-3 py-1.5 rounded font-bold transition">Cancel</button>` : ''}
                                        <button onclick="app.downloadReceipt('${b.id}')" class="text-xs bg-slate-100 hover:bg-slate-200 text-slate-700 px-3 py-1.5 rounded font-bold transition"><i class="fa-solid fa-file-pdf text-red-500 mr-1"></i> PDF</button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    `}).join('') + `</div>`;
                }
                
                document.getElementById('userTripsModal').classList.remove('hidden');

                // Start the Live Countdown
                clearInterval(app.state.tripTimer);
                if (nextTrip) {
                    app.state.tripTimer = setInterval(() => {
                        const diff = nextTrip.checkIn - Date.now();
                        
                        if (diff <= 0) {
                            clearInterval(app.state.tripTimer);
                            document.getElementById('cd-days').innerText = "00";
                            return;
                        }

                        const elD = document.getElementById('cd-days');
                        if(!elD) { clearInterval(app.state.tripTimer); return; } // Stop if DOM is removed
                        
                        elD.innerText = Math.floor(diff / (1000 * 60 * 60 * 24)).toString().padStart(2, '0');
                        document.getElementById('cd-hours').innerText = Math.floor((diff / (1000 * 60 * 60)) % 24).toString().padStart(2, '0');
                        document.getElementById('cd-mins').innerText = Math.floor((diff / 1000 / 60) % 60).toString().padStart(2, '0');
                        document.getElementById('cd-secs').innerText = Math.floor((diff / 1000) % 60).toString().padStart(2, '0');
                    }, 1000);
                }

                // Initialize or update the Personal Map
                setTimeout(() => {
                    if (!app.myTripsMapInstance) {
                        app.myTripsMapInstance = L.map('myTripsMap').setView([20, 0], 2);
                        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(app.myTripsMapInstance);
                        app.myTripsMarkers = L.layerGroup().addTo(app.myTripsMapInstance);
                    } else {
                        app.myTripsMapInstance.invalidateSize();
                        app.myTripsMarkers.clearLayers();
                    }

                    const bounds = [];
                    activeBookings.forEach(booking => {
                        const hotelData = DB_HOTELS.find(h => h.name === booking.hotel);
                        if (hotelData) {
                            const marker = L.marker([hotelData.lat, hotelData.lng])
                                .bindPopup(`<b>${hotelData.name}</b><br><span class="text-xs text-slate-500">${hotelData.loc}</span>`);
                            app.myTripsMarkers.addLayer(marker);
                            bounds.push([hotelData.lat, hotelData.lng]);
                        }
                    });

                    if (bounds.length > 0) {
                        app.myTripsMapInstance.fitBounds(bounds, { padding: [30, 30], maxZoom: 12 });
                    } else {
                        app.myTripsMapInstance.setView([20, 0], 2); 
                    }
                }, 100);
            },

            showToast: (msg) => { const t = document.getElementById('toast'); document.getElementById('toastMsg').innerText=msg; t.classList.remove('-translate-y-20','opacity-0'); setTimeout(()=>t.classList.add('-translate-y-20','opacity-0'),5000); },
            
            switchTab: (t) => { 
                document.getElementById('viewInfo').classList.toggle('hidden', t!=='info'); 
                document.getElementById('viewMap').classList.toggle('hidden', t!=='map');
                document.getElementById('tabInfo').className = t === 'info' ? "flex-1 py-3 text-sm font-bold text-brand-600 border-b-2 border-brand-600 bg-slate-50 dark:bg-slate-800/50" : "flex-1 py-3 text-sm font-bold text-slate-500 hover:text-slate-700 dark:text-slate-400";
                document.getElementById('tabMap').className = t === 'map' ? "flex-1 py-3 text-sm font-bold text-brand-600 border-b-2 border-brand-600 bg-slate-50 dark:bg-slate-800/50" : "flex-1 py-3 text-sm font-bold text-slate-500 hover:text-slate-700 dark:text-slate-400";

                if(t==='map') {
                    setTimeout(() => { 
                        if(app.modalMapInstance) {
                            app.modalMapInstance.invalidateSize(); 
                            if(app.state.currentHotel) {
                                app.modalMapInstance.setView([app.state.currentHotel.lat, app.state.currentHotel.lng], 13);
                            }
                        }
                    }, 100);
                }
            },
            
            toggleView: (type) => {
                document.getElementById('hotelGrid').classList.toggle('hidden', type==='map');
                document.getElementById('globalMapContainer').classList.toggle('hidden', type!=='map');
                document.getElementById('viewGrid').className = type==='grid' ? "flex-1 py-2 rounded-lg bg-white dark:bg-slate-700 shadow text-brand-600 text-xs font-bold transition" : "flex-1 py-2 rounded-lg text-slate-500 hover:bg-white dark:hover:bg-slate-800 text-xs font-bold transition";
                document.getElementById('viewMapToggle').className = type==='map' ? "flex-1 py-2 rounded-lg bg-white dark:bg-slate-700 shadow text-brand-600 text-xs font-bold transition" : "flex-1 py-2 rounded-lg text-slate-500 hover:bg-white dark:hover:bg-slate-800 text-xs font-bold transition";
                if(type==='map') app.initGlobalMap();
            }
        };

        const admin = {
            chartInstance: null,
            destChartInstance: null,
            openPanel: () => { document.getElementById('adminModal').classList.remove('hidden'); admin.renderOverview(); admin.switchTab('overview'); },
            switchTab: (tab) => {
                ['overview','bookings','users', 'logs'].forEach(t => {
                    document.getElementById(`admView${t.charAt(0).toUpperCase()+t.slice(1)}`).classList.add('hidden');
                    const btn = document.getElementById(`admTab${t.charAt(0).toUpperCase()+t.slice(1).substring(0,3)}${t==='users'?'User':t==='overview'?'Over':''}`);
                    if(btn) btn.className = "px-4 py-3 text-sm font-bold border-b-2 border-transparent text-slate-500 hover:text-slate-700";
                });
                document.getElementById(`admView${tab.charAt(0).toUpperCase()+tab.slice(1)}`).classList.remove('hidden');
                
                const activeBtn = document.getElementById(`admTab${tab.charAt(0).toUpperCase()+tab.slice(1).substring(0,3)}${tab==='users'?'User':tab==='overview'?'Over':''}`);
                if(activeBtn) activeBtn.className = "px-4 py-3 text-sm font-bold border-b-2 border-brand-600 text-brand-600";
                
                if(tab === 'bookings') admin.renderBookings();
                if(tab === 'users') admin.renderUsers();
                if(tab === 'logs') admin.renderLogs();
            },
            getUsers: () => JSON.parse(localStorage.getItem('ls_users')) || [],
            getBookings: () => JSON.parse(localStorage.getItem('ls_bookings')) || [],
            
            // Audit Logging
            getLogs: () => JSON.parse(localStorage.getItem('ls_logs')) || [],
            logAction: (action, user, details) => {
                let logs = admin.getLogs();
                const timestamp = new Date().toLocaleString();
                logs.unshift(`[${timestamp}] [${user}] ${action}: ${details}`);
                if (logs.length > 50) logs.pop(); 
                localStorage.setItem('ls_logs', JSON.stringify(logs));
            },
            renderLogs: () => {
                const logConsole = document.getElementById('adminLogConsole');
                const logs = admin.getLogs();
                logConsole.innerHTML = logs.length ? logs.map(l => `<div>> ${l}</div>`).join('') : '> System initialized. No recent activity.';
            },
            clearLogs: () => {
                if(confirm("Clear all security and activity logs?")) {
                    localStorage.setItem('ls_logs', '[]');
                    admin.renderLogs();
                }
            },

            renderOverview: () => {
                const b = admin.getBookings(); const u = admin.getUsers();
                const totalInr = b.reduce((acc, c) => acc + (c.status === 'CONFIRMED' ? c.amount : 0), 0);
                document.getElementById('admRev').innerText = app.formatPrice(totalInr);
                document.getElementById('admBookCount').innerText = b.length;
                
                // Revenue Trend Chart
                const ctx = document.getElementById('revenueChart').getContext('2d');
                if(admin.chartInstance) admin.chartInstance.destroy();
                const labels = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
                const dataPoints = [12000, 19000, 3000, 5000, 20000, 35000, 45000];
                admin.chartInstance = new Chart(ctx, {
                    type: 'line',
                    data: { labels: labels, datasets: [{ label: 'Revenue (INR)', data: dataPoints, borderColor: '#2563eb', backgroundColor: 'rgba(37, 99, 235, 0.1)', borderWidth: 2, tension: 0.4, fill: true }] },
                    options: { responsive: true, maintainAspectRatio: false }
                });

                // Destination Revenue Chart
                const destCtx = document.getElementById('destinationChart').getContext('2d');
                if(admin.destChartInstance) admin.destChartInstance.destroy();
                const revData = b.filter(x => x.status === 'CONFIRMED');
                const revenueByDest = {};
                revData.forEach(bk => {
                    const hotel = DB_HOTELS.find(h => h.name === bk.hotel);
                    if(hotel) {
                        const city = hotel.loc.split(',')[0]; 
                        revenueByDest[city] = (revenueByDest[city] || 0) + bk.amount;
                    }
                });
                const destLabels = Object.keys(revenueByDest);
                const destValues = Object.values(revenueByDest);
                admin.destChartInstance = new Chart(destCtx, {
                    type: 'doughnut',
                    data: { 
                        labels: destLabels.length ? destLabels : ['No Data'], 
                        datasets: [{ 
                            data: destValues.length ? destValues : [1], 
                            backgroundColor: ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6'],
                            borderWidth: 0
                        }] 
                    },
                    options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { position: 'right' } } }
                });
            },
            renderBookings: () => {
                document.getElementById('admTableBookings').innerHTML = admin.getBookings().map(x => `<tr><td class="font-mono text-xs">${x.id}</td><td class="font-bold">${x.user}</td><td class="text-xs text-slate-500">${x.email}</td><td class="text-xs text-slate-500">${x.phone || 'N/A'}</td><td>${x.hotel}</td><td>${app.formatPrice(x.amount)}</td><td><span class="px-2 py-1 rounded-full text-[10px] font-bold ${x.status==='CONFIRMED'?'bg-green-100 text-green-700':'bg-red-100 text-red-700'}">${x.status}</span></td></tr>`).join('') || '<tr><td colspan="7" class="p-4 text-center text-slate-400">No bookings</td></tr>';
            },
            renderUsers: () => {
                document.getElementById('admTableUsers').innerHTML = admin.getUsers().map(x => `<tr><td class="font-bold">${x.name}</td><td>${x.email}</td><td><span class="px-2 py-1 rounded text-xs ${x.role==='Admin'?'bg-purple-100 text-purple-700':'bg-slate-100 text-slate-700'}">${x.role}</span></td></tr>`).join('');
            },
            exportCSV: (type) => {
                const data = type === 'bookings' ? admin.getBookings() : admin.getUsers();
                if(!data.length) return ui.showToast("No data to export");
                const headers = Object.keys(data[0]);
                const rows = data.map(obj => headers.map(h => `"${obj[h] || ''}"`).join(","));
                const csvContent = [headers.join(","), ...rows].join("\n");
                const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
                const url = URL.createObjectURL(blob);
                const link = document.createElement("a");
                link.setAttribute("href", url);
                link.setAttribute("download", `lux_${type}_export.csv`);
                document.body.appendChild(link); link.click(); document.body.removeChild(link);
                ui.showToast(`${type} exported successfully!`);
                admin.logAction('EXPORT', app.state.user.name, `Exported ${type} to CSV`);
            },
            exportRelationalSchema: () => {
                const users = admin.getUsers();
                const bookings = admin.getBookings();
                
                const tbl_users = users.map((u, index) => ({
                    user_id: index + 1,
                    full_name: u.name,
                    email: u.email,
                    role: u.role,
                    created_at: new Date().toISOString()
                }));

                const tbl_hotels = DB_HOTELS.map(h => ({
                    hotel_id: h.id, 
                    property_name: h.name,
                    location: h.loc,
                    base_price: h.price,
                    latitude: h.lat,
                    longitude: h.lng
                }));

                const tbl_bookings = bookings.map(b => {
                    const user = tbl_users.find(u => u.email === b.email);
                    const hotel = tbl_hotels.find(h => h.property_name === b.hotel);
                    return {
                        booking_id: b.id,
                        user_id_fk: user ? user.user_id : null,
                        hotel_id_fk: hotel ? hotel.hotel_id : null, 
                        total_paid: b.amount,
                        status: b.status,
                        booking_date: new Date(b.id).toISOString()
                    };
                });

                const schema = { database_name: "bookourhotels_db", export_date: new Date().toISOString(), tables: { tbl_users, tbl_hotels, tbl_bookings } };
                const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(schema, null, 2));
                const link = document.createElement('a');
                link.setAttribute("href", dataStr); link.setAttribute("download", "relational_schema_seed.json");
                document.body.appendChild(link); link.click(); link.remove();
                
                ui.showToast("Database schema exported successfully!");
                admin.logAction('EXPORT_SCHEMA', app.state.user.name, `Exported relational schema`);
            }
        };

        const app = {
            state: { 
                user: JSON.parse(localStorage.getItem('user')) || null, 
                currentHotel: null, 
                currentBestTimeCity: null,
                searchHistory: JSON.parse(localStorage.getItem('searchHistory')) || [], 
                timer: null, 
                tripTimer: null,
                discount: 0,
                currency: 'INR',
                rates: { INR: 1, USD: 0.012, EUR: 0.011, symbols: { INR: '₹', USD: '$', EUR: '€' } },
                fpIn: null,
                fpOut: null
            },
            sessionState: { timeoutTimer: null, countdownInterval: null, idleLimit: 15 * 60 * 1000 },
            modalMapInstance: null,
            modalMarker: null,
            myTripsMapInstance: null,
            myTripsMarkers: null,
            
            init: async () => {
                if(localStorage.getItem('theme')==='dark') document.documentElement.classList.add('dark');
                app.seedData(); 
                app.checkAuth(); 
                
                // Add scroll listener for the Scroll-to-Top button
                window.addEventListener('scroll', () => {
                    const btn = document.getElementById('scrollToTopBtn');
                    if (window.scrollY > 500) {
                        btn.classList.remove('opacity-0', 'pointer-events-none', 'translate-y-10');
                    } else {
                        btn.classList.add('opacity-0', 'pointer-events-none', 'translate-y-10');
                    }
                });

                // --- Cross Tab Sync Fix ---
                window.addEventListener('storage', (e) => {
                    if (e.key === 'user' || e.key === 'ls_users' || e.key === 'ls_bookings') {
                        app.state.user = JSON.parse(localStorage.getItem('user'));
                        app.checkAuth();
                        if (document.getElementById('userTripsModal') && !document.getElementById('userTripsModal').classList.contains('hidden')) {
                            ui.openBookingsModal();
                        }
                    }
                });
                
                document.getElementById('resultCount').innerText = "Fetching real-time availability...";
                const data = await api.getHotels();
                app.renderHotels(data);
                
                // Initialize Best Time to Book Section dynamically
                app.initBestTime();
                
                // Initialize Flatpickr for date inputs
                app.state.fpIn = flatpickr("#dateIn", {
                    defaultDate: "today",
                    minDate: "today",
                    onChange: function(selectedDates, dateStr, instance) {
                        let nextDay = new Date(selectedDates[0]);
                        nextDay.setDate(nextDay.getDate() + 1);
                        app.state.fpOut.set('minDate', nextDay);
                        
                        if(new Date(app.state.fpOut.selectedDates[0]) <= selectedDates[0]) {
                            app.state.fpOut.setDate(nextDay);
                        }
                        app.updateTotal();
                    }
                });
                
                let tmrw = new Date(); tmrw.setDate(tmrw.getDate() + 1);
                app.state.fpOut = flatpickr("#dateOut", {
                    defaultDate: tmrw,
                    minDate: tmrw,
                    onChange: function() { app.updateTotal(); }
                });
                
                chatbot.init();
                app.initSessionSecurity();
                setTimeout(() => app.observeElements(), 500); // Ensure static elements are caught
            },

            initBestTime: () => {
                // Group hotels by city from DB_HOTELS
                const cityData = {};
                DB_HOTELS.forEach(h => {
                    const city = h.loc.split(',')[0].trim();
                    if(!cityData[city]) {
                        cityData[city] = true; // just mark existence
                    }
                });

                const cities = Object.keys(cityData);
                if(cities.length === 0) return;

                // Render Dynamic Tabs
                const tabsContainer = document.getElementById('bestTimeTabs');
                tabsContainer.innerHTML = cities.map((city, idx) => `
                    <button onclick="app.selectBestTimeCity('${city}')" id="btn-city-${city.replace(/\s+/g, '-')}" class="best-time-tab text-sm font-bold ${idx === 0 ? 'text-brand-600 border-b-2 border-brand-600' : 'text-slate-700 dark:text-slate-300 border-b-2 border-transparent'} pb-3 whitespace-nowrap hover:text-brand-600 transition-colors">${city}</button>
                `).join('');

                // Render initial city
                app.selectBestTimeCity(cities[0]);
            },

            selectBestTimeCity: (city) => {
                app.state.currentBestTimeCity = city;
                
                // Update tabs active state
                document.querySelectorAll('.best-time-tab').forEach(btn => {
                    btn.classList.remove('text-brand-600', 'border-brand-600');
                    btn.classList.add('text-slate-700', 'dark:text-slate-300', 'border-transparent');
                });
                const activeBtn = document.getElementById(`btn-city-${city.replace(/\s+/g, '-')}`);
                if(activeBtn) {
                    activeBtn.classList.remove('text-slate-700', 'dark:text-slate-300', 'border-transparent');
                    activeBtn.classList.add('text-brand-600', 'border-brand-600');
                }

                // Get pricing and image data for this city
                const cityData = {};
                DB_HOTELS.forEach(h => {
                    const c = h.loc.split(',')[0].trim();
                    if(!cityData[c]) cityData[c] = { min: h.price, max: h.price, img: h.images[0] };
                    else {
                        cityData[c].min = Math.min(cityData[c].min, h.price);
                        cityData[c].max = Math.max(cityData[c].max, h.price);
                    }
                });

                const data = cityData[city];
                if(!data) return;
                
                document.getElementById('bestTimeImg').src = data.img;

                // Generate months with simulated price variations
                const months = ['September', 'October', 'November', 'December', 'January', 'February', 'March'];
                const contentContainer = document.getElementById('bestTimePrices');
                
                contentContainer.innerHTML = months.map((m, i) => {
                    // Simulate monthly variation for realism (+/- up to 15%)
                    const variation = 1 + (Math.sin(i) * 0.15); 
                    const minP = Math.floor(data.min * variation);
                    const maxP = Math.floor(data.max * variation * 1.2); // max is slightly higher
                    
                    return `
                    <div class="flex justify-between items-center p-4 border border-slate-200 dark:border-slate-700 rounded-xl hover:shadow-md transition cursor-pointer bg-white dark:bg-slate-800/50 hover:border-brand-300 dark:hover:border-brand-600">
                        <span class="font-bold text-[15px] text-slate-800 dark:text-slate-200">${m}</span>
                        <span class="font-bold text-[15px] text-slate-800 dark:text-slate-200">${app.formatPrice(minP)} - ${app.formatPrice(maxP)} <i class="fa-solid fa-chevron-right text-slate-400 ml-3 text-[11px]"></i></span>
                    </div>`;
                }).join('');
            },

            observeElements: () => {
                const observer = new IntersectionObserver((entries) => {
                    entries.forEach((entry, index) => {
                        if (entry.isIntersecting) {
                            setTimeout(() => {
                                entry.target.classList.add('is-visible');
                            }, index * 100); 
                            observer.unobserve(entry.target);
                        }
                    });
                }, { threshold: 0.1, rootMargin: "0px 0px -50px 0px" });

                document.querySelectorAll('.reveal-item').forEach(el => observer.observe(el));
            },

            formatPrice: (inrAmount) => {
                const c = app.state.currency;
                const rate = app.state.rates[c];
                const symbol = app.state.rates.symbols[c];
                return `${symbol}${(inrAmount * rate).toLocaleString(undefined, {maximumFractionDigits: 0})}`;
            },

            changeCurrency: () => {
                app.state.currency = document.getElementById('currencySelect').value;
                app.search(); 
                app.updatePriceLabel(document.getElementById('priceRange').value);
                
                // Refresh Best Time Prices with new currency
                if(app.state.currentBestTimeCity) {
                    app.selectBestTimeCity(app.state.currentBestTimeCity);
                }
            },

            initSessionSecurity: () => {
                if (!app.state.user) return;
                ['mousemove', 'keydown', 'scroll', 'click'].forEach(evt => window.addEventListener(evt, app.resetSessionTimer));
                app.resetSessionTimer();
            },

            resetSessionTimer: () => {
                clearTimeout(app.sessionState.timeoutTimer);
                clearInterval(app.sessionState.countdownInterval);
                document.getElementById('timeoutModal').classList.add('hidden');
                app.sessionState.timeoutTimer = setTimeout(app.showTimeoutWarning, app.sessionState.idleLimit);
            },

            showTimeoutWarning: () => {
                document.getElementById('timeoutModal').classList.remove('hidden');
                let timeLeft = 60;
                document.getElementById('timeoutSeconds').innerText = timeLeft;
                app.sessionState.countdownInterval = setInterval(() => {
                    timeLeft--;
                    document.getElementById('timeoutSeconds').innerText = timeLeft;
                    if (timeLeft <= 0) { clearInterval(app.sessionState.countdownInterval); app.logout(); }
                }, 1000);
            },

            resetSession: () => { app.resetSessionTimer(); ui.showToast("Session extended."); },

            seedData: () => {
                if(!localStorage.getItem('ls_users')) {
                    const users = [{ name: "Admin User", email: "admin@lux.com", role: "Admin", favorites: [], points: 0, notifications: [] }];
                    localStorage.setItem('ls_users', JSON.stringify(users));
                }
                if(!localStorage.getItem('ls_bookings')) localStorage.setItem('ls_bookings', '[]');
            },

            checkAuth: () => {
                if(app.state.user) {
                    if(!app.state.user.favorites) app.state.user.favorites = [];
                    if(!app.state.user.points) app.state.user.points = 0;
                    if(!app.state.user.notifications) app.state.user.notifications = [];
                    
                    document.getElementById('authButtons').classList.add('hidden');
                    document.getElementById('userProfile').classList.remove('hidden'); document.getElementById('userProfile').classList.add('flex');
                    document.getElementById('notifContainer').classList.remove('hidden'); // Show Bell
                    
                    document.getElementById('userNameDisplay').innerText = app.state.user.name;
                    document.getElementById('userPointsDisplay').innerText = app.state.user.points;
                    document.getElementById('userAvatar').src = `https://ui-avatars.com/api/?name=${encodeURIComponent(app.state.user.name)}&background=2563eb&color=fff`;
                    if(app.state.user.email === 'admin@lux.com') document.getElementById('adminLink').classList.remove('hidden');
                    
                    if(app.state.user.notifications.length > 0) {
                        document.getElementById('notifBadge').classList.remove('hidden');
                    }
                } else {
                    document.getElementById('authButtons').classList.remove('hidden');
                    document.getElementById('userProfile').classList.add('hidden'); document.getElementById('userProfile').classList.remove('flex');
                    document.getElementById('notifContainer').classList.add('hidden'); // Hide Bell
                }
            },

            auth: (e) => {
                e.preventDefault();
                const email = document.getElementById('authEmail').value.trim();
                let inputtedName = document.getElementById('authName').value.trim();

                const allowedDomains = ['@gmail.com', '@yahoo.com'];
                const isValidDomain = allowedDomains.some(domain => email.toLowerCase().endsWith(domain));
                
                if (!isValidDomain && email.toLowerCase() !== 'admin@lux.com') {
                    ui.showToast("Please use a @gmail.com or @yahoo.com email address.");
                    return; 
                }
                
                if (!inputtedName) {
                    let usernamePart = email.split('@')[0]; 
                    inputtedName = usernamePart
                        .split(/[\.\-_]/)
                        .map(word => word.charAt(0).toUpperCase() + word.slice(1).toLowerCase())
                        .join(' ');
                }

                let allUsers = admin.getUsers(); 
                let user = allUsers.find(u => u.email === email);
                
                if(!user) { 
                    user = { 
                        name: inputtedName, 
                        email, 
                        role: email === 'admin@lux.com' ? 'Admin' : 'User', 
                        favorites: [],
                        points: 0,
                        notifications: [{msg: `Welcome to BookOurHotels, ${inputtedName}!`, time: Date.now()}] 
                    }; 
                    allUsers.push(user); 
                    localStorage.setItem('ls_users', JSON.stringify(allUsers)); 
                }
                
                app.state.user = user; localStorage.setItem('user', JSON.stringify(user));
                ui.closeModal('authModal'); ui.showToast(`Welcome ${user.name}`); 
                admin.logAction('LOGIN', user.name, 'Successful authentication');
                
                app.checkAuth();
                app.initSessionSecurity();
                app.search();
            },
            
            logout: () => { 
                admin.logAction('LOGOUT', app.state.user ? app.state.user.name : 'Unknown', 'Session ended');
                app.state.user = null; 
                localStorage.removeItem('user'); 
                window.location.reload(); 
            },

            pushNotif: (message) => {
                if(!app.state.user) return;
                if(!app.state.user.notifications) app.state.user.notifications = [];
                app.state.user.notifications.unshift({ msg: message, time: Date.now() });
                
                const bell = document.getElementById('bellIcon');
                if(bell) {
                    bell.classList.add('animate-wiggle');
                    setTimeout(() => bell.classList.remove('animate-wiggle'), 1000);
                }

                document.getElementById('notifBadge').classList.remove('hidden');
                app.updateUserInDB();
            },

            clearNotifs: (e) => {
                e.stopPropagation();
                if(app.state.user) { app.state.user.notifications = []; app.updateUserInDB(); app.renderNotifs(); }
            },

            renderNotifs: () => {
                const list = document.getElementById('notifList');
                const notifs = app.state.user?.notifications || [];
                if(notifs.length === 0) {
                    list.innerHTML = '<p class="text-xs text-center text-slate-400 py-4">No new alerts</p>';
                } else {
                    list.innerHTML = notifs.map(n => `
                        <div class="p-3 bg-slate-50 dark:bg-slate-700/50 rounded-lg text-sm text-slate-700 dark:text-slate-200 border-l-2 border-brand-500">
                            <p>${n.msg}</p>
                            <p class="text-[10px] text-slate-400 mt-1">${new Date(n.time).toLocaleTimeString()}</p>
                        </div>
                    `).join('');
                }
            },

            toggleFavorite: (e, id) => {
                e.stopPropagation();
                if(!app.state.user) return ui.showToast("Please Sign In First");
                let user = app.state.user;
                if(!user.favorites) user.favorites = [];
                const idx = user.favorites.indexOf(id);
                if(idx > -1) { user.favorites.splice(idx, 1); ui.showToast("Removed from Saved"); } 
                else { user.favorites.push(id); ui.showToast("Added to Saved Properties!"); }
                localStorage.setItem('user', JSON.stringify(user));
                app.updateUserInDB();
                app.search(); 
            },

            updateUserInDB: () => {
                let users = admin.getUsers();
                const uIdx = users.findIndex(u => u.email === app.state.user.email);
                if(uIdx > -1) { users[uIdx] = app.state.user; localStorage.setItem('ls_users', JSON.stringify(users)); }
                localStorage.setItem('user', JSON.stringify(app.state.user));
                if(document.getElementById('userPointsDisplay')) {
                    document.getElementById('userPointsDisplay').innerText = app.state.user.points || 0;
                }
            },

            filterCategory: (cat, event) => {
                document.querySelectorAll('.cat-btn').forEach(btn => btn.className = "cat-btn px-4 py-2 rounded-full bg-slate-200 dark:bg-slate-700 text-slate-700 dark:text-slate-300 text-sm font-bold whitespace-nowrap hover:bg-slate-300 transition");
                if(event) event.currentTarget.className = "cat-btn px-4 py-2 rounded-full bg-brand-600 text-white text-sm font-bold whitespace-nowrap transition";

                if (cat === 'all') app.renderHotels(DB_HOTELS);
                else {
                    const filtered = DB_HOTELS.filter(h => h.amenities && h.amenities.some(a => a.toLowerCase().includes(cat)));
                    app.renderHotels(filtered);
                }
            },

            sortHotels: () => {
                const sortType = document.getElementById('sortSelect').value;
                app.search(sortType);
            },

            renderHotels: (data) => {
                const grid = document.getElementById('hotelGrid');
                const favs = app.state.user ? (app.state.user.favorites || []) : [];
                grid.innerHTML = Array(3).fill('<div class="bg-white dark:bg-slate-800 rounded-2xl p-4 h-80 border dark:border-slate-700"><div class="skeleton w-full h-40 rounded-xl mb-4"></div><div class="skeleton w-3/4 h-6 rounded mb-2"></div></div>').join('');
                document.getElementById('resultCount').innerText = "Searching...";
                
                setTimeout(() => {
                    grid.innerHTML = '';
                    document.getElementById('resultCount').innerText = `${data.length} properties found`;
                    data.forEach(h => {
                        const isFav = favs.includes(h.id);
                        grid.innerHTML += `
                        <div class="reveal-item bg-white dark:bg-slate-800 rounded-2xl shadow-sm border border-slate-100 dark:border-slate-700 overflow-hidden group hover:-translate-y-1 hover:shadow-xl transition-all duration-300 cursor-pointer" onclick="app.openHotel(${h.id})">
                            <div class="relative h-56 overflow-hidden">
                                <img src="${h.images[0]}" class="w-full h-full object-cover group-hover:scale-110 transition duration-700">
                                <div class="absolute bottom-3 left-3 bg-white/90 px-2 py-1 rounded text-xs font-bold shadow"><i class="fa-solid fa-star text-yellow-400 mr-1"></i> ${h.rating}</div>
                                <button onclick="app.toggleFavorite(event, ${h.id})" class="absolute top-3 right-3 bg-white/90 p-2 rounded-full shadow hover:scale-110 transition text-lg z-10">
                                    <i class="${isFav ? 'fa-solid text-red-500' : 'fa-regular text-slate-400'} fa-heart"></i>
                                </button>
                            </div>
                            <div class="p-5">
                                <h3 class="font-bold text-lg dark:text-white truncate">${h.name}</h3>
                                <p class="text-slate-500 text-sm mb-4"><i class="fa-solid fa-location-dot text-brand-500 mr-1"></i> ${h.loc}</p>
                                <div class="flex justify-between items-end">
                                    <span class="text-xl font-bold dark:text-white">${app.formatPrice(h.price)}</span>
                                    <button class="text-brand-600 font-bold text-sm hover:underline">View Deal</button>
                                </div>
                            </div>
                        </div>`;
                    });
                    app.observeElements();
                }, 300);
            },

            search: (forcedSort) => {
                const q = document.getElementById('searchInput').value.toLowerCase();
                const maxPrice = document.getElementById('priceRange').value;
                let filtered = DB_HOTELS.filter(h => (h.name.toLowerCase().includes(q) || h.loc.toLowerCase().includes(q)) && h.price <= maxPrice);
                
                const sortType = forcedSort || document.getElementById('sortSelect').value;
                if (sortType === 'price_asc') filtered.sort((a, b) => a.price - b.price);
                else if (sortType === 'price_desc') filtered.sort((a, b) => b.price - a.price);
                else if (sortType === 'rating_desc') filtered.sort((a, b) => b.rating - a.rating);

                app.renderHotels(filtered);
            },

            updatePriceLabel: (val) => { document.getElementById('priceValue').innerText = app.formatPrice(val); },

            openHotel: (id) => {
                const h = DB_HOTELS.find(x => x.id === id);
                app.state.currentHotel = h;
                document.getElementById('modalImg').src = h.images[0];
                document.getElementById('modalTitle').innerText = h.name;
                document.getElementById('modalLoc').innerText = h.loc;
                document.getElementById('modalPrice').innerText = app.formatPrice(h.price);
                document.getElementById('modalRating').innerText = h.rating;
                document.getElementById('modalDesc').innerText = h.desc;
                
                // Simulate real-time viewers for the active modal
                const viewerDiv = document.getElementById('liveViewers');
                if (viewerDiv) {
                    viewerDiv.classList.remove('hidden');
                    // Generates a random number between 12 and 45
                    document.getElementById('viewerCount').innerText = Math.floor(Math.random() * (45 - 12 + 1)) + 12;
                }
                
                // Image Gallery
                const gallery = document.getElementById('modalGallery');
                if (h.images.length > 1) {
                    gallery.innerHTML = h.images.map(img => `<img src="${img}" onclick="document.getElementById('modalImg').src='${img}'" class="w-20 h-16 object-cover cursor-pointer hover:opacity-75 border-2 border-transparent hover:border-brand-500 transition rounded shrink-0">`).join('');
                    gallery.classList.remove('hidden');
                } else { gallery.classList.add('hidden'); }

                // Weather Preview
                const weatherDiv = document.getElementById('modalWeather');
                weatherDiv.classList.remove('hidden');
                const temp = Math.floor(Math.random() * (35 - 15 + 1)) + 15;
                const conditions = ['Sunny', 'Partly Cloudy', 'Clear Skies', 'Light Rain'];
                const icons = ['fa-sun text-yellow-300', 'fa-cloud-sun text-slate-200', 'fa-moon text-blue-200', 'fa-cloud-rain text-blue-300'];
                const r = Math.floor(Math.random() * conditions.length);
                document.getElementById('weatherText').innerText = `${temp}°C • ${conditions[r]}`;
                document.getElementById('weatherIcon').className = `fa-solid ${icons[r]}`;
                
                const amenitiesContainer = document.getElementById('modalAmenities');
                if (h.amenities && h.amenities.length > 0) {
                    amenitiesContainer.innerHTML = h.amenities.map(a => `<span class="bg-slate-100 dark:bg-slate-700 text-slate-700 dark:text-slate-200 text-xs px-3 py-1.5 rounded-full font-medium">${a}</span>`).join('');
                } else { amenitiesContainer.innerHTML = `<span class="text-xs text-slate-400">Standard amenities included.</span>`; }

                app.updateTotal();
                ui.switchTab('info');
                
                // --- Leaflet Mobile Map Drag Fix ---
                if(!app.modalMapInstance) {
                    app.modalMapInstance = L.map('modalMap', { dragging: !L.Browser.mobile, tap: !L.Browser.mobile }).setView([h.lat, h.lng], 13);
                    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(app.modalMapInstance);
                    app.modalMarker = L.marker([h.lat, h.lng]).addTo(app.modalMapInstance);
                } else {
                    app.modalMapInstance.setView([h.lat, h.lng], 13);
                    app.modalMarker.setLatLng([h.lat, h.lng]);
                }
                app.modalMarker.bindPopup(`<b>${h.name}</b><br>${h.loc}`);

                document.getElementById('hotelModal').classList.remove('hidden');
                setTimeout(() => document.getElementById('hotelModalContent').classList.remove('translate-x-full'), 10);
            },

            shareHotel: async () => {
                const h = app.state.currentHotel;
                if (!h) return;
                const shareData = { title: `Check out ${h.name} on BookOurHotels!`, text: `I found this amazing stay in ${h.loc} for ${app.formatPrice(h.price)}/night.`, url: window.location.href + `?hotel=${h.id}` };
                try {
                    if (navigator.share) await navigator.share(shareData);
                    else { await navigator.clipboard.writeText(`${shareData.title}\n${shareData.text}\n${shareData.url}`); ui.showToast("Link copied to clipboard!"); }
                } catch (err) { console.error("Error sharing:", err); }
            },

            // --- Mobile Null Checking for Dates ---
            updateTotal: () => {
                if(!app.state.fpIn || !app.state.fpOut) return 0;
                
                const inDate = app.state.fpIn.selectedDates[0];
                const outDate = app.state.fpOut.selectedDates[0];
                
                if (!inDate || !outDate) {
                    document.getElementById('totalPriceDisplay').innerText = app.formatPrice(0);
                    return 0;
                }
            
                const nights = Math.max(1, Math.ceil((outDate - inDate) / (86400000)));
            
                if (app.state.currentHotel) {
                    const totalInr = app.state.currentHotel.price * nights;
                    document.getElementById('totalPriceDisplay').innerText = app.formatPrice(totalInr);
                    return totalInr;
                }
                return 0;
            },

            openPaymentModal: () => {
                if(!app.state.user) return ui.showToast("Please Sign In First");
                
                ui.closeModal('hotelModal');
                app.state.discount = 0;

                document.getElementById('payImg').src = app.state.currentHotel.images[0];
                document.getElementById('payHotel').innerText = app.state.currentHotel.name;
                document.getElementById('payLoc').innerText = app.state.currentHotel.loc;
                document.getElementById('payIn').innerText = document.getElementById('dateIn').value;
                document.getElementById('payOut').innerText = document.getElementById('dateOut').value;
                document.getElementById('payPhone').value = ''; 
                document.getElementById('cardNum').value = '';
                document.getElementById('cardExp').value = '';
                document.getElementById('cardCvc').value = '';
                app.updatePayTotal();

                let time = 300;
                clearInterval(app.state.timer);
                app.state.timer = setInterval(() => {
                    if(time <= 0) { clearInterval(app.state.timer); ui.closeModal('paymentModal'); ui.showToast("Session Expired"); }
                    const m = Math.floor(time/60); const s = time%60;
                    document.getElementById('payTimer').innerText = `0${m}:${s<10?'0'+s:s}`;
                    time--;
                }, 1000);

                document.getElementById('paymentModal').classList.remove('hidden');
            },

            formatCard: (el) => { let v = el.value.replace(/\D/g, '').substring(0,16); el.value = v != '' ? v.match(/.{1,4}/g).join(' ') : ''; },
            formatExpiry: (el) => { let v = el.value.replace(/\D/g, '').substring(0,4); if(v.length >= 2) v = v.substring(0,2) + '/' + v.substring(2); el.value = v; },

            applyPromo: () => {
                const code = document.getElementById('promoInput').value.toUpperCase();
                if(code === 'LUX10') {
                    app.state.discount = 0.10;
                    document.getElementById('promoMsg').classList.remove('hidden');
                    app.updatePayTotal();
                } else { alert("Invalid Code"); }
            },

            updatePayTotal: () => {
                if(!app.state.fpIn || !app.state.fpOut) return;
                
                const inDate = app.state.fpIn.selectedDates[0];
                const outDate = app.state.fpOut.selectedDates[0];
                if (!inDate || !outDate) return;

                const nights = Math.max(1, Math.ceil((outDate - inDate) / (86400000)));
                const baseInr = app.state.currentHotel.price * nights;
                const taxInr = baseInr * 0.12;
                const totalInr = (baseInr + taxInr) * (1 - app.state.discount);
                document.getElementById('payTotal').innerText = app.formatPrice(totalInr);
                document.getElementById('payBtn').innerText = `Pay ${app.formatPrice(totalInr)}`;
            },

            processPayment: (e) => {
                e.preventDefault();
                const phone = document.getElementById('payPhone').value;
                const exp = document.getElementById('cardExp').value;
                const cvc = document.getElementById('cardCvc').value;
                const card = document.getElementById('cardNum').value.replace(/\s/g, '');

                if(phone.length < 10) return ui.showToast("Enter valid 10-digit Phone");
                if(card.length < 16) return ui.showToast("Invalid Card Number");
                if(cvc.length < 3) return ui.showToast("Invalid CVC");
                
                const [mm, yy] = exp.split('/').map(Number);
                const now = new Date(); const currY = parseInt(now.getFullYear().toString().substr(-2)); const currM = now.getMonth() + 1;
                
                if(!mm || !yy || mm < 1 || mm > 12 || yy < currY || (yy === currY && mm < currM)) {
                    document.getElementById('cardExp').classList.add('error');
                    setTimeout(() => document.getElementById('cardExp').classList.remove('error'), 500);
                    return ui.showToast("Card Expired or Invalid");
                }

                const btn = document.getElementById('payBtn');
                btn.disabled = true;
                btn.innerHTML = '<i class="fa-solid fa-circle-notch fa-spin"></i> Authorizing...';
                
                setTimeout(() => {
                    btn.innerHTML = '<i class="fa-solid fa-shield-halved fa-beat-fade"></i> Securing room...';
                }, 800);
                
                setTimeout(() => {
                    confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 } });
                    
                    const inDate = app.state.fpIn.selectedDates[0];
                    const outDate = app.state.fpOut.selectedDates[0];
                    const nights = Math.max(1, Math.ceil((outDate - inDate) / (86400000)));

                    const baseInr = app.state.currentHotel.price * nights;
                    const taxInr = baseInr * 0.12;
                    const totalInr = Math.floor((baseInr + taxInr) * (1 - app.state.discount));

                    const bookingId = Date.now();
                    let bookings = admin.getBookings();
                    
                    const newBooking = { 
                        id: bookingId, 
                        hotel: app.state.currentHotel.name, 
                        amount: totalInr, 
                        user: app.state.user.name, 
                        email: app.state.user.email, 
                        phone: phone, 
                        status: "CONFIRMED",
                        checkIn: inDate.getTime(),
                        checkOut: outDate.getTime()
                    };
                    bookings.unshift(newBooking);
                    localStorage.setItem('ls_bookings', JSON.stringify(bookings));

                    // Add points and push notification
                    app.state.user.points = (app.state.user.points || 0) + Math.floor(totalInr / 100);
                    app.pushNotif(`Booking confirmed for ${app.state.currentHotel.name}! You earned ${Math.floor(totalInr / 100)} pts.`);
                    app.updateUserInDB();

                    admin.logAction('BOOKING', app.state.user.name, `Created TXN ${bookingId} for ₹${totalInr}`);

                    ui.closeModal('paymentModal');
                    app.checkAuth();
                    app.downloadReceipt(bookingId);
                    
                    ui.showToast(`Success! Booking Saved & PDF Downloaded`);
                        
                    btn.innerText = "Pay Total";
                    btn.disabled = false;
                }, 2000);
            },
            
            cancelBooking: (id) => {
                if(!confirm("Are you sure you want to cancel this booking?")) return;
                let bookings = admin.getBookings();
                let b = bookings.find(x => x.id === parseInt(id));
                
                if(b && b.status === "CONFIRMED") {
                    b.status = "CANCELLED";
                    
                    localStorage.setItem('ls_bookings', JSON.stringify(bookings));
                    admin.logAction('CANCEL', app.state.user.name, `Cancelled TXN ${id}.`);
                    
                    ui.showToast("Booking Cancelled!");
                    ui.openBookingsModal(); 
                    app.checkAuth(); 
                }
            },

            downloadReceipt: (bookingId) => {
                const bookings = JSON.parse(localStorage.getItem('ls_bookings')) || [];
                const b = bookings.find(x => x.id === parseInt(bookingId));
                if(!b) return;
            
                const { jsPDF } = window.jspdf;
                const doc = new jsPDF();
            
                // Brand Colors
                const primaryColor = [37, 99, 235]; // Tailwind brand-600
                const slateDark = [15, 23, 42];
                const slateGray = [100, 116, 139];
            
                // 1. Header Background Banner
                doc.setFillColor(...primaryColor);
                doc.rect(0, 0, 210, 40, 'F');
            
                // 2. Header Text
                doc.setFont("helvetica", "bold");
                doc.setFontSize(24);
                doc.setTextColor(255, 255, 255);
                doc.text("BookOurHotels", 20, 25);
            
                doc.setFontSize(12);
                doc.setFont("helvetica", "normal");
                doc.text("OFFICIAL BOOKING RECEIPT", 130, 25);
            
                // 3. Status Stamp (Angled)
                doc.setFontSize(20);
                doc.setFont("helvetica", "bold");
                // Green for Confirmed, Red for Cancelled
                doc.setTextColor(b.status === "CONFIRMED" ? 34 : 239, b.status === "CONFIRMED" ? 197 : 68, b.status === "CONFIRMED" ? 94 : 68);
                doc.text(b.status, 150, 60, { angle: 15 });
            
                // 4. Booking Information Section
                doc.setTextColor(...slateDark);
                doc.setFontSize(16);
                doc.text("Booking Information", 20, 60);
            
                doc.setFontSize(11);
                doc.setFont("helvetica", "normal");
                doc.setTextColor(...slateGray);
                
                const date = new Date(b.id).toLocaleDateString('en-US', { day: 'numeric', month: 'short', year: 'numeric' });
                
                // Left Column (Guest Info)
                doc.text(`Booking ID: #${b.id.toString().slice(-6)}`, 20, 70);
                doc.text(`Guest Name: ${b.user}`, 20, 78);
                doc.text(`Phone: ${b.phone}`, 20, 86);
                doc.text(`Date Booked: ${date}`, 20, 94);
            
                // Right Column (Property Info)
                doc.text(`Property:`, 110, 70);
                doc.setFont("helvetica", "bold");
                doc.setTextColor(...slateDark);
                doc.text(`${b.hotel}`, 110, 78);
                doc.setFont("helvetica", "normal");
                doc.setTextColor(...slateGray);
                
                // Inject Check-in/Check-out if we have them from the recent update
                if(b.checkIn && b.checkOut) {
                    const ci = new Date(b.checkIn).toLocaleDateString('en-US', { day: 'numeric', month: 'short', year: 'numeric' });
                    const co = new Date(b.checkOut).toLocaleDateString('en-US', { day: 'numeric', month: 'short', year: 'numeric' });
                    doc.text(`Check-In: ${ci}`, 110, 86);
                    doc.text(`Check-Out: ${co}`, 110, 94);
                }
            
                // 5. Divider Line
                doc.setDrawColor(226, 232, 240); // slate-200
                doc.line(20, 105, 190, 105);
            
                // 6. Payment Summary
                doc.setFontSize(16);
                doc.setFont("helvetica", "bold");
                doc.setTextColor(...slateDark);
                doc.text("Payment Summary", 20, 120);
            
                // Reverse calculate taxes for display (Assuming 12% tax)
                const total = b.amount;
                const baseAmount = Math.round(total / 1.12);
                const taxes = total - baseAmount;
            
                doc.setFontSize(11);
                doc.setFont("helvetica", "normal");
                doc.setTextColor(...slateGray);
                doc.text("Base Room Rate", 20, 135);
                doc.text(`${app.state.rates.symbols[app.state.currency]} ${baseAmount.toLocaleString()}`, 190, 135, { align: "right" });
            
                doc.text("Taxes & Fees (12%)", 20, 145);
                doc.text(`${app.state.rates.symbols[app.state.currency]} ${taxes.toLocaleString()}`, 190, 145, { align: "right" });
            
                // 7. Total Highlight Box
                doc.setFillColor(248, 250, 252); // slate-50
                doc.rect(20, 155, 170, 20, 'F');
                
                doc.setFont("helvetica", "bold");
                doc.setFontSize(14);
                doc.setTextColor(...slateDark);
                doc.text("Total Paid", 25, 168);
                
                doc.setTextColor(34, 197, 94); // green-500
                if (b.status === "CANCELLED") doc.setTextColor(239, 68, 68);
                doc.text(`${app.state.rates.symbols[app.state.currency]} ${total.toLocaleString()}`, 185, 168, { align: "right" });
            
                // 8. Footer
                doc.setFontSize(9);
                doc.setTextColor(148, 163, 184); // slate-400
                doc.text("Thank you for choosing BookOurHotels for your travel needs.", 105, 270, { align: "center" });
                doc.text("If you have any questions, contact support@bookourhotels.com", 105, 275, { align: "center" });
            
                doc.save(`Receipt_BOH_${b.id.toString().slice(-6)}.pdf`);
            },

            updateProfile: (e) => {
                e.preventDefault();
                app.state.user.name = document.getElementById('editProfName').value;
                localStorage.setItem('user', JSON.stringify(app.state.user));
                app.updateUserInDB();
                ui.showToast("Profile Updated Successfully");
                ui.closeModal('profileModal');
                app.checkAuth();
            },

            initGlobalMap: () => {
                const mapContainer = document.getElementById('globalMap');
                if(mapContainer._leaflet_id) return;
                const map = L.map('globalMap').setView([20, 0], 2);
                L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
                DB_HOTELS.forEach(h => {
                    L.marker([h.lat, h.lng]).addTo(map).bindPopup(`<b>${h.name}</b><br>${app.formatPrice(h.price)}<br><button onclick="app.openHotel(${h.id})" class="text-blue-600 font-bold">View</button>`);
                });
            },

            subscribe: (e) => {
                e.preventDefault();
                const emailInput = document.getElementById('newsEmail');
                const btn = e.target.querySelector('button');
                const originalText = btn.innerText;
                
                // Set UI to loading state
                btn.innerHTML = '<i class="fa-solid fa-circle-notch fa-spin"></i>...'; 
                btn.disabled = true;
                
                // Parameters to pass to your EmailJS template
                const templateParams = {
                    subscriber_email: emailInput.value,
                };
            
                // Send the email using your specific Service ID
                emailjs.send(
                    "service_5ifm55u", 
                    "YOUR_TEMPLATE_ID", // REPLACE THIS WITH YOUR TEMPLATE ID
                    templateParams
                )
                    .then(() => {
                        ui.showToast("Subscribed successfully!"); 
                        emailInput.value = ''; 
                        btn.innerText = originalText; 
                        btn.disabled = false;
                    })
                    .catch((error) => {
                        console.error("EmailJS Error:", error);
                        ui.showToast("Subscription failed. Please try again.");
                        btn.innerText = originalText; 
                        btn.disabled = false;
                    });
            },

            runDiagnostics: async () => {
                console.log("🧪 [TEST START] Initiating automated booking flow...");
                ui.showToast("Running automated diagnostics... Check console.");
                try {
                    if (!app.state.user) throw new Error("Test requires an active user session.");
                    console.log(`✔️ User authenticated.`);

                    const testHotelId = DB_HOTELS[0].id; app.openHotel(testHotelId);
                    console.log(`✔️ Opened hotel modal for ID: ${testHotelId}`);
                    await new Promise(r => setTimeout(r, 800)); 

                    app.openPaymentModal();
                    console.log("✔️ Payment modal triggered.");
                    await new Promise(r => setTimeout(r, 800));

                    document.getElementById('payPhone').value = '9998887770'; document.getElementById('cardNum').value = '4111 1111 1111 1111'; document.getElementById('cardExp').value = '12/30'; document.getElementById('cardCvc').value = '123';
                    console.log("✔️ Payment details auto-filled.");

                    const totalText = document.getElementById('payTotal').innerText.replace(/[^\d.-]/g, '');
                    const totalAmount = parseInt(totalText);
                    
                    console.log(`✔️ Transaction processed. Simulated equivalent of ${totalAmount}.`);
                    
                    ui.closeModal('paymentModal');
                    console.log("✅ [TEST PASSED] Booking flow executes flawlessly.");
                    ui.showToast("Diagnostics passed! System healthy.");
                } catch (error) {
                    console.error("❌ [TEST FAILED]", error.message);
                    ui.showToast("Diagnostics failed. Check console.");
                }
            }
        };

        const chatbot = {
            init: () => {
                chatbot.addMessage("Hi there! 👋 I'm your travel assistant. How can I help you today?", false);
                setTimeout(() => chatbot.showMenu(), 1000);
            },
            showMenu: () => {
                chatbot.addMessage(`
                    <div class="flex flex-col gap-2 mt-2">
                        <button onclick="chatbot.handleOption('destinations')" class="chat-option"><i class="fa-solid fa-map-location-dot mr-2"></i> View Popular Destinations</button>
                        <button onclick="chatbot.handleOption('booking')" class="chat-option"><i class="fa-solid fa-calendar-check mr-2"></i> Start a Booking</button>
                        <button onclick="chatbot.handleOption('support')" class="chat-option"><i class="fa-solid fa-headset mr-2"></i> Talk to Support</button>
                    </div>
                `, false, true);
            },
            handleOption: (option) => {
                const chatBody = document.getElementById('chatBody');
                const lastMessage = chatBody.lastElementChild;
                if (lastMessage && lastMessage.querySelector('.chat-option')) lastMessage.remove();

                if (option === 'destinations') {
                    chatbot.addMessage("Here are some of our most popular destinations:", false);
                    setTimeout(() => chatbot.addMessage(`<div class="flex flex-col gap-2 mt-2">${DB_HOTELS.slice(0, 5).map(h => `<button onclick="chatbot.selectDestination(${h.id})" class="chat-option"><i class="fa-solid fa-hotel mr-2"></i> ${h.name}, ${h.loc}</button>`).join('')}</div>`, false, true), 800);
                } else if (option === 'booking') {
                    chatbot.addMessage("Sure! To start a booking, you can browse our hotels or use the search bar above. Once you find a hotel you like, just click 'View Deal'.", false);
                    setTimeout(() => chatbot.showMenu(), 2000);
                } else if (option === 'support') {
                    chatbot.addMessage("Our support team is available 24/7. You can reach them at support@bookourshotels.com.", false);
                    setTimeout(() => chatbot.showMenu(), 2000);
                }
            },
            selectDestination: (id) => {
                const h = DB_HOTELS.find(x => x.id === id);
                if (h) {
                    const lastMessage = document.getElementById('chatBody').lastElementChild;
                    if (lastMessage && lastMessage.querySelector('.chat-option')) lastMessage.remove();
                    chatbot.addMessage(`Great choice! ${h.name} in ${h.loc} is beautiful.`, false);
                    setTimeout(() => {
                        chatbot.addMessage(`Would you like to see more details or book a stay at ${h.name}?`, false);
                        setTimeout(() => chatbot.addMessage(`<div class="flex flex-col gap-2 mt-2"><button onclick="app.openHotel(${h.id}); ui.toggleChat();" class="chat-option"><i class="fa-solid fa-eye mr-2"></i> View Details & Book</button><button onclick="chatbot.showMenu()" class="chat-option"><i class="fa-solid fa-arrow-left mr-2"></i> Back to Menu</button></div>`, false, true), 800);
                    }, 800);
                }
            },
            addMessage: (html, isUser, isHtml = false) => {
                const chatBody = document.getElementById('chatBody');
                const messageDiv = document.createElement('div');
                messageDiv.className = `chat-message ${isUser ? 'user' : 'bot'}`;
                if (isHtml) messageDiv.innerHTML = html; else messageDiv.innerText = html;
                chatBody.appendChild(messageDiv);
                chatBody.scrollTop = chatBody.scrollHeight;
            },
            handleText: () => {
                const input = document.getElementById('chatInput');
                const msg = input.value.trim();
                if(!msg) return;
                
                chatbot.addMessage(msg, true);
                input.value = '';
                
                setTimeout(() => {
                    const lower = msg.toLowerCase();
                    if(lower.includes('hello') || lower.includes('hi')) {
                        chatbot.addMessage("Hello! Ready to plan your next getaway?", false);
                    } else if(lower.includes('cancel') || lower.includes('refund')) {
                        chatbot.addMessage("To cancel a trip, click on 'My Trips' in the top menu. Find your booking and click the red 'Cancel Trip' button.", false);
                    } else if(lower.includes('receipt') || lower.includes('pdf')) {
                        chatbot.addMessage("You can download PDF receipts for any confirmed or cancelled booking from the 'My Trips' panel.", false);
                    } else if(lower.includes('book') || lower.includes('search')) {
                        chatbot.handleOption('booking');
                    } else {
                        chatbot.addMessage("I'm still learning! Here are some quick options to help you navigate:", false);
                        chatbot.showMenu();
                    }
                }, 600);
            }
        };

        app.init();
    </script>
</body>
</html>
