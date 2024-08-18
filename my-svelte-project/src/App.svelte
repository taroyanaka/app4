<script>
/////////////////////////////client/////////////////////////////
/////////////////////////////client/////////////////////////////
/////////////////////////////client/////////////////////////////

	
	
const test_app4_text = `https://www.google.com
https://www.amazon.co.jp
https://www.apple.com/jp
https://www.microsoft.com/ja-jp
https://www.facebook.com`;

let is_editing_app4_title = false;
let url_list = '';


		let open_volume = 1;
		let url_list_lines = [];
		let options = [];
		let urls = [];

		// Reactive updates
		$: {
			url_list_lines = url_list.split('\n').filter(line => line.trim() !== '');
			if (url_list_lines.length > 100) {
				error_message = 'URLリストは100行までです。';
			} else {
				error_message = '';
			}
			options = Array.from({ length: url_list_lines.length }, (_, i) => i + 1);
			if (options.length > 0) {
				open_volume = Math.max(open_volume, options[options.length - 1]);
			}
		}
		function service_tab_open(url) {
			window.open(url, '_blank');
		}
		async function service_exe() {
			urls = url_list_lines.slice(0, open_volume);
			urls.forEach(url => service_tab_open(url));
		}
		function service_toggle_edit_app4_title() {
			is_editing_app4_title = !is_editing_app4_title;
		}
		function service_update_app4_title() {
			is_editing_app4_title = false;
		}
/////////////////////////////client/////////////////////////////
/////////////////////////////client/////////////////////////////
/////////////////////////////client/////////////////////////////

	
	
	
	
	
	
	
	
	{
/////////////////////////////server_express.js/////////////////////////////
/////////////////////////////server_express.js/////////////////////////////
/////////////////////////////server_express.js/////////////////////////////

/////////////////////////////AI/////////////////////////////
// 以下のコードを以下のルールで書き換えて
// pop_up_url => app4
// app4_title => app4_title TEXT not nullで1文字以上100文字以下
// url_list => app4_text TEXT nullで1文字以上1000文字以下
/////////////////////////////AI/////////////////////////////
	
// const crypto = require('crypto');

// const db_for_app4 = new sqlite('app4.db');

// const initializeDatabase_app4 = () => {
//     db_for_app4.exec('DROP TABLE IF EXISTS app4');
//     db_for_app4.exec(`
//         CREATE TABLE app4 (
//             id INTEGER PRIMARY KEY AUTOINCREMENT,
//             uid TEXT NOT NULL CHECK(length(uid) = 64), -- SHA-256 produces a 64-character hex string
//             app4_title TEXT NOT NULL CHECK(length(app4_title) >= 1 AND length(app4_title) <= 100),
//             app4_text TEXT NULL CHECK(length(app4_text) >= 1 AND length(app4_text) <= 1000),
//             created TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
//             updated TIMESTAMP DEFAULT CURRENT_TIMESTAMP
//         );
//     `);
// };
// initializeDatabase_app4();

// app.post('/app4/init-database', (req, res) => {
//     const { password } = req.body;

//     if (password === 'init') {
//         try {
//             initializeDatabase_app4();
//             res.status(200).json({ message: 'Database initialized successfully.' });
//         } catch (error) {
//             res.status(500).json({ error: 'Failed to initialize database.' });
//         }
//     } else {
//         res.status(403).json({ error: 'Unauthorized: Invalid password.' });
//     }
// });

// app.post('/app4/create', (req, res) => {
//     const hashUid = (uid) => {
//         return crypto.createHash('sha256').update(uid).digest('hex');
//     };

//     const { uid, app4_title, app4_text } = req.body;

//     if (typeof uid !== 'string' || uid.length < 1 || uid.length > 50) {
//         return res.status(400).json({ error: 'Invalid uid. It must be a string between 1 and 50 characters.' });
//     }

//     if (typeof app4_title !== 'string' || app4_title.length < 1 || app4_title.length > 100) {
//         return res.status(400).json({ error: 'Invalid app4_title. It must be a string between 1 and 100 characters.' });
//     }

//     if (app4_text && (typeof app4_text !== 'string' || app4_text.length < 1 || app4_text.length > 1000)) {
//         return res.status(400).json({ error: 'Invalid app4_text. It must be a string between 1 and 1000 characters.' });
//     }

//     const hashedUid = hashUid(uid);

//     const stmt = db_for_app4.prepare('INSERT INTO app4 (uid, app4_title, app4_text) VALUES (?, ?, ?)');
//     const result = stmt.run(hashedUid, app4_title, app4_text || null);

//     return res.status(201).json({
//         id: result.lastInsertRowid,
//         uid: hashedUid,
//         app4_title,
//         app4_text,
//         created: new Date().toISOString(),
//         updated: new Date().toISOString()
//     });
// });

// app.post('/app4/read', (req, res) => {
//     const hashUid = (uid) => {
//         return crypto.createHash('sha256').update(uid).digest('hex');
//     };

//     const { uid } = req.body;

//     try {
//         const stmt_all = db_for_app4.prepare('SELECT * FROM app4');
//         const all_json = stmt_all.all();

//         if (uid) {
//             const hashedUid = hashUid(uid);
//             const stmt_uid = db_for_app4.prepare('SELECT * FROM app4 WHERE uid = ?');
//             const uid_json = stmt_uid.all(hashedUid);
//             res.status(200).json({ uid_json, all_json });
//         } else {
//             res.status(200).json({ all_json });
//         }
//     } catch (error) {
//         res.status(500).json({ error: 'Failed to retrieve records.' });
//     }
// });

// app.post('/app4/update', (req, res) => {
//     const hashUid = (uid) => {
//         return crypto.createHash('sha256').update(uid).digest('hex');
//     };

//     const { id, uid, app4_title, app4_text } = req.body;

//     if (typeof id !== 'number' || typeof uid !== 'string' || uid.length < 1 || uid.length > 50 ||
//         typeof app4_title !== 'string' || app4_title.length < 1 || app4_title.length > 100 ||
//         (app4_text && (typeof app4_text !== 'string' || app4_text.length < 1 || app4_text.length > 1000))) {
//         return res.status(400).json({ error: 'Invalid input.' });
//     }

//     const hashedUid = hashUid(uid);

//     const stmt = db_for_app4.prepare('UPDATE app4 SET app4_title = ?, app4_text = ?, updated = CURRENT_TIMESTAMP WHERE id = ? AND uid = ?');
//     const result = stmt.run(app4_title, app4_text || null, id, hashedUid);

//     if (result.changes > 0) {
//         res.status(200).json({ message: 'Record updated successfully.' });
//     } else {
//         res.status(404).json({ error: 'Record not found or UID mismatch.' });
//     }
// });

// app.post('/app4/delete', (req, res) => {
//     const hashUid = (uid) => {
//         return crypto.createHash('sha256').update(uid).digest('hex');
//     };

//     const { id, uid } = req.body;

//     if (typeof id !== 'number' || typeof uid !== 'string') {
//         return res.status(400).json({ error: 'Invalid input.' });
//     }

//     const hashedUid = hashUid(uid);

//     const stmt = db_for_app4.prepare('DELETE FROM app4 WHERE id = ? AND uid = ?');
//     const result = stmt.run(id, hashedUid);

//     if (result.changes > 0) {
//         res.status(200).json({ message: 'Record deleted successfully.' });
//     } else {
//         res.status(404).json({ error: 'Record not found or UID mismatch.' });
//     }
// });
	
	
	/////////////////////////////server_express.js/////////////////////////////
	/////////////////////////////server_express.js/////////////////////////////
	/////////////////////////////server_express.js/////////////////////////////
	};
	
	
	
	/////////////////////////////server/////////////////////////////
	/////////////////////////////server/////////////////////////////
	/////////////////////////////server/////////////////////////////


