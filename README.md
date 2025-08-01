### Hi there 👋

Please support the following [memory safe](https://www.memorysafety.org/docs/memory-safety/) Rust projects. It would be great to have such a future:
* [Redox OS](https://github.com/redox-os)
  * [relibc](https://github.com/redox-os/relibc) C standard library written in Rust
  * [reqwest](https://github.com/seanmonstar/reqwest) instead of curl
  * [Servo](https://github.com/servo/servo) web browser
    * [WebRender](https://github.com/servo/webrender)
    * [Pathfinder](https://github.com/servo/pathfinder) for Canvas, Font, SVG
    * [Rustls](https://github.com/ctz/rustls)
      * [graviola](https://github.com/ctz/graviola) for x86_64 + arm64
      * [aws-lc-rs](https://github.com/aws/aws-lc-rs) or [ring](https://github.com/briansmith/ring) for the rest (riscv64, wasm32, loongarch64, powerpc64, s390x, mips, x86, arm32)
      * [Rustls-WebPKI](https://github.com/rustls/webpki) (based on [WebPKI](https://github.com/briansmith/webpki))
    * [rust-av](https://github.com/rust-av) (hopefully as [media backend](https://github.com/servo/media/tree/master/backends) sometime in the future)
      * [mkv/webm](https://github.com/rust-av/matroska), mp4
      * [Playback](https://github.com/rust-av/avp)
        * [Opus](https://github.com/lu-zero/opus), FLAC
        * [AV1](https://github.com/rust-av/dav1d-rs) (soon [rav1d](https://github.com/memorysafety/rav1d)), [VP8/VP9](https://github.com/rust-av/vpx-rs)
      * [Transcoding](https://github.com/rust-av/ave)
        * [rav1e](https://github.com/xiph/rav1e)
    * [boa](https://github.com/boa-dev/boa) (hopefully as safe JS interpreter in the far future)
  * [Hickory DNS](https://github.com/hickory-dns/hickory-dns) (former name was [Trust-DNS](https://github.com/bluejekyll/trust-dns))
  * [Stalwart server](https://github.com/stalwartlabs/mail-server): mail (JMAP,IMAP,SMTP,DKIM,DMARC,etc), contacts/calendars/files (WebDAV, later also JMAP), ACME client
  * [warp](https://github.com/seanmonstar/warp) or [axum](https://github.com/tokio-rs/axum) web server framework
    * [Yew](https://github.com/yewstack/yew) or [Leptos](https://github.com/leptos-rs/leptos) web app framework
* Please be green on [hardenize.com](https://hardenize.com) and use IPv6 (against IPv4 fragmentation attacks), [RPKI](https://nlnetlabs.nl/projects/rpki/routinator/) for BGP, [ECDSA DNSSEC](https://www.cloudflare.com/dns/dnssec/ecdsa-and-dnssec/), TLSA/DANE, TLS 1.3,DMARC (`p=reject`), SPF (`mx -all`), [CSP](https://report-uri.com/home/generate), [XHTML5](https://blog.whatwg.org/xhtml5-in-a-nutshell),
  * CAA
    * `CAA 128 issue "letsencrypt.org; validationmethods=dns-01"`
    * `CAA 0 issuemail ";"` [only issuemail can restrict S/MIME cert issuance](https://www.rfc-editor.org/rfc/rfc9495.html#name-no-issuemail-property)
    * `CAA 0 issuevmc ";"` [only issuevmc can restrict issuance of a signed maildomain logo](https://bimigroup.org/resources/VMC_Requirements_latest.pdf#page=59) for the [DNS TXT BIMI record](https://support.google.com/a/answer/10911321)
  * HSTS
    * [HTTP header + preload](https://hstspreload.org/): `Strict-Transport-Security: max-age=63072000; includeSubDomains; preload`
    * HSTS via HTTPS DNS RR (`HTTPS 1 . alpn=h2`)
<!--
**Darkspirit/Darkspirit** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
