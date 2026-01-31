---
layout: post
title: "A Glimpse into ProgCrypto's Future"
date: 2024-12-05
---

Over the past year, I've been deeply involved in developing **Anon Aadhaar**—a protocol that allows Indian Aadhaar holders to generate a Zero-Knowledge Proof (ZKP) attesting to their possession of a genuinely signed identity document, all without revealing personal information.

At **Devcon**, I was overwhelmed by the attention and positive feedback Anon Aadhaar received. Our collaboration with the Devcon team enabled nearly **1,000 Indian citizens** to anonymously redeem discounted tickets, showcasing the practical applications of our protocol and proving that the space has significantly advanced, now enabling impactful real-world use cases.

While I remain a strong advocate for ZKPs as a means to enhance privacy and empower users to locally prove computations over their personal data, recent discussions and talks have pushed me to refine my perspective. These experiences have solidified my vision for what we're now calling **programmable cryptography** (ProgCrypto).

After catching a few hours of sleep and spending countless more watching talks on YouTube that I couldn't attend in person, I felt compelled to organize my thoughts and reflections.

## The Challenge of Trust and Understanding in ZKP Protocols

Protocols like **Anon Aadhaar**, **ZkEmail**, and **OpenPassport** share a common structure: they rely on a trusted issuer (e.g., a government signing your identity document or a mail client signing your email) and a verifier seeking authentication based on the issuer's signature. Using ZKPs, we can derive proofs from these signatures and selectively disclose information from the signed data.

But here lies the challenge: traditional issuers—like governments—aren't prepared to understand or trust cryptographic constructs they didn't issue directly. A government signs a document but then receives this "weird object" containing terms like "Groth16." Even with standards like **W3C Verifiable Credentials**, simply wrapping ZKPs in a polished envelope doesn't guarantee verifiers will understand or trust the underlying computation. This raises a critical question:

*What is the right export format for protocols using ZKPs to bridge the gap between verifiable computation and trust in the outside world?*

![image](https://hackmd.io/_uploads/HyVLc9vM1x.png)


### The Dual-Edged Nature of ZKPs: Rethinking Their Role

I was few days ago a strong advocate for ZKPs as the future of authentication, but ZKPs while powerful tools for privacy and trust, are not inherently defensive. In fact, when consumed directly by external parties, they risk being misused or weaponized. Consider selective disclosure—a seemingly empowering concept where individuals prove computations on personal data without revealing the data itself.

This can be turned on its head: a third party could exploit ZKPs to enforce discriminatory policies. For example, a service provider might require proof derived from sensitive data, such as income or location, to gate access to services or groups. This shifts ZKPs from being a privacy tool to a means of enforcing exclusionary or exploitative practices, turning it as a offensive practice.

The fundamental issue lies in **context**. ZKPs alone lack the semantic and interpretive layer necessary for trust, they should not be made to be interpreted by the same entity that created the original authentication of the data. They need to be more than cryptographic constructs; they must convey the context of their computation and the assumptions underpinning their trustworthiness.

### A New Role for ZKPs: Semantic Teleportation

What if ZKPs weren't meant to be directly consumed by verifiers in their raw form? Instead, they could serve as a **mechanism for teleporting data into a new universe**—a space where their semantics, trust assumptions, and computations are universally understood and intrinsically trusted.

I was focusing on building rockets, a huge engine that doesn't have a well defined destination, while now I think we need to work on teleportation to another universe. In this universe, ZKPs wouldn't just be cryptographic evidence. They would act as interpretable, verifiable artifacts that encode meaning and trust, enabling secure computation without the risk of misinterpretation or misuse. By reimagining their role, we can mitigate the potential for ZKPs to be weaponized and instead unlock their full potential as enablers of trust in the cryptographic ecosystem.

<table>
<tr>
<td><img src="https://hackmd.io/_uploads/HkJK35Pf1l.jpg" alt="Earth with digital symbols" /></td>
<td><img src="https://hackmd.io/_uploads/rkjGa9Pzyx.jpg" alt="Teleportation portal" /></td>
</tr>
</table>

### Recursion and the Provable Object Data (PODs)

One of the most exciting insights came from conversations with **Gubsheep** and **Justin** from [**0xPARC**](https://0xparc.org/) about **PODs**. ZK recursion is key to this vision, addressing a major barrier: **the cost of data verification.**

Picture a world where you authenticate signed data—whether identity documents, emails, or API responses—with ZKPs and produce a verified object data. The cost of verifying its provenance is either already paid or negligible to repeat. Such objects would act as **tickets** paid only once, allowing data to seamlessly enter the ProgCrypto universe, where it can be securely processed in a trustless and peer-to-peer manner.

## A Glimpse into ProgCrypto's Future

Devcon provided a unique opportunity to explore this broader vision, especially through discussions on cryptographic techniques like **Fully Homomorphic Encryption (FHE)**, **Multiparty Computation (MPC)**, and **Indistinguishability Obfuscation (iO).**

I was born in the early '90s, growing up, I exchanged ringtones and photos via **infrared** by making physical contact with a friend's phone, or traded Pokémon by connecting two **Game Boys** with a cable. These interactions were magical—direct, peer-to-peer, and entirely local.

<table>
<tr>
<td><img src="https://hackmd.io/_uploads/HJyf5XvG1e.png" alt="Infrared exchange" /></td>
<td><img src="https://hackmd.io/_uploads/BJ3pFXwzke.png" alt="Game Boy cable" /></td>
</tr>
</table>

Today, this simplicity has been replaced by centralized servers. Every interaction now passes through a middleman—a server that stores, analyzes, and often exploits our data. **ProgCrypto** aims to bring back the spirit of peer-to-peer exchanges while leveraging modern cryptography to transcend physical barriers. Techniques like **MPC**, **FHE**, and **iO** act as the new cables, allowing global machines to serve as **blind intermediaries**—facilitating secure and controlled computation over exchanged data while ensuring no one "listens through the wall." Meanwhile, **ZKPs** provide the trust framework, enabling participants to verify each other's data without revealing sensitive information.

Imagine experiences like Pokémon trading, but no longer limited to physical presence, and scaled to include multiple participants seamlessly. This is the vision ProgCrypto offers: secure, collaborative exchanges that combine the simplicity of the past with the transformative power of modern cryptography.

## Building the Future of Trust

While we must advocate for **stronger defensive mechanisms** to ensure ZKPs and verifiable data remain tools of empowerment rather than exploitation, ProgCrypto opens the door to something far greater. It doesn't just defend against misuse; it has the potential to **redefine social interactions** and reshape how we trust, compute, and collaborate.

Projects like [**Cursive**](https://www.cursive.team/) demonstrate how ProgCrypto can revolutionize direct, peer-to-peer interactions, fostering deeper, more meaningful connections in a way that reduces reliance on intermediaries.

One area that particularly excites me is ProgCrypto's potential impact on **microeconomies**. Imagine decentralized auctions, more efficient price discovery, and trustless P2P markets enabled by secure computation—[**ZKP2P**](https://zkp2p.xyz/) being a great example of that idea. These innovations could fundamentally alter how small-scale economies operate, unlocking new opportunities for individuals and communities to collaborate and trade without centralized oversight.

ProgCrypto represents not just a technological shift but a new paradigm for how we interact, exchange, and build trust. By combining stronger defenses with these transformative opportunities, we can create a future where cryptographic tools empower individuals and societies in ways we've only just begun to imagine.

---

*Thanks to Pierre, Enrico, Cedoor, Mac, Vivek and Oskar for their review.
Ideas are my own and I'm not speaking in the name of any organisation.*