/////////////////////////////server/////////////////////////////
/////////////////////////////server/////////////////////////////
/////////////////////////////server/////////////////////////////
// 以下をapp4用に書き換えて
/////////////////////////////server/////////////////////////////
/////////////////////////////server/////////////////////////////
/////////////////////////////server/////////////////////////////


	let app4_title = 'GAFAM';
let app4_text = `https://www.google.com
https://www.amazon.com
https://www.apple.com
https://www.microsoft.com
https://www.facebook.com`;
let convert_data_0 = null;
let convert_data_1 = null;

// convert_dataの引数の初期値をapp4_title, app4_textに設定
function convert_data(data_0 = app4_title, data_1 = app4_text) {
    convert_data_0 = data_0;
    convert_data_1 = data_1;
}

const service_name = 'App4';
	
	
	
	
	
	import { onMount } from 'svelte';
		const firebase_config = {
			apiKey: "AIzaSyBcOlIDP2KWbJuKM0WeMHNp-WvjTVfLt9Y",
			authDomain: "p2auth-ea50a.firebaseapp.com",
			projectId: "p2auth-ea50a",
			storageBucket: "p2auth-ea50a.appspot.com",
			messagingSenderId: "796225429484",
			appId: "1:796225429484:web:ece56ef2fc0be28cd6eac9"
		};
		firebase.initializeApp(firebase_config);
		const google_provider = new firebase.auth.GoogleAuthProvider();
		let error_message = '';
		let user = null;
		let uid = "";
		let login_result = 'Not logged in';
		let web_data = [];
	
		function check_login() {
			firebase.auth().onAuthStateChanged(current_user => {
				user = current_user;
				if (user) {
					login_result = `Logged in as: ${user.displayName}`;
					uid = user.uid;
					fetch_data(); // Refresh the data
				} else {
					login_result = 'Not logged in';
					uid = "";
				}
			});
		}
	
		function google_login() {
			firebase.auth().signInWithPopup(google_provider).then(result => {
				user = result.user;
				login_result = `Logged in as: ${user.displayName}`;
			}).catch(error => {
				console.error('Error during Google login:', error);
				alert('Google login failed. ' + error.message);
			});
		}
	
		function sign_out() {
			firebase.auth().signOut().then(() => {
				user = null;
				login_result = 'Not logged in';
			}).catch(error => {
				console.error('Error during sign-out:', error);
				alert('Sign out failed. ' + error.message);
			});
		}
	
		async function fetch_data() {
    try {
        console.log('fetch_data');
        const response = await fetch('https://cotton-concrete-catsup.glitch.me/app4/read', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ uid })
        });
        const data = await response.json();
        web_data = data.all_json;
    } catch (error) {
        console.error("Error fetching data:", error);
    }
}

