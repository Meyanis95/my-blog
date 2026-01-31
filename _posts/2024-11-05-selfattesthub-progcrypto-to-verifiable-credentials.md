---
layout: post
title: "SelfAttestHub: A Port From ProgCrypto to Verifiable Credentials"
date: 2024-11-05
---

The ZkID team has recently intensified its research on **Self-Sovereign Identity (SSI)**, driven by the growing interest from governments in adopting more secure, digital methods for managing identities.

---

## Introduction to Self-Sovereign Identity (SSI)

At its core, **Self-Sovereign Identity (SSI)** empowers individuals to have complete control over their personal data. This control is exercised through **Identity Wallets**, where users can store and manage **Verifiable Credentials (VCs)**. These credentials represent pieces of identity data, authenticated by signatures and secured using **Public Key Infrastructure (PKI)**.

The key feature of SSI is that users, decide **what information to share, with whom, and when**.

![image](https://hackmd.io/_uploads/SyKnR7nyye.png)
*"The shift in control that happens in the transition from centralized or federated identity models to the self-sovereign model. Only the latter puts the individual at the center."*

This shift could empower users with:
- Passwordless registration and logins at any SSI-enabled website or service.
- Automatically maintain a private personal log of digital receipts—and provide proof of their purchases to any merchant or recommendation engine of their choice.
- Digital information sharing signed by trusted issuers, necessary to apply for a loan or mortgage in seconds.

The leading way to create this new realm is mainly defined by the creation of new standards, and they are 2 that are widely developed on and supported:
- [W3C Standard for Verifiable Credentials](https://www.w3.org/TR/vc-data-model-2.0/)
- [OpenID for Verifiable Presentations](https://openid.net/specs/openid-4-verifiable-presentations-1_0-14.html)

The principle of this system is simple, it's a 3 player game, with:
- **User**: the credential owner.
- **Issuer or Identity Provider**: they are the authorities responsible for attributing and signing credentials. This can be the government, your bank or your telecom provider.
- **Relying Party or Verifier**: the agent that actually wants you to authenticate or to present some credentials.

The issuer is responsible to deliver the credential to the user. Here is a basic example what it looks like;

In the most basic form, the Identity data is represented as an object, that is then hashed and signed with a private key, issuing a signature that can be verified with a public key.

```js
{
 first_name: "John",
 last_name: "Doe",
 birthdate: "01-01-1900",
 address: "Springfield"
}
```

![image](https://hackmd.io/_uploads/SkK6Z431ke.png)

### A Day in the Life of an SSI User

Imagine it's Monday morning. As you sip your coffee, a notification pops up on your phone: your government-issued digital ID, a Verifiable Credential (VC), is now ready. With a few taps, you store it in your digital wallet.

Later that day, you head to a new bank to open an account. No need for paperwork—the bank's app asks for ID verification, and you simply select your digital ID from your wallet. With one tap, the VC is sent, and the bank verifies your ID's authenticity by checking the government's signature and confirming it's really you using your public identifier. In seconds, your account is open.

After work, you stop by a local gym offering discounts for city residents. Thanks to a resident VC issued by the city hall and stored in your wallet, you're eligible for the discount with no paperwork or forms to fill. When the gym requests proof of residency, you select your resident VC and share only the city information. The gym instantly verifies your residency and applies the discount without needing any additional details.

By evening, you've effortlessly proven your identity, opened a bank account, and joined a gym at a resident discount—all while keeping control of your data. This is the power of Self-Sovereign Identity: convenience, privacy, and security, all in a single day.

### Verifiable Credentials (VCs): Standards and Specifications

This section will illustrate the [VC standard](https://www.w3.org/TR/vc-data-model-2.0/) and dive a bit deeper in the technical specifications.

```js
{
 "@context": [
     "https://www.w3.org/ns/credentials/v2",
     "https://www.w3.org/ns/credentials/examples/v2"
     ],
    "id": "urn:uuid:58172aac-d8ba-11ed-83dd-0b3aef56cc33",
    "type": [ "VerifiableCredential", "IdentityCredential" ],
    "name": "Identity Credential",
    "description": "An minimum viable example of an Identity Credential.",
    "issuer": "https://vc.example/issuers/5678",
    "validFrom": "2023-01-01T00:00:00Z",
    "credentialSubject": {
        "id": "did:example:abcdefgh",
        "first_name": "John",
        "last_name": "Doe",
        "birthdate": "01-01-1900",
        "address": "Springfield"
    },
    "proof": {
        "type": "DataIntegrityProof",
        "cryptosuite": "eddsa-2022",
        "created": "2023-02-24T23:36:38Z",
        "verificationMethod": "https://vc.example/issuers/5678#z6MkrJVnaZkeFzdQyMZu1cgjg7k1pZZ6pvBQ7XJPt4swbTQ2",
        "proofPurpose": "assertionMethod",
        "proofValue": "zytcLynZ8bVqCNnfw9zrGQonkmMgwRVsNiQs5WFUnUxpQsUzjT1tG27Tz9A7x8M4rGgA3nzqdQi3xjvAVhet1vFP"
 }
}
```

A VC is defined by the following components:
- **@context**: The context helps interpreters understand the structure and meaning of the data by linking to external vocabularies, for example in that case it can define the types used in the `credentialSubject`.
- **id**: The unique identifier of the credential.
- **type**: A list of types that describe the nature of the credential.
- **description**: A human-readable description of the credential.
- **issuer**: This represents the entity (typically a DID, *we're defining DID bellow*) that issued the credential.
- **validFrom**: The date and time from which the credential is valid. This ensures that credentials can be time-bound, preventing them from being valid indefinitely.
- **credentialSubject**: This section represents the subject of the credential—the entity about whom the claims are being made. The subject is often identified using a Decentralized Identifier (DID) and may include additional claims, such as "birthdate" in this example.
- **proof**: The cryptographic proof used to verify the credential. The proof object includes information such as the type of proof (e.g., digital signatures), the cryptographic suite used, the verification method (which is often a DID URL), the proof purpose, and the actual proof value (a cryptographic signature or similar).

The key advantages of using a standardized format for Verifiable Credentials (VCs) include:

- **Interoperability**: Any verifier, regardless of their platform or implementation, can interpret and understand the structure and claims of a VC using the context, allowing seamless interaction across systems.
- **Universal Trust**: Credentials issued by different entities can be verified and trusted across various services, fostering a more open and compatible identity ecosystem.
- **Security**: Standardized cryptographic proofs ensure that the data contained in a VC is tamper-proof and authentic, protecting both the issuer and the holder.
- **Versatility**: The standardized structure enables VCs to be used in a wide range of applications, from identity verification to access control, streamlining credential management in various scenarios.

By using this standard, verifiers can confidently and efficiently validate credentials, while issuers and holders benefit from enhanced security, privacy, and flexibility in a decentralized ecosystem.

### Decentralized Identifiers (DIDs): A Primer

[A **Decentralized Identifier (DID)**](https://www.w3.org/TR/did-core/) is a globally unique, self-sovereign identifier that can be resolved across multiple storage systems, especially decentralized ones like IPFS or blockchains. DIDs function similarly to DNS, enabling trust without relying on a single centralized authority (e.g. an URI pointing to a server), supporting decentralized and user-controlled identity systems (e.g. IPFS, Blockchain).


#### Technical Structure:

A DID follows the format:
```
did:<method>:<method-specific-identifier>
```
- **Method**: Defines how the DID is created and resolved (e.g., `did:ethr`, `did:web`), and each method can have specific resolving method.
- **Method-specific-identifier**: Unique to the method, representing the DID subject.

So for example we can use a DID to resolve a ethereum public key, by having a method looks like the following:

`did:ethr:0x1A2b3C4d5E6F7G8h9I0JkLmNOpQrStUvWxYz1234`

#### DID Document:

DIDs resolve to **DID Documents**, which contain:
- **Public keys** or other cryptographic verification methods for proving control over the DID.
- **Service endpoints** to enable interactions like messaging, authentication, or credential exchange.

DIDs support cryptographic proof-based interactions, providing security and privacy for decentralized applications.

#### Use Cases of DIDs

- **Self-Sovereign Identity**: Individuals can use DIDs to prove who they are without relying on centralized identity providers (like governments or companies).
- **Verifiable Credentials**: DIDs can serve as the identifiers for issuers, holders, and verifiers of credentials, ensuring that the claims are cryptographically secure.
- **Decentralized Applications**: DIDs can be used to identify users, devices, or organizations in decentralized applications (dApps), enabling trustless interactions.

So if we take again our example of proving my age is above 18, instead of sharing the copy of an identity document (e.g. an ID plastic card or passport), I can now ask a VC to my local government, store it in my wallet and any time I have to prove my age I can receive a request from the verifier and present my VC containing my birthdate. I don't have to ask the permission to my local government, I'm free to authenticate and use my credential, without having the government knows to whom I'm presenting it. To authenticate that I'm the legitimate owner of the credential, the Identity wallet can sign the credential itself and be identified through the public key of the user using DIDs.

### The Role of Identity Wallets in SSI

Identity wallets have pretty much the same specs as a crypto wallet, with difference that it's not made to store financial assets. Its main goal is to securely store a private key, with signing capabilities and it can be identified with a public key.

Here are the main features of an Identity wallet:
- **Storage**: Store your credentials, keys/keycards, bills/receipts, etc.
- **Security**: Ensure the security of the above assets.
- **UX**: Keep them handy—easily available and portable across all your devices.

To prevent agents to link your differents credentials or usage, the wallet can store multiple identifiers, that can be resolved by DIDs (like in the `did:ethr` example, resolving an ETH address)

### Protecting Privacy in SSI: Selective Disclosure and Zero-Knowledge Proofs

Great! Now I can authenticate anywhere that requires a proof of identity...but at every authentication I will have to disclose the full signed data object, making me less sovereign about what I want the data I'm sharing with the verifier.

That's where **Zero-Knowledge (ZK)** technology, which enables **proving without revealing** and **Selective Disclosure (SD)**, enters in action. SD allows users to disclose only specific attributes of their credentials—such as proving their age without revealing their exact date of birth—further protecting their privacy.

## State of Zero-Knowledge (ZK) in SSI

In the SSI ecosystem, **Zero-Knowledge (ZK)** technology is primarily used at the **Issuer** level, meaning that to achieve **Selective Disclosure (SD)** and enhanced privacy, the issuer must integrate ZK tools. Below is an overview of how ZK is applied:

- **CL Signatures**: A two-party computation (2PC) signature algorithm that allows the generation of a proof of a signature without revealing the content of the signed data. This is supported by [Hyperledger Anoncred](https://github.com/hyperledger/anoncreds-clsignatures-rs).

- [**BBS+ Signatures**](https://identity.foundation/bbs-signature/draft-irtf-cfrg-bbs-signatures.html): A secure multi-message signature scheme that enables proving knowledge of a signature while selectively disclosing any subset of the signed messages. This protocol is currently under standardization and is supported by [Mattr.global](https://github.com/mattrglobal/bbs-signatures).

- **Merkle Tree Proofs**: Supported by [Iden3](https://docs.iden3.io/), this approach allows issuers to construct a claim tree and generate a ZK proof that reveals only a specific leaf, while proving the root of the entire claim tree.

![image](https://hackmd.io/_uploads/Sy6pD4Se1g.png)
*This table is showing the differences between VC format, and SD enabled by ZKPs, [click here for more details](https://decentralized-id.com/web-standards/w3c/verifiable-credentials/data-integrity-bbs+/).*


So now if we take our above example again and let's say we are an authority using the BBS+ signature algorithm, at the issuance of the credential we're not going to sign the hash of the whole Identity data, rather we are going to sign every field of it. So now each attribute of my identity are signed and I can share only a proof of the signature of my birthdate, and even more I can issue a proof that my birthdate is signed by the government without revealing the signed value.

```
...
**proof**:
    {
     "pk": "b65b7...",
    "header": "11223344556677889900aabbccddeeff",
    "ph": "",
    "disclosedIndexes": [ 3 ],
    "disclosedMsgs": [
        "birthdate": "01-01-1900",
        "proof": "a6089a46f59e00..."
        ]
    }
...
```
*In the above example, we are only disclosing the birthdate, with the proof being the BBS+ signature of this field, note that I don't need to request another signature to the Issuer.*

This demonstration shows that with ZK tooling **adopted by the Issuer in SSI**, we achieve enhanced privacy through selective disclosure and greater control over the data we share. However, it's important to note that the user cannot independently generate such a proof—the choice of a ZK-enabled signature algorithm must be made by the Issuer (e.g., governments).

### The SSI Privacy Challenge in Practice

While the vision is promising, there's a major bottleneck: authorities need to adopt these solutions. This is the lifeblood of the ecosystem. In order to achieve it, you need:
- Authorities to adopt and integrate such a tooling, and even if they want to integrate it, it can take significant time--sometimes stretching over a decade
- For true privacy, authorities must take the option, especially adopting ZK, and while there is some efforts to standardise ZK (see [zkproof.org](https://zkproof.org/)), it's far from having a NIST stamp that will make governments more confident in using it

On a more positive note, we're witnessing more and more interest from governments to adopt this solution to protect the privacy of the citizens, the biggest initiative being discussed in europe with their [Digital Identity Wallet](https://ec.europa.eu/digital-building-blocks/sites/display/EUDIGITALIDENTITYWALLET/EU+Digital+Identity+Wallet+Home) project led by the European Commission. While this is promising, it's not going to be deployed tomorrow, they are targeting to have it for 2030. Also this will only concern European citizens.

## Overcoming Adoption Barriers for Privacy-Focused SSI

While the broader adoption of Self-Sovereign Identity (SSI) solutions by authorities is a long-term challenge, there are already existing Digital Identity programs that involves digitally signed identity documents. The most prominent examples are passports. These documents contain digital signatures from governments, typically embedded in QR codes or NFC chips.

These digital signatures allow anyone with the appropriate tools to verify the authenticity of the document. By querying the signed data, signature, and fetching the government public key (often published on official government websites), the verifier can then confirm that the document has not been tampered with, and access personal data.

![image](https://hackmd.io/_uploads/HJ7kORCk1g.png)

Here's a sample list of digital identity programs where you can find digitally signed identity documents:

| **Program**                | **Format**        | **Region/Country**  | **Verification Method**    |
|----------------------------|-------------------|---------------------|----------------------------|
| **Passports, ICAO TD3**         | NFC               | Worldwide            | Digital signature in NFC chip |
| **EU ID cards, ICAO TD1**       | NFC               | Europe               | Digital signature in NFC chip |
| **Aadhaar**                     | PDF, QR Code      | India               | Digital signature in QR code |
| **Argentina ID-1**              | NFC               | Argentina            | Digital signature in NFC chip |
| **Firma Digital**               | Physical card     | Costa Rica          | Digital signature in physical card |
| **Singpass**                | NFC, QR Code      | Singapore            | QR code or NFC verification via mobile app|
| **Carteira de Identidade Nacional (CIN)** | Blockchain, Digital ID | Brazil | Digital signature, blockchain-based verification |
| **My Number Card**       | NFC               | Japan                | Digital signature in NFC chip|

But we still have the same problem as before, the verifier have access to the full identity. **So how could we only give a proof that we have an official identity document that has not been tempered and potentially selectively disclose information from it?**

That's where we introduce **general-purpose zero-knowledge proofs**. They are programs that let us turn any computation into a set of arithmetic constraints, proving the right computation of a program. Without going into too much details, the main purpose of it is that we can set some parameters of our program secret, withtout revealing it to the verifier.

![image](https://hackmd.io/_uploads/SJ9sy11gyx.png)

With the use of this program we can now turn our identity document verification function into a secret function that is still verifying the signature is genuine but does not reveal neither the signature nor the signed message containing the full clear identity.

![image](https://hackmd.io/_uploads/HkOQlJkgJe.png)


### Anon Aadhaar: An Example Protocol for ZKP-Based Proof of Identity

Aadhaar is the largest identity program globally, covering over **1.4 billion** individuals. While Aadhaar is widely regarded as a proof of identity, it's technically a residency card. Holders of Aadhaar can generate two types of documents:

- **eAadhaar PDF**: A digitally signed document.
- **Aadhaar Secure QR Code**: Available through the mAadhaar app.

For our protocol, we use the **QR code** due to its smaller size and easier data parsing. Scanning the QR code returns a compressed string that, when decompressed, contains the signed data and its corresponding digital signature. The data includes:

- **referenceId**: The last 4 digits of the Aadhaar number and a timestamp in the format "DDMMYYYYHHMMSSsss" (milliseconds included).
- **Name**
- **Date of Birth**
- **Gender**
- **Address**
- **Resident's Photo**

To verify the signature, we also need the public key, which is published by UIDAI on their [website](https://www.uidai.gov.in/en/916-developer-section/data-and-downloads-section/11349-uidai-certificate-details.html).

![UIDAI Certificate Details](https://hackmd.io/_uploads/SyrOKeJxyl.png)

Now, let's walk through the ZKP generation process from the signed data.

#### 1. Signature Verification

The core goal is to generate a ZKP that proves possession of a properly signed document, revealing only the fact that a trusted public key signed it. In Aadhaar's case, the data is signed using **RSA-SHA256**. Since we need the data in plaintext to enable **selective disclosure**, we must perform both the **hashing of the data** and the **signature verification** on this hash.

#### 2. Data Extraction

Once the data is authenticated, we extract the identity fields of interest. Aadhaar already has predefined specifications that indicate the exact indices where relevant fields, such as name, date of birth, and gender can be found within the data.

#### 3. Selective Disclosure

With the extracted data, we can selectively disclose the following attributes:
- **Age > 18**
- **Gender**
- **State**
- **Pincode**

Importantly, the prover has the option not to reveal any data if selective disclosure is not needed.
Note that we intentionally limit the data that can be revealed, as unrestricted disclosure could allow a malicious agent to expose the user's full identity. Since the protocol is primarily designed for **on-chain identity verification**, this prevents the complete identity from being unintentionally disclosed on-chain, ensuring both privacy and security in decentralized environments.

#### 4. Nullifier Generation

One of the biggest challenges in designing such protocols is generating a **nullifier**, which prevents double-spending of the proof. The nullifier must be deterministic yet not reverse-engineerable by the issuer, and random enough to avoid collisions and dictionary attacks.

For Anon Aadhaar, we rely on the **photo data** as the input to create a unique identifier (nullifier). The photo provides enough entropy to reduce the risk of collisions (e.g., two people with identical data, will have the same nullifier). Additionally, we introduce a **seed** or **scope** to create application/action specific nullifiers. This seed prevents cross-application tracking, meaning that a user's nullifier will differ between two applications, provided they don't share the same `nullifierSeed`.

A good nullifier must adhere to these principles:
- **Uniqueness**: The inputs should be unique to the user and contain enough entropy to prevent collisions.
- **App/Action-specific randomness**: The nullifier should include a salt or seed that can be tied to a specific application or action, preventing cross-app tracking.
- **No easily discoverable personal data**: Inputs like name, birthdate, or ZIP code should not be used directly, as they could allow for dictionary attacks. For instance, a nullifier generated as `hash(name | birthdate | zipcode | nullifierSeed)` would be vulnerable if the seed were public, as anyone with access to personal details could compute the nullifier.

##### Caveats:

The nullifier is derived from the photo bytes stored on the card, but users have the ability to request a photo update. Currently, we lack a mechanism to verify when a photo has been changed, which limits the nullifier's validity period. Since the photo change process typically takes around 8 days, the nullifier can only be considered unique for this duration. Additionally, because the government has access to the photo bytes, there is a potential risk of brute force attacks, which could compromise the anonymity of the user by linking the nullifier back to their identity.

#### 5. Timestamp Validation

Aadhaar's signed data includes a timestamp, which proves useful for ensuring the proof's freshness. For example, the verifier can set rules to accept only proofs generated within the last 3 hours. This ensures that the data was signed recently, providing additional assurance that the user is still in control of the account where the data was generated.

This approach is highly specific to Aadhaar, and unfortunately, it's not easily applicable to most other documents, which are typically signed only once for a period of 10 to 15 years. However, alternative methods could be developed to achieve the same goal of safeguarding the protocol against document theft.

#### 6. Proof Construction

The final proof consists of two key components:
- **The ZK proof itself**: In this case, a **Groth16 proof**.
- **Public signals**: These are the inputs and outputs of the ZK circuit that are made public to the verifier.

The verifier uses both the ZK proof and the public signals to validate the proof.

### Summary: Recipe for a ZKP-based Proof of Identity from Signed Documents

To summarize the steps required to build a ZKP-based proof of identity:
1. **Digitally signed document**: Use a document signed with asymmetric cryptography (public/private key pair). The specific signature algorithm may affect proof generation time but isn't strictly limiting.
2. **Machine-readable signature**: The signature must be easily readable via methods like QR code or NFC scanning for compatibility with web or mobile apps.
3. **Parsable data**: The signed data must be in a format that's easy to parse. For example, formats like PDFs may be challenging due to their size and encoding.
4. **Nullifier generation**: Create a unique nullifier to prevent double-spending or Sybil attacks:
    - Avoid using easily guessable personal information.
    - Include a random, app-specific component to prevent cross-app tracking.
5. **Additional checks**: (e.g., timestamp) can be enforced depending on the protocol's requirements.

This approach ensures a flexible and privacy-preserving proof of identity using zero-knowledge proofs, with the added security of preventing replay attacks through robust nullifier generation.

### Wrapping ZKPs as Verifiable Credentials (VCs)

Now that we've demonstrated how to generate a proof of identity from a signed document using ZKPs, the next step is to make it compliant with the **W3C Verifiable Credential (VC) format**. This allows us to issue VCs, even if the government or issuer has not adopted the standard Self-Sovereign Identity (SSI) tools.

The simplest approach is to use our ZK proof as the **proof** within the VC, replacing the conventional cryptographic signature with our zero-knowledge proof (ZKP).

Here's an example:

```json
{
 "@context": [
     "https://www.w3.org/ns/credentials/v2",
     "https://www.w3.org/ns/credentials/examples/v2"
     ],
    "id": "urn:uuid:58172aac-d8ba-11ed-83dd-0b3aef56cc33",
    "type": [ "VerifiableCredential", "AnonAadhaarCredential", "Groth16Proof" ],
    "name": "Anon Aadhaar Credential",
    "description": "Anon Aadhaar proof of identity",
    "issuer": "https://anon-aadhaar.pse.dev/",
    "validFrom": "2023-01-01T00:00:00Z",
    "credentialSubject": {
        "nullifier": "79466646946986147...",
        "timestamp": "1720017000",
        "pubkeyHash": "151348740153163242...",
        "ageAbove18": "1",
        "gender": "M",
        "state": "",
        "pincode": ""
    },
    "proof": {
        // This could be replaced by a DID, with the verification key hosted on IPFS
        "verificationKey": "https://anon-aadhaar-artifacts.s3.eu-central-1.amazonaws.com/v2.0.0/vkey.json",
        "verificationMethod": "https://github.com/anon-aadhaar/anon-aadhaar/blob/8d6d38a52ee67e36286c1d1fa42a8952f8752fa7/packages/core/src/core.ts#L112",
        "groth16Proof": {
            "pi_a": [ "139159...", "72424812809...", "1" ],
            "pi_b": [
                [
                  "949367893...", "485706694..."
                ],
                [
                  "4983125622...", "96275896805..."
                ],
                [
                  "1",
                  "0"
                ]
              ],
            "pi_c": [ "1908185775...", "2263172213045...", "1" ],
            "protocol": "groth16",
            "curve": "bn128"
    },
    "pubkeyHash": "15134874015316324267425466444584014077184337590635665158241104437045239495873",
    "timestamp": "1720017000",
    "nullifierSeed": "1234",
    "nullifier": "7946664694698614794431553425553810756961743235367295886353548733878558886762",
    "signalHash": "10010552857485068401460384516712912466659718519570795790728634837432493097374",
    "ageAbove18": "0",
    "gender": "0",
    "pincode": "0",
    "state": "0"
  }
}
```

With this structure, our proof is now **compliant with the W3C Verifiable Credential standard**.

### Integration Overhead for Relying Parties (Verifiers)

In the VC, we specify the verification method, but the relying party (RP) would still need to integrate our SDK and roadmap the inclusion of our specific verifier, that can embed specific rules and not only the verification of the Groth16 proof (ZKP).

However, this raises several challenges:
1. **What if the relying party wants to verify a passport proof instead?**
2. **What if another ZKP protocol uses a different proving scheme?**
3. **What if the prover discloses only age, but the relying party also wants gender disclosed?**

These scenarios highlight the **overhead** involved with relying on independent protocols for each ZKP and verification type. Each protocol would require its own integration, creating friction for relying parties and limiting interoperability.

## Introducing SelfAttestHub: A Unified Interface for Identity Verification

To streamline this process, we propose a general interface—**SelfAttestHub**—which would require only one verifier. The idea is to establish a **shared interface** that supports multiple ZKP schemes and identity protocols, minimizing integration overhead and allowing relying parties to easily verify various forms of identity.

The main problem we are trying to solve is to have a setup where we could use one unique verifier for our proofs. While ZKVMs could be an answer in the future, there are not yet practical for client side proving, Merkle Trees inclusion proofs and Smart Contracts are offering a solution viable for today.

Rather than directly consuming and verifying individual identity proofs, this approach shifts the responsibility of proof verification to smart contracts. Users generate their identity proofs client-side and use them to join groups managed by smart contracts. These smart contracts act as decentralized issuers, publicly recording the proof and adding users to the group based on their verified identities.

Once a user's proof is validated by the smart contract, it is added as a claim within a Merkle Tree. This allows for efficient group membership verification—users can generate a new proof showing that their claim is included in the Merkle Tree, enabling verifiers to confirm membership without needing to directly handle or validate each ZK proof. This method not only streamlines the verification process but also ensures transparency and public auditability of group memberships, while preserving privacy by allowing selective disclosure of information.

#### Merkle Tree Inclusion Proofs (MTIP)

Merkle Trees are a cryptographic data structure that enable efficient proof of inclusion while maintaining privacy by not revealing the entire dataset. In a nutshell it's tree constructed by hashes, composed by leafs storing the data and a root, proving the leafs global state--If the value of a leaf change the root will also change. In the context of **SelfAttestHub**, after a user's identity proof is validated by a smart contract, it is added as a claim within a Merkle Tree composed of other claims. To prove that a specific claim (or identity attribute) is included in the tree, the user can generate a Merkle proof, which involves providing only a subset of the tree—the "neighbors" of the claim—without revealing the entire structure.

![image](https://hackmd.io/_uploads/B1MYrP5xkx.png)
*To prove that `Identity Claim 2` is part of the Claim tree, only the yellow-highlighted neighbors are needed for the proof.*

We can take this concept even further by combining MTIP with ZK. This allows users to prove that they know a claim within the tree, without revealing which specific claim it is. For instance, a user could demonstrate that they belong to a group without disclosing their exact identity or any specific claim, achieving even higher levels of privacy.

The key benefit of using group membership proofs, instead of directly verifying the identity proof, is that it enables the system to utilize a single, generic prover. This prover is only responsible for handling Merkle Tree inclusion proofs, greatly simplifying the verification process and reducing the complexity of handling individual identity proofs.

#### MTIP Applied to Identity Attributes

While we've introduced Merkle Tree Inclusion Proofs (MTIP) for group membership proofs, this approach can also extend to identity data itself. Each individual can commit to a *merklized* version of their identity document, where each leaf of the Merkle Tree represents a specific attribute (e.g., name, birthdate, address, etc.).

This structure enables selective disclosure, allowing users to prove the validity of individual attributes without revealing unnecessary personal information. By applying MTIP, users can generate proofs that a specific leaf is present in the committed `Identity Tree`, with the root of this tree published in the `Claim Tree` to prevent tampering. Additionally, because the Merkle Tree structure is hash-based, as long as one leaf remains random or secret, the data is secure against dictionary attacks. This approach can also support compliance with privacy standards like GDPR.

![image](https://hackmd.io/_uploads/SyAVKCrbJg.png)

#### Enhanced VC Structure with SelfAttestHub

We previously discussed the two levels of MT usage: one to commit to identity attributes and the other to commit to claims.

The first step for the user is to generate a proof from their identity document. The `Identity Prover` verifies the document as explained above, extracts all identity attributes, and output the root of the identity tree. At this step, the wallet records all attributes in plain text in the form of a VC, allowing the client to dynamically rebuild the `Identity Tree` when needed. However, this VC is not shared publicly, as it contains clear data. Instead, only the ZKP from the signature verification is sent to the on-chain issuer to be added to the `Claim Tree`, recording the root of the `Identity Tree`.

![image](https://hackmd.io/_uploads/SyEYYArZJx.png)

At this point, the user represented by the Identity Tree root is registered in the claim tree. However, the VC we just committed to cannot be shared as is, since we want to minimize the data disclosed.

Now that the user is registered, they can connect with third parties. The authentication process will involve an MTIP based on the VC stored locally. Depending on the specific data required by the third party, the user can selectively disclose only the relevant information during authentication.

![image](https://hackmd.io/_uploads/Bk4ipXPbkl.png)

In this example, the third party requests the user to have a verified identity and disclose age. The wallet receives this request, retrieves the local plain text VC, and passes it to the `Claim Prover`, which then builds an MTIP proof. This proof includes the leaf corresponding to the birthdate, allowing the user to prove their age. The prover can also include a range proof (e.g., age > 18) without disclosing the exact birthdate.

The locally stored VC with identity attributes is essential for the prover to dynamically rebuild the Identity Tree and support selective disclosure based on the verifier's request.

The last step on the client side, will be to authenticate the Verifiable Presentation (VP), that is going to be passed to the third party. This authentication mechanism could be a signature from the wallet private key.

Finally the third party could verify the Verifiable Presentation containing the MTIP proving the age, check against the on-chain issuer that the Identity Root is included in the `Claim Tree` and the authentication of the VP.

#### Example: SelfAttestHub with Anon Aadhaar

Taking Anon Aadhaar as an example, the original circuit, detailed above, needs to evolve. In this new setup, we are still extracting the identity attributes, but now the user will commit all of them through the root of an `Identity Tree`. The tree will be constructed inside of the circuit using a SNARK friendly hash function such as Poseidon. This ensures that users do not need to commit to their attributes again when they want to prove something about their identity.

![image](https://hackmd.io/_uploads/SJrdBfFlyx.png)
*Modified circuit architecture, now extracting all the identity attributes and  commiting to it through a Merkle Tree Root.*

![image](https://hackmd.io/_uploads/B1SMlFx-kg.png)
*The `Identity Tree` built in the circuit from the identity attributes extracted in the identity documents.*

Note here, that we are using the photo as source of randomness but to ensure the security of the data posted on-chain we actually need to have some randomness as a leaf of our tree, to prevent from deanonymization (e.g. I know someones identity and can recompute the tree root myself and spy on their activity). For example, A mechanism implying a secret could be used, where the secret is a leaf of the tree, and the user commits to a public value being the hash of the secret.

The wallet will also have now to store the VC, that will look something like that.

```json
{
 "@context": [
     "https://www.w3.org/ns/credentials/v2",
     "https://www.w3.org/ns/credentials/examples/v2"
     ],
    "id": "urn:uuid:58172aac-d8ba-11ed-83dd-0b3aef56cc33",
    "type": [ "VerifiableCredential", "AnonAadhaarCredential", "Groth16Proof" ],
    "name": "Anon Aadhaar Credential",
    "description": "Anon Aadhaar proof of identity",
    "issuer": "https://anon-aadhaar.pse.dev/",
    "validFrom": "2023-01-01T00:00:00Z",
    "credentialSubject": {
        "birthdate": "1990-01-01",
        "gender": "M",
        "address": "123 Main St, Springfield",
        // Base64 encoded
        "photo": "4AAQSkZJRgABAQAAAQABAAD/2wBDAAgGBgcGBQgH...",
        "firstName": "John",
        "lastName": "Doe"
    },
    "proof": {
        // This could be replaced by a DID, with the verification key hosted on IPFS
        "verificationKey": "https://anon-aadhaar-artifacts.s3.eu-central-1.amazonaws.com/v2.0.0/vkey.json",
        "verificationMethod": "https://github.com/anon-aadhaar/anon-aadhaar/blob/8d6d38a52ee67e36286c1d1fa42a8952f8752fa7/packages/core/src/core.ts#L112",
        "groth16Proof": {
            "pi_a": [ "139159...", "72424812809...", "1" ],
            "pi_b": [
                [
                  "949367893...", "485706694..."
                ],
                [
                  "4983125622...", "96275896805..."
                ],
                [
                  "1",
                  "0"
                ]
              ],
            "pi_c": [ "1908185775...", "2263172213045...", "1" ],
            "protocol": "groth16",
            "curve": "bn128"
    },
    "pubkeyHash": "15134874015316324267425466444584014077184337590635665158241104437045239495873",
    "timestamp": "1720017000",
    "nullifierSeed": "1234",
    "nullifier": "7946664694698614794431553425553810756961743235367295886353548733878558886762",
    "signalHash": "10010552857485068401460384516712912466659718519570795790728634837432493097374",
    "ageAbove18": "0",
    "gender": "0",
    "pincode": "0",
    "state": "0"
  }
}
```

But again as explained above, this VC is not meant to be consummed by third parties, only to be securely stored by the wallet to be consumed by the selective disclosure prover.

Next step is to commit to our `Identity Tree Root` to the on-chain issuer, that will verify the Anon Aadhaar Proof and record our root.

## Other SSI Properties

In this example, we've intentionally simplified some Self-Sovereign Identity (SSI) requirements and features. However, it's important to outline the core SSI properties that contribute to a secure and privacy-preserving identity system:

#### Unlinkability

Unlinkability is a critical SSI property that ensures each transaction or interaction using a VC cannot be easily linked to others. This protects the user's privacy, as a third party should not be able to correlate multiple uses of the credential to track or profile the user. Techniques such as ZKPs, unique one-time credential presentations, and key rotation to periodically change identifiers can help achieve unlinkability.

#### Revocation

Revocation is the ability to invalidate a credential before its expiration date if it's compromised, lost, or no longer valid. In SSI, this is typically achieved through decentralized revocation registries or cryptographic status lists that allow verifiers to check a credential's current status. Additionally, we could commit revoked VCs to another Merkle Tree recorded on-chain. Revocation mechanisms are designed to ensure that a credential can be invalidated without compromising the user's privacy or revealing the reasons for revocation.

#### Interoperability

SSI systems aim to support interoperability across different wallets and protocols. This means that credentials issued in one wallet should be transferable to another, enhancing data sovereignty and usability across platforms.

#### User Control and Consent

SSI is centered around user control, meaning that users must consent to each use of their credentials. Users decide when and where their information is shared, empowering them to manage their identity data autonomously. For third-party authentication, there should be a clear interface explaining to the user what information will be shared and with whom, along with a straightforward consent or deny option.

#### Recovery
As a user I should be able to always recover my credential in the case I'm losing control of my wallet or identifier. While here we are not specifying any specific recovery methods here, some solutions could be explored:
- **Automatic Encrypted Backup**: Regularly backs up your credential data to a secure, encrypted cloud service, enabling easy restoration if your wallet is lost.
- **Offline Recovery**: Provides a recovery option through a physical or offline method, such as a cold wallet.
- **Social Recovery**: Allows you to designate trusted contacts to help you recover your identifiers, ensuring access even if your primary device or wallet is lost.
- **Multi-Device Recovery**: Synchronizes credentials across multiple devices, allowing you to recover access from any authorized device if one is lost or compromised.

## Conclusion

SelfAttestHub is inspired by the [zk-creds paper](https://eprint.iacr.org/2022/878) and introduces a versatile, proving-system-agnostic framework designed to support a variety of ZKP projects with on-chain verifiers, such as OpenPassport, zkEmail, and could involve MPC-based projects such as TLSN. Our contribution is to create a way for PSE identity projects to share an anonymous credentials interface that comply with W3C standards, facilitating integration with SSI wallet providers.

SelfAttestHub could open new research avenues, including:
- **Efficient Client-Side Proving Improvements**: Enhancing client-side efficiency for proofs verifying the authenticity of identity documents by exploring other proving systems than Groth16, like STARKs, that could offer a post quatum resistant alternative.
- **Client-side ZKVMs**: which could be the killer application for this use case, as by default you would require only one unique verifier.
- **Exploring Alternative Identity Commitment Schemes**: Investigating new approaches, such as replacing Merkle Trees with polynomial commitments and utilizing KZG for selective disclosure, as proposed in [this paper](https://eprint.iacr.org/2020/081.pdf).

In the meantime, this solution could be implemented with minimal effort by leveraging existing infrastructure. Iden3, for example, already includes key SSI components such as MTIP using Groth16, revocation mechanisms, and BJJ signatures, all of which are W3C-compliant.

Key components for effective integration of Iden3 with SelfAttestHub include:

- **Standard Solidity Interface for On-Chain Issuers**: This interface would manage ZKP verification logic, accommodating the specific protocols built on top of Groth16 for consistency and reliability across projects.
- **Circom Library for Identity Tree Commitment**: Developing a Circom library that enables commitment to the Identity Tree is essential to ensure efficient Merkle Tree proofs and selective disclosure.
- **Mobile Web SDK**: A mobile SDK should be developed to facilitate signing and proving logic. While Privado provides an SDK, further analysis is needed to determine if a new SDK is required to ensure compatibility across a broad range of SSI providers, beyond Privado.

In sum, SelfAttestHub, combined with Iden3's existing framework, could offer a robust path forward for building privacy-preserving, and interoperable identity verification solutions through self-attestation in the SSI ecosystem.
