# Step 1 – Nuxt App erstellen

Dies ist der erste Schritt im **peerplay**-Workshop.\
Du wirst eine neue Nuxt 3 App anlegen und lokal starten.

## 🧰 Voraussetzungen

- Node.js (empfohlen: v18 oder höher)
- npm (wird mit Node installiert)
- Terminalzugriff
- Git

## 📚 Nützliche Links

- Nuxt: [https://nuxt.com/](https://nuxt.com/)

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

## ✅ Ergebnis

Du hast nun:

- eine Nuxt 3 App erstellt
- ein lokales Entwicklungs-Setup aufgesetzt

## ➞ Nächster Schritt

👉 Wechsle dazu in den Branch: `step-2`

```bash
git checkout step-2
```

---

# STEP 2 - P2P Node erstellen

Im zweiten Schritt erstellen wir eine einfache Peer-to-Peer (P2P) Node mit libp2p, die TCP-Verbindungen aufbaut, sich verschlüsselt und Datenströme multiplexed. So legen wir die Grundlage für eine dezentrale Kommunikation zwischen Computern.

## 📚 Nützliche Links

- Typescript: [https://www.typescriptlang.org/download/](https://www.typescriptlang.org/download/)
- js-libp2p Guide: [https://docs.libp2p.io/guides/getting-started/javascript/#lets-play-ping-pong](https://docs.libp2p.io/guides/getting-started/javascript/#lets-play-ping-pong)

## 🧱 P2P-Projekt anlegen

### 1. NPM initialisieren

```bash
mkdir p2p-node
cd p2p-node
npm init -y
```

### 2. Javascript

Javascript Datei anlegen: 

```bash
touch index.js
```
index.ts mit "Hello World" füllen:

```js
console.log("Hello World");
```

package.json Skript anpassen:

```json
{
  ...
  "scripts": {
    "test": "node index.js"
  }
  ...
}
```

"Hello World" Konsolen Ausgabe prüfen:

```bash
npm run test
```

### 3. node_modules ignorieren

Erstelle eine .gitignore-Datei:

```bash
touch .gitignore
```
Füge folgenden Eintrag hinzu:

```nginx
node_modules
```

### 4. libp2p installieren

```bash
npm install libp2p @libp2p/tcp @chainsafe/libp2p-noise @chainsafe/libp2p-yamux
```

### 5. index.js überschreiben

```js
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
    connectionEncrypters: [noise()],
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
### 6. Node starten und testen

```bash
npm run test
```

### 7. Fehlermeldung beheben

Die Warnung, die du bekommst:

```nginx
(node:52184) [MODULE_TYPELESS_PACKAGE_JSON] Warning: Module type of file:///workspaces/peerplay/p2p-node/index.js is not specified and it doesn't parse as CommonJS.
Reparsing as ES module because module syntax was detected. This incurs a performance overhead.
To eliminate this warning, add "type": "module" to /workspaces/peerplay/p2p-node/package.json.
(Use `node --trace-warnings ...` to show where the warning was created)
```

bedeutet:

- Deine index.js-Datei verwendet ESM-Syntax (z. B. import/export).
- Aber dein package.json gibt nicht an, dass dein Projekt ein ES-Modul ist.

Deswegen muss man in package.json folgendes hinzufügen:

```json
{
  ...
  "type": "module",
  ...
}
```

Teste es noch einmal, ob die Fehlermeldung weg ist:

```bash
npm run test
```

## ✅ Ergebnis

- Du hast eine einfache P2P Node mit libp2p erstellt.
- Die Node startet, baut TCP-Verbindungen auf, verschlüsselt die Kommunikation und multiplexed Datenströme.
- Die Node zeigt ihre Multiadressen an, an denen sie erreichbar ist.

## ➞ Nächster Schritt

👉 Wechsle dazu in den Branch: `step-3`

```bash
git checkout step-3
```

---

# STEP 3 - P2P-Beispiel-Chat mit libp2p

Im dritten Schritt schauen wir uns ein Beispiel-Chatprojekt an, das auf libp2p basiert.

## 📚 Nützliche Links

- Projekt auf HitHub: [https://github.com/libp2p/js-libp2p-example-chat](https://github.com/libp2p/js-libp2p-example-chat)

## 🧱 Beispielprojekt klonen

### 1. Repository klonen

Im Terminal im peerplay/-Verzeichnis folgenden Befehl ausführen:

```bash
git clone https://github.com/libp2p/js-libp2p-example-chat.git
```

### 2. Abhängigkeiten installieren

Danach ins Projektverzeichnis wechseln und die benötigten Pakete installieren:

```bash
cd js-libp2p-example-chat/
npm install
```

## 🚀 Projekt starten

### 3. Listener starten

Im aktuellen Terminal-Fenster den Listener starten:

```bash
cd src
node listener.js
```

### 4. Dialer starten

Ein neues Terminal öffnen und ebenfalls ins Projektverzeichnis wechseln, dann den Dialer starten:

```bash
cd js-libp2p-example-chat/src
node dialer.js
```

## 💬 Chat testen

Nun kann in den beiden Terminals (Listener und Dialer) eine Unterhaltung geführt werden.
Einfach Nachrichten eingeben – sie werden über das P2P-Netzwerk direkt übertragen.

## ✅ Ergebnis

- Du hast ein Beispielprojekt für einen P2P-Chat mit libp2p eingerichtet.
- Der Listener und der Dialer starten jeweils als eigene Nodes und verbinden sich direkt miteinander.
- Du kannst über das Terminal zwischen den beiden Instanzen chatten.

## ➞ Nächster Schritt

👉 Wechsle dazu in den Branch: `step-4`

```bash
git checkout step-4
```

---