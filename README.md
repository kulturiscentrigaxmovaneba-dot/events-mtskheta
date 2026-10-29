<!DOCTYPE html>
<html lang="ka">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EventHub Georgia - ღონისძიებები და სპექტაკლები</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Google Fonts for Georgian typography -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Georgian:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['"Noto Sans Georgian"', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Noto Sans Georgian', sans-serif;
            background-color: #f8fafc;
            color: #1e293b;
        }
        .glass-panel {
            background: rgba(255, 255, 255, 0.9);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
        }
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col">

    <!-- Toast Notification Container -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col gap-3 max-w-sm w-full px-4 pointer-events-none"></div>

    <!-- Header Navigation -->
    <header class="sticky top-0 z-40 glass-panel border-b border-slate-200/80 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-20">
                <!-- Logo -->
                <div class="flex items-center gap-3 cursor-pointer" onclick="switchView('public')">
                    <div class="w-11 h-11 rounded-2xl bg-gradient-to-tr from-blue-600 to-indigo-600 flex items-center justify-center text-white shadow-lg shadow-blue-500/25">
                        <i data-lucide="calendar" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <span class="text-xl font-bold bg-gradient-to-r from-blue-600 to-indigo-700 bg-clip-text text-transparent">EventHub</span>
                        <span class="block text-xs font-semibold text-slate-500 tracking-wider">GEORGIA</span>
                    </div>
                </div>

                <!-- Navigation Tabs -->
                <div class="hidden md:flex items-center gap-1 bg-slate-100 p-1.5 rounded-2xl border border-slate-200/80">
                    <button onclick="switchView('public')" id="nav-btn-public" class="px-4 py-2 rounded-xl text-sm font-bold transition-all bg-white text-blue-600 shadow-sm">
                        აფიშა
                    </button>
                    <button onclick="openAddEventModal(false)" class="px-4 py-2 rounded-xl text-sm font-semibold transition-all text-slate-600 hover:text-slate-900">
                        + დამატება
                    </button>
                    <button onclick="handleAdminTabClick()" id="nav-btn-admin" class="px-4 py-2 rounded-xl text-sm font-semibold transition-all text-slate-600 hover:text-slate-900 flex items-center gap-2">
                        <span>ადმინ პანელი</span>
                        <span id="pending-badge-nav" class="hidden bg-amber-500 text-white text-xs font-bold px-2 py-0.5 rounded-full">0</span>
                    </button>
                </div>

                <!-- Mobile Header Actions -->
                <div class="flex items-center gap-2 md:hidden">
                    <button onclick="openAddEventModal(false)" class="bg-blue-600 text-white p-2.5 rounded-xl shadow-md">
                        <i data-lucide="plus" class="w-5 h-5"></i>
                    </button>
                    <button onclick="handleAdminTabClick()" class="bg-slate-100 text-slate-700 p-2.5 rounded-xl border border-slate-200 relative">
                        <i data-lucide="shield-check" class="w-5 h-5 text-indigo-600"></i>
                        <span id="pending-badge-mobile" class="hidden absolute -top-1 -right-1 w-3 h-3 bg-amber-500 rounded-full border-2 border-white"></span>
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- PUBLIC VIEW SECTION -->
    <main id="public-view" class="flex-grow">
        <!-- Hero Section -->
        <section class="relative bg-slate-900 text-white py-14 sm:py-20 overflow-hidden">
            <div class="absolute inset-0 opacity-20">
                <img src="https://images.unsplash.com/photo-1514525253161-7a46d19cd819?auto=format&fit=crop&w=1600&q=80" alt="Background" class="w-full h-full object-cover">
            </div>
            <div class="absolute inset-0 bg-gradient-to-t from-slate-900 via-slate-900/60 to-transparent"></div>

            <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
                <span class="inline-block px-3.5 py-1 bg-blue-500/20 border border-blue-400/30 rounded-full text-blue-300 text-xs font-bold uppercase tracking-wider mb-4">
                    კულტურა • გართობა • ღონისძიებები
                </span>
                <h1 class="text-3xl sm:text-5xl font-extrabold tracking-tight mb-4 max-w-3xl mx-auto leading-tight">
                    აღმოაჩინე საინტერესო სპექტაკლები და ივენთები
                </h1>
                <p class="text-slate-300 text-sm sm:text-base mb-8 max-w-2xl mx-auto font-light">
                    იპოვე სპექტაკლები, კონცერტები, ფესტივალები და შეხვედრები მთელ საქართველოში.
                </p>

                <!-- Filter & Search Bar Box -->
                <div class="max-w-4xl mx-auto bg-white rounded-2xl p-3 shadow-2xl text-slate-800 flex flex-col md:flex-row gap-3 items-center">
                    <div class="relative flex-grow w-full">
                        <i data-lucide="search" class="w-5 h-5 absolute left-3.5 top-1/2 -translate-y-1/2 text-slate-400"></i>
                        <input type="text" id="search-input" oninput="renderPublicEvents()" placeholder="მოძებნე დასახელებით ან ადგილმდებარეობით..." class="w-full pl-11 pr-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 text-sm">
                    </div>

                    <div class="w-full md:w-56">
                        <select id="category-filter" onchange="renderPublicEvents()" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 text-sm cursor-pointer">
                            <option value="all">ყველა კატეგორია</option>
                            <option value="სპექტაკლი">სპექტაკლი</option>
                            <option value="კონცერტი">კონცერტი</option>
                            <option value="ფესტივალი">ფესტივალი</option>
                            <option value="ტრენინგი/სემინარი">ტრენინგი / სემინარი</option>
                            <option value="სხვა">სხვა</option>
                        </select>
                    </div>

                    <button onclick="resetFilters()" class="w-full md:w-auto px-5 py-3 bg-slate-100 hover:bg-slate-200 text-slate-700 font-semibold rounded-xl text-sm transition-colors flex items-center justify-center gap-2 whitespace-nowrap">
                        <i data-lucide="rotate-ccw" class="w-4 h-4"></i>
                        <span>გასუფთავება</span>
                    </button>
                </div>
            </div>
        </section>

        <!-- Events Grid Section -->
        <section class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
            <div class="flex items-center justify-between mb-8">
                <div>
                    <h2 class="text-2xl font-bold text-slate-900">მიმდინარე აფიშა</h2>
                    <p class="text-slate-500 text-xs sm:text-sm mt-0.5">დადასტურებული ღონისძიებების ჩამონათვალი</p>
                </div>
                <span id="events-count-badge" class="bg-blue-50 text-blue-700 font-semibold text-xs px-3 py-1.5 rounded-full border border-blue-200">
                    0 ღონისძიება
                </span>
            </div>

            <!-- Grid Container -->
            <div id="events-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8"></div>

            <!-- Empty State -->
            <div id="empty-events" class="hidden text-center py-16 bg-white rounded-3xl border border-dashed border-slate-300">
                <div class="w-16 h-16 bg-slate-100 rounded-full flex items-center justify-center mx-auto mb-4 text-slate-400">
                    <i data-lucide="calendar-x" class="w-8 h-8"></i>
                </div>
                <h3 class="text-lg font-bold text-slate-800 mb-1">ღონისძიება ვერ მოიძებნა</h3>
                <p class="text-slate-500 text-sm max-w-md mx-auto mb-6">მითითებული პარამეტრებით აფიშაში ჩანაწერები არ არის.</p>
                <button onclick="resetFilters()" class="inline-flex items-center gap-2 text-blue-600 font-semibold text-sm hover:underline">
                    ფილტრების გასუფთავება
                </button>
            </div>
        </section>
    </main>

    <!-- ADMIN PANEL VIEW SECTION -->
    <main id="admin-view" class="flex-grow hidden bg-slate-50 py-10">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <!-- Admin Banner Header -->
            <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-8 pb-6 border-b border-slate-200">
                <div>
                    <div class="flex items-center gap-2">
                        <span class="bg-indigo-100 text-indigo-800 text-xs font-bold px-2.5 py-1 rounded-md">ადმინისტრირება</span>
                        <h1 class="text-2xl sm:text-3xl font-extrabold text-slate-900">მართვის პანელი</h1>
                    </div>
                    <p class="text-slate-500 text-sm mt-1">მართეთ შემოსული განაცხადები და აქტიური აფიშა</p>
                </div>
                
                <div class="flex items-center gap-3">
                    <button onclick="openAddEventModal(true)" class="inline-flex items-center gap-2 bg-emerald-600 hover:bg-emerald-700 text-white px-4 py-2.5 rounded-xl font-medium text-sm transition-all shadow-sm">
                        <i data-lucide="plus-circle" class="w-4 h-4"></i>
                        <span>პირდაპირ დამატება</span>
                    </button>
                    <button onclick="switchView('public')" class="inline-flex items-center gap-2 bg-white hover:bg-slate-100 text-slate-700 px-4 py-2.5 rounded-xl font-medium text-sm border border-slate-200 transition-all">
                        <i data-lucide="arrow-left" class="w-4 h-4"></i>
                        <span>საიტზე დაბრუნება</span>
                    </button>
                </div>
            </div>

            <!-- Stats Bar -->
            <div class="grid grid-cols-1 sm:grid-cols-3 gap-6 mb-10">
                <div class="bg-white p-6 rounded-2xl border border-slate-200/80 shadow-sm flex items-center gap-4">
                    <div class="w-12 h-12 rounded-xl bg-emerald-50 border border-emerald-100 text-emerald-600 flex items-center justify-center">
                        <i data-lucide="check-circle" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <span class="block text-2xl font-bold text-slate-900" id="stat-approved">0</span>
                        <span class="text-xs font-medium text-slate-500">დადასტურებული (აქტიური)</span>
                    </div>
                </div>

                <div class="bg-white p-6 rounded-2xl border border-slate-200/80 shadow-sm flex items-center gap-4">
                    <div class="w-12 h-12 rounded-xl bg-amber-50 border border-amber-100 text-amber-600 flex items-center justify-center">
                        <i data-lucide="clock" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <span class="block text-2xl font-bold text-slate-900" id="stat-pending">0</span>
                        <span class="text-xs font-medium text-slate-500">განსახილველი განაცხადები</span>
                    </div>
                </div>

                <div class="bg-white p-6 rounded-2xl border border-slate-200/80 shadow-sm flex items-center gap-4">
                    <div class="w-12 h-12 rounded-xl bg-rose-50 border border-rose-100 text-rose-600 flex items-center justify-center">
                        <i data-lucide="x-circle" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <span class="block text-2xl font-bold text-slate-900" id="stat-rejected">0</span>
                        <span class="text-xs font-medium text-slate-500">უარყოფილი</span>
                    </div>
                </div>
            </div>

            <!-- Admin Subtabs -->
            <div class="flex border-b border-slate-200 mb-6 gap-6">
                <button onclick="setAdminTab('pending')" id="tab-btn-pending" class="pb-3 text-sm font-bold text-blue-600 border-b-2 border-blue-600 flex items-center gap-2">
                    <i data-lucide="inbox" class="w-4 h-4"></i>
                    <span>განაცხადები მოლოდინში</span>
                    <span id="tab-pending-count" class="bg-amber-100 text-amber-800 text-xs px-2 py-0.5 rounded-full font-semibold">0</span>
                </button>
                <button onclick="setAdminTab('approved')" id="tab-btn-approved" class="pb-3 text-sm font-semibold text-slate-500 hover:text-slate-800 flex items-center gap-2">
                    <i data-lucide="list-checks" class="w-4 h-4"></i>
                    <span>აქტიური აფიშა</span>
                </button>
            </div>

            <!-- Pending Section -->
            <div id="admin-pending-section" class="space-y-4">
                <div id="pending-list-container" class="space-y-4"></div>
                <div id="empty-pending-msg" class="hidden text-center py-12 bg-white rounded-2xl border border-slate-200">
                    <i data-lucide="check-check" class="w-10 h-10 text-emerald-500 mx-auto mb-3"></i>
                    <h3 class="text-base font-bold text-slate-800">ახალი განაცხადები არ არის</h3>
                    <p class="text-xs text-slate-500">ყველა შემოსული განაცხადი დამუშავებულია.</p>
                </div>
            </div>

            <!-- Approved Section -->
            <div id="admin-approved-section" class="hidden space-y-4">
                <div id="approved-list-container" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
            </div>
        </div>
    </main>

    <!-- Footer -->
    <footer class="bg-slate-900 text-slate-400 py-10 mt-auto border-t border-slate-800">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col md:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-2">
                <div class="w-8 h-8 rounded-lg bg-blue-600 flex items-center justify-center text-white">
                    <i data-lucide="calendar" class="w-5 h-5"></i>
                </div>
                <span class="text-white font-bold tracking-wider text-sm">EventHub GEORGIA</span>
            </div>
            <p class="text-xs text-slate-500">© 2026 EventHub Georgia. ყველა უფლება დაცულია.</p>
        </div>
    </footer>

    <!-- MODAL 1: ADD / SUBMIT EVENT -->
    <div id="add-modal" class="fixed inset-0 z-50 hidden bg-slate-900/60 backdrop-blur-sm flex items-center justify-center p-4 overflow-y-auto">
        <div class="bg-white w-full max-w-2xl rounded-3xl shadow-2xl overflow-hidden my-8">
            <div class="px-6 py-5 bg-slate-900 text-white flex justify-between items-center">
                <div>
                    <h3 id="modal-form-title" class="text-lg font-bold">ღონისძიების დამატება</h3>
                    <p class="text-xs text-slate-400 mt-0.5">შეავსეთ ფორმა ღონისძიების განსათავსებლად</p>
                </div>
                <button type="button" onclick="closeAddEventModal()" class="text-slate-400 hover:text-white p-1 rounded-lg">
                    <i data-lucide="x" class="w-6 h-6"></i>
                </button>
            </div>

            <form id="event-form" onsubmit="handleFormSubmit(event)" class="p-6 space-y-4">
                <input type="hidden" id="form-is-admin-direct" value="false">

                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="md:col-span-2">
                        <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">ღონისძიების დასახელება *</label>
                        <input type="text" id="form-title" required placeholder="მაგ: ჰამლეტი - რუსთაველის თეატრი" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-sm">
                    </div>

                    <div>
                        <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">კატეგორია *</label>
                        <select id="form-category" required class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-sm">
                            <option value="სპექტაკლი">სპექტაკლი</option>
                            <option value="კონცერტი">კონცერტი</option>
                            <option value="ფესტივალი">ფესტივალი</option>
                            <option value="ტრენინგი/სემინარი">ტრენინგი / სემინარი</option>
                            <option value="სხვა">სხვა</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">ფასი (₾ / უფასო) *</label>
                        <input type="text" id="form-price" required placeholder="მაგ: 25 ₾ ან უფასო" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-sm">
                    </div>

                    <div>
                        <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">თარიღი და დრო *</label>
                        <input type="datetime-local" id="form-date" required class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-sm">
                    </div>

                    <div>
                        <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">ადგილმდებარეობა *</label>
                        <input type="text" id="form-location" required placeholder="მაგ: თბილისი, რუსთაველის გამზ. 17" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-sm">
                    </div>

                    <div class="md:col-span-2">
                        <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">სურათის ბმული (არასავალდებულო)</label>
                        <input type="url" id="form-image" placeholder="https://images.unsplash.com/..." class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-sm">
                        <p class="text-[11px] text-slate-400 mt-1">თუ ცარიელს დატოვებთ, ავტომატურად აირჩევა სტანდარტული ფოტო</p>
                    </div>

                    <div>
                        <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">ორგანიზატორი *</label>
                        <input type="text" id="form-organizer" required placeholder="სახელი ან ორგანიზაცია" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-sm">
                    </div>

                    <div>
                        <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">საკონტაქტო ნომერი / ელ.ფოსტა *</label>
                        <input type="text" id="form-contact" required placeholder="599 XXXXXX ან info@mail.ge" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-sm">
                    </div>

                    <div class="md:col-span-2">
                        <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">დეტალური აღწერა *</label>
                        <textarea id="form-description" rows="3" required placeholder="დაწერეთ ღონისძიების დეტალები..." class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 outline-none text-sm"></textarea>
                    </div>
                </div>

                <div class="pt-4 border-t border-slate-100 flex justify-end gap-3">
                    <button type="button" onclick="closeAddEventModal()" class="px-5 py-2.5 bg-slate-100 hover:bg-slate-200 text-slate-700 text-sm font-semibold rounded-xl">
                        გაუქმება
                    </button>
                    <button type="submit" id="form-submit-btn" class="px-6 py-2.5 bg-blue-600 hover:bg-blue-700 text-white text-sm font-semibold rounded-xl shadow-md">
                        გაგზავნა
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL 2: EVENT DETAILS VIEW -->
    <div id="detail-modal" class="fixed inset-0 z-50 hidden bg-slate-900/60 backdrop-blur-sm flex items-center justify-center p-4 overflow-y-auto">
        <div class="bg-white w-full max-w-2xl rounded-3xl shadow-2xl overflow-hidden my-8">
            <div class="relative h-64 sm:h-80 bg-slate-800">
                <img id="detail-img" src="" alt="Cover" class="w-full h-full object-cover">
                <div class="absolute inset-0 bg-gradient-to-t from-slate-950 via-transparent to-black/40"></div>
                <button type="button" onclick="closeDetailModal()" class="absolute top-4 right-4 bg-slate-900/60 hover:bg-slate-900 text-white p-2 rounded-full backdrop-blur-md">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
                <div class="absolute bottom-4 left-6 right-6 text-white">
                    <span id="detail-category" class="inline-block bg-blue-600/90 text-white text-xs font-bold px-3 py-1 rounded-full mb-2"></span>
                    <h2 id="detail-title" class="text-2xl sm:text-3xl font-extrabold leading-tight"></h2>
                </div>
            </div>

            <div class="p-6 space-y-6">
                <div class="grid grid-cols-2 sm:grid-cols-3 gap-4 p-4 bg-slate-50 rounded-2xl border border-slate-100">
                    <div>
                        <span class="block text-xs font-semibold text-slate-400 uppercase">თარიღი</span>
                        <span id="detail-date" class="text-sm font-bold text-slate-800 mt-0.5 block"></span>
                    </div>
                    <div>
                        <span class="block text-xs font-semibold text-slate-400 uppercase">ადგილი</span>
                        <span id="detail-location" class="text-sm font-bold text-slate-800 mt-0.5 block"></span>
                    </div>
                    <div class="col-span-2 sm:col-span-1">
                        <span class="block text-xs font-semibold text-slate-400 uppercase">ფასი</span>
                        <span id="detail-price" class="text-sm font-bold text-emerald-600 mt-0.5 block"></span>
                    </div>
                </div>

                <div>
                    <h4 class="text-xs font-bold text-slate-400 uppercase tracking-wider mb-2">ღონისძიების შესახებ</h4>
                    <p id="detail-description" class="text-slate-600 text-sm leading-relaxed whitespace-pre-line"></p>
                </div>

                <div class="pt-4 border-t border-slate-100 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-2">
                    <div>
                        <span class="block text-xs text-slate-400">ორგანიზატორი:</span>
                        <span id="detail-organizer" class="text-sm font-bold text-slate-800"></span>
                    </div>
                    <div class="text-left sm:text-right">
                        <span class="block text-xs text-slate-400">კონტაქტი:</span>
                        <span id="detail-contact" class="text-sm font-semibold text-blue-600"></span>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL 3: ADMIN SECURITY PIN PASSWORD CHECK -->
    <div id="pin-modal" class="fixed inset-0 z-50 hidden bg-slate-900/70 backdrop-blur-md flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-sm rounded-3xl p-6 text-center shadow-2xl border border-slate-100">
            <div class="w-14 h-14 bg-indigo-50 border border-indigo-100 rounded-2xl flex items-center justify-center text-indigo-600 mx-auto mb-4">
                <i data-lucide="lock" class="w-7 h-7"></i>
            </div>
            <h3 class="text-xl font-bold text-slate-900 mb-1">ადმინისტრირება</h3>
            <p class="text-xs text-slate-500 mb-4">შეიყვანეთ ადმინისტრატორის PIN კოდი</p>

            <div class="p-2 mb-4 bg-indigo-50 border border-indigo-100 rounded-xl text-xs text-indigo-800 font-semibold">
                🔑 ნაგულისხმევი PIN: <span class="font-mono text-sm underline">1234</span>
            </div>

            <form onsubmit="verifyAdminPin(event)">
                <input type="password" id="pin-input" maxlength="8" required placeholder="****" class="w-full text-center text-2xl font-mono tracking-widest px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-indigo-500 outline-none mb-4">
                <div class="flex gap-2">
                    <button type="button" onclick="closePinModal()" class="w-1/2 py-2.5 bg-slate-100 text-slate-700 rounded-xl text-sm font-semibold hover:bg-slate-200">
                        გაუქმება
                    </button>
                    <button type="submit" class="w-1/2 py-2.5 bg-indigo-600 hover:bg-indigo-700 text-white rounded-xl text-sm font-semibold shadow-md">
                        შესვლა
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- JS LOGIC -->
    <script>
        const DEFAULT_IMAGE = "https://images.unsplash.com/photo-1514525253161-7a46d19cd819?auto=format&fit=crop&w=800&q=80";

        const INITIAL_EVENTS = [
            {
                id: 'ev-1',
                title: "სპექტაკლი „ჰამლეტი“",
                category: "სპექტაკლი",
                price: "25 ₾",
                date: "2026-11-15T19:00",
                location: "თბილისი, რუსთაველის თეატრი",
                image: "https://images.unsplash.com/photo-1507676184212-d03ab07a01bf?auto=format&fit=crop&w=800&q=80",
                organizer: "შოთა რუსთაველის სახელობის თეატრი",
                contact: "+995 32 2 93 11 06",
                description: "უილიამ შექსპირის ცნობილი ტრაგედია ახალი ინტერპრეტაციითა და გამორჩეული მსახიობების მონაწილეობით.",
                status: "approved"
            },
            {
                id: 'ev-2',
                title: "Black Sea Jazz Festival",
                category: "კონცერტი",
                price: "80 ₾",
                date: "2026-11-20T20:00",
                location: "ბათუმი, Black Sea Arena",
                image: "https://images.unsplash.com/photo-1511671782779-c97d3d27a1d4?auto=format&fit=crop&w=800&q=80",
                organizer: "Eastern Promotions",
                contact: "info@eastern.ge",
                description: "საერთაშორისო ჯაზ ფესტივალი მსოფლიო დონის ვარსკვლავების მონაწილეობით.",
                status: "approved"
            },
            {
                id: 'ev-pending-1',
                title: "ქართული როკ ჯგუფების კონცერტი",
                category: "კონცერტი",
                price: "15 ₾",
                date: "2026-11-25T19:30",
                location: "ქუთაისი, ცენტრალური პარკი",
                image: "https://images.unsplash.com/photo-1470225620780-dba8ba36b745?auto=format&fit=crop&w=800&q=80",
                organizer: "ახალგაზრდული კლუბი",
                contact: "599 12 34 56",
                description: "ადგილობრივი როკ ბენდების საქველმოქმედო ლაივ კონცერტი.",
                status: "pending"
            }
        ];

        let events = [];
        let isAdminAuthenticated = false;

        document.addEventListener('DOMContentLoaded', () => {
            loadEvents();
            renderPublicEvents();
            updateBadgesAndStats();
            lucide.createIcons();
        });

        function loadEvents() {
            try {
                const stored = localStorage.getItem('eventhub_events_db');
                if (stored) {
                    events = JSON.parse(stored);
                } else {
                    events = INITIAL_EVENTS;
                    saveEvents();
                }
            } catch (e) {
                events = INITIAL_EVENTS;
            }
        }

        function saveEvents() {
            try {
                localStorage.setItem('eventhub_events_db', JSON.stringify(events));
            } catch(e) {
                console.error("Storage error:", e);
            }
            updateBadgesAndStats();
        }

        function switchView(mode) {
            const publicView = document.getElementById('public-view');
            const adminView = document.getElementById('admin-view');
            const pubBtn = document.getElementById('nav-btn-public');
            const admBtn = document.getElementById('nav-btn-admin');

            if (mode === 'admin') {
                publicView.classList.add('hidden');
                adminView.classList.remove('hidden');
                pubBtn.className = "px-4 py-2 rounded-xl text-sm font-semibold transition-all text-slate-600 hover:text-slate-900";
                admBtn.className = "px-4 py-2 rounded-xl text-sm font-bold transition-all bg-white text-blue-600 shadow-sm flex items-center gap-2";
                renderAdminDashboard();
            } else {
                adminView.classList.add('hidden');
                publicView.classList.remove('hidden');
                pubBtn.className = "px-4 py-2 rounded-xl text-sm font-bold transition-all bg-white text-blue-600 shadow-sm";
                admBtn.className = "px-4 py-2 rounded-xl text-sm font-semibold transition-all text-slate-600 hover:text-slate-900 flex items-center gap-2";
                renderPublicEvents();
            }
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function handleAdminTabClick() {
            if (isAdminAuthenticated) {
                switchView('admin');
            } else {
                document.getElementById('pin-input').value = '';
                document.getElementById('pin-modal').classList.remove('hidden');
                setTimeout(() => document.getElementById('pin-input').focus(), 100);
            }
        }

        function closePinModal() {
            document.getElementById('pin-modal').classList.add('hidden');
        }

        function verifyAdminPin(e) {
            e.preventDefault();
            const pin = document.getElementById('pin-input').value.trim();
            if (pin === '1234') {
                isAdminAuthenticated = true;
                closePinModal();
                showToast('წარმატებული ავტორიზაცია!', 'success');
                switchView('admin');
            } else {
                showToast('არასწორი PIN კოდი!', 'error');
                document.getElementById('pin-input').value = '';
            }
        }

        function renderPublicEvents() {
            const grid = document.getElementById('events-grid');
            const emptyState = document.getElementById('empty-events');
            const countBadge = document.getElementById('events-count-badge');

            const searchQuery = (document.getElementById('search-input')?.value || '').toLowerCase().trim();
            const categoryQuery = document.getElementById('category-filter')?.value || 'all';

            const approvedEvents = events.filter(e => e.status === 'approved');

            const filtered = approvedEvents.filter(ev => {
                const matchesSearch = ev.title.toLowerCase().includes(searchQuery) || 
                                      ev.location.toLowerCase().includes(searchQuery);
                const matchesCat = categoryQuery === 'all' || ev.category === categoryQuery;
                return matchesSearch && matchesCat;
            });

            countBadge.innerText = `${filtered.length} ღონისძიება`;

            if (filtered.length === 0) {
                grid.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');
            grid.innerHTML = filtered.map(ev => `
                <div class="bg-white rounded-3xl overflow-hidden border border-slate-200/80 shadow-sm hover:shadow-xl transition-all duration-300 flex flex-col group">
                    <div class="relative h-52 overflow-hidden bg-slate-100">
                        <img src="${ev.image || DEFAULT_IMAGE}" alt="${ev.title}" class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500" onerror="this.src='${DEFAULT_IMAGE}'">
                        <span class="absolute top-4 left-4 bg-white/90 backdrop-blur-md text-slate-800 text-xs font-bold px-3 py-1 rounded-full shadow-sm">
                            ${ev.category}
                        </span>
                        <span class="absolute bottom-4 right-4 bg-slate-900/80 backdrop-blur-md text-emerald-400 text-xs font-extrabold px-3 py-1 rounded-full border border-emerald-500/30">
                            ${ev.price}
                        </span>
                    </div>
                    <div class="p-6 flex flex-col flex-grow justify-between">
                        <div>
                            <div class="flex items-center gap-2 text-xs font-semibold text-blue-600 mb-2">
                                <i data-lucide="calendar" class="w-4 h-4"></i>
                                <span>${formatDateGeorgian(ev.date)}</span>
                            </div>
                            <h3 class="text-lg font-bold text-slate-900 group-hover:text-blue-600 transition-colors line-clamp-1 mb-2">${ev.title}</h3>
                            <p class="text-xs text-slate-500 flex items-center gap-1.5 mb-4 line-clamp-1">
                                <i data-lucide="map-pin" class="w-3.5 h-3.5 text-slate-400"></i>
                                <span>${ev.location}</span>
                            </p>
                            <p class="text-slate-600 text-xs line-clamp-2 leading-relaxed mb-6">${ev.description}</p>
                        </div>
                        <button onclick="openDetailModal('${ev.id}')" class="w-full py-2.5 bg-slate-50 hover:bg-blue-50 text-slate-700 hover:text-blue-600 border border-slate-200 hover:border-blue-200 rounded-xl text-xs font-bold transition-all flex items-center justify-center gap-2">
                            <span>ვრცლად ნახვა</span>
                            <i data-lucide="chevron-right" class="w-4 h-4"></i>
                        </button>
                    </div>
                </div>
            `).join('');

            lucide.createIcons();
        }

        function resetFilters() {
            document.getElementById('search-input').value = '';
            document.getElementById('category-filter').value = 'all';
            renderPublicEvents();
        }

        function renderAdminDashboard() {
            updateBadgesAndStats();

            const pendingContainer = document.getElementById('pending-list-container');
            const approvedContainer = document.getElementById('approved-list-container');
            const emptyPending = document.getElementById('empty-pending-msg');

            const pendingEvents = events.filter(e => e.status === 'pending');
            const approvedEvents = events.filter(e => e.status === 'approved');

            if (pendingEvents.length === 0) {
                pendingContainer.innerHTML = '';
                emptyPending.classList.remove('hidden');
            } else {
                emptyPending.classList.add('hidden');
                pendingContainer.innerHTML = pendingEvents.map(ev => `
                    <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm flex flex-col lg:flex-row gap-5 justify-between items-start lg:items-center">
                        <div class="flex gap-4 items-start sm:items-center">
                            <img src="${ev.image || DEFAULT_IMAGE}" class="w-20 h-20 rounded-xl object-cover border border-slate-100 flex-shrink-0" onerror="this.src='${DEFAULT_IMAGE}'">
                            <div>
                                <div class="flex flex-wrap items-center gap-2 mb-1">
                                    <span class="bg-amber-100 text-amber-800 text-[11px] font-bold px-2 py-0.5 rounded">მოლოდინში</span>
                                    <span class="bg-slate-100 text-slate-700 text-[11px] font-semibold px-2 py-0.5 rounded">${ev.category}</span>
                                    <span class="text-xs text-slate-400">• ${formatDateGeorgian(ev.date)}</span>
                                </div>
                                <h4 class="text-base font-bold text-slate-900">${ev.title}</h4>
                                <p class="text-xs text-slate-500 mt-1">ორგანიზატორი: <b>${ev.organizer}</b> (${ev.contact})</p>
                                <p class="text-xs text-slate-600 mt-1 italic">"${ev.description}"</p>
                            </div>
                        </div>

                        <div class="flex items-center gap-2 w-full lg:w-auto pt-3 lg:pt-0 border-t lg:border-t-0 border-slate-100">
                            <button onclick="approveEvent('${ev.id}')" class="flex-1 lg:flex-none px-4 py-2 bg-emerald-600 hover:bg-emerald-700 text-white rounded-xl text-xs font-bold flex items-center justify-center gap-1.5 shadow-sm">
                                <i data-lucide="check" class="w-4 h-4"></i>
                                <span>დადასტურება</span>
                            </button>
                            <button onclick="rejectEvent('${ev.id}')" class="flex-1 lg:flex-none px-4 py-2 bg-rose-50 hover:bg-rose-100 text-rose-600 border border-rose-200 rounded-xl text-xs font-bold flex items-center justify-center gap-1.5">
                                <i data-lucide="x" class="w-4 h-4"></i>
                                <span>უარყოფა</span>
                            </button>
                        </div>
                    </div>
                `).join('');
            }

            approvedContainer.innerHTML = approvedEvents.map(ev => `
                <div class="bg-white p-4 rounded-2xl border border-slate-200 flex justify-between items-center gap-3">
                    <div class="flex items-center gap-3 overflow-hidden">
                        <img src="${ev.image || DEFAULT_IMAGE}" class="w-12 h-12 rounded-lg object-cover flex-shrink-0" onerror="this.src='${DEFAULT_IMAGE}'">
                        <div class="truncate">
                            <h5 class="text-sm font-bold text-slate-800 truncate">${ev.title}</h5>
                            <span class="text-xs text-slate-500">${formatDateGeorgian(ev.date)}</span>
                        </div>
                    </div>
                    <button onclick="deleteEvent('${ev.id}')" class="p-2 text-slate-400 hover:text-rose-600 hover:bg-rose-50 rounded-lg transition-colors flex-shrink-0" title="წაშლა">
                        <i data-lucide="trash-2" class="w-4 h-4"></i>
                    </button>
                </div>
            `).join('');

            lucide.createIcons();
        }

        function setAdminTab(tab) {
            const pendingSec = document.getElementById('admin-pending-section');
            const approvedSec = document.getElementById('admin-approved-section');
            const tabPendingBtn = document.getElementById('tab-btn-pending');
            const tabApprovedBtn = document.getElementById('tab-btn-approved');

            if (tab === 'pending') {
                pendingSec.classList.remove('hidden');
                approvedSec.classList.add('hidden');
                tabPendingBtn.className = "pb-3 text-sm font-bold text-blue-600 border-b-2 border-blue-600 flex items-center gap-2";
                tabApprovedBtn.className = "pb-3 text-sm font-semibold text-slate-500 hover:text-slate-800 flex items-center gap-2";
            } else {
                pendingSec.classList.add('hidden');
                approvedSec.classList.remove('hidden');
                tabApprovedBtn.className = "pb-3 text-sm font-bold text-blue-600 border-b-2 border-blue-600 flex items-center gap-2";
                tabPendingBtn.className = "pb-3 text-sm font-semibold text-slate-500 hover:text-slate-800 flex items-center gap-2";
            }
        }

        function approveEvent(id) {
            const ev = events.find(e => e.id === id);
            if (ev) {
                ev.status = 'approved';
                saveEvents();
                renderAdminDashboard();
                showToast(`„${ev.title}“ დადასტურდა!`, 'success');
            }
        }

        function rejectEvent(id) {
            const ev = events.find(e => e.id === id);
            if (ev) {
                ev.status = 'rejected';
                saveEvents();
                renderAdminDashboard();
                showToast('განაცხადი უარყოფილია', 'info');
            }
        }

        function deleteEvent(id) {
            events = events.filter(e => e.id !== id);
            saveEvents();
            renderAdminDashboard();
            renderPublicEvents();
            showToast('ღონისძიება წაიშალა', 'info');
        }

        function openAddEventModal(isAdminDirect = false) {
            document.getElementById('event-form').reset();
            document.getElementById('form-is-admin-direct').value = isAdminDirect ? "true" : "false";

            const titleElem = document.getElementById('modal-form-title');
            const submitBtn = document.getElementById('form-submit-btn');

            if (isAdminDirect) {
                titleElem.innerText = "პირდაპირ გამოქვეყნება (ადმინი)";
                submitBtn.innerText = "გამოქვეყნება";
            } else {
                titleElem.innerText = "ღონისძიების დამატების განაცხადი";
                submitBtn.innerText = "განაცხადის გაგზავნა";
            }

            document.getElementById('add-modal').classList.remove('hidden');
        }

        function closeAddEventModal() {
            document.getElementById('add-modal').classList.add('hidden');
        }

        function handleFormSubmit(e) {
            e.preventDefault();
            const isAdminDirect = document.getElementById('form-is-admin-direct').value === "true";

            const imgVal = document.getElementById('form-image').value.trim();

            const newEvent = {
                id: 'ev-' + Date.now(),
                title: document.getElementById('form-title').value.trim(),
                category: document.getElementById('form-category').value,
                price: document.getElementById('form-price').value.trim(),
                date: document.getElementById('form-date').value,
                location: document.getElementById('form-location').value.trim(),
                image: imgVal !== '' ? imgVal : DEFAULT_IMAGE,
                organizer: document.getElementById('form-organizer').value.trim(),
                contact: document.getElementById('form-contact').value.trim(),
                description: document.getElementById('form-description').value.trim(),
                status: isAdminDirect ? 'approved' : 'pending'
            };

            events.unshift(newEvent);
            saveEvents();
            closeAddEventModal();

            if (isAdminDirect) {
                showToast('ღონისძიება პირდაპირ გამოქვეყნდა!', 'success');
                renderAdminDashboard();
            } else {
                showToast('თქვენი განაცხადი გაიგზავნა განსახილველად!', 'success');
                renderPublicEvents();
            }
        }

        function openDetailModal(id) {
            const ev = events.find(e => e.id === id);
            if (!ev) return;

            document.getElementById('detail-img').src = ev.image || DEFAULT_IMAGE;
            document.getElementById('detail-category').innerText = ev.category;
            document.getElementById('detail-title').innerText = ev.title;
            document.getElementById('detail-date').innerText = formatDateGeorgian(ev.date);
            document.getElementById('detail-location').innerText = ev.location;
            document.getElementById('detail-price').innerText = ev.price;
            document.getElementById('detail-description').innerText = ev.description;
            document.getElementById('detail-organizer').innerText = ev.organizer;
            document.getElementById('detail-contact').innerText = ev.contact;

            document.getElementById('detail-modal').classList.remove('hidden');
            lucide.createIcons();
        }

        function closeDetailModal() {
            document.getElementById('detail-modal').classList.add('hidden');
        }

        function updateBadgesAndStats() {
            const pendingCount = events.filter(e => e.status === 'pending').length;
            const approvedCount = events.filter(e => e.status === 'approved').length;
            const rejectedCount = events.filter(e => e.status === 'rejected').length;

            const pendingNav = document.getElementById('pending-badge-nav');
            const pendingMob = document.getElementById('pending-badge-mobile');
            const tabPendingCount = document.getElementById('tab-pending-count');

            if (pendingCount > 0) {
                if (pendingNav) { pendingNav.innerText = pendingCount; pendingNav.classList.remove('hidden'); }
                if (pendingMob) pendingMob.classList.remove('hidden');
            } else {
                if (pendingNav) pendingNav.classList.add('hidden');
                if (pendingMob) pendingMob.classList.add('hidden');
            }

            if (tabPendingCount) tabPendingCount.innerText = pendingCount;

            document.getElementById('stat-approved').innerText = approvedCount;
            document.getElementById('stat-pending').innerText = pendingCount;
            document.getElementById('stat-rejected').innerText = rejectedCount;
        }

        function formatDateGeorgian(dateTimeStr) {
            if (!dateTimeStr) return '';
            try {
                const dateObj = new Date(dateTimeStr);
                const monthsKa = ['იანვარი', 'თებერვალი', 'მარტი', 'აპრილი', 'მაისი', 'ივნისი', 'ივლისი', 'აგვისტო', 'სექტემბერი', 'ოქტომბერი', 'ნოემბერი', 'დეკემბერი'];
                const day = dateObj.getDate();
                const month = monthsKa[dateObj.getMonth()];
                const year = dateObj.getFullYear();
                const hours = String(dateObj.getHours()).padStart(2, '0');
                const minutes = String(dateObj.getMinutes()).padStart(2, '0');

                return `${day} ${month}, ${year} • ${hours}:${minutes}`;
            } catch (e) {
                return dateTimeStr;
            }
        }

        function showToast(message, type = 'info') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            
            let bgClass = "bg-slate-900 text-white";
            let icon = "info";

            if (type === 'success') {
                bgClass = "bg-emerald-600 text-white";
                icon = "check-circle";
            } else if (type === 'error') {
                bgClass = "bg-rose-600 text-white";
                icon = "alert-circle";
            }

            toast.className = `pointer-events-auto p-4 rounded-2xl shadow-xl flex items-center gap-3 transition-all duration-300 transform translate-y-2 opacity-0 text-sm font-medium ${bgClass}`;
            toast.innerHTML = `
                <i data-lucide="${icon}" class="w-5 h-5 flex-shrink-0"></i>
                <span class="flex-grow">${message}</span>
            `;

            container.appendChild(toast);
            lucide.createIcons();

            setTimeout(() => {
                toast.classList.remove('translate-y-2', 'opacity-0');
            }, 10);

            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-2');
                setTimeout(() => toast.remove(), 300);
            }, 3500);
        }
    </script>
</body>
</html>
