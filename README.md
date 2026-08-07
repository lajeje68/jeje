<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Korean Tutors - Find Your Best Teacher</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f8f9fa;
            color: #333;
            padding: 20px;
        }

        header {
            text-align: center;
            margin-bottom: 40px;
            padding: 20px 0;
        }

        header h1 {
            color: #2c3e50;
            font-size: 2rem;
            margin-bottom: 8px;
        }

        header p {
            color: #7f8c8d;
            font-size: 1.05rem;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
        }

        /* Tutor Card Grid */
        .tutor-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 25px;
        }

        .tutor-card {
            background: #fff;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.08);
            overflow: hidden;
            transition: transform 0.2s ease, box-shadow 0.2s ease;
            display: flex;
            flex-direction: column;
        }

        .tutor-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0,0,0,0.12);
        }

        .avatar-container {
            width: 100%;
            height: 220px;
            background-color: #e9ecef;
            overflow: hidden;
            position: relative;
        }

        .avatar-container img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .tutor-info {
            padding: 20px;
            flex-grow: 1;
            display: flex;
            flex-direction: column;
        }

        .tutor-name {
            font-size: 1.3rem;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 5px;
        }

        .tutor-title {
            color: #3498db;
            font-size: 0.9rem;
            font-weight: 600;
            margin-bottom: 12px;
        }

        .tutor-bio {
            color: #666;
            font-size: 0.95rem;
            line-height: 1.4;
            margin-bottom: 15px;
            flex-grow: 1;
        }

        .tags {
            display: flex;
            gap: 6px;
            flex-wrap: wrap;
            margin-bottom: 20px;
        }

        .tag {
            background-color: #eef2f5;
            color: #4a5568;
            font-size: 0.75rem;
            padding: 4px 8px;
            border-radius: 4px;
            font-weight: 500;
        }

        .btn-group {
            display: flex;
            gap: 10px;
        }

        .btn {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.2s ease;
            font-size: 0.9rem;
        }

        .btn-video {
            background-color: #e74c3c;
            color: white;
        }

        .btn-video:hover {
            background-color: #c0392b;
        }

        .btn-select {
            background-color: #2ecc71;
            color: white;
        }

        .btn-select:hover {
            background-color: #27ae60;
        }

        /* Modal Background */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.7);
            justify-content: center;
            align-items: center;
            z-index: 1000;
            padding: 20px;
        }

        /* Modal Content */
        .modal-content {
            background-color: #fff;
            padding: 20px;
            border-radius: 12px;
            max-width: 600px;
            width: 100%;
            position: relative;
        }

        .close-btn {
            position: absolute;
            top: 10px;
            right: 15px;
            font-size: 1.8rem;
            cursor: pointer;
            color: #aaa;
        }

        .close-btn:hover {
            color: #000;
        }

        .video-container {
            position: relative;
            padding-bottom: 56.25%; /* 16:9 ratio */
            height: 0;
            overflow: hidden;
            border-radius: 8px;
            margin-top: 15px;
        }

        .video-container iframe {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
    </style>
</head>
<body>

    <div class="container">
        <header>
            <h1>🇰🇷 Find Your Korean Tutor</h1>
            <p>Watch introduction videos and choose the best tutor for you!</p>
        </header>

        <!-- Tutor List Grid -->
        <div class="tutor-grid" id="tutorGrid"></div>
    </div>

    <!-- Video Modal -->
    <div class="modal" id="videoModal">
        <div class="modal-content">
            <span class="close-btn" onclick="closeModal()">&times;</span>
            <h3 id="modalTutorName">Tutor Intro</h3>
            <div class="video-container">
                <iframe id="videoPlayer" src="" frameborder="0" allowfullscreen></iframe>
            </div>
        </div>
    </div>

    <script>
        // 강사 데이터 샘플 (실제 데이터로 교체 가능)
        const tutors = [
            {
                id: 1,
                name: "김민준 (Minjun)",
                title: "TOPIK & Business Korean Expert",
                bio: "안녕하세요! 5년 경력의 한국어 강사입니다. 초급부터 비즈니스 한국어까지 친절하게 가르쳐 드립니다.",
                tags: ["TOPIK", "Business", "Beginner"],
                image: "https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=500&auto=format&fit=crop&q=60",
                videoUrl: "https://www.youtube.com/embed/dQw4w9WgXcQ" // 유튜브 Embed 링크 형식
            },
            {
                id: 2,
                name: "이지은 (Sarah)",
                title: "K-Drama & Everyday Conversation",
                bio: "드라마와 드라마 속 표현으로 재미있게 한국어를 배워보세요! 자연스러운 회화를 원하시는 분께 추천합니다.",
                tags: ["Speaking", "K-Culture", "Daily Chat"],
                image: "https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=500&auto=format&fit=crop&q=60",
                videoUrl: "https://www.youtube.com/embed/dQw4w9WgXcQ"
            },
            {
                id: 3,
                name: "박서준 (Jun)",
                title: "Grammar & Academic Korean",
                bio: "체계적인 문법 정리와 정확한 발음 교정을 약속드립니다. 대학 입학 및 한국 유학 준비생을 환영합니다.",
                tags: ["Grammar", "Pronunciation", "Academic"],
                image: "https://images.unsplash.com/photo-1500648767791-00dcc994a43e?w=500&auto=format&fit=crop&q=60",
                videoUrl: "https://www.youtube.com/embed/dQw4w9WgXcQ"
            }
        ];

        // 강사 카드 화면에 렌더링
        function renderTutors() {
            const grid = document.getElementById('tutorGrid');
            grid.innerHTML = tutors.map(tutor => `
                <div class="tutor-card">
                    <div class="avatar-container">
                        <img src="${tutor.image}" alt="${tutor.name}">
                    </div>
                    <div class="tutor-info">
                        <div class="tutor-name">${tutor.name}</div>
                        <div class="tutor-title">${tutor.title}</div>
                        <div class="tutor-bio">${tutor.bio}</div>
                        <div class="tags">
                            ${tutor.tags.map(tag => `<span class="tag">#${tag}</span>`).join('')}
                        </div>
                        <div class="btn-group">
                            <button class="btn btn-video" onclick="openVideo('${tutor.name}', '${tutor.videoUrl}')">🎬 Video</button>
                            <button class="btn btn-select" onclick="selectTutor('${tutor.name}')">Select</button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        // 비디오 모달 열기
        function openVideo(name, videoUrl) {
            document.getElementById('modalTutorName').innerText = `${name} - Introduction`;
            document.getElementById('videoPlayer').src = videoUrl + "?autoplay=1";
            document.getElementById('videoModal').style.display = 'flex';
        }

        // 모달 닫기
        function closeModal() {
            document.getElementById('videoModal').style.display = 'none';
            document.getElementById('videoPlayer').src = ""; // 비디오 멈춤
        }

        // 강사 선택 시 동작
        function selectTutor(name) {
            alert(`You selected ${name}! Moving to the booking page.`);
            // 여기에 구글 폼 링크나 카카오톡 플러스친구 / 신청 페이지 연결
        }

        // 페이지 로드 시 실행
        renderTutors();
    </script>
</body>
</html>
