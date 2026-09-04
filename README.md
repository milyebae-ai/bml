[index.html.html](https://github.com/user-attachments/files/31836474/index.html.html)
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>나의 미디어 작품 갤러리</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Pretendard', 'Malgun Gothic', sans-serif;
            background-color: #f4f6f9;
            color: #222;
            padding: 30px 20px;
        }

        header {
            text-align: center;
            margin-bottom: 25px;
            padding-bottom: 20px;
            border-bottom: 2px solid #ddd;
        }

        header h1 {
            font-size: 2rem;
            color: #1a365d;
            margin-bottom: 8px;
        }

        header p {
            font-size: 1.1rem;
            color: #555;
        }

        /* 탭 버튼 메뉴 */
        .category-nav {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-bottom: 30px;
        }

        .tab-btn {
            background-color: #ffffff;
            border: 2px solid #cbd5e0;
            color: #4a5568;
            padding: 10px 24px;
            font-size: 1.1rem;
            font-weight: bold;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .tab-btn:hover {
            background-color: #edf2f7;
        }

        .tab-btn.active {
            background-color: #1a365d;
            border-color: #1a365d;
            color: #ffffff;
        }

        /* 갤러리 반응형 그리드 */
        .gallery-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 25px;
            max-width: 1400px;
            margin: 0 auto;
        }

        /* 개별 작품 카드 */
        .art-card {
            background: #ffffff;
            border-radius: 14px;
            overflow: hidden;
            box-shadow: 0 4px 12px rgba(0,0,0,0.06);
            display: none; /* 기본 숨김 후 선택된 탭만 노출 */
            flex-direction: column;
            position: relative;
        }

        /* 미디어 박스 */
        .media-container {
            width: 100%;
            height: 230px;
            background-color: #000;
            position: relative;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .art-media {
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: block;
        }

        /* 배지 */
        .media-badge {
            position: absolute;
            top: 12px;
            left: 12px;
            background: rgba(0, 0, 0, 0.75);
            color: #fff;
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: bold;
            z-index: 2;
        }

        .art-body {
            padding: 16px;
            display: flex;
            flex-direction: column;
            flex-grow: 1;
        }

        .art-title {
            font-size: 1.2rem;
            font-weight: bold;
            color: #2d3748;
            margin-bottom: 10px;
        }

        /* 좋아요 버튼 */
        .like-section {
            margin-bottom: 10px;
        }

        .like-btn {
            background-color: #fff0f3;
            border: 1.5px solid #ff4d6d;
            color: #ff4d6d;
            padding: 6px 14px;
            font-size: 0.95rem;
            font-weight: bold;
            border-radius: 20px;
            cursor: pointer;
            transition: 0.2s;
        }

        .like-btn:hover {
            background-color: #ff4d6d;
            color: white;
        }

        /* 댓글 영역 */
        .comment-section {
            margin-top: auto;
            border-top: 1px solid #edf2f7;
            padding-top: 10px;
        }

        .comment-input-box {
            display: flex;
            gap: 6px;
            margin-bottom: 8px;
        }

        .comment-input {
            flex-grow: 1;
            padding: 7px 10px;
            font-size: 0.9rem;
            border: 1px solid #cbd5e0;
            border-radius: 6px;
            outline: none;
        }

        .comment-btn {
            background-color: #3182ce;
            color: white;
            border: none;
            padding: 7px 12px;
            font-size: 0.9rem;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 500;
            white-space: nowrap;
        }

        .comment-list {
            list-style: none;
            max-height: 90px;
            overflow-y: auto;
            background-color: #f7fafc;
            border-radius: 6px;
            padding: 6px;
            font-size: 0.85rem;
        }

        .comment-list li {
            padding: 4px 6px;
            border-bottom: 1px dashed #e2e8f0;
            color: #4a5568;
            word-break: break-all;
        }

        .comment-list li:last-child {
            border-bottom: none;
        }
    </style>
</head>
<body>

    <header>
        <h1>🎬 나의 온라인 미디어 갤러리</h1>
        <p>멋진 사진과 영상을 감상하시고 좋아요와 따뜻한 한 줄 댓글을 남겨주세요!</p>
    </header>

    <!-- 사진 / 동영상 2개 카테고리 탭 -->
    <div class="category-nav">
        <button class="tab-btn active" onclick="filterCategory('image', this)">🖼️ 사진 갤러리</button>
        <button class="tab-btn" onclick="filterCategory('video', this)">🎥 동영상 갤러리</button>
    </div>

    <!-- 갤러리 그리드 -->
    <div class="gallery-grid" id="galleryContainer"></div>

    <script>
        const totalItems = 100;
        const container = document.getElementById('galleryContainer');
        let currentFilter = 'image'; // 기본 시작 카테고리: 사진

        for (let i = 1; i <= totalItems; i++) {
            const card = document.createElement('div');
            card.className = 'art-card';
            card.id = `card-${i}`;
            card.setAttribute('data-type', 'video'); // 기본 탐색 타입

            card.innerHTML = `
                <span class="media-badge" id="badge-${i}">🎥 동영상</span>
                <div class="media-container" id="mediaBox-${i}">
                    <video class="art-media" 
                           controls 
                           playsinline 
                           preload="metadata" 
                           src="${i}.mp4" 
                           onloadeddata="checkDisplay(${i})"
                           onerror="fallbackToImage(${i})">
                    </video>
                </div>
                
                <div class="art-body">
                    <div class="art-title">작품 ${i}</div>
                    
                    <div class="like-section">
                        <button class="like-btn" id="likeBtn-${i}" onclick="toggleLike(${i})">
                            ❤️ 좋아요 <span id="likeCount-${i}">0</span>
                        </button>
                    </div>

                    <div class="comment-section">
                        <div class="comment-input-box">
                            <input type="text" id="commentInput-${i}" class="comment-input" placeholder="댓글 입력..." onkeypress="handleEnter(event, ${i})">
                            <button class="comment-btn" onclick="addComment(${i})">등록</button>
                        </div>
                        <ul class="comment-list" id="commentList-${i}">
                            <li>💬 첫 번째 댓글을 남겨보세요!</li>
                        </ul>
                    </div>
                </div>
            `;
            container.appendChild(card);
        }

        // 동영상이 정상 로드되었을 때 현재 탭에 맞춰 화면 표시
        function checkDisplay(id) {
            const card = document.getElementById(`card-${id}`);
            if (card && currentFilter === 'video') {
                card.style.display = 'flex';
            }
        }

        // 1단계: 동영상(i.mp4)이 없으면 사진(i.jpg)으로 교체
        function fallbackToImage(id) {
            const card = document.getElementById(`card-${id}`);
            const mediaBox = document.getElementById(`mediaBox-${id}`);
            const badge = document.getElementById(`badge-${id}`);

            if (card) card.setAttribute('data-type', 'image');
            if (badge) badge.innerText = "🖼️ 사진";
            
            mediaBox.innerHTML = `
                <img src="${id}.jpg" 
                     alt="작품 ${id}" 
                     class="art-media" 
                     onload="onImageLoaded(${id})"
                     onerror="hideCard(${id})">
            `;
        }

        // 사진 파일이 정상 로드되었을 때 (기본 탭인 사진 탭일 때 즉시 표시)
        function onImageLoaded(id) {
            const card = document.getElementById(`card-${id}`);
            if (card && currentFilter === 'image') {
                card.style.display = 'flex';
            }
        }

        // 2단계: 사진도 없으면 완전 숨김
        function hideCard(id) {
            const card = document.getElementById(`card-${id}`);
            if (card) {
                card.setAttribute('data-type', 'none');
                card.style.display = 'none';
            }
        }

        // 카테고리 전환 (사진 <-> 동영상)
        function filterCategory(type, btnElement) {
            currentFilter = type;

            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            btnElement.classList.add('active');

            for (let i = 1; i <= totalItems; i++) {
                const card = document.getElementById(`card-${i}`);
                if (!card) continue;

                const cardType = card.getAttribute('data-type');
                
                if (cardType === type) {
                    card.style.display = 'flex';
                } else {
                    card.style.display = 'none';
                }
            }
        }

        // 좋아요 카운트
        function toggleLike(id) {
            const countSpan = document.getElementById(`likeCount-${id}`);
            let count = parseInt(countSpan.innerText, 10);
            countSpan.innerText = count + 1;
        }

        // 댓글 등록
        function addComment(id) {
            const input = document.getElementById(`commentInput-${id}`);
            const text = input.value.trim();
            if (text === "") {
                alert("댓글 내용을 입력해 주세요.");
                return;
            }

            const list = document.getElementById(`commentList-${id}`);
            const newComment = document.createElement('li');
            newComment.textContent = `✍️ ${text}`;
            list.appendChild(newComment);

            input.value = "";
            list.scrollTop = list.scrollHeight;
        }

        // 엔터키 지원
        function handleEnter(event, id) {
            if (event.key === "Enter") {
                addComment(id);
            }
        }
    </script>
</body>
</html>
