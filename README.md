<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Urban Cotton Co. - Enterprise Business Suite</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body { font-family: system-ui, -apple-system, sans-serif; background-color: #090d16; color: #f1f5f9; margin: 0; min-height: 100vh; display: flex; flex-direction: column; }
        .hidden { display: none !important; }
        .transition-smooth { transition: all 0.2s ease-in-out; }
        
        /* Typing Dots Animation */
        .typing-dot {
            animation: typingBounce 1.4s infinite ease-in-out both;
        }
        .typing-dot:nth-child(1) { animation-delay: -0.32s; }
        .typing-dot:nth-child(2) { animation-delay: -0.16s; }
        @keyframes typingBounce {
            0%, 80%, 100% { transform: scale(0); opacity: 0.3; }
            40% { transform: scale(1); opacity: 1; }
        }
    </style>
</head>
<body class="bg-[#090d16] text-slate-100 min-h-screen flex flex-col font-sans">

    <!-- Storage Restricted Warning Banner -->
    <div id="storage-warning-banner" class="bg-amber-500 text-slate-950 px-4 py-2 text-center text-xs font-bold hidden shadow-sm">
        <i class="fa-solid fa-triangle-exclamation mr-1"></i> Private Browsing / Restricted Storage Active. Persistence bound to session memory.
    </div>

    <!-- Header Navigation -->
    <header class="bg-slate-950/80 border-b border-slate-800 text-white shadow-lg sticky top-0 z-40 backdrop-blur-md">
        <div class="max-w-6xl mx-auto px-4 py-3.5 flex justify-between items-center">
            <h1 class="text-xl font-extrabold tracking-wide flex items-center gap-2.5 cursor-pointer" onclick="switchView('store')">
                <div class="w-8 h-8 rounded-xl bg-purple-600 flex items-center justify-center text-white shadow-md">
                    <i class="fa-solid fa-store text-sm"></i>
                </div>
                Urban Cotton Co.
            </h1>
            <nav class="flex items-center gap-2.5">
                <button onclick="switchView('store')" id="nav-store-btn" class="px-3.5 py-2 rounded-xl bg-purple-600 font-semibold text-xs transition-smooth shadow-sm hover:bg-purple-700">Store</button>
                <button onclick="switchView('cart')" id="nav-cart-btn" class="px-3.5 py-2 rounded-xl bg-slate-900 border border-slate-800 font-semibold text-xs transition-smooth hover:bg-slate-800 flex items-center gap-1.5">
                    <i class="fa-solid fa-cart-shopping text-purple-400"></i> Cart <span id="cart-badge" class="bg-purple-600 text-white text-[10px] px-1.5 py-0.5 rounded-full font-bold">0</span>
                </button>
                <button onclick="switchView('admin')" id="nav-admin-btn" class="px-3.5 py-2 rounded-xl bg-slate-900 border border-slate-800 font-semibold text-xs transition-smooth hover:bg-slate-800">Admin Manager</button>
            </nav>
        </div>
    </header>

    <!-- Main Workspace Container -->
    <main class="flex-grow max-w-6xl w-full mx-auto px-4 py-8">

        <!-- STOREFRONT VIEW -->
        <section id="view-store" class="space-y-8">
            <div class="bg-gradient-to-r from-purple-950 via-indigo-950 to-slate-950 border border-purple-900/50 text-white rounded-3xl p-8 md:p-10 shadow-2xl flex flex-col items-center text-center space-y-4 relative overflow-hidden">
                <div class="absolute -right-10 -bottom-10 w-48 h-48 bg-purple-600/10 rounded-full blur-3xl"></div>
                <h2 class="text-3xl md:text-5xl font-black tracking-tight">Urban Cotton Co.</h2>
                <p class="text-lg font-medium text-purple-300 italic">"Premium Menswear Bangladesh"</p>
                <p class="text-xs md:text-sm text-slate-300 max-w-xl leading-relaxed">Live Cloud Synchronized Storefront with Live Human Customer Support.</p>
            </div>

            <!-- Search Bar -->
            <div class="bg-slate-900/90 border border-slate-800 p-4 rounded-2xl shadow-sm space-y-3">
                <div class="relative">
                    <i class="fa-solid fa-search absolute left-4 top-3.5 text-slate-500"></i>
                    <input type="text" id="store-search-input" oninput="renderStorefront()" placeholder="Search products by name..." class="w-full pl-11 pr-4 py-3 bg-slate-950 border border-slate-800 rounded-xl text-xs md:text-sm text-slate-100 placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-purple-500 transition-smooth">
                </div>
            </div>

            <!-- Category Container -->
            <div id="store-category-sections" class="space-y-12"></div>
        </section>

        <!-- CART VIEW -->
        <section id="view-cart" class="hidden space-y-6 max-w-2xl mx-auto">
            <div class="bg-slate-900/90 border border-slate-800 p-6 md:p-8 rounded-3xl shadow-xl space-y-6">
                <h3 class="text-xl font-black text-slate-100 flex items-center gap-2.5 border-b border-slate-800 pb-4">
                    <i class="fa-solid fa-cart-shopping text-purple-400"></i> Shopping Cart & Checkout
                </h3>
                <div id="cart-items-container" class="space-y-3"></div>
                
                <div id="checkout-form-section" class="border-t border-slate-800 pt-6 space-y-5 hidden">
                    <h4 class="font-bold text-slate-200 text-sm flex items-center gap-2">
                        <i class="fa-solid fa-truck-fast text-purple-400"></i> Customer Shipping Details
                    </h4>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 text-xs">
                        <div>
                            <label class="block font-semibold text-slate-400 mb-1">Full Name *</label>
                            <input type="text" id="order-customer-name" maxlength="60" placeholder="e.g. Tanvir Ahmed" class="w-full px-3.5 py-2.5 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl focus:ring-2 focus:ring-purple-500 outline-none">
                        </div>
                        <div>
                            <label class="block font-semibold text-slate-400 mb-1">Phone Number *</label>
                            <input type="text" id="order-customer-phone" maxlength="20" placeholder="e.g. 01800000000" class="w-full px-3.5 py-2.5 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl focus:ring-2 focus:ring-purple-500 outline-none">
                        </div>
                        <div class="sm:col-span-2">
                            <label class="block font-semibold text-slate-400 mb-1">Delivery Zone *</label>
                            <select id="order-delivery-zone" onchange="calculateCheckoutSummary()" class="w-full px-3.5 py-2.5 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl focus:ring-2 focus:ring-purple-500 outline-none">
                                <option value="70">Inside Dhaka (৳70)</option>
                                <option value="100">Dhaka Suburbs (৳100)</option>
                                <option value="130">Outside Dhaka (৳130 - ৳200 Advance Req.)</option>
                            </select>
                        </div>
                        <div class="sm:col-span-2">
                            <label class="block font-semibold text-slate-400 mb-1">Full Delivery Address *</label>
                            <textarea id="order-customer-address" rows="2" maxlength="250" placeholder="e.g. House 12, Road 4, Mohammadpur, Dhaka" class="w-full px-3.5 py-2.5 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl focus:ring-2 focus:ring-purple-500 outline-none"></textarea>
                        </div>
                        <div class="sm:col-span-2">
                            <label class="block font-semibold text-slate-400 mb-1">Size / Color Notes (Optional)</label>
                            <input type="text" id="order-customer-notes" maxlength="150" placeholder="e.g. Size: L, Color: Maroon" class="w-full px-3.5 py-2.5 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl focus:ring-2 focus:ring-purple-500 outline-none">
                        </div>
                    </div>

                    <div class="space-y-2 pt-2 text-xs">
                        <label class="block font-semibold text-slate-400">Select Payment Method</label>
                        <div class="grid grid-cols-2 gap-3">
                            <label class="border border-purple-900/60 bg-purple-950/30 p-3 rounded-xl flex items-center gap-2.5 cursor-pointer font-semibold text-slate-200">
                                <input type="radio" name="payment_method" value="COD" checked class="text-purple-600 focus:ring-purple-500">
                                Cash on Delivery (COD)
                            </label>
                            <label class="border border-slate-800 p-3 rounded-xl flex items-center gap-2.5 cursor-pointer font-semibold text-slate-300 hover:bg-slate-950">
                                <input type="radio" name="payment_method" value="MFS" class="text-purple-600 focus:ring-purple-500">
                                bKash / Nagad Send Money
                            </label>
                        </div>
                    </div>

                    <div class="space-y-2.5 pt-3 text-xs bg-slate-950 p-4 rounded-2xl border border-slate-800">
                        <div class="flex justify-between text-slate-400"><span>Subtotal:</span> <span id="cart-subtotal" class="font-bold text-slate-100">৳0</span></div>
                        <div class="flex justify-between text-slate-400"><span>Delivery Charge:</span> <span id="cart-zone-fee" class="font-bold text-purple-400">৳70</span></div>
                        <div class="flex justify-between text-sm font-black text-slate-100 border-t border-slate-800 pt-2"><span>Grand Total:</span> <span id="cart-grand-total" class="text-purple-400">৳0</span></div>

                        <div class="pt-3">
                            <button onclick="checkoutCartWhatsApp()" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-3.5 px-4 rounded-2xl transition-smooth flex items-center justify-center gap-2 shadow-lg shadow-emerald-600/20 text-sm">
                                <i class="fa-brands fa-whatsapp text-base"></i> Confirm & Send Order to WhatsApp
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- ADMIN VIEW -->
        <section id="view-admin" class="hidden space-y-8">
            <div id="admin-lock" class="max-w-md mx-auto bg-slate-900/90 border border-slate-800 p-8 md:p-10 rounded-3xl shadow-xl text-center space-y-5">
                <div class="w-14 h-14 bg-purple-950 text-purple-400 rounded-2xl flex items-center justify-center mx-auto text-2xl shadow-inner border border-purple-900/50"><i class="fa-solid fa-lock"></i></div>
                <h3 class="text-xl font-black text-slate-100">Admin Authentication</h3>
                <input type="password" id="admin-passcode-input" onkeydown="handleAdminKey(event)" placeholder="Enter admin passcode..." class="w-full px-4 py-3 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl focus:ring-2 focus:ring-purple-500 outline-none text-center text-xs">
                <button onclick="verifyAdmin()" class="w-full bg-purple-600 text-white font-bold py-3 rounded-xl hover:bg-purple-700 transition-smooth shadow-md text-xs">Access Business Suite</button>
            </div>

            <div id="admin-content" class="hidden space-y-8">
                <!-- Analytics -->
                <div class="grid grid-cols-1 sm:grid-cols-4 gap-4">
                    <div class="bg-slate-900/90 border border-slate-800 p-6 rounded-3xl shadow-sm">
                        <p class="text-xs text-slate-400 uppercase font-bold tracking-wider">Total Chats</p>
                        <h4 id="stat-chats" class="text-3xl font-black text-slate-100 mt-1">0</h4>
                    </div>
                    <div class="bg-slate-900/90 border border-slate-800 p-6 rounded-3xl shadow-sm">
                        <p class="text-xs text-slate-400 uppercase font-bold tracking-wider">Total Orders</p>
                        <h4 id="stat-checkouts" class="text-3xl font-black text-slate-100 mt-1">0</h4>
                    </div>
                    <div class="bg-slate-900/90 border border-slate-800 p-6 rounded-3xl shadow-sm">
                        <p class="text-xs text-slate-400 uppercase font-bold tracking-wider">Tracked Customers</p>
                        <h4 id="stat-customers" class="text-3xl font-black text-purple-400 mt-1">0</h4>
                    </div>
                    <div class="bg-slate-900/90 border border-slate-800 p-6 rounded-3xl shadow-sm">
                        <p class="text-xs text-slate-400 uppercase font-bold tracking-wider">Categories</p>
                        <h4 id="stat-categories" class="text-3xl font-black text-indigo-400 mt-1">0</h4>
                    </div>
                </div>

                <!-- Category Manager -->
                <div class="bg-slate-900/90 border border-slate-800 p-6 md:p-8 rounded-3xl shadow-sm space-y-4">
                    <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-3">
                        <h3 class="text-lg font-black text-slate-100">Manage Categories</h3>
                        <div class="flex gap-2 w-full sm:w-auto">
                            <input type="text" id="new-category-name" placeholder="New category name..." class="px-3.5 py-2 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl text-xs flex-grow sm:w-48 outline-none">
                            <button onclick="createCategory()" class="bg-purple-600 text-white px-4 py-2 rounded-xl text-xs font-bold hover:bg-purple-700 transition-smooth">Add</button>
                        </div>
                    </div>
                    <div id="admin-category-pills" class="flex flex-wrap gap-2 pt-2"></div>
                </div>

                <!-- Products Table -->
                <div class="bg-slate-900/90 border border-slate-800 p-6 md:p-8 rounded-3xl shadow-sm space-y-6">
                    <div class="flex justify-between items-center">
                        <h3 class="text-lg font-black text-slate-100">Manage Products & Inventory</h3>
                        <button onclick="openProductModal(null)" class="bg-purple-600 text-white px-4 py-2.5 rounded-xl text-xs font-bold hover:bg-purple-700 transition-smooth shadow-md"><i class="fa-solid fa-plus mr-1"></i> Add Product</button>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse">
                            <thead><tr class="border-b border-slate-800 text-[11px] font-bold text-slate-400 uppercase tracking-wider"><th class="p-3">Product ID</th><th class="p-3">Name</th><th class="p-3">Category</th><th class="p-3">Price</th><th class="p-3">Stock & Specs</th><th class="p-3 text-right">Actions</th></tr></thead>
                            <tbody id="admin-product-table" class="divide-y divide-slate-800 text-xs"></tbody>
                        </table>
                    </div>
                </div>

                <!-- CRM Section -->
                <div class="bg-slate-900/90 border border-slate-800 p-6 md:p-8 rounded-3xl shadow-sm space-y-4">
                    <div class="flex justify-between items-center border-b border-slate-800 pb-3">
                        <h3 class="text-lg font-black text-slate-100 flex items-center gap-2.5">
                            <i class="fa-solid fa-users-viewfinder text-purple-400"></i> Customer Directory (CRM)
                        </h3>
                        <button onclick="clearCRMData()" class="text-xs text-rose-400 font-bold hover:underline">Clear Records</button>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="border-b border-slate-800 text-[11px] font-bold text-slate-400 uppercase tracking-wider">
                                    <th class="p-3">Session ID</th>
                                    <th class="p-3">Name</th>
                                    <th class="p-3">Address</th>
                                    <th class="p-3 text-center">Chats</th>
                                    <th class="p-3 text-center">Orders</th>
                                    <th class="p-3 text-right">Last Active</th>
                                </tr>
                            </thead>
                            <tbody id="admin-crm-table" class="divide-y divide-slate-800 text-xs"></tbody>
                        </table>
                    </div>
                </div>

                <!-- Backup & Export -->
                <div class="bg-slate-900/90 border border-slate-800 p-6 md:p-8 rounded-3xl shadow-sm space-y-4">
                    <h3 class="text-lg font-black text-slate-100">JSON Portability</h3>
                    <button onclick="exportStoreData()" class="bg-slate-800 text-slate-100 px-4 py-2.5 rounded-xl text-xs font-bold hover:bg-slate-700 transition-smooth">Export JSON Backup</button>
                </div>
            </div>
        </section>

    </main>

    <!-- LIVE HUMANLIKE SUPPORT CHAT MODAL -->
    <div id="ai-chat-modal" role="dialog" aria-modal="true" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-3 sm:p-4">
        <div class="bg-slate-900 border border-slate-800 w-full max-w-lg rounded-3xl shadow-2xl flex flex-col h-[85vh] overflow-hidden">
            <div class="bg-slate-950 border-b border-slate-800 text-white px-5 py-4 flex justify-between items-center shrink-0">
                <div class="flex items-center gap-3">
                    <div class="w-9 h-9 rounded-xl bg-purple-900 border border-purple-700 flex items-center justify-center text-white font-bold shadow-md">
                        <i class="fa-solid fa-user text-xs"></i>
                    </div>
                    <div>
                        <h4 class="font-bold text-sm">Urban Support Agent</h4>
                        <span id="chat-product-context" class="text-[10px] text-emerald-400 block font-mono">● Online | Customer Support</span>
                    </div>
                </div>
                <button onclick="closeAIChat()" class="text-slate-400 hover:text-white p-1.5 text-lg"><i class="fa-solid fa-xmark"></i></button>
            </div>
            
            <div id="chat-messages" class="flex-grow p-4 overflow-y-auto space-y-3 bg-slate-950 text-xs"></div>
            <div class="p-3 bg-slate-900 border-t border-slate-800 flex gap-2 shrink-0">
                <input type="text" id="expert-chat-input" onkeydown="handleChatKey(event)" placeholder="Type your message here..." class="flex-grow px-4 py-2.5 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl text-xs focus:outline-none focus:ring-2 focus:ring-purple-500">
                <button id="expert-chat-send-btn" onclick="sendExpertChatMessage()" class="bg-purple-600 hover:bg-purple-700 text-white px-4 py-2.5 rounded-xl text-xs font-bold transition-smooth shadow-md"><i class="fa-solid fa-paper-plane"></i></button>
            </div>
        </div>
    </div>

    <!-- Product Modal -->
    <div id="product-form-modal" role="dialog" aria-modal="true" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-800 w-full max-w-lg rounded-3xl shadow-2xl p-6 md:p-8 space-y-5 max-h-[92vh] overflow-y-auto">
            <div class="flex justify-between items-center border-b border-slate-800 pb-4"><h3 class="text-lg font-black text-slate-100">Manage Product Information</h3><button onclick="closeProductModal()" class="text-slate-400 hover:text-slate-200"><i class="fa-solid fa-xmark text-lg"></i></button></div>
            <input type="hidden" id="edit-product-id">
            <div class="space-y-4 text-xs">
                <div><label class="block font-semibold text-slate-400 mb-1">Product Name *</label><input type="text" id="prod-name" placeholder="e.g. Premium Cotton Polo" class="w-full px-3.5 py-2.5 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl outline-none focus:ring-2 focus:ring-purple-500"></div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block font-semibold text-slate-400 mb-1">Category *</label>
                        <select id="prod-category" class="w-full px-3.5 py-2.5 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl outline-none focus:ring-2 focus:ring-purple-500"></select>
                    </div>
                    <div><label class="block font-semibold text-slate-400 mb-1">Stock Status</label><select id="prod-stock" class="w-full px-3.5 py-2.5 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl outline-none focus:ring-2 focus:ring-purple-500"><option value="In Stock">In Stock</option><option value="Out of Stock">Out of Stock</option></select></div>
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div><label class="block font-semibold text-slate-400 mb-1">Price (BDT) *</label><input type="text" id="prod-price" placeholder="899" class="w-full px-3.5 py-2.5 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl outline-none focus:ring-2 focus:ring-purple-500"></div>
                    <div><label class="block font-semibold text-slate-400 mb-1">Available Sizes</label><input type="text" id="prod-sizes" placeholder="M, L, XL, XXL" class="w-full px-3.5 py-2.5 bg-slate-950 border border-slate-800 text-slate-100 rounded-xl outline-none focus:ring-2 focus:ring-purple-500"></div>
                </div>
            </div>
            <div class="flex justify-end gap-3 pt-2">
                <button onclick="closeProductModal()" class="px-4 py-2.5 border border-slate-800 text-slate-300 rounded-xl text-xs font-semibold hover:bg-slate-950 transition-smooth">Cancel</button>
                <button onclick="saveProduct()" class="px-5 py-2.5 bg-purple-600 text-white rounded-xl text-xs font-bold hover:bg-purple-700 transition-smooth shadow-md">Save Product</button>
            </div>
        </div>
    </div>

    <!-- MAIN LOGIC ENGINE -->
    <script>
        const GITHUB_RAW_URL = "https://raw.githubusercontent.com/syedahnafahmed624-max/ahnaf/refs/heads/main/products.json";
        const CLOUDFLARE_WORKER_URL = "https://dawn-bread-7e6e.syedahnafahmed624.workers.dev";

        const ADMIN_PASSCODE = "none other than the admin is allowed in this section @123";
        const SELLER_WHATSAPP = "8801911445566";

        let categories = ["polos", "t_shirts"];
        let products = [
            { id: "p1", name: "Classic Navy Polo", category: "polos", price: 899, stock: "In Stock", sizes: "M, L, XL" },
            { id: "p2", name: "Maroon Slim Fit Polo", category: "polos", price: 950, stock: "In Stock", sizes: "L, XL, XXL" },
            { id: "p3", name: "Urban Black Cotton T-Shirt", category: "t_shirts", price: 650, stock: "In Stock", sizes: "S, M, L, XL" }
        ];
        let analyticsStats = { chats: 0, checkouts: 0 };
        let customerCRM = {};
        let cart = [];
        let activeProductChatContext = null;
        let isChatProcessing = false;
        let currentCustomerSessionId = 'guest_' + Math.random().toString(36).substring(2, 7);

        window.addEventListener("DOMContentLoaded", async () => {
            testStorageAvailability();
            loadLocalState();
            await fetchGitHubInventory();
            trackSessionActivity();
            renderStorefront();
            updateCartBadge();
        });

        function testStorageAvailability() {
            try {
                localStorage.setItem("test_q", "1");
                localStorage.removeItem("test_q");
            } catch (e) {
                document.getElementById("storage-warning-banner").classList.remove("hidden");
            }
        }

        function trackSessionActivity() {
            if (!customerCRM[currentCustomerSessionId]) {
                customerCRM[currentCustomerSessionId] = { name: "Guest User", phone: currentCustomerSessionId, address: "Browsing Store", chats: 0, orders: 0, lastActive: Date.now() };
                saveToStorage();
            }
        }

        async function fetchGitHubInventory() {
            try {
                const response = await fetch(GITHUB_RAW_URL + "?cache_bust=" + Date.now());
                if (response.ok) {
                    const cloudData = await response.json();
                    if (cloudData) {
                        if (cloudData.products && cloudData.products.length > 0) products = cloudData.products;
                        if (cloudData.categories && cloudData.categories.length > 0) categories = cloudData.categories;
                        renderAdminCategories();
                        renderAdminProducts();
                    }
                }
            } catch (err) {
                console.warn("GitHub Raw fetch fallback triggered:", err);
            }
        }

        function saveToStorage() {
            try {
                localStorage.setItem("urban_crm", JSON.stringify(customerCRM));
                localStorage.setItem("urban_stats", JSON.stringify(analyticsStats));
                localStorage.setItem("urban_cart", JSON.stringify(cart));
            } catch (e) {}
        }

        function loadLocalState() {
            try {
                const c = localStorage.getItem("urban_crm");
                const s = localStorage.getItem("urban_stats");
                const ct = localStorage.getItem("urban_cart");
                if (c) customerCRM = JSON.parse(c);
                if (s) analyticsStats = JSON.parse(s);
                if (ct) cart = JSON.parse(ct);
            } catch (e) {}
        }

        function switchView(viewName) {
            document.getElementById('view-store').classList.add('hidden');
            document.getElementById('view-cart').classList.add('hidden');
            document.getElementById('view-admin').classList.add('hidden');

            document.getElementById('nav-store-btn').className = 'px-3.5 py-2 rounded-xl bg-slate-900 border border-slate-800 font-semibold text-xs hover:bg-slate-800';
            document.getElementById('nav-cart-btn').className = 'px-3.5 py-2 rounded-xl bg-slate-900 border border-slate-800 font-semibold text-xs hover:bg-slate-800 flex items-center gap-1.5';
            document.getElementById('nav-admin-btn').className = 'px-3.5 py-2 rounded-xl bg-slate-900 border border-slate-800 font-semibold text-xs hover:bg-slate-800';

            if (viewName === 'store') {
                document.getElementById('view-store').classList.remove('hidden');
                document.getElementById('nav-store-btn').className = 'px-3.5 py-2 rounded-xl bg-purple-600 font-semibold text-xs hover:bg-purple-700';
                renderStorefront();
            } else if (viewName === 'cart') {
                document.getElementById('view-cart').classList.remove('hidden');
                document.getElementById('nav-cart-btn').className = 'px-3.5 py-2 rounded-xl bg-purple-600 font-semibold text-xs hover:bg-purple-700 flex items-center gap-1.5';
                renderCartView();
            } else if (viewName === 'admin') {
                document.getElementById('view-admin').classList.remove('hidden');
                document.getElementById('nav-admin-btn').className = 'px-3.5 py-2 rounded-xl bg-purple-600 font-semibold text-xs hover:bg-purple-700';
            }
        }

        function renderStorefront() {
            const container = document.getElementById('store-category-sections');
            const searchVal = document.getElementById('store-search-input').value.toLowerCase();
            container.innerHTML = '';

            categories.forEach(cat => {
                const catProducts = products.filter(p => p.category === cat && p.name.toLowerCase().includes(searchVal));
                if (catProducts.length === 0 && searchVal) return;

                const section = document.createElement('div');
                section.className = 'space-y-4';
                
                const titleHeading = document.createElement('h3');
                titleHeading.className = 'text-xl font-black text-slate-100 uppercase tracking-wider border-b border-slate-800 pb-2 flex items-center gap-2';
                titleHeading.innerHTML = `<i class="fa-solid fa-tag text-purple-400 text-sm"></i> ${cat.replace('_', ' ')}`;
                section.appendChild(titleHeading);

                const grid = document.createElement('div');
                grid.className = 'grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6';

                if (catProducts.length === 0) {
                    grid.innerHTML = `<p class="text-xs text-slate-500 italic col-span-full">No products found.</p>`;
                } else {
                    catProducts.forEach(prod => {
                        const card = document.createElement('div');
                        card.className = 'bg-slate-900/90 border border-slate-800 rounded-3xl overflow-hidden shadow-lg flex flex-col justify-between hover:border-purple-900/60';
                        
                        card.innerHTML = `
                            <div class="p-5 space-y-2">
                                <div class="flex justify-between items-start">
                                    <h4 class="font-bold text-slate-100 text-sm leading-snug">${prod.name}</h4>
                                    <span class="text-[10px] font-bold px-2 py-0.5 rounded-full ${prod.stock === 'In Stock' ? 'bg-emerald-950 text-emerald-400 border border-emerald-900/50' : 'bg-rose-950 text-rose-400 border border-rose-900/50'}">${prod.stock}</span>
                                </div>
                                <p class="text-purple-400 font-black text-base">৳${prod.price}</p>
                                <p class="text-[11px] text-slate-400">Sizes: <strong class="text-slate-200">${prod.sizes || 'N/A'}</strong></p>
                            </div>
                            <div class="p-5 pt-0 grid grid-cols-2 gap-2.5">
                                <button onclick="openAIChat('${prod.id}')" class="bg-slate-800 hover:bg-slate-700 text-purple-300 border border-purple-900/40 font-bold py-2.5 px-3 rounded-xl text-xs flex items-center justify-center gap-1.5">
                                    <i class="fa-solid fa-comments"></i> Live Support
                                </button>
                                <button onclick="addToCart('${prod.id}')" class="bg-purple-600 hover:bg-purple-700 text-white font-bold py-2.5 px-3 rounded-xl text-xs flex items-center justify-center gap-1.5 shadow-md">
                                    <i class="fa-solid fa-cart-plus"></i> Add to Cart
                                </button>
                            </div>
                        `;
                        grid.appendChild(card);
                    });
                }
                section.appendChild(grid);
                container.appendChild(section);
            });
        }

        function addToCart(prodId) {
            const prod = products.find(p => p.id === prodId);
            if (!prod || prod.stock !== 'In Stock') return alert('Product is out of stock.');

            const existing = cart.find(item => item.id === prodId);
            if (existing) existing.qty += 1;
            else cart.push({ id: prod.id, name: prod.name, price: prod.price, qty: 1 });

            saveToStorage();
            updateCartBadge();
            alert(`Added "${prod.name}" to cart!`);
        }

        function updateCartQty(prodId, delta) {
            const item = cart.find(i => i.id === prodId);
            if (item) {
                item.qty += delta;
                if (item.qty <= 0) {
                    cart = cart.filter(i => i.id !== prodId);
                }
                saveToStorage();
                updateCartBadge();
                renderCartView();
            }
        }

        function removeCartItem(prodId) {
            cart = cart.filter(i => i.id !== prodId);
            saveToStorage();
            updateCartBadge();
            renderCartView();
        }

        function updateCartBadge() {
            document.getElementById('cart-badge').innerText = cart.reduce((acc, item) => acc + item.qty, 0);
        }

        function renderCartView() {
            const container = document.getElementById('cart-items-container');
            const formSection = document.getElementById('checkout-form-section');
            container.innerHTML = '';

            if (cart.length === 0) {
                container.innerHTML = `<p class="text-xs text-slate-500 italic py-4 text-center">Your shopping cart is empty.</p>`;
                formSection.classList.add('hidden');
                return;
            }

            formSection.classList.remove('hidden');
            cart.forEach(item => {
                const row = document.createElement('div');
                row.className = 'flex justify-between items-center bg-slate-950 p-3.5 rounded-2xl border border-slate-800 text-xs';
                row.innerHTML = `
                    <div class="space-y-0.5">
                        <h5 class="font-bold text-slate-100">${item.name}</h5>
                        <p class="text-slate-400">৳${item.price} × ${item.qty} = <strong class="text-purple-400">৳${item.price * item.qty}</strong></p>
                    </div>
                    <div class="flex items-center gap-2">
                        <button onclick="updateCartQty('${item.id}', -1)" class="w-7 h-7 bg-slate-900 border border-slate-800 rounded-lg text-slate-300 font-bold hover:bg-slate-800 flex items-center justify-center">-</button>
                        <span class="font-bold px-1">${item.qty}</span>
                        <button onclick="updateCartQty('${item.id}', 1)" class="w-7 h-7 bg-slate-900 border border-slate-800 rounded-lg text-slate-300 font-bold hover:bg-slate-800 flex items-center justify-center">+</button>
                        <button onclick="removeCartItem('${item.id}')" class="text-rose-400 hover:text-rose-300 ml-2 p-1"><i class="fa-solid fa-trash"></i></button>
                    </div>
                `;
                container.appendChild(row);
            });
            calculateCheckoutSummary();
        }

        function calculateCheckoutSummary() {
            const subtotal = cart.reduce((acc, i) => acc + (i.price * i.qty), 0);
            const zoneEl = document.getElementById('order-delivery-zone');
            const deliveryFee = zoneEl ? parseInt(zoneEl.value || "70") : 70;
            
            const subtotalEl = document.getElementById('cart-subtotal');
            const zoneFeeEl = document.getElementById('cart-zone-fee');
            const grandTotalEl = document.getElementById('cart-grand-total');

            if (subtotalEl) subtotalEl.innerText = `৳${subtotal}`;
            if (zoneFeeEl) zoneFeeEl.innerText = `৳${deliveryFee}`;
            if (grandTotalEl) grandTotalEl.innerText = `৳${subtotal + deliveryFee}`;
        }

        function checkoutCartWhatsApp() {
            const name = document.getElementById('order-customer-name').value.trim();
            const phone = document.getElementById('order-customer-phone').value.trim();
            const address = document.getElementById('order-customer-address').value.trim();
            const notes = document.getElementById('order-customer-notes').value.trim();
            const deliveryFee = parseInt(document.getElementById('order-delivery-zone').value || "70");

            if (!name || !phone || !address) return alert('Please fill in required shipping details.');

            analyticsStats.checkouts += 1;
            if (customerCRM[currentCustomerSessionId]) {
                customerCRM[currentCustomerSessionId].name = name;
                customerCRM[currentCustomerSessionId].phone = phone;
                customerCRM[currentCustomerSessionId].address = address;
                customerCRM[currentCustomerSessionId].orders += 1;
            }
            saveToStorage();

            const subtotal = cart.reduce((acc, i) => acc + (i.price * i.qty), 0);
            const itemsListText = cart.map(i => `• ${i.name} (Qty: ${i.qty})`).join('\n');
            const orderMsg = `*New Order - Urban Cotton Co.*\nName: ${name}\nPhone: ${phone}\nAddress: ${address}\nNotes: ${notes || 'None'}\n\n*Items:*\n${itemsListText}\n\n*Total:* ৳${subtotal + deliveryFee}`;

            window.open(`https://wa.me/${SELLER_WHATSAPP}?text=${encodeURIComponent(orderMsg)}`, '_blank');
        }

        // --- BACKEND PROXY INTEGRATION ---
        async function fetchGeminiAIResponse(userPrompt, productContext) {
            try {
                const response = await fetch(CLOUDFLARE_WORKER_URL, {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify({ prompt: userPrompt, context: products })
                });
                const data = await response.json();
                return data.reply;
            } catch (err) {
                return "Hey there! Thanks for reaching out to Urban Cotton Co. How can I help you today?";
            }
        }

        function openAIChat(prodId) {
            activeProductChatContext = prodId;
            const prod = products.find(p => p.id === prodId);
            document.getElementById('ai-chat-modal').classList.remove('hidden');
            
            const container = document.getElementById('chat-messages');
            container.innerHTML = '';
            appendChatMessage('assistant', `Hi! Thanks for checking out Urban Cotton Co. How can I help you with ${prod ? prod.name : 'our collection'} today?`);
        }

        function closeAIChat() {
            document.getElementById('ai-chat-modal').classList.add('hidden');
        }

        async function sendExpertChatMessage() {
            if (isChatProcessing) return;

            const inputEl = document.getElementById('expert-chat-input');
            const sendBtn = document.getElementById('expert-chat-send-btn');
            const messageText = inputEl.value.trim();
            if (!messageText) return;

            // Lock controls during flight
            isChatProcessing = true;
            inputEl.disabled = true;
            sendBtn.disabled = true;

            appendChatMessage('user', messageText);
            inputEl.value = '';

            analyticsStats.chats += 1;
            if (customerCRM[currentCustomerSessionId]) {
                customerCRM[currentCustomerSessionId].chats += 1;
                customerCRM[currentCustomerSessionId].lastActive = Date.now();
            }
            saveToStorage();

            const responsePromise = fetchGeminiAIResponse(messageText, activeProductChatContext);

            setTimeout(async () => {
                const typingId = appendTypingIndicator();
                const aiResponse = await responsePromise;
                updateChatMessage(typingId, aiResponse);

                // Unlock controls
                isChatProcessing = false;
                inputEl.disabled = false;
                sendBtn.disabled = false;
                inputEl.focus();
            }, 2000);
        }

        function appendChatMessage(sender, text) {
            const container = document.getElementById('chat-messages');
            const msgId = 'msg_' + Math.random().toString(36).substring(2, 7);
            const div = document.createElement('div');
            div.id = msgId;
            div.className = `flex ${sender === 'user' ? 'justify-end' : 'justify-start'}`;
            div.innerHTML = `<div class="max-w-[80%] px-4 py-3 rounded-2xl text-xs ${sender === 'user' ? 'bg-purple-600 text-white' : 'bg-slate-900 border border-slate-800 text-slate-200'}">${text}</div>`;
            container.appendChild(div);
            container.scrollTop = container.scrollHeight;
            return msgId;
        }

        function appendTypingIndicator() {
            const container = document.getElementById('chat-messages');
            const msgId = 'msg_' + Math.random().toString(36).substring(2, 7);
            const div = document.createElement('div');
            div.id = msgId;
            div.className = 'flex justify-start';
            div.innerHTML = `
                <div class="max-w-[80%] px-4 py-3 rounded-2xl text-xs bg-slate-900 border border-slate-800 text-slate-400 flex items-center gap-2">
                    <span class="font-medium text-slate-300">Typing</span>
                    <span class="flex items-center gap-1">
                        <span class="w-1.5 h-1.5 bg-purple-400 rounded-full typing-dot"></span>
                        <span class="w-1.5 h-1.5 bg-purple-400 rounded-full typing-dot"></span>
                        <span class="w-1.5 h-1.5 bg-purple-400 rounded-full typing-dot"></span>
                    </span>
                </div>
            `;
            container.appendChild(div);
            container.scrollTop = container.scrollHeight;
            return msgId;
        }

        function updateChatMessage(msgId, newText) {
            const msgEl = document.getElementById(msgId);
            if (msgEl) {
                msgEl.innerHTML = `<div class="max-w-[80%] px-4 py-3 rounded-2xl text-xs bg-slate-900 border border-slate-800 text-slate-200">${newText}</div>`;
                const container = document.getElementById('chat-messages');
                container.scrollTop = container.scrollHeight;
            }
        }

        function handleChatKey(e) {
            if (e.key === 'Enter') sendExpertChatMessage();
        }

        function handleAdminKey(e) {
            if (e.key === 'Enter') verifyAdmin();
        }

        function verifyAdmin() {
            if (document.getElementById('admin-passcode-input').value === ADMIN_PASSCODE) {
                document.getElementById('admin-lock').classList.add('hidden');
                document.getElementById('admin-content').classList.remove('hidden');
                renderAdminDashboard();
            } else alert('Invalid passcode.');
        }

        function renderAdminDashboard() {
            document.getElementById('stat-chats').innerText = analyticsStats.chats;
            document.getElementById('stat-checkouts').innerText = analyticsStats.checkouts;
            document.getElementById('stat-customers').innerText = Object.keys(customerCRM).length;
            document.getElementById('stat-categories').innerText = categories.length;

            renderAdminCategories();
            renderAdminProducts();
            renderAdminCRM();
        }

        function renderAdminCategories() {
            const container = document.getElementById('admin-category-pills');
            const selectEl = document.getElementById('prod-category');
            if (!container || !selectEl) return;

            container.innerHTML = '';
            selectEl.innerHTML = '';

            categories.forEach(cat => {
                const pill = document.createElement('span');
                pill.className = 'bg-slate-950 border border-slate-800 px-3 py-1.5 rounded-xl text-xs font-semibold text-slate-300 flex items-center gap-2';
                pill.innerHTML = `${cat} <button onclick="deleteCategory('${cat}')" class="text-rose-400 hover:text-rose-300"><i class="fa-solid fa-xmark"></i></button>`;
                container.appendChild(pill);

                const opt = document.createElement('option');
                opt.value = cat;
                opt.innerText = cat;
                selectEl.appendChild(opt);
            });
        }

        function createCategory() {
            const input = document.getElementById('new-category-name');
            const val = input.value.trim().toLowerCase().replace(/\s+/g, '_');
            if (!val || categories.includes(val)) return alert('Invalid or duplicate category.');
            categories.push(val);
            input.value = '';
            renderAdminCategories();
        }

        function deleteCategory(cat) {
            if (categories.length <= 1) return alert('At least one category is required.');
            categories = categories.filter(c => c !== cat);
            renderAdminCategories();
        }

        function renderAdminProducts() {
            const tbody = document.getElementById('admin-product-table');
            if (!tbody) return;
            tbody.innerHTML = '';

            products.forEach(prod => {
                const tr = document.createElement('tr');
                tr.className = 'border-b border-slate-800 hover:bg-slate-950/50';
                tr.innerHTML = `
                    <td class="p-3 font-mono text-slate-400">${prod.id}</td>
                    <td class="p-3 font-bold text-slate-100">${prod.name}</td>
                    <td class="p-3 text-purple-400">${prod.category}</td>
                    <td class="p-3">৳${prod.price}</td>
                    <td class="p-3"><span class="${prod.stock === 'In Stock' ? 'text-emerald-400' : 'text-rose-400'} font-semibold">${prod.stock}</span> (${prod.sizes || 'N/A'})</td>
                    <td class="p-3 text-right space-x-2">
                        <button onclick="openProductModal('${prod.id}')" class="text-purple-400 hover:underline">Edit</button>
                        <button onclick="deleteProduct('${prod.id}')" class="text-rose-400 hover:underline">Delete</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function openProductModal(prodId) {
            document.getElementById('product-form-modal').classList.remove('hidden');
            renderAdminCategories();
            if (prodId) {
                const prod = products.find(p => p.id === prodId);
                if (prod) {
                    document.getElementById('edit-product-id').value = prod.id;
                    document.getElementById('prod-name').value = prod.name;
                    document.getElementById('prod-category').value = prod.category;
                    document.getElementById('prod-price').value = prod.price;
                    document.getElementById('prod-stock').value = prod.stock;
                    document.getElementById('prod-sizes').value = prod.sizes || '';
                }
            } else {
                document.getElementById('edit-product-id').value = '';
                document.getElementById('prod-name').value = '';
                document.getElementById('prod-price').value = '';
                document.getElementById('prod-sizes').value = '';
                if (categories.length > 0) {
                    document.getElementById('prod-category').value = categories[0];
                }
            }
        }

        function closeProductModal() {
            document.getElementById('product-form-modal').classList.add('hidden');
        }

        function saveProduct() {
            const id = document.getElementById('edit-product-id').value || 'p_' + Math.random().toString(36).substring(2, 7);
            const name = document.getElementById('prod-name').value.trim();
            const category = document.getElementById('prod-category').value || (categories[0] || 'general');
            const price = parseFloat(document.getElementById('prod-price').value);
            const stock = document.getElementById('prod-stock').value;
            const sizes = document.getElementById('prod-sizes').value.trim();

            if (!name || isNaN(price)) return alert('Please enter valid product name and price.');

            const existingIndex = products.findIndex(p => p.id === id);
            if (existingIndex >= 0) {
                products[existingIndex] = { id, name, category, price, stock, sizes };
            } else {
                products.push({ id, name, category, price, stock, sizes });
            }

            closeProductModal();
            renderAdminProducts();
            renderStorefront();
        }

        function deleteProduct(prodId) {
            if (!confirm('Are you sure you want to delete this product?')) return;
            products = products.filter(p => p.id !== prodId);
            renderAdminProducts();
            renderStorefront();
        }

        function renderAdminCRM() {
            const tbody = document.getElementById('admin-crm-table');
            if (!tbody) return;
            tbody.innerHTML = '';

            Object.entries(customerCRM).forEach(([sid, data]) => {
                const tr = document.createElement('tr');
                tr.className = 'border-b border-slate-800';
                tr.innerHTML = `
                    <td class="p-3 font-mono text-slate-400">${sid}</td>
                    <td class="p-3 font-bold text-slate-100">${data.name}</td>
                    <td class="p-3 text-slate-400 truncate max-w-xs">${data.address}</td>
                    <td class="p-3 text-center text-purple-400 font-bold">${data.chats}</td>
                    <td class="p-3 text-center text-emerald-400 font-bold">${data.orders}</td>
                    <td class="p-3 text-right text-slate-400">${new Date(data.lastActive).toLocaleTimeString()}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        function clearCRMData() {
            if (!confirm('Clear all customer CRM logs?')) return;
            customerCRM = {};
            saveToStorage();
            renderAdminCRM();
        }

        function exportStoreData() {
            const blob = new Blob([JSON.stringify({ categories, products }, null, 2)], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `products.json`;
            a.click();
        }
    </script>
</body>
</html>
