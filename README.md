# Step 1 – Nuxt App erstellen

Dies ist der erste Schritt im **peerplay**-Workshop.\
Du wirst eine neue Nuxt 3 App anlegen und lokal starten.

---

## 🧰 Voraussetzungen

- Node.js (empfohlen: v18 oder höher)
- npm (wird mit Node installiert)
- Terminalzugriff
- Git

---

## 🧱 Nuxt-Projekt anlegen

### 1. Projekt starten

```bash
npm create nuxt@latest
```

Beantworte die folgenden Fragen wie angegeben:

| Frage                                                  | Antwort                    |
| ------------------------------------------------------ | -------------------------- |
| Where would you like to create your project?           | `./nuxt-app`               |
| Which package manager would you like to use?           | `npm`                      |
| Initialize git repository?                             | `No`                      |
| Would you like to install any of the official modules? | Alle auswählen (Leertaste) |

### 2. Server starten

```bash
cd nuxt-app
npm run dev
```

Öffne anschließend [http://localhost:3000](http://localhost:3000) im Browser.

---

## ✅ Ergebnis

Du hast nun:

- eine Nuxt 3 App erstellt
- ein lokales Entwicklungs-Setup aufgesetzt

---

## ➞ Nächster Schritt

👉 Wechsle dazu in den Branch: `step-2`

```bash
git checkout step-2
```

---

# Step 2 - P2P Node erstellen

Im zweiten Schritt erstellen wir eine einfache Peer-to-Peer (P2P) Node mit libp2p, die TCP-Verbindungen aufbaut, sich verschlüsselt und Datenströme multiplexed. So legen wir die Grundlage für eine dezentrale Kommunikation zwischen Computern.

---

## 📚 Nützliche Links

- Typescript: [https://www.typescriptlang.org/download/](https://www.typescriptlang.org/download/)
- js-libp2p Guide: [https://docs.libp2p.io/guides/getting-started/javascript/#lets-play-ping-pong](https://docs.libp2p.io/guides/getting-started/javascript/#lets-play-ping-pong)

---

## 🧱 P2P-Projekt anlegen

### 1. NPM initialisieren

```bash
mkdir p2p-node
cd p2p-node
npm init -y
```

### 2. Typescript installieren

```bash
npm install typescript --save-dev
npx tsc --version
mkdir src
touch src/index.ts
```

p2p-node/package.json öffnen und folgendes Skript hinzufügen bzw. überschreiben:

```json
{
  "scripts": {
    "dev": "ts-node src/index.ts"
  }
}
```

Danach starten wir das Projekt:

```bash
npm run dev
```

### 3. libp2p installieren

```bash
npm install libp2p @libp2p/tcp @chainsafe/libp2p-noise @chainsafe/libp2p-yamux
```

### 4. Typescript-Code in src/index.ts einfügen

```ts
import { createLibp2p } from 'libp2p'
import { tcp } from '@libp2p/tcp'
import { noise } from '@chainsafe/libp2p-noise'
import { yamux } from '@chainsafe/libp2p-yamux'

const main = async () => {
  const node = await createLibp2p({
    addresses: {
      // add a listen address (localhost) to accept TCP connections on a random port
      listen: ['/ip4/127.0.0.1/tcp/0']
    },
    transports: [tcp()],
    connectionEncryption: [noise()],
    streamMuxers: [yamux()]
  })

  // start libp2p
  await node.start()
  console.log('libp2p has started')

  // print out listening addresses
  console.log('listening on addresses:')
  node.getMultiaddrs().forEach((addr) => {
    console.log(addr.toString())
  })

  // stop libp2p
  await node.stop()
  console.log('libp2p has stopped')
}

main().then().catch(console.error)
```
### 5. Node starten und testen

```bash
npm run dev
```

---

## ✅ Ergebnis

- Du hast eine einfache P2P Node mit libp2p erstellt.
- Die Node startet, baut TCP-Verbindungen auf, verschlüsselt die Kommunikation und multiplexed Datenströme.
- Die Node zeigt ihre Multiadressen an, an denen sie erreichbar ist.

---

## ➞ Nächster Schritt

👉 Wechsle dazu in den Branch: `step-3`

```bash
git checkout step-3
```

---
