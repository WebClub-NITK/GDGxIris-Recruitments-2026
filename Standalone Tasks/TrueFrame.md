## Task ID: TrueFrame

#### `Web3`, `Applied Cryptography`, `Full Stack Web Development`, `Privacy`

Mentors: [Sacheth Koushal](https://github.com/ksacheth) ([+91 9346324359](https://wa.me/919346324359))

Difficulty: `Medium-Hard`

### Description

Deepfake videos spread faster than anyone can fact-check them, and real footage now gets dismissed as "AI-generated". Detectors only guess whether a clip is fake. Build **TrueFrame**, a web app that instead **proves a clip is real from the moment it is recorded**.

The clip is recorded in the app, its fingerprint (hash) is signed by a key that never leaves the recording device, and the fingerprint is anchored on a **public blockchain testnet** (Ethereum, Polygon, Solana or any other chain you prefer) so its time cannot be changed later. The witness shares the clip with a verification link, and anyone can open the link, drop in their copy, and see whether it is the original or has been edited, with **no app, no wallet and no account**. This follows the same idea as the [C2PA](https://c2pa.org/) Content Credentials standard used by camera makers.

### Features to Implement

1. **Capture and Fingerprint:**

   * Record video (with audio) in the browser using `getUserMedia` and `MediaRecorder`.
   * Split the recorded file into fixed-size chunks (e.g., 256 KB), hash each with SHA-256, and build a **Merkle tree** over the chunk hashes. The Merkle root is the clip's fingerprint.

2. **Device Key and Signing:**

   * On first use, generate an ECDSA P-256 key pair with WebCrypto, marked **non-extractable**, and store it in IndexedDB. The private key must never be exported or sent to the server.
   * Sign a payload containing the Merkle root, file size, capture time, district, sealed location (see 4) and the device public key.

3. **Anchor on a Blockchain:**

   * Pick any chain and use its **testnet**, for example Ethereum (Sepolia), Polygon (Amoy) or Solana (devnet). Justify your choice (cost, confirmation time, tooling) in the README.
   * Send a transaction carrying `SHA-256(signed payload)`, with a prefix such as `trueframe:v1:<hex>`. On EVM chains this can be a small contract that emits an event (verify it on the block explorer); on Solana you can use the [SPL Memo program](https://spl.solana.com/memo).
   * The witness should not need a wallet. The backend pays the fee with a testnet key kept in an environment variable, never in the repo.
   * Store only the proof (payload, signature, transaction signature). **The video itself is never uploaded.**

4. **Protect the Witness:**

   * Publish only the **district** of the recording location.
   * Store the exact coordinates as a salted commitment, `SHA-256(lat | lon | salt)`, and give the salt only to the witness.
   * No login, name or phone number. The device public key is the only (pseudonymous) identity.

5. **Verify Page:**

   * A shareable link like `/verify/<proofId>` that shows the recording time, district, device key and a link to the transaction on the chain's block explorer.
   * The user drops in a video and the page hashes it **in the browser**, then checks the signature, reads the transaction directly from a public RPC and checks that the anchored hash matches the payload, and compares the Merkle root.
   * Show a clear result: **Original**, **Edited** (list which chunks differ), or **Invalid proof** (and which check failed).

### Bonus Features (Optional)

*Implementing any two of these features will make the task count as `Hard`*

1. **"Not Before" Time:** Include a recent block hash from your chain in the signed payload so the clip is proven to be made after that moment too, not just before the anchor.
2. **Merkle Batching:** Anchor many proofs in one transaction using a batch Merkle root, and give each proof its inclusion path.
3. **Survives Trimming:** Record in independent time segments and sign the list of segment hashes, so a trimmed excerpt still verifies as "genuine excerpt, 0:10 to 0:25".
4. **Survives WhatsApp:** Sign a perceptual hash of sampled frames, so a re-compressed copy is recognised as a copy of the original.
5. **Court-Ready Export:** Generate a PDF with file hash, device key, capture time and transaction link, in the shape of a Section 63 certificate under the Bharatiya Sakshya Adhiniyam, 2023.
6. **Hardware Key:** Build the capture side as an Android app with the key in Android Keystore / StrongBox and verify its Key Attestation.

### Tips

-   Start small: get a whole-file SHA-256 signed, anchored and verified end to end first, then add chunking and the Merkle tree.

-   Hash large files in pieces with `File.slice()`, ideally in a Web Worker, so a 200 MB video does not freeze the tab.

-   Sign a **canonical** encoding of the payload (fixed key order). `JSON.stringify` on an object with unordered keys will break verification in subtle ways.

-   Reviewers will try these and expect a clear, specific failure: flip one byte of the video, edit the stored payload without re-signing, re-sign with a different key, and point a proof at another proof's transaction. They will also watch the Network tab to confirm the video is never uploaded.

-   Add a short note on the verify page about what TrueFrame does **not** prove: it proves this device signed this exact file by this time, not that the scene was not staged.

-   Testnet faucets are rate limited. Fund your key once and reuse it.

### Useful Resources

-   **Cryptography in the Browser**

    -   [MDN: SubtleCrypto](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto)

    -   [MDN: MediaRecorder API](https://developer.mozilla.org/en-US/docs/Web/API/MediaRecorder)

    -   [MDN: Using Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)

    -   [Merkle Trees](https://brilliant.org/wiki/merkle-tree/)

-   **EVM-Based (Ethereum, Polygon, etc.)**

    -   [Solidity Documentation](https://docs.soliditylang.org/)

    -   [Hardhat](https://hardhat.org/)

    -   [Ethers.js](https://docs.ethers.org/)

    -   [Ethereum Sepolia Testnet](https://ethereum.org/en/developers/docs/networks/#sepolia)

    -   [Polygon Amoy Testnet](https://docs.polygon.technology/tools/wallets/metamask/add-polygon-network/)

-   **Solana**

    -   [Solana Docs](https://solana.com/docs)

    -   [@solana/web3.js](https://solana-foundation.github.io/solana-web3.js/)

    -   [SPL Memo Program](https://spl.solana.com/memo)

    -   [Solana Explorer (devnet)](https://explorer.solana.com/?cluster=devnet)

-   **Content Provenance**

    -   [C2PA Specification](https://c2pa.org/specifications/specifications/2.1/index.html)

    -   [Content Authenticity Initiative: Open Source Tools](https://opensource.contentauthenticity.org/)

-   **Android (Bonus)**

    -   [Android Keystore System](https://developer.android.com/privacy-and-security/keystore)

    -   [Key Attestation](https://developer.android.com/privacy-and-security/security-key-attestation)
