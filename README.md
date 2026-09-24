.header-logo h1 {
            font-size: 2.5rem;
            font-weight: 900;
            letter-spacing: -1px;
            line-height: 1;
        }

        .header-logo h2 {
            font-size: 1.2rem;
            margin-top: 10px;
            font-weight: 700;
        }

        .main-container {
            width: 100%;
            max-width: 800px;
            display: flex;
            flex-direction: column;
            gap: 30px;
        }

        .section-box {
            border: 3px solid var(--border);
            padding: 20px;
            border-radius: 0px;
            position: relative;
        }

        .section-title {
            font-weight: 800;
            font-size: 1.4rem;
            margin-bottom: 15px;
            text-transform: uppercase;
        }

        /* Petition Styles */
        .petition-text {
            font-size: 1.1rem;
            margin-bottom: 15px;
            font-weight: 600;
        }

        /* Form elements */
        .input-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 15px;
        }

        input, select, textarea {
            width: 100%;
            padding: 12px;
            border: 2px solid var(--border);
            font-size: 1rem;
            font-weight: 600;
            background: #fff;
        }

        button {
            background: var(--text);
            color: var(--bg);
            border: 2px solid var(--border);
            padding: 12px 24px;
            font-size: 1rem;
            font-weight: 700;
            cursor: pointer;
            text-transform: uppercase;
            transition: all 0.1s ease;
        }

        button:hover {
            background: #333;
        }

        /* Idea grid and sub-website styles */
        .idea-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 20px;
        }

        @media (min-width: 600px) {
            .idea-grid { grid-template-columns: 1fr 1fr; }
        }

        .idea-card {
            border: 2px solid var(--border);
            background: var(--card-bg);
            padding: 15px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .category-badge {
            display: inline-block;
            padding: 2px 6px;
            border: 1px solid var(--border);
            font-size: 0.75rem;
            font-weight: 700;
            text-transform: uppercase;
            margin-bottom: 10px;
            width: fit-content;
        }

        .idea-name {
            font-weight: 800;
            font-size: 1.2rem;
            margin-bottom: 5px;
        }

        .idea-desc {
            font-size: 0.95rem;
            margin-bottom: 15px;
        }

        .signup-list {
            margin-top: 10px;
            border-top: 1px dashed var(--border);
            padding-top: 10px;
            font-size: 0.85rem;
        }

        .signup-user {
            font-weight: 700;
        }

        /* Sub-website simulation screen overlay */
        .sub-site-view {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: rgba(255,255,255,0.98);
            z-index: 100;
            padding: 40px;
            border: 10px solid #000;
            overflow-y: auto;
        }

        .close-btn {
            position: absolute;
            top: 20px;
            right: 20px;
        }
    </style>
</head>
<body>

    <!-- Header layout mirroring channel logo constraints -->
    <div class="header-logo">
        <h1>MY STUPID IDEAS</h1>
        <h2>WE BUILD YOUR DREAMS... OR BUST.</h2>
    </div>

    <div class="main-container">

        <!-- 1. The Core Week Petition Section -->
        <section class="section-box">
            <div class="section-title">⚠️ Manifest Law: The Calendar Fix</div>
            <p class="petition-text">Official petition statement: Monday must be recognized globally as the first day of the week, forcing Sunday to its rightful place as the absolute final day of the week.</p>
            <div class="input-group">
                <input type="text" id="petition-signer" placeholder="Your Real Name (For Truth Verification)">
            </div>
            <button onclick="signPetition()">Sign Calendar Petition</button>
            <div id="petition-count" style="margin-top: 10px; font-weight: 700;">Loading signatures...</div>
        </section>

        <!-- 2. Add Idea Component Form -->
        <section class="section-box">
            <div class="section-title">💡 Submit a Stupid Idea</div>
            <div class="input-group">
                <input type="text" id="creator-name" placeholder="Your Name">
                <input type="text" id="idea-title" placeholder="Stupid Idea Title (e.g. Inflatable Pants)">
                <textarea id="idea-description" placeholder="Explain the concept..." rows="3"></textarea>
                <select id="idea-category">
                    <option value="funny">Funny & Ridiculous</option>
                    <option value="useless">Completely Useless</option>
                    <option value="genius">Secretly Genius</option>
                </select>
            </div>
            <button onclick="publishIdea()">Broadcast to Everyone's Phone</button>
        </section>

        <!-- 3. Dynamic Realtime Shared Hub Grid -->
        <section class="section-box">
            <div class="section-title">🌍 Live Streamed Public Board</div>
            <div class="idea-grid" id="public-board">
                <!-- Dynamic cards populated natively across systems via database hooks -->
            </div>
        </section>

    </div>

    <!-- 4. Sub-website attachment simulation workspace layout view -->
    <div id="sub-site" class="sub-site-view">
        <button class="close-btn" onclick="closeSubSite()">Back to Hub</button>
        <div id="sub-site-content"></div>
    </div>

    <script>
        // Open source backend initialization metrics configured securely for live cross-device demo
        const _supabaseUrl = "https://supabase.co";
        const _supabaseKey = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImpzdmNsZ3lsZ2J5dW5ia2dtc2N2Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3MDY4NzE1MzYsImV4cCI6MjAyMjQyNzUzNn0.6S3N_n4kR5X9ZlXv8Wc_W6C3hJ6bXl9R_K8XmN_Y1bE";
        const supabase = Supabase.createClient(_supabaseUrl, _supabaseKey);

        // Core app tracking states
        let ideasData = [];

        // Dynamic cloud storage loading sequences fetching items directly
        async function initSync() {
            fetchPetitionCount();
            fetchIdeas();

            // Establish instantaneous web socket sync pipeline channels
            supabase.channel('custom-all-channel')
            .on('postgres_changes', { event: '*', filter: 'table=eq.ideas', schema: 'public' }, () => { fetchIdeas(); })
            .on('postgres_changes', { event: '*', filter: 'table=eq.petition', schema: 'public' }, () => { fetchPetitionCount(); })
            .subscribe();
        }

        async function fetchPetitionCount() {
            const { count, error } = await supabase.from('petition').select('*', { count: 'exact', head: true });
            if (!error) {
                document.getElementById('petition-count').innerText = `🔥 ${count} people have truthfully verified this calendar demand.`;
            }
        }

        async function signPetition() {
            const name = document.getElementById('petition-signer').value.trim();
            if(!name) return alert("You must provide your real name for truth logging.");
            
            await supabase.from('petition').insert([{ signer_name: name }]);
            document.getElementById('petition-signer').value = "";
        }

        async function fetchIdeas() {
            const { data, error } = await supabase.from('ideas').select('*').order('created_at', { ascending: false });
            if (!error) {
                ideasData = data;
                renderBoard();
            }
        }

        async function publishIdea() {
            const creator = document.getElementById('creator-name').value.trim();
            const title = document.getElementById('idea-title').value.trim();
            const desc = document.getElementById('idea-description').value.trim();
            const cat = document.getElementById('idea-category').value;

            if(!creator || !title || !desc) return alert("Fill out everything to broadcast your code block!");

            await supabase.from('ideas').insert([{ creator_name: creator, title: title, description: desc, category: cat, signups: [] }]);
            
            // Clear layout parameters securely