async function create_record() {
    try {
        convert_data(app4_title, app4_text);
        const response = await fetch('https://cotton-concrete-catsup.glitch.me/app4/create', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ 
                uid: uid,
                app4_title: convert_data_0,
                app4_text: convert_data_1,
            })
        });
        const data = await response.json();
        console.log('Record created:', data);
        fetch_data(); // Refresh the data
    } catch (error) {
        console.error('Error creating record:', error);
    }
}

async function update_record(id) {
    try {
        convert_data(app4_title, app4_text);
        // confirmしてから実行。yes出ない場合は何もしない
        if (confirm('更新しますか？')) {
            const response = await fetch('https://cotton-concrete-catsup.glitch.me/app4/update', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    id: id,
                    uid: uid,
                    app4_title: convert_data_0,
                    app4_text: convert_data_1,
                })
            });
            const data = await response.json();
            console.log('Record updated:', data);
            fetch_data(); // Refresh the data
        } else {
            return;
        }
    } catch (error) {
        console.error('Error updating record:', error);
    }
}

async function delete_record(id) {
    try {
        const response = await fetch('https://cotton-concrete-catsup.glitch.me/app4/delete', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ id, uid })
        });
        const data = await response.json();
        console.log('Record deleted:', data);
        fetch_data(); // Refresh the data
    } catch (error) {
        console.error('Error deleting record:', error);
    }
}

	
		onMount(() => {
			check_login();
			fetch_data();
		});
	/////////////////////////////server/////////////////////////////
	/////////////////////////////server/////////////////////////////
	/////////////////////////////server/////////////////////////////
	</script>
	
	<style>
		.container {
			display: flex;
			flex-direction: column;
			align-items: center;
			padding: 20px;
		}
		.header {
			display: flex;
			justify-content: space-between;
			width: 100%;
		}
		.content {
			display: flex;
			width: 100%;
		}
		.left-column, .right-column {
			flex: 1;
			padding: 10px;
		}
		textarea {
			width: 100%;
			height: 50vh;
		}
	</style>
	
	<div class="container">
	
	
	<div class="header">
		<h1>{service_name}</h1>
		{#if user}
			<button on:click={sign_out}>Logout</button>
		{:else}
			<button on:click={google_login}>Login</button>
		{/if}
	</div>
	<div class="content">
		<div class="left-column server_side">
			<div class="console">
				{#if error_message}
				<button id="error_message" on:click={() => error_message = ''} on:keydown={(e) => { if (e.key === 'Enter' || e.key === ' ') error_message = ''; }} style="background: none; border: none; padding: 0; margin: 0; color: inherit; font: inherit; cursor: pointer;">
					{error_message}
				</button>
				{/if}
				<p>{login_result}</p>
				<p>{app4_title}</p>
				{#if user}
				<button on:click={create_record}>Create Record</button>
				{/if}
				<p>uid: {uid}</p>
			</div>
			<div class="list">
				<ul>
					{#each web_data as item}
						<li>
							{item.app4_title}
							<button on:click={() => app4_text = item.app4_text}>Load</button>
							<!-- ログアウト状態ではupdateとdeleteは非表示 -->
							{#if user}
							<button on:click={() => update_record(item.id)}>Update</button>
							<button on:click={() => delete_record(item.id)}>Delete</button>
							{/if}
						</li>
					{/each}
				</ul>
			</div>
		</div>
		
	
		<div class="right-column">
			{#if is_editing_app4_title}
				<input type="text" bind:value={app4_title} />
				<button on:click={service_update_app4_title}>Update</button>
			{:else}
				<button on:click={service_toggle_edit_app4_title}>Change Title</button>
			{/if}
			<!-- test_app4_textを代入するボタン -->
			<button on:click={() => app4_text = test_app4_text}>Test Text List</button>
			<label for="open_volume">Open Volume:</label>
			<input type="number" id="open_volume" bind:value={open_volume} min="1" max={options.length} />
			<textarea bind:value={app4_text} placeholder="Enter text list here"></textarea>
			<button on:click={service_exe}>実行</button>
		</div>
		

	</div>
	
	
	</div>