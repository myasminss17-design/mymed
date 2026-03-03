<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MyMed | Gestão Médica Inteligente</title>
    
    <!-- Tailwind CSS (Via CDN para prototipagem rápida) -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Ícones (FontAwesome) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Fonte (Inter) -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">

    <!-- Configurações do Tailwind -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#f0f9ff',
                            100: '#e0f2fe',
                            500: '#0ea5e9', // Azul Tech
                            600: '#0284c7',
                            900: '#0c4a6e',
                        },
                        health: {
                            500: '#10b981', // Verde Saúde
                            600: '#059669',
                        }
                    }
                }
            }
        }
    </script>

    <style>
        /* Estilos Customizados */
        body { background-color: #f8fafc; }
        .sidebar-link { transition: all 0.2s; }
        .sidebar-link:hover, .sidebar-link.active { background-color: #f1f5f9; color: #0284c7; border-right: 3px solid #0284c7; }
        
        /* Scrollbar fina */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 3px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }

        /* Animação de entrada */
        .fade-in { animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body class="text-slate-800 antialiased h-screen flex overflow-hidden">

    <!-- LGPD MODAL (Simulação) -->
    <div id="lgpd-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm hidden">
        <div class="bg-white rounded-xl shadow-2xl p-8 max-w-md w-full mx-4 border border-slate-100">
            <div class="flex items-center gap-3 mb-4 text-brand-600">
                <i class="fa-solid fa-shield-halved text-2xl"></i>
                <h2 class="text-xl font-bold">LGPD & Segurança</h2>
            </div>
            <p class="text-sm text-slate-600 mb-6">
                O MyMed utiliza criptografia de ponta a ponta para proteger seus dados e de seus pacientes. Ao acessar, você concorda com nossa Política de Privacidade e Termos de Uso.
            </p>
            <div class="flex gap-3">
                <button onclick="acceptLGPD()" class="flex-1 bg-brand-600 hover:bg-brand-500 text-white font-medium py-2 px-4 rounded-lg transition">
                    Aceitar e Entrar
                </button>
            </div>
        </div>
    </div>

    <!-- SIDEBAR -->
    <aside class="w-64 bg-white border-r border-slate-200 flex flex-col justify-between hidden md:flex z-10">
        <div>
            <!-- Logo -->
            <div class="h-16 flex items-center px-6 border-b border-slate-100">
                <div class="flex items-center gap-2 text-brand-600 font-bold text-xl">
                    <i class="fa-solid fa-heart-pulse"></i> MyMed
                </div>
            </div>

            <!-- Menu -->
            <nav class="p-4 space-y-1">
                <a href="#" onclick="navigate('dashboard')" id="nav-dashboard" class="sidebar-link active flex items-center gap-3 px-4 py-3 text-slate-600 rounded-lg font-medium">
                    <i class="fa-solid fa-chart-pie w-5"></i> Dashboard
                </a>
                <a href="#" onclick="navigate('agenda')" id="nav-agenda" class="sidebar-link flex items-center gap-3 px-4 py-3 text-slate-600 rounded-lg font-medium">
                    <i class="fa-solid fa-calendar-check w-5"></i> Agenda Inteligente
                </a>
                <a href="#" onclick="navigate('pacientes')" id="nav-pacientes" class="sidebar-link flex items-center gap-3 px-4 py-3 text-slate-600 rounded-lg font-medium">
                    <i class="fa-solid fa-users w-5"></i> Pacientes
                </a>
                <a href="#" onclick="navigate('financeiro')" id="nav-financeiro" class="sidebar-link flex items-center gap-3 px-4 py-3 text-slate-600 rounded-lg font-medium">
                    <i class="fa-solid fa-wallet w-5"></i> Financeiro
                </a>
                <a href="#" onclick="navigate('configuracoes')" id="nav-configuracoes" class="sidebar-link flex items-center gap-3 px-4 py-3 text-slate-600 rounded-lg font-medium">
                    <i class="fa-solid fa-gear w-5"></i> Configurações
                </a>
            </nav>
        </div>

        <!-- User Profile -->
        <div class="p-4 border-t border-slate-100">
            <div class="flex items-center gap-3">
                <img src="https://ui-avatars.com/api/?name=Dr+Silva&background=0ea5e9&color=fff" class="w-10 h-10 rounded-full">
                <div>
                    <p class="text-sm font-semibold">Dr. Silva</p>
                    <p class="text-xs text-slate-500">Cardiologista</p>
                </div>
            </div>
        </div>
    </aside>

    <!-- MAIN CONTENT -->
    <main class="flex-1 flex flex-col h-screen overflow-hidden relative">
        
        <!-- Top Navbar -->
        <header class="h-16 bg-white border-b border-slate-200 flex items-center justify-between px-6 z-10">
            <div class="flex items-center gap-4">
                <button class="md:hidden text-slate-500"><i class="fa-solid fa-bars text-xl"></i></button>
                <div class="relative hidden sm:block">
                    <i class="fa-solid fa-search absolute left-3 top-1/2 -translate-y-1/2 text-slate-400"></i>
                    <input type="text" placeholder="Buscar paciente, CID ou receita..." class="pl-10 pr-4 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm focus:outline-none focus:ring-2 focus:ring-brand-500 w-64">
                </div>
            </div>
            
            <div class="flex items-center gap-4">
                <button class="relative p-2 text-slate-500 hover:bg-slate-50 rounded-full">
                    <i class="fa-regular fa-bell"></i>
                    <span class="absolute top-1 right-1 w-2 h-2 bg-red-500 rounded-full"></span>
                </button>
                <button onclick="window.open('termos.html', '_blank')" class="text-xs text-slate-400 hover:text-brand-600">Termos & LGPD</button>
            </div>
        </header>

        <!-- Dynamic Content Area -->
        <div id="content-area" class="flex-1 overflow-y-auto p-6 bg-slate-50">
            
            <!-- VIEW: DASHBOARD -->
            <div id="view-dashboard" class="fade-in">
                <div class="flex justify-between items-end mb-6">
                    <div>
                        <h1 class="text-2xl font-bold text-slate-800">Olá, Dr. Silva 👋</h1>
                        <p class="text-slate-500">Aqui está o resumo do seu dia.</p>
                    </div>
                    <button class="bg-brand-600 hover:bg-brand-500 text-white px-4 py-2 rounded-lg text-sm font-medium shadow-sm transition">
                        <i class="fa-solid fa-plus mr-2"></i> Nova Consulta
                    </button>
                </div>

                <!-- KPI Cards -->
                <div class="grid grid-cols-1 md:grid-cols-4 gap-6 mb-8">
                    <div class="bg-white p-5 rounded-xl border border-slate-200 shadow-sm">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs font-semibold text-slate-500 uppercase">Consultas Hoje</p>
                                <h3 class="text-2xl font-bold text-slate-800 mt-1">12</h3>
                            </div>
                            <div class="p-2 bg-blue-50 text-brand-600 rounded-lg"><i class="fa-solid fa-user-doctor"></i></div>
                        </div>
                    </div>
                    <div class="bg-white p-5 rounded-xl border border-slate-200 shadow-sm">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs font-semibold text-slate-500 uppercase">Receita Mensal</p>
                                <h3 class="text-2xl font-bold text-slate-800 mt-1">R$ 12.450</h3>
                            </div>
                            <div class="p-2 bg-green-50 text-health-600 rounded-lg"><i class="fa-solid fa-arrow
