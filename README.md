## 🧾 WhatsApp Bot Using GUI (with `whatsapp-web.js`)

This guide covers setting up a WhatsApp bot **with a visible browser**, using `whatsapp-web.js`. You'll be able to scan a QR code in your browser and interact with WhatsApp messages through Node.js.

---

### ✅ Requirements

- A machine running Linux (or Windows/macOS)
- Graphical user interface (GUI) available (e.g., desktop environment, VNC, etc.)
- WhatsApp account on your mobile phone
- Node.js & npm installed

---

### 1️⃣ Install Node.js and npm

On Linux (Debian/Ubuntu-based):

```bash
sudo apt update
sudo apt install nodejs npm -y
```

Verify versions:

```bash
node -v
npm -v
```

---

### 2️⃣ Set Up Your Project

```bash
mkdir whatsapp-bot
cd whatsapp-bot
npm init -y
npm install whatsapp-web.js qrcode-terminal
```

---

### 3️⃣ Create Your Bot File

Create a new file:

```bash
nano bot.js
```

Paste this code:

```js
const { Client, LocalAuth } = require('whatsapp-web.js');
const qrcode = require('qrcode-terminal');

const client = new Client({
    authStrategy: new LocalAuth()
});

client.on('qr', (qr) => {
    console.log('Scan this QR code with WhatsApp:');
    qrcode.generate(qr, { small: true });
});

client.on('ready', () => {
    console.log('✅ Bot is connected and ready!');
});

client.on('message', message => {
    const msg = message.body.toLowerCase();

    if (msg === 'hi' || msg === 'hello') {
        return message.reply('Hello! 🤖 I am your WhatsApp bot. Type "menu" to see commands.');
    }

    if (msg === 'menu') {
        return message.reply(
            `📋 Menu:\n` +
            `1️⃣ price – check product prices\n` +
            `2️⃣ order – place an order\n` +
            `3️⃣ support – contact support`
        );
    }

    if (msg.includes('price')) {
        return message.reply('Prices vary by item. Please specify what you’re looking for. 💰');
    }

    if (msg.includes('order')) {
        return message.reply('Please send the name or code of the item you want to order. 📝');
    }

    if (msg === 'support') {
        return message.reply('Our support team will contact you shortly. 🧑‍💼');
    }

    if (msg.includes('how are you')) {
        const replies = ['I’m great! 😄', 'All systems go!', 'Doing awesome!'];
        const random = replies[Math.floor(Math.random() * replies.length)];
        return message.reply(random);
    }

    if (msg.length < 20) {
        return message.reply('I didn’t understand that. Type "menu" to see available options.');
    }
});
```

---

### 4️⃣ Run the Bot

```bash
node bot.js
```

- A **QR code will appear** in the terminal.
- On your phone, open WhatsApp → Menu → Linked Devices → Scan the QR code.

---

### ✅ Done!

Your bot is now live. Try sending:
- `hi`
- `menu`
- `order`
- `how are you`

And the bot will respond accordingly.

---

### 🧠 Tips for Next Steps

- Use `LocalAuth` to keep your session alive
- Host the bot on a cloud server (like DigitalOcean or VPS)
- Add a database or webhook integrations
- Build a step-by-step form for orders
