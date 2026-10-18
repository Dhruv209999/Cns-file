
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Secure Messenger | AES + RSA</title>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

:root {
    --bg: #050d20;
    --panel: #0b1932;
    --border: #174b83;
    --blue: #2584ff;
    --cyan: #21e6f3;
    --green: #00d6a0;
    --purple: #a855f7;
    --text: #f1f6ff;
    --muted: #a7bddf;
}

body {
    font-family: Arial, sans-serif;
    background: radial-gradient(circle at top, #11254a, var(--bg) 65%);
    color: var(--text);
    min-height: 100vh;
    line-height: 1.6;
}

header {
    padding: 22px 6%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
    border-bottom: 1px solid var(--border);
    background: rgba(5, 13, 32, 0.9);
}

.logo {
    display: flex;
    align-items: center;
    gap: 14px;
}

.logo-icon {
    font-size: 42px;
    color: var(--cyan);
}

.logo h1 {
    font-size: 27px;
    line-height: 1.3;
}

.logo h1 span {
    color: var(--cyan);
}

.logo p, .header-text {
    color: var(--muted);
    font-size: 13px;
}

nav {
    display: flex;
    gap: 18px;
    flex-wrap: wrap;
}

nav a {
    text-decoration: none;
    color: var(--muted);
    font-size: 14px;
}

nav a:hover {
    color: var(--cyan);
}

.container {
    width: 90%;
    max-width: 1450px;
    margin: 32px auto;
}

.hero {
    text-align: center;
    margin-bottom: 30px;
}

.hero h2 {
    font-size: clamp(25px, 4vw, 38px);
    margin-bottom: 8px;
}

.hero h2 span {
    color: var(--cyan);
}

.hero p {
    color: var(--muted);
}

.cards {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 24px;
}

.card {
    background: linear-gradient(145deg, #102344, #08152c);
    border: 1px solid var(--border);
    border-radius: 17px;
    padding: 25px;
    box-shadow: 0 12px 35px rgba(0, 0, 0, 0.2);
}

.card-header {
    display: flex;
    align-items: center;
    gap: 15px;
    margin-bottom: 22px;
}

.icon {
    min-width: 54px;
    height: 54px;
    display: grid;
    place-items: center;
    border-radius: 15px;
    background: rgba(37, 132, 255, 0.15);
    color: var(--blue);
    font-size: 27px;
}

.receive .icon {
    background: rgba(0, 214, 160, 0.13);
    color: var(--green);
}

.card-header h3 {
    font-size: 21px;
}

.card-header p {
    color: var(--muted);
    font-size: 13px;
    margin-top: 5px;
}

label {
    display: block;
    margin: 15px 0 7px;
    font-weight: bold;
    font-size: 14px;
}

textarea {
    display: block;
    width: 100%;
    min-height: 90px;
    padding: 14px;
    resize: vertical;
    background: #061126;
    border: 1px solid #245a99;
    border-radius: 9px;
    outline: none;
    color: white;
    font: 14px Arial, sans-serif;
}

textarea:focus {
    border-color: var(--cyan);
}

textarea::placeholder {
    color: #829cc3;
}

.buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    margin: 20px 0;
}

.btn {
    display: inline-block;
    padding: 12px 20px;
    border-radius: 8px;
    text-align: center;
    font-size: 14px;
    font-weight: bold;
    text-decoration: none;
    border: 1px solid transparent;
    cursor: pointer;
}

.btn-blue {
    color: white;
    background: linear-gradient(100deg, #278bff, #4262ff);
}

.btn-green {
    color: #031b20;
    background: linear-gradient(100deg, #00dba6, #00bd91);
}

.btn-outline {
    color: white;
    background: #172a49;
    border-color: #42618e;
}

.btn:hover {
    filter: brightness(1.15);
}

.output {
    padding: 15px;
    margin-top: 12px;
    background: #061126;
    border: 1px solid #1a477b;
    border-radius: 10px;
}

.output h4 {
    font-size: 14px;
    margin-bottom: 8px;
}

.output p {
    color: var(--muted);
    font-size: 13px;
    overflow-wrap: anywhere;
}

.output-aes {
    border-left: 3px solid var(--cyan);
}

.output-rsa {
    border-left: 3px solid var(--purple);
}

.receive {
    border-color: #087b77;
}

.decrypted {
    border-color: #008d75;
    background: rgba(0, 214, 160, 0.06);
}

.workflow {
    margin-top: 28px;
    padding: 25px;
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 15px;
}

.workflow h3 {
    font-size: 23px;
    margin-bottom: 22px;
}

.steps {
    display: grid;
    grid-template-columns: repeat(5, minmax(0, 1fr));
    gap: 14px;
}

.step {
    padding: 17px 13px;
    border: 1px solid var(--border);
    border-radius: 11px;
    background: #09152b;
}

.number {
    display: inline-grid;
    place-items: center;
    width: 34px;
    height: 34px;
    border-radius: 50%;
    background: var(--blue);
    font-weight: bold;
    margin-bottom: 10px;
}

.step:nth-child(2) .number {
    background: #00bd91;
    color: #04231c;
}

.step:nth-child(3) .number {
    background: var(--purple);
}

.step:nth-child(4) .number {
    background: #ffab28;
    color: #231500;
}

.step:nth-child(5) .number {
    background: var(--cyan);
    color: #00272b;
}

.step h4 {
    font-size: 14px;
    margin-bottom: 7px;
}

.step p {
    font-size: 12px;
    color: var(--muted);
}

.notice {
    margin-top: 20px;
    padding: 15px;
    border-radius: 9px;
    border: 1px solid #73551d;
    background: rgba(255, 171, 40, 0.07);
    color: #ffd98b;
    font-size: 13px;
}

footer {
    text-align: center;
    padding: 25px 15px;
    color: var(--muted);
    font-size: 13px;
}

footer span {
    color: var(--cyan);
}

@media (max-width: 900px) {
    header {
        flex-direction: column;
        text-align: center;
    }

    .cards {
        grid-template-columns: 1fr;
    }

    .steps {
        grid-template-columns: repeat(2, minmax(0, 1fr));
    }
}

@media (max-width: 480px) {
    header {
        padding: 18px 5%;
    }

    .logo h1 {
        font-size: 22px;
    }

    nav {
        justify-content: center;
        gap: 12px;
    }

    .container {
        width: 94%;
        margin: 22px auto;
    }

    .card, .workflow {
        padding: 16px;
    }

    .card-header h3 {
        font-size: 18px;
    }

    .steps {
        grid-template-columns: 1fr;
    }

    .buttons {
        flex-direction: column;
    }

    .btn {
        width: 100%;
    }
}
</style>
</head>

<body>

<header>
    <div class="logo">
        <div class="logo-icon">🛡</div>
        <div>
            <h1>Secure <span>Messenger</span></h1>
            <p>AES + RSA Encrypted Messaging</p>
        </div>
    </div>

    <nav>
        <a href="#home">Home</a>
        <a href="#send">Send Message</a>
        <a href="#receive">Receive Message</a>
        <a href="#about">About</a>
    </nav>
</header>

<main class="container" id="home">

    <section class="hero">
        <h2>Private Messages. <span>Powerful Security.</span></h2>
        <p>A visual demonstration of AES symmetric and RSA asymmetric encryption.</p>
    </section>

    <section class="cards">

        <article class="card" id="send">
            <div class="card-header">
                <div class="icon">➤</div>
                <div>
                    <h3>1. Send Encrypted Message</h3>
                    <p>Encrypt a message with AES and protect its key with RSA.</p>
                </div>
            </div>

            <label for="message">Your Message</label>
            <textarea id="message" placeholder="Enter your secret message..."></textarea>

            <label for="public-key">Recipient's RSA Public Key</label>
            <textarea id="public-key" placeholder="Paste recipient's public key here..."></textarea>

            <div class="buttons">
                <a href="#send" class="btn btn-blue">🔒 Encrypt Message</a>
                <a href="#send" class="btn btn-outline">↻ Clear</a>
            </div>

            <h4>Encrypted Output</h4>

            <div class="output output-aes">
                <h4>Encrypted Message (AES)</h4>
                <p>Your AES-encrypted message will appear here.</p>
            </div>

            <div class="output output-rsa">
                <h4>Encrypted AES Key (RSA)</h4>
                <p>Your RSA-encrypted AES key will appear here.</p>
            </div>
        </article>

        <article class="card receive" id="receive">
            <div class="card-header">
                <div class="icon">🔐</div>
                <div>
                    <h3>2. Receive & Decrypt</h3>
                    <p>Recover the AES key and decrypt the received message.</p>
                </div>
            </div>

            <label for="private-key">Your RSA Private Key</label>
            <textarea id="private-key" placeholder="Paste your private key here..."></textarea>

            <label for="encrypted-key">Encrypted AES Key (RSA)</label>
            <textarea id="encrypted-key" placeholder="Paste encrypted AES key here..."></textarea>

            <label for="encrypted-message">Encrypted Message (AES)</label>
            <textarea id="encrypted-message" placeholder="Paste encrypted message here..."></textarea>

            <div class="buttons">
                <a href="#receive" class="btn btn-green">🔓 Decrypt Message</a>
                <a href="#receive" class="btn btn-outline">↻ Clear</a>
            </div>

            <div class="output decrypted">
                <h4>Decrypted Message</h4>
                <p>Your original decrypted message will appear here.</p>
            </div>
        </article>

    </section>

    <section class="workflow" id="about">
        <h3>⚙ How It Works</h3>

        <div class="steps">
            <div class="step">
                <div class="number">1</div>
                <h4>Original Message</h4>
                <p>The sender writes a private message.</p>
            </div>

            <div class="step">
                <div class="number">2</div>
                <h4>AES Encryption</h4>
                <p>A random AES key encrypts the message.</p>
            </div>

            <div class="step">
                <div class="number">3</div>
                <h4>RSA Encryption</h4>
                <p>The recipient's public key protects the AES key.</p>
            </div>

            <div class="step">
                <div class="number">4</div>
                <h4>Secure Transfer</h4>
                <p>The encrypted message and key are transmitted.</p>
            </div>

            <div class="step">
                <div class="number">5</div>
                <h4>Decryption</h4>
                <p>The private key recovers the AES key, which decrypts the message.</p>
            </div>
        </div>

        <div class="notice">
            <strong>Educational Demo:</strong> This HTML/CSS-only page
            demonstrates the interface. Encryption, decryption, key generation,
            and button actions are not implemented.
        </div>
    </section>

</main>

<footer>
    🛡 Secure Messenger | <span>AES + RSA</span> Encryption
    <p>Designed for learning cryptography and web development.</p>
</footer>

</body>
</html>
