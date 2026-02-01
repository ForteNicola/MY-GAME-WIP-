[YouTube.html](https://github.com/user-attachments/files/24994004/YouTube.html)
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>Anonima - Split View</title>
    <style>
        body { 
            font-family: 'Courier New', Courier, monospace; 
            margin: 0; 
            background: #121212; 
            color: #e0e0e0; 
            display: flex; 
            height: 100vh; 
            overflow: hidden; 
        }

        /* Colonna Sinistra: I POST */
        #left-side { 
            flex: 0 0 60%; 
            padding: 20px; 
            overflow-y: auto; 
            border-right: 1px solid #333; 
        }

        /* Colonna Destra: SCRIVI */
        #right-side { 
            flex: 0 0 40%; 
            padding: 20px; 
            background: #1a1a1a; 
            display: flex; 
            flex-direction: column; 
        }

        .box { 
            background: #222; 
            padding: 15px; 
            border-radius: 4px; 
            margin-bottom: 20px; 
            border: 1px solid #444; 
        }

        textarea { 
            width: 100%; 
            height: 150px; 
            background: #333; 
            color: #00ff00; 
            border: 1px solid #555; 
            padding: 10px; 
            box-sizing: border-box; 
            resize: none; 
            font-family: inherit;
        }

        button { 
            background: #333; 
            color: #00ff00; 
            border: 1px solid #00ff00; 
            padding: 12px; 
            cursor: pointer; 
            margin-top: 10px; 
            font-weight: bold;
            width: 100%;
        }

        button:hover { background: #00ff00; color: black; }

        .post-header { font-size: 0.75em; color: #888; margin-bottom: 8px; }
        
        .comment-section { 
            margin-top: 15px; 
            padding-left: 10px; 
            border-left: 1px solid #444; 
            font-size: 0.85em;
        }

        .comment { color: #aaa; margin-bottom: 5px; border-bottom: 1px solid #222; }

        input[type="text"] { 
            background: #222; 
            color: white; 
            border: 1px solid #444; 
            padding: 5px; 
            width: calc(100% - 90px); 
        }

        .btn-sm { width: auto; padding: 5px 10px; font-size: 0.8em; }

        h2 { border-bottom: 1px solid #00ff00; padding-bottom: 10px; color: #00ff00; }
    </style>
</head>
<body>

    <div id="left-side">
        <h2>Messaggi Recenti</h2>
        <div id="feed"></div>
    </div>

    <div id="right-side">
        <h2>Nuovo Post Anonimo</h2>
        <div class="box">
            <textarea id="postInput" placeholder="Cosa vuoi dire al mondo?"></textarea>
            <button onclick="addPost()">INVIA ORA</button>
        </div>
        <p style="font-size: 0.8em; color: #666;">
            I tuoi progressi vengono salvati localmente. Nessuno sa chi sei.
        </p>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', displayPosts);

        function addPost() {
            const text = document.getElementById('postInput').value;
            if(!text.trim()) return;
            
            const posts = JSON.parse(localStorage.getItem('anon_posts') || '[]');
            posts.unshift({ 
                text: text, 
                date: new Date().toLocaleTimeString(), 
                comments: [] 
            });
            localStorage.setItem('anon_posts', JSON.stringify(posts));
            
            document.getElementById('postInput').value = '';
            displayPosts();
        }

        function addComment(index) {
            const commentInput = document.getElementById(`comment-${index}`);
            const text = commentInput.value;
            if(!text.trim()) return;

            const posts = JSON.parse(localStorage.getItem('anon_posts'));
            posts[index].comments.push(text);
            localStorage.setItem('anon_posts', JSON.stringify(posts));

            displayPosts();
        }

        function displayPosts() {
            const feed = document.getElementById('feed');
            const posts = JSON.parse(localStorage.getItem('anon_posts') || '[]');
            feed.innerHTML = posts.map((post, i) => `
                <div class="box">
                    <div class="post-header">Anonimo • ${post.date}</div>
                    <div style="white-space: pre-wrap;">${post.text}</div>
                    
                    <div class="comment-section">
                        ${post.comments.map(c => `<div class="comment">👤 ${c}</div>`).join('')}
                        <div style="margin-top:10px;">
                            <input type="text" id="comment-${i}" placeholder="Commenta...">
                            <button class="btn-sm" onclick="addComment(${i})">Rispondi</button>
                        </div>
                    </div>
                </div>
            `).join('');
        }
    </script>
</body>
</html>
