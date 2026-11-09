# <!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0">
    <title>Антиквариатъ — Антикварная лавка</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Marck+Script&family=Caveat:wght@400;500;600;700&family=Cormorant+Garamond:ital,wght@0,400;0,500;0,600;1,400&display=swap" rel="stylesheet">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        :root {
            --bordo: #5c0f1c; --bordo-dark: #3f0a13; --bordo-light: #6e1424;
            --beige: #ead9bd; --beige-soft: #c8b695; --gold: #b99a5e;
            --line: rgba(185, 154, 94, 0.25); --danger: #c0564a;
        }
        html, body { overflow-x: hidden; }
        body {
            font-family: 'Caveat', cursive; background-color: var(--bordo);
            color: var(--beige); line-height: 1.4; font-size: 20px;
            min-height: 100vh; -webkit-text-size-adjust: 100%;
        }

        .admin-trigger {
            position: fixed; top: 0; left: 0; width: 70px; height: 70px;
            z-index: 1100; cursor: pointer; background: transparent;
            border: none; user-select: none; touch-action: manipulation;
        }
        .admin-trigger::after {
            content: ''; position: absolute; top: 12px; left: 12px;
            width: 10px; height: 10px; border-radius: 50%;
            background: var(--gold); opacity: 0.15; transition: opacity 0.3s;
        }

        .admin-panel {
            position: fixed; top: 16px; left: 16px; z-index: 1200;
            display: none; flex-direction: column; gap: 6px;
            align-items: flex-start; max-width: calc(100vw - 32px);
            max-height: calc(100vh - 32px); overflow-y: auto;
        }
        .admin-panel.open { display: flex; }
        .admin-controls {
            display: flex; flex-direction: column; gap: 6px;
            background: rgba(63, 10, 19, 0.98); padding: 12px;
            border: 1px solid var(--gold); box-shadow: 0 6px 30px rgba(0,0,0,0.6);
        }
        .admin-controls-title {
            font-size: 13px; color: var(--gold); text-transform: uppercase;
            letter-spacing: 1px; text-align: center;
            font-family: 'Cormorant Garamond', serif; margin-bottom: 4px;
        }
        .admin-btn {
            background: var(--bordo-dark); color: var(--beige);
            border: 1px solid var(--gold); padding: 8px 14px;
            font-size: 17px; font-family: 'Caveat', cursive;
            cursor: pointer; transition: all 0.2s;
            display: inline-flex; align-items: center; gap: 6px; white-space: nowrap;
        }
        .admin-btn:hover { background: var(--gold); color: var(--bordo-dark); }
        .admin-btn.danger { border-color: var(--danger); color: #e8a8a0; }
        .admin-btn.danger:hover { background: var(--danger); color: #fff; }
        .admin-btn.small { padding: 6px 10px; font-size: 16px; width: 100%; justify-content: center; }

        body.editing [data-edit] {
            outline: 1px dashed var(--gold); outline-offset: 2px;
            background: rgba(185, 154, 94, 0.08); cursor: text;
        }
        body.editing [data-edit]:focus {
            outline: 2px solid var(--gold);
            background: rgba(185, 154, 94, 0.16);
        }

        header {
            background: var(--bordo-dark); padding: 22px 0;
            position: sticky; top: 0; z-index: 100;
            border-bottom: 1px solid var(--line);
        }
        .header-content {
            max-width: 1200px; margin: 0 auto; padding: 0 28px;
            display: flex; justify-content: space-between; align-items: center;
            flex-wrap: wrap; gap: 12px;
        }
        .logo { font-family: 'Marck Script', cursive; font-size: 34px; color: var(--beige); line-height: 1; }
        .logo span { display: block; font-family: 'Caveat', cursive; font-size: 16px; color: var(--beige-soft); margin-top: 2px; }
        nav ul { display: flex; list-style: none; gap: 26px; flex-wrap: wrap; }
        nav a { color: var(--beige-soft); text-decoration: none; font-size: 19px; transition: color 0.25s; }
        nav a:hover { color: var(--gold); }

        .hero {
            background: linear-gradient(rgba(63, 10, 19, 0.9), rgba(92, 15, 28, 0.9)),
                        url('https://images.unsplash.com/photo-1513519245088-0e12902e5a38?w=1600') center/cover no-repeat;
            min-height: 480px; display: flex; align-items: center;
            justify-content: center; text-align: center; padding: 40px 20px;
            border-bottom: 1px solid var(--line);
        }
        .hero h1 { font-family: 'Marck Script', cursive; font-size: 64px; font-weight: normal; color: var(--beige); margin-bottom: 12px; line-height: 1.1; }
        .hero p { font-size: 24px; color: var(--beige-soft); margin-bottom: 32px; }
        .hero-btn {
            display: inline-block; padding: 12px 40px; border: 1px solid var(--gold);
            color: var(--gold); text-decoration: none; font-size: 20px;
            transition: all 0.3s; cursor: pointer; background: transparent; font-family: 'Caveat', cursive;
        }
        .hero-btn:hover { background: var(--gold); color: var(--bordo-dark); }

        .section { max-width: 1200px; margin: 0 auto; padding: 70px 24px; }
        .section-title { text-align: center; font-family: 'Marck Script', cursive; font-size: 46px; font-weight: normal; color: var(--beige); margin-bottom: 6px; line-height: 1.2; }
        .section-subtitle { text-align: center; color: var(--beige-soft); font-size: 19px; margin-bottom: 44px; }
        .section-subtitle::after { content: ''; display: block; width: 40px; height: 1px; background: var(--gold); margin: 16px auto 0; }

        .categories { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 1px; background: var(--line); border: 1px solid var(--line); }
        .category-card { background: var(--bordo); padding: 40px 22px; text-align: center; transition: background 0.3s; cursor: pointer; position: relative; }
        .category-card:hover { background: var(--bordo-dark); }
        .category-index { font-family: 'Cormorant Garamond', serif; font-size: 14px; color: var(--gold); letter-spacing: 2px; margin-bottom: 12px; }
        .category-card h3 { font-family: 'Marck Script', cursive; font-size: 28px; font-weight: normal; color: var(--beige); margin-bottom: 6px; line-height: 1.2; }
        .category-card p { font-size: 17px; color: var(--beige-soft); }
        .category-count { font-size: 14px; color: var(--gold); margin-top: 10px; font-family: 'Cormorant Garamond', serif; letter-spacing: 1px; }
        .category-thumb { width: 70px; height: 70px; margin: 0 auto 14px; background-size: cover; background-position: center; border: 1px solid var(--gold); display: none; }
        .category-thumb.has-image { display: block; }

        .products { display: grid; grid-template-columns: repeat(auto-fit, minmax(290px, 1fr)); gap: 26px; }
        .product-card { background: var(--bordo-dark); border: 1px solid var(--line); overflow: hidden; transition: border-color 0.3s; position: relative; }
        .product-card:hover { border-color: var(--gold); }
        .product-image { height: 240px; background-size: cover; background-position: center; position: relative; background-color: #2e0710; }
        .product-image.no-photo::after { content: 'без фото'; position: absolute; inset: 0; display: flex; align-items: center; justify-content: center; color: var(--beige-soft); font-size: 18px; }
        .product-badge { position: absolute; top: 14px; left: 14px; background: rgba(63, 10, 19, 0.9); color: var(--gold); border: 1px solid var(--gold); padding: 4px 10px; font-size: 15px; }
        .product-info { padding: 22px; border-top: 1px solid var(--line); }
        .product-info h3 { font-family: 'Marck Script', cursive; font-size: 27px; font-weight: normal; margin-bottom: 4px; color: var(--beige); line-height: 1.2; }
        .product-era { font-size: 16px; color: var(--beige-soft); margin-bottom: 14px; }
        .product-price { font-size: 24px; color: var(--gold); font-weight: 600; }
        .product-price small { font-size: 16px; color: var(--beige-soft); text-decoration: line-through; margin-left: 8px; font-weight: 400; }

        .category-page { display: none; }
        .category-page.active { display: block; }
        .back-btn { background: transparent; border: 1px solid var(--gold); color: var(--gold); padding: 8px 18px; font-size: 18px; font-family: 'Caveat', cursive; cursor: pointer; margin-bottom: 24px; transition: all 0.2s; }
        .back-btn:hover { background: var(--gold); color: var(--bordo-dark); }
        .category-header { text-align: center; margin-bottom: 40px; }
        .category-header h2 { font-family: 'Marck Script', cursive; font-size: 48px; font-weight: normal; color: var(--beige); margin-bottom: 6px; }
        .category-header .total { font-size: 20px; color: var(--gold); margin-top: 8px; }
        .empty-message { text-align: center; color: var(--beige-soft); font-size: 22px; padding: 40px 20px; border: 1px dashed var(--line); }

        .about { background: var(--bordo-dark); padding: 70px 24px; border-top: 1px solid var(--line); border-bottom: 1px solid var(--line); }
        .about-content { max-width: 960px; margin: 0 auto; display: grid; grid-template-columns: 1fr 1fr; gap: 50px; align-items: center; }
        .about-text h2 { font-family: 'Marck Script', cursive; font-size: 42px; font-weight: normal; margin-bottom: 18px; color: var(--beige); }
        .about-text p { color: var(--beige-soft); margin-bottom: 12px; font-size: 20px; }
        .about-image { height: 340px; background: url('https://images.unsplash.com/photo-1565791380713-1756b9a05343?w=800') center/cover; border: 1px solid var(--gold); position: relative; }
        .about-image .img-edit-btn { position: absolute; bottom: 10px; right: 10px; background: rgba(63,10,19,0.9); color: var(--gold); border: 1px solid var(--gold); padding: 6px 12px; font-family: 'Caveat', cursive; font-size: 16px; cursor: pointer; display: none; }
        body.editing .about-image .img-edit-btn { display: block; }

        .features { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 1px; background: var(--line); border: 1px solid var(--line); }
        .feature { background: var(--bordo); text-align: center; padding: 40px 22px; }
        .feature-num { font-family: 'Cormorant Garamond', serif; font-size: 30px; color: var(--gold); letter-spacing: 2px; margin-bottom: 10px; }
        .feature h3 { font-family: 'Marck Script', cursive; font-size: 26px; font-weight: normal; margin-bottom: 6px; color: var(--beige); }
        .feature p { font-size: 18px; color: var(--beige-soft); }

        .contacts { background: var(--bordo-dark); color: var(--beige); padding: 70px 24px; text-align: center; border-top: 1px solid var(--line); }
        .contacts h2 { font-family: 'Marck Script', cursive; font-size: 42px; font-weight: normal; margin-bottom: 36px; color: var(--beige); }
        .contact-grid { max-width: 960px; margin: 0 auto; display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 1px; background: var(--line); border: 1px solid var(--line); }
        .contact-item { background: var(--bordo-dark); padding: 26px 16px; }
        .contact-item h4 { font-size: 14px; letter-spacing: 2px; text-transform: uppercase; color: var(--gold); margin-bottom: 8px; font-weight: 400; font-family: 'Cormorant Garamond', serif; }
        .contact-item p { font-size: 20px; color: var(--beige); }

        .reviews { background: var(--bordo); padding: 70px 24px; border-top: 1px solid var(--line); }
        .reviews-inner { max-width: 960px; margin: 0 auto; }
        .reviews h2 { text-align: center; font-family: 'Marck Script', cursive; font-size: 42px; font-weight: normal; margin-bottom: 6px; color: var(--beige); }
        .reviews .section-subtitle { margin-bottom: 36px; }
        .reviews-list { display: flex; flex-direction: column; gap: 16px; margin-bottom: 30px; }
        .review-card { background: var(--bordo-dark); border: 1px solid var(--line); padding: 20px 22px; position: relative; }
        .review-head { display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 8px; flex-wrap: wrap; gap: 6px; }
        .review-author { font-family: 'Marck Script', cursive; font-size: 24px; color: var(--gold); }
        .review-date { font-size: 14px; color: var(--beige-soft); font-family: 'Cormorant Garamond', serif; }
        .review-text { font-size: 19px; color: var(--beige); white-space: pre-wrap; word-wrap: break-word; }
        .review-delete { position: absolute; top: 8px; right: 8px; width: 26px; height: 26px; background: var(--danger); color: #fff; border: none; font-size: 16px; cursor: pointer; display: none; font-family: sans-serif; line-height: 1; }
        body.editing .review-delete { display: block; }
        .review-empty { text-align: center; color: var(--beige-soft); font-size: 20px; padding: 30px 20px; border: 1px dashed var(--line); margin-bottom: 20px; }
        .review-form { background: var(--bordo-dark); border: 1px solid var(--line); padding: 22px; }
        .review-form h3 { font-family: 'Marck Script', cursive; font-size: 26px; font-weight: normal; color: var(--beige); margin-bottom: 14px; }
        .review-form label { display: block; font-size: 16px; color: var(--beige-soft); margin-bottom: 4px; margin-top: 10px; }
        .review-form input, .review-form textarea { width: 100%; background: var(--bordo); border: 1px solid var(--line); color: var(--beige); padding: 10px 12px; font-size: 19px; font-family: 'Caveat', cursive; outline: none; }
        .review-form input:focus, .review-form textarea:focus { border-color: var(--gold); }
        .review-form textarea { resize: vertical; min-height: 80px; }
        .review-form button { margin-top: 14px; padding: 10px 26px; border: 1px solid var(--gold); background: transparent; color: var(--gold); font-family: 'Caveat', cursive; font-size: 19px; cursor: pointer; transition: all 0.2s; }
        .review-form button:hover { background: var(--gold); color: var(--bordo-dark); }
        .review-error { color: #e08a8a; font-size: 16px; min-height: 20px; margin-top: 6px; }

        footer { background: #2e0710; color: var(--beige-soft); text-align: center; padding: 26px 20px; font-size: 16px; border-top: 1px solid var(--line); }

        .modal-overlay { position: fixed; inset: 0; background: rgba(20, 3, 7, 0.88); display: none; align-items: center; justify-content: center; z-index: 2000; padding: 16px; overflow-y: auto; }
        .modal-overlay.open { display: flex; }
        .modal { background: var(--bordo-dark); border: 1px solid var(--gold); padding: 30px 24px; width: 100%; max-width: 440px; margin: auto; }
        .modal h3 { font-family: 'Marck Script', cursive; font-size: 30px; font-weight: normal; color: var(--beige); margin-bottom: 20px; text-align: center; }
        .modal label { display: block; font-size: 16px; color: var(--beige-soft); margin-bottom: 4px; margin-top: 12px; }
        .modal input, .modal select { width: 100%; background: var(--bordo); border: 1px solid var(--line); color: var(--beige); padding: 10px 12px; font-size: 19px; font-family: 'Caveat', cursive; outline: none; }
        .modal input:focus, .modal select:focus { border-color: var(--gold); }
        .modal-btns { display: flex; gap: 10px; margin-top: 22px; }
        .modal-btns button { flex: 1; padding: 10px; border: 1px solid var(--gold); background: transparent; color: var(--gold); font-family: 'Caveat', cursive; font-size: 18px; cursor: pointer; transition: all 0.2s; }
        .modal-btns button:hover { background: var(--gold); color: var(--bordo-dark); }
        .modal-btns button.primary { background: var(--gold); color: var(--bordo-dark); }
        .modal-btns button.primary:hover { background: var(--beige); border-color: var(--beige); }
        .modal-error { color: #e08a8a; font-size: 16px; margin-bottom: 10px; min-height: 20px; text-align: center; }

        .add-btn { display: block; margin: 20px auto 0; padding: 10px 26px; border: 1px dashed var(--gold); background: transparent; color: var(--gold); font-family: 'Caveat', cursive; font-size: 20px; cursor: pointer; transition: all 0.2s; }
        .add-btn:hover { background: var(--gold); color: var(--bordo-dark); }

        .file-input-wrap { margin-top: 6px; }
        .file-input-wrap input[type="file"] { display: block; width: 100%; font-size: 15px; font-family: 'Caveat', cursive; color: var(--beige-soft); padding: 6px 0; }
        .file-input-wrap input[type="file"]::file-selector-button { background: var(--bordo); color: var(--gold); border: 1px solid var(--gold); padding: 6px 12px; font-family: 'Caveat', cursive; font-size: 16px; cursor: pointer; margin-right: 10px; }
        .image-preview { margin-top: 8px; max-width: 100%; max-height: 120px; border: 1px solid var(--line); display: none; object-fit: cover; }
        .image-preview.show { display: block; }

        .link-modal .modal { max-width: 520px; }
        .link-box { display: flex; gap: 8px; margin-top: 6px; }
        .link-box input { flex: 1; font-size: 15px; font-family: 'Cormorant Garamond', serif; padding: 10px; background: var(--bordo); border: 1px solid var(--line); color: var(--beige); outline: none; }
        .link-box button { padding: 10px 18px; background: var(--gold); color: var(--bordo-dark); border: 1px solid var(--gold); font-family: 'Caveat', cursive; font-size: 17px; cursor: pointer; white-space: nowrap; }
        .link-box button:hover { background: var(--beige); border-color: var(--beige); }
        .link-hint { font-size: 16px; color: var(--beige-soft); margin-top: 14px; line-height: 1.5; }

        .toast { position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%) translateY(100px); background: var(--gold); color: var(--bordo-dark); padding: 12px 26px; font-size: 18px; font-family: 'Caveat', cursive; z-index: 3000; transition: transform 0.3s; border: 1px solid var(--bordo-dark); white-space: nowrap; }
        .toast.show { transform: translateX(-50%) translateY(0); }

        @media (max-width: 768px) {
            body { font-size: 19px; }
            .header-content { flex-direction: column; gap: 10px; padding: 0 16px; }
            .logo { font-size: 28px; } .logo span { font-size: 14px; }
            nav ul { gap: 16px; justify-content: center; }
            nav a { font-size: 18px; }
            .hero { min-height: 400px; padding: 30px 16px; }
            .hero h1 { font-size: 42px; } .hero p { font-size: 20px; }
            .section { padding: 50px 16px; }
            .section-title { font-size: 36px; }
            .section-subtitle { font-size: 17px; margin-bottom: 32px; }
            .categories { grid-template-columns: 1fr 1fr; }
            .category-card { padding: 28px 14px; }
            .category-card h3 { font-size: 23px; } .category-card p { font-size: 15px; }
            .products { grid-template-columns: 1fr; gap: 20px; }
            .product-image { height: 220px; }
            .about-content { grid-template-columns: 1fr; gap: 30px; }
            .about-text h2 { font-size: 34px; } .about-image { height: 260px; }
            .contacts h2, .reviews h2 { font-size: 34px; }
            .contact-grid { grid-template-columns: 1fr 1fr; }
            .category-header h2 { font-size: 36px; }
            .admin-panel { top: 8px; left: 8px; max-width: calc(100vw - 16px); }
            .modal { padding: 24px 18px; }
            .toast { font-size: 16px; padding: 10px 18px; max-width: 90vw; white-space: normal; text-align: center; }
        }
        @media (max-width: 420px) {
            .categories { grid-template-columns: 1fr; }
            .contact-grid { grid-template-columns: 1fr; }
            .hero h1 { font-size: 34px; }
        }
    </style>
</head>
<body>

    <button class="admin-trigger" id="adminTrigger" aria-label="admin"></button>

    <div class="admin-panel" id="adminPanel">
        <div class="admin-controls" id="adminControls">
            <div class="admin-controls-title">Редактор</div>
            <button class="admin-btn small" id="addCategoryBtn">+ Категория</button>
            <button class="admin-btn small" id="addProductBtn">+ Товар</button>
            <button class="admin-btn small" id="copySiteLinkBtn">🔗 Ссылка на сайт</button>
            <button class="admin-btn small" id="saveBtn">💾 Сохранить</button>
            <button class="admin-btn small danger" id="resetBtn">↺ Сброс</button>
            <button class="admin-btn small" id="logoutBtn">Выйти</button>
        </div>
    </div>

    <div class="modal-overlay" id="loginModal">
        <div class="modal">
            <h3>Вход для админа</h3>
            <div class="modal-error" id="loginError"></div>
            <label>Пароль</label>
            <input type="password" id="passwordInput" placeholder="••••" maxlength="10" autocomplete="off" inputmode="numeric">
            <div class="modal-btns">
                <button id="loginCancel">Отмена</button>
                <button id="loginSubmit" class="primary">Войти</button>
            </div>
        </div>
    </div>

    <div class="modal-overlay" id="categoryModal">
        <div class="modal">
            <h3>Новая категория</h3>
            <label>Название</label>
            <input type="text" id="catName" placeholder="Например: Иконы">
            <label>Описание</label>
            <input type="text" id="catDesc" placeholder="Краткое описание">
            <label>Фото категории</label>
            <div class="file-input-wrap"><input type="file" id="catImageFile" accept="image/*"></div>
            <input type="text" id="catImageUrl" placeholder="или вставьте ссылку на фото" style="margin-top:8px;">
            <img class="image-preview" id="catImagePreview" alt="">
            <div class="modal-btns">
                <button id="catCancel">Отмена</button>
                <button id="catSubmit" class="primary">Создать</button>
            </div>
        </div>
    </div>

    <div class="modal-overlay" id="productModal">
        <div class="modal">
            <h3 id="prodModalTitle">Новый товар</h3>
            <label>Категория</label>
            <select id="prodCategory"></select>
            <label>Название</label>
            <input type="text" id="prodName" placeholder="Например: Подсвечник">
            <label>Эпоха / происхождение</label>
            <input type="text" id="prodEra" placeholder="Например: Россия, 1890 г.">
            <label>Цена</label>
            <input type="text" id="prodPrice" placeholder="Например: 45 000 ₽">
            <label>Старая цена (необязательно)</label>
            <input type="text" id="prodOldPrice" placeholder="Например: 60 000 ₽">
            <label>Бейдж (необязательно)</label>
            <input type="text" id="prodBadge" placeholder="Например: Раритет">
            <label>Фото товара</label>
            <div class="file-input-wrap"><input type="file" id="prodImageFile" accept="image/*"></div>
            <input type="text" id="prodImageUrl" placeholder="или вставьте ссылку на фото" style="margin-top:8px;">
            <img class="image-preview" id="prodImagePreview" alt="">
            <div class="modal-btns">
                <button id="prodCancel">Отмена</button>
                <button id="prodSubmit" class="primary">Сохранить</button>
            </div>
        </div>
    </div>

    <div class="modal-overlay" id="catImgModal">
        <div class="modal">
            <h3>Фото категории</h3>
            <label>Загрузить фото</label>
            <div class="file-input-wrap"><input type="file" id="catImgFileEdit" accept="image/*"></div>
            <input type="text" id="catImgUrlEdit" placeholder="или вставьте ссылку" style="margin-top:8px;">
            <img class="image-preview" id="catImgPreviewEdit" alt="">
            <div class="modal-btns">
                <button id="catImgCancel">Отмена</button>
                <button id="catImgRemove" class="danger" style="background:var(--danger);color:#fff;">Удалить фото</button>
                <button id="catImgSubmit" class="primary">Сохранить</button>
            </div>
        </div>
    </div>

    <div class="modal-overlay link-modal" id="siteLinkModal">
        <div class="modal">
            <h3>Ссылка на сайт</h3>
            <label>Скопируйте и отправьте</label>
            <div class="link-box">
                <input type="text" id="siteLinkInput" readonly>
                <button id="siteLinkCopyBtn">Копировать</button>
            </div>
            <div class="link-hint">Отправьте эту ссылку тому, кому хотите показать сайт. Она откроет главную страницу лавки.</div>
            <div class="modal-btns">
                <button id="siteLinkClose">Закрыть</button>
            </div>
        </div>
    </div>

    <div class="toast" id="toast"></div>

    <div id="mainPage">
        <header>
            <div class="header-content">
                <div class="logo">
                    <span data-edit>Антиквариатъ</span>
                    <span data-edit>съ 1892 года</span>
                </div>
                <nav>
                    <ul>
                        <li><a href="#catalog" data-edit>Каталог</a></li>
                        <li><a href="#about" data-edit>О нас</a></li>
                        <li><a href="#features" data-edit>Гарантии</a></li>
                        <li><a href="#reviews" data-edit>Отзывы</a></li>
                        <li><a href="#contacts" data-edit>Контакты</a></li>
                    </ul>
                </nav>
            </div>
        </header>

        <section class="hero">
            <div>
                <h1 data-edit>Антикварная лавка</h1>
                <p data-edit>Предметы с историей. Ценности, проверенные временем</p>
                <a href="#catalog" class="hero-btn" data-edit>Смотреть каталог</a>
            </div>
        </section>

        <section class="section" id="catalog">
            <h2 class="section-title" data-edit>Категории</h2>
            <p class="section-subtitle" data-edit>Нажмите на раздел, чтобы посмотреть предметы</p>
            <div class="categories" id="categoriesGrid"></div>
        </section>

        <section class="section" style="padding-top: 0;">
            <h2 class="section-title" data-edit>Избранные предметы</h2>
            <p class="section-subtitle" data-edit>Жемчужины нашей коллекции</p>
            <div class="products" id="featuredGrid"></div>
        </section>

        <section class="about" id="about">
            <div class="about-content">
                <div class="about-text">
                    <h2 data-edit>О лавке</h2>
                    <p data-edit>Наша антикварная лавка открыла свои двери более трёх десятилетий назад. Мы специализируемся на подлинных предметах русского и европейского искусства XVIII–XX веков.</p>
                    <p data-edit>Каждый экспонат проходит тщательную экспертизу: подлинность, провенанс, состояние сохранности. Мы работаем с частными коллекционерами, музеями и ценителями прекрасного.</p>
                </div>
                <div class="about-image" id="aboutImage">
                    <button class="img-edit-btn" id="aboutImgBtn">✎ Сменить фото</button>
                </div>
            </div>
        </section>

        <section class="section" id="features">
            <h2 class="section-title" data-edit>Наши гарантии</h2>
            <p class="section-subtitle" data-edit>Почему нам доверяют</p>
            <div class="features">
                <div class="feature">
                    <div class="feature-num" data-edit>01</div>
                    <h3 data-edit>Экспертиза</h3>
                    <p data-edit>Каждый предмет проверен профессиональными искусствоведами</p>
                </div>
                <div class="feature">
                    <div class="feature-num" data-edit>02</div>
                    <h3 data-edit>Подлинность</h3>
                    <p data-edit>Сертификат и полная история происхождения предмета</p>
                </div>
                <div class="feature">
                    <div class="feature-num" data-edit>03</div>
                    <h3 data-edit>Доставка</h3>
                    <p data-edit>Бережная транспортировка по всей России и за рубеж</p>
                </div>
                <div class="feature">
                    <div class="feature-num" data-edit>04</div>
                    <h3 data-edit>Консультация</h3>
                    <p data-edit>Поможем с оценкой и подбором предмета под ваш интерьер</p>
                </div>
            </div>
        </section>

        <section class="reviews" id="reviews">
            <div class="reviews-inner">
                <h2 data-edit>Отзывы</h2>
                <p class="section-subtitle" data-edit>Что говорят наши посетители</p>
                <div class="reviews-list" id="reviewsList"></div>
                <div class="review-form">
                    <h3>Оставить отзыв</h3>
                    <label>Ваше имя</label>
                    <input type="text" id="reviewName" maxlength="40" placeholder="Как к вам обращаться">
                    <label>Отзыв</label>
                    <textarea id="reviewText" maxlength="600" placeholder="Поделитесь впечатлением..."></textarea>
                    <div class="review-error" id="reviewError"></div>
                    <button id="reviewSubmit">Отправить отзыв</button>
                </div>
            </div>
        </section>

        <section class="contacts" id="contacts">
            <h2 data-edit>Свяжитесь с нами</h2>
            <div class="contact-grid">
                <div class="contact-item">
                    <h4 data-edit>Адрес</h4>
                    <p data-edit>Москва, ул. Арбат, 24</p>
                </div>
                <div class="contact-item">
                    <h4 data-edit>Телефон</h4>
                    <p data-edit>+7 (495) 123-45-67</p>
                </div>
                <div class="contact-item">
                    <h4 data-edit>E-mail</h4>
                    <p data-edit>info@antikvariat.ru</p>
                </div>
                <div class="contact-item">
                    <h4 data-edit>Часы работы</h4>
                    <p data-edit>Пн–Сб: 10:00–20:00</p>
                </div>
            </div>
        </section>

        <footer>
            <span data-edit>© 2026 Антиквариатъ — Антикварная лавка. Все права защищены.</span>
        </footer>
    </div>

    <div id="categoryPage" class="category-page">
        <section class="section">
            <button class="back-btn" id="backBtn">← Назад к категориям</button>
            <div class="category-header">
                <h2 id="catPageTitle"></h2>
                <div class="total" id="catPageTotal"></div>
            </div>
            <div class="products" id="catPageProducts"></div>
            <button class="add-btn" id="catPageAddBtn" style="display:none;">+ Добавить товар в эту категорию</button>
        </section>
        <footer>
            <span data-edit>© 2026 Антиквариатъ — Антикварная лавка. Все права защищены.</span>
        </footer>
    </div>

    <script>
        (function () {
            const PASSWORD = '888';
            const STORAGE_KEY = 'antikvariat_data_v4';

            // ===== ДАННЫЕ С ПОХОЖИМИ ФОТО (UNSPLASH) =====
            const defaultData = {
                categories: [
                    {
                        id: 'c1', name: 'Сервизы', desc: 'Фарфоровые сервизы', image: '', products: [
                            { id: 'p1', name: 'Ильменау (Бармен)', era: 'Германия, XX век', price: '350 000 ₽', oldPrice: '', badge: '', image: 'https://images.unsplash.com/photo-1601645191163-3fc0d5d64e35?w=600&q=80' },
                            { id: 'p2', name: 'Катарина (Веймар)', era: 'Германия, XX век', price: '160 000 ₽', oldPrice: '', badge: '', image: 'https://images.unsplash.com/photo-1579783902614-a3fb3927b6a5?w=600&q=80' },
                            { id: 'p3', name: 'Мадонна', era: 'Германия (ГДР), XX век', price: '12 000 ₽', oldPrice: '', badge: '', image: 'https://images.unsplash.com/photo-1610375461246-83df859d849d?w=600&q=80' },
                            { id: 'p4', name: 'Мадонна (кофейный)', era: 'Германия (ГДР), XX век', price: '5 000 ₽', oldPrice: '', badge: '', image: 'https://images.unsplash.com/photo-1513519245088-0e12902e5a38?w=600&q=80' }
                        ]
                    },
                    {
                        id: 'c2', name: 'Хрустали', desc: 'Хрустальная посуда', image: '', products: [
                            { id: 'p5', name: 'Графин', era: 'Европа, XX век', price: '5 000 ₽', oldPrice: '', badge: '', image: 'https://images.unsplash.com/photo-1565791380713-1756b9a05343?w=600&q=80' }
                        ]
                    },
                    {
                        id: 'c3', name: 'Серебро', desc: 'Серебряные изделия', image: '', products: []
                    },
                    {
                        id: 'c4', name: 'Мельхиор', desc: 'Изделия из мельхиора', image: '', products: [
                            { id: 'p6', name: 'Половник', era: 'Европа, XX век', price: '4 500 ₽', oldPrice: '', badge: '', image: 'https://images.unsplash.com/photo-1509048191080-d2984bad6ae5?w=600&q=80' },
                            { id: 'p7', name: 'Ведро для еды', era: 'Европа, XX век', price: '7 000 ₽', oldPrice: '', badge: '', image: 'https://images.unsplash.com/photo-1524805444758-089113d48a6d?w=600&q=80' },
                            { id: 'p8', name: 'Стопки', era: 'Европа, XX век', price: '7 000 ₽', oldPrice: '', badge: '', image: 'https://images.unsplash.com/photo-1601645191163-3fc0d5d64e35?w=600&q=80' }
                        ]
                    },
                    {
                        id: 'c5', name: 'Скатерти', desc: 'Кружевные скатерти', image: '', products: [
                            { id: 'p9', name: 'Скатерть «Кружевная дорожка»', era: 'Европа, XX век', price: '4 000 ₽', oldPrice: '', badge: '', image: 'https://images.unsplash.com/photo-1579783902614-a3fb3927b6a5?w=600&q=80' },
                            { id: 'p10', name: 'Скатерть «Квадратная кружевная»', era: 'Европа, XX век', price: '2 500 ₽', oldPrice: '', badge: '', image: 'https://images.unsplash.com/photo-1610375461246-83df859d849d?w=600&q=80' }
                        ]
                    },
                    {
                        id: 'c6', name: 'Фарфор и керамика', desc: 'Отдельные предметы', image: '', products: [
                            { id: 'p11', name: 'Тарелка КПМ (Выставочное блюдо)', era: 'Германия, XX век', price: 'Цена по запросу', oldPrice: '', badge: 'Выставочное', image: 'https://images.unsplash.com/photo-1513519245088-0e12902e5a38?w=600&q=80' }
                        ]
                    },
                    {
                        id: 'c7', name: 'Картины', desc: 'Живопись и графика', image: '', products: [
                            { id: 'p12', name: '«Дети с собакой» (Е. М. Бём)', era: 'Россия, конец XIX века', price: '52 000 ₽', oldPrice: '', badge: '', image: 'https://images.unsplash.com/photo-1565791380713-1756b9a05343?w=800&q=80' }
                        ]
                    }
                ],
                reviews: [],
                aboutImage: '',
                texts: {}
            };

            let data = null;
            let isAdmin = false;
            let currentCategoryId = null;
            let editingProductId = null;
            let pendingCatImage = '';
            let pendingProdImage = '';
            let editCatImageId = null;

            const adminTrigger = document.getElementById('adminTrigger');
            const adminPanel = document.getElementById('adminPanel');
            const saveBtn = document.getElementById('saveBtn');
            const resetBtn = document.getElementById('resetBtn');
            const logoutBtn = document.getElementById('logoutBtn');
            const addCategoryBtn = document.getElementById('addCategoryBtn');
            const addProductBtn = document.getElementById('addProductBtn');
            const copySiteLinkBtn = document.getElementById('copySiteLinkBtn');

            const loginModal = document.getElementById('loginModal');
            const passwordInput = document.getElementById('passwordInput');
            const loginError = document.getElementById('loginError');
            const loginSubmit = document.getElementById('loginSubmit');
            const loginCancel = document.getElementById('loginCancel');

            const categoryModal = document.getElementById('categoryModal');
            const catName = document.getElementById('catName');
            const catDesc = document.getElementById('catDesc');
            const catImageFile = document.getElementById('catImageFile');
            const catImageUrl = document.getElementById('catImageUrl');
            const catImagePreview = document.getElementById('catImagePreview');
            const catSubmit = document.getElementById('catSubmit');
            const catCancel = document.getElementById('catCancel');

            const productModal = document.getElementById('productModal');
            const prodModalTitle = document.getElementById('prodModalTitle');
            const prodCategory = document.getElementById('prodCategory');
            const prodName = document.getElementById('prodName');
            const prodEra = document.getElementById('prodEra');
            const prodPrice = document.getElementById('prodPrice');
            const prodOldPrice = document.getElementById('prodOldPrice');
            const prodBadge = document.getElementById('prodBadge');
            const prodImageFile = document.getElementById('prodImageFile');
            const prodImageUrl = document.getElementById('prodImageUrl');
            const prodImagePreview = document.getElementById('prodImagePreview');
            const prodSubmit = document.getElementById('prodSubmit');
            const prodCancel = document.getElementById('prodCancel');

            const catImgModal = document.getElementById('catImgModal');
            const catImgFileEdit = document.getElementById('catImgFileEdit');
            const catImgUrlEdit = document.getElementById('catImgUrlEdit');
            const catImgPreviewEdit = document.getElementById('catImgPreviewEdit');
            const catImgSubmit = document.getElementById('catImgSubmit');
            const catImgCancel = document.getElementById('catImgCancel');
            const catImgRemove = document.getElementById('catImgRemove');

            const categoriesGrid = document.getElementById('categoriesGrid');
            const featuredGrid = document.getElementById('featuredGrid');
            const reviewsList = document.getElementById('reviewsList');
            const reviewName = document.getElementById('reviewName');
            const reviewText = document.getElementById('reviewText');
            const reviewError = document.getElementById('reviewError');
            const reviewSubmit = document.getElementById('reviewSubmit');

            const mainPage = document.getElementById('mainPage');
            const categoryPage = document.getElementById('categoryPage');
            const catPageTitle = document.getElementById('catPageTitle');
            const catPageTotal = document.getElementById('catPageTotal');
            const catPageProducts = document.getElementById('catPageProducts');
            const catPageAddBtn = document.getElementById('catPageAddBtn');
            const backBtn = document.getElementById('backBtn');
            const aboutImage = document.getElementById('aboutImage');
            const aboutImgBtn = document.getElementById('aboutImgBtn');

            const siteLinkModal = document.getElementById('siteLinkModal');
            const siteLinkInput = document.getElementById('siteLinkInput');
            const siteLinkCopyBtn = document.getElementById('siteLinkCopyBtn');
            const siteLinkClose = document.getElementById('siteLinkClose');
            const toast = document.getElementById('toast');

            function loadData() {
                const saved = localStorage.getItem(STORAGE_KEY);
                if (saved) {
                    try {
                        data = JSON.parse(saved);
                        if (!data.texts) data.texts = {};
                        if (!data.reviews) data.reviews = [];
                        if (!data.aboutImage) data.aboutImage = '';
                    } catch (e) {
                        data = JSON.parse(JSON.stringify(defaultData));
                    }
                } else {
                    data = JSON.parse(JSON.stringify(defaultData));
                }
            }

            function saveData() {
                document.querySelectorAll('#mainPage [data-edit]').forEach((el, i) => {
                    data.texts['main_' + i] = el.innerHTML;
                });
                document.querySelectorAll('#categoryPage [data-edit]').forEach((el, i) => {
                    data.texts['cat_' + i] = el.innerHTML;
                });
                localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
                flashButton(saveBtn, '✓ Сохранено');
            }

            function restoreTexts() {
                document.querySelectorAll('#mainPage [data-edit]').forEach((el, i) => {
                    if (data.texts['main_' + i] !== undefined) el.innerHTML = data.texts['main_' + i];
                });
                document.querySelectorAll('#categoryPage [data-edit]').forEach((el, i) => {
                    if (data.texts['cat_' + i] !== undefined) el.innerHTML = data.texts['cat_' + i];
                });
            }

            function flashButton(btn, text) {
                const original = btn.textContent;
                btn.textContent = text;
                setTimeout(() => btn.textContent = original, 1400);
            }

            function showToast(msg) {
                toast.textContent = msg;
                toast.classList.add('show');
                clearTimeout(toast._t);
                toast._t = setTimeout(() => toast.classList.remove('show'), 2200);
            }

            function getBaseUrl() { return location.origin + location.pathname; }
            function copyToClipboard(text, onSuccess) {
                if (navigator.clipboard && window.isSecureContext) {
                    navigator.clipboard.writeText(text).then(() => { if (onSuccess) onSuccess(); }).catch(() => fallbackCopy(text, onSuccess));
                } else { fallbackCopy(text, onSuccess); }
            }
            function fallbackCopy(text, onSuccess) {
                const ta = document.createElement('textarea');
                ta.value = text; ta.style.position = 'fixed'; ta.style.opacity = '0';
                document.body.appendChild(ta); ta.select();
                try { document.execCommand('copy'); if (onSuccess) onSuccess(); } catch (e) { alert('Не удалось скопировать. Скопируйте вручную:\n' + text); }
                document.body.removeChild(ta);
            }
            function openSiteLinkModal() { siteLinkInput.value = getBaseUrl(); siteLinkModal.classList.add('open'); }

            function renderCategories() {
                categoriesGrid.innerHTML = '';
                data.categories.forEach((cat, idx) => {
                    const card = document.createElement('div');
                    card.className = 'category-card deletable';
                    card.dataset.catId = cat.id;
                    const thumbClass = cat.image ? 'category-thumb has-image' : 'category-thumb';
                    const thumbStyle = cat.image ? `style="background-image:url('${cat.image}')"` : '';
                    card.innerHTML = `
                        <div class="${thumbClass}" ${thumbStyle}></div>
                        <div class="category-index" data-edit>${romanNumeral(idx + 1)}</div>
                        <h3 data-edit>${escapeHtml(cat.name)}</h3>
                        <p data-edit>${escapeHtml(cat.desc)}</p>
                        <div class="category-count">${cat.products.length} предметов</div>
                    `;
                    card.addEventListener('click', (e) => {
                        if (e.target.closest('.admin-delete') || e.target.closest('.cat-img-edit')) return;
                        if (isAdmin && document.body.classList.contains('editing') && e.target.closest('[data-edit]')) return;
                        openCategory(cat.id);
                    });
                    if (isAdmin) {
                        const del = document.createElement('button');
                        del.className = 'admin-delete';
                        del.style.cssText = 'position:absolute;top:4px;right:4px;width:26px;height:26px;background:var(--danger);color:#fff;border:none;font-size:16px;cursor:pointer;z-index:20;display:none;';
                        del.textContent = '✕';
                        del.addEventListener('click', (e) => {
                            e.stopPropagation();
                            if (confirm('Удалить категорию «' + cat.name + '» и все её товары?')) {
                                data.categories = data.categories.filter(c => c.id !== cat.id);
                                saveData(); renderAll();
                            }
                        });
                        card.appendChild(del);

                        const imgBtn = document.createElement('button');
                        imgBtn.className = 'cat-img-edit';
                        imgBtn.style.cssText = 'position:absolute;bottom:6px;right:6px;background:rgba(63,10,19,0.9);color:var(--gold);border:1px solid var(--gold);padding:4px 8px;font-family:Caveat,cursive;font-size:14px;cursor:pointer;z-index:20;display:none;';
                        imgBtn.textContent = '📷';
                        imgBtn.addEventListener('click', (e) => { e.stopPropagation(); openCatImgModal(cat.id); });
                        card.appendChild(imgBtn);
                    }
                    categoriesGrid.appendChild(card);
                });
                updateDeleteButtons();
            }

            function renderFeatured() {
                featuredGrid.innerHTML = '';
                const all = [];
                data.categories.forEach(cat => cat.products.forEach(p => all.push({ ...p, catId: cat.id })));
                all.slice(0, 3).forEach(p => featuredGrid.appendChild(createProductCard(p, false)));
                if (all.length === 0) featuredGrid.innerHTML = '<div class="empty-message">Пока нет предметов</div>';
            }

            function createProductCard(p, showDelete) {
                const card = document.createElement('div');
                card.className = 'product-card deletable';
                const hasImg = !!p.image;
                const imgClass = hasImg ? 'product-image' : 'product-image no-photo';
                const imgStyle = hasImg ? `style="background-image:url('${p.image}')"` : '';
                card.innerHTML = `
                    <div class="${imgClass}" ${imgStyle}>
                        ${p.badge ? `<span class="product-badge" data-edit>${escapeHtml(p.badge)}</span>` : ''}
                    </div>
                    <div class="product-info">
                        <h3 data-edit>${escapeHtml(p.name)}</h3>
                        <div class="product-era" data-edit>${escapeHtml(p.era)}</div>
                        <div class="product-price">
                            <span data-edit>${escapeHtml(p.price)}</span>
                            ${p.oldPrice ? `<small data-edit>${escapeHtml(p.oldPrice)}</small>` : ''}
                        </div>
                    </div>
                `;
                if (isAdmin) {
                    const del = document.createElement('button');
                    del.className = 'admin-delete';
                    del.style.cssText = 'position:absolute;top:4px;right:4px;width:26px;height:26px;background:var(--danger);color:#fff;border:none;font-size:16px;cursor:pointer;z-index:20;display:none;';
                    del.textContent = '✕';
                    del.addEventListener('click', (e) => {
                        e.stopPropagation();
                        if (confirm('Удалить товар «' + p.name + '»?')) deleteProduct(p.id);
                    });
                    card.appendChild(del);
                }
                return card;
            }

            function openCategory(catId, prodId, fromUrl) {
                const cat = data.categories.find(c => c.id === catId);
                if (!cat) { if (!fromUrl) showToast('Категория не найдена'); return; }
                currentCategoryId = catId;
                catPageTitle.textContent = cat.name;
                catPageTitle.setAttribute('data-edit', '');
                const total = cat.products.reduce((sum, p) => sum + parsePrice(p.price), 0);
                catPageTotal.textContent = cat.products.length + ' предметов · на сумму ' + formatPrice(total);
                catPageProducts.innerHTML = '';
                if (cat.products.length === 0) {
                    catPageProducts.innerHTML = '<div class="empty-message">В этой категории пока нет предметов</div>';
                } else {
                    cat.products.forEach(p => catPageProducts.appendChild(createProductCard(p, true)));
                }
                catPageAddBtn.style.display = isAdmin ? 'block' : 'none';
                mainPage.style.display = 'none';
                categoryPage.classList.add('active');
                if (!fromUrl) history.pushState({ catId }, '', getBaseUrl() + '?category=' + encodeURIComponent(catId));
                window.scrollTo(0, 0);
                updateDeleteButtons();
            }

            function closeCategory(fromUrl) {
                categoryPage.classList.remove('active');
                mainPage.style.display = 'block';
                currentCategoryId = null;
                if (!fromUrl) history.pushState({}, '', getBaseUrl());
                window.scrollTo(0, 0);
                renderAll();
            }

            function deleteProduct(prodId) {
                data.categories.forEach(cat => { cat.products = cat.products.filter(p => p.id !== prodId); });
                saveData();
                if (currentCategoryId) openCategory(currentCategoryId, null, true);
                else renderAll();
            }

            function renderReviews() {
                reviewsList.innerHTML = '';
                if (!data.reviews || data.reviews.length === 0) {
                    reviewsList.innerHTML = '<div class="review-empty">Пока нет отзывов. Будьте первым!</div>';
                    return;
                }
                data.reviews.slice().reverse().forEach(r => {
                    const card = document.createElement('div');
                    card.className = 'review-card';
                    card.innerHTML = `
                        <div class="review-head">
                            <span class="review-author">${escapeHtml(r.name)}</span>
                            <span class="review-date">${escapeHtml(r.date)}</span>
                        </div>
                        <div class="review-text">${escapeHtml(r.text)}</div>
                    `;
                    if (isAdmin) {
                        const del = document.createElement('button');
                        del.className = 'review-delete'; del.textContent = '✕'; del.title = 'Удалить отзыв';
                        del.addEventListener('click', () => {
                            if (confirm('Удалить этот отзыв?')) {
                                data.reviews = data.reviews.filter(x => x.id !== r.id);
                                saveData(); renderReviews();
                            }
                        });
                        card.appendChild(del);
                    }
                    reviewsList.appendChild(card);
                });
            }

            function submitReview() {
                const name = reviewName.value.trim();
                const text = reviewText.value.trim();
                if (!name) { reviewError.textContent = 'Укажите имя'; reviewName.focus(); return; }
                if (!text) { reviewError.textContent = 'Напишите отзыв'; reviewText.focus(); return; }
                reviewError.textContent = '';
                const now = new Date();
                const dateStr = now.toLocaleDateString('ru-RU') + ' ' + now.toLocaleTimeString('ru-RU', { hour: '2-digit', minute: '2-digit' });
                data.reviews.push({ id: uid(), name, text, date: dateStr });
                saveData(); reviewName.value = ''; reviewText.value = '';
                renderReviews(); showToast('Спасибо за отзыв!');
            }

            function parsePrice(str) { if (!str) return 0; return parseInt(String(str).replace(/[^\d]/g, '')) || 0; }
            function formatPrice(num) { return num.toLocaleString('ru-RU') + ' ₽'; }
            function escapeHtml(str) { if (!str) return ''; return String(str).replace(/[&<>"']/g, s => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' }[s])); }
            function romanNumeral(num) { const map = ['', 'I', 'II', 'III', 'IV', 'V', 'VI', 'VII', 'VIII', 'IX', 'X', 'XI', 'XII']; return map[num] || num; }
            function uid() { return 'id_' + Date.now() + '_' + Math.random().toString(36).slice(2, 7); }

            function setEditMode(on) {
                isAdmin = on;
                document.body.classList.toggle('editing', on);
                document.querySelectorAll('[data-edit]').forEach(el => { el.setAttribute('contenteditable', on ? 'true' : 'false'); });
                adminPanel.classList.toggle('open', on);
                renderAll(); updateDeleteButtons();
            }

            function updateDeleteButtons() {
                document.querySelectorAll('.admin-delete, .cat-img-edit').forEach(btn => { btn.style.display = isAdmin ? 'block' : 'none'; });
            }

            function renderAll() {
                renderCategories(); renderFeatured(); renderReviews(); restoreTexts();
                if (data.aboutImage) aboutImage.style.backgroundImage = `url('${data.aboutImage}')`;
                else aboutImage.style.backgroundImage = "url('https://images.unsplash.com/photo-1565791380713-1756b9a05343?w=800')";
                if (isAdmin) { document.querySelectorAll('[data-edit]').forEach(el => el.setAttribute('contenteditable', 'true')); }
                updateDeleteButtons();
            }

            function openModal(m) { m.classList.add('open'); }
            function closeModal(m) { m.classList.remove('open'); }

            let lastTap = 0;
            adminTrigger.addEventListener('click', () => {
                const now = Date.now();
                if (now - lastTap < 400) { if (isAdmin) adminPanel.classList.toggle('open'); else openLogin(); }
                lastTap = now;
            });
            adminTrigger.addEventListener('dblclick', (e) => { e.preventDefault(); if (isAdmin) adminPanel.classList.toggle('open'); else openLogin(); });

            function openLogin() { loginError.textContent = ''; passwordInput.value = ''; openModal(loginModal); setTimeout(() => passwordInput.focus(), 100); }
            function tryLogin() {
                if (passwordInput.value === PASSWORD) { closeModal(loginModal); setEditMode(true); }
                else { loginError.textContent = 'Неверный пароль'; passwordInput.value = ''; passwordInput.focus(); }
            }

            function readImageFile(file, callback) {
                if (!file) return;
                if (!file.type.startsWith('image/')) { alert('Выберите изображение'); return; }
                if (file.size > 3 * 1024 * 1024) { alert('Файл больше 3 МБ'); return; }
                const reader = new FileReader();
                reader.onload = e => callback(e.target.result);
                reader.readAsDataURL(file);
            }

            function setupImagePreview(fileInput, urlInput, previewImg, onChange) {
                fileInput.addEventListener('change', () => {
                    readImageFile(fileInput.files[0], (dataUrl) => {
                        if (onChange) onChange(dataUrl);
                        previewImg.src = dataUrl; previewImg.classList.add('show');
                        if (urlInput) urlInput.value = '';
                    });
                });
                if (urlInput) {
                    urlInput.addEventListener('input', () => {
                        const url = urlInput.value.trim();
                        if (url) { if (onChange) onChange(url); previewImg.src = url; previewImg.classList.add('show'); fileInput.value = ''; }
                        else { previewImg.classList.remove('show'); if (onChange) onChange(''); }
                    });
                }
            }

            function openCategoryModal() {
                catName.value = ''; catDesc.value = ''; catImageFile.value = ''; catImageUrl.value = '';
                catImagePreview.classList.remove('show'); pendingCatImage = '';
                openModal(categoryModal); setTimeout(() => catName.focus(), 100);
            }

            setupImagePreview(catImageFile, catImageUrl, catImagePreview, v => pendingCatImage = v);

            function submitCategory() {
                const name = catName.value.trim();
                if (!name) { catName.focus(); return; }
                data.categories.push({ id: uid(), name: name, desc: catDesc.value.trim() || 'Без описания', image: pendingCatImage || '', products: [] });
                saveData(); closeModal(categoryModal); renderAll(); showToast('Категория добавлена');
            }

            function openCatImgModal(catId) {
                const cat = data.categories.find(c => c.id === catId);
                if (!cat) return;
                editCatImageId = catId; catImgFileEdit.value = ''; catImgUrlEdit.value = cat.image || '';
                if (cat.image) { catImgPreviewEdit.src = cat.image; catImgPreviewEdit.classList.add('show'); }
                else catImgPreviewEdit.classList.remove('show');
                openModal(catImgModal);
            }

            setupImagePreview(catImgFileEdit, catImgUrlEdit, catImgPreviewEdit);

            function submitCatImage() {
                const cat = data.categories.find(c => c.id === editCatImageId);
                if (!cat) return;
                if (catImgFileEdit.files[0]) { readImageFile(catImgFileEdit.files[0], (dataUrl) => { cat.image = dataUrl; saveData(); closeModal(catImgModal); renderAll(); }); }
                else { cat.image = catImgUrlEdit.value.trim(); saveData(); closeModal(catImgModal); renderAll(); }
            }

            function removeCatImage() {
                const cat = data.categories.find(c => c.id === editCatImageId);
                if (!cat) return;
                cat.image = ''; saveData(); closeModal(catImgModal); renderAll();
            }

            function fillCategorySelect() {
                prodCategory.innerHTML = '';
                data.categories.forEach(cat => {
                    const opt = document.createElement('option'); opt.value = cat.id; opt.textContent = cat.name;
                    prodCategory.appendChild(opt);
                });
            }

            function openProductModal(catId, prodId) {
                fillCategorySelect(); editingProductId = prodId || null;
                pendingProdImage = ''; prodImageFile.value = ''; prodImageUrl.value = ''; prodImagePreview.classList.remove('show');

                if (prodId) {
                    let found = null;
                    for (const c of data.categories) { const p = c.products.find(x => x.id === prodId); if (p) { found = { p, catId: c.id }; break; } }
                    if (!found) return;
                    prodModalTitle.textContent = 'Редактировать товар'; prodSubmit.textContent = 'Сохранить';
                    prodCategory.value = found.catId; prodName.value = found.p.name; prodEra.value = found.p.era;
                    prodPrice.value = found.p.price; prodOldPrice.value = found.p.oldPrice || ''; prodBadge.value = found.p.badge || '';
                    if (found.p.image) { prodImagePreview.src = found.p.image; prodImagePreview.classList.add('show'); pendingProdImage = found.p.image; }
                } else {
                    prodModalTitle.textContent = 'Новый товар'; prodSubmit.textContent = 'Добавить';
                    if (catId) prodCategory.value = catId;
                    prodName.value = ''; prodEra.value = ''; prodPrice.value = ''; prodOldPrice.value = ''; prodBadge.value = '';
                }
                openModal(productModal); setTimeout(() => prodName.focus(), 100);
            }

            setupImagePreview(prodImageFile, prodImageUrl, prodImagePreview, v => pendingProdImage = v);

            function submitProduct() {
                const name = prodName.value.trim(); const price = prodPrice.value.trim();
                if (!name) { prodName.focus(); return; } if (!price) { prodPrice.focus(); return; }
                const catId = prodCategory.value; const cat = data.categories.find(c => c.id === catId);
                if (!cat) return;
                let finalImage = pendingProdImage;
                if (prodImageFile.files[0]) { readImageFile(prodImageFile.files[0], (dataUrl) => { saveProductWithImage(name, price, cat, dataUrl); }); return; }
                if (prodImageUrl.value.trim()) finalImage = prodImageUrl.value.trim();
                saveProductWithImage(name, price, cat, finalImage);
            }

            function saveProductWithImage(name, price, cat, image) {
                const prodData = { name: name, era: prodEra.value.trim() || '—', price: price, oldPrice: prodOldPrice.value.trim(), badge: prodBadge.value.trim(), image: image || '' };
                if (editingProductId) { data.categories.forEach(c => { c.products = c.products.filter(p => p.id !== editingProductId); }); cat.products.push({ id: editingProductId, ...prodData }); }
                else { cat.products.push({ id: uid(), ...prodData }); }
                saveData(); closeModal(productModal);
                if (currentCategoryId) openCategory(currentCategoryId, null, true); else renderAll();
                showToast('Товар сохранён');
            }

            aboutImgBtn.addEventListener('click', () => {
                const url = prompt('Вставьте ссылку на фото для раздела «О нас»:', data.aboutImage || '');
                if (url === null) return;
                data.aboutImage = url.trim(); saveData(); renderAll();
            });

            function resetData() {
                if (!confirm('Сбросить все изменения и вернуть исходный вид сайта?')) return;
                localStorage.removeItem(STORAGE_KEY); location.href = getBaseUrl();
            }

            saveBtn.addEventListener('click', saveData);
            resetBtn.addEventListener('click', resetData);
            logoutBtn.addEventListener('click', () => { if (confirm('Выйти из режима администратора?')) setEditMode(false); });
            addCategoryBtn.addEventListener('click', openCategoryModal);
            addProductBtn.addEventListener('click', () => openProductModal(currentCategoryId, null));
            copySiteLinkBtn.addEventListener('click', openSiteLinkModal);

            loginSubmit.addEventListener('click', tryLogin);
            loginCancel.addEventListener('click', () => closeModal(loginModal));
            passwordInput.addEventListener('keydown', e => { if (e.key === 'Enter') tryLogin(); if (e.key === 'Escape') closeModal(loginModal); });
            loginModal.addEventListener('click', e => { if (e.target === loginModal) closeModal(loginModal); });

            catSubmit.addEventListener('click', submitCategory);
            catCancel.addEventListener('click', () => closeModal(categoryModal));
            catName.addEventListener('keydown', e => { if (e.key === 'Enter') submitCategory(); });
            categoryModal.addEventListener('click', e => { if (e.target === categoryModal) closeModal(categoryModal); });

            prodSubmit.addEventListener('click', submitProduct);
            prodCancel.addEventListener('click', () => closeModal(productModal));
            productModal.addEventListener('click', e => { if (e.target === productModal) closeModal(productModal); });

            catImgSubmit.addEventListener('click', submitCatImage);
            catImgCancel.addEventListener('click', () => closeModal(catImgModal));
            catImgRemove.addEventListener('click', removeCatImage);
            catImgModal.addEventListener('click', e => { if (e.target === catImgModal) closeModal(catImgModal); });

            backBtn.addEventListener('click', () => closeCategory(false));
            catPageAddBtn.addEventListener('click', () => openProductModal(currentCategoryId, null));

            reviewSubmit.addEventListener('click', submitReview);

            siteLinkCopyBtn.addEventListener('click', () => {
                copyToClipboard(siteLinkInput.value, () => {
                    showToast('Ссылка скопирована');
                    siteLinkCopyBtn.textContent = '✓ Готово';
                    setTimeout(() => siteLinkCopyBtn.textContent = 'Копировать', 1500);
                });
            });
            siteLinkClose.addEventListener('click', () => closeModal(siteLinkModal));
            siteLinkModal.addEventListener('click', e => { if (e.target === siteLinkModal) closeModal(siteLinkModal); });

            document.querySelectorAll('nav a[href^="#"]').forEach(a => {
                a.addEventListener('click', (e) => {
                    const href = a.getAttribute('href');
                    if (href.length > 1) { e.preventDefault(); const target = document.querySelector(href); if (target) target.scrollIntoView({ behavior: 'smooth' }); }
                });
            });

            document.addEventListener('keydown', e => {
                if ((e.ctrlKey || e.metaKey) && e.key.toLowerCase() === 's' && isAdmin) { e.preventDefault(); saveData(); }
                if (e.key === 'Escape' && isAdmin && !document.querySelector('.modal-overlay.open')) { setEditMode(false); }
            });

            function handleUrl() {
                const params = new URLSearchParams(location.search);
                const catId = params.get('category');
                if (catId) { const cat = data.categories.find(c => c.id === catId); if (cat) { openCategory(catId, null, true); return; } }
                mainPage.style.display = 'block'; categoryPage.classList.remove('active');
            }

            window.addEventListener('popstate', () => {
                const params = new URLSearchParams(location.search);
                const catId = params.get('category');
                if (catId) { const cat = data.categories.find(c => c.id === catId); if (cat) { openCategory(catId, null, true); return; } }
                categoryPage.classList.remove('active'); mainPage.style.display = 'block'; currentCategoryId = null; renderAll();
            });

            loadData(); renderAll(); setEditMode(false); handleUrl();
        })();
    </script>
</body>
</html>
