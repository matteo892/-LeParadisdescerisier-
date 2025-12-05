<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>NebulaFeed</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f1f3f4;
            margin: 0;
            padding: 0;
        }

        header {
            background: linear-gradient(90deg, #7f00ff, #e100ff);
            color: white;
            padding: 15px;
            font-size: 26px;
            font-weight: bold;
            text-align: center;
            letter-spacing: 1px;
        }

        .container {
            width: 650px;
            margin: 25px auto;
        }

        /* Formulaire */
        .post-form {
            background: white;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .post-form input, .post-form textarea, .post-form select {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
        }

        .post-form button {
            background: #7f00ff;
            color: white;
            padding: 10px 15px;
            border: none;
            cursor: pointer;
            border-radius: 4px;
            font-size: 16px;
        }

        /* Post */
        .post {
            background: white;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        .post img {
            max-width: 100%;
            margin-top: 10px;
            border-radius: 4px;
        }

        .title {
            font-weight: bold;
            font-size: 18px;
        }

        .category {
            background: #e100ff33;
            display: inline-block;
            padding: 3px 7px;
            border-radius: 5px;
            margin-bottom: 10px;
            font-size: 12px;
            color: #700070;
        }

        .content {
            margin-top: 5px;
        }

        .vote-box {
            margin-top: 10px;
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 20px;
        }

        .vote-btn {
            cursor: pointer;
            font-size: 22px;
            user-select: none;
        }

        .vote-score {
            font-weight: bold;
        }
    </style>
</head>

<body>

<header>NebulaFeed</header>

<div class="container">

    <!-- Formulaire de poste -->
    <div class="post-form">
        <input type="text" id="title" placeholder="Titre du post">

        <textarea id="content" placeholder="Contenu du post"></textarea>

        <select id="category">
            <option value="Général">Général</option>
            <option value="Meme">Meme</option>
            <option value="Gaming">Gaming</option>
            <option value="Technologie">Technologie</option>
            <option value="Art">Art</option>
        </select>

        <label>Importer une image :</label>
        <input type="file" id="imageFile" accept="image/*">

        <button onclick="addPost()">Publier</button>
    </div>

    <div id="posts"></div>

</div>

<script>
    function addPost() {
        const title = document.getElementById("title").value;
        const content = document.getElementById("content").value;
        const category = document.getElementById("category").value;
        const fileInput = document.getElementById("imageFile");

        if (!title || !content) {
            alert("Titre et contenu obligatoires !");
            return;
        }

        let imageURL = "";
        if (fileInput.files.length > 0) {
            const file = fileInput.files[0];
            imageURL = URL.createObjectURL(file); // Permet d'afficher l'image locale
        }

        const postsDiv = document.getElementById("posts");

        // Création du post
        const post = document.createElement("div");
        post.className = "post";

        const id = Date.now(); // Un ID unique pour gérer les votes

        post.innerHTML = `
            <div class="category">${category}</div>
            <div class="title">${title}</div>
            <div class="content">${content}</div>
            ${imageURL ? `<img src="${imageURL}" alt="image du post">` : ""}

            <div class="vote-box">
                <span class="vote-btn" onclick="vote(${id}, 1)">⬆️</span>
                <span id="score-${id}" class="vote-score">0</span>
                <span class="vote-btn" onclick="vote(${id}, -1)">⬇️</span>
            </div>
        `;

        postsDiv.prepend(post);

        // Reset
        document.getElementById("title").value = "";
        document.getElementById("content").value = "";
        document.getElementById("category").value = "Général";
        fileInput.value = "";
    }

    // Système de vote simple
    function vote(id, value) {
        const scoreSpan = document.getElementById("score-" + id);
        let score = parseInt(scoreSpan.innerText);
        score += value;
        scoreSpan.innerText = score;
    }
</script>

</body>
</html>
