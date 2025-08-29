<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>엑셈 채용 브랜딩 대시보드</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Noto Sans KR', sans-serif;
        }
        .chart-container {
            position: relative;
            height: 250px;
            width: 100%;
        }
        /* Task & List Styles */
        .task-list-item {
            transition: background-color 0.2s ease;
        }
        .task-status {
            cursor: pointer;
            transition: all 0.2s ease-in-out;
        }
        .task-status:hover {
            transform: scale(1.05);
        }
        .task-status[data-status="대기"] { background-color: #e5e7eb; color: #4b5563; }
        .task-status[data-status="진행중"] { background-color: #dbeafe; color: #1d4ed8; }
        .task-status[data-status="완료"] { background-color: #dcfce7; color: #166534; }
        /* Phase Card Styles */
        .phase-card {
            transition: all 0.3s ease;
        }
        .phase-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
        }
        /* Animation Styles */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        @keyframes fadeOut {
            from { opacity: 1; transform: translateY(0); }
            to { opacity: 0; transform: translateY(10px); }
        }
        .animate-fadeIn { animation: fadeIn 0.4s ease-out forwards; }
        .animate-fadeOut { animation: fadeOut 0.4s ease-out forwards; }

        /* New Task Bar Styles */
        .task-bar {
            position: absolute;
            height: 2.5rem; /* 40px */
            padding: 0.5rem; /* 8px */
            border-radius: 0.5rem; /* 8px */
            display: flex;
            align-items: center;
            justify-content: space-between;
            font-size: 0.875rem; /* 14px, Increased for readability */
            color: white;
            overflow: hidden;
            white-space: nowrap;
            text-overflow: ellipsis;
            transition: all 0.3s ease;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        .task-bar-title {
            overflow: hidden;
            white-space: nowrap;
            text-overflow: ellipsis;
            margin-right: 8px;
        }
        .task-bar[data-status="대기"] { background-color: #9ca3af; }
        .task-bar[data-status="진행중"] { background-color: #6366f1; }
        .task-bar[data-status="완료"] { background-color: #22c55e; }
        
        /* Modal Styles */
        .modal-overlay {
            transition: opacity 0.3s ease;
        }
        .modal-content {
            transition: transform 0.3s ease, opacity 0.3s ease;
        }
        .modal-overlay.hidden .modal-content {
            transform: scale(0.95);
            opacity: 0;
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-800">
    <div class="max-w-full mx-auto">
        <!-- Header -->
        <div class="relative w-full py-24 sm:py-32 text-center text-white overflow-hidden">
            <img src="https://www.ex-em.com/resources/image/sub/intro_top.png" alt="엑셈 오피스 배경 이미지" class="absolute inset-0 w-full h-full object-cover z-0">
            <div class="absolute inset-0 bg-black/50 z-10"></div>
            <header class="relative z-20">
                <h1 class="text-4xl md:text-5xl font-bold mb-4" style="text-shadow: 2px 2px 4px rgba(0,0,0,0.5);">EVP 개발 및 직원 인터뷰 실행 계획</h1>
                <p class="max-w-3xl mx-auto text-base md:text-lg text-slate-200" style="text-shadow: 1px 1px 3px rgba(0,0,0,0.5);">
                    EX팀 채용 브랜딩 프로젝트
                </p>
            </header>
        </div>

        <div class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
            <!-- Main Grid -->
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                
                <!-- Left Column: View Switcher -->
                <div class="lg:col-span-2 bg-white rounded-xl shadow-md p-6 relative overflow-hidden">
                    <!-- Summary Section -->
                    <div id="summary-section">
                        <!-- Content will be injected by JS -->
                    </div>

                    <!-- Overview Section -->
                    <div id="overview-section">
                        <!-- Content will be injected by JS -->
                    </div>

                    <!-- Calendar Section (Initially Hidden) -->
                    <div id="calendar-section" class="hidden">
                        <button id="back-to-overview" class="mb-4 text-sm font-semibold text-indigo-600 hover:text-indigo-800">
                            &larr; 전체 개요로 돌아가기
                        </button>
                        <div id="calendar-title" class="text-center mb-4"></div>
                        <div id="calendar-container" class="relative">
                            <div id="calendar-grid" class="grid grid-cols-7 gap-1"></div>
                            <div id="task-bar-container" class="absolute top-0 left-0 w-full h-full"></div>
                        </div>
                    </div>
                </div>

                <!-- Right Column: Keywords & Interviewees -->
                <div class="space-y-6">
                    <!-- EVP Keywords -->
                    <div class="bg-white rounded-xl p-6 shadow-md">
                        <h3 class="text-lg font-bold mb-4 text-indigo-600">💡 EVP 키워드</h3>
                        <div class="flex flex-wrap gap-3">
                            <span class="bg-indigo-100 text-indigo-800 text-md font-semibold px-4 py-2 rounded-full">#Philinnovator</span>
                            <span class="bg-indigo-100 text-indigo-800 text-md font-semibold px-4 py-2 rounded-full">#성장</span>
                            <span class="bg-indigo-100 text-indigo-800 text-md font-semibold px-4 py-2 rounded-full">#연결</span>
                            <span class="bg-indigo-100 text-indigo-800 text-md font-semibold px-4 py-2 rounded-full">#소통</span>
                        </div>
                    </div>

                    <!-- Interviewees -->
                    <div class="bg-white rounded-xl p-6 shadow-md">
                        <h3 class="text-lg font-bold mb-4 text-indigo-600">🎤 인터뷰 대상자 현황</h3>
                        <div class="space-y-3" id="interviewee-list">
                            <!-- Content will be injected by JS -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Custom Tooltip -->
    <div id="tooltip" class="hidden absolute bg-gray-800 text-white text-xs rounded-md px-2 py-1 shadow-lg z-50 transition-opacity duration-200"></div>

    <!-- Branding Modal -->
    <div id="branding-modal" class="modal-overlay hidden fixed inset-0 bg-black bg-opacity-60 flex items-center justify-center z-50 p-4">
        <div class="modal-content bg-white rounded-lg shadow-xl w-11/12 max-w-6xl relative">
            <div class="p-4 border-b flex justify-between items-center">
                <h2 class="text-xl font-bold text-gray-800">채용 브랜딩 전략</h2>
                <button id="close-modal-btn" class="text-gray-500 hover:text-gray-800 text-3xl font-light">&times;</button>
            </div>
            <div id="modal-content-body" class="w-full h-[80vh] overflow-y-auto">
                <iframe id="branding-iframe" class="w-full h-full border-0"></iframe>
            </div>
        </div>
    </div>


    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const statuses = ["대기", "진행중", "완료"];
            
            const tasksByPhase = {
                phase1: [ 
                    { week: 1, title: '경영진 / 직책자 / 실무자 설문 조사 진행 → 엑셈 고유 가치와 연결' },
                    { week: 2, title: '설문 결과 분석 + 타 기업 EVP 벤치마킹 → EVP 차별화' },
                    { week: 3, title: '엑셈 핵심 가치와 연결된 EVP 초안 작성 (3~5개 키 메시지)' },
                    { week: 4, title: '경영진 및 직책자 피드백 → 최종 EVP 확정' },
                ],
                phase2: [ 
                    { week: 1, title: '인터뷰 대상 선정 및 일정 조율 → 직군, 경력, 신입 등 콘텐츠 선정' },
                    { week: 2, title: 'EVP와 매칭되는 질문 리스트 작성 및 인터뷰 가이드북 제작' },
                    { week: 3, title: '인터뷰 진행 및 초안 리뷰' },
                    { week: 4, title: '인터뷰 내용 편집 및 피드백 반영 후 추가 인터뷰 진행' },
                ],
                phase3: [ 
                    { week: 1, title: '채용 홈페이지에 EVP 섹션 개편 (인터뷰 및 직원 스토리 추가)' },
                    { week: 2, title: '사람인, 잡코리아, 원티드 등 모든 채용 채널 및 공고에 콘텐츠 업로드' },
                    { week: 3, title: '지원자 수, 공고 클릭 수, 면접 전환율 비교' },
                    { week: 4, title: '성과 분석 및 피드백 반영' },
                ]
            };
            
            let allTasks = [];

            const interviewees = [
                { team: '통합개발본부', name: '홍길동', role: '신입', date: '2025-10-06' },
                { team: 'DB기술본부', name: '김철수', role: '경력', date: '2025-10-07' },
                { team: '경영관리본부', name: '김영희', role: '경력', date: '협의 중' },
                { team: '영업본부', name: '김엑셈', role: '팀장', date: '협의 중' },
            ];

            const summarySection = document.getElementById('summary-section');
            const overviewSection = document.getElementById('overview-section');
            const calendarSection = document.getElementById('calendar-section');
            const backBtn = document.getElementById('back-to-overview');
            const calendarTitle = document.getElementById('calendar-title');
            const calendarGrid = document.getElementById('calendar-grid');
            const taskBarContainer = document.getElementById('task-bar-container');
            const tooltip = document.getElementById('tooltip');
            
            // Modal elements
            const brandingBtn = document.getElementById('branding-btn');
            const brandingModal = document.getElementById('branding-modal');
            const closeModalBtn = document.getElementById('close-modal-btn');

            const phaseDetails = {
                phase1: { month: 8, title: "1단계 : EVP 조사 및 초안 도출 → 메시지 방향성 확정", monthName: "9월" },
                phase2: { month: 9, title: "2단계 : EVP 전달을 위한 직원 인터뷰 컨텐츠 제작", monthName: "10월" },
                phase3: { month: 10, title: "3단계 : EVP 및 인터뷰 콘텐츠 채용 채널 배포 → 성과 측정 및 피드백 반영", monthName: "11월" }
            };

            const brandingGuideHtml = `
                <!DOCTYPE html>
                <html lang="ko">
                <head>
                    <meta charset="UTF-8">
                    <meta name="viewport" content="width=device-width, initial-scale=1.0">
                    <title>채용 브랜딩 전략</title>
                    <script src="https://cdn.tailwindcss.com"><\/script>
                    <link rel="preconnect" href="https://fonts.googleapis.com">
                    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
                    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700&display=swap" rel="stylesheet">
                    <style>
                        body {
                            font-family: 'Noto Sans KR', sans-serif;
                        }
                        .card {
                            transition: transform 0.3s ease, box-shadow 0.3s ease;
                        }
                        .card:hover {
                            transform: translateY(-5px);
                            box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);
                        }
                    <\/style>
                </head>
                <body class="bg-slate-50 text-slate-800">

                    <!-- Header Section with Background Image -->
                    <div class="relative w-full py-24 sm:py-32 text-center text-white overflow-hidden">
                        <img src="https://www.ex-em.com/resources/image/sub/intro_top.png" alt="엑셈 오피스 배경 이미지" class="absolute inset-0 w-full h-full object-cover z-0">
                        <div class="absolute inset-0 bg-black/50 z-10"></div>
                        <header class="relative z-20">
                            <h1 class="text-4xl md:text-5xl font-bold mb-4" style="text-shadow: 2px 2px 4px rgba(0,0,0,0.5);">채용 브랜딩 전략</h1>
                            <p class="max-w-3xl mx-auto text-base md:text-lg text-slate-200" style="text-shadow: 1px 1px 3px rgba(0,0,0,0.5);">
                                "빠른 변화 속에서도 <strong>함께 성장</strong>하고, <strong>소통</strong>하며, 의미 있는 <strong>경험</strong>을 만드는 여정"
                            </p>
                        </header>
                    </div>

                    <div class="container mx-auto p-4 sm:p-6 lg:p-8">
                        <!-- EVP Development Section -->
                        <section class="mb-12">
                            <h2 class="text-2xl md:text-3xl font-bold text-center mb-8 text-slate-900">EVP 개발</h2>
                            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 text-center">
                                <!-- 소통 (Communication) Card -->
                                <div class="bg-white p-6 rounded-xl shadow-md border border-slate-200/80 hover:shadow-lg transition-shadow">
                                    <div class="flex justify-center mb-4">
                                        <div class="bg-indigo-100 text-indigo-600 rounded-full p-4">
                                            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                                                <path stroke-linecap="round" stroke-linejoin="round" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z" />
                                            </svg>
                                        </div>
                                    </div>
                                    <h3 class="text-xl font-bold text-slate-800 mb-2">소통</h3>
                                    <p class="text-slate-600 text-sm">엑셈 특유의 자유롭고<br>수평적인 소통 문화</p>
                                </div>
                                <!-- 성장 (Growth) Card -->
                                <div class="bg-white p-6 rounded-xl shadow-md border border-slate-200/80 hover:shadow-lg transition-shadow">
                                    <div class="flex justify-center mb-4">
                                        <div class="bg-green-100 text-green-600 rounded-full p-4">
                                            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                                                <path stroke-linecap="round" stroke-linejoin="round" d="M2.25 18L9 11.25l4.306 4.307a11.95 11.95 0 015.814-5.519l2.74-1.22m0 0l-3.75-.625m3.75.625l-6.25 3.75" />
                                            </svg>
                                        </div>
                                    </div>
                                    <h3 class="text-xl font-bold text-slate-800 mb-2">성장</h3>
                                    <p class="text-slate-600 text-sm">직원들과의 연결을 통해<br>자연스럽게 성장하는 환경</p>
                                </div>
                                <!-- 경험 (Experience) Card -->
                                <div class="bg-white p-6 rounded-xl shadow-md border border-slate-200/80 hover:shadow-lg transition-shadow">
                                    <div class="flex justify-center mb-4">
                                        <div class="bg-amber-100 text-amber-600 rounded-full p-4">
                                            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                                                <path stroke-linecap="round" stroke-linejoin="round" d="M11.48 3.499a.562.562 0 011.04 0l2.125 5.111a.563.563 0 00.475.345l5.518.442c.499.04.701.663.321.988l-4.204 3.602a.563.563 0 00-.182.557l1.285 5.385a.562.562 0 01-.84.61l-4.725-2.885a.563.563 0 00-.586 0L6.982 20.54a.562.562 0 01-.84-.61l1.285-5.386a.562.562 0 00-.182-.557l-4.204-3.602a.563.563 0 01.321-.988l5.518-.442a.563.563 0 00.475-.345L11.48 3.5z" />
                                            </svg>
                                        </div>
                                    </div>
                                    <h3 class="text-xl font-bold text-slate-800 mb-2">경험</h3>
                                    <p class="text-slate-600 text-sm">소통과 성장의 경험으로<br>고객 가치를 창출하는 여정</p>
                                </div>
                            </div>
                        </section>

                        <!-- Process Grid -->
                        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                            
                            <!-- 1. 진단 -->
                            <div class="card bg-white rounded-xl shadow-lg p-6 border border-slate-200/80 flex flex-col">
                                <div class="flex items-start justify-between mb-4">
                                    <h2 class="text-xl font-bold text-indigo-700">1. 진단</h2>
                                    <span class="text-3xl font-bold text-slate-200">01</span>
                                </div>
                                <div class="text-sm text-slate-600 space-y-4 flex-grow">
                                    <div>
                                        <div class="flex items-center gap-2">
                                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" /></svg>
                                            <h4 class="font-bold text-indigo-600">목적</h4>
                                        </div>
                                        <p class="pl-7 pt-1">직원들이 경험한 회사의 장점과 개선점을 객관적으로 진단</p>
                                    </div>
                                    <div>
                                        <div class="flex items-center gap-2">
                                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01" /></svg>
                                            <h4 class="font-bold text-indigo-600">방법</h4>
                                        </div>
                                        <ul class="pl-7 pt-1 list-['–_'] list-outside ml-2">
                                            <li>재직자·신입사원·퇴직자 인터뷰/설문</li>
                                            <li>채용·온보딩 과정 피드백 분석</li>
                                        </ul>
                                    </div>
                                    <div>
                                        <div class="flex items-center gap-2">
                                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M11 3.055A9.001 9.001 0 1020.945 13H11V3.055z" /><path stroke-linecap="round" stroke-linejoin="round" d="M20.488 9H15V3.512A9.025 9.025 0 0120.488 9z" /></svg>
                                            <h4 class="font-bold text-indigo-600">결과</h4>
                                        </div>
                                        <div class="pl-7 pt-1">
                                            <p><strong class="text-slate-700">장점:</strong> 능력 있는 동료, 자유로운 소통, 카페·식당, 고가의 사무가구 등 복리후생</p>
                                            <p><strong class="text-slate-700">개선점:</strong> 최근 복지 축소, 워라밸 저하</p>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <!-- 2. 연결 -->
                            <div class="card bg-white rounded-xl shadow-lg p-6 border border-slate-200/80 flex flex-col">
                                <div class="flex items-start justify-between mb-4">
                                    <h2 class="text-xl font-bold text-indigo-700">2. 연결</h2>
                                    <span class="text-3xl font-bold text-slate-200">02</span>
                                </div>
                                <div class="text-sm text-slate-600 space-y-3 flex-grow">
                                    <div>
                                        <div class="flex items-center gap-2">
                                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1" /></svg>
                                            <h4 class="font-bold text-indigo-600">목적</h4>
                                        </div>
                                        <ul class="pl-7 pt-1 list-['–_'] list-outside ml-2">
                                            <li>장점은 EVP 핵심 메시지로 직접 연결</li>
                                            <li>개선점은 EVP의 ‘개선 근거’로 활용하여 신뢰성과 실행 의지를 강조
                                                <span class="block text-xs text-slate-500 mt-1">(예: 워라밸 저하의 직원들을 위해 주 1회 재택근무 파일럿 테스트)</span>
                                            </li>
                                        </ul>
                                    </div>
                                    <div>
                                        <div class="flex items-center gap-2">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M11 4a2 2 0 114 0v1a1 1 0 001 1h3a1 1 0 011 1v3a1 1 0 01-1 1h-1a2 2 0 100 4h1a1 1 0 011 1v3a1 1 0 01-1 1h-3a1 1 0 01-1-1v-1a2 2 0 10-4 0v1a1 1 0 01-1 1H7a1 1 0 01-1-1v-3a1 1 0 00-1-1H4a2 2 0 110-4h1a1 1 0 001-1V7a1 1 0 011-1h3a1 1 0 001-1V4z" /></svg>
                                            <h4 class="font-bold text-indigo-600">연결 방식</h4>
                                        </div>
                                        <ul class="pl-7 pt-1 list-['–_'] list-outside ml-2">
                                            <li><strong>소통:</strong> 오픈미팅, 커피챗 문화 반영 / 정기 타운홀 미팅 운영</li>
                                            <li><strong>성장:</strong> 동료와의 협업 사례 공유 / 집중 프로젝트를 성장 기회로 전환</li>
                                            <li><strong>경험:</strong> 직원 경험을 통한 스토리텔링 / 실제 성장 사례</li>
                                        </ul>
                                    </div>
                                </div>
                                <blockquote class="mt-4 p-3 bg-indigo-50 border-l-4 border-indigo-500 text-indigo-800 rounded-r-lg text-center">
                                    <p class="text-base font-bold text-indigo-700">엑셈뿐입니다.</p>
                                    <p class="mt-1 text-xs leading-relaxed">
                                        최고의 동료와 함께 몰입하고, 성장하며,<br>
                                        소중한 경험을 설계할 수 있는 곳은.
                                    </p>
                                </blockquote>
                            </div>

                            <!-- 3. 기획 -->
                            <div class="card bg-white rounded-xl shadow-lg p-6 border border-slate-200/80 flex flex-col">
                                <div class="flex items-start justify-between mb-4">
                                    <h2 class="text-xl font-bold text-indigo-700">3. 기획</h2>
                                    <span class="text-3xl font-bold text-slate-200">03</span>
                                </div>
                                <div class="text-sm text-slate-600 space-y-4 flex-grow">
                                    <div>
                                        <div class="flex items-center gap-2">
                                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z" /></svg>
                                            <h4 class="font-bold text-indigo-600">목적</h4>
                                        </div>
                                        <p class="pl-7 pt-1">도출한 EVP를 조직 내·외부에 효과적으로 적용할 수 있도록 구체적인 실행 계획 수립</p>
                                    </div>
                                    <div>
                                        <div class="flex items-center gap-2">
                                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M4 6h16M4 10h16M4 14h16M4 18h16" /></svg>
                                            <h4 class="font-bold text-indigo-600">단계</h4>
                                        </div>
                                        <ul class="pl-7 pt-1 list-['–_'] list-outside ml-2 space-y-2 text-xs">
                                            <li><strong>세부 메시지 설계:</strong> EVP 키워드별 대표 사례·스토리·데이터 확보 / 직무·대상별 맞춤형 메시지 제작</li>
                                            <li><strong>적용 계획 수립:</strong> 채용공고, 온보딩 자료 등에 EVP 반영 / 사내 커뮤니케이션 채널 활용</li>
                                            <li><strong>콘텐츠 제작 기획:</strong> 영상, 카드뉴스, 인터뷰 등 콘텐츠 기획 / EVP 관련 캠페인 설계</li>
                                            <li><strong>성과 측정 계획:</strong> 지원율, 만족도, 인지도 등 지표 선정 / 분기별·연간 성과 리뷰</li>
                                        </ul>
                                    </div>
                                </div>
                            </div>

                            <!-- Conclusion/Summary Section -->
                            <div class="card bg-indigo-600 text-white rounded-xl shadow-lg p-6 border border-indigo-700 lg:col-span-3">
                                <div class="text-center">
                                    <p class="text-sm md:text-base font-medium leading-relaxed">
                                        소통, 성장, 경험이라는 EVP를 연결한 메시지를 채용 전 과정에 적용해 채용 브랜딩 활용
                                    </p>
                                    <p class="mt-3 text-xs md:text-sm font-light leading-relaxed">
                                        EX팀은 채용 효율성과 지원자 경험을 극대화하며, 조직 경쟁력 강화라는 직접적인 성과를 창출하고, 장기적으로 회사의 성장과 조직 문화 정착에 기여한다.
                                    </p>
                                </div>
                            </div>

                            <!-- 4. 인터뷰 기획 -->
                            <div class="card bg-white rounded-xl shadow-lg p-6 border border-slate-200/80 lg:col-span-3 flex flex-col">
                                <div class="flex items-start justify-between mb-4">
                                    <h2 class="text-xl font-bold text-indigo-700">인터뷰 기획</h2>
                                </div>
                                <div class="text-sm text-slate-600 space-y-4 flex-grow grid grid-cols-1 md:grid-cols-2 gap-x-8">
                                    
                                    <div class="space-y-4">
                                        <div>
                                            <div class="flex items-center gap-2">
                                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z" /><path stroke-linecap="round" stroke-linejoin="round" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
                                                <h4 class="font-bold text-indigo-600">목적</h4>
                                            </div>
                                            <ul class="pl-7 pt-1 list-['–_'] list-outside ml-2">
                                                <li>EVP(소통·성장·경험)를 실제 사례와 얼굴로 보여줘 메시지의 신뢰성과 설득력 강화</li>
                                                <li>스튜디오룸·전문 포토그래퍼라는 강점을 활용해 차별화된 콘텐츠 제작 
                                                <br />(예 : 건축상 수상 사옥, 다양한 휴식 공간, 다이닝룸 불멍)</li>
                                            </ul>
                                        </div>

                                        <div>
                                            <div class="flex items-center gap-2">
                                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283-.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0z" /></svg>
                                                <h4 class="font-bold text-indigo-600">대상 선정</h4>
                                            </div>
                                            <ul class="pl-7 pt-1 list-['–_'] list-outside ml-2">
                                                <li>직군 다양성: 개발, 기획, 영업 등 핵심 직군</li>
                                                <li>경력 스펙트럼: 입사 6개월·2년·5년 차 등</li>
                                                <li>스토리성: 독특한 입사 계기, 성장 경험 보유자</li>
                                            </ul>
                                        </div>

                                        <div>
                                            <div class="flex items-center gap-2">
                                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.546-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
                                                <h4 class="font-bold text-indigo-600">질문 구성 (EVP 중심)</h4>
                                            </div>
                                            <ul class="pl-7 pt-1 list-['–_'] list-outside ml-2">
                                                <li><strong>소통:</strong> 오픈미팅·커피챗에서 인상 깊었던 경험</li>
                                                <li><strong>성장:</strong> 능력 있는 동료와의 협업, 빠른 커리어 성장 사례</li>
                                                <li><strong>경험:</strong> 최고의 동료와 몰입했던 순간, 사옥·환경이 준 몰입감</li>
                                            </ul>
                                        </div>
                                    </div>
                                    
                                    <div class="space-y-4">
                                        <div>
                                            <div class="flex items-center gap-2">
                                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2-2H5a2 2 0 01-2-2V9z" /><path stroke-linecap="round" stroke-linejoin="round" d="M15 13a3 3 0 11-6 0 3 3 0 016 0z" /></svg>
                                                <h4 class="font-bold text-indigo-600">촬영 포인트</h4>
                                            </div>
                                            <ul class="pl-7 pt-1 list-['–_'] list-outside ml-2">
                                                <li>스튜디오룸에서 인물 집중 촬영</li>
                                                <li>사옥 공간(카페·라운지·옥상) 활용 장면</li>
                                                <li>B-roll: 회의, 협업, 사옥 전경, 자연스러운 교류</li>
                                            </ul>
                                        </div>

                                        <div>
                                            <div class="flex items-center gap-2">
                                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M7 4v16M17 4v16M3 8h4m10 0h4M3 12h18M3 16h4m10 0h4M4 20h16a1 1 0 001-1V5a1 1 0 00-1-1H4a1 1 0 00-1 1v14a1 1 0 001 1z" /></svg>
                                                <h4 class="font-bold text-indigo-600">콘텐츠 포맷</h4>
                                            </div>
                                            <ul class="pl-7 pt-1 list-['–_'] list-outside ml-2">
                                                <li>풀버전 영상(3~5분) → 채용 페이지, 유튜브</li>
                                                <li>숏폼 하이라이트 → 인스타, 링크드인 등 SNS</li>
                                                <li>사진·카드뉴스 → 채용공고, 블로그, 사내 뉴스레터</li>
                                            </ul>
                                        </div>

                                        <div>
                                            <div class="flex items-center gap-2">
                                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M13 10V3L4 14h7v7l9-11h-7z" /></svg>
                                                <h4 class="font-bold text-indigo-600">활용 계획</h4>
                                            </div>
                                            <ul class="pl-7 pt-1 list-['–_'] list-outside ml-2">
                                                <li>채용공고 상단, 온보딩 자료, 채용 설명회에 삽입</li>
                                                <li>사옥·문화 강점 강조로 차별화된 인재 유치</li>
                                            </ul>
                                        </div>
                                    </div>
                                </div>
                            </div>

                        </div> <!-- End Process Grid -->

                        <!-- Footer -->
                        <footer class="text-center mt-12">
                            <p class="text-sm text-slate-500">EXEM Corp. | EX Team</p>
                        </footer>

                    </div>

                </body>
                </html>
            `;

            function assignTasksToDates() {
                const year = 2025;
                Object.entries(tasksByPhase).forEach(([phase, tasks]) => {
                    const month = phaseDetails[phase].month;
                    tasks.forEach(task => {
                        const firstDayOfWeek = new Date(year, month, (task.week - 1) * 7 + 1);
                        let taskDate = new Date(firstDayOfWeek);
                        while (taskDate.getDay() !== 1) { // Find Monday
                            taskDate.setDate(taskDate.getDate() + 1);
                        }
                        task.startDate = taskDate;
                        task.status = '대기';
                        task.id = `task-${Math.random()}`;
                        task.phase = phase;
                        allTasks.push(task);
                    });
                });
            }

            function renderOverview() {
                overviewSection.innerHTML = `<h2 class="text-2xl font-bold text-center mb-6 text-gray-700">단계별 개요</h2><div class="space-y-4"></div>`;
                const container = overviewSection.querySelector('.space-y-4');
                Object.entries(phaseDetails).forEach(([phase, details]) => {
                    const phaseTasks = allTasks.filter(t => t.phase === phase);
                    const completedPhaseTasks = phaseTasks.filter(t => t.status === '완료').length;
                    const progress = phaseTasks.length > 0 ? (completedPhaseTasks / phaseTasks.length) * 100 : 0;

                    const card = document.createElement('div');
                    card.className = 'phase-card cursor-pointer p-6 bg-gray-100 rounded-lg border border-gray-200';
                    card.dataset.phase = phase;
                    card.innerHTML = `
                        <div class="flex justify-between items-start mb-2">
                            <h3 class="text-lg font-bold text-indigo-700">${details.title}</h3>
                            <span class="font-semibold text-gray-500 ml-4">${details.monthName}</span>
                        </div>
                        <div class="w-full bg-gray-200 rounded-full h-2.5">
                            <div class="bg-indigo-500 h-2.5 rounded-full" style="width: ${progress}%"></div>
                        </div>
                        <div class="text-right text-sm mt-1 text-gray-500">${completedPhaseTasks} / ${phaseTasks.length} 완료</div>
                    `;
                    container.appendChild(card);
                });
            }
            
            function renderSummary() {
                summarySection.innerHTML = `
                    <div class="bg-indigo-50 border border-indigo-200 rounded-lg p-6 mb-6">
                        <h2 class="text-xl font-bold text-center mb-4 text-indigo-800">계획 요약</h2>
                        <div class="grid grid-cols-3 text-center mb-4">
                            <div>
                                <p class="text-sm text-gray-500">총 기간</p>
                                <p class="text-2xl font-bold text-indigo-600">3개월</p>
                            </div>
                            <div>
                                <p class="text-sm text-gray-500">총 미션</p>
                                <p id="summary-total-tasks" class="text-2xl font-bold text-indigo-600">0개</p>
                            </div>
                            <div>
                                <p class="text-sm text-gray-500">완료</p>
                                <p id="summary-completed-tasks" class="text-2xl font-bold text-indigo-600">0개</p>
                            </div>
                        </div>
                        <div class="space-y-2 text-sm text-gray-700 mb-4">
                            <p><span class="font-bold text-indigo-600">9월 (1단계):</span> ${phaseDetails.phase1.title.split(':')[1].trim()}</p>
                            <p><span class="font-bold text-indigo-600">10월 (2단계):</span> ${phaseDetails.phase2.title.split(':')[1].trim()}</p>
                            <p><span class="font-bold text-indigo-600">11월 (3단계):</span> ${phaseDetails.phase3.title.split(':')[1].trim()}</p>
                        </div>
                        <div>
                            <p class="text-sm text-gray-600 mb-1">전체 진행률</p>
                            <div class="w-full bg-gray-200 rounded-full h-2.5">
                                <div id="summary-progress-bar" class="bg-indigo-500 h-2.5 rounded-full" style="width: 0%; transition: width 0.8s ease-in-out;"></div>
                            </div>
                        </div>
                    </div>
                `;
            }

            function updateSummaryValues() {
                const totalTasks = allTasks.length;
                const completed = allTasks.filter(t => t.status === '완료').length;
                const progress = totalTasks > 0 ? (completed / totalTasks) * 100 : 0;

                document.getElementById('summary-total-tasks').textContent = `${totalTasks}개`;
                document.getElementById('summary-completed-tasks').textContent = `${completed}개`;
                
                const progressBar = document.getElementById('summary-progress-bar');
                if (progressBar) {
                    progressBar.style.width = `${progress}%`;
                }
            }

            function renderCalendarView(phase) {
                const details = phaseDetails[phase];
                const year = 2025;
                const month = details.month;
                
                calendarTitle.innerHTML = `
                    <span class="block text-xl text-indigo-600 font-bold">${details.title}</span>
                    <span class="text-sm text-gray-500 font-semibold mt-1">${year}년 ${month + 1}월</span>
                `;

                calendarGrid.innerHTML = '';
                taskBarContainer.innerHTML = '';

                const firstDayOfMonth = new Date(year, month, 1);
                const firstDayOfWeek = firstDayOfMonth.getDay();
                const daysInMonth = new Date(year, month + 1, 0).getDate();

                ['일', '월', '화', '수', '목', '금', '토'].forEach(day => {
                    const dayEl = document.createElement('div');
                    dayEl.className = 'text-center font-bold text-gray-400 text-xs py-2';
                    dayEl.textContent = day;
                    calendarGrid.appendChild(dayEl);
                });

                for (let i = 0; i < firstDayOfWeek; i++) {
                    const emptyCell = document.createElement('div');
                    emptyCell.className = 'border-t border-gray-200';
                    calendarGrid.appendChild(emptyCell);
                }

                for (let i = 1; i <= daysInMonth; i++) {
                    const dayEl = document.createElement('div');
                    dayEl.className = 'border-t border-gray-200 h-24 p-1 text-sm text-gray-600';
                    dayEl.innerHTML = `<span>${i}</span>`;
                    dayEl.id = `date-${year}-${month}-${i}`;
                    calendarGrid.appendChild(dayEl);
                }
                
                // Use a timeout to ensure the grid is rendered before calculating positions
                setTimeout(() => {
                    const phaseTasks = allTasks.filter(t => t.phase === phase);
                    phaseTasks.forEach(task => {
                        const startDate = task.startDate;
                        const startCell = document.getElementById(`date-${startDate.getFullYear()}-${startDate.getMonth()}-${startDate.getDate()}`);
                        
                        if (startCell) {
                            const bar = document.createElement('div');
                            bar.className = 'task-bar font-medium';
                            bar.dataset.status = task.status;
                            bar.dataset.title = task.title; // Store full title for tooltip
                            bar.innerHTML = `
                                <span class="task-bar-title">${task.title}</span>
                                <span class="task-status text-xs font-bold px-2 py-0.5 rounded-full bg-white bg-opacity-30" data-task-id="${task.id}">${task.status}</span>
                            `;

                            const cellWidth = startCell.offsetWidth;
                            const gap = 1; // From grid-gap-1
                            
                            bar.style.top = `${startCell.offsetTop + 28}px`;
                            bar.style.left = `${startCell.offsetLeft}px`;
                            bar.style.width = `${(cellWidth * 5) + (gap * 4)}px`;

                            taskBarContainer.appendChild(bar);
                        }
                    });
                }, 0);
            }
            
            taskBarContainer.addEventListener('click', (e) => {
                if (e.target.classList.contains('task-status')) {
                    const taskEl = e.target;
                    const taskId = taskEl.dataset.taskId;
                    const task = allTasks.find(t => t.id === taskId);
                    
                    const currentIndex = statuses.indexOf(task.status);
                    task.status = statuses[(currentIndex + 1) % statuses.length];
                    
                    // Update UI
                    taskEl.dataset.status = task.status;
                    taskEl.textContent = task.status;
                    taskEl.closest('.task-bar').dataset.status = task.status;

                    updateSummaryValues();
                }
            });

            // Tooltip logic
            taskBarContainer.addEventListener('mouseover', (e) => {
                const bar = e.target.closest('.task-bar');
                if (bar) {
                    tooltip.textContent = bar.dataset.title;
                    tooltip.classList.remove('hidden');
                }
            });

            taskBarContainer.addEventListener('mouseout', (e) => {
                const bar = e.target.closest('.task-bar');
                if (bar) {
                    tooltip.classList.add('hidden');
                }
            });

            taskBarContainer.addEventListener('mousemove', (e) => {
                if (!tooltip.classList.contains('hidden')) {
                    tooltip.style.left = `${e.pageX + 15}px`;
                    tooltip.style.top = `${e.pageY + 15}px`;
                }
            });


            overviewSection.addEventListener('click', (e) => {
                const card = e.target.closest('.phase-card');
                if (card) {
                    const phase = card.dataset.phase;
                    overviewSection.classList.add('animate-fadeOut');
                    overviewSection.addEventListener('animationend', () => {
                        overviewSection.classList.add('hidden');
                        overviewSection.classList.remove('animate-fadeOut');
                        
                        renderCalendarView(phase);
                        calendarSection.classList.remove('hidden');
                        calendarSection.classList.add('animate-fadeIn');
                        calendarSection.addEventListener('animationend', () => {
                            calendarSection.classList.remove('animate-fadeIn');
                        }, { once: true });
                    }, { once: true });
                }
            });

            backBtn.addEventListener('click', () => {
                calendarSection.classList.add('animate-fadeOut');
                calendarSection.addEventListener('animationend', () => {
                    calendarSection.classList.add('hidden');
                    calendarSection.classList.remove('animate-fadeOut');

                    renderOverview();
                    overviewSection.classList.remove('hidden');
                    overviewSection.classList.add('animate-fadeIn');
                    overviewSection.addEventListener('animationend', () => {
                        overviewSection.classList.remove('animate-fadeIn');
                    }, { once: true });
                }, { once: true });
            });

            function renderInterviewees() {
                const container = document.getElementById('interviewee-list');
                container.innerHTML = ''; // Clear list before rendering
                interviewees.forEach(p => {
                    const personElement = document.createElement('div');
                    personElement.className = 'flex items-center justify-between bg-gray-100 p-2 rounded-lg';
                    personElement.innerHTML = `
                        <div class="flex-grow">
                            <p class="font-semibold text-gray-700">${p.team} ${p.name}</p>
                            <p class="text-sm text-gray-500">${p.role} (${p.date})</p>
                        </div>
                    `;
                    container.appendChild(personElement);
                });
            }

            // Modal Logic
            function showModal() {
                brandingModal.classList.remove('hidden');
            }

            function hideModal() {
                brandingModal.classList.add('hidden');
            }

            if(brandingBtn) {
                brandingBtn.addEventListener('click', showModal);
            }
            closeModalBtn.addEventListener('click', hideModal);
            brandingModal.addEventListener('click', (e) => {
                // Close modal if overlay is clicked
                if (e.target === brandingModal) {
                    hideModal();
                }
            });


            // --- Initial Load ---
            document.getElementById('branding-iframe').srcdoc = brandingGuideHtml;
            assignTasksToDates();
            renderSummary();
            updateSummaryValues();
            renderOverview();
            renderInterviewees();
        });
    </script>
</body>
</html>
